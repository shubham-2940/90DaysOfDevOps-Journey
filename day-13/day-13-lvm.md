# Day 13 – Linux Volume Management (LVM)

## Objective

The goal of this exercise was to understand and practice Linux Logical Volume Management (LVM), including creating Physical Volumes (PV), Volume Groups (VG), Logical Volumes (LV), formatting filesystems, mounting volumes, and extending storage dynamically.

---

# Task 1 – Check Current Storage

Before creating any LVM resources, I checked the available disks and storage information.

## Commands Used

```bash
lsblk
pvs
vgs
lvs
df -h
```

## Purpose

These commands provide information about:

* Available disks and partitions
* Existing Physical Volumes (PV)
* Existing Volume Groups (VG)
* Existing Logical Volumes (LV)
* Mounted filesystems and disk usage

### Screenshot

![Current Storage Information](./screenshots/linux-volumes.png)

---

# Task 2 – Create Physical Volumes

The available disks were:

```text
/dev/nvme1n1
/dev/nvme2n1
/dev/nvme3n1
```

I initialized all three disks as Physical Volumes.

## Command Used

```bash
pvcreate /dev/nvme1n1 /dev/nvme2n1 /dev/nvme3n1
```

## Verification

```bash
pvs
```

### Result

All three disks were successfully converted into Physical Volumes and became available for LVM management.

### Screenshot

![Physical Volumes](./screenshots/linux-volumes.png)

---

# Task 3 – Create Volume Group

After creating the Physical Volumes, I combined two of them into a Volume Group named **sd-vg**.

## Command Used

```bash
vgcreate sd-vg /dev/nvme1n1 /dev/nvme2n1
```

## Verification

```bash
vgs
```

### Result

Volume Group created successfully.

```text
VG Name : sd-vg
Size    : 21.99 GB
```

### Screenshot

![Volume Group Creation](./screenshots/linux-volumes.png)

---

# Task 4 – Create Logical Volume

I created a Logical Volume named **sd-lv** with a size of 10 GB.

## Command Used

```bash
lvcreate -L 10G -n sd-lv sd-vg
```

## Verification

```bash
lvs
```

### Result

```text
Logical Volume: sd-lv
Size: 10 GB
```

### Screenshot

![Logical Volume Creation](./screenshots/linux-volumes.png)

---

# Task 5 – Format and Mount the Logical Volume

## Create Mount Directory

```bash
mkdir /mnt/sd-lv-mount
```

## Format the Logical Volume

To use the logical volume, I created an EXT4 filesystem on it.

```bash
mkfs.ext4 /dev/sd-vg/sd-lv
```

### Screenshot

![Formatting Logical Volume](./screenshots/volume-format.png)

---

## Mount the Logical Volume

```bash
mount /dev/sd-vg/sd-lv /mnt/sd-lv-mount
```

## Verify Mount

```bash
df -h
```

### Result

The logical volume was successfully mounted.

```text
/dev/mapper/sd--vg-sd--lv
```

Mounted on:

```text
/mnt/sd-lv-mount
```

### Screenshot

![Mounted Logical Volume](./screenshots/volume-mount.png)

---

# Task 6 – Extend the Logical Volume

One of the major advantages of LVM is the ability to increase storage without recreating partitions.

Initially, the logical volume size was:

```text
10 GB
```

## Extend Logical Volume

```bash
lvextend -L +5G /dev/sd-vg/sd-lv
```

### Result

The logical volume size increased from:

```text
10 GB → 15 GB
```

### Screenshot

![Logical Volume Extension](./screenshots/extending-volume.png)

---

## Resize Filesystem

After extending the logical volume, the filesystem was resized to utilize the additional storage.

```bash
resize2fs /dev/sd-vg/sd-lv
```

## Verify

```bash
df -h
```

### Result

The mounted filesystem reflected the new size successfully.

---

# Additional Practice – Mounting a Separate Disk

To further understand storage management, I formatted and mounted a separate disk outside the LVM setup.

## Create Mount Directory

```bash
mkdir /mnt/sd-disk-mount
```

## Create Filesystem

```bash
mkfs -t ext4 /dev/nvme3n1
```

### Screenshot

![Disk Formatting](./screenshots/disk-mount-1.png)

---

## Mount Disk

```bash
mount /dev/nvme3n1 /mnt/sd-disk-mount
```

## Verify

```bash
df -h
```

### Result

The disk was mounted successfully and became available for use.

### Screenshot

![Mounted Disk](./screenshots/showing-mounted-disk.png)

---

# Commands Used

```bash
lsblk

pvs
vgs
lvs

df -h

pvcreate /dev/nvme1n1 /dev/nvme2n1 /dev/nvme3n1

vgcreate sd-vg /dev/nvme1n1 /dev/nvme2n1

lvcreate -L 10G -n sd-lv sd-vg

mkdir /mnt/sd-lv-mount

mkfs.ext4 /dev/sd-vg/sd-lv

mount /dev/sd-vg/sd-lv /mnt/sd-lv-mount

lvextend -L +5G /dev/sd-vg/sd-lv

resize2fs /dev/sd-vg/sd-lv

mkdir /mnt/sd-disk-mount

mkfs -t ext4 /dev/nvme3n1

mount /dev/nvme3n1 /mnt/sd-disk-mount
```

---

# What I Learned

* Physical Volumes (PV) are the foundation of LVM and are created from disks.
* Volume Groups (VG) combine multiple disks into a single storage pool.
* Logical Volumes (LV) can be resized dynamically without repartitioning.
* Filesystems must be resized after extending a logical volume.
* LVM provides flexible and scalable storage management for Linux systems.

---

# Why This Matters for DevOps

Storage requirements in production environments continuously grow. LVM helps DevOps engineers manage storage efficiently by allowing online volume expansion, flexible disk management, and better resource utilization without significant downtime.

This makes LVM an essential skill for Linux system administration and DevOps operations.

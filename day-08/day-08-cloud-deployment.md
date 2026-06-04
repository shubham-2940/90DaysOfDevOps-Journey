# Day 08 – Cloud Deployment with AWS EC2 and Nginx

## Objective

Today's goal was to deploy a real web server on the cloud and gain hands-on experience with server provisioning, remote access, web server installation, security configuration, and log management.

---

## Environment

* Cloud Provider: AWS EC2
* Operating System: Ubuntu Server
* Web Server: Nginx
* Access Method: SSH
* Log Transfer Method: SCP

---

## Tasks Performed

### 1. Launched an AWS EC2 Instance

* Created an Ubuntu EC2 instance.
* Generated and downloaded a key pair (.pem file).
* Configured Security Group rules to allow:

  * SSH (Port 22)
  * HTTP (Port 80)

### 2. Connected to the Server via SSH

Successfully connected to the EC2 instance using SSH from the local machine.

```bash
ssh -i day08-key.pem ubuntu@<PUBLIC_IP>
```

### 3. Updated the Server

```bash
sudo apt update
sudo apt upgrade -y
```

### 4. Installed Nginx

```bash
sudo apt install nginx -y
```

Verified the service status:

```bash
sudo systemctl status nginx
```

Enabled Nginx to start automatically on boot:

```bash
sudo systemctl enable nginx
```

### 5. Verified Web Server Functionality

Tested locally on the server:

```bash
curl localhost
```

The command returned the default Nginx HTML page, confirming the web server was running successfully.

Verified public access by opening:

```text
http://<PUBLIC_IP>
```

The Nginx Welcome Page was successfully displayed in the browser.

### 6. Inspected Nginx Logs

Viewed access logs:

```bash
sudo tail /var/log/nginx/access.log
```

Viewed error logs:

```bash
sudo tail /var/log/nginx/error.log
```

### 7. Saved Logs to a File

```bash
sudo cp /var/log/nginx/access.log ~/nginx-logs.txt
```

Verified the contents:

```bash
cat ~/nginx-logs.txt
```

### 8. Downloaded Log File to Local Machine

```bash
scp -i day08-key.pem ubuntu@<PUBLIC_IP>:~/nginx-logs.txt .
```

---

## Commands Used

```bash
ssh -i day08-key.pem ubuntu@<PUBLIC_IP>

sudo apt update
sudo apt upgrade -y

sudo apt install nginx -y

sudo systemctl status nginx
sudo systemctl enable nginx

curl localhost

sudo tail /var/log/nginx/access.log
sudo tail /var/log/nginx/error.log

sudo cp /var/log/nginx/access.log ~/nginx-logs.txt

cat ~/nginx-logs.txt

scp -i day08-key.pem ubuntu@<PUBLIC_IP>:~/nginx-logs.txt .
```

---

## Challenges Faced

### Issue 1: Configuring Public Web Access

Initially, the web page was not accessible from the browser.

**Solution:**
Verified that the Security Group allowed inbound HTTP traffic on Port 80 and confirmed Nginx was running.

### Issue 2: Understanding Nginx Log Locations

Needed to identify where Nginx stores access and error logs.

**Solution:**
Used the default log paths:

```bash
/var/log/nginx/access.log
/var/log/nginx/error.log
```

and successfully extracted the logs.

---

## What I Learned

* How to launch and configure a cloud server using AWS EC2.
* How to connect securely to a remote server using SSH.
* How to install, start, and manage Nginx services.
* How to verify a web application is publicly accessible.
* How to inspect, save, and transfer server log files.

---

## Why This Matters for DevOps

This exercise teaches important real-world DevOps skills:

* Cloud infrastructure provisioning – launching and configuring servers.
* Remote server management – SSH access, security, and administration.
* Service deployment – installing and managing applications like Nginx.
* Log management – accessing, analyzing, and exporting logs.
* Security – configuring firewalls and security groups.

These are foundational skills used daily by DevOps Engineers, Cloud Engineers, and System Administrators in production environments.

---

## Conclusion

Successfully deployed a cloud-based Ubuntu server, installed and configured Nginx, verified public web access, extracted logs, and practiced secure remote server administration using AWS EC2.

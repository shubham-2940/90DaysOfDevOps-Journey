🚀 **Day 18/90 – Shell Scripting: Writing Bash Like a DevOps Engineer**

Today I realized something interesting...

Anyone can write a Bash script that *works*.

But writing a Bash script that's **safe, reusable, and easy to maintain** is a completely different skill.

So today I moved beyond basic scripting and focused on writing production-style Bash scripts.

Here's what I built:

✅ Functions to make scripts modular and reusable
✅ Parameterized functions with arguments and return values
✅ `set -euo pipefail` to catch errors before they become bigger problems
✅ Local variables to avoid unexpected side effects
✅ A complete **System Information Reporter** that displays:

* Hostname & OS details
* System uptime
* Disk usage
* Memory usage
* Top CPU-consuming processes

One of the biggest takeaways was understanding why experienced DevOps engineers always recommend:

```bash
set -euo pipefail
```

At first, it looked like just another line of code.

Now I understand it's one of the simplest ways to make Bash scripts far more reliable by preventing silent failures and catching mistakes early.

💡 **Learning isn't just about making scripts run.**
It's about writing scripts that are predictable, maintainable, and production-ready.

Every day in this #90DaysOfDevOps journey is making me think less like someone who writes commands... and more like someone who automates systems.

📌 **Question for the DevOps community:**

What's one Bash scripting practice or trick that made the biggest difference in your career?

I'd love to learn from your experience. 👇

#90DaysOfDevOps #DevOps #Linux #Bash #ShellScripting #Automation #Cloud #AWS #Infrastructure #SystemAdministration #OpenToWork #ContinuousLearning

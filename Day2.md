# 🎄 TryHackMe Advent of Cyber 2024 – Day 2: Log Analysis Challenge

## Challenge Overview 🎅

Day 2 was all about log analysis—a crucial part of any SOC analyst’s work. The challenge tasked us with sifting through Elastic SIEM logs to figure out whether Mayor Malware or Glitch was behind some suspicious activity. Encoded PowerShell commands, brute-force login attempts, and filtering through tons of logs made this one an engaging task!

## Objectives 🎯

1. Set up Elastic SIEM to filter and navigate the logs.  
2. Identify the attacker and their failed login attempts.  
3. Decode an encoded PowerShell command to uncover its intent.  

## Step-by-Step Process 🔍

### Setting Up Elastic SIEM

Before diving into the logs, Elastic SIEM needed a bit of setup:
1. Logged in with:
   - **Username:** elastic  
   - **Password:** elastic  
2. Went to the **Discover** tab using the three-stripe menu on the top left.
3. Adjusted the timeframe to **00:00 Nov 29, 2024 – 23:30 Dec 1, 2024** and clicked “Update.”
4. Added these fields to the filters on the left:
   - `host.hostname`
   - `source.ip`
   - `user.name`
   - `process.command_line`
   - `event.outcome`

Once this was done, I was ready to start investigating.

### Q1: What is the name of the account causing all the failed login attempts?

Using the search bar, I filtered for failed login attempts:

```
event.outcome : failure
```

The `user.name` field in the filtered results showed the culprit.

**Answer:** `service_admin`

### Q2: How many failed logon attempts were observed?

After filtering for failed logins, I checked the **top-left corner** of the chart to find the total hits.

**Answer:** `6791`

### Q3: What is the IP address of Glitch?

Glitch was said to have accessed `ADM-01`. To find their IP, I used:

```
host.hostname : ADM-01
```

Among the logs, the `source.ip` field stood out for a successful login.

**Answer:** `10.0.255.1`

### Q4: When did Glitch successfully log on to ADM-01?

To find the login timestamp, I refined the query:

```
host.hostname : ADM-01 and event.outcome : success
```

The `@timestamp` field provided the exact time.

**Answer:** `Dec 1, 2024 @ 08:54:39.0`

### Q5: What is the decoded command executed by Glitch to fix the systems of Wareville?

Next, I focused on processes to locate the encoded PowerShell command:

```
event.category : process
```

The `process.command_line` field contained the encoded string:

```
C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe -EncodedCommand SQBuAHMAdABhAGwAbAAtAFcAaQBuAGQAbwB3AHMAVQBwAGQAYQB0AGUAIAAtAEEAYwBjAGUAcAB0AEEAbABsACAALQBBAHUAdABvAFIAZQBiAG8AbwB0AA==
```

I decoded the Base64 string using **CyberChef**:
1. Pasted the string into the "Input" field.
2. Added the **FromBase64** and **Decode Text (UTF-16LE)** recipes to the "Recipe" section.

The decoded command was:

**Answer:** `Install-WindowsUpdate -AcceptAll -AutoReboot`

## Lessons Learned 🌟

- **Context Matters:** Logs can be misleading without proper context. It’s easy to jump to conclusions without analyzing the full picture.
- **Filters Are Your Friend:** Narrowing down the dataset is crucial for uncovering patterns in large amounts of data.
- **Don’t Assume Malice:** Not all unusual behavior is malicious, as we saw with Glitch fixing the system instead of causing harm.

## Tools and Techniques 🛠️

- **Elastic SIEM:** To filter and navigate through logs.  
- **Search Queries:** Used queries to pinpoint specific events and actions.  
- **CyberChef:** Decoded the Base64-encoded PowerShell command quickly and easily.  

## Final Thoughts 🎁

This challenge emphasized the importance of log analysis and thoughtful investigation. Glitch turned out to be a surprising hero rather than a villain, fixing the system by updating Windows credentials. It’s a great reminder to approach cybersecurity incidents with curiosity and not jump to conclusions. Bring on Day 3!

## Table of Contents

- [Main Page](README.md)
- [Day 1: OPSEC Challenge](day1.md)
- [Day 3: Log Analysis](Day3.md)
- [Day 4: Coming Soon!](Day4.md)
- [Day 5: Coming Soon!](Day5.md)
- [More Days to Come!](#)

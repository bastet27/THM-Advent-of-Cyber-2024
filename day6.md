# 🎄 TryHackMe Advent of Cyber 2024 – Day 6: Sandboxes and Malware Analysis Challenge

## Challenge Overview 🎅

Day 6 dives into the intriguing world of **malware analysis** and **sandbox environments**, where attackers test their creations against detection mechanisms. Mayor Malware is fine-tuning his malware in a sandbox, attempting to bypass detection by tools like **YARA** and **FLOSS**, while we analyze his tricks to uncover flags.

## Objectives 🎯

1. Detect and analyze malware in a sandbox environment.
2. Explore YARA rules to identify malicious patterns.
3. Understand malware evasion techniques and use **FLOSS** to decode obfuscated data.

## Key Concepts 💡

### What is a Sandbox?
- A sandbox is an isolated environment for safely running suspicious files or programs without affecting the system or network.

### YARA Rules
- YARA identifies malware patterns based on specific strings or conditions in the code. Analysts write custom rules to detect malicious behavior effectively.

### FLOSS
- FLOSS extracts obfuscated strings from malware binaries, revealing concealed information like commands or file paths.

## Step 1: Detecting Malware in the Sandbox

### Initial Setup
1. **Start the Sandbox Environment:**
   - Launch the virtual machine and connect via the provided credentials.
2. **Monitor for Events:**
   - Open PowerShell and navigate to `C:\Tools`.
   - Run the script:  
     ```
     .\JingleBells.ps1
     ```
   - This script monitors logs for registry queries and triggers alerts.

### Executing the Malware
1. **Run the Malware:**
   - Navigate to `C:\Tools\Malware` and execute `MerryChristmas.exe`.
2. **Observe the EDR Alert:**
   - A popup window appears with the flag after detecting the malware.

**Answer:** `THM{D3t3ction_4ctiv3}`

## Step 2: Extracting Obfuscated Strings with FLOSS

### Using FLOSS
1. **Run FLOSS:**
   - Open PowerShell and navigate to `C:\Tools\FLOSS`.
   - Execute the command:  
     ```
     floss.exe C:\Tools\Malware\MerryChristmas.exe | Out-File C:\tools\malstrings.txt
     ```
2. **Analyze the Output:**
   - Open `malstrings.txt` in a text editor.
   - Search for obfuscated strings related to the malware.

**Answer:** `THM{M4lw4r3_F1nn3s3}`

## Step 3: Analyzing Sysmon Logs with YARA

### Investigating Sysmon Logs
1. **Check the EDR Log:**
   - Run the command:  
     ```
     Get-Content C:\Tools\YaraMatches.txt
     ```
   - Note the `Event Record ID` for the detected event.
2. **Filter Sysmon Logs:**
   - Open the **Windows Event Viewer** and navigate to:
     ```
     Applications and Services Logs > Microsoft > Windows > Sysmon > Operational
     ```
   - Apply a filter using the `Event Record ID`:
     - Go to **Filter Current Log** > **XML** > **Edit Query Manually** and enter:
       ```
       <QueryList>
         <Query Id="0" Path="Microsoft-Windows-Sysmon/Operational">
           <Select Path="Microsoft-Windows-Sysmon/Operational">
             *[System[(EventRecordID="INSERT_EVENT_RECORD_ID_HERE")]]
           </Select>
         </Query>
       </QueryList>
       ```
   - Replace `INSERT_EVENT_RECORD_ID_HERE` with the noted Event Record ID.
3. **Analyze the Event Details:**
   - Review keys like `ParentImage`, `CommandLine`, and `UtcTime` to trace the malware's execution path.

## Lessons Learned 🌟

### Key Takeaways
1. **Understanding Sandboxes:**
   - Sandboxes provide an isolated environment to study malware behavior without endangering the host system.
2. **YARA Rules:**
   - Effective for detecting malicious patterns, YARA rules rely on precise definitions to identify threats.
3. **Malware Evasion Techniques:**
   - Obfuscation, such as encoding commands in Base64, can bypass basic detection but tools like FLOSS counteract this tactic.
4. **Sysmon Insights:**
   - Sysmon logs offer a wealth of information for tracing malware actions, including process creation and network activity.

### Mitigation Strategies
1. **Strengthen YARA Rules:**
   - Update YARA rules regularly to detect emerging threats.
2. **Leverage Advanced Tools:**
   - Use FLOSS and Sysmon in combination for comprehensive malware analysis.
3. **Conduct Routine Threat Hunts:**
   - Regularly analyze logs and system activities to identify anomalies.

## Tools Used 🛠️

1. **YARA:** Detected registry-related events triggered by the malware.
2. **FLOSS:** Extracted obfuscated strings from the malware binary.
3. **Sysmon:** Monitored and logged malware activity for detailed analysis.

## Final Thoughts 🎁

Day 6 demonstrated the complexities of malware detection and evasion. While tools like sandboxes, YARA, and FLOSS are powerful, attackers continually innovate to bypass them. The challenge emphasized:
- The importance of layered defenses.
- The role of vigilant threat hunting.
- Continuous learning to stay ahead of attackers.

As defenders, we must adapt as rapidly as the threats we face.

## Questions and Answers

1. **What is the flag displayed in the popup window after the EDR detects the malware?**  
   **Answer:** `THM{D3t3ction_4ctiv3}`

2. **What is the flag found in the malstrings.txt document after running floss.exe?**  
   **Answer:** `THM{M4lw4r3_F1nn3s3}`

## Table of Contents 📚

- [Day 1: OPSEC Challenge](day1.md)
- [Day 2: Log Analysis](day2.md)
- [Day 3: Log Analysis](day3.md)
- [Day 4: Atomic Red Team](day4.md)
- [Day 5: XXE Exploitation](day5.md)
- [Day 7: Coming Soon!](day7.md)
- [More Days to Come!](README.md)
```

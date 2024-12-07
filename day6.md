# 🎄 TryHackMe Advent of Cyber 2024 – Day 6: Sandboxes and Malware Analysis Challenge

## Challenge Overview 🎅

Day 6 dives into the intriguing world of **malware analysis** and **sandbox environments**, where attackers test their creations against detection mechanisms. Mayor Malware is fine-tuning his malware in a sandbox, attempting to bypass detection by tools like **YARA** and **FLOSS**, while we analyze his tricks to uncover flags.

## Objectives 🎯

1. Detect and analyze malware in a sandbox environment.
2. Explore YARA rules to identify malicious patterns.
3. Use FLOSS to decode obfuscated data and uncover hidden information.

## Key Concepts 💡

### What is a Sandbox?
- A sandbox is an isolated environment for safely running suspicious files or programs without affecting the system or network.

### YARA Rules
- YARA identifies malware patterns based on specific strings or conditions in the code. Analysts write custom rules to detect malicious behavior effectively.

### FLOSS
- FLOSS extracts obfuscated strings from malware binaries, revealing concealed information like commands or file paths.

## Step 1: Detecting Malware in the Sandbox

### Process
1. **Start the EDR Tool:**
   - Open PowerShell, navigate to `C:\Tools`, and run:
     ```
     .\JingleBells.ps1
     ```
   - This script monitors the system and triggers alerts when suspicious activity is detected.
2. **Execute the Malware:**
   - Navigate to `C:\Tools\Malware`.
   - Double-click on `MerryChristmas.exe`.
3. **Observe the Popup Alert:**
   - The EDR will display a popup containing the first flag.

**Answer:** `THM{GlitchWasHere}`

## Step 2: Extracting Obfuscated Strings with FLOSS

### Process
1. **Run FLOSS:**
   - Open PowerShell and navigate to `C:\Tools\FLOSS`.
   - Execute the command:
     ```
     floss.exe C:\Tools\Malware\MerryChristmas.exe | Out-File C:\tools\malstrings.txt
     ```
2. **Analyze the Output:**
   - Open `malstrings.txt` in a text editor.
   - Use `CTRL+F` to search for the string **Mayor Malware**.
   - The second flag will be revealed in the file.

**Answer:** `THM{HiddenClue}`

## Lessons Learned 🌟

### Key Takeaways
1. **Understanding Sandboxes:**
   - Sandboxes provide an isolated environment to study malware behavior without endangering the host system.
2. **Using FLOSS for Obfuscation:**
   - FLOSS effectively extracts hidden strings, countering obfuscation techniques like Base64 encoding.
3. **Proactive Detection:**
   - YARA rules and real-time monitoring via EDR tools are crucial for detecting and stopping malware in its early stages.

### Mitigation Strategies
1. **Strengthen YARA Rules:**
   - Regular updates are necessary to identify emerging threats.
2. **Layered Security:**
   - Use a combination of sandboxes, YARA, and tools like FLOSS for comprehensive threat detection.

## Tools Used 🛠️

1. **YARA:** Detected registry-related events triggered by the malware.
2. **FLOSS:** Extracted obfuscated strings from the malware binary.
3. **Sysmon:** Monitored and logged malware activity for detailed analysis.

## Final Thoughts 🎁

Day 6 provided practical experience with malware analysis and detection in sandbox environments. As attackers evolve their tactics, defenders must continuously improve their skills and tools to stay one step ahead. Key takeaways include:
- The importance of understanding evasion techniques.
- Leveraging tools like FLOSS and YARA for effective analysis.
- Maintaining a proactive approach to threat detection.

## Questions and Answers

1. **What is the flag displayed in the popup window after the EDR detects the malware?**  
   **Answer:** `THM{GlitchWasHere}`

2. **What is the flag found in the malstrings.txt document after running floss.exe?**  
   **Answer:** `THM{HiddenClue}`

## Table of Contents 📚

- [Day 1: OPSEC Challenge](day1.md)
- [Day 2: Log Analysis](day2.md)
- [Day 3: Log Analysis](day3.md)
- [Day 4: Atomic Red Team](day4.md)
- [Day 5: XXE Exploitation](day5.md)
- [Day 7: Coming Soon!](day7.md)
- [More Days to Come!](README.md)

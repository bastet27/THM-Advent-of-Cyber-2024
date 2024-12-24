# 🎄 TryHackMe Advent of Cyber 2024 - Day 21: Reverse Engineering

## Objectives 🎯
- Understand the structure of a binary file.
- Differentiate between disassembly and decompiling.
- Familiarize with multi-stage binaries and their behavior.
- Reverse-engineer a multi-stage binary to uncover its functionality.

## Steps 🚀
1. **Analyze the Binary with PEStudio:**
   - Open PEStudio and load the `WarevilleApp.exe` located at `C:\Users\Administrator\Desktop\`.
   - Extract key metadata, such as file hashes, sections, and indicators, to understand the binary's structure.

2. **Decompile the Binary with ILSpy:**
   - Open ILSpy and load `WarevilleApp.exe`.
   - Explore the decompiled code to understand the main function and any critical operations performed by the binary.

3. **Inspect the Binary Execution:**
   - Execute `WarevilleApp.exe` in the sandbox environment and monitor its actions.
   - Note any files downloaded, created, or executed during its runtime.

4. **Identify Stages of Execution:**
   - Investigate the second-stage binary downloaded to the `Downloads` folder.
   - Analyze the actions performed by the second-stage binary, such as file creation and C2 communication.

5. **Track C2 Server Communication:**
   - Use decompilation and execution observations to identify the C2 server and data exfiltration methods.

## Key Findings 🔑
- **Stage 1 Binary (`WarevilleApp.exe`):**
  - The main function, `DownloadAndExecuteFile`, downloads and executes a secondary binary, `explorer.exe`.
  - The file is downloaded from the domain `mayorc2.thm`.

- **Stage 2 Binary (`explorer.exe`):**
  - Creates a zip file, `CollectedFiles.zip`, containing victim's computer data.
  - Attempts to upload the zip file to the C2 server `anonymousc2.thm`.

## Lessons Learned 🌟
- **Binary Analysis:** Tools like PEStudio and ILSpy are essential for static analysis and understanding binary behavior.
- **Multi-Stage Binaries:** Multi-stage binaries complicate analysis and detection, making reverse engineering critical.
- **Controlled Execution:** Running binaries in a sandbox environment allows for safe observation of malicious behaviors.

### Tools and Techniques 🛠️
- **PEStudio:** For static analysis of Windows binaries, including metadata and indicators.
- **ILSpy:** For decompiling .NET binaries to understand the high-level logic.
- **Sandbox Environment:** Safe execution of suspicious binaries for behavioral analysis.

## Final Thoughts 🎁
This task provided valuable insights into reverse engineering techniques, including static and dynamic analysis. By decompiling and executing the binary in a sandbox, we uncovered its malicious actions and C2 communication.

### Answers ✅
1. **What is the function name that downloads and executes files in the WarevilleApp.exe?**  
   `DownloadAndExecuteFile`
2. **Once you execute the WarevilleApp.exe, it downloads another binary to the Downloads folder. What is the name of the binary?**  
   `explorer.exe`
3. **What domain name is the one from where the file is downloaded after running WarevilleApp.exe?**  
   `mayorc2.thm`
4. **The stage 2 binary is executed automatically and creates a zip file comprising the victim's computer data; what is the name of the zip file?**  
   `CollectedFiles.zip`
5. **What is the name of the C2 server where the stage 2 binary tries to upload files?**  
   `anonymousc2.thm`

## Table of Contents 📚

- [Main Page](README.md)
- [Day 1: OPSEC Challenge](day1.md)
- [Day 2: Log Analysis](day2.md)
- [Day 3: Log Analysis](day3.md)
- [Day 4: Atomic Red Team](day4.md)
- [Day 5: XXE](day5.md)
- [Day 6: Sandboxes](day6.md)
- [Day 7: AWS Sandboxes](day7.md)
- [Day 8: Shellcodes](day8.md)
- [Day 9: Risk Assessment](day9.md)
- [Day 10: Phishing](day_10.md)
- [Day 11: Wi-Fi Attacks](day_11.md)
- [Day 12: Web Timing Attacks](day_12.md)
- [Day 13: WebSockets](day_13.md)
- [Day 14: Certificate Mismanagement](day_14.md)
- [Day 15: Active Directory](day_15.md)
- [Day 16: Azure Exploitation](day_16.md)
- [Day 17: Log Analysis](day_17.md)
- [Day 18: Prompt Injection](day_18.md)
- [Day 19: Game Hacking](day_19.md)
- [Day 20: Traffic Analysis](day_20.md)
- **Day 21: Reverse Engineering**
- [More Days to Come!](#)

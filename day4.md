# 🎄 TryHackMe Advent of Cyber 2024 – Day 4: Atomic Red Team

## Objectives 🎯

1. Identify artifacts created by a simulated spear-phishing attack.
2. Explore MITRE ATT&CK techniques and sub-techniques.
3. Simulate a ransomware attack and analyze the artifacts generated.
4. Create a detection rule to address gaps in monitoring and detection.

## Steps 🚀

### Step 1: Spear-Phishing Simulation

#### Running the Test
1. Executed the spear-phishing simulation using:
   ```
   Invoke-AtomicTest T1566.001 -TestNumbers 1
   ```
2. Located the generated file in:
   ```
   C:\Users\Administrator\AppData\Local\Temp\
   ```

#### Results
- **Flag found in the phishing artifact directory:**  
  **Answer:** `THM{GlitchTestingForSpearphishing}`

### Step 2: Exploring MITRE ATT&CK Techniques

#### Technique and Sub-Technique
1. **Identified the relevant technique ID:**  
   **Answer:** `T1059`
2. **Identified the sub-technique ID for Windows Command Shell:**  
   **Answer:** `T1059.003`

### Step 3: Simulating a Ransomware Attack

#### Running the Test
1. Displayed the details of the ransomware test using:
   ```
   Invoke-AtomicTest T1059.003 -ShowDetails
   ```
   - **Name of the Test:**  
     **Answer:** `Simulate BlackByte Ransomware Print Bombing`
   - **Name of the File Used:**  
     **Answer:** `Wareville_Ransomware.txt`
2. Simulated the ransomware attack:
   ```
   Invoke-AtomicTest T1059.003 -TestNumbers 4
   ```
3. Located the flag in the output.

#### Results
- **Flag from the ransomware simulation:**  
  **Answer:** `THM{R2xpdGNoIGlzIG5vdCB0aGUgZW5lbXk=}`

## Key Findings 🔑

1. **Spear-Phishing Artifacts:** The simulation created an artifact in a known location with a clear indicator of compromise.
2. **Windows Command Shell Usage:** The attack leveraged PowerShell commands, highlighting the need for robust process monitoring.
3. **Ransomware Activity:** The simulated ransomware created identifiable artifacts that can inform detection strategies.

## Lessons Learned 🌟

### MITRE ATT&CK Framework
- Provides a structured way to understand attacker behaviors and map them to defensive strategies.

### Threat Simulation Benefits
- **Gaps in Detection:** Simulations reveal blind spots in current monitoring setups.
- **Custom Detection Rules:** Artifacts from simulations can directly inform the creation of new detection rules.

### Crafting Effective Defenses
- Monitor suspicious PowerShell activity (`Invoke-WebRequest` is a red flag).
- Keep logs from tools like Sysmon centralized for analysis.

### Tools and Techniques 🛠️

1. **[Atomic Red Team](https://github.com/redcanaryco/atomic-red-team):** Conducted tests emulating adversarial techniques.  
2. **[MITRE ATT&CK](https://attack.mitre.org/):** Referenced techniques and sub-techniques for detailed analysis.  
3. **Sysmon:** Monitored processes and network connections during simulations.  
4. **Windows Event Viewer:** Analyzed logs and events generated during the attack.  

## Final Thoughts 🎁

Day 4 showcased the value of adversary emulation in identifying and addressing detection gaps. Simulating attacks like spear-phishing and ransomware provided critical insights into how attackers operate and how defenders can build stronger detection and prevention mechanisms. Combining tools like Atomic Red Team with MITRE ATT&CK offers a powerful approach to improving security posture.

### Answers ✅

1. **Flag found in the phishing artifact directory:**  
   **Answer:** `THM{GlitchTestingForSpearphishing}`
2. **What ATT&CK technique ID would be our point of interest?**  
   **Answer:** `T1059`
3. **What ATT&CK sub-technique ID focuses on the Windows Command Shell?**  
   **Answer:** `T1059.003`
4. **What is the name of the Atomic Test to be simulated?**  
   **Answer:** `Simulate BlackByte Ransomware Print Bombing`
5. **What is the name of the file used in the test?**  
   **Answer:** `Wareville_Ransomware.txt`
6. **What is the flag found from this Atomic Test?**  
   **Answer:** `THM{R2xpdGNoIGlzIG5vdCB0aGUgZW5lbXk=}`

## Table of Contents 📚

- [Main Page](README.md)
- [Day 1: OPSEC Challenge](day1.md)
- [Day 2: Log Analysis](day2.md)
- [Day 3: Log Analysis](day3.md)
- **Day 4: Atomic Red Team**
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
- [More Days to Come!](#)

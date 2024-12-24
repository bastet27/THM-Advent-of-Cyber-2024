# 🎄 TryHackMe Advent of Cyber 2024 - Day 23: Hash Cracking

## Objectives 🎯
- Understand how hash functions work and their application in password storage.
- Learn the process of hash cracking using wordlists and tools.
- Crack a password-protected PDF file to uncover its contents.

## Steps 🚀

### **1. Crack the Password Hash from the Data Breach**
1. Navigate to the directory containing `hash1.txt`:
   ```
   cd ~/AOC2024/
   ```
2. Display the contents of `hash1.txt` to get the hash value:
   ```
   cat hash1.txt
   ```
3. Use the `hash-id.py` tool to identify the hash type:
   ```
   python hash-id.py
   ```
   - Result: The hash type is identified as `SHA-256`.
4. Use John the Ripper to crack the hash using `rockyou.txt`:
   ```
   john --format=raw-sha256 --wordlist=/usr/share/wordlists/rockyou.txt hash1.txt
   ```
5. If no result, apply additional rules to expand possibilities:
   ```
   john --format=raw-sha256 --rules=wordlist --wordlist=/usr/share/wordlists/rockyou.txt hash1.txt
   ```
6. Retrieve the cracked password using:
   ```
   john --format=raw-sha256 --show hash1.txt
   ```

### **2. Crack the Password-Protected PDF**
1. Use `pdf2john.pl` to extract the hash from the PDF file:
   ```
   pdf2john.pl private.pdf > pdf.hash
   ```
2. Attempt cracking the PDF password using `rockyou.txt`:
   ```
   john --wordlist=/usr/share/wordlists/rockyou.txt pdf.hash
   ```
3. If unsuccessful, use a custom wordlist:
   ```
   john --rules=single --wordlist=wordlist.txt pdf.hash
   ```
4. Extract the cracked password using:
   ```
   john --show pdf.hash
   ```
5. Open the PDF using the cracked password and retrieve the flag:
   ```
   evince private.pdf
   ```

## Key Findings 🔑
- **Password for hash1.txt:** `fluffycat12`
- **Flag in private.pdf:** `THM{do_not_GET_CAUGHT}`

## Lessons Learned 🌟
- **Password Security:** Always use salted hashes and avoid reusing passwords across platforms.
- **Hash Cracking Techniques:** Tools like John the Ripper are powerful for cracking hashes, especially with the help of custom rules and wordlists.
- **Custom Wordlists:** Tailoring wordlists based on contextual information about the target can significantly improve success rates.

### Tools and Techniques 🛠️
- **Hash Identification:** `hash-id.py`
- **Hash Cracking:** John the Ripper with wordlists and rules.
- **PDF Hash Extraction:** `pdf2john.pl`
- **Custom Wordlists:** Context-specific wordlists to target likely passwords.

## Final Thoughts 🎁
This challenge demonstrated the importance of robust password practices and how attackers exploit weak hash implementations. By combining technical tools with contextual reasoning, we successfully uncovered Mayor Malware's secrets.

### Answers ✅
1. **Crack the hash value stored in hash1.txt. What was the password?**  
   `fluffycat12`
2. **What is the flag at the top of the private.pdf file?**  
   `THM{do_not_GET_CAUGHT}`
3. **To learn more about cryptography, we recommend the Cryptography module. If you want to practice more hash cracking, please consider the John the Ripper: The Basics room.**  
   No answer needed.

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
- [Day 21: Reverse Engineering](day_21.md)
- [Day 22: Kubernetes DFIR](day_22.md)
- **Day 23: Hash Cracking**
- [More Days to Come!](#)

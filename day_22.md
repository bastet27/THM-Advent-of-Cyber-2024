# 🎄 TryHackMe Advent of Cyber 2024 - Day 22: Kubernetes DFIR

## Objectives 🎯
- Understand Kubernetes and its role in container orchestration.
- Learn the basics of Digital Forensics and Incident Response (DFIR) in a Kubernetes environment.
- Investigate and analyze Kubernetes logs to uncover Mayor Malware's actions.
- Trace the attack path using Kubernetes audit logs and Docker registry logs.

## Steps 🚀
1. **Start the Kubernetes Cluster:**
   - Use `minikube start` to initialize the cluster and verify that all pods are running.

2. **Investigate the Compromised Pod:**
   - Connect to the `naughty-or-nice` pod using `kubectl exec`.
   - Analyze the Apache2 access logs and identify suspicious activity (e.g., access to `shelly.php`).

3. **Analyze Backups:**
   - Navigate to `/home/ubuntu/dfir_artefacts/` to review archived logs:
     - `pod_apache2_access.log` for pod activity.
     - `docker-registry-logs.log` for registry interactions.

4. **Trace Malicious Activity in the Registry:**
   - Identify unexpected connections to the Docker registry using `grep` and filter for suspicious IPs.
   - Investigate HTTP requests from the malicious IP (`10.10.130.253`) to detect unauthorized image modifications.

5. **Examine Kubernetes Audit Logs:**
   - Parse the `audit.log` file to trace Mayor Malware’s actions, such as:
     - Attempting to access secrets.
     - Describing roles and role bindings.
     - Exploiting the `exec` permission to gain pod access.

6. **Confirm Exploitation Path:**
   - Verify how Mayor Malware escalated privileges using the `job-runner-sa` service account.
   - Confirm that the `pull-creds` secret contained both pull and push credentials.

## Key Findings 🔑
- **Webshell Used:** `shelly.php` 
- **Sensitive File Accessed:** `db.php`
- **Tool Searched:** `nc` (netcat)
- **Malicious Registry IP:** `10.10.130.253`
- **Timeline:**
  - First connection to Docker registry: `29/Oct/2024:10:06:33 +0000`
  - Malicious image pushed: `29/Oct/2024:12:34:28 +0000`
- **Pull-Creds Secret Value:**  
  `{“auths”:{“http://docker-registry.nicetown.loc:5000":{"username":"mr.nice","password":"Mr.N4ughty","auth":"bXIubmljZTpNci5ONHVnaHR5"}}}`

## Lessons Learned 🌟
- **RBAC Misconfigurations:** Overly permissive roles, such as allowing `exec` permissions, can lead to privilege escalation.
- **Credential Mismanagement:** Sharing pull and push credentials can allow attackers to modify registry images.
- **Kubernetes Visibility:** Enabling audit logging and maintaining backup logs is critical for detecting and analyzing incidents.

### Tools and Techniques 🛠️
- **Kubernetes:** For managing containers and investigating cluster activities.
- **Docker Logs:** For analyzing registry activity and tracing malicious actions.
- **Log Analysis:** Using commands like `grep` and `cut` for filtering and parsing large logs.
- **Audit Logs:** To track user actions and identify security misconfigurations.

## Final Thoughts 🎁
This task highlighted the challenges of DFIR in ephemeral Kubernetes environments and the importance of strong RBAC configurations. By thoroughly investigating logs, we traced Mayor Malware's steps and uncovered critical misconfigurations that allowed the attack.

### Answers ✅
1. **What is the name of the webshell that was used by Mayor Malware?**  
   `shelly.php`
2. **What file did Mayor Malware read from the pod?**  
   `db.php`
3. **What tool did Mayor Malware search for that could be used to create a remote connection from the pod?**  
   `nc`
4. **What IP connected to the docker registry that was unexpected?**  
   `10.10.130.253`
5. **At what time is the first connection made from this IP to the docker registry?**  
   `29/Oct/2024:10:06:33 +0000`
6. **At what time is the updated malicious image pushed to the registry?**  
   `29/Oct/2024:12:34:28 +0000`
7. **What is the value stored in the "pull-creds" secret?**  
   `{“auths”:{“http://docker-registry.nicetown.loc:5000":{"username":"mr.nice","password":"Mr.N4ughty","auth":"bXIubmljZTpNci5ONHVnaHR5"}}}`

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
- **Day 22: Kubernetes DFIR**
- [More Days to Come!](#)

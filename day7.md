# 🎄 TryHackMe Advent of Cyber 2024 – Day 7: AWS Log Analysis

## Objectives 🎯

Day 7 focuses on **AWS log analysis** and **investigation techniques**. The challenge is to analyze CloudTrail and RDS logs to uncover suspicious activity within an AWS environment. The goal is to:
- Understand the structure of AWS CloudTrail logs.
- Use `jq` to filter and analyze JSON-formatted log data.
- Investigate anomalous user activities and identify malicious actions.
- Tie together findings to build a comprehensive incident timeline.
  
## Steps 🚀

### Step 1: Reviewing CloudTrail Logs
- **Navigate to the logs directory:**  
  I began by entering the `wareville_logs` directory, where the CloudTrail logs (`cloudtrail_log.json`) were stored.
- **Filter relevant S3 bucket activities:**  
  I used the following command to filter for events related to the `wareville-care4wares` S3 bucket:
  ```
  jq -r '.Records[] | select(.eventSource == "s3.amazonaws.com" and .requestParameters.bucketName=="wareville-care4wares")' cloudtrail_log.json
  ```
- **Refine the output:**  
  I added specific fields to narrow down the results:
  ```
  jq -r '["Event_Time", "Event_Name", "User_Name", "Bucket_Name", "Key", "Source_IP"], (.Records[] | select(.eventSource == "s3.amazonaws.com" and .requestParameters.bucketName=="wareville-care4wares") | [.eventTime, .eventName, .userIdentity.userName // "N/A", .requestParameters.bucketName // "N/A", .requestParameters.key // "N/A", .sourceIPAddress // "N/A"]) | @tsv' cloudtrail_log.json | column -t
  ```

### Step 2: Investigating the Anomalous User
- **Focus on the `glitch` user:**  
  Filtering all actions tied to the `glitch` user revealed they performed two actions:
  - `ListObjects` (listing objects in the bucket).
  - `PutObject` (uploading a new file).
  ```
  jq -r '["Event_Time", "Event_Source", "Event_Name", "User_Name", "Source_IP"], (.Records[] | select(.userIdentity.userName == "glitch") | [.eventTime, .eventSource, .eventName, .userIdentity.userName // "N/A", .sourceIPAddress // "N/A"]) | @tsv' cloudtrail_log.json | column -t
  ```

### Step 3: Analyzing the IAM Events
- **Check who created the anomalous user:**  
  Filtering for `CreateUser` and `AttachUserPolicy` events revealed the `mcskidy` user created the `glitch` user and granted them `AdministratorAccess` permissions:
  ```
  jq '.Records[] | select(.eventSource=="iam.amazonaws.com" and .eventName== "CreateUser")' cloudtrail_log.json
  jq '.Records[] | select(.eventSource=="iam.amazonaws.com" and .eventName== "AttachUserPolicy")' cloudtrail_log.json
  ```

### Step 4: Cross-Referencing IP Addresses
- **Confirm source IPs:**  
  I focused on identifying the IP addresses used by different users, revealing the same IP (`53.94.201.69`) was used for the malicious actions:
  ```
  jq -r '["Event_Time", "Event_Source", "Event_Name", "User_Name", "Source_IP"], (.Records[] | select(.sourceIPAddress=="53.94.201.69") | [.eventTime, .eventSource, .eventName, .userIdentity.userName // "N/A", .sourceIPAddress // "N/A"]) | @tsv' cloudtrail_log.json | column -t
  ```

### Step 5: Investigating RDS Logs
- **Analyze donation recipients:**  
  Using the `grep` command, I identified a sudden change in donation recipients on November 28:
  ```
  grep INSERT rds.log
  ```

## Key Findings 🔑

1. **Malicious User Actions:**  
   The `glitch` user uploaded a tampered bank account flyer to the S3 bucket, redirecting donations to a different account.
2. **Source IP:**  
   All malicious actions originated from `53.94.201.69`, which was used by `mcskidy`, `glitch`, and `mayor_malware`.
3. **IAM Misuse:**  
   The `mcskidy` user was impersonated to create the `glitch` user and assign administrative privileges.
4. **Donation Hijacking:**  
   Donations initially intended for the charity were redirected to a bank account owned by Mayor Malware.

## Lessons Learned 🌟

### Key Takeaways
- **Log Analysis Matters:**  
  Investigating CloudTrail and RDS logs provided clear evidence of malicious activity.
- **IAM Misconfigurations:**  
  Monitoring IAM events is critical to detecting unauthorized user creation and policy changes.
- **The Importance of IP Correlation:**  
  Tracking IP addresses helped identify the true origin of malicious actions.

### Tools and Techniques 🛠️
1. [JQ](https://stedolan.github.io/jq/) – Processed and filtered JSON-formatted CloudTrail logs.
2. [grep](https://www.gnu.org/software/grep/) – Searched RDS logs for specific entries.
3. **AWS CloudTrail Logs** – Tracked user actions within the AWS environment.
4. **RDS Logs** – Provided insights into transaction activities.


## Final Thoughts 🎁

This challenge highlighted the importance of log analysis in cloud environments. By combining tools like JQ and grep, I was able to extract meaningful data from large log files. Tracking down the attacker emphasized the value of correlating IAM events, IP addresses, and user behavior to build a complete picture of the incident. Another great challenge!

### Answers ✅

1. **What is the other activity made by the user glitch aside from the ListObject action?**  
   **Answer:** `PutObject`
2. **What is the source IP related to the S3 bucket activities of the user glitch?**  
   **Answer:** `53.94.201.69`
3. **Based on the eventSource field, what AWS service generates the ConsoleLogin event?**  
   **Answer:** `signin.amazonaws.com`
4. **When did the anomalous user trigger the ConsoleLogin event?**  
   **Answer:** `2024-11-28T15:21:54Z`
5. **What was the name of the user that was created by the mcskidy user?**  
   **Answer:** `glitch`
6. **What type of access was assigned to the anomalous user?**  
   **Answer:** `AdministratorAccess`
7. **Which IP does Mayor Malware typically use to log into AWS?**  
   **Answer:** `53.94.201.69`
8. **What is McSkidy's actual IP address?**  
   **Answer:** `31.210.15.79`
9. **What is the bank account number owned by Mayor Malware?**  
   **Answer:** `2394 6912 7723 1294`

## Table of Contents 📚

- [Main Page](README.md)
- [Day 1: OPSEC Challenge](day1.md)
- [Day 2: Log Analysis](day2.md)
- [Day 3: Log Analysis](day3.md)
- [Day 4: Atomic Red Team](day4.md)
- [Day 5: XXE Exploitation](day5.md)
- [Day 6: Sandboxes](day6.md)
- [Day 7: AWS Log Analysis](day7.md)
- [More Days to Come!](#)
```

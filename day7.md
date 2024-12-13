# 🎄 TryHackMe Advent of Cyber 2024 – Day 7: AWS Log Analysis

## Objectives 🎯

1. Analyze AWS CloudTrail logs to identify anomalous user activity.  
2. Use `jq` to filter and extract meaningful information from JSON-formatted logs.  
3. Investigate RDS logs to uncover donation hijacking.  
4. Correlate IAM, IP, and user activities to build a comprehensive incident timeline.

## Steps 🚀

### Reviewing CloudTrail Logs
- **Filter for S3 Bucket Activities:**  
  Using `jq`, I filtered the logs for activities related to the `wareville-care4wares` S3 bucket:
  ```
  jq -r '.Records[] | select(.eventSource == "s3.amazonaws.com" and .requestParameters.bucketName=="wareville-care4wares")' cloudtrail_log.json
  ```
- **Refine the Results:**  
  Added relevant fields to focus on timestamps, event names, and user activity:
  ```
  jq -r '["Event_Time", "Event_Name", "User_Name", "Bucket_Name", "Key", "Source_IP"], (.Records[] | select(.eventSource == "s3.amazonaws.com" and .requestParameters.bucketName=="wareville-care4wares") | [.eventTime, .eventName, .userIdentity.userName // "N/A", .requestParameters.bucketName // "N/A", .requestParameters.key // "N/A", .sourceIPAddress // "N/A"]) | @tsv' cloudtrail_log.json | column -t
  ```

### Investigating the Anomalous User
- **Focus on `glitch`:**  
  By filtering for actions by the `glitch` user, I discovered two key events:
  - `ListObjects`
  - `PutObject`
  ```
  jq -r '["Event_Time", "Event_Source", "Event_Name", "User_Name", "Source_IP"], (.Records[] | select(.userIdentity.userName == "glitch") | [.eventTime, .eventSource, .eventName, .userIdentity.userName // "N/A", .sourceIPAddress // "N/A"]) | @tsv' cloudtrail_log.json | column -t
  ```

### Examining IAM Events
- **Identify the Creator of `glitch`:**  
  Filtered for `CreateUser` and `AttachUserPolicy` events to confirm that `mcskidy` was impersonated to create and assign `AdministratorAccess` to `glitch`:
  ```
  jq '.Records[] | select(.eventSource=="iam.amazonaws.com" and .eventName== "CreateUser")' cloudtrail_log.json
  jq '.Records[] | select(.eventSource=="iam.amazonaws.com" and .eventName== "AttachUserPolicy")' cloudtrail_log.json
  ```

### Verifying Source IPs
- **Track Malicious Activity:**  
  Correlated IP addresses across actions to reveal that `53.94.201.69` was used by `mcskidy`, `glitch`, and `mayor_malware`:
  ```
  jq -r '["Event_Time", "Event_Source", "Event_Name", "User_Name", "Source_IP"], (.Records[] | select(.sourceIPAddress=="53.94.201.69") | [.eventTime, .eventSource, .eventName, .userIdentity.userName // "N/A", .sourceIPAddress // "N/A"]) | @tsv' cloudtrail_log.json | column -t
  ```

### Investigating RDS Logs
- **Identify Donation Changes:**  
  Using `grep`, I pinpointed a sudden switch in donation recipients on November 28:
  ```
  grep INSERT rds.log
  ```
  The logs showed donations being redirected to an account owned by Mayor Malware.

## Key Findings 🔑

1. **Malicious Actions by `glitch`:**  
   Uploaded a tampered bank account flyer to the S3 bucket, redirecting donations.
2. **Consistent Source IP:**  
   `53.94.201.69` was used for all malicious activities, including IAM changes and S3 uploads.
3. **Misuse of IAM:**  
   The `mcskidy` account was impersonated to create and elevate `glitch`.
4. **Donation Hijacking:**  
   Donations meant for the charity were diverted to an account controlled by Mayor Malware.

## Lessons Learned 🌟

1. **Importance of CloudTrail Logs:**  
   CloudTrail logs are essential for tracing user actions and identifying malicious behavior in cloud environments.
2. **IAM Oversight:**  
   Monitoring IAM activities, such as user creation and policy changes, is critical to detecting potential abuse.
3. **IP and Behavior Correlation:**  
   Tracking IP addresses across events can reveal patterns and tie malicious actions to their origin.

### Tools and Techniques 🛠️

1. [JQ](https://stedolan.github.io/jq/): Filtered and analyzed JSON-formatted logs.  
2. [grep](https://www.gnu.org/software/grep/): Searched RDS logs for specific patterns.  
3. **AWS CloudTrail Logs:** Tracked actions within the AWS environment.  
4. **RDS Logs:** Identified changes in donation recipients.  

## Final Thoughts 🎁

Day 7 reinforced the importance of log analysis and correlation in cloud environments. By leveraging tools like JQ and grep, I was able to uncover a sophisticated donation hijacking scheme. The insights gained highlight the need for strong IAM controls and regular monitoring of AWS environments.

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

- [Day 1: OPSEC Challenge](day1.md)  
- [Day 2: Log Analysis](day2.md)  
- [Day 3: Log Analysis](day3.md)  
- [Day 4: Atomic Red Team](day4.md)  
- [Day 5: XXE Exploitation](day5.md)  
- [Day 6: Sandboxes](day6.md)  
- **Day 7: AWS Log Analysis**
- [Day 8: Shellcodes](day8.md)
- [Day 9: Risk Assessment](day9.md)
- [Day 10: Phishing](day_10.md)
- [Day 11: Wi-Fi Attacks](day_11.md)
- [Day 12: Web Timing Attacks](day_12.md)
- [Day 13: Coming Soon!](day_13.md)
- [More Days to Come!](README.md)  

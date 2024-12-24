# 🎄 TryHackMe Advent of Cyber 2024 – Day 24: Communication Protocols

## Objectives 🎯
- Understand the MQTT protocol and its role in IoT communication.
- Analyze MQTT traffic using Wireshark to identify commands.
- Reverse-engineer MQTT messages to determine malicious actions.
- Publish the correct MQTT message to restore the smart lighting system.

## Steps 🚀

1. **Exploring MQTT Protocol**:
   - Learned about MQTT’s publish/subscribe model and its use in IoT devices.
   - Identified MQTT components: brokers, clients, and topics.

2. **Analyzing Captured Traffic**:
   - Opened `challenge.pcapng` in Wireshark.
   - Applied the `mqtt` filter to isolate MQTT packets.
   - Identified topics such as `lights/control` and messages like `"off"` responsible for sabotaging the lighting.

3. **Reverse-Engineering Malicious Commands**:
   - Examined MQTT messages to find the correct topic and commands.
   - Discovered that publishing `"on"` under `lights/control` would restore the lights.

4. **Restoring the Lighting System**:
   - Ran `challenge.sh` to simulate the MQTT system.
   - Published the correct message using the `mosquitto_pub` command:
     ```
     mosquitto_pub -h localhost -t "lights/control" -m "on"
     ```
   - Successfully restored the lighting system, revealing the flag.

## Key Findings 🔑
- **MQTT Protocol**: Enables communication between IoT devices through a broker and topics.
- **Vulnerability**: Unsecured MQTT communication allowed malicious commands to disrupt the system.
- **Resolution**: Publishing the appropriate command restored functionality.

## Lessons Learned 🌟
- IoT devices require secure communication protocols to prevent exploitation.
- Wireshark is a powerful tool for analyzing and troubleshooting network protocols.
- Understanding MQTT communication is essential for securing smart devices.

### Tools and Techniques 🛠️
- **Wireshark**: Used to analyze MQTT protocol traffic and detect anomalies.
- **Mosquitto_pub**: Command-line utility to publish MQTT messages and resolve issues.

## Final Thoughts 🎁
This challenge emphasized the importance of securing IoT communication protocols like MQTT. By identifying and reversing malicious commands, we restored the lighting system and ensured a brighter SOC-mas.

### Answers ✅
**What is the flag?**  
`THM{Ligh75on-day54ved}`

## Table of Contents 📚

| Day  | Challenge                              | Day  | Challenge                               | Day  | Challenge                               |
|------|----------------------------------------|------|-----------------------------------------|------|-----------------------------------------|
| 📖  | [README](../README.md)                 | 9    | [Risk Assessment](days/day9.md)         | 17   | [Log Analysis](days/day17.md)           |
| 1    | [OPSEC Challenge](days/day1.md)        | 10   | [Phishing](days/day10.md)               | 18   | [Prompt Injection](days/day18.md)       |
| 2    | [Log Analysis](days/day2.md)           | 11   | [Wi-Fi Attacks](days/day11.md)          | 19   | [Game Hacking](days/day19.md)           |
| 3    | [Log Analysis](days/day3.md)           | 12   | [Web Timing Attacks](days/day12.md)     | 20   | [Traffic Analysis](days/day20.md)       |
| 4    | [Atomic Red Team](days/day4.md)        | 13   | [WebSockets](days/day13.md)             | 21   | [Reverse Engineering](days/day21.md)    |
| 5    | [XXE](days/day5.md)                    | 14   | [Certificate Mismanagement](days/day14.md)| 22  | [Kubernetes DFIR](days/day22.md)        |
| 6    | [Sandboxes](days/day6.md)              | 15   | [Active Directory](days/day15.md)       | 23   | [Hash Cracking](days/day23.md)          |
| 7    | [AWS Sandboxes](days/day7.md)          | 16   | [Azure Exploitation](days/day16.md)     | 24   | **Communication Protocols**             |
| 8    | [Shellcodes](days/day8.md)             |      | **Merry SOC-mas!** 🎁✨☃️              |      |  🎄✨🎅                               |

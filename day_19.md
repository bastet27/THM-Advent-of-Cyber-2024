# 🎄 TryHackMe Advent of Cyber 2024 - Day 19: Game Hacking

## Objectives 🎯
- Understand how to interact with an executable's API.
- Learn how to intercept and modify internal APIs using Frida.
- Gain experience hacking a game with the help of Frida.

## Steps 🚀
1. **Setup:**
   - Start the machine and access the VM for the task.
   - Navigate to `/home/ubuntu/Desktop/TryUnlockMe` and launch the game using `./TryUnlockMe`.

2. **Level 1: Cracking the OTP Lock**
   - Start the game and interact with the penguin to trigger the `_Z7set_otpi` function.
   - Use Frida to trace the `set_otp` function and log its argument.
   - Enter the extracted OTP to unlock the first level.

3. **Level 2: Billionaire Item Purchase**
   - Explore the next stage and locate the penguin with the costly item.
   - Trace the `_Z17validate_purchaseiii` function to manipulate the item price.
   - Log the parameters to identify the item ID, price, and player's coins.
   - Set the price parameter to `0` using Frida and purchase the item for free.

4. **Level 3: Biometrics Override**
   - Trace the `_Z16check_biometricsPKc` function to bypass the biometrics check.
   - Log the string parameter and the return value of the function.
   - Use Frida to replace the return value with `True` (`1`) to bypass the check.

5. **Flags Collection:**
   - Extract the flags for each level:
     - OTP flag.
     - Billionaire item flag.
     - Biometric flag.

## Key Findings 🔑
- The game logic relies on library functions that can be intercepted and manipulated using Frida.
- By modifying arguments and return values, it is possible to bypass game challenges and achieve objectives.

## Lessons Learned 🌟
### Tools and Techniques 🛠️
- **Frida:** A dynamic instrumentation toolkit to analyze and manipulate running applications.
- **JavaScript Hooks:** Used for intercepting and modifying library function calls in real-time.
- **Memory Debugging:** Techniques for inspecting and modifying application parameters.

## Final Thoughts 🎁
This task provided a hands-on experience with game hacking using Frida. It highlighted the importance of securing library function calls and validating arguments in applications.

### Answers ✅
1. **What is the OTP flag?**  
   `THM{one_tough_password}`
2. **What is the billionaire item flag?**  
   `THM{credit_card_undeclined}`
3. **What is the biometric flag?**  
   `THM{dont_smash_your_keyboard}`

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
- **Day 19: Game Hacking**
- [More Days to Come!](#)

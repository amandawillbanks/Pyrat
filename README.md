# TCP Service Enumeration & Input Evaluation Discovery

## Overview

This project documents my methodology for enumerating and analyzing a custom TCP service discovered during a security lab exercise.

The objective was not simply to brute force credentials, but to:

- Identify exposed services  
- Enumerate protocol behavior  
- Test how input is processed  
- Automate interaction after validation  

---

## Initial Enumeration

The first step was identifying open ports and services using Nmap:

```bash
nmap -sV -sC <target_ip>
```

### Flags Used

- `-sV` — Service version detection  
- `-sC` — Default script scan (basic enumeration)  

This scan revealed a TCP service running on a non-standard port.

---

## Manual Service Interaction

Before writing any automation, I manually connected using netcat:

```bash
nc <target_ip> <port>
```

Instead of assuming it was a standard login service, I tested how the service handled arbitrary input.

---

## Behavioral Testing

I sent simple Python-like expressions to observe how the service processed input:

```python
7*7
print("hello")
```

The responses indicated that the service was **interpreting input**, rather than treating it as plain text.

This strongly suggested that user input was being evaluated dynamically.

---

## Key Observation

The service behavior implied that input was likely being passed into something similar to:

```python
eval(user_input)
```

This observation completely shifted the attack path.

Instead of brute-forcing credentials, the focus became:

- Understanding execution context  
- Controlling input safely  
- Automating interaction for reliability  

---

## Automation

After validating behavior manually, I wrote a Python script to:

- Establish a TCP connection  
- Synchronize with service prompts  
- Send controlled input  
- Capture and parse responses  
- Repeat reliably  

### Key Implementation Considerations

- One connection per interaction (service state reset)  
- Proper socket timeouts  
- Prompt synchronization  
- Avoiding false positives in output parsing  

---

## Lessons Learned

1. Enumeration always comes first.  
2. Open port does not automatically mean brute-force target.  
3. Manual interaction reveals protocol behavior.  
4. Automation should follow validation.  
5. Understanding application-layer behavior is critical in both offensive and defensive security work.  

---

## Security Takeaway

This lab reinforced the importance of:

- Carefully observing service behavior  
- Identifying unsafe input handling  
- Thinking beyond tool output  
- Analyzing how data flows through an application  

The difference between simply running tools and understanding protocol-level behavior is significant.

---

## Tools Used

- Nmap  
- Netcat  
- Python (socket programming)  

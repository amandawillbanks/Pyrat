# Pyrat
Pyrat Walkthrough
TCP Service Enumeration & Input Evaluation Discovery
Overview

This project documents my methodology for enumerating and analyzing a custom TCP service discovered during a security lab exercise.

The objective was not simply to brute force credentials, but to:

Identify exposed services

Enumerate protocol behavior

Test how input is processed

Automate interaction after validation

Initial Enumeration

The first step was identifying open ports and services using Nmap:

nmap -sV -sC <target_ip>

Flags Used

-sV → Service version detection

-sC → Default script scan (basic enumeration)

This revealed a TCP service running on a non-standard port.

Manual Service Interaction

Before writing any automation, I manually connected using netcat:

nc <target_ip> <port>


Instead of assuming it was a standard login service, I tested how the service handled arbitrary input.

Behavioral Testing

I sent simple Python-like expressions:

7*7
print("hello")


The responses indicated that the service was interpreting input, rather than treating it as plain text.

This strongly suggested that user input was being evaluated dynamically.

Key Observation

The service behavior implied that input was likely being passed into something similar to:

eval(user_input)


This shifted the attack path completely.

Instead of brute-forcing credentials, the focus became:

Understanding execution context

Controlling input safely

Automating interaction for reliability

Automation

After validating behavior manually, I wrote a Python script to:

Establish a TCP connection

Synchronize with service prompts

Send controlled input

Capture and parse responses

Repeat reliably

Key implementation considerations:

One connection per interaction (service state reset)

Proper socket timeouts

Prompt synchronization

Avoiding false positives in output parsing

Lessons Learned

Enumeration always comes first.

Open port ≠ brute-force target.

Manual interaction reveals protocol behavior.

Automation should follow validation.

Understanding application-layer behavior is critical in both offensive and defensive security work.

Security Takeaway

This lab reinforced the importance of:

Carefully observing service behavior

Identifying unsafe input handling

Thinking beyond tool output

Analyzing how data flows through an application

The difference between "running a script" and understanding protocol-level behavior is significant.

Tools Used

Nmap

Netcat

Python (socket programming)

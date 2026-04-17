**What This Is**
A cross-platform keylogger that captures keyboard events, tracks active windows, and delivers logs remotely via webhooks. Built with Python using pynput for event capture, this demonstrates how malware monitors user activity and exfiltrates data without detection.

**Why This Matters**
Keyloggers are one of the oldest and most effective attack vectors. They've been used in major breaches including the 2013 Target breach where attackers used keylogging malware on point-of-sale systems to steal 40 million credit cards. Corporate espionage frequently relies on keyloggers to steal credentials, intellectual property, and sensitive communications.

**Real world scenarios where this applies:**

Credential theft: Attackers deploy keyloggers to capture login credentials for banking, email, and corporate systems. The 2017 Equifax breach started with stolen credentials that could have been captured this way.
Corporate espionage: Nation-state actors use keyloggers to monitor executives and steal trade secrets. The 2015 Anthem breach involved keylogging components that helped attackers pivot through the network.
Insider threats: Malicious insiders or investigators use keyloggers to monitor employees, sometimes legally for compliance, sometimes illegally for blackmail or competitive advantage.
What You'll Learn
This project teaches you how keyboard capture malware works under the hood. By building it yourself, you'll understand:

Security Concepts:

Keyboard event interception: How operating systems expose keyboard events to applications and why this creates a security boundary that's difficult to protect
Data exfiltration patterns: The techniques malware uses to send stolen data to command-and-control servers, including batching to avoid detection
Cross-platform malware development: Platform-specific APIs for Windows (win32gui), macOS (AppKit), and Linux (xdotool) that malware exploits
Technical Skills:

Event-driven programming with callbacks that process keyboard input in real time
Thread-safe logging using locks to prevent race conditions when multiple threads access shared resources
Platform detection and conditional imports to create malware that adapts to different operating systems
File rotation strategies to manage log sizes and avoid filling up disk space
Tools and Techniques:

pynput library for low-level keyboard and mouse event capture
Webhook delivery for remote data exfiltration over HTTPS
Process and window tracking to correlate keystrokes with the applications they were typed into
Prerequisites
Before starting, you should understand:

Required knowledge:

Python fundamentals including classes, dataclasses, enums, and type hints (the code uses modern Python 3.13 features)
Basic threading concepts like locks and events for synchronization
File I/O operations and path manipulation with pathlib
How HTTP POST requests work (for webhook delivery)
Tools you'll need:

Python 3.13 or later
uv package manager (install with curl -LsSf https://astral.sh/uv/install.sh | sh)
Terminal access with permissions to install packages
Helpful but not required:

Understanding of operating system process models and how applications interact with the OS
Familiarity with defensive security concepts like EDR (Endpoint Detection and Response)
Knowledge of JSON for understanding webhook payload structure

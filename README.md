# EWB Phishing URL Awareness Checker

[![Live Demo](https://img.shields.io/badge/Live%20Demo-GitHub%20Pages-brightgreen)](https://karthikk-2004.github.io/phishing-url-awareness/)
[![GitHub Repo](https://img.shields.io/badge/GitHub-Repository-blue)](https://github.com/KarthikK-2004/phishing-url-awareness)

## Overview
Phishing is a deceptive social engineering attack where malicious actors distribute fraudulent links designed to mimic trusted websites, aiming to steal confidential data such as passwords, banking information, or session tokens. The **EWB Phishing URL Awareness Checker** is a Python-based defensive educational tool that inspects visible URL strings for structural, syntactic, and lexical red flags. By operating strictly locally and analyzing text patterns without establishing network connections or visiting links, it helps learners identify suspicious indicators and build proactive cybersecurity awareness.

- **GitHub Repository**: [https://github.com/KarthikK-2004/phishing-url-awareness](https://github.com/KarthikK-2004/phishing-url-awareness)
- **Live Interactive Demo**: [https://karthikk-2004.github.io/phishing-url-awareness/](https://karthikk-2004.github.io/phishing-url-awareness/)

## Features
- Detects an `@` symbol in the URL (used to trick users regarding the actual destination)
- Flags unusually long URLs (over 100 characters often used to obscure malicious domains)
- Flags IP-address hostnames (avoiding registered domain registration oversight)
- Flags excessive hostname segments and subdomains (5 or more parts)
- Flags punycode-style hostnames (`xn--` markers often used in homograph attacks)
- Flags the unencrypted HTTP scheme
- **Interactive Multi-URL Session**: Check several URLs in sequence without restarting the script
- **Actionable Risk Summary**: Displays a clear "REVIEW CAREFULLY" warning count when risks are detected
- **Crash Prevention on Malformed Input**: Safely handles illegal URL characters and malformed hostnames

## Requirements
Python 3.10 or later. No extra packages needed (uses standard Python libraries: `re`, `urllib.parse`).

## How to Run
Windows:
```powershell
python phishing_checker.py
```
Mac / Linux:
```bash
python3 phishing_checker.py
```

## Example Output
```text
EWB Phishing URL Awareness Checker
Text checks only - this program does not open URLs.

Enter a sample URL (or type q to quit): http://192.0.2.10/account

Result:
- Uses an IP address instead of a familiar domain.
- Uses HTTP. HTTPS alone does not prove trust.

REVIEW CAREFULLY: 2 warning sign(s) found.
Never enter passwords from unexpected links.

Enter a sample URL (or type q to quit): q
Goodbye! Stay alert online.
```

## Testing
| # | Input | Expected | Actual | Result |
|---|-------|----------|--------|--------|
| 1 | `https://www.example.com/login` | No warning signs | No listed warning signs found. This does NOT prove safety. | PASS |
| 2 | `http://192.0.2.10/account` | IP address + HTTP | Uses an IP address instead of a familiar domain.<br>Uses HTTP. HTTPS alone does not prove trust. | PASS |
| 3 | `http://example.com@sample.invalid/login` | @ symbol + HTTP | Contains an @ symbol; inspect carefully.<br>Uses HTTP. HTTPS alone does not prove trust. | PASS |
| 4 | `https://a.b.c.d.e.example.invalid/path` | Many hostname segments | Contains many hostname segments/subdomains. | PASS |
| 5 | `https://xn--example.invalid/` | Punycode-style hostname | Contains a punycode-style hostname. | PASS |

*Full evidence and edge-case evaluations are documented in [`tests/test_samples.md`](tests/test_samples.md).*

## Screenshots
![Sample run](screenshots/sample_run.png)
![Test results](screenshots/test_results.png)

## Improvements
1. **Interactive Loop with Quit (Option A)** - Replaced single execution with a `while True` loop allowing multiple URL inspections in a single terminal session, terminating cleanly upon typing `q`, `quit`, or `exit`.
2. **Review Carefully Summary Counter (Option B)** - Added a `has_warnings()` helper that evaluates whether findings represent actual security anomalies or neutral status messages, outputting a high-visibility summary (`REVIEW CAREFULLY: X warning sign(s) found.`).
3. **Safe Parsing for Malformed Input (Option C)** - Wrapped the parsing routine in a `try...except ValueError` block, preventing program crashes when processing malformed inputs like `http://[bad` and returning a clear user message.

## Limitations
This is an educational tool. It only reads URL text. It does not visit
URLs, scan for malware or prove that a URL is safe. No warning signs
found does NOT mean a URL is safe.

## Ethical Use
Use sample strings only. Do not collect credentials. Do not use this
project to deceive anyone or to test websites without permission.

## Author
Karthik K, Cybersecurity Internship, EWB Courses

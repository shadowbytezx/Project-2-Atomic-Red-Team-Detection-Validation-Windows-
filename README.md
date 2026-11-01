# Project-2-Atomic-Red-Team-Detection-Validation-Windows-

Used Atomic Red Team to execute MITRE ATT&CK-mapped test T1059.001-17 ("PowerShell Command Execution") — a Base64-obfuscated PowerShell command that prints a benign message, simulating how attackers hide payloads from simple text-based detection.

**Test executed:**
```powershell
Invoke-AtomicTest T1059.001 -TestNumbers 17
```

**Result:** Wazuh's default ruleset correctly detected the technique without any custom rule required.

| Alert | Rule ID | Level | MITRE |
|---|---|---|---|
| Powershell.exe spawned a powershell process which executed a base64 encoded command | 92057 | 12 | T1059.001 |

**Observation:** this confirms Wazuh + Sysmon (SwiftOnSecurity config) has out-of-the-box coverage for encoded/obfuscated PowerShell execution, a common real-world evasion technique, validating that the detection pipeline built in Project 1 actually works against a known attacker TTP, not just custom-crafted test conditions.

**Troubleshooting note:** an earlier search showed no results for this event due to VM clock drift (~15 hours) between the Windows endpoint and the Wazuh server. Lesson: always verify time sync across lab VMs before concluding a detection gap exists — "no alert found" can mean "wrong search window," not "no detection."


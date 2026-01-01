
# Attack Scenario & Detection Workflow

## Objective

This project demonstrates the deployment of a Wazuh SIEM and its ability to detect, correlate, and contextualize a realistic Windows attack scenario, from initial access attempts to post-compromise activities.

---

## Attack Scenario Overview

The attack was simulated from a Kali Linux machine targeting a Windows host monitored by a Wazuh agent.

The following phases were observed and detected.

---

## 1. Initial Access – Authentication Failures

Multiple failed authentication attempts were generated against the Windows machine using invalid credentials.

**Detection:**
- Windows logon failures detected  
- Event ID: 4625  
- Wazuh rule: `60122`

This behavior is commonly associated with password guessing or brute force attempts.

---

## 2. Brute Force Detection (Correlation)

After multiple failed attempts within a short time window, Wazuh successfully triggered a brute force alert.

**Detection:**
- Five authentication failures in five minutes  
- Custom correlation rule: `110001`

This demonstrates the SIEM’s ability to correlate raw events into meaningful security alerts.


---

## 3. Successful RDP Connection (Lateral Movement)

A successful Remote Desktop (RDP) connection was established from the Kali machine using an administrator account.

**Indicators:**
- LogonType: `10` (Remote Interactive)  
- Authentication method: NTLM  
- Privileged account involved  

**Detection:**
- Wazuh rule detecting successful remote logon  
- Alert indicating possible lateral movement or pass-the-hash activity  

The use of NTLM for RDP access on an administrator account represents a high-risk behavior.

---

## 4. Post-Compromise Activity – Privilege Escalation

After gaining access, the attacker added a new user to the local Administrators group using native Windows tools.

**Command observed:**
```powershell
net localgroup Administrateurs test_soc /add
````

**Detection:**

* Windows group modification event
* Wazuh rule detecting administrator group changes

This step illustrates a persistence and privilege escalation technique.

![Privilege Escalation Detection](screen/Capture d'écran 2025-12-31 202531.png)

---

## 5. Suspicious Command Execution

Encoded PowerShell commands were executed on the compromised system.

**Indicators:**

* PowerShell launched with the `-enc` parameter
* Encoded payload detected in the command line

**Detection:**

* Custom rule matching encoded PowerShell execution
* Typical post-exploitation behavior

---

## Result and Analysis

The sequence of alerts forms a complete and coherent attack chain:

1. Authentication failures
2. Brute force detection
3. Remote access via RDP
4. Privilege escalation
5. Suspicious command execution

Despite minor timestamp inconsistencies caused by virtualization and log processing delays, the logical order of events remains consistent and exploitable for security analysis.

---

## Conclusion

This project validates the effectiveness of the Wazuh SIEM in detecting and correlating multiple stages of a realistic attack scenario. It highlights the importance of centralized logging, event correlation, and contextual analysis in a Security Operations Center (SOC) environment.

---

## Future Improvements

* Add time-based and source-based correlation rules
* Integrate cloud logs (AWS / Azure)
* Implement automated response actions
* Improve time synchronization (NTP)




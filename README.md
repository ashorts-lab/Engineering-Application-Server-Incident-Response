# Engineering-Application-Server-Incident-Response
Incident response investigation involving a suspected malware infection introduced via vendor email spoofing, resulting in unauthorized remote communication and resource exhaustion on a mission-critical engineering application server.
Project Overview
This project documents a simulated enterprise incident response engagement performed within a controlled virtual lab environment. The exercise involved detecting, investigating, and remediating a compromised engineering application server at a manufacturing company experiencing unexpected resource usage and operational outages. The scenario and response actions align with DoD/NIST-style incident handling procedures, emphasizing detection, containment, eradication, and recovery, while supporting follow-up recommendations for continuous improvement.
The project demonstrates my ability to:
Analyze and triage escalated helpdesk incidents linked to business disruption
Identify indicators of compromise through SIEM telemetry and host-level analysis
Contain malicious activity, restore system availability, and enforce firewall controls
Apply structured documentation aligned with incident-response reporting standards

Scenario Summary
Engineers reported severe latency and application timeouts while using the Pro-Engineer application responsible for model development. Initial troubleshooting and a server reboot failed to resolve the performance issues. Subsequent investigation revealed that the server had recently received an application update from a spoofed vendor email, which introduced malicious software.
Analysis using the SIEM platform exposed abnormally high CPU/GPU utilization and persistent outbound connections to an unknown external host over port 3333, indicating unauthorized remote communication. The compromised service was identified as a cryptomining process (XMRig) consuming system resources and impacting business operations.

Technical Environment
Component	Value
Hostname	WIN-6JNN6RLT6IL
IP Address	10.10.20.10
Operating System	Windows
Primary Function	Hosts Pro-Engineer application for model creation & editing
Malicious Port Activity	TCP/UDP 3333 used for remote cryptominer C2 traffic

Incident timeline
1.) Employees reported that the Pro-Engineer application was running slowly and timing out, resulting in work stoppages. 
2.) The operations team identified high resource utilization on the server that stores the engineering files and subsequently rebooted the server. 
3.) After the reboot, performance issues continued, and engineers who were still experiencing issues submitted help-desk tickets, which were escalated to the security team. 
4.) An investigation revealed that somebody recently installed updates for the Pro-Engineering application from an email that spoofed a trusted vendor. 
5.) SIEM analysis revealed unusually high GPU and CPU usage on the engineering application server during and after office hours, directed to an unknown external IP address over the non-standard port 3333. 
6.) The system was confirmed to be compromised, the malicious process was terminated, and a new firewall rule was added to block unauthorized traffic.

Indicators of Compromise
Category ->	IOC
Process	-> XMRig miner
Network Activity ->	Unauthorized outbound connections via port 3333
System Behavior -> Sustained abnormal CPU & GPU utilization
Delivery Method	-> Social engineering — spoofed vendor update email

Actions Taken
Detection: 
Identified affected host and port usage via SIEM telemetry (Wazuh → Agents → OpenSearch Dashboards → Discover → Malicious Traffic Search) 
Captured screenshots answering challenge questions validating proper workflow.

Investigation: 
Determined the malicious destination port (3333)
Confirmed unauthorized cryptomining process driving resource exhaustion

Remediation:
Terminated malware process and validated resource normalization
Restored Windows Defender Antivirus functionality and re-enabled threat scanning
Implemented outbound firewall block rule to restrict traffic over port 3333

Preventative Recommendations
Action ->	Negative Impact Addressed ->	Prevention Method
Vendor update verification -> Malware installation via spoofed emails	-> Enforce digital signature verification + vendor domain allowlisting
Restrict server internet access -> External command-and-control links -> Disable egress except for approved update repositories
Email filtering & impersonation controls -> Social engineering leading to compromise -> Implement DMARC/SPF/DKIM and alerting on spoofed domains
SIEM alerting for non-standard port usage -> Hidden remote channels & cryptomining -> Create port-usage detection rules with automated triage workflows

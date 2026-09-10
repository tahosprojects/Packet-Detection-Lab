# Packet-Detection-Lab
 
Hands-on lab getting practical reps with OPNsense, Wireshark, and Zeek. A segmented network (OPNsense as router/firewall between an attacker box and a victim/internal segment) where I capture and analyze real attack traffic, then build detections on top of it.
 
## What's In Here
 
- **OPNsense setup** — two-segment topology, routing, and firewall rules
- **Baseline traffic** — Wireshark + Zeek capturing normal traffic before any attacks run
- **Attack captures** — ARP spoofing, DNS tunneling, and C2 beaconing, each manually walked through in Wireshark at the byte level
- **Detection** — Suricata rules for each attack, running inline through OPNsense in IPS mode (actively blocking, not just alerting), with OPNsense ACLs enforcing segmentation
- **TLS interception demo** — MITM on my own traffic, decrypted, showing what becomes visible
- **Incident report** — final writeup mapping all three attacks to MITRE ATT&CK
## Stack
 
`Wireshark` `Zeek` `Suricata` `OPNsense` `Kali Linux`
 
No SIEM here, on purpose. The goal is raw traffic analysis and detection skill, independent of a platform doing the interpretation for me.
 
## Repo Structure
 
- `/topology` — network and firewall setup
- `/baseline` — normal traffic capture and analysis
- `/arp-spoofing`, `/dns-tunneling`, `/c2-beaconing` — capture, manual Wireshark walkthrough, and detection rule for each attack
- `/tls-interception` — before/after decryption comparison
- `/incident-report` — final report mapped to MITRE ATT&CK
 

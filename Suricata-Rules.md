# Suricata Rules

All custom detection rules used in this lab, with the reasoning behind each one.

**Engine:** Suricata 8.0.3, standalone (`apt`), running on the Ubuntu victim VM
**Mode:** IDS, alert-only — not inline/IPS

> The OPNsense Suricata plugin was tried first but had a persistent "no rules loaded" bug that was never resolved. Detection moved to a standalone install instead.

---

## DNS Tunneling

| | |
|---|---|
| **Attack** | Attack 2 |
| **SID / Rev** | `1000001` / `5` |
| **Logic** | Direct hex content match on the literal string `"tunnel"` |

```suricata
alert udp any any -> any any (msg:"Possible DNS Tunneling"; content:"|74 75 6e 6e 65 6c|"; sid:1000001; rev:5;)
```

The hex string decodes to the literal word **tunnel** — the fake domain used in this lab (`tunnel.local`) is known in advance, so this is a direct match, not a length- or entropy-based heuristic. A real adversary using a different tunnel domain would not trip this rule as written.

---

## C2 Beaconing

| | |
|---|---|
| **Attack** | Attack 3 |
| **SID / Rev** | `1000005` / `1` |
| **Logic** | 3+ connections to port 4444 from one source within 60s, threshold-suppressed |

```suricata
alert tcp any any -> any 4444 (msg:"Possible C2 Beaconing - Repeated Connections"; flow:to_server; threshold:type both, track by_src, count 3, seconds 60; sid:1000005; rev:1;)
```

Fires once after three connections land within sixty seconds, then suppresses repeats for the rest of that window rather than alerting on every attempt — a deliberate alert-fatigue reduction, not a limitation.

---

## ARP Spoofing — *not* detectable by Suricata

| | |
|---|---|
| **Attack** | Attack 1 |
| **SID / Rev** | n/a |
| **Logic** | n/a — ARP

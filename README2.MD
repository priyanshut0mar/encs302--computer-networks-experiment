# ENCS304 — Assignment 2: Packet Flow Visualization Using Simulation Mode
## 👨‍🎓 Student Details

| Field | Information |
|-------|-------------|
| Name | Priyanshu Tomar |
| Roll No. | 2301010162 |
| Program | B.Tech (CSE) |
| Semester | 6 |
| Course Code | ENCS304 |
| Course Name | Computer Networks Lab |
| School | School of Engineering and Technology |

---


---

## Experiment Overview

This assignment uses **Cisco Packet Tracer Simulation Mode** to visualize how packets
travel through a small LAN. We capture ARP and ICMP events, observe how a switch learns
MAC addresses, and compare the behaviour of a first ping versus a subsequent ping.

---

## Repository Structure

```
.
├── exp2_packetflow.pkt      # Packet Tracer simulation file
├── output_exp2.txt          # Event list observations (all 4 tasks)
├── report_exp2.pdf          # Full lab report with screenshots & analysis
└── README.md                # This file
```

---

## Network Topology

```
[PC1] 192.168.20.1/24 ──Fa0/1──┐
                                 │
[PC2] 192.168.20.2/24 ──Fa0/2──┤  [Switch0] Cisco 2960 (Layer 2)
                                 │
[PC3] 192.168.20.3/24 ──Fa0/3──┘
```

---

## Tasks Performed

| Task | Description |
|------|-------------|
| Task 1 | Built a simple LAN: 1 switch + 3 PCs with 192.168.20.x/24 addresses |
| Task 2 | Enabled Simulation Mode; performed first ping PC1→PC2; observed ARP + ICMP |
| Task 3 | Performed second ping PC1→PC2; confirmed ARP is skipped (cached) |
| Task 4 | Verified switch MAC address table via CLI (`show mac address-table`) |

---

## Key Concepts Demonstrated

### ARP (Address Resolution Protocol)
- Before ICMP can run, ARP resolves the destination IP → MAC address.
- First ping triggers an ARP **broadcast** (`FF:FF:FF:FF:FF:FF`).
- ARP reply is **unicast** back to the requester.

### ICMP Ping
- Only begins after ARP completes.
- Echo Request (Type 8) → Echo Reply (Type 0).

### Switch MAC Learning
- Switch learns source MACs from incoming frames.
- Unknown destination → frame is **flooded** to all ports.
- Known destination → frame is **unicast forwarded** to specific port.
- Entries are **DYNAMIC** with a 300-second aging timer (default).

---

## First Ping vs Second Ping

| Aspect | First Ping | Second Ping |
|--------|-----------|-------------|
| ARP Required | ✅ Yes — broadcast sent | ❌ No — ARP cache hit |
| Event Count | ~12–16 (ARP + ICMP) | ~8 (ICMP only) |
| Switch Behaviour | Floods then unicasts | Unicast only |
| Latency | Higher | Lower |

---

## How to Open the Simulation

1. Open **Cisco Packet Tracer** (v8.x or later recommended).
2. File → Open → select `exp2_packetflow.pkt`.
3. Click the **Simulation Mode** button (stopwatch icon, bottom-right).
4. Set Event List filters to **ARP** and **ICMP**.
5. Open PC1 → Desktop → Command Prompt → `ping 192.168.20.2`.
6. Click **Play** or **Capture/Forward** to step through events.

---

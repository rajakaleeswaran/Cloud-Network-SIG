# TASK 03 — Cisco Packet Tracer Ping Troubleshooting

## Objective
Diagnose failed ping tests by isolating the problem through OSI Layers 1, 2 and 3, then verify the fix in Cisco Packet Tracer.

## Topology
- PC0 → Switch0 → PC1
- No router is required for the same-subnet test.

## Initial fault: mismatched subnet
Configure:

| Device | IPv4 Address | Subnet Mask | Default Gateway |
|---|---|---|---|
| PC0 | 192.168.10.25 | 255.255.255.0 | — |
| PC1 | 192.168.20.26 | 255.255.255.0 | — |

Run from PC0:

```text
ping 192.168.20.26
```

Expected result: the ping fails because the two hosts are on different /24 networks and there is no router.

## Troubleshooting procedure

### 1. Layer 1 — Physical
- Confirm every link indicator is green.
- Check that the interfaces are connected and not administratively down.
- If a link is red, check the cable and interface state.
- If a switch port is amber, allow STP to converge or use Fast Forward Time.

### 2. Layer 2 — Data Link / ARP
From the PC command prompt:

```text
arp -a
```

Use Simulation Mode and enable ARP and ICMP filters. Use Capture/Forward to observe ARP resolution and the ICMP packet path.

### 3. Layer 3 — IP configuration
Run:

```text
ipconfig /all
```

Compare the IP address and subnet mask on both PCs. For a direct same-subnet connection, both addresses must belong to the same network.

Also test:

```text
ping 127.0.0.1
ping 192.168.10.25
```

The first checks the local TCP/IP stack. The second checks the PC's own interface configuration.

## Fix
Change PC1 to:

```text
IP Address: 192.168.10.26
Subnet Mask: 255.255.255.0
Default Gateway: —
```

Then test from PC0:

```text
ping 192.168.10.26
```

Expected result: successful replies.

## Additional diagnostic cases

| Fault | Diagnostic action |
|---|---|
| Wrong/missing default gateway | Check `ipconfig /all`; verify the gateway matches the router LAN interface. |
| Duplicate IP | Check Packet Tracer warnings and `arp -a` for changing MAC associations. |
| Amber switch link | Wait for STP convergence or use Fast Forward Time. |
| Wrong cable / down interface | Check link indicators and router `show ip interface brief`. |
| ARP failure | Use `arp -a` and Simulation Mode with ARP enabled. |

## Submission checklist
- [ ] Topology created in Cisco Packet Tracer
- [ ] Initial failed ping captured
- [ ] `ipconfig /all` checked on both PCs
- [ ] Simulation Mode used with ARP and ICMP filters
- [ ] Root cause identified
- [ ] Configuration corrected
- [ ] Final successful ping verified

> Note: The GitHub repository can store the Packet Tracer `.pkt` file, but a `.pkt` file must be created/saved by Cisco Packet Tracer itself. This README documents the exact lab configuration and verification procedure.

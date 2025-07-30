
# SSH Brute-Force Detection - SOC Home Lab Report

## Analyst: Kishor Pudasaini
**Date**: July 30, 2025  
**Environment**: Ubuntu 22.04 (Local VM)  
**Tools Used**:  
- `journalctl`  
- `sshd` logs  
- Wireshark  
- Hydra (for brute-force simulation)

---

## Objective

Simulate a brute-force SSH attack on localhost and analyze detection methods using logs and Wireshark traffic capture. This is part of a SOC Analyst home lab project to develop hands-on skills in incident detection and response.

---

## Attack Simulation

A brute-force attack was simulated using `hydra`:

```bash
hydra -l invaliduser -P /usr/share/wordlists/rockyou.txt ssh://127.0.0.1
```

The system was monitored for:
- Repeated failed SSH login attempts
- Authentication errors in logs
- TCP reset flags in packet captures

---

## Detection

### 1. SSH Logs via journalctl:

```bash
sudo journalctl -u ssh | grep "Failed password"
```

### 2. Count of Attempts:

```bash
sudo journalctl | grep "Failed password" | awk '{print $(NF)}' | sort | uniq -c
```

> Result: `2475` failed SSH attempts detected.

---

## Wireshark Analysis

- Filter used: `tcp.port == 22`
- Observed multiple TCP RST flags indicating rejected SSH sessions
- High number of short-lived SSH connections with very small packet size
- Loopback traffic (127.0.0.1) confirming internal simulation

---

## Screenshots

Included screenshots from:
- Terminal commands and log inspection
- Hydra brute-force session
- Wireshark TCP stream and reset packet analysis
- Log filtering using `awk`, `grep`, and `journalctl`

---

## Summary

This exercise demonstrated how to simulate and analyze a brute-force SSH attack using free open-source tools and Linux logs. Key skills applied:

- Log filtering using `journalctl`
- Packet analysis using Wireshark
- Recognizing brute-force attack patterns in SSH logs

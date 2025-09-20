# Routing Protocols: RIP and OSPF

This project compares **RIP (distance-vector)** and **OSPF (link-state)** routing protocols.  
It studies their route discovery, update propagation, failure recovery, and convergence behavior.

## Project Structure
- `RIP___OSPF.pdf` → Full detailed lab report
- `RIP___OSPF.md` → Markdown version of the report 

## Tools
- Netkit network emulator
- Wireshark packet captures

## Key Results
- RIP relies on periodic hop-count updates, converges slowly (minutes in case of failures).  
- OSPF propagates LSAs quickly, converges in ~10s, and scales better.  
- OSPF chooses efficient paths with cost-based metrics (Dijkstra).  
- RIP limited by count-to-infinity and hop limit (15).  

## Report Access
- [Full Report (PDF)](RIP___OSPF.pdf)
- [Full Report (Markdown)](RIP___OSPF.md)

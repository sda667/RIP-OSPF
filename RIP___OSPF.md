# Routing Protocols: RIP and OSPF Dynamic Routing  
**Author:** Ali Bahja  
**Date:** 6 December 2024  

---

## Objective
The purpose of this lab is to analyze and compare the behavior of two routing protocols:  
**Routing Information Protocol (RIP)**, a distance-vector protocol, and **Open Shortest Path First (OSPF)**, a link-state protocol.  

The experiments focused on:  
- Route discovery  
- Update propagation  
- Failure handling  
- Convergence times  

---

## Topology and Setup
- Experiments use a **Netkit** network with four routers (R1–R4).  
- Multiple links configured to test both RIP and OSPF.  
- **Wireshark** captures used to analyze routing updates and protocol behavior.  

---

## Procedure and Results

### Routing Information Protocol (RIP)
- Initially, R1’s forwarding table only contained directly connected destinations.  
- After launching RIP on R2, it advertised its destinations, distances (hop count), and next hops.  
- R1 updated its table accordingly. As R3 and R4 joined, routing info propagated until all nodes were reachable.  

**Captured RIP packets showed:**  
- Advertisements of destination networks, metrics, and next hops.  
- Metric value of 16 → represents infinity (route unreachable).  
- Mechanism called *poisoned reverse* prevents loops by advertising such routes back.  

**Stable topology path (R2 → R4):**  
$$
R2 → R3 → R4
$$

**Failure case (R3 eth2 shut down):**  
- Traceroute from R2 to R4 failed ~15s.  
- R2 initially tried old path via R3.  
- Updated route:  
$$
R2 → R1 → R4
$$
- Metric increased from 2 → 3.  

**Router failure (R3 removed):**  
- Convergence required ~1–2 minutes.  
- RIP’s periodic updates and timers explain the slow recovery.  

---

### Open Shortest Path First (OSPF)
**Setup:** backbone routers (bb0–bb4) with IPs:  
- bb0: 10.0.2.3  
- bb1: 10.0.3.1  
- bb2: 10.0.1.1  
- bb3: 10.0.2.2  
- bb4: 10.0.3.2  

**OSPF behavior:**  
- Routers exchanged *Link State Advertisements (LSAs)*.  
- LSAs contained destination, source, checksum, flags, Area ID, domains, and link costs.  
- Every router built a complete topology view.  
- Dijkstra’s algorithm computed shortest paths.  

**Link costs configured as:**  
- x0: 10  
- x1: 45  
- y0: 21  
- y1: 36  
- z0: 10  
- z1: 10  

**Optimal path from bb1 → 10.0.2.1:**  
$$
bb1 → bb2 → bb3 → 10.0.2.1
$$

- Path avoided costly links (x1, y1), preferring multiple low-cost links.  
- Total cost = 41 (minimum).  

**Failure handling:**  
- OSPF updates nearly instantaneous.  
- State changes multicast through domains, rapidly propagated.  
- Convergence ~10s, much faster than RIP.  

---

## Analysis
Key differences confirmed:  
- RIP: distance-vector, slow convergence, limited scalability.  
- RIP: poisoned reverse helps avoid loops, but count-to-infinity still possible.  
- OSPF: link-state, fast convergence, global topology knowledge.  
- OSPF: cost-based metrics give efficient paths.  

---

## Conclusion
This lab demonstrates fundamental differences between distance-vector and link-state routing protocols:  
- RIP shows limitations of hop-based metrics and periodic updates.  
- OSPF shows efficiency of global knowledge and Dijkstra-based computation.  

**Overall:** OSPF offers faster convergence, more accurate routing, and better scalability compared to RIP.  

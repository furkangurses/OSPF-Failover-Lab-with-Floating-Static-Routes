![Topology Diagram]()

# OSPF Failover Lab with Floating Static Routes

---

## 🎯 Lab Objective
Demonstrate the configuration and operation of floating static routes to provide failover in an OSPF-enabled network. Learn how administrative distance (AD) influences route selection and ensures backup paths are used when primary links fail.

---

## 🌐 Topology Overview
- Two LANs for Enterprise A:
  - R1 LAN: 10.0.1.0/24
  - R2 LAN: 10.0.2.0/24
- Direct fiber connection between R1 and R2.
- Both R1 and R2 have dual Internet connections via:
  - SPR1 & SPR2 (ISP A)
  - ISP B (optional)
- Goal: Ensure reachability between R1 and R2 even if the direct link fails, using floating static routes.

---

## ⚙️ Configuration Steps
1. Verify routing tables on R1 and R2:
   ```bash
   enable
   show ip route

    ```

## Identify OSPF-learned routes.
- Configure floating static route on R1 (backup via ISP A):
- configure terminal
- ip route 10.0.2.0 255.255.255.0 203.0.113.1 111

## Configure floating static route on R2 (backup via ISP A):
- configure terminal
- ip route 10.0.1.0 255.255.255.0 203.0.113.5 111

## Test failover by shutting down R2 G0/2/0 interface:
- interface g0/2/0
- shutdown
- show ip route

## Verify traffic reaches target networks via backup routes:
- ping 10.0.2.1
- traceroute 10.0.2.1

## 📝 Commands Used
- enable
- show ip route
- configure terminal
- ip route <destination> <mask> <next-hop> <AD>
- interface g0/2/0
- shutdown
- ping <target-IP>
- traceroute <target-IP>

---

## 🧠 Technical Explanation
- Floating static routes: Static routes configured with a higher AD than dynamic routes, acting as backup paths.
- Administrative Distance (AD): Determines route preference. Default static AD = 1, OSPF AD = 110.
- Failover Logic: If the primary OSPF route fails, the floating static route with AD > 110 becomes active.
- Loopback Interfaces: Simulate remote hosts for lab testing without physical devices.
- Traceroute vs Ping: Traceroute shows each hop along the path; ping only verifies reachability.

---

## 🌍 Real-World Use Case
- Enterprise networks with redundant WAN links.
- Backup routes maintain connectivity during primary link failures.
- Useful when OSPF is primary, but fallback routes are required for high availability.

---

## 🛠️ Skills Gained
- Understanding OSPF and AD influence on routing.
- Configuring floating static routes for failover.
- Routing table analysis and verification.
- Simulating remote networks using loopback interfaces.
- Troubleshooting primary and backup paths.

---

## ⚡ Possible Improvements
- Add multiple ISP backup links with varying AD for multi-level redundancy.
- Integrate with BGP for Internet-facing failover.
- Expand lab with multi-area OSPF design for advanced path selection.

---

## 🧩 Troubleshooting Notes
- Ensure AD of floating static routes is higher than dynamic route AD.
- Verify next-hop IPs are reachable before setting static routes.
- Use show ip route and traceroute to confirm failover behavior.
- Test failover by shutting down interfaces to validate backup path usage.

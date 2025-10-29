# minirack-homelab
A compact 10" rack designed for low-power, modular experimentation.
This setup hosts networking, routing, and containerized workloads across a mix of x86 edge nodes.

![20251029_114409](https://github.com/user-attachments/assets/91d8eda1-b740-4228-84e3-997dc46f7381)


## Overview

This rack is primarily for all the networking and lightweight hosting applications. Leaving any heavy applications (like locally hosted LLMs) to more powerful machines. It's goal is not only to be a learning experience but also hosting useful applications.

| Device | Role | Notes |
|:--|:--|:--|
| **ZimaBlade (8 GB)** | **Docker Swarm Manager** | Primary controller for low-power cluster, runs Docker Swarm and Portainer Agent. |
| **2 × ZimaBoard (8 GB)** | **Docker Swarm Workers** | Lightweight compute nodes for containerized services. |
| **Lenovo ThinkCentre** | **Ultimate Router (Proxmox)** | Runs **Proxmox VE** hosting an **OPNsense router/firewall VM** and a **Pi-hole VM** for DNS. [Used a AliExpress **M.2 → 2 × 2.5 GbE NIC** adapter for dual 2.5 gig LAN.](https://www.aliexpress.us/item/3256806381458551.html?spm=a2g0o.order_list.order_list_main.5.1dc11802P7EsVM&gatewayAdapt=glo2usa) |
| **Lenovo ThinkCentre** | **Test Environment** | Isolated sandbox for testing VMs, containers, or experimental configs before deployment. Plans to convert into a NAS|
| **MikroTik CRS310-8G+2S+IN** | **Core Switch** | 8 × 2.5 GbE + 2 × 10 Gb SFP+. Handles VLANs and backbone switching between servers. |
| **10″ Patch Panel** | **Cable Management** | Organizes Cat6a links for clean rack layout. |

---

## Networking

- **Hitron CODA-65 DOCSIS 3.1 Modem** - Connects to ISP
- **OPNsense VM** — WAN, DHCP, Firewall, and VLAN routing
- **TPLink AX1800** - WiFi AP 
- **Switch (CRS310)** — central L2 hub   
- **Pi-hole VM** — DNS caching and some ad blocking  

---

## Software Stack

| Layer | Software | Purpose |
|:--|:--|:--|
| Virtualization | **Proxmox** | Hosts OPNsense + Pi-hole VMs. |
| Firewall / Routing | **OPNsense** | Core firewall, NAT, and VLAN router. |
| DNS | **Pi-hole** | Network-wide DNS filter. |
| Container Orchestration | **Docker Swarm (on DietPi)** | Lightweight cluster across Zima nodes. |
| Management UI | **Portainer CE** | Central control plane for Docker and Swarm. |
| Monitoring | **Cockpit** | Web dashboards because I can't be bothered to SSH |

## Low-Power Docker Swarm

### Node Roles
| Node | Hostname | Role |
|:--|:--|:--|
| ZimaBlade 8 GB | `helios` | Swarm Manager |
| ZimaBoard 8 GB #1 | `EOS-01` | Swarm Worker |
| ZimaBoard 8 GB #2 | `SELENE-02` | Swarm Worker |

### Highlights
- Runs on **DietPi (Bookworm)** for minimal overhead  
- Docker Engine + Portainer Agent on each node  
- Swarm initialized on manager → auto service distribution  
- Managed through seperate Portainer instance  
- Cluster idle draw ≈ 25 – 30 W

---

### Future Plans
- Convert the test ThinCentre into a nas using a M.2 → SATA + 2.5gig USB dongle
- Look into beszel for monitoring
- Make a backup and log server
- Add RGB for additional performance
- Use a small power station as a UPS 

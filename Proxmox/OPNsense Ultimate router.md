# The "Ultimate Router"

Transforms an old Lenovo ThinkCentre into a cheap but powerful router.

Used software: **Proxmox**, **OPNsense**, and **Pi-hole**.
### Note: 
It needs to have 2 nics, this can easily be done using a PCIe network card

If it's a Mini PC with a m.2: [AliExpress **M.2 → 2 × 2.5 GbE NIC** adapter for dual 2.5 gig LAN.](https://www.aliexpress.us/item/3256806381458551.html?spm=a2g0o.order_list.order_list_main.5.1dc11802P7EsVM&gatewayAdapt=glo2usa)


## Hardware

- **Base system:** Repurposed Lenovo ThinkCentre (E-waste rescue) - can go for cheap on eBay
- **Purpose:** All-in-one router, firewall, DNS, and local service manager
- **Virtualization layer:** [Proxmox](https://www.proxmox.com/en/proxmox-ve)

---

## VM Layout

| VM | Role | Note |
|----|------|-------------|
| **OPNsense** | Firewall, Router, IDS/IPS, DHCP | The main firewall and routing system. Handles network segmentation, DHCP leases, and intrusion detection/prevention. |
| **Pi-hole** | DNS, Ad-blocker, | Provides DNS filtering with a ~500k domain blocklist, plus local DNS combined with NGINX for internal service access. |


---

## OPNsense Config

- **Primary Role:** Acts as both the **firewall** and **router**.
- **Services:**
  - DHCP Server
  - Intrusion Detection & Prevention
  - Firewall Rules for all subnets
  - NAT and VLAN configs
- **Gateway:** All outbound and inbound traffic routes through OPNsense.

---

## Pi-hole Config

- Deployed in its own VM, only needs 256mb of RAM
- **Blocklist:** ~500,000 domains (a few popular ads, trackers, and malicious site lists)
- **DNS:** 
  - Handles all DNS queries for the LAN
  - Provides **local DNS resolution** for internal services pointing at local NGINx server
- **NGINX Reverse Proxy:** 
  - Configured to map internal services to `.lan` hostnames
    ```
    https://192.168.0.69:9443 → http://portainer.lan
    ```


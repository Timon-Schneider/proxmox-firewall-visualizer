# Proxmox Firewall Visualizer

A single-file HTML tool that turns your Proxmox firewall configuration into an interactive network topology diagram.

## Features

- 🎨 **Visual Network Topology** - See your VMs, external IPs, and connections at a glance
- 🖱️ **Interactive Drag & Drop** - Rearrange nodes and connection labels freely
- ⚙️ **Customizable Layout** - Adjust circle radii, connection curves, and node sizes
- 💾 **Export to PNG** - Save your diagram as an image
- 🚀 **Zero Dependencies** - Just HTML, CSS, and vanilla JavaScript

## Usage

1. Download `index.html`
2. Open it in your browser
3. Paste your Proxmox firewall configuration (from `pve-firewall compile`)
4. Click "Generate Diagram"
5. Drag nodes and labels to customize the layout

## What It Shows

- Virtual machines and their IPs
- External IP addresses and connections
- Proxmox host
- Management network
- Port mappings and protocols
- Connection types (VM-to-VM, External-to-VM, External-to-Host)

## Example

Try pasting this sample configuration to see the visualizer in action:

```
[IPSET PVEFW-0-management-v4]
192.168.2.0/24

[RULES]

[group VM-SSH]
IN ACCEPT -p tcp -dport 22

[group VM-WEB]
IN ACCEPT -p tcp -dport 80
IN ACCEPT -p tcp -dport 443

# VM 100 - Web Server
add PVEFW-100-ipfilter-net0-v4 192.168.2.10
-A tap100i0-IN -s 0.0.0.0/0 -d 192.168.2.10 -p tcp --dport 80 -j ACCEPT
-A tap100i0-IN -s 0.0.0.0/0 -d 192.168.2.10 -p tcp --dport 443 -j ACCEPT
-A tap100i0-IN -s 192.168.2.20 -d 192.168.2.10 -p tcp --dport 3306 -j ACCEPT

# VM 101 - Database Server
add PVEFW-101-ipfilter-net0-v4 192.168.2.20
-A tap101i0-IN -s 192.168.2.10 -d 192.168.2.20 -p tcp --dport 3306 -j ACCEPT
-A tap101i0-IN -s 192.168.2.30 -d 192.168.2.20 -p tcp --dport 3306 -j ACCEPT

# VM 102 - Application Server
add PVEFW-102-ipfilter-net0-v4 192.168.2.30
-A tap102i0-IN -s 0.0.0.0/0 -d 192.168.2.30 -p tcp --dport 8080 -j ACCEPT
-A tap102i0-IN -s 192.168.2.20 -d 192.168.2.30 -p tcp --dport 5432 -j ACCEPT

# VM 103 - Monitoring Server
add PVEFW-103-ipfilter-net0-v4 192.168.2.40
-A tap103i0-IN -s 203.0.113.0/24 -d 192.168.2.40 -p tcp --dport 9090 -j ACCEPT
-A tap103i0-IN -s 192.168.2.10 -d 192.168.2.40 -p tcp --dport 9100 -j ACCEPT

# Management Network Access
add PVEFW-0-management-v4 192.168.2.0/24
-A PVEFW-HOST-IN -m set --match-set PVEFW-0-management-v4 src -p tcp --dport 8006 -j RETURN
-A PVEFW-HOST-IN -m set --match-set PVEFW-0-management-v4 src -p tcp --dport 22 -j RETURN

# External SSH Access to Host
-A PVEFW-HOST-IN -s 198.51.100.0/24 -d 192.168.2.1 -p tcp --dport 22 -j RETURN

# VPN Access
-A PVEFW-HOST-IN -s 203.0.113.50 -d 192.168.2.1 -p udp --dport 1194 -j RETURN
```

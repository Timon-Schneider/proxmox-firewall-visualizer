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

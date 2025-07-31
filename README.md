# Azure NSG Firewall Project (Security+ Lab)

This project demonstrates how to secure an Azure virtual network using **Network Security Groups (NSGs)** to restrict traffic between segmented subnets. It’s designed as a **Security+ portfolio project** and includes firewall setup, logging, and rule enforcement.


# Objectives

- Segment network into Web, App, and Admin subnets
- Apply least-privilege inbound NSG rules
- Enable NSG flow logs for traffic visibility
- Simulate and test real-world RDP and SSH scenarios
- Analyze logs and validate Deny/Allow logic


# Architecture

- **VNet Name**: `SecVNet`
- **Subnets**:
  - WebSubnet: `10.0.1.0/24`
  - AppSubnet: `10.0.2.0/24`
  - AdminSubnet: `10.0.3.0/24`
- **VMs**:
  - WebVM: Public HTTP access
  - AppVM: Internal only
  - AdminVM: RDP access only from allowed source

<Insert architecture/network-diagram.png here>


# NSG Rules Overview

| NSG Name  | Rule            | Port  | Priority | Action | Description              |
|-----------|------------------|--------|----------|--------|--------------------------|
| WebNSG    | Allow-HTTP       | 80     | 100      | Allow  | Allow HTTP from internet |
| AppNSG    | DenyAllInbound   | *      | 4096     | Deny   | Deny all inbound         |
| AdminNSG  | Allow-RDP        | 3389   | 100      | Allow  | RDP from My IP           |
| AdminNSG  | DenyAllInbound   | *      | 4096     | Deny   | Block all other traffic  |


# Testing Summary

- Verified RDP allowed to AdminVM from trusted IP
- Confirmed SSH blocked to AppVM
- Flow logs captured denied traffic from unauthorized source
- Used `Test-NetConnection` to simulate access


# Log Sample

```json
{
  "rule": "DenyAllInbound",
  "src": "10.0.1.4",
  "dst": "10.0.2.4",
  "port": "3389",
  "protocol": "T",
  "action": "D"
}

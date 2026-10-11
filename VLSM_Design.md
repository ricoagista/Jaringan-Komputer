# VLSM Addressing Scheme Design & Implementation

## Overview
Design and implement a Variable Length Subnet Mask (VLSM) addressing scheme for a campus network.

## Task Reference
**ID: 011** - VLSM Addressing Scheme Implementation

## Proposed VLSM Scheme

### Network Layout
```
Core Layer (R1-1, R1-2)
├── Core Router (R1-1)
│   ├── Edge Router (R1-2)
│   │   ├── PCs (Workstations)
│   │   ├── Printers
│   │   └── Wireless APs
└── Distribution Layer (R2-1, R2-2)
    ├── R2-1
    │   ├── Subnet A (192.168.10.0/24) - 30 hosts
    │   ├── Subnet B (192.168.11.0/26) - 6 hosts
    │   └── Subnet C (192.168.12.0/27) - 4 hosts
    └── R2-2
        ├── Subnet D (192.168.20.0/24) - 50 hosts
        └── Subnet E (192.168.21.0/26) - 12 hosts
```

### Detailed VLSM Breakdown

| Subnet | Network | CIDR | Hosts Supported | Purpose |
|--------|---------|------|------------------|----------|
| Subnet A | 192.168.10.0/24 | /24 | 254 | General office workstations (30 hosts) |
| Subnet B | 192.168.11.0/26 | /26 | 62 | Lab equipment / shared printers (6 hosts) |
| Subnet C | 192.168.12.0/27 | /27 | 14 | VoIP phones / VoIP gateway (4 hosts) |
| Subnet D | 192.168.20.0/24 | /24 | 254 | Main floor workstations (50 hosts) |
| Subnet E | 192.168.21.0/26 | /26 | 62 | Secondary floor workstations (12 hosts) |

### Why This VLSM?

1. **/24 (255.255.255.0)** - Most common for general office use (254 hosts)
   - Used for Subnets A and D
   - Sufficient for 30+ devices per area

2. **/26 (255.255.255.192)** - Medium density (62 hosts)
   - Used for Subnet B (VoIP/shared equipment)
   - Good balance between scalability and address efficiency

3. **/27 (255.255.255.224)** - Low density (14 hosts)
   - Used for Subnet C (VoIP phones)
   - Minimal waste, ideal for sparse device placement

4. **Address Planning**
   - Total usable addresses: 5 × 254 = 1270 (plus 5 network addresses + 5 broadcast = 1280 total)
   - Very efficient use of IP space
   - No overlap between subnets

## Implementation Steps in Cisco Packet Tracer

### Step 1: Create Core Devices
- **R1-1** (Core Router) - Configure as router with VLSM subnets
- **R1-2** (Edge Router) - Connect to distribution layer

### Step 2: Configure Core Router (R1-1)
```bash
! Enable VLSM subnets
conf t

! Subnet A - 192.168.10.0/24
interface GigabitEthernet0/0
 ip address 192.168.10.1 255.255.255.0
 no shutdown

! Subnet B - 192.168.11.0/26
interface GigabitEthernet0/1
 ip address 192.168.11.1 255.255.255.192
 no shutdown

! Subnet C - 192.168.12.0/27
interface GigabitEthernet0/2
 ip address 192.168.12.1 255.255.255.224
 no shutdown

! Subnet D - 192.168.20.0/24
interface GigabitEthernet0/3
 ip address 192.168.20.1 255.255.255.0
 no shutdown

! Subnet E - 192.168.21.0/26
interface GigabitEthernet0/4
 ip address 192.168.21.1 255.255.255.192
 no shutdown

end
wr fwdings
```

### Step 3: Create Distribution Devices (R2-1, R2-2)
- Configure as layer 3 switches or routers
- Connect to respective subnets
- Use trunk links between R1-1 and R2-x

### Step 4: Connect End Devices
- PCs connected to appropriate subnets
- Printers assigned to Subnet B/C
- Wireless APs connected to edge router

## Verification
- Use `show ip interface brief` to confirm IP assignments
- Verify no overlapping subnets
- Test connectivity between subnets via core router

## Notes
- **ID 011** corresponds to this VLSM implementation task
- All subnets are properly isolated (no overlap)
- VLSM ensures optimal IP utilization
- Consider future expansion: reserve /23 for future growth

## File Summary
- **VLSM_Design.md** - This design document
- **Packet Tracer project** - Will be built following the steps above
- **Subnet tables** - Referenced in the design

---
*Task ID: 011*
*Date: 2026-10-07*
*Platform: Cisco Packet Tracer*

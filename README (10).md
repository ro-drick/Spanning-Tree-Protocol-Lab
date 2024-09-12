# STP Configuration Lab - Spanning Tree Protocol

## Lab Overview
In this lab, we will explore the Spanning Tree Protocol (STP) configuration and optimization by modifying the root bridge selection, adjusting interface costs and port priorities, and enhancing network reliability using PortFast and BPDU Guard features. The topology consists of four switches connected in a loop with two VLANs, and the tasks involve manipulating the STP roles and paths.

## Tasks Overview

### Task 1: Check Current STP Topology

1. Use the CLI to check the current STP topology.
2. Identify the **current root bridge**.
3. Determine the **STP role/state** of each port on each switch.

#### Commands:
```bash
# On each switch
Switch# show spanning-tree vlan 1
Switch# show spanning-tree vlan 2
```
These commands will show:
- The current root bridge for each VLAN.
- The role and state (Root, Designated, Blocked, etc.) of each port.
### Task 2: Configure SW1 and SW2 as Root Bridges
1. Configure **SW1** as the **primary root** for **VLAN1** and the **secondary root** for **VLAN2**.
2. Configure **SW2** as the **primary root** for **VLAN2** and the **secondary root** for **VLAN1**.
3. Verify the new STP role/state of each port on each switch.
#### Commands:
```bash
# On SW1
SW1(config)# spanning-tree vlan 1 root primary
SW1(config)# spanning-tree vlan 2 root secondary

# On SW2
SW2(config)# spanning-tree vlan 2 root primary
SW2(config)# spanning-tree vlan 1 root secondary

# Verify on all switches
Switch# show spanning-tree vlan 1
Switch# show spanning-tree vlan 2
```
### Task 3: Increase Interface Cost on SW4
1. Increase the **VLAN1** cost of **SW4**'s **F0/2** interface to **100**.
2. Check if SW4 selects a different root port.
3. Explain why the selection did or did not change.
#### Commands:
```bash
# On SW4
SW4(config)# interface fastEthernet 0/2
SW4(config-if)# spanning-tree vlan 1 cost 100
SW4# show spanning-tree vlan 1  # Verify root port selection
```
### Task 4: Increase Port Priority on SW1
1. Increase the **VLAN1 port priority** of **SW1**'s **F0/1** to **240**.
2. Check if **SW3** selects a different root port.
3. Explain why the selection did or did not change.
### Commands:
```bash
# On SW1
SW1(config)# interface fastEthernet 0/1
SW1(config-if)# spanning-tree vlan 1 port-priority 240
SW1# show spanning-tree vlan 1  # Verify port priority change

# Verify on SW3
SW3# show spanning-tree vlan 1  # Check root port
```
### Task 5: Configure PortFast and BPDU Guard
1. Configure **PortFast** and **BPDU Guard** on the **F0/3** interfaces of **SW3** and **SW4**.
2. These settings will help avoid spanning-tree recalculations and protect against BPDU attacks on edge ports.
#### Commands:
```bash
# On SW3 and SW4
SW3(config)# interface fastEthernet 0/3
SW3(config-if)# spanning-tree portfast
SW3(config-if)# spanning-tree bpduguard enable

SW4(config)# interface fastEthernet 0/3
SW4(config-if)# spanning-tree portfast
SW4(config-if)# spanning-tree bpduguard enable

# Verify
SW3# show spanning-tree interface fastEthernet 0/3 detail
SW4# show spanning-tree interface fastEthernet 0/3 detail
```
### Summary of Configuration
- **Root Bridge**: SW1 for VLAN1, SW2 for VLAN2.
- **Port Costs and Priorities**: Adjusted to influence root port selection.
- **PortFast and BPDU Guard**: Enabled on access ports (F0/3) to improve network stability and prevent topology changes due to BPDU reception on edge ports.
### Verification and Testing
- Use `show spanning-tree` commands to verify changes in the root bridge, root ports, and designated ports.
- Test BPDU Guard by sending a BPDU to an edge port and confirming it shuts down the port.
## Acknowledgements


Special thanks to **Jeremy's IT Lab** for providing valuable resources and tutorials that greatly contributed to the completion of this exercise. His in-depth explanations and practical demonstrations have been instrumental in enhancing my understanding of Cisco networking concepts and the effective use of Packet Tracer.

For more information and additional resources, visit [Jeremy's IT Lab](https://jeremysitlab.com/) and check out his YouTube for the full course, [Jeremy's IT Lab Free CCNA 200-301 | Complete Course](https://www.youtube.com/playlist?list=PLxbwE86jKRgMpuZuLBivzlM8s2Dk5lXBQ)

# NAT and Port Forwarding using Linux Network Namespaces

## Overview

This project demonstrates how to set up Network Address Translation (NAT) and port forwarding using Linux network namespaces, `iproute2`, and `iptables`. It creates a simulated network environment with the following components:
- A private LAN (192.168.10.0/24) with a client node.
- A router acting as a NAT gateway.
- A simulated public internet.

The topology is as follows:

|Client (192.168.10.2)| <--------> |Router (NAT) (192.168.10.1/24, 10.0.0.1/24)| <--------> |Internet (10.0.0.2)|

The commands used and the results can be checked in the "Nat using netns.docx" file in this repository.
Bonus task regarding configuration of port forwarding has been done

### Challenges faced
1. Legacy Version Issues with iptables:
During the setup, masquerading was not possible due to compatibility issues with the version of iptables installed. This required switching to legacy mode to enable the NAT functionality correctly. It involved using legacy iptables commands to ensure proper NAT and port forwarding operations.

2. Network Namespace Management:
Managing network namespaces and veth pairs required careful configuration to ensure proper communication between the private network and the public network.

3. Name Length Restrictions:
Encountered issues with the naming conventions of veth interfaces due to Linux restrictions on interface name lengths. Adjusted the interface names to be shorter to avoid validation errors.

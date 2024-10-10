# NAT and Port Forwarding using Linux Network Namespaces

## Overview

This project demonstrates how to set up Network Address Translation (NAT) and port forwarding using Linux network namespaces, `iproute2`, and `iptables`. It creates a simulated network environment to emulate real-world networking scenarios, enabling devices within a private LAN to access external networks through a router acting as a NAT gateway.

### Components of the Setup:
- **Private LAN** (`192.168.10.0/24`): Contains a client node that will access the internet.
- **Router (NAT Gateway)**: Acts as a bridge between the private network and the simulated internet, handling NAT and port forwarding.
- **Simulated Public Internet**: Represents the external network that the private LAN connects to.

### Network Topology:

|Client (192.168.10.2)| <--------> |Router (NAT) (192.168.10.1/24, 10.0.0.1/24)| <--------> |Internet (10.0.0.2)|


This setup allows the client in the private LAN to reach the simulated internet through the router’s NAT functionality, while also enabling port forwarding to expose a service running on the client to the public network.

### Documentation and Results

All commands executed during this setup, along with their results, are documented in the [NAT using netns.docx](NAT%20using%20netns.docx) file included in this repository. Additionally, the bonus task of configuring port forwarding has been successfully implemented and is detailed in the document.

---

## Prerequisites

- A Linux system with `iproute2` and `iptables` installed.
- Root or `sudo` access to run commands.
- Basic understanding of networking concepts, such as NAT, IP masquerading, and port forwarding.

---

## Key Concepts

1. **Network Namespaces**: 
   - Provides isolated network environments within a single Linux system. Each namespace can have its own network interfaces, routing tables, and firewall rules, making it ideal for simulating complex network setups.

2. **NAT (Network Address Translation)**: 
   - Allows devices in the private network to communicate with external networks using a single public IP address. This is crucial for conserving IP addresses and providing a layer of security by hiding internal IP addresses from the outside world.

3. **IP Masquerading**: 
   - A specific form of NAT used in this setup to enable the private client to access the simulated public network. It modifies the source IP of outgoing packets to match the public IP of the router.

4. **Port Forwarding**: 
   - Enables access to specific services running in the private LAN from the external network. For instance, exposing a web server running on the client to external users through the router's public IP and a specified port.

---

## Challenges Faced

### 1. **Legacy Version Issues with `iptables`**:
   - During the initial setup, it became evident that masquerading was not functioning due to compatibility issues with the version of `iptables` installed on the system. Modern versions use `nftables` by default, which posed challenges with legacy scripts.
   - **Solution**: Switched to `iptables` legacy mode to ensure the NAT functionality could be configured correctly. This involved adapting commands to be compatible with the legacy version, ensuring the smooth operation of NAT and port forwarding rules.

### 2. **Network Namespace Management**:
   - Managing multiple namespaces and virtual Ethernet (veth) pairs required careful attention to detail. Each namespace acts like a separate network stack, making it necessary to ensure proper interface assignments and IP configuration for communication between namespaces.
   - **Solution**: Thoroughly planned the topology and double-checked configurations to ensure that interfaces were assigned correctly and IP addresses were configured to enable communication across the namespaces.

### 3. **Name Length Restrictions**:
   - Encountered errors related to the naming conventions of veth interfaces due to Linux limitations on interface name lengths. Linux restricts interface names to 15 characters, leading to validation errors during the setup.
   - **Solution**: Adjusted the interface names to shorter versions to remain within the length limit. This allowed the virtual Ethernet pairs to be created without validation issues, enabling seamless communication between namespaces.

---

## How It Works

- **NAT Configuration**: 
   - The router namespace is configured to perform IP masquerading, allowing the client in the `192.168.10.0/24` subnet to access the external network (`10.0.0.0/24`). The router translates the client’s private IP into its own public IP for outgoing packets and manages the reverse for incoming responses.

- **Port Forwarding**: 
   - A simple HTTP server is set up on the client node using Python. Port forwarding is configured on the router to allow access to this server from the public network. External requests to the router's public IP on port `8080` are forwarded to the client’s IP in the private LAN.

- **Testing**:
   - Connectivity between namespaces is verified using `ping` and `curl` commands to ensure that the client can reach the simulated internet and that port forwarding is functioning correctly.

---

## Usage

To replicate this setup on your own system, follow the step-by-step instructions provided in the [Nat_using_netns.docx](Nat_using_netns.docx) file. Each step is documented with the exact commands to execute and expected outputs, making it easy to understand and troubleshoot.

---

## References

For further reading and a deeper understanding of the concepts used in this project, refer to the following resources:
- [Linux Network Namespaces](https://blogs.igalia.com/dpino/2016/04/10/network_namespaces/)
- [Understanding NAT and IP Masquerading](https://radagast.ca/linux/nat_and_ip_masquerade.pdf)
- [ip-netns Manual](https://man7.org/linux/man-pages/man8/ip-netns.8.html)

---

## Conclusion

This project offers a practical demonstration of NAT, IP masquerading, and port forwarding using Linux network namespaces. It is a powerful learning tool for understanding how private networks interact with external networks and how specific services within a private network can be made accessible to external clients. With this setup, you can simulate various real-world networking scenarios and gain hands-on experience with key networking concepts.

---


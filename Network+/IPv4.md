[[Introduction to IP]] #IP
Operates at OSI Layer 3; Network Layer
- **The subnet mask**: (e.g., 255.255.255.0) helps the local device to determine which subnet it's on.
- **default gateway**: (e.g., 192.168.1.1) allows communication outside the local subnet and must an IP in UR local subnet
- **Loopback Address**: a self-reference address to your local device, use ping (127.0.01)
  it's between 127.0.01 and 127.255.255.254
- **Reserved address** : range (240.0.0.1 - 254.255.255.254) for future use and testing, "Class E Addresses".
- **DHCP**: Automates IP configuration, making network setup easier
- **APIPA**: Assigns link-local addresses when DHCP isn’t available, can only communicate in your local subnet, no forwarding by routers, e.g., 169.254.0.1 - 169.254.255.254.
  it uses ARP to make sure that no one in the network has the same IP.
-  **Virtual IP Address (VIP)**: not associated with with a physical network adapter but used in virtual machines  and internal router address.
due to the limitation of IPv4 public addresses there is a way to use more IP ranges and it's to use private IP addresses, but the problem is those IPs can't communicate to the any other external network:
- **Network Address Translation (NAT)**: NAT facilitates communication between private networks and the internet by translating private IP addresses into public ones. This feature is crucial for enabling secure and efficient internet access while conserving public IP address space.
![[Pasted image 20241204135209.png]]

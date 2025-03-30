# Network Function Virtualization
	- Replace physical network devices with virtual versions and manga from hybervisor

# Virtual Private Cloud (VPC)
- a pool of resources inside a public cloud
- communicate between VPCs with a transit gateway (Like a Cloud Router)
- Use a VPN Connection if you want to connect from outside the cloud
- VPC gateway : Connects users from the internet
- VPC NAT gateway : (NAT > Network Address Translation) 
   - private cloud connects to external resources
   - doesn't allow external users to connect inbound
- VPC Endpoint: Direct Connection Between Cloud Providers
# Security
In the virtual firewall settings we can set communications settings:
- inbound or outbound
- specify a Port Number to communicate (TCP or UDP)
- or specify an IP address or range of IPv4 or IPv6 addresses
You can make Network Security Groups because when you edit the settings it will be applied to all of your VPCs and then you can't edit a specific VPC 
# Types
- Iaas : Infrastructure as a service
- Paas : Platform as a service
- Saas : Software as a service
**Network security** is the deployment and monitoring of cybersecurity solutions to protect an organization’s IT systems from attacks and breaches. It also covers policies surrounding the handling of sensitive information.
**application attack** exploits a vulnerability in an application or other software on a computer.
**service attack** works to shut down a computer or network, making it unavailable to their regular users.

----

**Evil twin** attacks are a type of man-in-the-middle attack in which malicious actors set up a fake wifi source to steal information or further infiltrate a connecting device. The fake provider often appears similar to a known or legitimate wifi spot in users’ devices.
**rogue access point** is any wireless access point that does not belong to the network. It might not be a security risk; some are installed without malicious intent or by IT personnel for testing or another legitimate purpose. Unauthorized wireless access points, however, can be used to leak sensitive information, including passwords and cardholder data, outside of the business. A rogue access point installed by an attacker can bypass network firewalls and other security devices and open a network to attacks. It can also enable anyone accessing it to monitor what websites users access or what they download. It can even redirect the user to a false website that the hacker has created to get vulnerable information or cause a download of malicious software.
**jamming** attack is a type of denial-of-service attack in which malicious nodes intentionally interfere with wireless networks to prevent legitimate communication. A common method of jamming uses a device that emits electromagnetic energy that makes the network unusable by sending out signals and increasing noise. Other methods interfere with the function of communication software, such as causing a malfunction in the “handshake” protocol so that devices do not connect.
**bluesnarfing**, attackers exploit Bluetooth vulnerabilities to steal information or use the device. By sneaking into mobile devices with connections which have been left open, cybercriminals can perform a bluesnarfing attack on a device from a distance. Attackers can access and copy the contents of your device, including your email, contact list, phone number, passwords, and pictures. Lately, smart speakers have become another vulnerable device. Some bluesnarfing attackers use the victim’s phone to make expensive or even illegal calls.

---

**firewall** acts as a network’s gatekeeper. It filters traffic and blocks outsiders from gaining unauthorized access to confidential data on network computers. It can also help block malicious software from infecting network devices.
- **Stateful inspection filtering** analyzes traffic to discover whether it’s part of a previously established connection.
- **Application-level filtering** blocks traffic from applications that are not relevant to work.
- **Source-based and destination-based filtering** allows or blocks traffic based on where the packet came from or where it is going.
- Simple **allow or block filtering** is the most basic filtering type. It allows or blocks all traffic that meets the criteria that you set.

**Routers** are a network’s hardware connection to outside data, usually from the internet.
**Router encryption** :
- **Wired Equivalent Privacy (WEP)** is the oldest and most popular form of router encryption available but also the least secure. It uses radio waves that are easy to crack and the same encryption key for every data packet. With automated software, attackers can easily analyze this information.
- The Wi-Fi Alliance developed **Wi-Fi Protected Access (WPA)** to offer an encryption protocol without the shortcomings of WEP. It solves the vulnerability of radio transmission by scrambling the encryption key. Like WEP, WPA is less secure than some other types of router encryption, but it’s still much more secure than WEP.
- **Wi-Fi Protected Access 2 (WPA2)** is currently the most secure form of router encryption available. Like WPA, WPA2 scrambles the encryption key. It also does not allow the use of a less secure protocol, Temporal Key Integrity Protocol (TKIP).
- When using WPA2 or WPA, you should also use **Advanced Encryption Standard (AES)** if possible. This extremely secure encryption is the same type used by the United States government to secure classified information. Routers made after 2006 should have the option to enable AES along with WPA2.

**network switch** integrates all the devices on a network, allowing for seamless sharing and data transfer among them.
**proxy server** or **proxy** is a system or router that provides a gateway between users and the internet.
**load balancer** is a dedicated hardware device or an internet-facing server running a load balance service. Load balancers distribute traffic among multiple servers and decrypt website traffic that uses SSL.
**hardware security module (HSM)** is a dedicated cryptographic processor that is specifically designed to protect the cryptographic key lifecycle.

---
**Network architecture** refers to a network’s structural and logical layout. It describes which network devices are used, how the network devices are connected, and the rules that govern data transfer between them.
**Network design** is the process of creating network architecture for a specific organization and situation. It includes network analysis, hardware selection, and implementation planning, among other planning processes.
- **Business requirements** help define how the network will help the organization reach its goals.
- **Technical or functional requirements** describe performance goals that the network must meet.
**Network infrastructure devices** are the components of a network that control communications needed for data, applications, services, and multimedia. These devices include routers, firewalls, switches, servers, load balancers, intrusion detection systems, domain name systems, and storage area networks.

### **Designing safe networks**
- Disable unencrypted remote administrator protocols used to manage network infrastructure.
- Disable unnecessary services such as discovery protocols, source routing, HTTP, and simple network management protocol (SNMP).
- Implement robust password policies and use the strongest password encryption available.
- Restrict physical access to routers and switches, and secure access to the console, auxiliary, and virtual terminal lines.
- Back up configurations and store them offline.
- Use the latest version of the network device operating system and keep it updated with all patches.

---

**DMZ** is a separate network that protects and adds an extra layer of security to an organization’s internal local area network (LAN) from untrusted traffic. The DMZ connects to the internet to provide access for some resources, but has only a limited, secure connection to the internal network.
**Network address translation (NAT)** is a process by which one unique IP address can represent multiple computers. A network device, often a router or NAT firewall, assigns this single public IP address to a computer or group of computers inside a private network.
> With a NAT configuration, a request arrives at the public IP address and port, and the NAT instructions send it where it should go without revealing the private IP addresses of the destinations.

**honeypot** system is designed to attract attackers. It catches attention by looking, feeling, and acting like a network full of valuable resources, but it also contains tools for monitoring and other security functions.
> A honeypot, or a set of them configured as a **honeynet**, contains tools to track and study how a malicious person moves through a system without actually endangering the network. Honeypots can also distract attackers from an organization’s real resources and assets.

**Network segmentation** splits a larger network into smaller segments, usually through switches and routers.
**extranet** is a private network open to external users such as business partners, suppliers, and key customers.
**intranet** is a private network for distributing communications exclusively to their internal users.
**air gap** provides extremely strict security by completely isolating a digital device component or private network from other devices and networks, including the public internet.
>The strictest form of network division creates a wholly self-contained network with no traffic in or out

**Network access control (NAC)** is a process for controlling and managing access to a network by authenticating users and devices before allowing them to connect.
- In **identification**, the system tries to recognize a user or a device that attempts to access a system or resource. It might do this with usernames, employee IDs, or IP addresses.
- In **authentication**, the system tries to verify the identity of a user or device to make sure they really are who they claim to be. It might do so by asking for passwords or biometric data.
- In **authorization**, the system checks the user’s privileges and grants or denies access to resources based on the authenticated identity of the user or device. When a user is authenticated, they will receive access only to the resources that they are authorized to access.
- The **accounting** process tracks and records user activity and resource usage. This process supplies a means to track and trace access and usage for specific users and devices, which might help detect security breaches or company policy violations. Some examples include logging user sessions, logging access attempts, and logging resource usage.
**Knowledge-based authentication (KBA)** verifies the identity of a user by asking questions based on information that only the user knows
**Single sign-on (SSO)** enables users to access multiple resources with one set of login credentials. The SSO portal fully authenticates the user and creates a certificate or token that acts as a security key for other resources.
**Multifactor authentication (MFA)** requires users to provide two or more authentication factors to prove their identities
**Adaptive authentication**, also known as **risk-based authentication**, changes authentication requirements in real time when needed. A user logging in from a usual device might need only to enter a username and password. If the same user connects from an untrusted device or tries to view sensitive information, the system might require additional authentication factors.
**Access control schemes** help provide consistency in access control to network resources, ensuring that only authorized users can access the resources they need. They also prevent unauthorized access, theft, and damage.
- **Attribute-based access control (ABAC)** schemes base access control decisions on attributes that define the user, the resource, and the environment where users are requesting access. These attributes can include factors such as the user’s job title or location or the time of day. The system grants or denies access based on how these attributes match predefined policies.
- **Role-based access control (RBAC)** schemes determine access control decisions based on the roles assigned to users or groups.
- **discretionary access control (DAC)** schemes, every object or resource in the system has an owner who determines which users can access it. The owner can grant or deny access to other users based on their discretion.
- **mandatory access control (MAC)** schemes, users do not have control over their own access rights. Instead, a central authority such as a security administrator regulates what access rights each user has based on predetermined rules and policies. These usually create a tiered, hierarchical access structure.
**Filesystem controls** determine which accounts, users, groups, or services can perform actions such as reading, writing, and running files.

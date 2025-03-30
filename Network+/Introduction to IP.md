#IP 
![[Pasted image 20241006052658.png]]
 **TCP vs. UDP**: TCP ensures reliable delivery with acknowledgments and control over flow control, while UDP is connectionless, Doesn't have flow control and faster.
 - Connection-oriented VS Connectionless.

**IPv4 Socket** : IP address , Protocol , Port Number
- Client and server
 **Port Numbers** : 
 - Well-Known Port Numbers are from 0 to 1023  (Non-Temporary or Non-ephemeral)
 - TCP and UDP Port Numbers can be any number from 0 to 65,535
 - TCP 80 is different from UDP 80
# Ports
- 📁 **FTP**: uses TCP port 20 (data) and TCP port 21 (control) (File Transfer Protocol)
- 📁 **SFTP**: uses TCP port 22 for secure file transfers
- 🔒 **SSH**: Uses TCP port 22 for secure remote communication.
-  ✉️ **SMTP**: Operates on TCP port 25 for email transfer; port 587 for encrypted SMTP (Using TLS encryption).
    - It's a server to server protocol and may used  to send from a device to server
    - to receive as a device use IMAP or POP3
- 🌐 **DNS**: Uses UDP port 53 for domain name resolution (IP to name).
- 📅 **NTP**: Synchronizes time across devices using UDP port 123.
- 📊 **SNMP**: Manages network devices on UDP port 161, with traps on port 162.
- 📡 **Telnet**: uses TCP port 23 for non-secure remote access (Unlike SSH).
- 📡 **DHCP**: uses UDP port 67 (server) and UDP port 68 (client) for IP address assignment and auto configurations.
     - Requires a DHCP Server (Exists in your router a server is required for enterprises)
     - renew the IPs every beginning of a new interval
     - assigned by MAC address
- ⚡ **TFTP**: uses UDP port 69 for quick, unsecured file transfers
- 🌐 **HTTP**: uses TCP port 80 for non-secure web traffic
- 🔒 **HTTPS**: uses TCP port 443 for secure web traffic
- 📂 **LDAP**: uses TCP port 389 for directory access
- 🔒 **LDAPS**: uses TCP port 636 for secure directory access
- 🖥️ **SMB**: uses TCP port 445 for file and printer sharing in Windows
- 📜 **Syslog**: uses UDP port 514 for transferring log data
- 📅 **SQL Server**: uses TCP port 1433 for database queries
- 🖥️ **RDP**: uses TCP port 3389 for remote desktop access
- 📞 **SIP**: uses TCP port 5060 and TCP port 5061 for call initiation and management

# Other Common Ports
📡 **ICMP**: 
   - text messaging for network devices whether it's a respond to a hello are you there message or a TTL message when things don't go well
   - no data transfer so it's not based on UDP or TCP
- 🕵️‍♂️ **Ping Command**: Uses ICMP to determine device responsiveness by sending echo requests.
- 🔒 **GRE Tunnels**: Generic Routing Encapsulation, It's "The Tunnel" between two end points that the VPN uses, so it lacks encryption
- 🔐 **IPSec**: encrypts data for confidentiality and provides integrity and anti-replay functionality.
   - Commonly used with different manufacturers’ devices for it's standardized
  - Two cores IPsec Protocols: (AH) or authentication Header and Encapsulation Security Payload or (ESP)
- 🔄 **Internet Key Exchange (IKE)**: establishes shared encryption keys for the VPN tunnel without sending the key across the network and build a security association (SA)
  - *Phase one*: use Deffie-hellman to create shared secret key using UDP 500 (ISAKMP)
  - *Phase Two*: Coordinate ciphers and key sizes and negotiate an inbound and outbound  SA for IPSec
![[Pasted image 20241011182128.png]]
- **Transport mode and Tunnel mode** :
>> The Dark Green is encrypted.
 ![[Pasted image 20241011182526.png]]
 
 - 🔗 **AH vs. ESP**: While AH provides integrity, ESP combines encryption with authentication, making it the preferred choice for most implementations, emphasizing the need for both security and data protection in VPNs.
 ![[Pasted image 20241011183243.png]]
 ![[Pasted image 20241011183258.png]]
 
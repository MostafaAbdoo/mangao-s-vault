**ifconfig** > show the configuration assigned to network interface
**arp** > display the local machine's ARP cache
**ping** > test connectivity
**route** > view and configure the host's local routing table
**traceroute google.com(for example)** > trace a route for a specific web site
**netstat** > show the state of TCP/UDP ports on the local machine
**ip neighbor** > ?IPs in the same network
**dig** > DNS query
# NMAP
*there's pdf file named nmap cheat sheet in sec+ folder in learning*
When used without switches like this, the default behavior of Nmap is to ping and send a TCP ACK packet to ports 80 and 443 to determine whether a host is present. 
• On a local network segment, Nmap will also perform ARP and ND (Neighbor Discovery) sweeps. • If a host is detected, Nmap performs a port scan against that host to determine which services it is running.
Having identified active IP hosts on the network and gained an idea of the network topology, the next step in network reconnaissance is to work out which operating systems are in use, which network services each host is running, and, if possible, which application software is underpinning those services. This process is described as service discovery. Service discovery can also be used defensively, to probe potential rogue systems and identify the presence of unauthorized network service ports.
✓TCP SYN (-sS)—this is a fast technique also referred to as half-open scanning, as the scanning host requests a connection without acknowledging it, The target's response to the scan's SYN packet identifies the port state.
✓UDP scans (-sU)—scan UDP ports, As these do not use ACKs, Nmap needs to wait for a response or timeout to determine the port state, so UDP scanning can take a long time, A UDP scan can be combined with a TCP scan.
✓Port range (-p)—by default, Nmap scans 1000 commonly used ports, as listed in its configuration file, Use the -p argument to specify a port range
![[Pasted image 20250111021052.png]]

# Banner grabbing
Banner grabbing is a technique used to gather information about a service or application running on a specific port of a target machine. This can be done using tools like `curl`
### **Using `curl` for HTTP/HTTPS Banner Grabbing**

1. **Basic HTTP Request**
    `curl -I http://example.com`
    - The `-I` flag fetches the HTTP headers (e.g., `Server`, `Content-Type`).
    Example Output:
    `HTTP/1.1 200 OK
    `Date: Sat, 11 Jan 2025 12:34:56 GMT
    `Server: Apache/2.4.41 (Ubuntu) 
    `Content-Type: text/html; charset=UTF-8`

`curl -v http://example.com`
- The `-v` flag provides detailed information about the request and response.
    Example Output:
    `* Rebuilt URL to: http://example.com/ * Connected to example.com (93.184.216.34) port 80 (#0) > GET / HTTP/1.1 > Host: example.com > User-Agent: curl/7.68.0 > Accept: */* < HTTP/1.1 200 OK < Server: Apache/2.4.41 (Ubuntu) < Content-Type: text/html; charset=UTF-8`
### **Using `curl` for FTP Banner Grabbing**
`curl ftp://example.com`

Example Output:
`220 Welcome to the Example FTP server.` 
 > u can use it with more servers like smtp,pop3 and more

# PACKET CAPTURE AND TCPDUMP
• Packet and protocol analysis depends on a sniffer tool to capture and decode the frames of data. • Network traffic can be captured from a host or from a network segment. 
• Using a host means that only traffic directed at that host is captured. 
• Capturing from a network segment can be performed by a switched port analyzer (SPAN) port (or mirror port).
• This means that a network switch is configured to copy frames passing over designated source ports to a destination port, which the packet sniffer is connected to.
## tcpdump
is a command line packet capture utility for Linux (linux.die.net/man/8/tcpdump ). 
- The basic syntax of the command is `tcpdump -i eth0`, where eth0 is the interface to listen on. The utility will then display captured packets until halted manually (Ctrl+C). 
- Frames can be saved to a .pcap file using the -w option. 
- Alternatively, you can open a pcap file using the -r option.
tcpdump is often used with some sort of filter expression to reduce the number of frames that are captured: 
- Type—filter by host, net, port, or port range.
- Direction—filter by source (src) or destination (dst) parameters (host, network, or port). Protocol—filter by a named protocol rather than port number (for example, arp, icmp, ip, ip6, tcp, udp, and so on).
- and(&&) 
- or(||) 
- not(!)
For Example: `tcpdump -i eth0 "src host 10.1.0.100 and (dst port 53 or dst port 80)"`
## Wireshark
• You can apply a capture filter using the same expression syntax as tcpdump (though the expression can be built via the GUI tools too). 
• You can save the output to a .pcap file or load a file for analysis.
• Another useful option is to use the Follow TCP Stream context command to reconstruct the packet contents for a TCP session

# PACKET INJECTION AND REPLAY
Often, network sniffing software libraries allow frames to be inserted (or injected) into the network stream. 
There are also tools that allow for different kinds of packets to be crafted and manipulated.
Well-known tools used for packet injection include: 
- Dsniff 
- Ettercap
- Scapy 
- hping
## hping
is an open-source spoofing tool that provides a penetration tester with the ability to craft network packets to exploit vulnerable firewalls and IDSs, hping can perform the following types of test:
- Host/port detection and firewall testing—like Nmap, hping can be used to probe IP addresses and TCP/UDP ports for responses
- Traceroute—if ICMP is blocked on a local network, hping offers alternative ways of mapping out network routes, hping can use arbitrary packet formats, such as probing DNS ports using TCP or UDP, to perform traces
- Denial of service (DoS)
## tcpreplay
As the name suggests, tcpreplay takes previously captured traffic that has been saved to a .pcap file and replays it through a network interface.
Optionally, fields in the capture can be changed, such as substituting MAC or IP addresses

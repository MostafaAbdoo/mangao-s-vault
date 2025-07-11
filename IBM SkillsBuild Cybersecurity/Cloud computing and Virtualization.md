**Virtualization** is a process by which a single physical machine can run multiple operating systems.
**virtual host** is a hosting platform that provides computing and storage resources to single or multiple websites, apps, or services, each with a unique domain name and IP address. A virtual host allows users to host multiple domains or different site versions on a single server.
**virtual appliance** is preinstalled software on one or more virtual machines that serves a specific function. You create a virtual appliance by installing a software appliance on a virtual machine that’s packaged into an image.

A virtual appliance typically comes in the **open virtualization format (OVF)**. It is either a preinstalled and configured virtual server image or a preinstalled virtual server that can be imported and used immediately. If a virtual server requires rebuilding, you can simply reimport the virtual appliance instead of reinstalling the operating system and applications from scratch.

A virtual appliance comes in **closed and open configurations**. A virtual appliance is always packaged, distributed, maintained, updated, and managed as a unit in closed configurations. In open configurations, it is accessible to customers for modification and has interfaces for custom configuration or delivering patches and updates. Virtual appliances are critical in quickly provisioning operating systems and applications in cloud delivery platforms.

VMware provides several different virtualization products for various-sized environments. VMware Workstation Pro is ideal for running virtual machines on Windows, Linux, or Mac computers and is good for creating local virtual environments for learning or testing. VMware vCenter is a centralized management platform designed for VMware environments. This platform offers administrators a single web portal to efficiently oversee virtual machines (VMs), hosts, storage, and networking components. Key functionalities include VM provisioning, monitoring, resource allocation, performance optimization, live migration (vMotion), and high availability (HA). VMware vCenter is particularly suited for handling large-scale vSphere deployments.

**hypervisor** is a unique software that enables a single physical computer to run multiple virtual machines.

----
**Network virtualization** combines hardware, software resources, and network functionality into a single, software-based system. With network virtualization, providers can combine multiple physical networks into one virtual, software-based network. They can also divide a single physical network into a separate, independent virtual network.
- **Internal network virtualization** creates a pretend network inside a single server to make the server more efficient. With network virtualization, software containers are set up on the server. With containers, different operating systems and apps can run on the same server. Internal virtualization provides many benefits, such as using less hardware, being more flexible, and changing network resources to meet different needs.
- **External network virtualization** helps service providers create virtual local area networks (VLANs) by either grouping physical systems that are connected to the same LAN or dividing separate LANs into the same VLAN. Both approaches enhance network efficiency by allowing providers to optimize their server resources. For example, a provider can use external network virtualization to make separate VLANs for different groups or customers. Each group would have its own security policies and network settings.
**Network functions virtualization (NFV)** is a technology that virtualizes network services, such as routers, firewalls, and load balancers, by packaging them as virtual machines or containers on standard servers, resulting in improved network speed, flexibility, and security.
**network interface card (NIC)** is a hardware component that connects a computer or other electronic device to a network. The NIC provides a physical interface for the device to send and receive data over the network. In a virtualized environment, you create and use a **virtual network interface card (VNIC)** to represent a physical NIC.
- Data travels from the zone through its VNIC.
- The data goes through the virtual switch.
- Finally, the data goes through the physical interface (NIC), which transmits the data to the network.
**Public - Private - Hybrid Cloud**

---
new type of private cloud called a community cloud is becoming popular among certain business communities.
- With a community cloud, multiple businesses **share the same infrastructure** provided by a CSP. This infrastructure includes software and development tools customized for a specific community’s needs.
- Each business also has its own **private cloud space** customized to meet its security, privacy, and compliance needs.
- The rise of community clouds shows how cloud computing is evolving. CSPs are combining different types of clouds and service models to create **customized cloud solutions** that meet the specific needs of each business.
----
**Cloud as a service (CaaS)** refers to application and infrastructure resources that reside on the internet. Cloud service providers contract with individual or corporate subscribers who can use these services without paying for or maintaining hardware and software.1
**Infrastructure as a service (IaaS)**, you can rent virtualized hardware resources from the CSP.
**Software as a service (SaaS)** delivers applications over the internet. Users can access these applications through a web browser or mobile app without installing software on their local devices.
**platform as a service (PaaS)**, developers can build, deploy, and manage applications without concern about the technical foundation.
**Database as a service (DBaaS)** provides users with access to a fully managed database system through a CSP

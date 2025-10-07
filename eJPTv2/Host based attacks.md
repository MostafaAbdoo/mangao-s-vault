## What Are System/Host Based Attacks?

- **System/Host based attacks** are attacks that are targeted towards a specific system or host running a specific operating system, for example, **Windows** or **Linux**.
- **Network services** are not the only attack vector that can be targeted during a **penetration test**.
- System/Host based attacks usually come in to play after you have gained access to a target network, whereby, you will be required to **exploit servers, workstations or laptops** on the internal network.
## Frequently Exploited Windows Services

| Protocol/Service                                    | Ports              | Purpose                                                                                                                                                   |
| --------------------------------------------------- | ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Microsoft IIS** (Internet Information Services)   | TCP ports 80/443   | Proprietary web server software developed by Microsoft that runs on Windows.                                                                              |
| **WebDAV** (Web Distributed Authoring & Versioning) | TCP ports 80/443   | HTTP extension that allows clients to update, delete, move and copy files on a web server. WebDAV is used to enable a web server to act as a file server. |
| **SMB/CIFS** (Server Message Block Protocol)        | TCP port 445       | Network file sharing protocol that is used to facilitate the sharing of files and peripherals between computers on a local network (LAN).                 |
| **RDP** (Remote Desktop Protocol)                   | TCP port 3389      | Proprietary GUI remote access protocol developed by Microsoft and is used to remotely authenticate and interact with a Windows system.                    |
| **WinRM** (Windows Remote Management Protocol)      | TCP ports 5986/443 | Windows remote management protocol that can be used to facilitate remote access with Windows systems.                                                     |

## Microsoft IIS

- **IIS (Internet Information Services)** is a proprietary extensible web server software developed by Microsoft for use with the **Windows NT family**.
- It can be used to **host websites/web apps** and provides administrators with a robust **GUI** for managing websites.
- IIS can be used to host both **static and dynamic web pages** developed in **ASP.NET** and **PHP**.
- Typically configured to run on **ports 80/443**.
- Supported executable file extensions:
    - **.asp**
    - **.aspx**
    - **.config**
    - **.php**

## WebDAV Exploitation

- The **first step** of the exploitation process will involve **identifying** whether **WebDAV** has been configured to run on the **IIS web server**.
- We can perform a **brute-force attack** on the **WebDAV server** in order to identify **legitimate credentials** that we can use for **authentication**.
- After obtaining legitimate credentials, we can **authenticate** with the **WebDAV server** and **upload a malicious .asp payload** that can be used to **execute arbitrary commands** or obtain a **reverse shell** on the target.

## Tools

- **davtest** - Used to **scan, authenticate and exploit a WebDAV server**.
- **cadaver** - cadaver supports **file upload, download, on-screen display, in-place editing, namespace operations (move/copy), collection creation and deletion, property manipulation, and resource locking** on WebDAV servers.

---
## SMB

- **SMB (Server Message Block)** is a **network file sharing protocol** that is used to facilitate the **sharing of files and peripherals (printers and serial ports)** between computers on a **local network (LAN)**.
- SMB uses **port 445 (TCP)**. However, originally, SMB ran on top of **NetBIOS** using **port 139**.
- **SAMBA** is the open source **Linux implementation of SMB**, and allows **Windows systems to access Linux shares and devices**.

## SMB Authentication

- The **SMB protocol** utilizes **two levels of authentication**, namely:
    - **User Authentication**
    - **Share Authentication**
- **User authentication** - Users must provide a **username and password** in order to authenticate with the SMB server in order to access a share.
- **Share authentication** - Users must provide a **password** in order to access restricted share.

## PsExec

- **PsExec** is a lightweight **telnet-replacement** developed by **Microsoft** that allows you **execute processes on remote windows systems** using any user's credentials.
- **PsExec authentication** is performed via **SMB**.
- We can use the **PsExec utility** to **authenticate** with the target system legitimately and run **arbitrary commands** or launch a **remote command prompt**.
- It is very similar to **RDP**, however, instead of controlling the remote system via **GUI**, commands are sent via **CMD**.
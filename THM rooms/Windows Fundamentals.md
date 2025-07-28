# The File System
The file system used in modern versions of  Windows  is the **New Technology File System** or simply  [NTFS](https://docs.microsoft.com/en-us/windows-server/storage/file-server/ntfs-overview) .

Before NTFS, there was  **FAT16/FAT32** (File Allocation Table) and **HPFS** (High Performance File System). 

You still see FAT partitions in use today. For example, you typically see FAT partitions in USB devices, MicroSD cards, etc.  but traditionally not on personal Windows computers/laptops or Windows servers.

NTFS is known as a journaling file system. In case of a failure, the file system can automatically repair the folders/files on disk using information stored in a log file. This function is not possible with FAT.   

NTFS addresses many of the limitations of the previous file systems; such as: 

- Supports files larger than 4GB
- Set specific permissions on folders and files
- Folder and file compression
- Encryption ( [Encryption File System](https://docs.microsoft.com/en-us/windows/win32/fileio/file-encryption) or **EFS** )

Another feature of NTFS is **Alternate Data Streams** ( **ADS** ).

Alternate Data Streams  (ADS) is a file attribute specific to Windows  NTFS  (New Technology File System).

Every file has at least one data stream ( `$DATA` ), and ADS allows files to contain more than one stream of data. Natively [Window Explorer](https://support.microsoft.com/en-us/windows/what-s-changed-in-file-explorer-ef370130-1cca-9dc5-e0df-2f7416fe1cb1) doesn't display ADS to the user. There are 3rd party executables that can be used to view this data, but [Powershell](https://docs.microsoft.com/en-us/powershell/scripting/overview?view=powershell-7.1) gives you the ability to view ADS for files.

From a security perspective, malware writers have used ADS to hide data.

Not all its uses are malicious. For example, when you download a file from the Internet, there are identifiers written to ADS to identify that the file was downloaded from the Internet.
# Windows / System 32 Folders
The Windows folder (`C:\Windows`) is traditionally known as the folder which contains the Windows operating system. The folder doesn't have to reside in the C drive necessarily. It can reside in any other drive and technically can reside in a different folder.

This is where environment variables, more specifically system environment variables, come into play. Even though not discussed yet, the system environment variable for the Windows directory is `%windir%`.

The System32 folder holds the important files that are critical for the operating system.
# UAC
A user doesn’t need to run with high (elevated) privileges on the system to run tasks that don’t require such privileges, such as surfing the Internet, working on a Word document, etc. This elevated privilege increases the risk of system compromise because it makes it easier for malware to infect the system. Consequently, since the user account can make changes to the system, the malware would run in the context of the logged-in user.

UAC (User Account Control) was introduced to protect the local user with such privileges but doesn’t apply to the local administrator account by default.

How does UAC work? When a user with an account type of administrator logs into a system, the current session doesn’t run with elevated permissions. When an operation requiring higher-level privileges needs to execute, the user will be prompted to confirm if they permit the operation to run.
# Control Panel

The Settings menu was introduced in Windows 8, the first Windows operating system catered to touchscreen tablets. The Control Panel is the menu where you will access more complex settings and perform more complex actions. In some cases, you can start in Settings and end up in the Control Panel.
# Task Manager

The Task Manager provides information about the applications and processes currently running on the system. Other information is also available, such as how much CPU and RAM are being utilized, which falls under Performance.

# System Configuration
The **System Configuration** utility (`MSConfig`) is for advanced troubleshooting, and its main purpose is to help diagnose startup issues.
The utility has five tabs across the top. Below are the names for each tab. We will briefly cover each tab in this task. 

1. General
2. Boot
3. Services
4. Startup
5. Tools
In the **General** tab, we can select what devices and services for Windows to load upon boot. The options are: **Normal**, **Diagnostic**, or **Selective**. 

In the **Boot** tab, we can define various boot options for the Operating System.
The **Services** tab lists all services configured for the system regardless of their state (running or stopped). A service is a special type of application that runs in the background.
In the **Startup** tab, you won't see anything interesting in the attached VM.  Below is a screenshot of the Startup tab for **MSConfig** from my local machine.
As you can see, Microsoft advises using **Task Manager (**`taskmgr`**)** to manage (enable/disable) startup items. The System Configuration utility is **NOT** a startup management program.

# Computer Management
The Computer Management (`compmgmt`) utility has three primary sections and we’ll cover each one in detail.
**System Tools**

Let's start with **Task Scheduler**. Per Microsoft, with Task Scheduler, we can create and manage common tasks that our computer will carry out automatically at the times we specify.

A task can run an application, a script, etc., and tasks can be configured to run at any point. A task can run at log in or at log off. Tasks can also be configured to run on a specific schedule, for example, every five mins.
Next is **Event Viewer**.

Event Viewer allows us to view events that have occurred on the computer. These records of events can be seen as an audit trail that can be used to understand the activity of the computer system. This information is often used to diagnose problems and investigate actions executed on the system.
**Shared Folders** is where you will see a complete list of shares and folders shared that others can connect to.
Under **Sessions**, you will see a list of users who are currently connected to the shares. In this VM, you won't see anybody connected to the shares.

All the folders and/or files that the connected users access will list under **Open Files**.

The **Local Users and Groups** section you should be familiar with from [Windows Fundamentals 1](https://tryhackme.com/room/windowsfundamentals1xbx) because it's `lusrmgr.msc`.

In **Performance**, you'll see a utility called **Performance Monitor** (`perfmon`).
Perfmon is used to view performance data either in real-time or from a log file. This utility is useful for troubleshooting performance issues on a computer system, whether local or remote.
**Storage**, Under Storage is **Windows Server Backup** and **Disk Management**. We'll only look at Disk Management in this room.
# System Information
The system information (`msinfo32`) tool gathers information about your computer and displays a comprehensive view of your hardware, system components, and software environment, which you can use to diagnose computer issues.

System Summary will display general technical specifications for the computer, such as processor brand and model.
The information displayed in **Hardware Resources** is not for the average computer user. If you want to learn more about this section, refer to the official Microsoft [page](https://docs.microsoft.com/en-us/windows-hardware/drivers/kernel/hardware-resources#:~:text=Hardware%20resources%20are%20the%20assignable,of%20bus%2Drelative%20memory%20addresses.).
Under **Components**, you can see specific information about the hardware devices installed on the computer. Some sections don't show any information, but some sections do, such as **Display** and **Input**.
In the **Software Environmen**t section, you can see information about software baked into the operating system and software you have installed. Other details are visible in this section as well, such as the **Environment Variables** and **Network Connections**.
# Resource Monitor
Resource monitor (`resmon`) displays per-process and aggregate CPU, memory, disk, and network usage information, in addition to providing details about which processes are using individual file handles and modules. Advanced filtering allows users to isolate the data related to one or more processes (either applications or services), start, stop, pause, and resume services, and close unresponsive applications from the user interface.
# Registry Editor
The Windows Registry is a central hierarchical database used to store information necessary to configure the system for one or more users, applications, and hardware devices. The registry contains information that Windows continually references during operation.
- Profiles for each user
- Applications installed on the computer and the types of documents that each can create
- Property sheet settings for folders and application icons
- What hardware exists on the system
- The ports that are being used.
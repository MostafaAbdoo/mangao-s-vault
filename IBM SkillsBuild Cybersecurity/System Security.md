**Firmware** is a critical computer program embedded in a device, providing control for the device’s specific hardware functions, it is embedded into a hardware device or component. It controls the device’s behavior and is designed to be permanently stored in non-volatile memory (NVM). Non-volatile memory stores data in a way that it can retain stored information even when you turn off the device.
- **Low-level firmware** is stored on a non-volatile memory chip, such as read-only memory (ROM). Therefore, you cannot rewrite or update it; it is an inherent part of the device. It provides the most basic control for a device’s hardware, typically managing the hardware’s initial startup processes.
- **High-level firmware** is used with flash memory chips for updating. It usually has more complex instructions than low-level firmware. It offers more functionality and control, often providing an interface for the user to interact with the hardware, such as the firmware in a printer that manages print jobs and user settings.
- **Subsystem firmware**, or device firmware, is a specialized type of firmware that functions independently of the main system firmware. For example, a printer operates with its own firmware, but firmware is also embedded in the ink cartridge chip to manage communication with the printer about ink levels.

Many device types that you might interact with daily contain firmware:
- - **Computers:** Basic input/output system (BIOS), unified extensible firmware interface (UEFI) 
    
- **Mobile devices:** Bootloader, baseband firmware 
- **Medical devices:** Heart defibrillator 
- **Smart home devices:** Internet of Things (IoT) devices such as smart dishwashers and thermostats 
- **Consumer electronics:** Devices such as TVs, Blu-ray players, and digital cameras 
- **Peripherals:** Printers, routers, keyboards, and mice
## **Common security vulnerabilities**
- Missing data encryption 
- OS command injection 
- SQL injection 
- Buffer overflow 
- Missing authentication for critical function 
- Missing authorization 
- Unrestricted upload of dangerous file types 
- Reliance on untrusted inputs in a security decision 
- Cross-site scripting and forgery 
- Downloading of codes without integrity checks 
- Use of broken algorithms 
- URL redirection to untrusted sites
# **Firmware hacking**
- **Code injection** : First, the attacker injects malware into the lower-level code that controls a device before and after system initialization.
- **Destructive actions** : After the malware has infiltrated the system, it can conduct various destructive actions. For example, it can alter and disable the firmware, target specific sections of the operating system, and infiltrate software.
Firmware hacking can take various forms, including malware, bootkits, and rootkits, exploiting infected USB flash drives, corrupting hard disk drives, and faulty firmware products.

----------------

**Jailbreaking** and **rooting** are methods used to gain access to the underlying operating system of a device, such as a smartphone or a tablet. Jailbreaking is the term used for iPhones, and rooting is the term used for Androids.
These methods involve removing software restrictions that the device manufacturer intentionally put in place, thereby opening the door to installing software not made available by the manufacturer.

------------
A **boot program** is software that loads the OS into the computer, allowing applications to interact with its hardware. The boot program resides in firmware, such as BIOS, and it runs when you turn on the computer.

The **kernel** is the core component of the OS. It’s software that manages essential functions of the OS, such as managing memory and device drivers and scheduling processes, and ensures proper coordination between hardware and software. When application programs require specific services from the OS, they use the designated application program interface (API) that the kernel provides.

**OS Types** :
- server OS
- Workstation OS
- Mobile OS

**OS security** is the process of ensuring the OS’s confidentiality, integrity, and availability. It involves protecting the OS from threats, such as viruses, worms, malware, and remote hacker intrusions.

**System hardening** is the process of securing a computer system or server by mitigating potential vulnerabilities.
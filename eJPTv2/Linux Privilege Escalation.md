[[THM rooms/Linux Privilege Escalation|Linux Privilege Escalation]]  https://youtu.be/krCjTu_hy3s?si=NPpxf2HncNE2SD6f

After gaining access we need an interactive shell, we can achieve this goal using python or ruby and so on. But we need to make sure it's installed first using `which` command.
Then we write `python -c 'import pty;pty.spwan("/bin/bash")'`

first we try Kernel Exploit (in real target try it last because it may crash the kernel if the exploit didn't work well)
run command `uname -a` this will give u the kernel version and you can search for it's exploit
you can move the exploit to the target machine using python server and `wget` command and add the execute permission on the file `chmod +x` and then run the file

tools : https://www.101labs.net/comptia-security/lab-86-how-to-enumerate-for-privilege-escalation-on-a-linux-target-with-linpeas/
https://github.com/rebootuser/LinEnum

sites : https://lolbas-project.github.io/#
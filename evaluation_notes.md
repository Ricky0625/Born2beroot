# Born2beroot Evaluation Notes

## Project overview

- What is virtual machine

Imagine you have a computer that runs on Window Operating System. As we know, a computer has hardware like CPU, Storage and RAM. And then you have Window operating system that used to communicate with your hardware, to control how the application is going to use all these hardware resources. And on top of the operating system, we will have applications that communicates with the operating system. Now, what if we want to use Linux OS? By that, we maybe need another computer with different hardware, and on top of that is the Linux Operating System.

What if I tell you that you can install the Linux OS on top of the Windows OS? This can be achieve with "Virtualization". With virtualization, you dont need to have a separate set of physical hardware or a physical computer to install an operating system. (Examples here...).

To do that, you need something called "Hypervisor". Hypervisor, also known as virtual machine monitor, is a software that creates and runs virtual machines (VMs). A hypervisor allows one physical computer to host multiple operating systems on top of the operating system that you have installed.

- How does a virtual machine works

So how does a virtual machine works? When we are creating a VM using Hypervisor, it will ask for some of the hardware resources that manage by your operating system to create virtual CPU, virtual RAM, virtual storage for the VM. So it means that now you are sharing your hardware resources to run multiple operating system.

Noted that these virtual machine doesn't know the existence of the host. They are completely isolated. The VM thinks itself as a independent computer and it's the only one running on the computer. The seperation between host and VM is actually great because if something breaks inside VM, it will not affect the host machine. The host machine might not even know what's happening inside the VM.

- The purpose of virtual machines

There are couples of usage for virtual machines:

1. Just explore new operating system, learn something new
2. Digital Forensic. When the digital forensic engineers want to inspect the compromised operating system, the common practice is to capture the disk image of the suspect's operating system and create a VM based on it. This is because they don't want to modify any data on the suspect's computer because the evidence might be modified and deleted in the process.
3. Test your app on different OS. So maybe you are developing an cross-platform app but you want to see how it works on different platform right? Then you can create a VM to test the application.

> Digital forensics is used to gain additional evidence after a crime has been committed to support charges against the suspect. It also used after a cyberattack.

- Choice of operating system

My choice of operating system for this project is Debian 11. It's the latest stable version of Debian.

- Differences between CentOS and Debian

1. Both are free and have great community support. But if we really want to compare, CentOS actually has a larger community providing support and it also receive support by Red Hat Limited meaning CentOS has its own source of funding. Hence, CentOS is more stable.
2. Both get updated on a timely basis. But the difference is that CentOS does not support regular major updates, but some minor bug fixes or patches to make the system stable. Debian on the other had, does support regular version updates every 2 to 3 years.

- The difference between aptitude and apt, what is APPArmor

Aptitude and apt both are tools to handle package management in Linux. It can perform activites on packages such as installation, removal etc.

> In linux, a package file is the basic unit of software. These packages can contain the source code, libraries, scripts, metadata of the software you installed. The software can be a GUI application, Command line tool and so on.

Apt is a command line tool with no GUI. It is a low-level package magement tool. To use that, just type `apt-get [package name]` then your application will be installed.

Aptitude is a more advanced packaging tool, which provides a menu-like interface for the user to interact with. It is a high-level package management tool.

What's the differences between them?

Firstly, apt is low-level package management tool while aptitude is a high-level package management tool. Low-level tools mainly handles tasks like installing and removing package files, while high-level tools can perform more advanced activites on top of the low-level tools such as metadata searching, dependency resolution and such. Aptitude even can suggest the user which package to install and why they should install it.

Secondly, it's their interface. apt only has command line interface while aptitude provides a menu-like interface for the user to interact.

To conclude, aptitude has more advanced features hence a better package management tool compared to apt.

What is AppArmor? AppArmor is basically a software that allows the system administator the ability to restrict access to application, scripts, or restrict a program to only do certain things.

For example, if I run chrome and visit a malicious site that tries to install virus or malware in my `/home/` directory and delete all the files in it. So, if you configure the AppArmor correctly, like not allowed chrome to accses my home directory, then no harm can be done.

## Simple setup

- Check if the machine does not have a graphical environment at launch
- A password will be requested before attempting to connect to the machine (Encrypted volume)
- Login, not root
- Check UFW service is started

```shell
# check service status
sudo service ufw status

# enable ufw
sudo ufw enable

# allow connection using Port xxxx
sudo ufw allow 4242

# check the list of allowed port
sudo ufw status
```

- Check SSH service is started

```shell
# check service status
sudo service ssh status
```

- Check the info of operating system in terminal

```shell
uname -a
```

## User

- list all the users

```shell
# get list of users
getent passwd

# check whether a user exists in the Linux system
getent passwd | grep jack
```

- Check if my login user belongs to the "sudo" and "user42" groups

```shell
# list all the groups
groups

# check if I exists in the group
getent group sudo
getent group user42
```

- How I set up the password policy

There are 2 files that need to be modified.

1. `/etc/login.defs`
2. `/etc/pam.d/common-password`. comes with the `libpam-pwquality` package, a password policy management tool.

Configure the password age policy:

- Password expire every 30 days
- Minimum number of days between password changes to 2 days
- Send warning message 7 days before password expiry date

Configure password strength:

- minimum length 10
- uppercase, lowercase letter at least one
- maximum of 3 consecutive identical characters
- avoid using username as password
- the new password need to have at least 7 character difference from the old one
- apply password policy for root user as well

- Check if the password policy has been implemented by following the steps below:
    1. Create new user

        ```shell
        # add user
        sudo adduser username
        ```

    2. Assign password, follow the password policy, test password policy in the process

        ```shell
        # check password expiry information
        sudo chage -l username

        # change password
        passwd <username>
        ```

    3. Create a group named, "evaluating"

        ```shell
        sudo addgroup evaluating
        ```

    4. Assign the newly created user to the group

        ```shell
        sudo adduser <username> evaluating
        ```

    5. Check if the user belongs to the group

        ```shell
        getent group evaluating
        ```

- Advantages of having password policy

The main benefit of having password policy is that they makes your passwords harder to crack. The more rules you enforce, the higher the number of possible combinations of letters, numbers, and characters. This increases the amount of work a computer will have to do to crack the password. The goal is not create an uncrackable password, because such a thing doesn't exist. With enough time and computing power, all passwords can be cracked. So the goal is that it makes a password diffifcult to crack.

With password policy, your system will be secured. The risk of having cyberattacks, data breaches will be decreased as well.

The rules imposed in the subject is pretty much the standard, when comes to password policy.

- Disadvantages of its implementation

When the password policy is getting more complex, it might force the user to create a password that they can't even remember. Or when you ask the user to change their password every month, they might get frustrated real quick. Or, they can't even think of a new password that is easy to remember.

For me, it seems like these are like minor problems, the security of the system is way more important. So password policy is a need.

## Hostname and partitions

- Check if the hostname of the machine follow this format: login42
- Modify hostname by replacing the login with the evaluator's username, reboot the machine

```shell
sudo nano /etc/hostname
```

- Check if the hostname is updated or not
- Restore machine to the original hostname
- Show the partitions of the virtual machine, compare output with the subject

```shell
lsblk
```

- Explain how LVM works, what it is all about

LVM is a tool for logical volume management which includes allocating disks, moving, or resizing logical volumnes. With LVM, a hard drive or a set of hard drives is allocated to one or more physical volumes.

## SUDO

Sudo is a command in a Unix-like operating system that allows you to temporarily elevate your current user account to have root previleges. Some tasks requires the user to have the priviledge of a system adminstrator to do. For example like, change the hostname, modify a config file, etc.

- Check if "sudo" is installed

```shell
dpkg -l | grep sudo
```

`dpkg` is a low-level package management and it's the main package management system in Debian or Debian-based system. `dpkg -l`. show the list of package

`grep` is a command that search for a string in groups of files. `grep sudo` to get "sudo".

- Assign the user to the sudo group

```shell
sudo adduser <username> sudo
sudo usermod -aG sudo <username>
```

> Grant a user the prvileges to use sudo

- Show the implementation of the rules (sudo)
- Verify if the "/var/log/sudo" folder exists and has at least one file

Show the file, before and after using a sudo command

## UFW

- Check if "ufw" is installed

```shell
dpkg -l | grep ufw
```

- Check if it's working

```shell
sudo service ufw status
```

- what is UFW and its value

UFW (Uncomplicated Firewall) is an utility that is designed to simplify the setup and management of firewall rules. So basically, a firewall is a tool that used to get control over who or what is able to access our server or machine.

- List all the active rules in UFW. Port 4242 must exist

```shell
sudo ufw status
```

- Add new rule (port 8080), check if it's added

```shell
sudo allow 8080
```

- Delete the added rule

```shell
# delete rules with numbered index
sudo ufw status numbered
sudo ufw delete 3

# delete rules
sudo ufw delete allow 22/tcp
```

## SSH

- Check if the SSH service is installed

```shell
dpkg -l | grep ssh
```

- Check if it's working

```shell
sudo service ssh status
```

- What is SSH and the value of using it

SSH or Secure Shell is a network commmunication protocol that enables two computers to communicate and share data. A crucial feature of ssh is that the communication between two computers is encrypted meaning that it is suitable for use on insecure netwroks.

- SSH service only uses port 4242

Check `/etc/ssh/sshd_config`

- SSH on local machine to log in the VM

```shell
ssh wricky-t@localhost -p 4242
```

- Check if cannot use root user in SSH

Try this and this should not work

```shell
ssh root@localhost -p 4242
```

## Script monitoring

- how the script works

```shell
# To display system information, -a is to show all information
arc=$(uname -a)
# /proc/cpuinfo shows what type of processor your system is running
# sort is to sort a file, arrangin the records in a particular order
# uniq is to filter out the repeated lines in a file
# wc -l. word count. -l to count newlines
pcpu=$(grep "physical id" /proc/cpuinfo | sort | uniq | wc -l)
vcpu=$(grep "^processor" /proc/cpuinfo | wc -l)
# free outputs a summary of RAM usage, including total, used, free, shared, and available memory and swap space.
# awk is designed for text processing and typically used as a data extraction and reporting tool
# awk $0 for whole line
# $1 for first field
# $2 for second field, etc
fram=$(free -m | awk '$1 == "Mem:" {print $2}')
uram=$(free -m | awk '$1 == "Mem:" {print $3}')
pram=$(free | awk '$1 == "Mem:" {printf("%.2f"), $3/$2*100}')
# df is used to display disk space used in the file system
# it defines the number of blocks used, the number of blocks available
fdisk=$(df -Bg | grep '^/dev/' | grep -v '/boot$' | awk '{ft += $2} END {print ft}')
udisk=$(df -Bm | grep '^/dev/' | grep -v '/boot$' | awk '{ut += $3} END {print ut}')
pdisk=$(df -Bm | grep '^/dev/' | grep -v '/boot$' | awk '{ut += $3} {ft+= $2} END {printf("%d"), ut/ft*100}')
# top shows a real-time view of running processes in Linux and displays kernel-managed tasks. Provides a summary of the resource utilization, CPU and memory usage
cpul=$(top -bn1 | grep '^%Cpu' | cut -c 9- | xargs | awk '{printf("%.1f%%"), $1 + $3}')
# show the list of currently logged in users
lb=$(who -b | awk '$1 == "system" {print $3 " " $4}')
# show partitions details
lvmt=$(lsblk | grep "lvm" | wc -l)
lvmu=$(if [ $lvmt -eq 0 ]; then echo no; else echo yes; fi)
#You need to install net tools for the next step [$ sudo apt install net-tools]
ctcp=$(cat /proc/net/sockstat{,6} | awk '$1 == "TCP:" {print $3}')
# show the user names of users currently logged in to the current host
# wc -w is to cound words
ulog=$(users | wc -w)
ip=$(hostname -I)
mac=$(ip link show | awk '$1 == "link/ether" {print $2}')
cmds=$(journalctl _COMM=sudo | grep COMMAND | wc -l) # journalctl should be running as sudo but our script is running as root so we don't need in sudo here

# wall is used to displays a message on the terminals of all logged-in users
wall "#Architecture: $arc
#CPU physical: $pcpu
#vCPU: $vcpu
#Memory Usage: $uram/${fram}MB ($pram%)
#Disk Usage: $udisk/${fdisk}Gb ($pdisk%)
#CPU load: $cpul
#Last boot: $lb
#LVM use: $lvmu
#Connexions TCP: $ctcp ESTABLISHED
#User log: $ulog
#Network: IP $ip ($mac)
#Sudo: $cmds cmd"
```

- what "cron" is

In simple words, "crontab" is a job scheduler. By using cron, you can let the system to run a task automatically according to your schedule. The schedule is called the crontab.

> edit the file ```sudo crontab -u root -e```

The crontab format:

```shell
MIN HOUR DOM MON DOW CMD

# MIN Minute
# HOUR Hour
# DOM Day of Month
# MON Month
# DOW Day of week
# CMD Command
```

- ensure that the script runs every 10 minutes

```shell
10 * * * * CMD
```

- Runs every 30 seconds

> Sleep command is used to delay for a fixed amount of time during the execution of any script

```shell
* * * * * CMD
* * * * * (sleep 30s && CMD)
```

- make the script stop running when the server has started up

```shell
@reboot CMD
```

## Bonus

- Show wordpress

```shell
# open in the browser
localhost:80
```

- Why postfix?

For my extra service, I chose to install postfix, which is a open-source mail transfer agent that helps in delivering emails on a Linux System.

How to configure?

Edit `/etc/postfix/main.cf`

1. Set up app password for gmail, a secondary password that you can give out to different applications that is associated with your gmail account but it's more secure than giving our your real gmail account password.

2. Create `/etc/postfic/sasl/sasl_password`. This file is basically for your gmail credentials. SMTP. Simple Mail Transfer Protocol, is a communication protocol behind the email communication.

3. Create hash database file of our gmail credentials

```shell
sudo postmap /etc/postfix/sasl/sasl_passwd
```

Now have a plain text file that consists of our gmail credentials. So we need to make sure that only the root can modify or access the file. To do that we need to change the permissions.

```shell
# Change ownership
sudo chown root:root /etc/postfix/sasl/sasl_passwd /etc/postfix/sasl/sasl_passwd.db

# Change mod
sudo chmod 0600 /etc/postfix/sasl/sasl_passwd /etc/postfix/sasl/sasl_passwd.db
```

4. Configure the `/etc/postfix/main.cf`

> relayhost is a server which your email are sent first from your server before being delivered to the actual recipient's server. It's like poslaju lah.

```shell
# change this line from
relayhost = 

# to this
relayhost = [smtp.gmail.com]:587

# add these lines
# Enable SASL authentication
smtp_sasl_auth_enable = yes
smtp_sasl_security_options = noanonymous
smtp_sasl_password_maps = hash:/etc/postfix/sasl/sasl_passwd
smtp_tls_security_level = encrypt
smtp_tls_CAfile = /etc/ssl/certs/ca-certificates.crt
```

Why I installed this?

We can co-operate postfix with crontab so that the system can send an email automatically according to your schedule. So for example, let's say you are hosting a website on this VM and you are not available in front of the computer but you want to know the status every 2 hours. So you just to create a cron task and set it to 2 hours and your email account will receive the status of the server every 2 hours. And you don't have to physically present infront of your computer to know the server status.

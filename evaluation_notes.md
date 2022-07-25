# Born2beroot Evaluation Notes

## Project overview

- How a virtual machine works
- Choice of operating system
- Differences between CentOS and Debian
- The purpose of virtual machines
- The difference between aptitude and apt, what is APPArmor

## Simple setup

- GUI based operating system, CLI based operating system
- Check UFW service is started

```shell
sudo service ufw status
```

- Check SSH service is started

```shell
sudo service ssh status
```

- Check the info of operating system in terminal

```shell
uname -a
```

## User

- Check if my login user belongs to the "sudo" and "user42" groups
- How I set up the password policy
- Check if the password policy has been implemented by following the steps below:
    1. Create new user
    2. Assign password, follow the password policy
    3. Create a group named, "evaluating"
    4. Assign the newly created user to the group
    5. Check if the user belongs to the group
- Advantages of having password policy
- Advantages and disadvantages of its implementation

## Hostname and partitions

- Check if the hostname of the machine follow this format: login42
- Modify hostname by replacing the login with the evaluator's username, reboot the machine
- Check if the hostname is updated or not
- Restore machine to the original hostname
- Show the partitions of the virtual machine
- Compare output with the subject
- Explain how LVM works, what it is all about

## SUDO

- Check if "sudo" is installed
- Assign the user to the sudo group
- The value and operation of sudo
- Show the implementation of the rules (sudo)
- Verify if the "/var/log/sudo" folder exists and has at least one file
- Check the file that has a history of the commands used with sudo
- Run a sudo command, check if the file in "var/log/sudo" has been updated

## UFW

- Check if "ufw" is installed
- Check if it's working
- what is UFW and its value
- List all the active rules in UFW. Port 4242 must exist
- Add new rule (port 8080), check if it's added
- Delete the added rule

## SSH

- Check if the SSH service is installed
- Check if it's working
- What is SSH and the value of using it
- SSH servicve only uses port 4242
- SSH on local machine to log in the VM
- Check if cannot use root user in SSH

## Script monitoring

- how the script works
- what "cron" is
- ensure that the script runs every 10 minutes
- make the script stop running when the server has started up

## Bonus

- Show wordpress
- postfix. why install this

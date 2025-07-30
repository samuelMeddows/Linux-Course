# Linux Basics

### User Accounts

- Systems Accounts
  - Don't have a home directory
- Regular Users
  - Can access thier own files and directories
  - Can not perform admin tasks or access othe user's files
- Superuser (root)
  - Unretricted access to the entire system
  - Can add/remove users, install software and change the system configuration

**Extras**
Show everything in uname: `uname -a`
Show the kernel release: `uname -r`

How do you check how much disk space is left (human redable): `df -ah`

How to check a service: `systemctl status <serviceName>`

Checking networking socket/ports: `netstat -tcplpn`

Check CPU usage for a process: `ps aux | grep netmanager` or `top` or `htop`

How would you mount a new HDD/USB Drive:
`ls /mnt`
`mount /dev/sda2 mnt/`

Check existing mounts: `mount`

Mount on boot: `nano /etc/fstab`

### Elevating Privlages: sudo

View the file in /root (not acceeible to Regular Users)
`sudo ls /root`

Change to root user
`sudo -i`

---

### Installing Software - RHEL Based

Update software registry and upgrade any packages: `sudo dnf update && upgrade -y`

Istall additional package list: `sudo dnf install epel-release`

Enable additional package lists: `sudo crb enable` `sudo dnf update`

Install software: `sudo dnf install cowsay`

Uninstall software: `sudo dnf remove cowsay`

It is recommended to update after software install: `sudo dnf remove cowsay; dnf update`

Note: Yum is still vailid but is beng replaced by dnf.

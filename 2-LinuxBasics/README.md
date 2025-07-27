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

---

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

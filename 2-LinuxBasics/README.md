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

### Package Management

Package management is slighty different from each linus distro.

<b>Debian Based</b>

<b>RHEL Based</b>

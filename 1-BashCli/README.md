# Bash CLI

### Echo command

Fix copy paste issue from host VM<br>
example: `^[[200~echo -e 'Hello \nWorld!'~`

1. Create the file: `touch ~/.inputrc`
2. Edit it using nano: `nano ~/.inputrc`
3. Add this line to the file: `set enable-bracketed-paste off`
4. Save and exit (Ctrl+s, then Ctrl+x)
5. Apply the change immediately: `bind -f ~/.inputrc`

Show Bash version: `echo "${BASH_VERSION}"`

Echo text without line break: `echo -n 'Hello World!'`

Echo with format characters: `echo -e 'Hello \nWorld!'`

Echo arguments can be combined together: `echo -en 'Hello \nWorld!'`

---

### File Navigation Commands

Print working directory: `pwd`

Change Directory: `cd Deskstop`

Change to root folder: `cd /`

Change up a directory: `cd ..`

Change to Home directory: `cd ~`

Change to desktop referncing the Home directory: `cd ~/Desktop/`

List contents of directory: `ls`

List in reverse order: `ls -r`

List contents sorting by time: `ls -t`

List all file including hidden and show permissions: `ls -al`

Specify a path to list: `ls ~/Desktop`

---

### Executing Multiple Commands

Combining echo: `echo -n 'Hello '; echo 'World!'`

Combining cd with ls: `cd ..; ls`

---

### Help and Manual

Show help: `ls --help` or `ls --h`

Show manual: `man ls`

---

### Installing Software

Update software registry and upgrade any packages: `sudo dnf update && upgrade -y`

Istall additional package list: `sudo dnf install epel-release`

Enable additional package lists: `sudo crb enable` `sudo dnf update`

Install software: `sudo dnf install cowsay`

Uninstall software: `sudo dnf remove cowsay`

It is recommended to update after software install: `sudo dnf remove cowsay; dnf update`

Note: Yum is still vailid but is beng replaced by dnf.

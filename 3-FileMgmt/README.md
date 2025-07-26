# File Managment

### Create

Create an empty file: `touch readme.txt`
Create multiple files: `touch sarah.txt lisa.txt laura.txt`
Create a folder: `mkdir invites`
List files/folders with additional information: `ls -l`

---

### Move and Rename

Move file to new location: `mv sarah.txt invites/`
Rename a file: `mv sarah.txt sara.txt`
Rename a folder: `mv invites-bu/ invites-bak/`

---

### Copy

Copy a file: `cp laura.txt eva.txt`
Copy to a folder: `cp laura.txt invites/casey.txt`
Copy a folder: `cp -R invites/ invites-bu/`

---

### Delete

Delete file(s): `rm Create Create`
Delete folder: `rm -r invites-bak/`
Delete empty folder. This can prevent data loss as it will not run if the folder contains files : `rmdir invites`

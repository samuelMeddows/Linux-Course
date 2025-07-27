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

---

### Filename Expansion (globbing)

Move all images into images folder: `mv *.jpg images`
Move all files from images back up one foler:`mv images/* .`

---

### Advanced Globbing Wildcards

- `*` Matches everything
- `?` Matches any single character
- `[0-9]` Square brackets allows us to specify a character range.
- `**` Matches zero up to arbitararily many characters including `/`
  - Only support in Bash 4.0 or higher
  - It may need to be enabled with `shopt -s glabstar`

Show all .jpg file: `echo **.*.jpg`
Copy all .jpg nad .mov files up one folder: `cp **/*.jpg **/*.mov .`

### Be Careful with Globbing

- The name of a file might be interpreted as a paramaeter. -rf is a valid file name.
- If we run `rm *` and there is a file named -rf.
  - The `*` will be expanded, so -rf will appear in the command
  - `rm` will think that -rf is a parameter
    - `-r` means recursive
    - `-f` means don't ask

A safer approach: `rm ./*`

---

### Exersize

Exersize - Get all excel and pdf files from each department for Jan and Feb.<br>
List the directory: `tree`<br>
.
├── Export
├── Purchasing
│   ├── 01 - January
│   │   ├── Balance Sheet.xlsx
│   │   └── Invoice.pdf
│   ├── 02 - February
│   │   ├── additional-table.xlsx
│   │   ├── important-business-figures.xlsx
│   │   ├── important-invoice.pdf
│   │   └── not-important.mp4
│   └── 03 - March
│   └── numbers-from-march.xlsx
└── Sales
├── 01 - January
│   └── Sales-January.xlsx
├── 02 - February
│   └── Balance-Sheet-February.pdf
└── 03 - March
├── Additional-Invoice-March.pdf
└── Sales-March.xlsx

Exersize Solution:<br>
`mkdir Export`
`*/0[1-2]*/*.{pdf,xlsx} Export`

### File Searching - Find

Search Current folder recursively: `find .`
Find a type of file: `find . -type f`
Find a type of folder: `find . -type d`
Find all files that have been modified in the last 7 days: `find . -type f -mtime -7`
Find all file that are lager than 10mb: `find size +1M . -type f `
Delete all empty files in a directory: `find . -empty -delete`
Get help for find: `man help`

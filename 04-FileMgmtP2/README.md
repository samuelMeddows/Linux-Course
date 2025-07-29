# File Managment Part 2

Show file content: `cat bash.txt`
Add content to file: `echo "Bash is cool!" >  bash.txt`
Show the start of a text file: `head -n 5 data.txt`
Show the end of a text file: `tail -n 5 data.txt`

### Reading large files - less

less allows us to read through files using scroll (arrow keys) and f (page down) and b (page up): <br>
`less data.txt`
Quit less: `q`

**While in less:**
Go to 50% of the document: `:50p`
Check the location of the document: `:= (ENTER)`
Show line numbers: `:-N (ENTER)`

**Searching in less:**
Forward search: `:/Search Term (ENTER)`
Backward search: `:?Search Term (ENTER)`

### Get the size of a file - wc

Get the word count of a file: `wc data.txt` or `wc -lwc data.txt`<br>

- `-l` Number of lines
- `-w` Number of words
- `-c` Number of bits (file size)
  This outputs the number of lines, words and bytes of the file:
  `5642  29003 163708 data.txt`

### Get the disk usage of a file - du

Disk usage of current folder: `du`
Disk useage of file: `du data.txt`
This will show a number of units. In CentOS one unit is 1024 bytes.
Disk useage of file in human redable format: `du -f data.txt`

### Editing files in CLI - nano

Edit a text file: `nano data.txt`
Confirm filename and save: ctrl+o
Save: ctrl+s
Exit: ctrl+x

### Exersize - Analyze a log file

1. What could the log file be? : Web server log
2. How large is the file? : 2.6mb
3. How many lines does it contain? 10000 lines

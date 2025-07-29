# Pipes - Data Processsing Through Command Chaining

Pipe the ls output to wc list: `ls | wc -l`
Show number of non existing files: `du -h file1.txt file-not-exist.txt 2>&1 > /dev/null | wc -l`

---

### Dual Output: the Utility tee

Write echo output to a file (-a is append): `echo 'Hello World!' | tee -a hello.txt`
Output ping and any errors to file: `ping google.com 2>&1 | tee ping.txt`
Note: tee is great for logging errors from a programs output.

---

### Shorterning & Removing duplicates - sort & uniq

Sort the contents of a file or stdin:`sort`

- By default: alhabetical order
- sort -r: sort in reverse alhabetical order
- sort -n: sorts numbers in numerical order
- sort -c: check wether the contents in a file are sorted and find unsorted elements
- sort -k column_number (starting at 1): sort data by a sepcific column

Sort a list of users to stdout: `sort users.txt`
Revers a list of users to stdout: `sort -r users.txt`
Sort by last name (second column): `sort -k 2 users.txt`

Remove duplicate lines with uniq: `sort users.txt | uniq`
Sort can do the same with -u: `sort -u users.txt`
Show duplicate lines in a file `sort users.txt | uniq -d`

---

### Searching for Patterns in Text - grep

Basic usage: `grep -F 'pattern' file.txt`
It can also worn on stdin: `[command] | grep -F 'pattern'`
Note: Grep work with every file. It can work with binary data but it is not recommended to use grep in this way.

List files that cotain file in the filaname `ls | rep -F 'file'`
Print out the IP address information: `ip a | grep -F 'inet'`

---

### Chatacter Replacement & Reversal - tr & rev

Replace b with d from echo output: `echo 'bash' | tr 'b' 'd'`
Convert lower to uppercase: `echo 'awsome' | tr 'a-z' 'A-Z'`
Delete characters: `echo 'Bash is Awesome' | tr -d ' '`
Reverse a string: `echo 'Was it a cat I saw? | rev'`

---

### Selective Extraction - cut

It allows us to process and extract data from a standard input.
Cut by bytes: `uptime | cut -b 1-10`
Cut by chatacters: `uptime | cut -c 1-10`
Cut by fields - 2 is needed as uptime starts with a white space: `uptime | cut -d ' ' -f 2`

---

### Text Substitution with the Stream Editor - sed

Sed can delete lines, insert lines or replace lines.
Replace word: `echo 'Hello world!' | sed 's/world/bash/g'`

---

### Exersize - Log File Analysis

1. How many requests have downloaded .zip-files? `cat access.log | grep -F '.zip' -c` : 4061
2. How different .zip-files have been downloaded? : `grep -F '.zip' access.log | cut -d ' ' -f 7 | uniq | wc -l `

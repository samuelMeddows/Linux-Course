# Redirection - Manage Data Streams

**Echo a string**
Insert a string into a file: `echo 'Bash is aswesome! > file.txt'`
If the file does not exist it will be created, else it will be overwritten.

**Send output to a file >**
Send ls output to a file (overwrite): `ls > output.txt`
Append ls output to a file: `ls >> output.txt`

---

# The Standard Streams - stdin, stdout, stderr

By default there are 3 communication channels for data

- 0: sandard input (from the keyboard); **stdin**
- 1: standard output (on the screen); **stdout**
- 2: standard error output (on the screen); **stderr**

By using > or >> we are redirecting the output to a file.
If a file does not exist, the stdout and stderr gets printed to the screen.

---

### Managing Error Messages: redirecting stderr

A program may print errors we wish to ingore or me may want to redirect error to a particular file:
File does not exist log error: `du -h IMG_9328.jpg > output.txt 2> error.txt`
File does not exist ignore errors: `du -h IMG_9328.jpg IMG_2210.jpg 2> /dev/null > output.txt`

---

### Redirecting stdout to stderr

Redirect output and error to the same file: `du -h IMG_9328.jpg >> output.txt 2>> output.txt`

Redirect stderr to stdout: `du -h IMG_9328.jpg IMG_2210.jpg > output.txt 2>&1`

---

### Redirecting stdin

Not adding a file name will open stdin: `wc -l`

```
This is Bash!
Another Line
```

ctrl+d
Output is 2.

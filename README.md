# OverTheWire-Bandit-Writeup
My hands-on learning journey through OverTheWire Bandit, covering Linux commands, file permissions, encoding, compression, SSH, networking, and basic cybersecurity concepts.
Introduction
I started the OverTheWire Bandit wargame to improve my Linux and cybersecurity skills.
Bandit is a beginner-friendly Linux-based security challenge where each level requires finding a password for the next level.
While solving these levels, I learned how to use Linux commands, search files, work with permissions, decode data, connect to network services, and use SSH.
This file contains my personal learning notes and commands from the Bandit levels I completed.
Level 0 → Level 1
Objective
The first level required me to connect to the Bandit server using SSH and find the password stored in a file called readme.
Commands
```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
ls
cat readme
```
What I learned
ssh is used to connect to a remote computer.
ls displays files and directories.
cat displays the contents of a file.
Level 1 → Level 2
Objective
The password was stored in a file whose name was -.
Command
```bash
cat ./-
```
What I learned
Linux treats - specially because it can represent standard input.
Using ./- tells Linux that - is the filename in the current directory.
Level 2 → Level 3
Objective
The password was stored in a file containing spaces in its filename.
Command
```bash
cat "spaces in this filename"
```
Another way:
```bash
cat spaces\ in\ this\ filename
```
What I learned
Spaces normally separate arguments in Linux commands.
Quotes or backslashes can be used when a filename contains spaces.
Level 3 → Level 4
Objective
The password was stored inside a hidden file.
Commands
```bash
cd inhere
ls -la
cat .hidden
```
What I learned
Files beginning with . are hidden files in Linux.
The -a option of ls displays hidden files.
Level 4 → Level 5
Objective
There were several files in the inhere directory. Only one contained human-readable text.
Commands
```bash
cd inhere
file ./*
```
I checked the output and identified the file containing human-readable text.
Then:
```bash
cat ./<filename>
```
What I learned
The file command helps identify the type of a file.
This is useful when the filename does not tell us what kind of data is inside.
Level 5 → Level 6
Objective
I had to find a file with specific properties:
Human-readable
Exactly 1033 bytes
Not executable
Command
```bash
find . -type f -size 1033c ! -executable
```
Then I displayed the correct file:
```bash
cat ./<filename>
```
What I learned
The find command is very useful for searching files based on conditions.
Some useful options are:
-type f → search for files
-size 1033c → search for a file of exactly 1033 bytes
! -executable → exclude executable files
Level 6 → Level 7
Objective
The password was somewhere on the server.
I had to find a file that:
Belonged to user bandit7
Belonged to group bandit6
Was exactly 33 bytes
Command
```bash
find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null
```
After finding the file:
```bash
cat <filename>
```
What I learned
I learned how Linux file ownership works.
-user searches for files owned by a particular user.
-group searches for files belonging to a particular group.
2>/dev/null hides permission-denied error messages.
Level 7 → Level 8
Objective
The password was stored in data.txt next to the word millionth.
Command
```bash
grep millionth data.txt
```
What I learned
grep is used to search for specific text inside files.
This is one of the most useful commands when working with large text files.
Level 8 → Level 9
Objective
The password was the only line that appeared only once in the file.
Command
```bash
sort data.txt | uniq -u
```
What I learned
I learned about Linux pipes.
sort sorts the lines.
uniq -u displays only unique lines.
| sends the output of one command to another command.
Level 9 → Level 10
Objective
The password was hidden among readable strings inside a binary file.
Command
strings data.txt | grep "="
What I learned
strings extracts readable text from binary files.
Combining commands with pipes makes it easier to filter the required information.
Level 10 → Level 11
Objective
The contents of data.txt were encoded using Base64.
Command
```bash
base64 -d data.txt
```
What I learned
Base64 is an encoding method used to represent binary data using text characters.
The -d option tells the command to decode the Base64 data.
Level 11 → Level 12
Objective
The password was encoded using ROT13.
Command
```bash
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```
What I learned
ROT13 replaces each letter with the letter 13 positions away in the alphabet.
I also learned how the tr command can be used to translate characters.
Level 12 → Level 13
Objective
The password was stored in a file that had been compressed multiple times.
I had to repeatedly identify the file type and extract it until I reached the readable password.
Commands
First, I copied the file to /tmp so that I could work safely:
```bash
cp data.txt /tmp/bandit12
cd /tmp
```
Then I checked the file type:
```bash
file bandit12
```
Depending on the output, I used commands such as:
```bash
gunzip
bunzip2
tar -xf
```
I repeated the process until the final file contained readable text.
What I learned
This level helped me understand different compression formats.
I also learned why the file command is important when working with unknown files.
Level 13 → Level 14
Objective
Instead of directly finding the password, I was given an SSH private key.
I used the key to log in as the next Bandit user.
Command
```bash
ssh -i sshkey.private bandit14@localhost
```
After logging in:
```bash
cat /etc/bandit_pass/bandit14
```
What I learned
SSH can authenticate users using private keys instead of passwords.
I also learned that private keys are sensitive information and should never be uploaded publicly.
Level 14 → Level 15
Objective
The password had to be sent to a service running on port 30000.
Command
```bash
nc localhost 30000
```
Then I entered the current Bandit password.
What I learned
nc stands for Netcat.
It can be used to create network connections and communicate with services running on specific ports.
Level 15 → Level 16
Objective
This level required connecting to a service using SSL/TLS on port 30001.
Command
```bash
openssl s_client -connect localhost:30001
```
After the connection was established, I entered the current Bandit password.
What I learned
I learned that openssl s_client can be used to create an SSL/TLS connection to a server.
I also got a basic understanding of how encrypted network communication works.
Commands I Practiced
```text
ssh
ls
cat
cd
file
find
grep
sort
uniq
strings
base64
tr
gunzip
bunzip2
tar
nc
openssl
```
I also practiced Linux concepts such as:
File permissions
Hidden files
File ownership
Groups
File searching
Pipes
Encoding and decoding
Compression
SSH authentication
Network ports
SSL/TLS connections
What I Learned From Bandit
The biggest thing I learned from Bandit is that cybersecurity is not only about knowing security tools.
A strong understanding of Linux and command-line operations is also very important.
These challenges helped me become more comfortable with:
Linux terminal
File systems
Permissions
Searching files
Network connections
Encoding and decoding
Compression
SSH
Basic security concepts
I also learned that when solving a security problem, it is important to understand what the command is actually doing instead of simply copying commands.
Security Note
The passwords obtained while solving Bandit are intentionally not included in this file.
I have also not uploaded any private SSH keys or other sensitive information.
This file is only for documenting my learning process and the techniques I practiced.
Conclusion
OverTheWire Bandit gave me a good starting point for learning Linux and cybersecurity.
Each level introduced a different concept, and solving the challenges helped me understand how Linux commands can be combined to investigate and solve problems.
I plan to continue with more OverTheWire challenges and improve my practical cybersecurity skills.
Platform
OverTheWire Bandit
I am using this file as a personal learning log while practicing Linux and cybersecurity fundamentals.

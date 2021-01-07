# Bandit- 


Bandit
Ayush Vatsa

Login : ssh bandit<level_no.>@bandit.labs.overthewire.org -p 2220

Level 1


The file named “-”
Terminal cannot read file names starting with - or some other special commands.
We use ./<file_name> to refer the the file.

Command: ~$ cat ./-

Pass:CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9




Level 2

File has the name “SPACES IN THIS FILENAME”
To determine it is one file name and not a different files name (“Spaces” “in” “this” “filename”), we have to use double inverted commas

Command: ~$ cat “spaces in this filename”

Pass: UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK



Level 3

The password file is inside a directory “inhere”. The file with the password is hidden. Hidden file names in Unix start with “.” (e.g- “.vatsa”, “.unix”). Command “ls” cannot view hidden files. Attribute needed.

Command: ~$ cd inhere
       ~$ ls -a
	       ~$ cat ./.hidden

Pass:pIwrPrtPN36QITSp3EQaw936yaFoFgAB





Level 4

Again the password file is in a directory “inhere”. But there are many files but only one with readable text(ASCII text). Hence we try to find the file types.


Commands- ~$ cd inhere
	        ~$ ls -a
	        ~$ File ./*
(Returns that only -file7 has readable text)
	        ~$ Cat ./-file7

Pass:koReBOKuIDDepwhWk7jZC0RTdopnAYKh



Level 5

Again the password is in the directory “inhere” but there are a lot of directories. And the search criterias are given. Checking every folder and every file within is not efficient. Hence mass search.

Commands: ~$ cd inhere
	        ~$ find -type f -size 1033c

Pass:DXjZPULLxYr17uwoI01bNLQbtFemEgo7


Level 6

The file with the password is hidden somewhere. But the search credentials are given.

User- bandit7		group- bandit6		size- 33Kb

Commands: ~$ find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
Used for getting output. (Giving a target to computer)

Pass:HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs






Level 7

The password for the next level is stored in the file data.txt next to the word millionth. The file is too big to search manually.


Commands: ~$ awk '/^millionth/ {print $2;}' data.txt
Pass: cvX2JJa4CFALtqS87jk27qwqGhBM9plV



Level 8
The password is the only line of text that appears once. We use sorting methods.


Commands:~$ $cat data.txt | sort | uniq -q
Pass: UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR


Level 9

The password for the next level is stored in the file data.txt in one of the few human-readable strings, beginning with several ‘=’ characters.

Commands: ~$ strings data.txt | grep “=”

Pass: truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk


Level 10

The password is in data.txt which is base64 encoded. We need to find the contents and decode it.

Commands: ~$ cat data.txt
	       
~$ echo VGhlIHBhc3N3b3JkIGlzIElGdWt3S0dzRlc4TU9xM0lSRnFyeEUxaHhUTkViVVBSCg== | base64 --decode


Pass: The password is IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR



Level 11

The password file has shuffled letters. We need to rearrange the alphabet with the given schematics. We use tr command here.

Commands: ~$ cat data.txt
~$  echo Gur cnffjbeq vf 5Gr8L4qetPEsPk8htqjhRK8XSP6x2RHh | tr [a-zA-Z] [n-za-mN-ZA-M]

Pass: The password is 5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu



Level 12

Bitch level!!

The password is a hexdump file that has been compressed repeatedly. We do not have the write permission hence we have to make a file in tmp folder and cpy the contents of data.txt to decompress there.
Before each decompression, we have to determine the file type and decompress according to the file type we’ll give extension and decompress until we get file type as ASCII text.

Commands used: file <filename>
	                 Mv <file name> <file name req>
		     Decompression
┌────────────────────────────────────────┬─────────────────────────┐
|     file <fileName> Output message     |  Command to decompress  |
├────────────────────────────────────────┼─────────────────────────┤
| fileName.gz:  gzip compressed data     | gzip -d <fileName.gz>   |
| fileName:     bzip2 compressed data    | bzip2 -d <fileName.bz2> |
| fileName:     POSIX tar archive (GNU)  | tar xvf <fileName.tar>  |
└────────────────────────────────────────┴─────────────────────────┘
┌─────────┬───────────────────────────────────────────────┐
│ Options │       Explanation (tar -xvf <fileName>)       │
├─────────┼───────────────────────────────────────────────┤
│ x       │ Extract the archive                           │
│ v       │ Displays verbose information                  │
│ f       │ Creates the archive with the given <fileName> │
└─────────┴───────────────────────────────────────────────┘


Pass: The password is 8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL
 


Level 13

In this level, we are introduced to the use of .private key. We need to login to the localhost using the provided sshkey.

Commands: ~$ ssh -i sshkey.private bandit14@localhost
	~$ cat /etc/bandit_pass/bandit14

Pass: 4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e


Level 14

We need to login to the current localhost through port 30000.

Commands: cat /etc/bandit_pass/bandit14 | nc localhost 30000

Pass: Correct!
BfMYroe26WYalil77FoDi9qh59eK5xNr


Level 15


The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL encryption.
We will use openssl here. We will use s_client -quiet -connect attributes and pipe the password from the last level.

Commands: ~$ echo BfMYroe26WYalil77FoDi9qh59eK5xNr | openssl s_client -quiet -connect localhost:30001

Pass: Correct!
cluFn7wTiGryunymYOu4RcffSxQluehd

cluFn7wTiGryunymYOu4RcffSxQluehd








Level 16

------Bitch Level part 2------

The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000.
We need to check the ports that are active first. We use nmap to find the ports that are active between the given range. After that, we check what each of the ports contain in the given file. 


Finally we get an RSA key in the port 31790. After we get the rsa key we need to create a ssh key to login to the next level.  



Commands: ~$ nmap -p 31000-32000 -sV localhost
~$cat /etc/bandit_pass/bandit16 | openssl s_client -connect localhost:31790 -quiet
We get the RSA key here

-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----

Now we save this RSA key in a file “sshkey.private” in /tmp/vatsa144(created by us) and use this key file to login to Bandit 17 and get the password there.

~$ mkdir /tmp/vasta144
~$ cd /tmp/vatsa144
~$ nano sshkey.private
(Enter the RSA key found inside the nano text editor and save it.)

~$ cat sshkey.private
(should return the RSA key with the heading and tha footer)

~$ chmod 600 sshkey.private
(To change the authentication and can be read by just the user)
~$ ssh -i ./sshkey.private bandit17@localhost

Now we are inside bandit17
We need to find the pass.

~$ cat /etc/bandit_pass/bandit17

Pass: xLYVMN9WE5zQ5vHacb0sZEVqbrp7nBTn



Level 17

WE need to find the different line in the files given (Password.new password.old). Kinda easy.

Commands: ~$ diff pass*
<output> 

< kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd
---
> w0Yfolrc5bwjS4qw5mq1nnQi6mF03bii

#note that our pass is  kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd because “Diff” shows what needs to be changed to make the files equal.

Pass:  kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd



Level 18

Someone has edited the .bashrc file to log us out whenever we login using ssh. So we need to login with a different method. We can pipe the later commands.

Commands: ~$ ssh bandit18@bandit.labs.overthewire.org -p 2220 ls -la
~$  ssh bandit18@bandit.labs.overthewire.org -p 2220 cat readme
Pass: IueksS7Ubh8G3DCwVzrTd8rAVOwq3M5x



Level 19

This level basically teaches us to use setuid commands. 

Commands: ~$ ls -a
~$ ./bandit20-do
<output>
Run a command as another user.
  Example: ./bandit20-do id

~$ ./bandit20-do cat /etc/bandit_pass/bandit20


Pass: GbKksEFF4yrVs6il55v6gwY5aVje5f0


Level 20

We need to use two terminals. We connect to a port on one terminal and while it is connected, we use another window to run the given setuid file to get the pass.

Commands: ~$ ls -a
~$ nc -l 31234 < /etc/bandit_pass/bandit20

#in other terminal
~$ ./suconnect 31234

<output>
Read: GbKksEFF4yrVs6il55v6gwY5aVje5f0j
Password matches, sending next password


Pass: GbKksEFF4yrVs6il55v6gwY5aVje5f0j

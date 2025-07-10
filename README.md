# Nibbles_Walkthrough

This is my first machine on HackTheBox, so don't judge me harshly. I'm doing this for the first time 😅

P.S. I used guided mode for this machine

### Task 1: How many open TCP ports are listening on Nibbles?
Used nmap -sT <ip_address> for this.

<img width="1152" height="365" alt="image" src="https://github.com/user-attachments/assets/14f8144f-8703-4bbd-8ee0-6a47b506ec3a" />

There are 2 open TCP ports: 80 and 22.

### Task 2: What is the relative path on the webserver to a blog?
To find this, I opened http://<ip_address> in my browser and checked the source code (Ctrl+U).
Web Page:

<img width="1917" height="800" alt="image" src="https://github.com/user-attachments/assets/d3180453-fec6-48c8-8270-67d2904080d4" />

Source Code:

<img width="1919" height="790" alt="image" src="https://github.com/user-attachments/assets/a73e9cc3-1775-4767-ba0a-06b48000a64b" />

The /nibbleblog/ directory is the answer.

### Task 3: What content management system (CMS) is being used by the blog?
Went to http://<ip_address>/nibbleblog/ and saw that it's powered by Nibbleblog.

<img width="1366" height="769" alt="image" src="https://github.com/user-attachments/assets/2af29068-21e6-48bc-acfe-23c9c026b8b6" />

### Task 4: What is the relative path to an XML file that contains the admin username?
I enumerated files using Gobuster:

<img width="1093" height="659" alt="image" src="https://github.com/user-attachments/assets/842acd30-1936-486a-8248-7f546a916fd4" />

Found several useful paths and looked through them. Found that /nibbleblog/content/private/users.xml contains the admin username.

### Task 5: What is the admin user's password to log into the blog?

I managed to log in using a common password: nibbles.

Struggled a bit to find the login page. It’s located at http://<ip_address>/nibbleblog/admin.php.

<img width="1392" height="736" alt="image" src="https://github.com/user-attachments/assets/2cac0869-78fd-4548-b5ef-27b92e436418" />

### Task 6: What version of Nibbleblog is running on the target machine? (Do not include the "v")
Went to "Settings" in the admin panel, scrolled down, and found the version: 4.0.3

<img width="894" height="172" alt="image" src="https://github.com/user-attachments/assets/6c55cec0-dbe9-4d4d-b68c-22e0598df39d" />

### Task 7: What is the 2015 CVE ID for an authenticated code execution by file upload vulnerability in this version of Nibbleblog?
I browsed exploits for Nibbleblog 4.0.3 and found:
CVE-2015-6967

### Task 8: Which user is the Nibbleblog instance running as on the target machine?
To find this out, I needed shell access. I searched for available exploits using Searchsploit, and found one:

<img width="1918" height="197" alt="image" src="https://github.com/user-attachments/assets/16a38814-dd27-4db0-858a-2116748a8fea" />

Used Metasploit, loaded the exploit:

<img width="1469" height="716" alt="image" src="https://github.com/user-attachments/assets/11e22868-75fc-45f9-a4c7-c2645fcfe1b6" />

Set the required options, ran the exploit, and got a shell. Ran whoami — I was logged in as nibbler.

<img width="1076" height="726" alt="image" src="https://github.com/user-attachments/assets/b20786bb-29ea-4a85-8174-694929774024" />

### Task 9: Submit the flag located in the nibbler user's home directory.
I was already in the shell, so I just navigated to the home directory and retrieved the flag.

<img width="394" height="220" alt="image" src="https://github.com/user-attachments/assets/6b84ef02-f6cb-4d24-b39c-5d601db3ffd3" />

### Task 10: What is the name of the script that nibbler can run as root on Nibbles?
Used sudo -l to find this:

<img width="1263" height="167" alt="image" src="https://github.com/user-attachments/assets/8f489952-7135-46c8-94dd-47d0be21f851" />

### Task 11: Enter the permission set on monitor.sh (use Linux format like -rw-rw-r--).
Unzipped personal.zip and listed the permissions using ls -l.

<img width="621" height="357" alt="image" src="https://github.com/user-attachments/assets/64c9e6de-4a16-4b9d-ab31-22cce5abad3c" />

### Task 12: Submit the flag located in root's home directory.
I removed the original monitor.sh and created my own monitor.sh file on my host machine with the content bash -i
Then I hosted it and downloaded it onto the victim machine.

<img width="990" height="546" alt="image" src="https://github.com/user-attachments/assets/bacb627e-0fe5-4bba-bf85-1612e6152918" /> 
<img width="849" height="375" alt="image" src="https://github.com/user-attachments/assets/0f71283a-0216-472e-b8f4-bede9d49153f" />

Made the file executable and ran it using sudo (since I could run this script as root). This gave me a root shell.

<img width="819" height="248" alt="image" src="https://github.com/user-attachments/assets/b254ac96-74ed-4663-8045-751521e29bb5" />

Navigated to /root and opened root.txt.

<img width="434" height="219" alt="image" src="https://github.com/user-attachments/assets/94dd0fc6-2cc6-4ec9-811a-748e1a52c136" />





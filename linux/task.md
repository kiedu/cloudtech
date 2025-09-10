# LINUX VM TASK

>Source: https://campus.epam.ua/ua/training/3619


### Prerequisite
* [tester](tester) (binary file) to check your task status
* virtual machine (VM) on your local machine:
- Operating system: Ubuntu 20.04 (Server) Note: in case the link above is expired, look for the new ubuntu-20.04.X-live-server-amd64.iso file on the page [https://releases.ubuntu.com/focal/](https://releases.ubuntu.com/focal/)
- Default user: ubuntu
- there is NO swap partition
- 2 disks (see Additional Materials > Virtualizations > How to add hard disks to VM):
primary disk for installing OS (e.g., 20GB)
secondary disk for LVM tasks (e.g., 1GB)


### Task Description
A task may contain the following sections:

* Description: describe the task
* Before: show you how VM may look before getting the task done
* After: show you how VM may look after getting the task done
* Notes: contains some tips or advice

### How to work with the task
To find answer (secret phrase) on a quiz, you need to pass tester's test with the same name (tester must be run on your VM). If you passed, a secret phrase is printed.

Example:
```
ubuntu@ubuntu:~$ ./tester
[ Check Linux VM task ], 1..27 tests
-----------------------------------------------------------------------------------
✓  1  Change of VM hostname: hostname, 21ms, secret phrase: "SECRET_PHRASE_HERE"
✗  2  Change of VM hostname: resolve hostname to 127.0.0.1, 40ms
...
```

## Change of VM hostname

Description:
* Change VM hostname to student
* The new hostname name should be resolved to 127.0.0.1.
* The changes should be permanent.

Example:
Before:
```
ubuntu@ecsc00a0cdda:~$ hostname -s
ecsc00a0cdd9
ubuntu@ecsc00a0cdda:~$ hostname -f
ecsc00a0cdda.epam.com
ubuntu@ecsc00a0cdda:~$ hostname -i
10.6.220.135
ubuntu@ecsc00a0cdda:~$ hostname -A
ECSC00A0CDDA.epam.com
```
After:
```
ubuntu@ecsc00a0cdda:~$ sudo reboot
::1 127.0.0.1
ubuntu@abutsko:~$ hostname -s
student
ubuntu@abutsko:~$ hostname -f
student
ubuntu@abutsko:~$ hostname -i
127.0.0.1
ubuntu@abutsko:~$ hostname -A
student
```
Notes:
You have not done something if you see the following warning:
* 'sudo: unable to resolve host: Temporary failure in name resolution'
* hostname: Temporary failure in name resolution



## User Creation

Description:
Create a new user:
* login: student
* user ID: 1040
* Primary group: student_group (group ID: 1050)
* home directory: /home/student_home
* Supplementary groups: cdrom

Example:

Before:
```
ubuntu@abutsko:~$ id student
id: ‘student’: no such user
```
After:
```
ubuntu@abutsko:~$ id student
uid=1040(student) gid=1050(student_group) groups=1050(abutsko_group),24(cdrom)
ubuntu@abutsko:~$ getent passwd abutsko | cut -d: -f6
/home/abutsko_home
ubuntu@abutsko:~$ su student
Password: 
student@abutsko:/home/ubuntu$ whoami
student
abutsko@abutsko:/home/ubuntu$ echo $HOME
/home/student_home
abutsko@abutsko:/home/ubuntu$ 
```


## LVM Configuration
Description:

Configure LVM on an available device:
* Create a physical volume on an available device
* Create a volume group student
* Create a logical volume student:
- size: 50% of free space
- file system: ext4
* mount (UUID must be used) the logical volume to /lvm/student directory (permanently)

Before:
```
root@abutsko:/home/ubuntu# lvdisplay
root@abutsko:/home/ubuntu#
```
After:
```
root@abutsko:/home/ubuntu# reboot
ubuntu@abutsko:~$ lsblk | grep student-student
└─abutsko-abutsko 253:0    0  508M  0 lvm  /lvm/student
ubuntu@abutsko:~$ mount | grep student-student
/dev/mapper/student-student on /lvm/student type ext4 (rw,relatime)
root@abutsko:/home/ubuntu# lvdisplay
  --- Logical volume ---
  LV Path                /dev/student/student
  LV Name                student
  VG Name                student
  LV UUID                Rvn8DC-6RLz-Erjm-bsI7-me5Q-3DfS-SfcMc5
  LV Write Access        read/write
  LV Creation host, time student, 2021-11-24 14:04:09 +0000
  LV Status              available
  # open                 0
  LV Size                508.00 MiB
  Current LE             127
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:0
```


## Sudo Rights
Description:

Provide student user sudo rights to:
* use sudo apt install and sudo apt-get install commands without asking password
* use other commands with asking password

**Leave the default sudoers file untouched** (if you add your configuration here, the test fails). Instead, find another way to do it according best practices.

Before:
```
student@student:~$ whoami
student
student@student:~$ sudo echo
[sudo] password for student: 
student is not in the sudoers file.  This incident will be reported.
student@student:~$ sudo apt install vim
[sudo] password for student: 
student is not in the sudoers file.  This incident will be reported.
student@student:~$ sudo apt update
[sudo] password for student: 
student is not in the sudoers file.  This incident will be reported.
```
After:
```
student@student:~$ whoami
student
student@student:~$ sudo -k
student@student:~$ sudo echo
[sudo] password for student: 

student@student:~$ sudo -k
student@student:~$ sudo apt install vim
Reading package lists... Done
...
Do you want to continue? [Y/n] n
Abort.
student@student:~$ sudo -k
student@student:~$ sudo apt update
[sudo] password for student: 
Hit:1 http://mirror.minsk.epam.com/ubuntu focal InRelease
...
73 packages can be upgraded. Run 'apt list --upgradable' to see them.
student@student:~$ 
```
Notes:

* Use sudo -k command for cleaning sudo password cache


## Swap Creation

Description:

Create a swap file:
* size: 1G
* swap file should be permanently mounted (even after rebooting)

Before:
```
ubuntu@abutsko:~$ sudo swapon --show
ubuntu@abutsko:~$ free -h | grep -i 'swap'
Swap:            0B          0B          0B
```
After:
```
ubuntu@abutsko:~$ sudo reboot
ubuntu@abutsko:~$ sudo swapon --show
NAME             TYPE  SIZE USED PRIO
/swap1/swap_file file 1024M   0B   -2
ubuntu@abutsko:~$ free -h | grep -i 'swap'
Swap:         1.0Gi          0B       1.0Gi
```


## Docker Installation

Description:

Install Docker using the Docker repository:
* version: 19.03.15
* docker can be run from student user

Before:
```
abutsko@abutsko:~$ docker version
Command 'docker' not found, but can be installed with:
apt install docker.io
Please ask your administrator.
```
After:
```
abutsko@abutsko:~$ docker version
Client: Docker Engine - Community
 Version:           19.03.15
 ...

Server: Docker Engine - Community
 Engine:
  Version:          19.03.15
  ...
```

Notes:

If you see the following error (e.g., run docker version command):
* Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.40/version: dial unix /var/run/docker.sock: connect: permission denied

>>it means Docker can not be run from student user


## NGINX Installation

Preprequisite
* 'jq' cli-tool MUST BE installed since it is used in the test suite for parsing your page. You can use the following command to install it:
```
abutsko@abutsko:~$ sudo apt install jq
```

Description:

Install and run NGINX:
* port: 8080
* create a custom HTML page (This is a static page your values can be hardcoded) to retrieve json:
```
{
  "name": "student",
  "os": "<ubuntu>",
  "ip": "<vm-private-ip",
  "cpu": "<cpu-count>",
  "memory": "<memory in Gi>"
}
```

After:
```
abutsko@abutsko:~$ apt -qq list nginx
nginx/focal-updates,focal-security,now 1.18.0-0ubuntu1.2 all [installed]
abutsko@abutsko:~$ sudo ss -lntp | grep 8080
LISTEN    0         511                0.0.0.0:8080             0.0.0.0:*        users:(("nginx",pid=21276,fd=6),("nginx",pid=21275,fd=6))                      
LISTEN    0         511                   [::]:8080                [::]:*        users:(("nginx",pid=21276,fd=7),("nginx",pid=21275,fd=7))                      

abutsko@abutsko:~$ curl student:8080
{                   
  "name": "student",
  "os": "Ubuntu 20.04.3 LTS",
  "ip": "10.6.220.135",
  "cpu": "1",
  "memory": "1.6"
}
```


## Cron Job

Description:

Create a cron job:
* to create a new backup file for /etc/nginx/nginx.conf
* backup file pattern: <file_name>-<timestamp>.conf.bak
* timestamp: number of seconds since 1970-01-01 00:00:00 UTC
* save backup files in /etc/nginx/conf.d/ directory
* do not use additional scripts when doing nginx.conf backup

Before:
```
ubuntu@abutsko:~$ ls /etc/nginx/conf.d/
ubuntu@abutsko:~$
```
After:
```
ubuntu@abutsko:~$ ls /etc/nginx/conf.d/
nginx-1637688421.conf.bak
```



## NGINX Logrotate

Description:
Configure NGINX to weekly rotate log files using logrotate

Before:
```
abutsko@abutsko:~$ logrotate -d /etc/logrotate.d/nginx 
...
rotating pattern: /var/log/nginx/*.log  after 1 days (14 rotations)
...
```
After:
```
abutsko@abutsko:~$ logrotate -d <...>

...
rotating pattern: /var/log/nginx/*.log  weekly (14 rotations)
...
```


## Make an immutable file /immutable.txt

Description:

Make an immutable file (/immutable.txt) with the next parameters:
* owner: student
* group: cdrom
* has all permission for everyone (read, write, execute)
* cannot be removed by anyone

After:
```
student@abutsko:~$ ls -l /immutable.txt
-rwxrwxrwx 1 student cdrom 0 Nov 23 16:36 /immutable.txt
student@abutsko:~$ rm /immutable.txt
rm: cannot remove '/immutable.txt': Operation not permitted
student@abutsko:~$ sudo rm /immutable.txt
rm: cannot remove '/immutable.txt': Operation not permitted
```




## Bash Alias Creating

Description:

Create Bash alias (name: eip) for student user:
* to get an external IP address
* should be permanent (working after rebooting)

Before:
```
abutsko@abutsko:~$ eip

Command 'eip' not found, did you mean:
...
```
After:
```
abutsko@abutsko:~$ sudo reboot
abutsko@abutsko:~$ eip
86.57.255.94
```



## Static Route

Preprequisite
'yq' cli-tool MUST BE installed since it is used in the test suite. You can use the following command to install it:
```
abutsko@abutsko:~$ sudo wget -qO /usr/local/bin/yq https://github.com/mikefarah/yq/releases/latest/download/yq_linux_amd64

abutsko@abutsko:~$ sudo chmod a+x /usr/local/bin/yq
```

Description:

Create a temporary (advanced level: persistent) static route to:
* send packets to 8.8.8.8 through 4.4.4.4;

Notes:
* If the static route is on, you are not able to connect to 8.8.8.8 but others should not be affected

Before:
```
ubuntu@abutsko:~$ ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=63 time=20.4 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=63 time=20.4 ms

ubuntu@abutsko:~$ ping www.google.com
PING www.google.com (216.58.208.196) 56(84) bytes of data.
64 bytes from waw07s02-in-f4.1e100.net (216.58.208.196): icmp_seq=1 ttl=63 time=20.5 ms
```

After:
```
ubuntu@abutsko:~$ vagrant@student:~$ ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
From 10.0.2.15 icmp_seq=1 Destination Host Unreachable


ubuntu@abutsko:~$ ping www.google.com
PING www.google.com (216.58.208.196) 56(84) bytes of data.
64 bytes from waw07s02-in-f4.1e100.net (216.58.208.196): icmp_seq=1 ttl=63 time=20.5 ms
```

Notes:
* What is /etc/netplan/50-cloud-init.yaml?



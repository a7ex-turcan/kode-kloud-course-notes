# Security and File Permissions

## Linux Accounts

* The info about an user account is stored in `etc/passwd` file

* Each user has a `username` and a `UID`
* The user also has a `GID` the ID of the group they are part of
* The user can be part of multiple groups
* If no group is specified -> GID = UID
* Each user gets a `/home/michael` directory
* Defalt shell

* To check any of these:

```bash
id michael
```

### Type of accounts

#### User accounts

* Simple user that need access to the system
* Superuser Account -> root
  * have the UID = 0
  * has unrestricted access
* System Accounts
  * have UID <100 or between 500-1000
* Service Accounts
  * similar to sytem accounts

* to see the users currently logged into the sytem:

```bash
who
```

* The log records of all users

```bash
last
```

#### Switching users

* to root:

```bash
su -
```

* to give the current user admin access

```bash
sudo <any command>
```

* the config for sudo is definded under `/etc/sudoers` file

## Linux Group

* A collection of user
* The info about group is store in `etc/group`

## Access Control Files

* The password file: located at: `/etc/passwd` - doesn't actually store any passwords
* The passwords are stored at: `/etc/shadow` - the contents are hashed
* The group file: `/etc/group`

## User Management

* Create a local user

```bash
useradd bob
```

* Set the password

```bash
passwd bob
```

* Check the user is

```bash
bob
```

* Delete a user
```bash
userdel bob
```

* Add a group

```bash
groupadd -g 1011 developer
```

* Delete a group

```bash
groupdel developer
```

## Linux File Permissions

* to find the permissions for a file

```bash
ls -ld bash-script.sh
```

* The output string is decoded as:
  * first char - type of file
  * next 3 chars - owner permissions (u)
  * next 3 chars - group permissions (g)
  * next 3 chars - permissions for all other user (o)

* The permissions are checked sequentially:
  * If the user is the owner - the (u) applies - others are ignored

* The chars have the following meaning:
  * r - read - octal value: 4
  * w - write - octal value: 2
  * x - execute - octal value: 1

### Modify the permissions for a file

```bash
chmod <permissions> file
```

#### Symbolic mode

```bash
# Provide full access to owner (u)
chmod u+rwx test-file
```

```bash
#provide read acccess to owner, group and others, Remove execute access
chmod ugo+r-x
```

```bash
#Full access to Owner, add read, remove execute for group, and no access for others
chmod u+rwx,g+r-x,o-rwx test-file
```

#### Numeric mode

```bash
# Provide full acces to Owners, group and others
chmod 777 test-file
```

```bash
# Provide Read and execute access to Owners groups and others
chmod 555 
```

```bash
# Provide Read and Write access for Owner and Group, No access for others
chmod 660 test-file
```

```bash
# Full access for Owner, read and execute for group and no access for others
chmod 750 test-file
```

### Modify the Owner and the Group of a file

```bash
#change the owner and the group
chown owner:group file
```

```bash
#change just the owner
chown owner file
```

```bash
#change just the owner of all the files within the directory
chown -R owner directory
```

```bash
# change just the group
chgrp android test-file
```

## SSH and SCP

### SSH

* Used for loggin into a remote computer and executing commands

```bash
ssh [user]@<ip-address | host name>
```

* For this to work the remote server should have an SSH server running on port `22`
* The client should also have an ssh client
* The user should also exist on the remote system

#### Passwordless SSH

* Needs a key pair: Private + Public key

* To setup passowrdless ssh:
    1. Create the key pair on you local machine

        ```bash
        ssh-keygen -t rsa
        ```

        * Public key will be stored at: `/home/user/.ssh/id_rsa.pub`
        * Private key: `home/user/.ssh/id_rsa`
    2. Copy the key to the remote server

        ```bash
        ssh-copy-id bob@devapp01
        ```

        * The key on the remote will be stored under: `home/user/.ssh/authorized_keys`

### SCP

SCP allows copying data through SSH

```bash
#Copy a file
scp <path-on-client> <server>:<path-on-the-server>

# Copy a directory
scp -pr <path-on-client> <server>:<path-on-the-server>
```

## IP Tables

* Rules can be applied at either individual machine or router level
* IP Table allow to create rules that control the network traffic
* IP Tables are the alternative of the Firewall rules in Windows

* In redhand, and centos systems, iptables are installed by default
* In ubuntu you may have to install it by: `sudo apt install iptables`

* To List the default rules: `sudo iptables -L`
* 3 Types of chains are configured by default:
  * INPUT - rules for incomming connections
  * FORWARD - rules to forward data to another device (usually used in routers)
  * OUTPUT - rules for outgoing connections

* Each chain can have multiple rules
* A rule within the chain can check:
    * The source a packet is comming from
    * The destination a packet is headed to: the host, the port, the protocol

* To add an input rule:

```bash
# -A Add Rule
# -p Protocol
# -s Source
# -d Destination
# --dport Destination port
# -j Action to take
iptables -A INPUT -p tcp -s 172.16.238.187 --dport 22 -j ACCEPT
```

* The sequence of rules is very important: *The rule that appears first in the list is applied and effective, rules below it are ignored*

* To insert a rule to the top of the chain: 

```bash
# -I allows adding rules to the top of the list
iptables -I OUTPUT -p tcp -d <IP> --dport 443 -j ACCEPT
```

* To Delete a rule:

```bash
# -D - delete
# 5 - the position of the rule to be deleted
iptables -D OUTPUT 5
```

## Cronjobs

* A user can specify any date time or frequency
* This is done by the `crond` service that runs in the background

* To schedule the job

```bash
crontab -e
```

* The format of the cron expression
    1. Minutes
    2. Hours
    3. Day
    4. Month
    5. Weekday
* A `*` means any value
* Step values: `*/2` - runs every 2 minutes, months etc
* To list all the scheduled jobs:

```bash
crontab -l
```

* To inspect the if the job has been run:

```bash
tail /var/log/syslog
```

*
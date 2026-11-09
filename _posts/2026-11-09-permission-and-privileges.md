
---
title: permission and privileges
    
---


My friend is trying to break in to the world of linux system administration, he was frustrated by the lack of materials that transfer the beginner to the intermediate level, he finds either beginner material or advanced ones that does not link the missing dots that he has.

so i decided to lunch this section for bridging the upper beginner to the pre-advanced level of linux system administration.

my method of learning is to know a little bit about the subject then dive in directly with an exercise or a lab to solidify that knowledge.So it will be a definition, exercise then a solution.

**1. Linux file system.**

The Linux filesystem is a hierarchical filesystem,  
similar to Microsoft Windows. At the top level, there is the root directory, which  
is represented by the / symbol. Windows uses a drive letter, such as C:, for the  
root directory. On Windows, you can have many drive letters with their own root  
levels, such as C:, D:, E:, and so on. In Linux, there is only one root directory /, then we have subdirectories under root dir:

/ :The root filesystem only contains other directories but no individual files.  
/bin The binaries directory contains executable files. Points to /usr/bin.  
/dev The device directory contains device files to address peripherals.  
/etc Contains system configuration files for users and services.  
/home Users’ home directories  
/lib System libraries files. Points to /usr/lib.  
/media Directory for mounting media such as USB drives or DVD disks.  
/mnt The mount directory for mounting remote filesystems.  
/opt Directory in which third-party software is installed.  
/proc A virtual filesystem that tracks system processes.  
/root The root user’s home directory.  
/run Variable and volatile runtime data.  
/sbin System binary (executable) files.  
/srv Might contain data from system services.  
/sys Contains kernel information.  
/tmp Directory for storing session information and temporary files.  
/usr Programs and libraries for users and user-related programs.  
/var Variable files such as logs, spools, and queues.

we have some few commands to navigate the file sys.

pwd: print working directory,displays where you are on the filesys‐
tem

cd: change directory, a command that puts you in another directory.

ls: list. it shows what a directory contains.

**2. Permissions and Privileges.**

there are two types of users, regular one and a root user.

to become a root user you must : login as a root user, or use sudo command or su (substitute user).

**Sudo command**

The sudo command allows an appropriately configured user account to issue individual commands as the root user.

![Screenshot From 2026-08-09 19-05-40.png](.attachments.6625/Screenshot%20From%202026-08-09%2019-05-40.png)

this warning means that the user jane is not in the file of /etc/sudoers

you can do that by using the command visudo which is designed specifically for that, you must have access to root privileges to do that.

![Screenshot From 2026-08-09 19-15-12.png](.attachments.6625/Screenshot%20From%202026-08-09%2019-15-12.png)

we put jane in the sudoers file.

![Screenshot From 2026-08-09 19-31-42.png](.attachments.6625/Screenshot%20From%202026-08-09%2019-31-42.png)

now jane has super sayan sudo powers.

![Screenshot From 2026-08-09 19-34-03.png](.attachments.6625/Screenshot%20From%202026-08-09%2019-34-03.png)

  
**Reading and writing permissions**

Read (r)  numeric representation: (4).

write (w): (2).

execute (x): (1) 

none (0)

**Group permissions**

four file permission apply to a user’s group.

User : u, Group: g, other:o, All: a.

![Screenshot From 2026-08-20 18-46-45.png](.attachments.6625/Screenshot%20From%202026-08-20%2018-46-45.png)

![Screenshot From 2026-08-20 18-54-20.png](.attachments.6625/Screenshot%20From%202026-08-20%2018-54-20.png)

this is its numerical representation.

![Screenshot From 2026-08-20 19-02-27.png](.attachments.6625/Screenshot%20From%202026-08-20%2019-02-27.png)

**Changing file permission.**

to change the file permission we use the chmod command (change mode), we can use it either by numerical values or symbolic ones rwx-ugo.  

**Exercise.**

create this structure:

permission-lab/
├── public.txt
├── private.txt
└── scripts/
└── backup.sh

Requirements:

- `public.txt`: everyone can read it, but only the owner can modify it. 
- `private.txt`: only the owner can read and modify it. Nobody else should have any permissions. 
- `backup.sh`: the owner can read, modify, and execute it. Everyone else can read and execute it, but cannot modify it. 

**Solution.**

![Screenshot From 2026-08-20 18-42-42.png](.attachments.6625/Screenshot%20From%202026-08-20%2018-42-42.png)

- `public.txt`: everyone can read it, but only the owner can modify it.

![Screenshot From 2026-08-20 19-21-30.png](.attachments.6625/Screenshot%20From%202026-08-20%2019-21-30.png)

- `private.txt`: only the owner can read and modify it. Nobody else should have any permissions. 

![Screenshot From 2026-08-20 19-35-57.png](.attachments.6625/Screenshot%20From%202026-08-20%2019-35-57.png)

- `backup.sh`: the owner can read, modify, and execute it. Everyone else can read and execute it, but cannot modify it. 

![Screenshot From 2026-08-22 20-35-09.png](.attachments.6625/Screenshot%20From%202026-08-22%2020-35-09.png)

on the first ls -l we got 755 because i have created the backup.sh before because the touch command cant change the permissions it only can change the timestamp of creation.

**Umask**

the umask is telling linux take off these permissions for the newly created objects.

![Screenshot From 2026-11-09 04-56-30.png](.attachments.6625/Screenshot%20From%202026-11-09%2004-56-30.png)

the first 0 is for special permissions like set uid,set gid …

the important ones are:

      -owner : 0 : remove nothing.

      -group : 0 :remove nothing.

      -others: 2 : remove write permission.      

for the new created files they have the starting  permission of 666 because they don’t have the right to be executed. so if the umask is set to 002 then : 666-002=664 which give us : -wr-wr-w--

![Screenshot From 2026-11-09 05-13-10.png](.attachments.6625/Screenshot%20From%202026-11-09%2005-13-10.png)

for the new directories they have starting permission of 777 they need the right of x so we can access them. so if the umask is set to 002 then : 777-002=775 which give us : -wrxwrxwr-

![Screenshot From 2026-11-09 05-26-07.png](.attachments.6625/Screenshot%20From%202026-11-09%2005-26-07.png)

you may want to remove the write permission from others because you dont want everyone to write to your files. the umask gets reset to 002 every new session so you would want to set your umask permanently to 006 

![Screenshot From 2026-11-09 05-42-29.png](.attachments.6625/Screenshot%20From%202026-11-09%2005-42-29.png)

every time you login the umask is set to 006.

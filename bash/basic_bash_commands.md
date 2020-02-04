# Basics
Shell is a part of the operating system how the terminal will behave. Bash is kind of shell. Commands are case-sensitve and space used as a default separator.

### Structure of a command
`ls -l /home/robert` where:
- ls is the command
- -l /home/robert is arguments
  - -l called an option

### Paths
Every path which begins with **/** is an absolute path. **/** means the root directory. Anything else is called relative path.
- **~**: stands for your home directory
- **.**: current directory

### Important directories
`man hier` gives a general overview about file hierarchy
- `/etc:`config files
- `/var/log:`   log files
- `/bin`: commonly used programs
- `/usr/bin:` another location for programs
- `/sbin`: system binaries, often root privileges needed
- `/etc/passwd`: users listed here
- `/etc/shadow`: hashed passwords
- `/etc/group`: groups
- `/proc`: process directories
- `/temp`: temporary directory
- `/lib`: system libraries

### User management
Users can be found in `/etc/passwd`. It's important to note not all of them are real users. Many of them are just for running services. But how does one line look like? <br/>
`cloudera:x:501:501::/home/cloudera:/bin/bash` <br/>
`username:placeholderForPassword:userID:groupID:userInfo:homeDir:typeOfShell`

User **groups** can be found in `/etc/group`:<br/>
`groupName:placeholder:groupID:usersBelongToThisGroup`

**Hashed passwords** can be found in `/etc/shadow`.
`username:password:expiraryInfo` <br/>
-> **\*** in place of password means no login with that<br/>
-> **!** stands before password means login is blocked

**Users can be created** with the following command:<br/>
`useradd` more information in the manual<br/>
`newusers` for batch user creation (more users in one step)



### Commands
Command options have a short version, and long version. Short begins with -, long version with --. Short options can be appended after eachother.
- `pwd:` where I am
- `ls: ` lists contents of a directory (except hidden files, these begins with **.**)
    - `ls -l:` long listing: `drwxr-xr-x  2 ryan users 4096 Mar 23 13:34 bin`
      - first character: **-** for normal file, **d** for directory, **l** for links
      - next 9 characters are permissions
      - **ryan** is the the file or directory owner
      - **users** the group the file/directory belongs to
      - **4096** is the size
    - `ls -a`: lists hidden files also
- `cd`: change directory
  - if name contains *space* in it, put path in '`path`'
- `file:` gives information about file type
- `man`: manual page of a given command
  - pressing q to exit from manual page
- `head` and `tail`: prints first and last lines
- `less`: a convinient way to scroll through a file
- `w`, `who`: who is running what
- `ln -s`: creating symbolic link
- `command1` && `command2`: command2 will run if command1 was finished successfully
- `id`: gives information about your ownerID, groups

### Permissions
What can be done with a file?
- **r**: read the file
- **w**: write the file
- **x**: execute the file if it is a program or a script

3 type of people:
- **owner**: person who ownes the file
- **group**: every file belongs to a single group
- **others**: everyone else who is not in the group or owner

How to modify permissions?<br/>
`chmod [type_of_peope][grant_or_revoke][rwx]`:
- type_of_people:
  - **u** for owner
  - **g** for group
  - **o** for others
- grant_or_revoke:
  - **+** for granting
  - **-** for revoking

An example looks like this:
`chmod go-x /user/cloudera/frog.png`<br/>
*Another way to modify permissions is using binary bits notations.*

### Data Streams
- STDIN (0) - Standard input
- STDOUT (1) - Standard output, defaults to terminal
- STDERR (2) - Standard error, defaults to terminal

##### Operators:
- overwriting the content: **>**
- appending: **>>**
- piping: **|**

### Processes/Tasks
A process has two main parts:
- address space where it can write to
- kernel data structure where kernel keeps information about the process

Every process has a parent who started that one. If parent dies then child process is reparented to init. Init process runs first.

**Process states**
- runnable: it has everything to run just need CPU time
- sleeping: it is waiting for something
- zombie: it has finished and waiting to give information back
- stopped: the process has been stopped due to some reasons

**Process niceness**<br/>
It defines the priority of a process. The higher the number the lower the priority. Range is between -20 and 19.


`top` and `htop` is the command which lists processes out in real time. Result looks like this:
```Bash
Tasks: 174 total, 3 running, 171 sleeping, 0 stopped
KiB Mem: 4050604 total, 3114428 used, 936176 free <- memory
Kib Swap: 2104476 total, 18132 used, 2086344 free <- this is the virtual memory

 PID USER %CPU %MEM COMMAND
6978 ryan 3.0  21.2 firefox
  11 root 0.3   0.0 rcu_preempt
6601 ryan 2.0   2.4 kwin
```
Sleeping processes are waiting for a given event to occur and act upon that.

`ps` lists out processes running in your current terminal. Use `ps aux` to print out a comple system view.

`kill ProcessID` kills a process with a given PID. There are many option how to kill a process:
- TODO
- TODO

Linux has several virtual consoles. We can switch between consoles by pressing **CTRL + ALT + F[1-7]**.<br/>

**Background processes** <br/>
`jobs` lists out background processes. To put a process in the background type `sleep 5 &`, use the **&** smybol.

A running process in the foreground can be put in the background as stopped by pressing **ctrl+z**. A background stopped process can be taken in the foreground with `fg PID_num`.

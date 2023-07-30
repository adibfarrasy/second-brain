#software #linux

## Basic Linux Commands

### Files & Navigating

`ls` - directory listing (list all files/ folders on current dir) `ls -l` - formatted listing `ls -la` - formatted listing including hidden files `cd DIRNAME` - change directory to DIRNAME `cd ..` - change to parent directory `cd ../DIRNAME` - change to DIRNAME in parent directory `cd` - change to home directory `pwd` - print working directory `mkdir DIRNAME` - create a directory DIRNAME `rm FILENAME` - delete file FILENAME `rm -f FILENAME` - force delete file FILENAME `rm -r DIRNAME` - delete directory DIRNAME `rm -rf DIRNAME` - force delete directory DIRNAME `cp FILENAME1 FILENAME2` - copy FILENAME1 contents to FILENAME2 `mv FILENAME1 FILENAME2` - rename FILENAME1 to FILENAME2 `mv FILENAME1 DIRNAME/FILENAME2` - move FILENAME1 to DIRNAME/FILENAME2 `touch FILENAME` - create or update file FILENAME `cat FILENAME` - output contents of FILENAME `cat > FILENAME` - write standard input into FILENAME `cat >> FILENAME` - append standard input into FILENAME `tail -f FILENAME` - output contents of FILENAME as it grows

### Networking

`ping HOSTNAME` - ping HOSTNAME e.g. [www.google.com](http://www.google.com/)`dig HOSTNAME` - get connection details about HOSTNAME `wget URL/FILENAME` - download FILENAME from URL `wget -c FILENAME` - continue stopped download of FILENAME `wget -r URL` - recursively download files from URL `curl URL` - outputs the webpage from URL `curl -o test.html URL` - writes the content of URL to test.html `ssh USER@HOSTNAME` - connect to HOSTNAME as USERNAME `ssh -p PORT@HOSTNAME` - connect to HOSTNAME using PORT `ssh -i SECURITYKEY USER@HOSTNAME` - connect to cloud via SSH e.g. AWS

### Processes

`ps` - display currently active processes `ps aux` - display detailed active processes `kill PID` - kill processes with PID tag `killall PROCESSNAME` - kill all processes named PROCESSNAME

### System Info

`date` - show current date/time `uptime` - show how long the system is active `whoami` - show current logged in user `w` - display users in the machine `cat /proc/cpuinfo` - display CPU info `cat /proc/meminfo` - display memory info `free` - show memory and swap usage `du` - show directory space usage `du -sh` - show directory space usage in GB `df` - show disk usage `uname -a` - show kernel config

## Compressing

`tar cf FILENAME.tar FILE1 ... FILEN` - compress FILE1 to FILEN into FILENAME.tar `tar xf FILENAME.tar` - unarchive FILENAME.tar into current directory `tar tf FILENAME.tar` - show contents of the archive

Options: `c` - create archive `t` - table of contents `x` - extract `z` - use zip/gzip `f` - specify filename `j` - bzip2 compression `w` - ask for confirmation `k` - do not overwrite `v` - verbose

### Permissions

`chmod OCTAL FILENAME` - change permission of file FILENAME

- 4 - read(r)
- 2 - write(w)
- 1 - execute(x)

order: owner/group/world

eg. `chmod 777 FILENAME` - rwx for everyone, `chmod 755 FILENAME` - rwx for owner, rx for group and world.

## Miscellaneous

`grep PATTERN FILENAME1 ... FILENAMEN` - search for PATTERN in FILENAME1 to FILENAMEN `grep -r PATTERN DIRNAME` - search for PATTERN recrusively in DIRNAME `whereis APPNAME` - show possible locations for APPNAME `man COMMAND` - show manual page for command

## Advanced Linux Commands

`wall MESSAGE` - broadcast MESSAGE to all the logged in users in the linux system `write USERNAME` - sends a message to one user USERNAME `mesg y` - enable receiving message `mesg n` - disable receiving message `sort/ sort -r` - arrange lines in a file. use -r tag to sort in descending order.
# Linux Command Cheat Sheet

## System Commands

| Command | Description |
|---------|-------------|
| `uname -a` | Display Linux system information |
| `uname -r` | Display kernel information |
| `cat /etc/os-release` | Display OS information like name and version |
| `uptime` | Show how long the system has been running |
| `hostname` | Display the hostname |
| `hostname -I` | Display all local IP addresses |
| `last reboot` | Show reboot history |
| `date` | Display current date and time |
| `cal` | Display calendar of current month |
| `whoami` | Display current login name |
| `history` | Show command history |
| `clear` | Clear terminal |
| `shutdown -h now` | Shutdown the system |
| `reboot` | Restart the system |

## Hardware Commands

| Command | Description |
|---------|-------------|
| `cat /proc/cpuinfo` | Display CPU information |
| `cat /proc/meminfo` | Display memory information |
| `free -h` | Show free and used memory (-h for human readable, -m for MB, -g for GB) |
| `lspci -tv` | Display PCI devices |
| `lsusb -tv` | Display USB devices |
| `lsblk` | Display information about block devices |
| `dmidecode` | Display DMI/SMBIOS (hardware info) from BIOS |
| `hdparm -i /dev/sda` | Display disk sda information |
| `hdparm -tT /dev/sda` | Perform read speed test on disk sda |
| `badblocks -s /dev/sda` | Test for unreadable blocks on disk sda |

## Monitoring Commands

| Command | Description |
|---------|-------------|
| `mpstat 1` | Display processor-related statistics |
| `vmstat 1` | Display virtual memory statistics |
| `iostat 1` | Display I/O statistics |
| `tail -100 /var/log/messages` | Display last 100 system logs |
| `tcpdump -i eth0` | Capture and display all packets on interface eth0 |
| `tcpdump -i eth0 'port 80'` | Monitor all traffic on port 80 (HTTP) |
| `lsof` | List all open files on the system |
| `lsof -u user` | List files opened by user |
| `watch df -h` | Execute and "watch" the `df -h` command |

## User Commands

| Command | Description |
|---------|-------------|
| `id` | Display current user and group IDs |
| `last` | Show last logged in users |
| `who` | Show who is logged into the system |
| `w` | Show who is logged in and what they're doing |
| `groupadd test` | Create a group named "test" |
| `groupdel test` | Delete the group named "test" |
| `useradd -c "This is NhaX" -m nhax` | Create user account "nhax" with comment and home directory |
| `userdel nhax` | Delete user account "nhax" |
| `usermod -aG sales nhax` | Add user "nhax" to group "sales" |
| `usermod [option] username` | Modify user account information (group, home dir, shell, expiry date) |


## File and Directory Commands

| Command | Description |
|---------|-------------|
| `pwd` | Display current working directory |
| `cd` | Change to home directory |
| `cd foldername` | Change to directory "foldername" |
| `cd ..` | Move up one directory level |
| `ls` | List all files and folders in current directory |
| `ls -al` | List all files and folders including hidden ones, with details |
| `mkdir directory` | Create a directory named "directory" |
| `rm file` | Delete file |
| `rm -r directory` | Delete directory and its contents recursively |
| `rm -f file` | Force delete file without confirmation |
| `rm -rf directory` | Force delete directory recursively |
| `cp file1 file2` | Copy file1 to file2 |
| `cp -r source_directory destination` | Copy source_directory to destination |
| `mv file1 file2` | Rename or move file1 to file2 |
| `ln -s /path/to/file linkname` | Create symbolic link to linkname |
| `touch file` | Create an empty file or update file timestamps |
| `cat file` | View file contents |
| `cat file1 file2 > file3` | Combine file1 and file2 and store output in file3 |
| `less file` | Browse through a file |
| `head file` | Display first 10 lines of file |
| `tail file` | Display last 10 lines of file |
| `tail -f file` | Display last 10 lines and "watch" the file for changes |

## Process Commands

| Command | Description |
|---------|-------------|
| `ps` | Display currently running processes |
| `pstree` | Display processes in a tree-like diagram |
| `ps -ef` | Display all running processes on the system |
| `ps -ef \| grep processname` | Display process named "processname" |
| `top` | Display top processes |
| `htop` | Interactive process viewer (more visual than top) |
| `kill pid` | Kill process with process ID pid |
| `killall processname` | Kill all processes named "processname" |
| `program &` | Run program in background |
| `bg` | List stopped or background jobs |
| `fg` | Bring most recent background job to foreground |
| `fg n` | Bring job n to foreground |
| `lsof` | List open files |
| `pidof processname` | Get PID of any running process |

## File Permissions Commands

| Command | Description |
|---------|-------------|
| `ls -l filename` | Check current permissions of file |
| `chmod 777 filename` | Set full permissions (read, write, execute) for everyone |
| `chmod -R 777 dirname` | Set full permissions for directory and all subdirectories |
| `chmod -x filename` | Remove execute permission from file |
| `chown username filename` | Change file ownership |
| `chown user:group filename` | Change file owner and group ownership |
| `chown -R user:group dirname` | Change ownership of directory and all subdirectories |

## Networking Commands

| Command | Description |
|---------|-------------|
| `ip a` | Display network interfaces and IP addresses |
| `ip addr show dev eth0` | Show eth0 address and details |
| `ip addr add IP-Address dev eth1` | Add temporary IP address to eth1 interface |
| `ethtool eth0` | Query or control network driver and hardware settings |
| `ping host` | Send ICMP echo request to host |
| `whois domain` | Display whois information for domain |
| `dig domain` | Display DNS information for domain |
| `dig -x IP_ADDRESS` | Reverse lookup of IP_ADDRESS |
| `host domain` | Display IP address for domain |
| `hostname -i` | Display network address of hostname |
| `hostname -I` | Display all local IP addresses |
| `wget http://domain.com/file` | Download file |
| `netstat -nutlp` | Display listening tcp and udp ports and corresponding programs |

## Archive Commands (TAR Files)

| Command | Description |
|---------|-------------|
| `tar -cvf filename.tar filename` | Create tar archive |
| `tar -xvf filename.tar` | Extract tar archive |
| `tar -tvf filename.tar` | List contents of tar archive |
| `tar -xvf filename.tar file1.txt` | Extract single file from tar archive |
| `tar -rvf filename.tar file2.txt` | Add file to tar archive |
| `zip filename.zip filename` | Create zip archive |
| `zip filename.zip file1.txt file2.txt file3.txt` | Create zip archive with multiple files |
| `zip -u filename.zip file4.txt` | Add file to zip archive |
| `zip -d filename.zip file4.txt` | Remove file from zip archive |
| `unzip -l filename.zip` | List contents of zip archive |
| `unzip filename.zip` | Extract zip archive |
| `unzip filename.zip -d /dirname` | Extract zip archive to specific directory |

## Installing Packages

| Command | Description |
|---------|-------------|
| `apt-get install packagename` | Install package on Debian-based distributions |
| `apt-get remove packagename` | Remove package on Debian-based distributions |
| `dpkg -l \| grep -i installed` | Get list of all installed packages on Debian-based distribution |
| `dpkg -i packagename.deb` | Install .deb package |
| `apt-get update` | Update repository on Debian-based distributions |
| `apt-get upgrade packagename` | Upgrade specific package on Debian-based distributions |
| `apt-get autoremove` | Remove all unwanted packages on Debian-based distributions |
| `yum install packagename` | Install package on RPM-based distributions |
| `yum remove packagename` | Remove package on RPM-based distributions |
| `yum update` | Update all system packages to latest version on RPM-based distributions |
| `yum list --installed` | List all installed packages on RPM-based distributions |
| `yum list --available` | List all available packages on RPM-based distributions |

## Search Commands

| Command | Description |
|---------|-------------|
| `grep pattern file` | Search for "pattern" in file |
| `grep -r pattern directory` | Search recursively for "pattern" in directory |
| `locate name` | Find files and directories by name |
| `find /home/john -name 'prefix*'` | Find files in /home/john starting with "prefix" |
| `find /home -size +100M` | Find files larger than 100MB in /home |

## SSH Logins

| Command | Description |
|---------|-------------|
| `ssh host` | Connect to host as your local username |
| `ssh user@host` | Connect to host as user |
| `ssh -p port user@host` | Connect to host using port |

## File Transfers

| Command | Description |
|---------|-------------|
| `scp file.txt server:/tmp` | Securely copy file.txt to the /tmp directory on the server |
| `scp server:/var/www/*.html /tmp` | Copy *.html files from server to local /tmp directory |
| `scp -r server:/var/www /tmp` | Recursively copy all files and directories from server to local /tmp directory |
| `rsync -a /home /backups/` | Synchronize /home to /backups/home |
| `rsync -avz /home server:/backups/` | Synchronize files/directories between local and remote system with compression |

## Disk Usage

| Command | Description |
|---------|-------------|
| `df -h` | Display free and used space on mounted filesystems |
| `df -i` | Display free and used inodes on mounted filesystems |
| `fdisk -l` | Display disk partitions, sizes, and types |
| `du -ah` | Display disk usage for all files and directories in human-readable format |
| `du -sh` | Display total disk usage of current directory |

## Security Commands

| Command | Description |
|---------|-------------|
| `passwd` | Change password of current user |
| `sudo -i` | Switch to root account with root's environment |
| `sudo -s` | Execute your current shell with root privileges |
| `sudo -l` | List sudo privileges for current user |
| `visudo` | Edit the sudoers configuration file |
| `getenforce` | Display current SELinux mode |
| `sestatus` | Display SELinux status and configuration |
| `setenforce 0` | Change current SELinux mode to Permissive (does not persist on reboot) |
| `setenforce 1` | Change current SELinux mode to Enforcing (does not persist on reboot) |
| `SELINUX=enforcing` | Set SELinux mode to enforcing on boot in /etc/selinux/config |
| `SELINUX=permissive` | Set SELinux mode to permissive on boot in /etc/selinux/config |
| `SELINUX=disabled` | Set SELinux mode to disabled on boot in /etc/selinux/config |
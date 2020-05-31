# Lsof (list open files)

lsof is a commandline utility, which is used in many Unix-like systems to report a list of all open files and the processes that opened them.

## List of file descriptors

FD   | Description |
:----|:------------|
**cwd** | current working directory |
**L**nn | library references (AIX) |
**err** | FD information error |
**jld** | jail directory (FreeBSD) |
**ltx** | shared library text (code and data) |
**M**xx | hex memory-mapped type number xx |
**m86** | DOS Merge mapped file |
**mem** | memory-mapped file |
**mmap** | memory-mapped device |
**pd** | parent directory |
**rtd** | root directory |
**tr** | kernel trace file (OpenBSD) |
**txt** | program text (code and data) |
**v86** | VP/ix mapped file |

## List of node types associated with files

TYPE | Description |
:----|:------------|
**IPv4** | IPv4 socket |
**IPv6** | open IPv6 network file - even if its address is IPv4, mapped in an IPv6 address |
**ax25** | Linux AX.25 socket |
**inet** | Internet domain socket |
**lla** | HP-UX link level access file |
**rte** | AF_ROUTE socket |
**sock** | socket of unknown domain |
**unix** | UNIX domain socket |
**x.25** | HP-UX x.25 socket |
**BLK** | block special file |
**CHR** | character special file |
**DEL** | Linux map file that has been deleted |
**DIR** | directory |
**DOOR** | VDOOR file |
**FIFO** | FIFO special file |
**KQUEUE** | BSD style kernel event queue file |
**LINK** | symbolic link file |
**MPB** | multiplexed block file |
**MPC** | multiplexed character file |
**NOFD** | Linux /proc/<PID>/fd directory that can't be opened -- the directory path appears in the NAME column, followed by an error message |
**PAS** | /proc/as file |
**PAXV** | /proc/auxv file |
**PCRE** | /proc/cred file |
**PCTL** | /proc control file |
**PCUR** | for the current /proc process |
**PCWD** | /proc current working directory |
**PDIR** | /proc directory |
**PETY** | /proc executable type (etype) |
**PFD** | /proc file descriptor |
**PFDR** | /proc file descriptor directory |
**PFIL** | executable /proc file |
**PFPR** | /proc FP register set |
**PGD** | /proc/pagedata file |
**PGID** | /proc group notifier file |
**PIPE** | for pipes |
**PLC** | /proc/lwpctl file |
**PLDR** | /proc/lpw directory |
**PLDT** | /proc/ldt file |
**PLPI** | /proc/lpsinfo file |
**PLST** | /proc/lstatus file |
**PLU** | /proc/lusage file |
**PLWG** | /proc/gwindows file |
**PLWI** | /proc/lwpsinfo file |
**PLWS** | /proc/lwpstatus file |
**PLWU** | /proc/lwpusage file |
**PLWX** | /proc/xregs file |
**PMAP** | /proc map file (map) |
**PMEM** | /proc memory image file |
**PNTF** | /proc process notifier file |
**POBJ** | /proc/object file |
**PODR** | /proc/object directory |
**POLP** | old format /proc light weight process file |
**POPF** | old format /proc PID file |
**POPG** | old format /proc page data file |
**PORT** | SYSV named pipe |
**PREG** | /proc register file |
**PRMP** | /proc/rmap file |
**PRTD** | /proc root directory |
**PSGA** | /proc/sigact file |
**PSIN** | /proc/psinfo file |
**PSTA** | /proc status file |
**PSXSEM** | POSIX semaphore file |
**PSXSHM** | POSIX shared memory file |
**PTS** | /dev/pts file |
**PUSG** | /proc/usage file |
**PW** | /proc/watch file |
**PXMP** | /proc/xmap file |
**REG** | regular file |
**SMT** | shared memory transport file |
**STSO** | stream socket |
**UNNM** | unnamed type file |
**XNAM** | OpenServer Xenix special file of unknown type |
**XSEM** | OpenServer Xenix semaphore file |
**XSD** | OpenServer Xenix shared data file |

## List all open files of your system

```
$ lsof

COMMAND PID USER   FD   TYPE DEVICE SIZE/OFF    NODE NAME
bash      1 root  cwd    DIR  0,113     4096 1180219 /opt
bash      1 root  rtd    DIR  0,113     4096 1837831 /
bash      1 root  txt    REG  0,113  1183448 1180249 /usr/bin/bash
bash      1 root  mem    REG  254,1          1180249 /usr/bin/bash (path dev=0,113)
bash      1 root  mem    REG  254,1          1181031 /usr/lib/x86_64-linux-gnu/libnss_files-2.31.so (path dev=0,113)

~~~ truncated ~~~
```

## List open files of specific file system

```
$ lsof /proc
COMMAND PID USER   FD   TYPE DEVICE SIZE/OFF   NODE NAME
lsof    526 root    3r   DIR  0,115        0      1 /proc
lsof    526 root    6r   DIR  0,115        0 144100 /proc/526/fd
```

## List of open files for specific user

```
$ lsof -u root
COMMAND PID USER   FD   TYPE DEVICE SIZE/OFF    NODE NAME
bash      1 root  cwd    DIR  0,113     4096 1180219 /opt
bash      1 root  rtd    DIR  0,113     4096 1837831 /
bash      1 root  txt    REG  0,113  1183448 1180249 /usr/bin/bash
bash      1 root  mem    REG  254,1          1180249 /usr/bin/bash (path dev=0,113)

~~~ truncated ~~~
```

## List of open files for all except a specific user

```
$ lsof -u ^root
```

## List all open Internet and UNIX domain files

```
$ lsof -i -U
```

## List all open IPv4 network files

```
$ lsof -i 4
```

## List all open network files for IPv6

```
$ lsof -i 6
```

## List all TCP & UDP process running on specific port

```
$ lsof -i TCP:8888
$ lsof -i TCP:1-1048
$ lsof -i UDP:12342
$ lsof -i :22
```

## Show only TCP connections

```
$ lsof -iTCP
```

## List all open files for specific device

```
$ lsof  /dev/sda
```

## List processes with open files on NFS file system

```
$ lsof -b <nfs-mount-point>
```

## List terminal related open files

```
$ lsof /dev/tty1
```

## List all the network connections

```
$ lsof -i
```

## Show connections to a specific host and port

```
$ lsof -i@192.168.1.104
$ lsof -i@[::1]
$ lsof -i@192.168.1.104:22
$ lsof -i @www.example.com:80-1024
```

## Find listening port

```
$ lsof -i -sTCP:LISTEN
```

## Find established connection

```
$ lsof -i -sTCP:ESTABLISHED
```

## List all Process or Commands that belongs to a Process ID

```
$ lsof -p <pid>
```

## Kill all process that belongs to a specific user

```
$ kill -9 `lsof -t -u <username>`
```

## List all open files under a specific directory

```
$ lsof +D /var/log/
```

## Find who opened a file

```
$ lsof -t /var/log/messages
$ ps -fp "$(lsof -t /var/log/messages | xargs echo)"
```

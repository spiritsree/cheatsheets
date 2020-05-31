# Strace (Linux Syscall Tracer)

Strace is a diagnostic, debugging and instructional userspace utility for Linux. It is used to monitor and tamper with interactions between processes and the Linux kernel, which include system calls, signal deliveries, and changes of process state.

## Strace Options

* **-c** – See what time is spend and where (combine with -S for sorting)
* **-f** – Track process including forked child processes
* **-o my-process-trace.txt** – Log strace output to a file
* **-p 1234** – Track a process by PID
* **-P /tmp** – Track a process when interacting with a path
* **-T** – Display syscall duration in the output
* **-t** - Show time of the day of syscall

Strace by specific syscall group

* **-e trace=ipc** – Track communication between processes (IPC)
* **-e trace=memory** – Track memory syscalls
* **-e trace=network** – Track memory syscalls
* **-e trace=process** – Track process calls (like fork, exec)
* **-e trace=signal** – Track process signal handling (like HUP, exit)
* **-e trace=file** – Track file related syscalls

## Attach to a running process

```
$ strace -p 14675
strace: Process 14675 attached
...
```

## Run a command with strace

```
$ strace ls

execve("/usr/bin/ls", ["ls"], 0x7ffdb79d2a90 /* 8 vars */) = 0
brk(NULL)                               = 0x55ca9e2da000
arch_prctl(0x3001 /* ARCH_??? */, 0x7ffc11777d70) = -1 EINVAL (Invalid argument)
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=6530, ...}) = 0
mmap(NULL, 6530, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7fc49f78c000
close(3)                                = 0
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libselinux.so.1", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0@p\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=163200, ...}) = 0

~~~ truncated ~~~
```

## Count number of syscalls made by a command

```
$ strace -c ls

% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ----------------
 30.26    0.000915          35        26           mmap
 10.78    0.000326          40         8           openat
 10.22    0.000309          34         9           mprotect
 10.02    0.000303          43         7           read
  9.26    0.000280          28        10           close
  6.94    0.000210          26         8           pread64
  6.45    0.000195          21         9           fstat
  3.51    0.000106          53         2           getdents64
  2.25    0.000068          68         1           set_robust_list
  2.15    0.000065          32         2         2 statfs
  1.16    0.000035          11         3           brk
  1.16    0.000035          17         2           ioctl
  1.09    0.000033          33         1           munmap
  0.99    0.000030          15         2         2 access
  0.86    0.000026          13         2         1 arch_prctl
  0.86    0.000026          26         1           set_tid_address
  0.86    0.000026          26         1           prlimit64
  0.63    0.000019          19         1           write
  0.36    0.000011           5         2           rt_sigaction
  0.20    0.000006           6         1           rt_sigprocmask
  0.00    0.000000           0         1           execve
------ ----------- ----------- --------- --------- ----------------
100.00    0.003024                    99         5 total
```

## Trace only syscalls accessing a path

```
$ strace -P /etc/ld.so.cache ls
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=6530, ...}) = 0
mmap(NULL, 6530, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7fb690310000
close(3)                                = 0

+++ exited with 0 +++
```

## Filter by type of syscall

```
-e trace=%desc     Trace all file descriptor related system calls.
         %file     Trace all system calls which take a file name as an argument.
         %fstat    Trace fstat and fstatat syscall variants.
         %fstatfs  Trace fstatfs, fstatfs64, fstatvfs, osf_fstatfs, and osf_fstatfs64 system calls.
         %ipc      Trace all IPC related system calls.
         %lstat    Trace lstat syscall variants.
         %memory   Trace all memory mapping related system calls.
         %network  Trace all the network related system calls.
         %process  Trace all system calls which involve process management.
         %pure     Trace syscalls that always succeed and have no arguments.
         %signal   Trace all signal related system calls.
         %stat     Trace stat syscall variants.
         %statfs   Trace statfs, statfs64, statvfs, osf_statfs, and osf_statfs64 system calls.
         %%stat    Trace syscalls used for requesting file status.
         %%statfs  Trace syscalls related to file system statistics.
```

## Strace process including forked processes

```
$ strace -f -p 14675
```

## Perform a syscall fault injection

```
$ strace -e trace=open -e fault=open cat
open("/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = -1 ENOSYS (Function not implemented) (INJECTED)
open("/lib/x86_64-linux-gnu/tls/x86_64/libc.so.6", O_RDONLY|O_CLOEXEC) = -1 ENOSYS (Function not implemented) (INJECTED)
open("/lib/x86_64-linux-gnu/tls/libc.so.6", O_RDONLY|O_CLOEXEC) = -1 ENOSYS (Function not implemented) (INJECTED)
open("/lib/x86_64-linux-gnu/x86_64/libc.so.6", O_RDONLY|O_CLOEXEC) = -1 ENOSYS (Function not implemented) (INJECTED)
open("/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = -1 ENOSYS (Function not implemented) (INJECTED)
cat: error while loading shared libraries: libc.so.6: cannot open shared object file: Error 38
+++ exited with 127 +++
```

## Show instruction pointer during syscall

```
$ strace -i ls
[00007fe202d7316b] execve("/usr/bin/ls", ["ls"], 0x7fffe034eac8 /* 9 vars */) = 0
[00007f4b0483feab] brk(NULL)            = 0x558355b87000
[00007f4b0483eb55] arch_prctl(0x3001 /* ARCH_??? */, 0x7ffc57c86870) = -1 EINVAL (Invalid argument)
[00007f4b04840d5b] access("/etc/ld.so.preload", R_OK) = -1 ENOENT (No such file or directory)
[00007f4b04840ec8] openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
[00007f4b04840c99] fstat(3, {st_mode=S_IFREG|0644, st_size=6600, ...}) = 0
[00007f4b048410e6] mmap(NULL, 6600, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f4b04820000
[00007f4b04840d8b] close(3)             = 0

~~~ truncated ~~~
```

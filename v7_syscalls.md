X V7 System Calls
http://www.faqs.org/faqs/unix-faq/faq/part6/section-2.html
https://groups.google.com/forum/#!topic/comp.os.minix/B2I_cxskEsU
http://www.di.uevora.pt/~lmr/syscalls.html

## `access`
Checks the permissions of a file before the calling process can access the file.  Ensures the specified file exists, then checks if the process/user may read, write, and/or execute the file.

Headers:
* unistd.h
* fcntl.h - for `AT_*` constants

### `access(const char *path, int mode)`
Checks the accessibility of the file at `path` for the access permissions indicated by `mode`.  The check is done using the calling process's real UID and GID rather than effective IDs (as would be done attempting an operation like `open`).  For non-root users, an empty set of capabilities is used.

This allows set-user-ID programs and capability-endowed programs to easily determine the invoking user's authority. In other words, access() does not answer the "can I read/write/execute this file?" question. It answers a slightly different question: "(assuming I'm a setuid binary) can the user who invoked me read/write/execute this file?", which gives set-user-ID programs the possibility to prevent malicious users from causing them to read files which users shouldn't be able to read.

If the calling process is privileged (i.e., its real UID is zero), then an X_OK check is successful for a regular file if execute  permission is enabled for any of the file owner, group, or other.

If `path` is relative, it is assumed to be located relative to the current working directory.  Symlinks are dereferenced.

`mode` can be either:
* `F_OK` - tests for existence
* A bitwise-inclusive OR of:
    * `R_OK` - read permission
    * `W_OK` - write permission
    * `X_OK` - execute/search permission

### `faccessat(int fd, const char *path, int mode, int flag)`
* If `path` is relative, it is assumed to be located relative to directory associated with the file descriptor `fd`
* If `path` is an absolute path, `fd` is ignored
* If `path` is relative and `fd` is the special value `AT_FDCWD`, it is assumed to be located relative to the current working directory (same as `access`)

`flag` is an OR of one of the following:
* `AT_EACCESS` - perform access checks using effective UID and GID
* `AT_SYMLINK_NOFOLLOW` (Linux only?) - do not dereference symlink, instead return information about it

## `acct`
Turns process accounting (record keeping of executed commands) on or off, allowing admins to be aware of what commands were executed and by whom.  Permitted only to superuser.

Headers:
* unistd.h

### `acct(const char *filepath)`
`filename`
* For an existing file path, record collection is enabled for every process initiated which terminates under normal conditions, for which an accounting record is appended to the file (non-terminating processes are not accounted for)
* For null, record collection is disabled

## `alarm`

## `brk`

## `chdir`

## `chmod`

## `chown`

## `chroot`

## `close`

## `creat`

## `dup`

## `* dup2`

## `exec*`
includes exec (obs?) execl, execv, execle, execve

## `exit` and `_exit`

## `fork`

## `fstat`

## `ftime`

## `getgid` and `getegid`

## `getpid`

## `getuid` and `geteuid`

## `gtty`
obsolete; invokes ioctl internally

## `indir`
silly kludge probably unnecessary on non-11 systems

## `ioctl`

## `kill`

## `link`

## `lock`
lock process into memory

## `lseek`

## `mknod`
makes directories and devices

## `mount`

## `mpxcall`
(mpxchan?) you don't want to know... not in all V7s

## `nice`

## `open`

## `pause`

## `phys`
map memory into user space

## `pipe`

## `pkoff`

## `pkon`

## `profil`

## `ptrace`

## `read`

## `sbrk`

## `setgid`

## `setuid`

## `signal`

## `stat`

## `stime`

## `stty`
obsolete; invokes ioctl internally

## `sync`

## `tell`

## `time`

## `times`

## `umask`

## `umount`

## `unlink`

## `utime`

## `wait`

## `write`

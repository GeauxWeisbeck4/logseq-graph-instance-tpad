tags:: linux, virtual-machines, c-lang, hacking

- # Intro
	- Virtual memory - idealized abstraction of the storage resources that are actually available on a given machine
	- Virtual Machine
		- An operating system maps a bunch of memory addresses (virtual addresses) used by a process/program into physical addresses.
		- Main storage, as seen by a process or task, is seen as a contiguous address space or collection of contiguous segments
	- The OS manages the assignment of real memory to virtual memory space
	- Benefits include:
		- Freeing applications from having to use a shared memory  space
		- Increased security due to memory isolation
		- Conceptually being able to use more memory than is actually physically available on computer due to the technique of paging
	- ## Key Points
		- Each process has its own virtual memory
		- The amount of virtual memory depends on your system's architecture
		- Most OS virtual memory handling process looks like:
			- ![virtual_memory.png](../assets/virtual_memory_1743518013319_0.png)
		- High memory address
			- Command line arguments and environment variables
			- The stack growing downwards
		- Lower memory addresses
			- "Executable" - a little more complicated but good enough for now
			- Heap growing upwards
				- Heap is a portion of memory that is dynamically allocated using `malloc`
	- **Virtual memory is not the same as RAM**
- # Simple C Program
	- Writing a simple c program here:
	- ```c
	  #include <stdlib.h>
	  #include <stdio.h>
	  #include <string.h>
	  
	  /**
	   * main - uses strdup to create a new string, and prints the
	   * address of the new duplcated string
	   *
	   * Return: EXIT_FAILURE if malloc failed. Otherwise EXIT_SUCCESS
	   */
	  int main(void)
	  {
	      char *s;
	  
	      s = strdup("Holberton");
	      if (s == NULL)
	      {
	          fprintf(stderr, "Can't allocate mem with malloc\n");
	          return (EXIT_FAILURE);
	      }
	      printf("%p\n", (void *)s);
	      return (EXIT_SUCCESS);
	  }
	  ```
	- ## `strdup`
		- How does `strdup` create copy of string "Holberton"?
			- It has to first reserve space since it is creating a new string - probably using `malloc`
			- We can confirm in the manual page:
				- ```
				  DESCRIPTION
				         The  strdup()  function returns a pointer to a new string which is a duplicate of the string s.
				         Memory for the new string is obtained with malloc(3), and can be freed with free(3).
				  ```
			- So will this operation be in a low or high virtual memory address? low obvious. since it needs to be dynamically allocated - let's say the heap and test our theory by compiling our program
				- ```
				  $ gcc -Wall -Wextra -pedantic -Werror main.c -o holberton
				  $ ./holberton
				  // 0x55d327a432a0
				  ```
			- We get our duplicated string address of `0x55d327a432a0` - how do we know if this is low or high virtual memory address?
- # How big is the virtual machine?
	- Depends on your computer architecture (64-bit)
		- So theoretically the size of each process' virtual memory is 2^64 bytes
			- Theoretically, the highest memory address possible is `0xffffffffffffffff` (1.8446744e+19), and the lowest is `0x0`
		- `0x55d327a43` is small compared to `0xffffffffffffffff` so it is probably a lower memory address
			- We will confirm this when we look at the `proc` file system
- # The proc filesystem
	- ```
	  The proc filesystem is a pseudo-filesystem which provides an interface to kernel data structures.  It is commonly mounted at `/proc`.  Most of it is read-only, but some files allow kernel variables to be changed.
	  ```
	- If you list the contents of the `/proc` directory, there will be a lot of files. We'll focus on:
		- `/proc/[pid]/mem`
		- `/proc/[pid]/maps`
- # mem
	- ```
	  /proc/[pid]/mem
	                This file can be used to access the pages of a process's memory
	            through open(2), read(2), and lseek(2).
	  ```
		- Can we access and modify the entire virtual memory of any process?
- # maps
	- ```
	  /proc/[pid]/maps
	                A  file containing the currently mapped memory regions and their access permissions.
	            See mmap(2) for some further information about memory mappings.
	  
	                The format of the file is:
	  
	         address           perms offset  dev   inode       pathname
	         00400000-00452000 r-xp 00000000 08:02 173521      /usr/bin/dbus-daemon
	         00651000-00652000 r--p 00051000 08:02 173521      /usr/bin/dbus-daemon
	         00652000-00655000 rw-p 00052000 08:02 173521      /usr/bin/dbus-daemon
	         00e03000-00e24000 rw-p 00000000 00:00 0           [heap]
	         00e24000-011f7000 rw-p 00000000 00:00 0           [heap]
	         ...
	         35b1800000-35b1820000 r-xp 00000000 08:02 135522  /usr/lib64/ld-2.15.so
	         35b1a1f000-35b1a20000 r--p 0001f000 08:02 135522  /usr/lib64/ld-2.15.so
	         35b1a20000-35b1a21000 rw-p 00020000 08:02 135522  /usr/lib64/ld-2.15.so
	         35b1a21000-35b1a22000 rw-p 00000000 00:00 0
	         35b1c00000-35b1dac000 r-xp 00000000 08:02 135870  /usr/lib64/libc-2.15.so
	         35b1dac000-35b1fac000 ---p 001ac000 08:02 135870  /usr/lib64/libc-2.15.so
	         35b1fac000-35b1fb0000 r--p 001ac000 08:02 135870  /usr/lib64/libc-2.15.so
	         35b1fb0000-35b1fb2000 rw-p 001b0000 08:02 135870  /usr/lib64/libc-2.15.so
	         ...
	         f2c6ff8c000-7f2c7078c000 rw-p 00000000 00:00 0    [stack:986]
	         ...
	         7fffb2c0d000-7fffb2c2e000 rw-p 00000000 00:00 0   [stack]
	         7fffb2d48000-7fffb2d49000 r-xp 00000000 00:00 0   [vdso]
	  
	                The address field is the address space in the process that the mapping occupies.
	            The perms field is a set of permissions:
	  
	                     r = read
	                     w = write
	                     x = execute
	                     s = shared
	                     p = private (copy on write)
	  
	                The offset field is the offset into the file/whatever;
	            dev is the device (major:minor); inode is the inode on that device.   0  indicates
	                that no inode is associated with the memory region,
	            as would be the case with BSS (uninitialized data).
	  
	                The  pathname field will usually be the file that is backing the mapping.
	            For ELF files, you can easily coordinate with the offset field
	                by looking at the Offset field in the ELF program headers (readelf -l).
	  
	                There are additional helpful pseudo-paths:
	  
	                     [stack]
	                            The initial process's (also known as the main thread's) stack.
	  
	                     [stack:<tid>] (since Linux 3.4)
	                            A thread's stack (where the <tid> is a thread ID).
	                It corresponds to the /proc/[pid]/task/[tid]/ path.
	  
	                     [vdso] The virtual dynamically linked shared object.
	  
	                     [heap] The process's heap.
	  
	                If the pathname field is blank, this is an anonymous mapping as obtained via the mmap(2) function.
	            There is no easy  way  to  coordinate
	                this back to a process's source, short of running it through gdb(1), strace(1), or similar.
	  
	                Under Linux 2.0 there is no field giving pathname.
	  ```
	- This means that we can look at the `/proc/[pid]/mem` file to locate the heap of a running process. If we can read from the heap, we can locate the string we want to modify. And if we can write to the heap, we can replace this string with whatever we want.
- # pid
	- a process is an instance of a program with a unique process ID. This process ID used by many functions and system calls to interact with and manipulate processes
		- Use `ps` to get PID of running processes
- # C program
	- We now have everything we need to write a script or program that finds a string in the heap of a running process and then replaces it with another string (of the same length or shorter). We will work with the following simple program that infinitely loops and prints a “strduplicated” string.
		- ```c
		  #include <stdlib.h>
		  #include <stdio.h>
		  #include <string.h>
		  #include <unistd.h>
		  
		  /**              
		   * main - uses strdup to create a new string, loops forever-ever
		   *                
		   * Return: EXIT_FAILURE if malloc failed. Other never returns
		   */
		  int main(void)
		  {
		       char *s;
		       unsigned long int i;
		  
		       s = strdup("Holberton");
		       if (s == NULL)
		       {
		            fprintf(stderr, "Can't allocate mem with malloc\n");
		            return (EXIT_FAILURE);
		       }
		       i = 0;
		       while (s)
		       {
		            printf("[%lu] %s (%p)\n", i, s, (void *)s);
		            sleep(1);
		            i++;
		       }
		       return (EXIT_SUCCESS);
		  }
		  ```
		- When we compile it and run it, we notice it runs forever
		- How would we write a script that finds a string in a heap of a running process?
			- We need to find the PID
				- ```
				  geauxweisbeck4@DESKTOP-U0OP3L5:~/guides/hacking-virtual-machine/part-one$ ps aux | grep ./loop | grep -v grep
				  geauxwe+  193703  0.0  0.0   2680  1048 pts/5    S+   11:04   0:00 ./loop
				  ```
			- Our PID is 193703 -> `maps` and `mems` we want are in the `/proc/193703` directory
				- `/proc/193703/maps`
				- `/proc/193703/mem`
			- Run `ls -la` in the directory
				- ```
				  geauxweisbeck4@DESKTOP-U0OP3L5:/proc/193703$ ls -la
				  total 0
				  dr-xr-xr-x   9 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:04 .
				  dr-xr-xr-x 375 root           root           0 Mar 30 13:58 ..
				  -r--r--r--   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 arch_status
				  dr-xr-xr-x   2 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 attr
				  -r--------   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 auxv
				  -r--r--r--   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 cgroup
				  --w-------   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 clear_refs
				  -r--r--r--   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:04 cmdline
				  -rw-r--r--   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 comm
				  -rw-r--r--   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 coredump_filter
				  -r--r--r--   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 cpuset
				  lrwxrwxrwx   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:04 cwd -> /home/geauxweisbeck4/guides/hacking-virtual-machine/part-one
				  -r--------   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:04 environ
				  lrwxrwxrwx   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 exe -> /home/geauxweisbeck4/guides/hacking-virtual-machine/part-one/loop
				  dr-x------   2 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:04 fd
				  dr-xr-xr-x   2 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 fdinfo
				  -rw-r--r--   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 gid_map
				  -r--------   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 io
				  -r--r--r--   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 limits
				  -rw-r--r--   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 loginuid
				  dr-x------   2 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 map_files
				  -r--r--r--   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 maps
				  -rw-------   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 mem
				  -r--r--r--   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 mountinfo
				  -r--r--r--   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 mounts
				  -r--------   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 mountstats
				  dr-xr-xr-x  65 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 net
				  dr-x--x--x   2 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 ns
				  -rw-r--r--   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 oom_adj
				  -r--r--r--   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 oom_score
				  -rw-r--r--   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 oom_score_adj
				  -r--------   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 pagemap
				  -r--------   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 personality
				  -rw-r--r--   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 projid_map
				  lrwxrwxrwx   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 root -> /
				  -rw-r--r--   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 sched
				  -r--r--r--   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 schedstat
				  -r--r--r--   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 sessionid
				  -rw-r--r--   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 setgroups
				  -r--r--r--   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 smaps
				  -r--r--r--   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 smaps_rollup
				  -r--------   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 stack
				  -r--r--r--   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:04 stat
				  -r--r--r--   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 statm
				  -r--r--r--   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:04 status
				  -r--------   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 syscall
				  dr-xr-xr-x   3 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 task
				  -rw-r--r--   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 timens_offsets
				  -r--r--r--   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 timers
				  -rw-rw-rw-   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 timerslack_ns
				  -rw-r--r--   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:09 uid_map
				  -r--r--r--   1 geauxweisbeck4 geauxweisbeck4 0 Apr  1 11:04 wchan
				  ```
- # `/proc/pid/maps`
	- Print out the file of our process:
		- ```
		  geauxweisbeck4@DESKTOP-U0OP3L5:/proc/193703$ cat maps
		  560baa234000-560baa235000 r--p 00000000 08:20 186896                     /home/geauxweisbeck4/guides/hacking-virtual-machine/part-one/loop
		  560baa235000-560baa236000 r-xp 00001000 08:20 186896                     /home/geauxweisbeck4/guides/hacking-virtual-machine/part-one/loop
		  560baa236000-560baa237000 r--p 00002000 08:20 186896                     /home/geauxweisbeck4/guides/hacking-virtual-machine/part-one/loop
		  560baa237000-560baa238000 r--p 00002000 08:20 186896                     /home/geauxweisbeck4/guides/hacking-virtual-machine/part-one/loop
		  560baa238000-560baa239000 rw-p 00003000 08:20 186896                     /home/geauxweisbeck4/guides/hacking-virtual-machine/part-one/loop
		  560bd500a000-560bd502b000 rw-p 00000000 00:00 0                          [heap]
		  7f41ae9c1000-7f41ae9c4000 rw-p 00000000 00:00 0
		  7f41ae9c4000-7f41ae9ec000 r--p 00000000 08:20 51803                      /usr/lib/x86_64-linux-gnu/libc.so.6
		  7f41ae9ec000-7f41aeb74000 r-xp 00028000 08:20 51803                      /usr/lib/x86_64-linux-gnu/libc.so.6
		  7f41aeb74000-7f41aebc3000 r--p 001b0000 08:20 51803                      /usr/lib/x86_64-linux-gnu/libc.so.6
		  7f41aebc3000-7f41aebc7000 r--p 001fe000 08:20 51803                      /usr/lib/x86_64-linux-gnu/libc.so.6
		  7f41aebc7000-7f41aebc9000 rw-p 00202000 08:20 51803                      /usr/lib/x86_64-linux-gnu/libc.so.6
		  7f41aebc9000-7f41aebd6000 rw-p 00000000 00:00 0
		  7f41aebe3000-7f41aebe5000 rw-p 00000000 00:00 0
		  7f41aebe5000-7f41aebe6000 r--p 00000000 08:20 51800                      /usr/lib/x86_64-linux-gnu/ld-linux-x86-64.so.2
		  7f41aebe6000-7f41aec11000 r-xp 00001000 08:20 51800                      /usr/lib/x86_64-linux-gnu/ld-linux-x86-64.so.2
		  7f41aec11000-7f41aec1b000 r--p 0002c000 08:20 51800                      /usr/lib/x86_64-linux-gnu/ld-linux-x86-64.so.2
		  7f41aec1b000-7f41aec1d000 r--p 00036000 08:20 51800                      /usr/lib/x86_64-linux-gnu/ld-linux-x86-64.so.2
		  7f41aec1d000-7f41aec1f000 rw-p 00038000 08:20 51800                      /usr/lib/x86_64-linux-gnu/ld-linux-x86-64.so.2
		  7ffff4b87000-7ffff4ba8000 rw-p 00000000 00:00 0                          [stack]
		  7ffff4be7000-7ffff4beb000 r--p 00000000 00:00 0                          [vvar]
		  7ffff4beb000-7ffff4bed000 r-xp 00000000 00:00 0                          [vdso]
		  ```
		- Circling back to what we said earlier, we can see that the stack (`[stack]`) is located in high memory addresses and the heap (`[heap]`) in the lower memory addresses.
	- # [heap]
		- Using the map file we cand find the information needs to locate our string:
			- ```
			  560bd500a000-560bd502b000 rw-p 00000000 00:00 0                          [heap]
			  ```
		- The heap
			- Starts at address `560bd500a000` in the virtual memory process
			- Ends at memory address `560bd502b000`
			  id:: 67ec0266-f75b-46fa-8e35-751cdddb3dc6
			- Is readable and writable (rw)
		- If we look at our still running process
			- ```
			  [569] Holberton (0x560bd500a2a0)
			  ```
	- -> `0x560bd500a2a0` < `0x560bd500a2a0` <
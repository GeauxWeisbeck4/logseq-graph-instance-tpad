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
			- Our PID is 193703 -> `maps` and `mems` we want are in the `/proc/193`
-
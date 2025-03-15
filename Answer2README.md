CPS321 - 2025 Spring - Assignment #2 
DUE DATE: Mar. 14 (Fri.) no later than 11:59 PM EST via email 
REQUIREMENTS 
Each group will hand in only one submission. 
Print names of all members are required in submissions. 
The submission file should be titles: CPS321_2025_Spring_[HW NUMBER]_[PRINT NAMES].zip/others . 
All submissions will be sent to Fanchao (fmeng@misericordia.edu) via emails. Fanchao will 
confirm each submission. 
Late submissions are NOT accepted unless you have the permission from Fanchao. 

 

 
Problems (100 points in total) 
A closer look at system calls in Linux 0.11/0.12 
The objective of this task is to concretely understand how system calls are used by user programs. 

 
MANDATORY TASK 
Explain in detail, when in a C user program the function write in libc is invoked to write chars to a tty
device (e.g. a console), how this function call reaches a system call in the kernel. Please use Linux 0.11/0.12
and libc-0.12 (https://github.com/oldlinux-web/oldlinux-files/tree/master/libs/libc/libc-0.1x) as your code 
references. The full execution path is required. You are NOT required to explain how the chars are actually 
written into the device. 

 
BONUS TASK 
1. Create a system call in the kernel, and successfully use it in a C program. 

 
Submission 
1. A document for the explanation of the system call. 
2. For the bonus task, code and results are required. 

 

Answer 

To answer this question, we must start from the beginning with write function itself. Within libc-0.12, the
write function is defined within write.c and the function contains _syscall3. This will set up the system call
arguments. It also contains the $0x80 instruction that causes an interrupt to switch to kernel mode. In the kernel,
we enter the system call table that's defined in kernel/sys_call.s and jump to sys_write. The function retrieves
file descriptor and checks to verify that the file is valid and actually contains a write function. Then the actual
write command is set at the end with the file->f_op->write() instruction. This should point to tty_write which 
should forward the call to the tty drivers' write function, con_write.  

 
Bonus Task

To implement the new system call sys_hellofanchao, we begin by adding the function definition in the kernel source. First, the function is added to sys.c with a simple printk call to print the message "Hello Fanchao!". Then, in include/unistd.h, the syscall number is defined as #define __NR_hellofanchao 74, assigning a unique syscall number to this new function.

Next, the system call table is modified in sys_call_table.c (or sys.h) to include an entry for sys_hellofanchao. The table maps syscall numbers to their corresponding functions, ensuring that when the kernel receives syscall number 74, it will jump to sys_hellofanchao.

Finally, the function is invoked in init/main.c within the main() function, ensuring that the new system call is executed early in the boot process. When the system boots, the message "Hello Fanchao!" is printed, demonstrating that the system call is correctly invoked.
 

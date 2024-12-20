***Flag:*** <br>
picoCTF{549698}
<br>
***Approaching the challenge*** <br>
I was made to understand that the **EAX** register is used to hold a value. Hence at the end of the main function the ***EAX** register is supposed to return the function's
return value. <br>
It was also clear that the value was going to be hexadecimal which was to be converted to normal decimal before getting the final flag. <br>
Installed gdb as per the hint on my subsystem. <br>
```
advait@Advait:~$ gdb debugger0_a
GNU gdb (Ubuntu 12.1-0ubuntu1~22.04.2) 12.1
Copyright (C) 2022 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<https://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from debugger0_a...
(No debugging symbols found in debugger0_a)
(gdb) run
Starting program: /home/advait/debugger0_a
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".

Breakpoint 1, 0x0000555555555131 in main ()
(gdb) step
Single stepping until exit from function main,
which has no line number information.
__libc_start_call_main (main=main@entry=0x555555555129 <main>, argc=argc@entry=1, argv=argv@entry=0x7fffffffe398) at ../sysdeps/nptl/libc_start_call_main.h:74
74      ../sysdeps/nptl/libc_start_call_main.h: No such file or directory.
(gdb) info registers eax
eax            0x86342             549698
```
chatGPT suugest to use break in the function by either *step* or *next* <br>
Finally using the info registers eax command it'll display all the current value of registers which had both the hexadecimal and the normal decimal value in output.

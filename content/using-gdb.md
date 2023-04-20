---
title: "How to gdb"
date: 2023-04-15T19:32:37-03:00
tags: ['C','Debugging','GDB']
---

# How to C code using GDB 

GNU Debugger (also known as GDB) is a debugger that allows the user 
to see "what's happening" inside a program as it executes.\
\
GDB can be used to debug programs written in C, C++, Fortran, and Modula-2.

# Creating a sample code

```c
# include <stdio.h>

int main()
{
        int i, num, j;
        printf ("Type a number: ");
        scanf ("%d", &num );

        for (i=1; i<num; i++)
                j=j*i;    

        printf("%d factor is: %d!\n",num,j);
}
```

The code receives a number from the user through the scanf command and 
calculates its factorial.\
\
However, this code intentionally contains an error (if you know, you know) 
that when the number 6 is entered, the program returns "0" when it should 
return "720".

```c
6 factor is: 0!
```

Why does this happen? That's what we'll try to find out using GDB!

# Step 1 - Compiling the code

To use GDB in your code, you need to use the "-g" argument when compiling, 
which allows the compiler to collect information that will be essential for 
the debugging process. (See an example below)

```bash
$ cc -g factorial.c -o factorial
```

# Step 2 - Initialize GDB

Initialize GDB with the code below:

```bash
$ gdb factorial.c
```

If you don't have GDB installed on your system, just use the following command:

```bash
$ sudo apt install gdb
```

# Step 3 - Setting a "breakpoint"

A breakpoint is basically a signal that will define where your code should 
stop while it is executing, this will prevent your code from skipping over 
the part you want to examine.\
\
To set a new breakpoint is simple, just use the "break" command 
(you can use just "b" as an abbreviation for "break"):

```bash
(gdb) break line_number
```

In our case, since I want to examine the code loop, I will set the 
breakpoint for line 9:

```bash
(gdb) break 9
Breakpoint 1 at 0x11d3: file factorial.c, line 9.
```

# Step 4 - Running the program

To run the program, use the "run [args]" command (arguments can be used 
as if you were running the program normally). Since in our case arguments 
are not necessary, I will just use "run".

```bash
run
Starting program: /home/user/folder/factorial
```

With the code running it will execute normally until it reaches the first 
breakpoint defined and display the following message:

```bash
Breakpoint 1, main () at factorial.c:7
7	    for (i=1; i<num; i++)
```

And now you will use some commands to debug, as explained in the topics below...

# Step 5 - Displaying variable values

To check the value of a variable, just use the "print" command along 
with the variable name (you can use just "p" as an abbreviation for "print"), 
for example:

```bash
(gdb) print i
$1 = 1
(gdb) print j
$2 = 3042592
(gdb) print num
$3 = 6
```

As you can see, the variable "j" was not initialized correctly. Therefore, 
it will have a default garbage value, which will result in an unexpected 
value in the factorization.\
\
To fix the problem, simply define the "j" variable as 1, recompile the 
program, and run it again. The problem seems to be fixed... but even 
after that, something continues to cause the wrong factorial to be returned.\
\
So... before I bore everyone to death explaining step by step how to solve 
it, I propose that you apply what you learned from this post and try to 
find out what is wrong with this code on your own. (In the next topic, 
I will explain some extra commands to help you solve the problem)\
\
Extra Commands
\
Although it is an extra topic, the commands that will be mentioned are just 
as essential as those addressed above.
\
There are three types of operations you can perform in GDB after the 
program has reached the defined breakpoint. These commands are:
\
    `c` or `continue`: The code will continue to execute until it reaches another breakpoint.
    `n` or `next`: It will execute a single line of instructions, something like skipping to the next command to be executed.
    `s` or `step`: Similar to `next`, it will execute single instructions but with the difference that functions will not be treated as single instructions, making it execute line by line.
    `l` or `list`: This command will display the program source code.
    `help`: Displays a help screen with some other useful commands.

## References

https://linux.die.net/man/1/gdb
https://u.osu.edu/cstutorials/2018/09/28/how-to-debug-c-program-using-gdb-in-6-simple-steps/

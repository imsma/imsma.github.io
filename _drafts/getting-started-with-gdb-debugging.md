---
layout: post
title:  "Getting started with GDB Debugging"
author: sma
categories: [ C,C++,GDB,GCC ]
image: assets/images/posts/computer-connection-data-1181675.jpg
description: "Debugging with GDB, Getting started with GDB, GDB Guide, GDB Quick Start, Debugging C/C++ Code with GDB"
tags: [featured]
---

In this article we will learn how to debug C/C++ programs using the [GDB (GNU Debugger)](https://www.gnu.org/software/gdb/){:target="_blank"}. Before diving deep into the world of *GDB* let's define the term *debugger* and *debugging*.



## What is Debugger

*Debugger* is a computer program used to inspect another computer program to find issues, defects and bugs. Debugger allows us to execute the target program in a controlled manner i,e. to break the execution of a program when a specific condition happen such as.
 
 1. Break the the program when execution hits a specific line of a function.
 2. Break the program when value of variable v equals x e.g  v = 10.


## What is GDB
[GDB](https://www.gnu.org/software/gdb/){:target="_blank"} also known as GNU Project debugger is a portable thats works on all major operating systems (Windows, Linuxa and MacOS) and supports a lot of programming languages. GDB supports native (target program running on same machine) and remote (target program running on another machine) debugging.

### What GDB can do?
With controlled execution of the target program (program under debugging), GDB enables us to perform following tasks.

1. Let you stop your program on a specific condition, including a specific line of code or state of a variable during program execution.
2. Examine and change the value of variables to test the behavior of program with the modified value / state.


### Programming languages supported by GDB.

*GDB* supports following programming languages, however this article will only discuss debugging of programs written in C/C++.

1. C
2. C++
3. Assembly
4. Objective-C
5. Rust
6. Go
7. Pascal
8. Ada
9. D
10. Fortran
11. OpenCL
12. Modula-2


## Installing GCC Toolchain with GDB

In this section we will explore how to install *gcc* toolchain along-with gdb debugger on different platforms / operating systems.

### Installing GCC on MacOS
Open a terminal and run following command to install "Command Line Tools", these tools include the GCC toolchain.

```
xcode-select --install
```

Then install the GDB debugger using homebrew.

```
brew install gdb
```




### Installing GCC on Ubuntu

Follow these steps to install GCC toolchain on Ubuntu Linux.

1. Update the package list.
    ```
    sudo apt update
    ```
2. Install the `build-essential` package.
    ```
    sudo apt install build-essential
    ```
3. Run the `gcc --version` command to verify the correct installation of GCC toolchain.
    ```
    gcc --version
    ```

### Installing GCC on Windows

You can use [Cygwin](https://sourceware.org/cygwin/){:target="_blank"} or [MinGW](http://www.mingw.org){:target="_blank"} to get GCC toolchain on Microsoft Windows.

#### Note
For further information please visit the official [GNU GCC Installation page](https://gcc.gnu.org/install/binaries.html){:target="_blank"}.


## Sample C Program

Let's first create a sample C program that we need to debug, create a new file named `hello.c` using `nano` text editor.

```
nano hello.c
```

Copy following code to the newly created `hello.c` file, save and exit the nano editor using `Ctrl + O` and `Ctrl + X` shortcut keys.

```
#include <stdio.h>
int main()
{
  printf ("Hello World!\n");
  return 0;
}
```

Now let's compile and run the program to verify if works properly.

```
gcc hello.c -o hello
```

The above command will output the `hello` binary file, let's execute it to check if it works.

```
.\hello
```

The above program will print `Hello World!` on screen.

```
Hello World!
```

Let's start debugging the `hello` binary using `gdb` command.

```
gdb hello
```

Oh! it doesn't work, as you can see in following output the GDB is complaining about `debugging symbols` with an error message saying `(No debugging symbols found in hello)`, we will fix this problem in next step.

```
GNU gdb (GDB) 8.3
Copyright (C) 2019 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-apple-darwin18.5.0".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from hello...
(No debugging symbols found in hello)
(gdb) 
```



### Compiling the C/C++ program debugging symbols

To enable debugging of a binary executable we need to build it with *symbolic debugging information*, this is done using `-g` switch for `gcc` compiler. So let's compile our sample program with *symbolic debugging information*.

```
gcc -g hello.c -o hello
```

If your program is written in C++, you can compile it using `g++` compiler as shown in following example.

```
g++ -g hello.cpp -o hello
```

## Debugging C/C++ program using GDB

Let's attach the GDB debugger with our `hello` binary.

```
gdb hello
```

The above command will start the gdb debugger console as shown in following output.

```
GNU gdb (GDB) 8.3
Copyright (C) 2019 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-apple-darwin18.5.0".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from hello...
Reading symbols from /Users/u1/hello.dSYM/Contents/Resources/DWARF/hello...
(gdb) 
```

### Getting GDB Help
To get GDB help simply type `help` on `(gdb)` prompt and press enter.

```
(gdb) help
```

The above command will produce following output.

```
List of classes of commands:

aliases -- Aliases of other commands
breakpoints -- Making program stop at certain points
data -- Examining data
files -- Specifying and examining files
internals -- Maintenance commands
obscure -- Obscure features
running -- Running the program
stack -- Examining the stack
status -- Status inquiries
support -- Support facilities
tracepoints -- Tracing of program execution without stopping the program
user-defined -- User-defined commands

Type "help" followed by a class name for a list of commands in that class.
Type "help all" for the list of all commands.
Type "help" followed by command name for full documentation.
Type "apropos word" to search for commands related to "word".
Command name abbreviations are allowed if unambiguous.
```

### Existing the GDB Debugger

To exit the current debugging session of GDB type `quit` on `(gdb)` prompt and press enter.

```
(gdb) quit
```

The above command will terminate the running GDB session.

>>>>>
https://beej.us/guide/bggdb/#compiling


https://beej.us/guide/bggdb/#compiling




The [GDB](https://www.gnu.org/software/gdb/){:target="_blank"} (GNU Debugger) 






That's it, hope you enjoyed it. You like this article, have any questions or suggestions please let us know in the comments section.

Thanks and Happy Learning!

# The Command Line

## Learning Objectives

- understand what is meant by a command line interface
- know how to use the command line to navigate between directories

## Computers

Computers are machines that can perform calculations and operations on data. The development of computers since the 1930s has taken off at an explosive pace and today computers are present throughout the modern world. Computers are made up of many different physical components such as monitors, keyboards and chips that we refer to together as **hardware**. **Software** is the set of instructions written to be processed and executed by the hardware that makes up a computer.

## Operating systems

Modern computers such as a laptop often have to run several applications at the same time. A user could be browsing the internet whilst listening to music at the same time on 2 different applications. Each of these applications are made up of software which will require access to the physical resources of the computer in order to run. In addition, the various pieces of hardware used to make up your computer are manifold and complex - it would take a very long time indeed in order to fully comprehend the complexity of the hardware and the ways in which they interact with each other.

**Operating systems** are essentially master programs that manage and co-ordinate the different software and how they get access to the physical hardware of a computer. Without the use of an operating system programmers would have to directly interface with complicated hardware which would take up a lot of time. Operating systems allow people to build software far more efficiently by abstracting away the complexities of the hardware on a machine.

### Unix

Unix is an operating system first created and developed by a team of developers working in Bell Labs in the late 1960s. Many operating systems today are said to be **unix-like** including Mac OS, meaning that Mac OS behaves or has many of the features of the Unix Operating system.
Linux is another operating system invented in the early 1990s after Linus Torvalds built a new operating system based off the standards of Unix. The drive behind the development of Linux was the creation of an open-source operating system with many of the features and behaviours of the Unix operating system. The linux operating system is used by many computers across the world today and is often a popular operating system of choice amongst developers.

### DOS

Windows operating system emerged from the MS-DOS (MicroSoft Disk Operating System). The main difference between MS-DOS and Unix is that MS-DOS was designed for a single user.

## Interacting with a computer

An essential part of using a computer will be providing the computer or machine a set of instructions to **execute**. We can refer to a set of instructions that a computer executes as a **computer program**. **Execute** in this context means the machine carrying out the set of instructions contained within some computer program.

As users we can issue **commands** to the computer in order to perform certain tasks. A **terminal** is a computer interface that is used for entering text commands into your computer. You can type a command into your terminal, hit enter and expect to get back some form of text output as a result of running that command.

A **console** is a set of tools that are used to operate a machine. In the context of computers, a **console** comprises all the tools used to operate a machine, such as the **terminal** and the keyboard you use in order to type commands into the **terminal**. However, sometimes the terms **console** and **terminal** are used interchangeably.

## Shells

The term shell and terminal are often used interchangeably: however, they are not the same thing. A terminal allows users to input commands but this application doesn't know what to do with the command itself. A shell is a program that interprets commands and executes a program that will carry out the task. A user inputs a command in a **terminal** and then the **shell** will interpret and execute the command. A shell is a **command line interpreter** because it interprets and executes the commands inputted by a user.

### Bash

Bash is both a **command line interpreter** and a **programming language**. The default terminal used that is started when we open up a terminal will often be bash (when working on Unix-like operating systems such as Mac OS and Linux). The rich set of commands we can use to control our machine from the terminal is also said to form a **command line interface**.

### `echo` command

We can start to enter commands into the **terminal** and bash will interpret and execute these commands and then finally send some text output back to the terminal. One of the most basic commands we can use is `echo`:

```bash
$ echo <some-text-input>
```

In the above example, we are using the name of the command `echo` and then separated by a space some text input. The output is displayed in our terminal as follows:

```terminal
$ echo hello world
hello world
```

So the `echo` command will take some text input and output the same text in the terminal.

## File system

Computers that can store vast amounts of data. It makes sense for this storage of data to be organised so each piece of information has its own separate space. Collections of data are organised into groups called `files` which can can then be accessed and used to store more information. A **directory** is a container for multiple files and directories. We can organise a collection of related files into a single directory for organisational purposes.

### Navigating the file system

If we start in a new terminal we can use the command `pwd` which stands for **print working directory**. The shell program will currently be **inside** a directory that is to say it will have immediate access to all the files and sub-directories in this directory without having to access another file or folder.

```terminal
$ pwd
/Users/mitch
```

If I run the command `pwd` then we see the following printed to the terminal:

```terminal
/Users/mitch
```

This is referred to as the home directory and will be the directory the shell program starts **inside** when the terminal is first created.
The output `/Users/mitch` is a **file path**. A **file path** is a list of different directories separated by a `/`, indicating which `directory` the shell program is currently inside and which directories it is nested inside.

### `ls`

We will frequently want to ask which folders and directories are currently inside of the current directory. For this task we can use the command `ls` which can be used to print the contents - files an directories inside the current directory. `ls` command stands short for **list directory contents**. `ls` command is used in the following way:

```bash
$ ls <name-of-directory>
```

If no directory is specified when using this command then it will default to **listing the directory contents** of the **current working directory**.

```terminal
$ ls
Scores  Zoom  mitch_learnings  paradigms.pdf  solution_lectures_videos
```

When I run the `ls` command inside of my current directory I get an output listing 4 other directories:
`Scores`, `Zoom`, `mitch_learnings` and `solution_lecture_videos`
There is also a single file `paradigms.pdf` inside the current directory.

### `cd`

Suppose we wanted to enter one of the directories from above when we could use the `cd` command which stands for **change directory**. The `cd` command is used in the following way:

```bash
$ cd <name-of-directory>
```

A typical usage would be after running the `ls` command to see **which directories are currently inside the current directory** and then using `cd` in order **to change into that directory**:

```terminal
$ ls
Scores  Zoom  mitch_learnings  paradigms.pdf  solution_lectures_videos

$ cd Scores

$ pwd
/Users/mitch/Scores
```

It is very common to want to move into a parent directory from inside a current directory. Two dots like this `..` signify the parent directory and therefore we could write:

```terminal
$ cd ..
```

This command will move the user to the parent directory.

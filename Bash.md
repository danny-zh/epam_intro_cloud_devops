# Bash Bootcamp

## 1. Bash Introduction
### 1.1 What is the shell?

The shell is a CLI (Command Line Interface) program that interprets user commands that can be entered from the keyboard of read from a file. When commands are read from a file it is referred to as shell script or shell program. 

To interact with shells you can do:
- `$ cat /etc/shells` Lists the available shells on the system
- `$ /bin/sh && echo ps --pid $$` Changes the shell to sh and prints the process id of that sheel. $$ returns the pid of the last executed command.

### 1.2 Shell Type

Some common shell types are:

- Sh: Bourne shell. Used within Unix and Unix-like environments.
- Bash: Bourne again shell. A superset of sh, it includes an advanced parameter systems and it is the standard for modern linux distros. Bash is backwards compatible with sh commands. However not all sh commands are compatible with bash.
- Csh: C shell. Its sitax resembles that of  C programming language.
- Tcsh: Tenex Shell or Turbo C shell. A superset of csh
- Ksh: Korn Shell. A supertset of Bash, mainly appreciated by Unix users. A nightmare for beginners.

### 1.3 Bash Startup Files

The are two modes to start a shell: interactive login shell and non interactive login shell. The interactive login shell means the user needs to authenticate by username and password whereas non interective login shell means the user doesn't have to authenticate. An example of a non interactive login shell is when a user in a GUI opens the terminal application. There are some scripts or files that are run automatically when a new shell is opened and closed. The reading of the files depends on the shell type.

Interactive login shell reads:
- `/etc/profile` Global profile configuration when login
- `~/.bash_profile, ~/.bash_login or ~/.profile` User specific profile configuration when login. The first file available is read.
- `~/.bash_logout` User specific profile configuration when logout.

Non-interactive login shell reads:
- `~/bashrc` This file is usually referred to in ``~/bash_profile`

### 1.4 Shell Programming

Shell is also a programming language and specially used in scripting to automate repetitive tasks. Scripting means instructions are read from a flat file one line at a time, the flat file contains logic and commands that the shell interpreter can read and execute.

Advantages of shell programming:
- The syntax is simple and straightforward
- Short programs runs well in the first time.
- Debugging is straightforward

### 1.5 DO's and DON'Ts

Shell is a programming language to support automation tasks and creation of small system utilities, however it should not be considered to be used as a developement language since advanced features like data validation, OOP, data structures and multi-dimensional array support are not present into it.

Following is a brief list of where you shouln't use shell programming:
- Resource-intensive tasks like sorting, recursion, hashing
- Complex applications
- Security, where you need to protect against attacks
- Need to generate or manipulate GUI
- Propietary, closed-source applications (shell scripts are flat files that can be read by anyone)

## 2. Bash Syntax

### 2.1 Script Developement and Invocation

A script is nothing more that a sequence of commands stored in a file. This saves time of typing the same sequence of commands each time the script is invoked. The Sha-bang(#!) at the beginning of the file tells the systems that the following commands are to be fed to the command interpreter indicated by it. Afte sha-bang there a path to the command line interpreter to read and execute the commands line by line. The path can lead to a shell, a programming language of a system utility. 

The following are examples of valid sha-bangs:
- `#!/bin/sh`
- `#!/bin/bash`
- `#!/usr/bin/perl`
- `#!/usr/bin/env python`
- `#!/bin/sed -f`
- `#!/bin/awk -f`

Once the script is written, you can execute it by calling 
- `$ sh script_file` Using Sh shell
- `$ bash script_file` Using Bash Shell
- `./script_file` After adding executable (x) permission to the script file

### 2.1 Bash Options

Options are settings that change the shell or script behaviour. This options are added or removed by the set command. The set command follows the structure **set [-+]o option_name** or **set [-+abrv_option_name]** where the symbol "-" and "+" means add and remove options respectively. Figure 1 shows an example of set options.

<p align="center">
  <img src="https://github.com/userforpyhon47/epam_intro_cloud_devops/assets/134888524/e44938f1-00f4-4b57-b863-15213b95a524"
         alt="Figure 1" width="600" height="300"/>
  <br/>
  <em>Figure 1. Common set options</em>
</p>

Some examples:

- `set -ex` Exits (-e) the script if a status code diferent than 0 is returned from a command exept in loops and list construct. Prints (-x) commands and its argumments as they are executed
- `set -u` Exits (-u) the script if attempt to reading a variable that is not defined
- `set -o pipefail` Exits (-o pipefile) the script if a pipeline command return a non-zero value

### 1.3 Exit Codes

Exit codes are returned by commands to indicate the final status of its execution. Exit codes can range from 0 to 255, an exit code different to 0 indicates that the command ran into an error during its execution; depending of the exit code number you can figure out what happened. Figure 2 shows the list of error codes returned by commands.

<p align="center">
  <img src="https://github.com/userforpyhon47/epam_intro_cloud_devops/assets/134888524/2e07bdba-1a48-4c7c-a1fe-638c045e73c7"
         alt="Figure 2" width="600" height="300"/>
  <br/>
  <em>Figure 2. Exit codes</em>
</p>

### 1.4 Special Characters

A special character means a character that has an extended meaning when interpreted by the shell. Some of the special characters are shown in figure 3. The special characters can be used directly into the shell or in a script file.

<p align="center">
  <img src="https://github.com/userforpyhon47/epam_intro_cloud_devops/assets/134888524/260e6538-387b-4969-a108-ac2f9dce2140"
         alt="Figure 3" width="600" height="300"/>
  <br/>
  <em>Figure 3. Special Characters</em>
</p>

### 1.5 Variables

Variables are key-values pairs that allows for temporary storage of information. Variables are labels that are assigned to RAM memory locations where the actual data is stored.


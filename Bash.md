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
- `~/bashrc` This file is usually referred to in `~/bash_profile`

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

### 2.2 Bash Options

Options are settings that change the shell or script behaviour. This options are added or removed by the set command. The set command follows the structure **`set [-+]o option_name`** or **`set [-+abrv_option_name]`** where the symbol "`-`" and "`+`" means add and remove options respectively. Figure 1 shows an example of set options.

<p align="center">
  <img src="https://github.com/userforpyhon47/epam_intro_cloud_devops/assets/134888524/e44938f1-00f4-4b57-b863-15213b95a524"
         alt="Figure 1" width="600" height="300"/>
  <br/>
  <em>Figure 1. Common set options</em>
</p>

Some examples:

- `set -ex` Exits (-e) the script if a status code diferent than 0 is returned from a command except in loops and list construct (AND/OR). Prints (-x) commands and its argumments as they are executed
- `set -u` Exits (-u) the script if attempt to reading a variable that is not defined
- `set -o pipefail` Exits (-o pipefile) the script if a pipeline command return a non-zero value

### 2.3 Exit Codes

Exit codes are returned by commands to indicate the final status of its execution. Exit codes can range from 0 to 255, an exit code different to 0 indicates that the command ran into an error during its execution; depending of the exit code number you can figure out what happened. Figure 2 shows the list of error codes returned by commands.

<p align="center">
  <img src="https://github.com/userforpyhon47/epam_intro_cloud_devops/assets/134888524/2e07bdba-1a48-4c7c-a1fe-638c045e73c7"
         alt="Figure 2" width="600" height="300"/>
  <br/>
  <em>Figure 2. Exit codes</em>
</p>

### 2.4 Special Characters

A special character means a character that has an extended meaning when interpreted by the shell. Some of the special characters are shown in figure 3. The special characters can be used directly into the shell or in a script file.

<p align="center">
  <img src="https://github.com/userforpyhon47/epam_intro_cloud_devops/assets/134888524/260e6538-387b-4969-a108-ac2f9dce2140"
         alt="Figure 3" width="600" height="300"/>
  <br/>
  <em>Figure 3. Special Characters</em>
</p>

### 2.5 Variables

Variables are key-values pairs that allows for temporary storage of information. Variables are labels that are assigned to RAM memory locations where the actual data is stored. It is always preferred to quote with "" the value of a variable to delimit its range, specially when assigning a list of values to an array variable. Also it's important to follow best practices for variable naming and assignment, always stick to the conventions used in existing scripts. A good guide for shell style guide can be found at <a href="https://google.github.io/styleguide/shellguide.html" target="_blank">Style Guide</a>

To set a variable you can do one of the following:
- `var1="Some value here"` Preferred for single value assignment
- `var2=value` Less recommended for single value assignment
- `var3=("value1 value2 value3" "value4 value 5 value6")` Preferred for multiple value assignment

To access a variable you can do:
- `echo "${var1}"` Preferred
- `echo "$var2"` Acceptable
- `echo $var3` Less recommended

Variables in shell programming don´t have a type and normally they are interpreted as strings. However, depending on the context upon which those variables are invoked they allow arithmetic operations.

Bash has an extended set of options for variable manipulation, we will explore some of them:

**2.5.1 String Manipulation**  

2.5.1.1 Get the variable length whether its a single value variable or an array
   - `echo "${#var}"` Echoes the length of the variable or the lenth of the first item if variable is an array
   - `echo "${#var[@]}"  | echo "${#var[*]}"` Echoes length of the array

2.5.1.2 Get substring from string variable
  - `echo "${var:start_position:subs_length}"` Gets substring starting from index start_position and length subs_length from string variable var

2.5.1.3 left and right strip (deletion) of pattern from variable.
  - `echo "${var#pattern}"` Non greedy left strip of pattern from variable var
  - `echo "${var##pattern}"` Greedy left strip of pattern from variable var
  - `echo "${var%pattern}"` Non greedy right strip of pattern from variable var
  - `echo "${var%%pattern}"` Greedy right strip of pattern from variable var

2.5.1.4 Pattern replacement from variable
  - `echo "${var/pattern/replacement}"` Replace first occurence of pattern in the string var by replacement
  - `echo "${var//pattern/replacement}"` Replace all occurence of pattern in the string var by replacement

2.5.1.5 Prefix and sufix pattern replacement from variable
  - `echo "${var/#pattern/replacement}"` Replace occurence of pattern on the left side of the string var by replacement. This a greedy prefix replacement
  - `echo "${var/%pattern/replacement}"` Replace occurence of pattern in the right side of the string var by replacement. This is a greedy sufix replacement

**2.5.2 Parameter substitution**
Parameter substitution allows the shell to react based on the presence (variable set with value), absence(variable not set) or nullability (variable set without a value) of a variable and use, set or modify the variable's value

2.5.2.1 Use default value if variable is not setted or if it's setted with a null value.
- `${var-default}` Use default value if variable var is not setted
- `${var:-default}` Use default value if variable is not setted or if variable is setted with a null value

2.5.2.2 Set variable with default value if variable is not setted or if it's setted with a null value
- `${var=default}` Set variable var with default value if variable is not setted
- `${var:=default}` Set variable var with default value if variable is not setted or if variable is setted with a null value

2.5.2.3 Use value if variable is setted or if it's setted with a null value. If variable is not setted it will use null string
- `${var+value}` Use value if variable var is setted
- `${var+:value}` Use value if variable var is setted but has null value

2.5.2.4 If variable is setted use it, otherwise use error message and exits script with exit code 1
- `${var?err_msg}` Use err_msg if variable var is not setted and exit with code 1
- `${var?:err_msg}` Use err_msg if variable var is setted but has null value and exit with code 1


Bash also manages some special variables types:

- Local variables: Variables that live inside a code block, cannot be accesed from outside world
    ```
      func() {
        var="$1"
        echo "${var"
      }
      echo "${var}" #Echoes null string
      func Hi! #Echoes positional paramenter Hi!
    ```
- Environment variables: Variables that affect the behaviour of the shell or the user interface. The environment variables can be set to:
  - `/etc/environment` Change system-wide behaviour
  - `export VAR=VALUE` Change current user shell behaviour
  - `~/bash_profile; ~/bashrc` Change all sessions of user shell behaviour
  - `. ~/env_vars or source ~/env_vars` set env variables from an script
- Positional parameters: These are the arguments that are passed to an script
  - `$1, $2, $3 ... ${n}` Means to access the nth argument passed to the script. If the argument is greater than 9, {} must be used to access it
  - `$0` Means the name of the script being executed
  - `$#` Means the number of arguments passed to the script
  - `$* or $@` Means all the arguments passed to the script 
- Built-in variables: Are the environment variables set and used by the system
  - `env or printenv` Displays environment variables
  - `set` Displays shell variables. These are temporary variables that exists as long as the shell does
- Special variables: Are variables that can be used during script execution for advanced logic. Figure 4 shows special variables

<p align="center">
  <img src="https://github.com/userforpyhon47/epam_intro_cloud_devops/assets/134888524/edc95624-29c4-4b27-bb9d-5123ab298136"
         alt="Figure 4" width="600" height="300"/>
  <br/>
  <em>Figure 4. Special Variables</em>
</p>

Bash also support arrays. Arrays are variables that hold multiple values. To access a particular value it can be referenced by its index.

- `declare -a var` Explicitly declares variable var as an array
- `var=("value 1" "value 2" "value 3")` Implicity declares variable var as an array
- `var[4]="value"` Assigns a value to array var at index 4
- `echo "${var[4]}"` Accesses to value at index 4 of the array variable var
- `echo "${var[@]}" or echo "${var[*]}"` Accesses all values of array variable var
- `ècho ${#var[@]}` Prints the length of the array
- `declare -p var` Displays the type of the varible var, in this case an array

2.6 Conditions

Shell programming includes conditional logic, this allows for an advanced flow control of the scripts. In terms of conditions bash includes the following:

- `$ test -f myfile && echo "exists" || echo "missing"` The test command, tests whether myfile is present in current directory. Returns 0 for true and 1 for false
- `[ -f myfile ]` An abbreviation of the test command
- `[[ -f myfile ]]` An extended set of the test command. Allows the use of && or || inside the condition where as single bracket [] generates an error
- `let "a=1, b=2, c=a+b";echo "${c}"` The let command allows to perform arithmetic operation. This sets shell variables
- `(( result = 1 + 3));echo $result` The (()) command is similar to let. Its used is preferred over let since it can be used directly in conditional statements
- **if/elif** constructs
  ```
    if [[ condition1 ]]; then
      command1
      command2
    elif [[ condition2 ]]; then
      command4
      command5
    else
      default-command
    fi
  ```
- **case** constructs: Each case clause must be ended with ;;
  ```
    case EXPRESSION in
      case1)
        command1;
        command2;
      ;;
      case2)
        command3
      ;;
      …
      caseN)
        commandM
      ;;
    esac
  ```
- **Lists** constructs: the AND list and OR list constructs provide an easier way to replace complex nested if/elif statements. For AND list construct each command executes its turn only if the previous command exit code was true; for OR list construct each command executes its turn only if the previous command exit code was false.
  ```
    $ command-1 && command-2 && command-3 && ... command-n
    $ command-1 || command-2 || command-3 || ... command-n
  ```

### 2.7 Loops

Loops are control instructions that iterates over the same block of code until the control condition statement is met. In shell programming there three types of loops:

- For: Iterates over a set on known values
  ```
      for file in "$( find . -type l )"; do
        echo "$file"
      done | sort
  ```
- While: Iterates while the control condition is true (returns exit code 0)
  ```
    while [ "$a" -le $LIMIT ] ; do
      echo -n "$a "
      let "a+=1"
    done
  ```
- Until: Iterates until the control condition becomes true (returns exit code 0)
  ```
    until [ condition-is-true ] ; do
      command(s)…
    done
  ```

### 2.8 I/O Redirection

In linux there is the concept of file descriptors which represent an unique number identifier that is associated to each opened file or other system input/output resources. File descriptors are normally used by programs to read from or to write to those opened files or input/output resources. By default there are always three files opened that refers to the default file descriptors 0, 1 and 2 respectively.

- STDIN (file descriptor &0): The keyboard
- STDOUT (file descriptor &1): The screen
- STDERR (file descriptor &2): Error message output to the screen

All the opened files or system input/output resources can be redirected. I/O redirection means capturing the output of a file, command, script, program or a code block and sending it as input to another file, command, script or program. The redirection normally uses the default file descriptors and are referred to as STDIN redirection (&0), STDOUT redirection (&1) and STDERR redirection (&2)

- STDIN Redirection: Redirects the input for a file, command, script, program or code block. To redirect STDIN you have to use "<"
  - `$ grep keyword < filename` Redirects the contents of filename as input to the grep command, then grep search inside the content for the keyword.
- STDOUT Redirection: Redirects the output of a file, command, script, program or code block. To redirect STDOUT you have to use "1> or >". In STDOUT if the write to file doesn't exist, it is created.
  -`$ ls -la > file_list.txt` Redirects the ouput of the command ls -la to the file file_list.txt
  -`: > file_list.txt` Truncates the file file_list.txt to zero length
  -`$ ls -la >> file_list.txt` Redirects the ouput of the command ls -la and appends it to the end of the file file_list.txt
- STDERR Redirection: Redirects the error message of a file, command, script, program or code block. To redirect STDERR you have to use "2>". In STDERR if the write to file doesn´t exist, it is created
  -`$ cat file 2> error_messages.txt` Redirects the error message generated by the command cat to the file error_message.txt
  -`$ cat file 2>> error_messages.txt` Redirects the error message generated by the command cat and appends it to the end of file error_message.txt
  -`$ cat file 1> messages.txt 2>&1` Redirects the error message generated by the command cat to the file descriptor &1
  -`$ cat file &> messages.txt ` Redirects both STDOUT and STDERR to the specified file.


Another redirection similar to that of STDIN can be achieved by using "here documents" and "here strings"

- Here documents or here doc (<<DELIMITER): This type of redirection instructs the shell to read input from the current source until a line containing only delimiter (with no trailing blanks) is seen. All of the lines read up to that point are then used as the standard input for a command.
  - `$ cat << EOF > lines.txt` It will read all the lines input by keyboard until it sees the delimiter EOF. The the output is redirected to file lines.txt

- Here strings (<<<$command): It is a version of a here doc, the $command it's expanded and the result fed to the STDIN of the target.
  - `$ cat <<< $PATH` It will expand the env variable $PATH and fed into the STDIN of cat


Another redirection similar to that of STDOUT can be performed with the PIPE operator "|", this operator allows to chain up multiple commands

- `$ cat file | cut -d":" -f1 | sort > result.file` Example of pipe operator
- `$ echo "${PIPESTATUS[@]}"` Echoes the exit codes status of each command executed in the pipe

In linux there are some other special filenames as ilustrated in the figure 5

<p align="center">
  <img src="https://github.com/userforpyhon47/epam_intro_cloud_devops/assets/134888524/8eecb1c4-f8f6-4115-a5f4-1ac23df7c86b"
         alt="Figure 5" width="600" height="300"/>
  <br/>
  <em>Figure 5. Special Filenames</em>
</p>

An example for using a file descriptor with special filename /dev/tcp to set up a tcp network connection to a host
  `$ exec 4<>/dev/tcp/google.com/80` Opens tcp connection to google.com and assigns it to file descriptor 4
  `$ echo -e "GET /HTTP/1.0\n" >&4` Send get request to the opened connection using STDOUT to file descriptor 4
  `$ cat <&4` STDIN to cat command the contents of the response from file descriptor 4
  `$ exec 4<&-` Release the file descriptor 4
  
An example of code block STDIN redirection is as follows:

- Using for loop
```
  #!/bin/bash
  for arg in $(cat); do
    echo "${arg:-none}"
  done <<< $(echo "$PATH" | xargs -0 -d ":" -n 1)
```

- Using while loop
```
  #!/bin/bash
  while IFS= read -r line; do
    echo "${line:-none}"
  done <<< $(echo "$PATH" | xargs -0 -d ":" -n 1)
```

### 2.9 Functions

Functions are litte pieces of code blocks that can be executed reapeatedly. The basic sintax to create a shell function is as shown below;

```
  function_name () {
    commands
  }
```

To invoke the function we simply follow:

- `$ function_name $arg1 $arg2` Arguments are passed to the function independently whether the function processes them or not, the function receives them as positional arguments, i.e, $1, $2, ..., ${n}
- `exit code_number` Functions can impement exiting of the script if a condition is met, for instance if an argument is missing. The code_number can range from 1 to 255.
- `return code_number` Functions can impement the return of a code number. The return only quits from the function, not from the script. The code_number can range from 1 to 255.
  
## 3. Bash Style Guide

### 3.1 Environment

All error messages should go to STDERR. This makes it easir to separate them from normal messages. A print status error function is recommended.

```
  error ()
  {
    echo "Error: [$(date +"%Y-%m-%dT%H:%M:%S%Z")] $*" >2 #Redirection to STDERR
  }

  if ! do_something; then
    error "Unable to do something"
    exit 1
  fi
```

### 3.2 Comments

Comments are important for other users who will maintain the code to understand the purpose and behaviour of your scripts. You have to comment:

- File header comments: Includes a brief overview and description of the script.
```
  #!/bin/bash
  #
  # Perform hot backups of Oracle databases.
```
- Functions comments: Any function that is not both obvious and short must be commented. Any function in a library must be commented regardless of length or complexity. The function comment describes the purpose of the functions as well as the global parameters used and modified, the input arguments, the output to STDOUT or STDERR, and the return code

```
  #######################################
  # Cleanup files from the backup directory.
  # Globals:
  #   BACKUP_DIR
  #   ORACLE_SID
  # Arguments:
  #   Backup path
  # Output:
  #   Write success to STDOUT or failure to STDERR
  # Return:
  #   Non-zero on error
  #######################################
  function cleanup() {
    …
  }
```
- Implementation comments: Describes tricky, non-obvious or interesting parts of your code
- TODO comments: Use TODO comments for code that is temporary, a short-term solution, or good-enough but not perfect. Describes pending tasks to be completed.
```
  # TODO(mrmonkey): Handle the unlikely edge cases (bug ####)
```

### 3.3 Formatting

Formatting refers to the use of the best practices when writing an script. It is helpful to maintain an uniform formatting style along all the scripts specially when those are mantained by other members, this is intended to keep consistency. Some rules to keep formatting are:

- Identation: Use 2 whitespaces, not tabs. For code block separation use 1 blank line
- Line length and long strings: Maximum line length is 80 characters. If larger than that you can use a here document (<<EOF) or embedded new lines in the string are also ok

```
  cat <<END
  I am an exceptionally long
  string.
  END
    
  # Embedded newlines are ok too
  long_string="I am an exceptionally
  long string."
```

- Pipelines (|, &&, ||): All chained commands must fit into a single line. If not you have to split each command line by line with the pipe (|) or logical compounds (||, &&) in the beginning of the new line and use 2 whitespaces after that for writing the command

```
  # All fits on one line
  $ command1 | command2
  
  # Long commands
  $ command1 \
    | command2 \
    | command3 \
    | command4
```
- Loop: Put "; do" and "; then" on the same line as the while, for or if. Else statement should be in his own line.
- Case statement: Ident alternatives by 2 whitespaces. A white space must be added after the closing parentheses of the pattern.

  ```
    case "${expression}" in
      a) 
        variable="…"
        some_command "${variable}" "${other_expr}" …
        ;;
      absolute) 
        actions="relative"
        another_command "${actions}" "${other_expr}" …
        ;;
      *) 
        error "Unexpected expression '${expression}'"
        ;;
    esac
  
    # Simple commands may be put on the same line as the pattern and ;; as long as they keep simple and readable
    verbose='false'
    aflag=''
    bflag=''
    files=''
    while getopts 'abf:v' flag; do
      case "${flag}" in
        a) aflag='true' ;;
        b) bflag='true' ;;
        f) files="${OPTARG}" ;;
        v) verbose='true' ;;
        *) error "Unexpected option ${flag}" ;;
      esac
    done
  ```

- Variable expansion:
  - Stay consistent
  - Don’t brace-delimit single character shell specials or positional parameters, unless strictly necessary for avoiding deep confusion.
  - Prefer brace-delimiting all other variables. Prefer "${var}" over "$var".
    
```
  # Section of *recommended* cases.
  
  # Preferred style for 'special' variables:
  echo "Positional: $1" "$5" "$3"
  echo "Specials: !=$!, -=$-, _=$_. ?=$?, #=$# *=$* @=$@ \$=$$ …"
  
  # Braces necessary:
  echo "many parameters: ${10}"
  
  # Braces avoiding confusion:
  # Output is "a0b0c0"
  set -- a b c
  echo "${1}0${2}0${3}0"
  
  # Preferred style for other variables:
  echo "PATH=${PATH}, PWD=${PWD}, mine=${some_var}"
```

- Quoting:
  - 'Single' quotes indicate that no substitution is desired.
  - "Double" quotes indicate that substitution is required/tolerated.
 
### 3.4 Naming Conventions

- Function names: Lowercase with underscore to separate words and "::" to separate libraries (packagers). The keyword function is optinal, but its use or dimiss should be consistent.

```
  # Single function
  my_func() {
    …
  }
  
  # Part of a package
  mypackage::my_func() {
    …
  }
```

- Variable names: Variables names for loops should be similarly named for any variable you’re looping through.

```
  for zone in "${zones[@]}"; do
    something_with "${zone}"
  done
```

- Contants and environment variables names: Must be declared at the top of the file. Use uppercase and underscore to separate words. Some things become constant at their first setting (for example, via getopts). Thus, it’s OK to set a constant in getopts or based on a condition, but it should be made readonly immediately afterwards. For the sake of clarity readonly or export is recommended instead of the equivalent declare commands.

```
  # Constant
  readonly PATH_TO_FILES='/some/path'
  
  # Both constant and environment
  declare -xr ORACLE_SID='PROD' #-r means readonly, -x means export to env

  VERBOSE='false'
  while getopts 'v' flag; do
    case "${flag}" in
      v) VERBOSE='true' ;;
    esac
  done
  readonly VERBOSE
```
- Local variables: Declare local variables that are specific inside the functions you create. Declaration and assignment should be on different lines.
```
  my_func2() {
    # Separate lines for declaration and assignment:
    local my_var
    my_var="$(my_func)"
    (( my_Var == 0 )) || return
  }
```

- Function location: Declare all functions just below the all the constants. Do not include executable code between functions as debugging become nasty.
- Main function: For scrtips long enough you can define a main function as the botton one in the functions definitions. The main function allows to encapsulate the main code and can help to quickly identify where the program starts. Besides that, main also support to define other local variables that would be impossible to named as local if used outside a function block scope. To call the main function just do "main $@"

### 3.4 Calling Commands

Allows to check the status of the the commads called in a script.

- Checking return values: Always check return values and give informative return values. For unpiped commands can use "#?" direcltly or also can use an id statement

```
  if ! mv "${file_list[@]}" "${dest_dir}/"; then
    echo "Unable to move ${file_list[*]} to ${dest_dir}" >&2
    exit 1
  fi
  
  # Or
  mv "${file_list[@]}" "${dest_dir}/"
  if (( $? != 0 )); then
    echo "Unable to move ${file_list[*]} to ${dest_dir}" >&2
    exit 1
fi
```

- Pipestatus: For pipes you can check the return values of all commands in the pipe. Pipestatus is overwritten by following comands after the pipe, that's why to keep the code statuses of the pipe you can assign them to a variable. Specially if you need to react differently to code statuses of each command.
```
  tar -cf - ./* | ( cd "${DIR}" && tar -xf - )
  return_codes=( "${PIPESTATUS[@]}" )
  if (( return_codes[0] != 0 )); then
    do_something
  fi
  if (( return_codes[1] != 0 )); then
    do_something_else
  fi
```
- Built-in Command Vs External Commands: Given the choice of using built-in and external commands that can carry the task, prefer to use built-in commands since they are more portable and robust. External commands will invoke another process.

```
  # Prefer this:
  addition=$(( X + Y ))
  substitution="${string/#foo/bar}" #Built-in shell prefix substitution
  
  # Instead of this:
  addition="$(expr "${X}" + "${Y}")"
  substitution="$(echo "${string}" | sed -e 's/^foo/bar/')"
```

## 4. Text Processor: AWK & SED

### 4.1 AWK Overview



# Chapter 1, Section 1: Writing Shell Scripts (Bash Scripts)

This guide to the RHCE follows on from the RHCSA course. Please check that course out first before proceeding with the RHCE course. That exam is a preliminary for sitting the RHCSA and the skills built up for that exam are required for sitting the RHCE.

Repetitive tasks can be automated on RHEL using shell scripts (also known as bash scripts). These scripts are text files which contain Linux commands, control structure (such as if statements or loops) and comments.

Scripts can accept run-time arguments and can contain simple or very complex commands. Scripts are executed directly on the terminal which are interpreted at run time and not compiled like some programming languages. As with other languages, the scripting skill improves over time as more scripts are written and read.

Commands in scripts are executed in the order they are written within the script (top-down). Each line is executed as if the script were typed at the terminal. As the script executes commands, control structures such as if statements and for loops can be used to add structure to the commands, so that commands are only executed based on a given criteria, or to repeat commands several times. Comments are used to explain the commands within scripts and to embed other information, such as the scripts author or version.

When the script is executed, any issues with the commands are displayed on screen just as if the command were executed manually on the shell. We will stick to using the bash shell, but scripts written for that shell can be used with others if they are slightly modified.

Bash scripts can be written in almost any text editor (such as notepad++, atom, gedit, emacs) but using vi is recommended. The nl command can be used to quickly enumerate a script with line numbers.

## A simple script
need example script

```
  #!/bin/bash
```

The first line indicates the shell in which the script will run, in our case, it is bash. This first line can also be used to indicate other executables, such as python or bash, which can be used to execute scripts in those languages.

## Making the script executable to other Users

First make it executable

```
 # chmod +x /usr/local/bin/example_script.sh
```

The default path variable includes /usr/local/bin by default, so users can execute scripts in that directory either by full path, relative path or script name. If this folder is not in the path variable or if you want to use scripts in another folder, the default profile will need to be modified to include it (/etc/profile). Users can do this manually by modifying the .bash_profile file in their home directory.

To modify the path file at the command line, use the following command. This will append /usr/local/bin to whatever the current path variable contains:

```
  export PATH=$PATH:/usr/local/bin
```

## Debugging shell scripts

Writing scripts is typically a rinse-and-repeat process: you write the script, you run it and check for errors, fix them and repeat. However, the following debug technique makes scripting much easier. Add the following the -x option to the bash shell line at the top of the script:

```
  #!/bin/bash -x
```

You can also run scripts using bash directly with the -x option like so:

```
 bash -x /some/script.sh
```

## Using Variables
need examples of using local variables in bash scripts
need examples of using environment variables

## Parsing Command Output
It possible to store the result of a command into a variable within a script. There are two ways to do this shown below:

```
  #!/bin/bash
  HOSTNAME_FULL=`hostname -f`
  KERNEL_VERSION=$(uname -r)
  echo "Kernel Version: $KERNEL_VERSION"
  echo "Hostname: $HOSTNAME_FULL"
```

need script output

## Positional Parameters/Command line arguements
The positional parameters/variables and their meanings:

```
  # Need listing of positional parameters
  $0 Represents the command or script
  $1-9 Represents arguments 1 through 9
  ${10} and above Represents arguments 10 and above.
  $# Gives the count of all parameters  
  $* represents all arguements
  $$ Represents the PID of the command/script

```

## Shifting Command Line Arugments

The shift command is used to move command line arguments one position to the left. During this move, the value of the first argument is lost.

```
  #!/bin/bash
  # need example of using shift.

  echo "The params passed to this script are: $$"
  echo "The first param passed to the script is: $1"
  shfit
  echo "param 2 has now been shifted to param 1 and the value is $1"
```

The shift command can also be used with a number to indicate how many left shits should occur. For example, shift 5 will shift the parameters five places to the left.

```
  # need example of multiple shift
```

# Interactive Bash Scripts (Getting input from the User)

Though run-time parameters are useful, sometimes it is better to ask the user for input as it can be more user friendly for some. Bash scripts can prompt the user with questions (such as enter an IP address) and store the input into a variable which can be used within the script. The read command is used to get information from the user and can be seen in the example script below.

```
   #!/bin/bash
   echo -e "Enter a value: \c"
   read USERVALUE
   echo "You input the variable: $USERVALUE"
```

Note the use of the \c escape sequence in this script. This ensures that the echo command waits for user input before proceeding. There are other escape sequences used by the bash shell, such as \t for tab, \n for newline, \a for beep (alert), \f for form feed, \r for carriage return and \b for backspace. The escape sequences \h, \u and \w can be used to display the hostname, username and current working directory.

# Logical Statements in scripts
Logical statements are used to break up and manipulate the flow of execution within bash scripts. At some point a script will most likely need to check for a certain condition before running a command. To do this, the shell offers two logical constructs, the if-then-fi statement and the case statement.

## Exit Codes (return code)
Exit codes refer to the value returned by a program or script when it finishes execution. The exit code is determined by the outcome of the program based on if there was an error during execution or if it finished successfully.

```
  need example
```

## Test Conditions
Test conditions can be set on numeric values, string values or file using the test command. Test conditions should be enclosed within square brackets [] when not using the test command explicitly.

```
  need examples of different tests
  equal
  not equal
  less than
  greater than
  ...
```

# IF statements

The if-then-fi statement evaluates the condition for true or false. The statement executes the command(s) specified within the test condition and if they execute to true, then the command(S) within the if block will be executed. If the condition is not met, the code within the block is skipped.

```
  need example if then fi
```

# If-then-elif-fi statements

```
 need example of if-then-elif-fi statements
```

# Looping statements (repetition)
Loop statements can be used to repeat a series of commands several times. There are three different statements which can be used for looping, these are for-do-done, while-do-done and until-do-done.

```
  need examples of different loops
```

## Loop Test conditions
The let statement is used in looping constructs to evaluate the condition at each iteration. It compares the value stored in a variable against a pre-defined value. For each iteration the condition is evaluated and depending on the outcome (true or false), the loop will continue.

```
  need example of let condition.

```

## The for-do-done loop

The for-do-done loop is one looping construct available for use on the bash terminal. The for-do-done loop is executed on an array of elements until all the elements in the list are used up. Each element is assigned to a variable one after the other for being processed within the loop.   

```
  for VAR in list
  do
    command statements
  done
```

```
  need example of for-do-done loop

```

## The while-do-done

The while-do-done loop is another available looping construct for use on the bash terminal. The test condition is usually an arithmetic expression containing the test or let command.

```
  while   condition
  do
    command block
  done
```

```
  need example of while-do-done loop
```

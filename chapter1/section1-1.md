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

# Writing an interactive script

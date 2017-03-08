# Chapter 1, Section 1: Writing Shell Scripts (Bash Scripts)

This guide to the RHCE follows on from the RHCSA course. Please check that course out first before proceeding with the RHCE course. That exam is a preliminary for sitting the RHCSA and the skills built up for that exam are required for sitting the RHCE.

Repetitive tasks can be automated on RHEL using shell scripts (also known as bash scripts). These scripts are text files which contain Linux commands, control structure (such as if statements or loops) and comments.

Scripts can accept run-time arguments and can contain simple or very complex commands. Scripts are executed directly on the terminal which are interpreted at run time and not compiled like some programming languages. As with other languages, the scripting skill improves over time as more scripts are written and read.

Commands in scripts are executed in the order they are written within the script (top-down). Each line is executed as if the script were typed at the terminal. As the script executes commands, control structures such as if statements and for loops can be used to add structure to the commands, so that commands are only executed based on a given criteria, or to repeat commands several times. Comments are used to explain the commands within scripts and to embed other information, such as the scripts author or version.

When the script is executed, any issues with the commands are displayed on screen just as if the command were executed manually on the shell. We will stick to using the bash shell, but scripts written for that shell can be used with others if they are slightly modified.

Bash scripts can be written in almost any text editor (such as notepad++, atom, gedit, emacs) but using vi is recommended. The nl command can be used to quickly enumerate a script with line numbers.

## A simple script

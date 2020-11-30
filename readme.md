# Linux Commands

This tutorial helps in understanding the basic linux commands.

## Commands:

- alias:    Used to create an alias.
    
    Example:
    ```
    alias myfirstalias='ls -lrt | grep -i name'
    ```
- unalias:  Used to remove the alias.
- type:     Used to provide a short description about the command.

## Redirection:

Basic function of any command is to take some input and provide an output. With redirection, this Input and Output can be changed.

- **stdin**: standard input, which is the keyboard
- **stdout**: standard output, which is the screen
- **stderr**: standard error, a device which collects all the error messages from commands

The symbol used for redirection is, `>`. This will redirect the output from `stdout` to any specified file.

Below examples with demonstrate multiple usage of redirection:

### Example 1: Redirect stdout to a file

To redirect Output of a command to a file:

```
ls -lrt > listings.txt
```

The above command will create a New file, everytime the command executes, and will redirect the output of the command `ls -lrt` into the file `listings.txt`. 

> NOTE: This will override the existing content as well.

### Example 2: Redirect stdout to an existing file by appending

To append to an existing file with the redirect output:

```
ls -lrt >> listings.txt
```

The `>>` redirect symbol will append the output of command `ls -lrt` to the existing file `listings.txt`. This way you can process the output of any shell script to a file and analyze the execution status at the end.

### Example 3: Redirect stdin to a command

To redirect `stdin` the operator used is `<`. Lets say, to send the content of a file as an attachment to a mail, you can use something like below:

```
mail -s "News Today" abc@gmail.com < NewsFlash.txt
```

Another Example:

Output of below 2 commands are same:

```
cat myfile
```

and 

```
cat  < myfile
```

>NOTE for few command explicitly specifying the redirection is not mandatory.

### Example 4: Redirect stderr of a command to a file

Here need to learn a new concept called file descriptor. Below are the file descriptors for each one:

| stdin     | 0 |
| stdout    | 1 |
| stderr    | 2 |

To transfer `stderr` to a file, we need to specify the file descriptor.

```
mkdir '' 2> mkdir-log.txt
```

```
sort < /etc/passwd > outlog 2> errlog
```

### Example 5: Redirection using pipe (|) command

Pipe redirects the output of one command to another command.

```
ls -l /usr/bin/ | less
ps -ef | grep abc
ls -l /usr/ /usr/bin/ | sort | less
```

## Processes in Linux:

| `ps`      | Prints the process started by the current shell session.          |
| `ps -A/e` | Prints all processes currently running in the system.             |
| `ps -aux` | Prints all processes currently running in the system with states  |

- Process States: Below are the major process states

R - Running : Process is currently Running to about to start
S - Sleeping : Process is waiting for input like keystroke or network packet or something
D - Uninterruptible Sleep : Process is waiting for IO, like disk drive.
T - Stopped : Process has been stopped
Z - Zombie : Here the child process has been terminated but not cleaned up by its parent.
< - High Priority Process : Processes not NICE, as they need more CPUs to complete.
N - Low Priority Process: NICE processes, as they need less CPU resources for execution.

Few Commands to monitor the processes:
- pstree: displays the processes in tree format
- htop: external package: dispalys good way of processes
- top: displays detailed processes and memory usage

There are multiple ways to kill a process:
1. Using `ctrl+c` command: If the process is running in the foreground, you can use `ctrl+c` to kill the process.
2. Using `kill` command: If the process is running in the background, then you can kill the process using its PID. Kill commands takes multiple SIG values listed below:
    - `kill -l` displays all the kill signals. Most commonly used kill signals are 1, 3, 9, 15.
        - SIGHUP - The SIGHUP signal disconnects a process from the parent process. This an also be used to restart processes. For example, "killall -SIGUP compiz" will restart Compiz. This is useful for daemons with memory leaks.
        - SIGINT - This signal is the same as pressing ctrl-c. On some systems, "delete" + "break" sends the same signal to the process. The process is interrupted and stopped. However, the process can ignore this signal.
        - SIGQUIT - This is like SIGINT with the ability to make the process produce a core dump.
        - SIGILL - When a process performs a faulty, forbidden, or unknown function, the system sends the SIGILL signal to the process. This is the ILLegal SIGnal.
        - SIGTRAP - This signal is used for debugging purposes. When a process has performed an action or a condition is met that a debugger is waiting for, this signal will be sent to the process.
        - SIGABRT - This kill signal is the abort signal. Typically, a process will initiate this kill signal on itself.
        - SIGBUS - When a process is sent the SIGBUS signal, it is because the process caused a bus error. Commonly, these bus errors are due to a process trying to use fake physical addresses or the process has its memory alignment set incorrectly.
        - SIGFPE - Processes that divide by zero are killed using SIGFPE. Imagine if humans got the death penalty for such math. NOTE: The author of this article was recently drug out to the street and shot for dividing by zero.
        - SIGKILL - The SIGKILL signal forces the process to stop executing immediately. The program cannot ignore this signal. This process does not get to clean-up either.
        - SIGUSR1 - This indicates a user-defined condition. This signal can be set by the user by programming the commands in sigusr1.c. This requires the programmer to know C/C++.
        - SIGSEGV - When an application has a segmentation violation, this signal is sent to the process.
        - SIGUSR2 - This indicates a user-defined condition.
        - SIGPIPE - When a process tries to write to a pipe that lacks an end connected to a reader, this signal is sent to the process. A reader is a process that reads data at the end of a pipe.
        - SIGALRM - SIGALRM is sent when the real time or clock time timer expires.
        - SIGTERM - This signal requests a process to stop running. This signal can be ignored. The process is given time to gracefully shutdown. When a program gracefully shuts down, that means it is given time to save its progress and release resources. In other words, it is not forced to stop. SIGINT is very similar to SIGTERM.
        - SIGCHLD - When a parent process loses its child process, the parent process is sent the SIGCHLD signal. This cleans up resources used by the child process. In computers, a child process is a process started by another process know as a parent.
        - SIGCONT - To make processes continue executing after being paused by the SIGTSTP or SIGSTOP signal, send the SIGCONT signal to the paused process. This is the CONTinue SIGnal. This signal is beneficial to Unix job control (executing background tasks).
        - SIGSTOP - This signal makes the operating system pause a process's execution. The process cannot ignore the signal.
        - SIGTSTP - This signal is like pressing ctrl-z. This makes a request to the terminal containing the process to ask the process to stop temporarily. The process can ignore the request.
        - SIGTTIN - When a process attempts to read from a tty (computer terminal), the process receives this signal.
        - SIGTTOU - When a process attempts to write from a tty (computer terminal), the process receives this signal.
        - SIGURG - When a process has urgent data to be read or the data is very large, the SIGURG signal is sent to the process.
        - SIGXCPU - When a process uses the CPU past the allotted time, the system sends the process this signal. SIGXCPU acts like a warning; the process has time to save the progress (if possible) and close before the system kills the process with SIGKILL.
        - SIGXFSZ - Filesystems have a limit to how large a file can be made. When a program tries to violate this limit, the system will send that process the SIGXFSZ signal.
        - SIGVTALRM - SIGVTALRM is sent when CPU time used by the process elapses.
        - SIGPROF - SIGPROF is sent when CPU time used by the process and by the system on behalf of the process elapses.
        - SIGWINCH - When a process is in a terminal that changes its size, the process receives this signal.
        - SIGIO - Alias to SIGPOLL or at least behaves much like SIGPOLL.
        - SIGPWR - Power failures will cause the system to send this signal to processes (if the system is still on).
        - SIGSYS - Processes that give a system call an invalid parameter will receive this signal.
        - SIGRTMIN* - This is a set of signals that varies between systems. They are labeled SIGRTMIN+1, SIGRTMIN+2, SIGRTMIN+3, ......., and so on (usually up to 15). These are user-defined signals; they must be programmed in the Linux kernel's source code. That would require the user to know C/C++.
        - SIGRTMAX* - This is a set of signals that varies between systems. They are labeled SIGRTMAX-1, SIGRTMAX-2, SIGRTMAX-3, ......., and so on (usually up to 14). These are user-defined signals; they must be programmed in the Linux kernel's source code. That would require the user to know C/C++.
        - SIGEMT - Processes receive this signal when an emulator trap occurs.
        - SIGINFO - Terminals may sometimes send status requests to processes. When this happens, processes will also receive this signal.
        - SIGLOST - Processes trying to access locked files will get this signal.
        - SIGPOLL - When a process causes an asynchronous I/O event, that process is sent the SIGPOLL signal.

    - Use `pgrep` command to get the process id of any command.

## Scheduling a Process

Processes in Linux system can be scheduled in following ways:
    - at daemon
    - cron daemon
    - anacron daemon

### Other Infos

- Deamons: A service that runs continously in the background and exists for the purpose of handling periodic requests that the system needs. It forwards the requests to other programs as appropriate.

## References:

- aHR0cHM6Ly95b3V0dS5iZS9rMXNjNF8tZS01VT90PTg4NTk=
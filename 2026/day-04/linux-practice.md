# Practiced on Process commands, Service Commands, Log Commands.
----

## Process Management Commands.
**1. ps** command show you process running on your kernel.:-
* PID :- is the unique processID of process. *
* TTY:- is the type of terminal where user is logged in. pts means pseudo terminal. *
* TIME gives you how long the process has been running. *
* CMD is the command that you run to launch the process. *

| Commands | Description | Example |
| -------- | ----------- | ------- |
| `ps` | It show running process in the current shell | `ps`  |
| `ps -U` | It show information about all processes run by user. | `ps -U username` |
| `pstree` | It show running processs information in tree . | `pstree` |

------

**2. top** command show you the running process in real-time with memory and cpu usage.
**top** command use for monitoring of processes.
* PID: Unique Process ID given to each process. *
* User: Username of the process owner. *
* PR: Priority given to a process while scheduling. *
* NI: ‘nice’ value of a process. *
* VIRT: Amount of virtual memory used by a process. *
* RES: Amount of physical memory used by a process. *
* SHR: Amount of memory shared with other processes. *
* S: state of the process
‘D’ = uninterruptible sleep
‘R’ = running
‘S’ = sleeping
‘T’ = traced or stopped
‘Z’ = zombie *
* %CPU: Percentage of CPU used by the process. *
* %MEM; Percentage of RAM used by the process. *
* TIME+: Total CPU time consumed by the process. *
* Command: Command used to activate the process. *

| Commands | Description | Example |
| -------- | ----------- | ------- |
| `top` | It track the running process. | `top` |

------

**3. kill** command **kill(stop)** your running process.
* it will use with speccific process ID or name of the process. *

| Commands | Description | Example |
| -------- | ----------- | ------- |
| `kill` | It use for stop process in your OS. | `kill 9 1234` |

-------

**4. nice** command use for start new process with priority.
* Priority values range from -20 (highest) to 19 (lowest). *
* it will use with speccific process ID or name of the process. *
* **renice** command use for change priority of running process. *
* it will use with speccific process ID or name of the process. *

| Commands | Description | Example |
| -------- | ----------- | ------- |
| `nice` | It will use for start new process with priority | `nice -n 10 command` |
| `renice` | It will use for change priority of process | `renice -n 5 -p 1234` |

-------

**5. free** command show free and used memory(RAM) on linux.
* **free -m** show you output in MB. *
* **free -g** show you output in GB. *
* **free -h** show you output in readable format. *

| Commands | Description | Example |
| -------- | ----------- | ------- |
| `free` | It show you free and used memory on your system. | `free -h` |

-----

6. **df** command show free hard disk space on your linux system.
* **df -h** show you output in readable format. *

| Commands | Description | Example |
| -------- | ----------- | ------- |
| `df` | It show you free hard disk on your linux system.| `df -h` |

-----

**7. bg** command send process to background.
* **df -h** show you output in readable format. *

| Commands | Description | Example |
| -------- | ----------- | ------- |
| `bg` | It show you free hard disk on your linux system.| `bg` |

-----
**8. fg** command use to run a stopped process in foreground.

| Commands | Description | Example |
| -------- | ----------- | ------- |
| `fg` | It will run process in forground which is stopped.| `fg %id_of_job &` |

-----------

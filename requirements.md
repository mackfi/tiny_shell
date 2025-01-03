1. The prompt is the string "tsh> ".

2. The command line typed by the user consists of a name and zero or more arguments, all separated by one or more spaces.  If the name is a built-in command, then tsh handles it immediately and waits for the next command line.  Otherwise, tsh assumes that the name is the path of an executable file, which it loads and runs in the context of an initial child process.  (In this context, the term "job" refers to this initial child process.)

3. tsh only needs to support two processes piped together (|).  tsh does not need to support file redirection (< and >).

4. Typing ctrl-c or ctrl-z causes a SIGINT or SIGTSTP, respectively, signal to be sent to the current foreground job, as well as any descendants of that job (e.g., any child processes that it forked).  If there is no foreground job, then the signal has no effect.

5. If the command line ends with an ampersand '&', then tsh runs the job in the background.  Otherwise, it runs the job in the foreground.

6. Each job can be identified by either a process ID (PID) or a job ID (JID), which is a positive integer assigned by tsh.  JIDs is denoted on the command line by the prefix ‘%’.  For example, “%5” denotes JID 5, and “5” denotes PID 5.  (Note: The provided code contains all routines needed for manipulating the job list.)

7. tsh supports the following built-in commands:

    * The quit command terminates the shell.  (Do not use SIGQUIT for this, simply exit the shell.)

    * The jobs command lists all background jobs.

    * The bg <job> command restarts <job> by sending it a SIGCONT signal, and then runs it in the background.  The <job> argument must be a JID.

    * The fg <job> command restarts <job> by sending it a SIGCONT signal, and then runs it in the foreground.  The <job> argument must be a JID.
                                           
8. tsh reaps all of its zombie children.  If any job terminates because it receives a signal that it did not catch, then tsh recognizes this event and prints a message with the job’s PID and a description of the offending signal.

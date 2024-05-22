<mark style="background: #FFB86CA6;">A process is an abstraction of a running program</mark>, and it is a key component to create the illusion of running multiple programs simultaneously. A process can be summarized by:

- CPU (registers: program counter, stack pointer, frame pointer)
- Memory (address space)
- I/O information (open files, etc.)

The OS needs to provide some <mark style="background: #FFB86CA6;">capabilities</mark>:

- Create
	- This involves: loading (eager or lazy) code and data, allocating stack and heap, initializing I/O (for example `stdout`, `stderr`, and `stdin`), and starting the program.
- Destroy
- Wait
- Miscellaneous control
- Status

The process can be in one of the following <mark style="background: #FFB86CA6;">states</mark> (or more):

- Running
- Ready
- Blocked (waiting for an event)

The <mark style="background: #FFB86CA6;">process list</mark> contains information about all the processes, and it is composed of <mark style="background: #FFB86CA6;">process control block</mark> that contains information about a specific process.

![[04. Process.pdf]]

The UNIX system has three main system calls for handling processes.

- `fork()`
	- Useful for running the same program.
	- Use the return code of `fork()` to handle the parent and child process.
- `exec()`
	- Useful for running a different program.
	- Hijacks the current process.
- `wait()`
	- Enables some form of determinism for parent and child (i.e. the child process finishes before the parent process).

<mark style="background: #FFB86CA6;">Signals</mark> are used to control (stop, continue, terminate, etc.) processes. The notion of <mark style="background: #FFB86CA6;">users</mark> allow us to limit who (user) can control what (process). The <mark style="background: #FFB86CA6;">superuser</mark> is a special type of user that can control all processes.

![[05. Process API.pdf]]

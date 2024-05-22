>[!question] How do you efficiently virtualize the CPU while maintaining control?
>By using limited direct execution—which is just a fancy way of saying just run the program directly on the CPU, but making sure that the hardware is able to limit what a process can do without OS intervention.

But first, we need to ask two questions.

1. How can the OS make sure the program doesn't do anything that we don't want it to do?
2. How does the OS stop the program from running and switching to another process (i.e. implementing time sharing)?

The answer to the first question is restricted operations through <mark style="background: #FFB86CA6;">user and kernel mode</mark> through the use of the <mark style="background: #FFB86CA6;">trap (and return-from-trap) instruction</mark>.

The second question can be answered by using a non-cooperative approach with an <mark style="background: #FFB86CA6;">interrupt timer and handler</mark>. Once the OS has been passed the baton, the scheduler will decide if a different process should run (which in turn will require a context switch if the scheduler does decide that it is another process' turn to run).

![[06. Direct Execution.pdf]]

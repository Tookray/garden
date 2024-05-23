There are two scheduling metrics to be aware of.

1. <mark style="background: #FFB86CA6;">Turnaround time</mark>

$$
T_{turnaround} = T_{completion} - T_{arrival}
$$

2. <mark style="background: #FFB86CA6;">Response time</mark>

$$
T_{response} = T_{firstrun} - T_{arrival}
$$

Assumptions.

1. Each job runs for the same amount of time.
2. All jobs arrive at the same time.
	- <mark style="background: #FFB86CA6;">Convoy effect</mark> (large job followed by small jobs).
3. Once started, each job runs to completion.
	- <mark style="background: #FFB86CA6;">(Non-)preemptive</mark> scheduler.
4. All jobs only use the CPU (i.e. they perform no I/O).
	- <mark style="background: #FFB86CA6;">Overlap</mark> operations to maximize utilization by treating sub-jobs as an independent job.
5. The run-time of each job is unknown.

Types of schedulers.

- First in, first out
- Shortest job first (SJF)
- Shortest time-to-completion first (STCF)
	- Good <mark style="background: #BBFABBA6;">turnaround time</mark> but poor <mark style="background: #FF5582A6;">response time</mark>.
- Round-robin (RR)
	- Poor <mark style="background: #FF5582A6;">turnaround time</mark> (due to issues arising from context switching like cache misses) but good <mark style="background: #BBFABBA6;">response time</mark> (fair).

In general, a policy that is fair (like round-robin) will perform poorly on metrics like turnaround time. On the other hand, if you are willing to be unfair, you can run shorter jobs to completion, but at the cost of response time. It is an inherent trade-off. You can't have your cake and eat it too.

![[07. CPU Scheduling.pdf]]

The <mark style="background: #FFB86CA6;">multi-level feedback queue</mark> (MLFQ) scheduler fixes the problems (trade-off of turnaround and response time) of the schedulers above. It doesn't need to have a priori knowledge about the job's nature (whether it's a short-running interactive or long-running CPU intensive), it observes its execution and prioritizes it accordingly. Amazingly enough, it is the best of both worlds.

The MLFQ scheduler must adhere to the following five rules:

1. If $Priority(A) > Priority(B)$, then $A$ runs.
2. If $Priority(A) = Priority(B)$, then $A$ and $B$ run in RR fashion using the time slice of the given queue
	- The time slice length can be variable depending on the priority-level of the queue, generally lower priority queues have longer time slices whereas higher priority queues have shorter time slices.
3. When a job enters the system, it is placed at the highest priority.
4. Once a job uses up its time allotment at a given level (regardless of how many times it has given up the CPU), its priority is reduced.
5. After some time period $S$, move all the jobs in the system to the topmost queue.

Tuning the parameters is *difficult*, to say the very least. For "hard-coded" values, take a look at the default values of Solaris MLFQ implementation. For a more dynamic behaviour, take a look at decay-usage algorithms and their properties (<mark style="background: #ADCCFFA6;">look into this</mark>).

>[!question] Is control theory applicable for choosing priority levels?
>I'm not too sure about this, but this seems like a very interesting topic to explore.

Another note is that the system should allow some user advice (hints) to help set priorities. Also, consider reserving the highest level priority levels for OS work (<mark style="background: #ADCCFFA6;">look into this</mark>).

![[08. Multi-level Feedback.pdf]]

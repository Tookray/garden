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
- Shortest job first
- Shortest time-to-completion first
	- Good <mark style="background: #BBFABBA6;">turnaround time</mark> but poor <mark style="background: #FF5582A6;">response time</mark>.
- Round-robin (time-slicing)
	- Poor <mark style="background: #FF5582A6;">turnaround time</mark> (due to issues arising from context switching like cache misses) but good <mark style="background: #BBFABBA6;">response time</mark> (fair).

In general, a policy that is fair (like round-robin) will perform poorly on metrics like turnaround time. On the other hand, if you are willing to be unfair, you can run shorter jobs to completion, but at the cost of response time. It is an inherent trade-off. You can't have your cake and eat it too.

![[07. CPU Scheduling.pdf]]

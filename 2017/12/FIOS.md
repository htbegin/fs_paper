# content

* fair timeslice mangement
    * allow timeslice fragmentation
    * concurrent request issuance from multiple tasks
* read preference combined with write blocking
    * write starvation

# 3 most important things

* during concurrent access, writes can dramatically affect the response time of
  read requests
* support for parallel IO
* the lack of seek overhead eliminates the performance benefit of anticipatory I/P,
  but proper I/O anticipation is still needed for the purpose of fairness.

# conclusion

* design a new Flash I/O scheduling approach to ensure fairness with high efficiency
    * timeslice management that allows timeslice fragmentation and concurrent request issuance
    * read/write interference management
    * I/O parallelism
    * I/O anticipation for fairness

# quora

```
While I/O anticipation was proposed as a performance-enhancing
seek-reduction technique for mechanical disks [17], its role
for maintaining fairness has been largely ignored.
```

```
Under this approach, a read is only blocked by a write when
the read arrives at the I/O scheduler after the write has already
been issued. This is due to the non-preemptibility of I/O
```

```
On mechanical disks, I/O anticipation can substantially improve the I/O
efficiency by reducing the seek and rotation overhead. In contrast, I/O
spatial proximity has much less benefic for Flash storage. Therefore
I/O anticipation has a negative performance effect and it must be
used judiciously for the purpose of maintaining fairness.
```

```
or predictor of the application inter-I/O thinktime. For
seek-reduction on mechanical disks, the I/O anticipation
bound is set to roughly the time of a disk I/O operation
which leads to competitive performance compared to the
optimal offline I/O anticipation
```


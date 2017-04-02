
# Scheduling Algorithms for Modern Disk Drives

* reduce positioning time (seek and rotation delay)
* improve starvation resistance

## content

how the features of modern disk affect the performance of various scheduling algorithms
* the mapping of logical blocks to phyiscal media is unavailable
* large prefetching caches

more realistic workload in addition to synthetic workloads
* request starting locations are uniformly distributed across the entire disk
* posion distribution ?

C-LOOK
* C-SCAN treas each cylinder equally
    * when the arm reaches the last cylinder, a full-stroke seek returns it to the
      first cylinder without servicing any requests along the way
* LOOK changes the scanning direction if no pending request exist in the current
  direction of travel
* design to reduce the variance of response time

seek delay reduction
* seek to the cylinder

positioning delay reduction
* rotation to the sector

evaluation of seek-reducing algorithms
* average response time
* the squared coefficient of variation

ASPCTF (Aged Shortest Positioning with Cache Time First)
* SPTF(Shortest Positioning Time First): seek and rotational latency
    *  SPTF is highly susceptible to request starvation
* ASPTF (Aged Shorted Positioning Time First)
    * position delay prediction Tpos
    * effective position delay: Teff = Tpos - (W * Twait)
* estimate a positioing time of zero for any request that can be
  satified (at least partially) from the cache

## 3 most import things

* the full knowledge of the mapping of logic block to physical block doesn't help to
  decrease the response time

* utilizing prefetching disk cache provides significant performance improvement for
  workloads with read sequentiality
    * C-LOOK (cyclical scan algorithm) performances best
        * schedule requests in ascending logic order

* utilize prefetching cache and reduce overall positioning delay produce
  the highest performance

## quote

```
A signicant portion of request service time may be comprised
of mechanical positioning delays, which are highly
dependent on the relative positions of the various request
blocks and the current disk arm position.
```

* performance
```
system manufacturers. The host or intermediate controller
presents a request to the disk drive in terms of the starting
logical block number (LBN) and total request size. The
details of the subsequent media access are typically hidden
from the host. This approach ooads most of the man-
agement overhead associated with the actual data storage
from the host or controller to the disk drive electronics. On
the other hand, scheduling entities outside of the drive it-
self often have little or no knowledge of the data layout on
the media, the status of any on-board disk cache, and the
various overhead delays associated with each request.
```

```
The complexity of the mapping is increased by zoned recordin,
track/cylinder skew, and defect management.
```

```
We conclude that maintaining a full LBN-to-PBN map-
ping is not justied for seek-reducing algorithms when
scheduling random workloads.
```

```
pings. We conclude, as was the case for random workloads,
that the complexity of maintaining a full LBN-to-PBN map-
ping is not justied for seek-reducing algorithms.
```

```
We believe that a modied version of ASPCTF which
specically schedules sequential requests in ascending logical
order will increase performance beyond C-LOOK in all cases.
This represents another item for future research.
```

```
(STF) in [Selt90] and Shortest Access Time First (SATF) in
[Jaco91], but we use the term Shortest Positioning Time
First (SPTF) to clarify the exact focus of the algorithm.
SPTF, like SSTF, suers from poor starvation resis-
tance. To reduce response time variance, priority can be
given to requests that have been in the pending queue for
excessive periods of time. The priority may slowly increase
as the request ages, or a time limit may be set after which
requests are given a higher priority [Selt90, Jaco91].
```

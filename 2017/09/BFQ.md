
=content

High Throughput Disk Scheduling with Fair Bandwidth Distribution

Budge Fiar Queuing

work-conserving scheduler: throughput
timestamp-based scheduler: guarantee


=3 most important things

* disk idling for high throughput
* back-shifting of request timestamps: enforce guarantee
* hanlde deceptive idleness and delayed arrivals
* based on C-LOOK and WF2Q+
=single deficiency

=conclusion

* most mainstream applications usually issue one request or batch
  of requests at a time

* scheduler category
    * real-time scheduler
        * definition
            >time-stamp-based scheduler that associate a deadline to each request.
            >They usually start from an Earliest Deadline First schedule, and reorder
            >requests to reduce seek and rotational latency without vioalting deadline.
        * disadvantage
            * throughput boosting
            * deadline ?
    * proportional share timestamp-based scheduler
    * proportional share round-robin scheduler 

* disk internal queuing and idling
    * ssd ?

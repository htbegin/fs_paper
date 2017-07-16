# Split-Level I/O Scheduling

## the 3 most important things

* split I/O scheduling logic across handlers at three layers of the storage stack:
  block, system call, and page cache
* throught, latency, isolation goals: priority-misallocation ? tail latency ?
  sensitivity to interference ?

## I/O scheduler

* three kinds of scheduler: priority/deadline/resource-limit
* three common needs:
    * map causes to identify which process is repsonsible for an I/O request
    * estimate costs in order to optimize performance and peform accounting properly
    * reorder I/O requests so that the operations most important to archiev scheduling goal
      are served first.

## cause mapping

metadata is usually shared, and I/Os are usually batched, so there may be multiple causes
for a single dirty page or a single request. Thus, the split framework tags I/O operations
with sets of causes, instead of simple scalar tags.



# quote

>Unfortuantely, making decision at the block level is problematic, for two reasons.
>First, and most importantly, the block-level scheduler fundamentally cannot reorder
>certain write requests; file systems carefully control write ordering to preserve
>consistency in the event of system crash or power loss. Second the block-level
>scheduler cannot performance accurate accouting; the scheduler lacks the requisite
>information to determinate wich application was responsible for a particular I/O request.

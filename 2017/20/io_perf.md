# Enlightening the I/O Path: A Holistic Approach for Application Performance

## request-centric I/O prioritytization (RCP)

* dynamically detects and prioritizes I/Os delaying request handling at all layers
    * I/O priority inversion: statically deciding the priority of an I/O is insufficent
    * multiple independent layers: holistically considered

### twofold goals

* minimizing application modification for detecting critical I/O
* processing background tasks in a best-effort manner while minimizing the interference
to foreground tasks


## the 3 most important things

* the holistical way
    * application
        * enlightenment API
    * the cache layer
        * separated dirty ratio
    * the file system layer
    * the block layer
        * separated the queue slots
        * priority-based I/O scheduler
    * storage device
        * limit the number of non-critical I/Os dispatched

* I/O priority inversion
    * a task dependency
        * I/O priority inheritance
            * kernel level
            * user level
    * an I/O dependency
        * red-black tree for request status
    * transitive dependency
        * blocking cause and related solution

* I/O classification
    * performance critical I/Os
        * in the cirtical path of request handling since the response time of a request
          determinates the level of application performance.
    * non-critical I/Os

## content

* two types of dependencies cause I/O priority inversion
    * a task dependency
        * two tasks interact with each other via synchronization primitives
            * fsync() by a foreground task and jbd2 thread
    * an I/O dependency
        * between a task and an outstanding I/O
            * directly wait for the completion of an ongoing I/O in order to
              ensure correctness and/or durability

* resolving I/O priority inversion is challenging
    * dependency relationships can not be statically determinated since
      they depend on various runtime conditions
    * dependency occurs in a transitive manner involving multiple concurrent
      tasks blocked at either synchronization primitives or various I/O stages
      in mulitple layers
    * a dependency relationship might not be visible at the kernel-level because
      of the extensive use of user-level synchronizations based on kernel-level
      supports

## quote

>First, the existing schemes do not fully consider multiple independent layers
including caching, file system, and block lay- ers in modern storage stacks.
Prioritizing I/Os only in one or two layers of the I/O path cannot achieve
proper I/O prioritization for foreground tasks.

>Second and more importantly, the existing schemes do not address the I/O
priority inversion problem caused by runtime dependencies among
concurrent tasks and I/Os. I/O priority inversions can occur across
different I/O stages in multiple layers due to transitive dependencies.

>classify I/Os into two priority levels: (performance) critical and non-critical I/Os.
In particular, we define a critical I/O as an I/O in the critical path of request handling
since the response time of a request determines the level of application performance.

>To deal with the condition varible-induced dependency, we borrow a solution in previous work [27].
In particular, a task is registered as a helper task [27] into the corresponding object of a condition
variable when the task is going to signal the condition variable. Later, the helper task inherits I/O
priority of a task blocked to wait for the condition become true. The inherited I/O priority is revoked
when the helper task finally signals the waiting task.

>We registered jbd2 kernel thread as a helper task for the condition variables `j_wait_transcation_locked`
and `j_wait_done_commit`. For the condition variable `j_wait_updates`, a helper task is dynamically assigned
and removed since a helper task can be any task having a user context.

>For tasks issuing non-critical writes, the applied dirty ratio is low (1% by default) to mitigate the
interference to foreground tasks.

>Moreover, a foreground task calling fsync() does not need to depend on a large amount of dirty data
generated by background tasks resulting from the file system ordering requirement [42, 43].

>However, if τn is blocked at one of the admission stages, inheriting the critical I/O priority
is insufficient because the cause of the blocking should also be resolved.

>In order to resolve the transitive dependencies, we record a blocking status into the descriptor of
a task when the task is about to be blocked. A blocking status consists of blocking cause and an object
to resolve the cause. Blocking cause can be one of task dependency, I/O dependency, and blocking at
admission control stage.

>For RCP, we configured 1% dirty ratio for non-critical writes and 20% dirty ratio
(the default ratio in Linux) for critical writes. We separately allocated 128 slots
for each critical and non-critical block-level queues. The number of non-critical
I/Os outstanding to a storage device is limited to one, and the timeout for non-critical
I/Os is set to 10 ms to prevent starvation at the block-level queue.

>SPLIT-A and -D are effective in resolving the file system-induced dependencies by scheduling
writes at the system-call layer. However, SPLIT-A causes over one second latency at `write_entry`
and `fsync_entry` because it prevents foreground tasks from fully utilizing the OS buffer cache.
SPLIT-D also cuases about one second latency at inode mutext (`mutex_lock`) due to I/O priority
inversion.

>SPLIT-A and SPLIT-D deteriorate application performance because SPLIT-A does not fully utilize write
buffer in the caching layer and SPLIT-D does not protect non-critical writes from filesystem level
entanglement.

>we selectively disabled one of the components in our scheme, the caching layer, the block layer,
the kernel-level dependency handling, the user-level dependency handling, and the transitive
dependency handling.

>However, the improvement in system throughput comes with the degraded latency of foreground I/O.
In particular, the average latency of foreground I/O gradu- ally increases from 110 us to 1771 us
(over 16× slow down).

>Our prototype implementation involves modifications to all the layers and the synchronization methods
in the I/O path, thereby hindering our scheme from wide adoption. The most promising direction to be
pratically viable is exploiting the Split framework. It provides a collection of handler for an I/O scheduler
to operate acrss all layers in the I/O path. We believe our scheme can be cleanly implemented based on
the framework by controlling non-critical writes at the system-call level, before the caching
and file system layer generates complex dependencies, and non-critical reads at the block-level.

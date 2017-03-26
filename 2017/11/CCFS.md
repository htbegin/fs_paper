# content

* ordering and atomicity

* false ordering dependency
    * global ordering is unnecessary
        * examples: ext4 under data-journaling mode (?)
    * order within a stream
        * within an application

* fsync
    * for durability
    * for ordering: wrong usage of fsync

* ordering and performance

* crash consistency filesystem
    * one transcation per stream

* weak atomicity
    * one sector overwrite
    * append content ?
    * many sectors overwrites
    * directory operation
* ordering
    * overwrite: any op
    * append: any op
    * dir op: any op 
    * append -> rename

# 3 most import things

* two guarantees
    * order: the effect of system calls should be persisted on disk
      in the order they were issued by application.
    * weak atomicity: the effect of the system call should be atomic
      across a system crash
        * for directory opreations (including the creation, deletion, and
          renaming of files and hard links)
        * write to files at sector granularity and append write

* two challenges:
    * metadata structs and the journaling mechanism need to be separated between streams
        * independent structes shareing the same block: hybrid-granularity journaling
        * metadata structes are shared between different streams: delta journaling
        * pointer-less structes
        * order-less space reuse
    * order needs to be maintained with each stream efficiently

* false dependencies are rare and have little impact for common workloads

# conclusion

* improve correctness: order and weak atomicity
* maintain and sometimes improve performance

# quota

```
“Vulnerability”: Place in application source code that can lead to
inconsistency, depending on FS behavior
```

```
1. Both streams update directory’s modification date
-
 Solution: Delta journaling
2. Directory entries contain pointers to adjoining entry
-
 Solution: Pointer-less data structures
3. Directory entry freed by stream A can be reused by stream B
-
 Solution: Order-less space reuse
4. Ordering technique: Data journaling cost
-
 Solution: Selective data journaling [Chidambaram et al., SOSP 2013]
5. Ordering technique: Delayed allocation requires re-ordering
-
 Solution: Order-preserving delayed allocation
```

```
SQLite can also be heavily optimized when running atop ccfs by
omitting unnecessary fsync calls; with our workload, this results
in a 685x improvement
```

# question

* what about XFS ? Does it do the same thing ?
```
Ext4 already handles the situation for block allocation (for reasons of
security) by resuing blocks only after the transcation that frees those
blocks has fully committed.
```

# Reference

FAST17 

# NOVA: A Log-structured File System for Hybrid Volatile/Non-volatile Main Memories

## the 3 most important things


## low overhead / high performance

* assign each inode a seperate log to maximize concurrency during normal operation and recovery
* store the logs as linked list, so they do not need to be contiguous in memory
* use atomic updates to a log's tail pointer to provide atomic log append
* use lightweight journaling for operations that span multiple inodes
* does not log data to accelerate the recovery process and reduce gargage collection overhead

* atomic mmap

## strong consistency guarantee

log-structured file system

## design decision

* keep logs and file data in NVMM and builds radix trees in DRAM to quickly perform search operations,
  making the in-NVMM data structures simple and efficient.
* Each inode has its own log, allowing concurrent updates across files without synchronization
* use logging and lightweight journaling for complex atomic updates
* implement the log as a singly linked list
* do not log file data. NOVA uses copy-on-write for modified pages and appends metadata about the write
  to the log. The metadata describe the update and point to the data pages.

## quotes

>we designed NOVA based on three observations
>logs that support atomic updates are easy to implement correctly in NVMM, but they
are not efficient for search operations. Conversely, data structures that support fast
search are more difficult to implement correctly and efficiently in NVMM.
>the complexity of cleaning logs stems primarily from the need to supply contiguous free
regoins of storage, but this is not necessary in NVMM, because random access is cheap.
>using a single log make sense for disks (where there is a single disk head and improving spatial
locality is paramount), but it limits concurrency. In NVMMs using multiple logs doesn't negatively
impact performance

>To atomically write data to a log, NOVA first appends data to the log and then atomically updates
the log tail to commit the updates, thus avoiding both the duplicate writes overhead of journaling
file systems and the cascading update costs of shadow paging systems.

>Some directory operations, such as a move between directories, span multiple inodes and NOVA uses
journaling to atomically update multiple logs. NOVA first writes data at the end of each inode’s log,
and then journals the log tail updates to update them atomically. NOVA journaling is lightweight
since it only involves log tails (as opposed to file data or metadata) and no POSIX file operation
operates on more than four inodes.

>NOVA relies on three write or- dering rules to ensure consistency. First, it commits data
and log entries to NVMM before updating the log tail. Second, it commits journal data to NVMM
before propagating the updates. Third, it commits new versions of data pages to NVMM before
recycling the stale versions


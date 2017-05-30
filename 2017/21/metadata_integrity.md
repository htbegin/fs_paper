# High-Performance Metadata Integrity Protection in the WAFL Copy-on-Write File System

## incremental checksum for in-memory scribble preventation

* low-cost

## transaction auditing mechanism for fs consistency invariants

* lightweight digest-based
* obviate the need to cache the unmodified metadata blocks
* the cost of recording the changes is proportional to the actual changes rather than
  to the number of modified blocks.

## page protection pin-points the root cause of software-caused scribbles

* low-overhead
* global write-protect control

## in-memory corruptions of metadata

* logic bugs in metadata computation and updates
* scribbles of metadata blocks that are used as input for new metadata computation
* scribbles of metadata blocks before they are written to persistent storage

## two categories of consistency invariants

* local: confine to a given metadata block
* distributed: relationship across several blocks from different metadata

## tool to inject corruption into in-memory data structure

## the 3 most important things

## quote

>Data at rest is protected from media failures by using detection techniques
such as checksums and scrubbing, and by using recovery techinques such as
redundancy. File system crash consistency is provided by techniques such as
journaling, shadow paging, or soft updates. However, memory scribbles that
are caused by software bugs or by hardware failures, or logic bugs that
are in the file system code can still introduce metadata inconsistencies
in the write path.

>However, this scheme does not perform well if metadata is modified frequently.
The frequent switching of page permissions (read-only to read/write to read-only)
not only causes a flood of TLB flushes, but also creates a storm of inter-processor
TLB invalidation interrupts.

>Furthermore, none of these techinques can detect a corruption that is caused by hardware
problems.

>Two types of problems can cause file system recovery runs: (1) inconsistencies due to
in-memory corruptions of metadata in the write path, and (2) inconsistencies due to the
loss of metadata because of media failures that are beyond the redundancy threshold of
the underlying RAID mechanisum. This paper addresses problem 1.

>Upon detection of a scribble, we abort the ongoing transaction commit, preventing the
corruption from being persisted. To protect the ONTAP node from any other potential
corruptions from the same bug, we reboot the node instead of aborting an individual transaction.
Becuase ONTAP is configured in high-availability pairs, the partner node takes ownership of the
rebooted nodes' file system, and those file system are all still consisten because they are defined
by their mosts recently completed transaction.


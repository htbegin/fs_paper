# End-to-end Data Integrity for File Systems: A ZFS Case Study

## content

### disk corruption

* definition

define disk corruption as a state when any data accessed from
disk does not have the expected contents due to some problem
in the storage stack.

* solution
    * checksums
    * redundancy
    * RAID storage

### meomry corruption

* definition
define memory corruption as the state when
the contents accessed from the main memory have one or
more bits changed from the expected value.

* solution
    * ECC
    * programming models and tools


## the 3 most important things

## quote

>Through careful and thorough fault injection, we show that ZFS is robus
to a wide range of disk faults. We further demonstrate that ZFS is less
resilient to memory corruption, which can lead to corrupt data being
returned to applications or system crashes.

>Unfortunately, the effects of memory corruption on data integrity have been
largely ignored in file system design. Hardware-based memory corruption occurs
as both transient soft errors and repeatable hard errors due to a variety of
radiation mechanisms.


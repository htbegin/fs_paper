=content

* COW vs WAFL ?
    * WAFL is a COW filesystem hat is especially suited for NFS and CIFS worloads.

* file system challenge
    * scalability
    * data integrity
    * disk diversity


* defragmentation in the presence of snapshot

* the traceoffs required to maintain even performance in the face of a wide spectrum of workloads

* what is updated in place
    * only superblock

* the modification ripple up the sub-volumn tree until its root

* multiple device support: DM handles checksum error ?

=3 most important things

* a filesystem supporting important new features, while providing reasonable performance under most workloads.
    * data checksum
    * writeable snapshots
    * multi-device support
    * online resize and defragmentation
    * compression

* the main worload effect is to make writes more sequential, and reads more random
    * the same as log structured filesystem

* COW friendly b-tree


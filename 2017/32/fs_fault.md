# Redundancy Does Not Imply Fault Tolerance: Analysis of Distributed Storage Reactions to Single Errors and Corruptions


## the 3 most important things

* data integrity strategy
    * metadata checksum
    * data checksum
    * background scrubbing
    * external repair tools
    * snapshot redundancy

* why distributed system are not tolerent of single file-system fault
    * they expect the underlying storage stack layers to reliably store data
    * recovery code is not rigorously tested, contributing to underisable behaviors
    * the existing research work has not been used by commodity distributed storage system to
      tolerate faults other than crashes
    * redundancy remain underutilized as a source of recovery from file-system and other partial faults

* effectivly use redundancy
    * corrupted or inaccessible parts of data can be indentified: carefully design the on-disk data structures
    * corruption recovery has to be decoupled from crash recovery to fix only the corrupted or inaccessible portion of data

* research work on tackling partial faults
    * Detection and Correction of Silent Data Corruption for Large-scale High-performance Computing
    * Understanding Latent Sector Errors and How to Protect Again Them
    * Ensuring Data Integrity in Storage: Techinques and Applications
    * Dealing with Server Corruption in Weakly Consistent Replicated Data Systems
    * Robustness in the Salus Scalable Block Store
    * Enhaching Data Availability in Disk Drives through Backgroud Activities
    * Fusion-IO Data Integrity
    * Preventing Data Corruption with HARD
    * Pratical Hardening of Crash-Tolerant Systems
    * XFT: Practical Fault Tolerance Beyond Crashes
    * End-to-end Data Integrity for File Sytems: A ZFS Case Study

## concept

how modern distributed storage system react to file-system faults

* faults are often undetected locally, and even if detected, crashing is
  the most common reaction

* crash and corruption handling are entangled
  systems often conflate recovering from a crash with recovering from corruption

## partial storage fault

* certain blocks of data become inaccessible (read and write errors)
* data is silently corrupted

## file-system faults

* faults thrown by the file system to its application


# quotes

>The most important overarching lesson from our study is this:
a single file-system fault can induce catastrophic outcomes in
most modern distributed storage systems.  Despite the presence
of checksums, redundancy, and other resiliency methods prevalent
int distributed storage, a single untimely file-system fault can
lead to data loss, corruption, unavailability, and, in some cases,
the spread of corruption to other intact replicas

>nuances in commonly used distributed protocols can spread corruption
or data loss

>Contrary to the widespread expectation that redundancy in distributed
systems can help recover from single faults, we observe that event a single
error or corruption can cause adverse cluster-wide problems such as total
unavailability, silent corruption, and loss or inaccessibility of inordinate
amount of data.

>We find that detection and recovery code of many systems often inadvertently
try to detect and fix two fundamentally distinct problem: crashes and data corruption.

>The underlying reason for this problem is the inability to differentiate corruptions
due to crashes from real storage stack corruptions


# Evolving Ext4 for Shingled Disks 

## the 3 most important things

* Drive-Managed SMR (Shingled Magnetic Recording)

* metadata separation idea

### phyiscal journaling vs logical journaling


## quote
>Unlike CMR disks that have a low but consistent througput under random writes,
DM-SMR disks offer high throughput for a short period followd by a precipitous
drop, as shown in Figure 1.

>Ext4-lazy, on the other hand, marks the block as clean after writing it to
the journal, to prevent the writeback, and inserts a mapping (S,J) to an in-memory
map allowing the filesystem to access the block in the journal. Ext4-lazy uses a
large journal so that it can continue writing updated blocks while reclaiming the
space from the stale blocks. During mount, it reconstructs the in-memmory map from
the journal resulting in a modest increase in mount time.


>We show that for physical journaling, a small journal is a bottleneck for
metadata-heavy workloads.

>SMR leverages the differences in the width of the disk's write head and read head
to squeeze more tracks into the same area then CMR.

>Seagate disks are known to clean during idle times and to have static mapping.
Therefore, they have high througput while the persistent cache is not full, and
ultra-low throughput after it fills. Western Digital disks, on the other hand,
are likey to clean constantly and have dynamic mapping. Therefore, they have
lower througput than Segate disk while the persistent cache is not full, but
higher througput after it fills.

>A committed transcation becomes check-pointed when every metadata block in it is
either written back to its static location due to a dirty timer expiration, or
it is written to the journal as part of a newer transcation

>Ext4-lazy also benefits from skipping metadata writeback, but most of the improvement
comes from eliminating long seeks for metadata reads.

>The significant difference in the volumn of journal writes between ext4-baseline and
ext4-lazy seen in Talbe 2 is caused by metadata write coalescing: since ext4-lazy completes
faster, there are more operations in each transcation, with many modifying the same metadata
blocks, each of which is only written once to the journal (delayed logging)

>unlike CMR disks, where ext4-lazy had a big impact on just metadata-heavy workloads, on DM-SMR
disks it providies significant improvement on both, metadata-heavy and metadata-light workloads.

>The rationale for introducing cylinder groups in FFS, which manifest themselves as block groups
in ext4, was to create clusters of inodes that are spread over the disk close to the blocks that
they references, to avoid long seeks between an inode and its associated data.

>Ext4-lazy, however, puts hot metadata in the journal located at the center of the disk, requiring
a half-seek to read a file in the worst case

>Similar to these file systems ext4-lazy separates metadata and data, but unlike them it does not confine
metadata to a log - it uses a hybrid design whete metadata can mirgrate bacn and forth between file system
and log as needed....showinng that a journaling file system can benefit from the metadata sepration idea
with a small set of changes that does not require on-disk format changes.

>It also suggests that while three decades ago it was wise for file systems depending on the block interface
to scatter the metadata across the disk, today, with large memory sizes that cache metadata data and with
changing recording technology, putting metadata at the center of the disk and managing it as a log looks like
a better choice.

>file systems should work to eliminate structures that induce small isolated writes, especially if the user
workload is not forcing them
>When random writes are unavoidable, file systems can reduce their cost by confine them to the smallest perimeter
possible


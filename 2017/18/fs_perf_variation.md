# On the Performance Variation in Modern Storage Stacks

## content

## the 3 most important things

* Performance variation is a complex issue
It could be caused and affected by various factors: the file system,
configurations of the storage system, the running workload,
or even the time window for quantifying the performance.

* Non-linearity is inherent in complex storage systems.
It could lead to large differences in results, even in well-controlled
experiments; conclusions drawn from these could be misleading or even wrong.

* Disable all lazy initialization and any background activities
while formatting, mounting, and experimenting on file systems, disable them

## quote
>HDD configurations are generally slower in terms of throughput but show
a higher variation, compared with SSDs.

>However, block allocation is not a deterministic procedure in Ext4 [18].
Even given the same distribution of file sizes and directory width,
and also the same number of files as defined by Filebench,
multiple trials of dataset creation on a freshly formatted,
clean file system did not guarantee to allocate blocks from
the same or even near physical locations on the hard disk.

>XFS had the least amount of variation among the three file systems,
and is fairly stable in most cases, as others have reported before,
albeit with respect to tail latencies [18]
Reducing file sys- tem tail latencies with Chopper.

>This suggests that XFS-HDD configurations might exhibit high variations
with shorter time windows, but produces more stable results in longer
windows. It also indicates that the choice of window sizes matters
when discussing performance variations.

>For most experiments, no matter what the file system type is,
performance starts slow and climbs up quickly in the beginning
phase of experiments. This is because initially the application
is reading cold data and metadata from physical devices into the caches;
once cached, performance improves.  Also, for some period of time,
dirty data is kept in the cache and not yet flushed to stable media,
delaying any impending slow writes.

>With the empirical data that we collected—per-operation latency
distributions and throughput—we were able to dis- cover correlations
between the speed of individual operations and the throughput.

>We then computed the Pearson Correlation Coefficient (PCC) between
the relative range of throughput and the range of latency variation.

>Although such correlations do not always imply direct causality,
we still feel that this correlation analysis sheds light on how
each operation type might contribute to the overall performance
variation in storage stacks.

>Our analysis revealed that block allocation is a major cause of
performance variation in Ext4-HDD configurations. From the temporal
view, the magnitude of throughput variation also depends on the
window size and changes over time. Latency distribution for
the same operation type could also vary even over repeated
runs of the same experiment.



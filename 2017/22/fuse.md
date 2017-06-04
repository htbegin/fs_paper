# To FUSE or Not to FUSE: Performance of User-Space File Systems

## the popularity of user-space file system
* stackable fs over existing fs
* quick experimentation and prototyping of new approaches
* existed user space fs

## two trade-off factors

* how large is the performance overhead caused by a user-space implementation
* how much easier is it to develop in user space

## fuse optimizations

* writeback cache
* splicing for zero copy
* multithreading for daemon

## the 3 most important things

* visibly worse for metadata-intensive worloads

## quote

>We introduced a two-dimensional array where a row index represents the request type
and the column index represents the time. Every cell in the array stores the number
of requests of a corresponding type that were processed within the 2^(N+1) ~ 2^(N+2)
nanoseconds where N is the column index.


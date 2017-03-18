# content

* ordering and atomicity

* false ordering dependency
    * global ordering is unnecessary
        * examples: ext4 under data-journaling mode (?)
    * order within a stream
        * within an application
        * ?

* fsync
    * for durability
    * for ordering ??

* ordering and performance

* crash consistency filesystem
    * one transcation per stream

* challenge
    * block level journaling ?

* atomicity
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

# conclusion

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
# Reference

FAST17 

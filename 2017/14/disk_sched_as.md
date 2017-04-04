# Anticipatory scheduling

a disk scheduling framework to overcome deceptive idleness in synchronous I/O

## content

* work-conserving schedule
    * schedule a request as soon as the previous request has finished
    * select a request for service as soon as (or before) the previous request has completed

* deceptive idleness
    * assume the last request issuing process has no further requests, and
      become forced to switch to a request from another process

* based on non-work-conserving scheduling discipline
    * ?

* spatial locality
    * read/write to a nearby position

* temporal locality
    * ?

* prefetch
>and issuing its immediately forthcoming request before the
current one completes. Each process thus maintains mul-
tiple outstanding requests at decision points, and gives the
scheduler the chance to service consecutive requests from the
same process. Seek reduction opportunities can therefore

    * access pattern is hard to predict accurately

* decay factor
    * forge 95% of the old positioning time value after ten requests

## 3 most important things

* peformance improvement
    * seek reduction

* little overhead
    * ?

* proportional-share schedulers become more accurate and efficient
    * ?

* anticipation heuristics

* complement prefetching techniques ?
## quote

```
needs. This solution complements prefetching techniques
deployed at the application and kernel levels, and is most
useful in frequently occurring situations where prefetching
is difficult or infeasible. It is easy to implement, and suited
for incorporation into general-purpose operating systems.
```

```
The underlying problem
In both examples, the scheduler reorders the available re-
quests according to its scheduling policy, but fails to meet
overall objectives of performance and quality of service. In
essence, processes that issue synchronous requests cause the
work-conserving disk scheduler to receive no requests from
1that process, in time for the following decision point. This
leads to deceptive idleness, rendering the scheduler inca-
pable of exploiting spatial and temporal locality among syn-
chronous requests.
```

# Appendices and Further Reading

This page contains a variety of short appendices; If a given section becomes too
large, we should consider expanding it into a full chapter.

## Telemetry Resolution

In our discussion of telemetry signals, we focus on logs, metrics, and traces to
the exclusion of other types. Other authors in this space will often add a
fourth or fifth, such as 'profiles' or 'events'.

The reason we focus on these three is because they generally sit at the 'sweet
spot' of resolution to make it easy enough to understand the overall state of
transaction and resource health in aggregate, while preserving enough resolution
to 'zoom in' to a single fault (or a group of like faults) with sufficient
metadata to help operators understand why that fault is happening. This is
roughly what you need in order to set and monitor SLOs, or to generate
hypotheses for further debugging.

As a specific example, profiling is a useful way to inspect line or
function-level detail about a given process. Profile dumps of service code in
production can be invaluable in understanding memory usage, garbage collection,
or logic errors. The ability for engineers to generate and collect these dumps
is an important part of a resilient system. That said, it's not a pre-requisite
for _observability_.

<!-- markdownlint-disable line-length-->
## It's Only A Trace If It's From The Dapper Region Of Google, Otherwise It's Just Sparkling Logs With Context
<!-- markdownlint-enable line-length-->

<!-- TODO: I just like this header -->
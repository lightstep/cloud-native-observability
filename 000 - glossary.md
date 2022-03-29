# Glossary of Terms

This page is intended to be a quick reference to terms that we use throughout
this project.

---

_Distributed System_: A system that's characterized by its components
being split up across multiple distinct computers on a network.

_Distributed Tracing_: A method of profiling and monitoring distributed system
performance by capturing telemetry data associated with each request.

_Instrument_: A logical object that emits a specific piece of named telemetry.
For example, an instrument might measure the current number of requests being
handled by a process, or the amount of free disk space.

_Instrumentation_: Code that exists to emit telemetry signals to some external
system or endpoint. For example, a logging statement or a metric instrument.

_Logs_: Human-readable text output from services and computers in a system.

_Metrics_: Lightweight statistical data about a system designed to be plotted
and tracked over time.

_Monitoring_: The process of observing a system for changes.

_Observability_: The ability to both understand and reason about the behavior
of a software system without needing to make **changes** to that software system.
In practice, observability requires both high-quality telemetry and
high-quality observability tooling.

_Resource_: A durable unit of computing that is consumed to power a transaction.
See [Resources](./02%20-%20customers-and-engineers.md#resources) for more
information.

_Service_: A logical process that does some useful work in a system.

_SLA_: A Service Level Agreement, a contractual instrument between parties about
the performance and availability of a service.

_SLI_: A Service Level Indicator, a statistic about a system that corresponds to
the health of a transaction or resource.

_SLO_: A Service Level Objective, a statistic about a system that ties SLIs to
business objectives and goals.

_Structured Logs_: A type of log message that comports to a schema, usually
intended to be read and processed by a machine.

_Telemetry_: Data, either human- or machine-readable, that describes the
internal state of software services or infrastructure.

_Transaction_: A single unit of useful work in a system. See
[Transactions](./02%20-%20customers-and-engineers.md#transactions) for more
information.

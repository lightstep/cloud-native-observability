# Glossary of Terms

This page is intended to be a quick reference to terms that we use throughout
this project.

---

_Distributed System_: A system that's characterized by its components
being split up across multiple distinct computers on a network.

_Distributed Tracing_: The technique of following – or "tracing" – requests as
they traverse distributed systems, representing them as telemetry, and
reassembling and analyzing models of the distributed requests.

_Instrument_: A logical object that emits a specific piece of named telemetry.
For example, an instrument might measure the current number of requests being
handled by a process, or the amount of free disk space.

_Instrumentation_: Code that exists to emit telemetry signals to some external
system or endpoint. For example, a logging statement or a metric instrument.

_Logs_: Structured, or unstructured, output from a process to aid in debugging
or insight into a service or system. Usually optimized for human-readability.

_Metrics_: Statistical data about resources and transactions that is gathered
periodically, typically stored in a time-series database, and used as part of a broader
observability solution.

_Monitoring_: An aspect of observability that tracks the health of individual
resources and components, relative to the health of the entire system.

_Observability_: The ability to both understand and reason about the behavior
of a software system without needing to make **changes** to that software system.
In practice, observability requires both high-quality telemetry and
high-quality observability tooling.

_Resource_: A finite and durable unit of computation, storage, or other work
that is typically shared across many distinct transactions.
See [Resources](./02%20-%20end-users-and-engineers.md#resources) for more
information.

_Microservice_: A service in a distributed system that generally handles a
well defined and tightly scoped amount of work. An example would be an
authorization and authentication service.

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

_Transaction_: The input, output, and path of a single client's request in a
cloud-native application, consuming resources along the way. See
[Transactions](./02%20-%20end-users-and-engineers.md#transactions) for more
information.

# Glossary of Terms

This page is intended to be a quick reference to terms that we use throughout
this guide. These terms are defined within the context of cloud-native software
systems – more general definitions also exist for more general contexts but are
not provided here.

---

_Distributed System_: A system that's characterized by its components
being split up across multiple distinct computers on a network.

_Instrument_: A logical object that emits a specific piece of named telemetry.
For example, an instrument might measure the current number of requests being
handled by a process, or the amount of free disk space.

_Instrumentation_: Code that exists to emit telemetry signals to some external
system or endpoint. For example, a logging statement or a metric instrument.

_Observability_: The ability to both understand and reason about the behavior
of a software system without needing to make *changes* to that software system.
In practice, observability requires both high-quality telemetry and
high-quality observability tooling.

_Telemetry_: Data, either human- or machine-readable, that describes the
internal state of software services or infrastructure.

<!---
TODO:
- Distributed Tracing
- Metrics
- Logs
- Structured Logging
- Resource
- Transaction
- Monitoring
- SLO
- SLI
--->

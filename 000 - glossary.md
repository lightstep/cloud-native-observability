# Glossary of Terms

This page is intended to be a quick reference to terms that we use throughout
this guide.

---

_Telemetry_: A stream of data, either human or machine readable, that
communicates the internal state of an application or device.

_Observability_: A collection of tools and practices that leverage multiple
streams of telemetry data in order to answer questions about system state and
health.

_Distributed System_: A system that's characterized by its components
being split up across multiple distinct computers on a network.

_Instrumentation_: Code that exists to emit telemetry signals to some external
system or endpoint. For example, a logging statement or a metric instrument.

_Instrument_: A logical object that emits a specific piece of named telemetry.
For example, an instrument might measure the current number of requests being
handled by a process, or the amount of free disk space.

> COMMENT: we should add a definition of tracing here
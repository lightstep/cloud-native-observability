![Chapter 4 - Telemetry Creation and OpenTelemetry](./img/ch4_header.png)

# Telemetry Creation and OpenTelemetry

The foundation of observability is high quality, ubitquious, telemetry data. You
need high quality signals that contain rich metadata, tooling that aids in the
collection and processing of those signals, and APIs that are flexible enough to
integrate into a wide variety of resources.

While this might sound obvious, it's been a strictly aspirational goal for many
years. Historically, everyone just kinda does whatever makes sense based on
whatever tools have the most acclaim for their language or tech stack. There's
some fairly broad 'standards' -- [Apache
Logging](https://httpd.apache.org/docs/2.4/logs.html) and
[syslog](https://datatracker.ietf.org/doc/html/rfc5424) for logs,
[statsd](https://github.com/statsd/statsd/blob/master/docs/metric_types.md) and
[Prometheus](https://prometheus.io/docs/concepts/data_model/) for metrics,
[Zipkin](https://zipkin.io/pages/data_model.html) for traces. Commercial vendors
have generally reacted to this by creating agent-based pipelines that accept a
variety of formats, then translate them to a vendor-specific dialect.

The status quo presents multiple challenges that need to be overcome. An
agent-based model where telemetry data is hydrated with additional metadata
after it's emitted is going to lack specific transactional context (as this
context is generally lost after a transaction concludes), even if it can be
correlated with resource metadata. Time-window correlation can be helpful, but
in highly concurrent or asynchronous transactions, it usually isn't specific
enough to narrow down your search space. Furthermore, the reliance on external
agents and tools to translate this data into a common format locks us in to a
specific commercial vendor to handle that data. This also stymies implementors
of open source libraries, managed cloud services, and other resources that we
might wish to collect telemetry signals from -- they're forced to use the
lowest common denominator of signals to maximize compatibility with their
end-users telemetry pipelines. Finally, all of these additional translation
layers has a cost not only in implementation complexity, but in simple time --
if all of your telemetry data is five minutes old by the time you see it because
it had to be written to logs, translated into metrics, then batched up with a
bunch of other reports and fed into a general database, you're gonna have a bad
time actually trying to fight fires as you'll always be a few steps behind
whatever's actually happening.

Enter [OpenTelemetry](https://opentelemetry.io), a broadly-supported project
backed by the cloud native community and a host of monitoring and observability
vendors. OpenTelemetry has a simple goal -- make observability a built-in
feature of cloud-native software. It achieves this by providing a consistent API
and SDK to generate telemetry signals, all backed by a shared context that
allows you to connect specific signals to specific transactions. It provides a
standardized exposition format, and semantic conventions for telemetry data,
enabling better analytical and persistence options for end-users. In short,
OpenTelemetry is the foundation for cloud-native observability practices.

> Further Reading
>
> A more complete discussion of OpenTelemetry, along with more depth on the topics in
> this chapter can be found in [The Future of Observability with
> OpenTelemetry](https://www.oreilly.com/library/view/the-future-of/9781098118433/).

Independent of OpenTelemetry, however, there's the actual act of instrumentation
itself, and how telemetry is created in the first place. Let's step back to talk
about how we should think about instrumentation in general, before we summarize
the specific role of OpenTelemetry in the cloud-native landscape.

## Instrumentation and Granularity



## OpenTelemetry and Commodity Telemetry

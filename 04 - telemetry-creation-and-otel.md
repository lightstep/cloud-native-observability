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

Cloud-native observability requires high quality, _thoughtful_, instrumentation
of software. This is a challenging ask, and it's probably the reason that so
many teams fail to achieve their observability goals. Much existing
instrumentation is either upscaled (i.e., hydrating existing logging or
metrics with additional metadata from an orchestration layer, like Kubernetes)
or downscaled (i.e., fine-grained function or line level stack traces and
metrics being compacted or aggregated).

There's two maxims we should abide by, here -- first, white-box instrumentation
is going to always be of a higher quality than black-box instrumentation, and
second, your telemetry instruments should live one step away from what you're
trying to understand. The first is self-explanatory; If your instrumentation is
able to emit signals that are aware of the transactional context of your
application, then you're going to have access to more useful data and metadata
about that transaction. This means that telemetry creation should be something
you think about as part of development rather than something that gets added in
later. Imagine a world where you define the expected state of a transaction by
implementing it as a trace, then you know you're done when the expected trace
and the actual one match!

The second point requires a bit more nuance. What does 'one step away' mean? In
practical terms, it means your telemetry code shouldn't necessarily live with
your business logic. Imagine a simple HTTP server responding to API requests;
What part of this code should be instrumented with tracing, logs, and metrics?
The function handler that defines a route? The underlying HTTP library that
handles connection management? The runtime or system libraries that are being
called to open and close sockets? None of these are necessarily the _wrong_
answer, but the right answer depends a great deal on what your workflows can
support. If you mostly can control the code that you've written, then
instrumenting at the library makes a lot of sense. If you can control the code
and the libraries, then another layer at the runtime makes sense. If you control
all of that, then system-level instrumentation makes a lot of sense.

These differing granularities need to also be configurable, either through a
rules engine, or heuristic-based algorithms (i.e., when _x_ error rate is high,
increase collection rate/depth, when _y_ metadata is present disable
collection). We'll talk more about it in a later chapter, but high ROI
observability requires high granularity, but not in perpetuity.

What should be avoided, however, is over-indexing on a specific type of
telemetry. Metrics, logs, and traces are all _required_ to represent the work
being done by transactions, and the resources that they consume. It's not three
pipes, it's a single braid of unified, contextual data.

## OpenTelemetry and Commodity Telemetry

How do we achieve this nirvana of granular, omnipresent telemetry data? As we
mentioned above, OpenTelemetry exists as an open-source standard for creating
and representing metrics, logs, and traces. The real goal of OpenTelemetry isn't
necessarily to supplant all existing telemetry formats, but to provide a _lingua
franca_ for how that data is collected and expressed, to provide a shared
context for associating transactional telemetry together, and to ensure that
high-quality data is a built-in feature of the cloud-native ecosystem.

The status quo sucks, this much is true. Proprietary vendors spend millions on
writing commodified integrations with popular open source libraries and
packages, or on translation layers between existing telemetry formats and their
own. If you look at the actual code, though, most of this is all the same stuff!
There's only so many ways to get a trace or a metric from a SQL client or a HTTP
library. OpenTelemetry promises to change this, by providing open source
maintainers a single, well-supported, and unopinionated standard for writing
their own instrumentation code _or_ translating their existing instrumentation
into the OpenTelemetry format.

To reiterate, because as of this writing, we don't think it's well understood --
OpenTelemetry is **not really an end-user API**. The core API and SDK is really
intended to be integrated by upstream library or service authors. If you're a
user of telemetry data, the goal is for it to simply be _present_, available on
a specific endpoint, quietly waiting for you to connect a compatible analysis
tool and begin inspecting it. If you think about it from this lens, many of the
decisions the project has made makes more sense. Will it always be this way?
Probably not -- as the project stabilizes and matures, more work will go into
the end-user experience, and it'll become easier to use for non-integrators as
well. That said, the original goal is still the goal; OpenTelemetry should be a
checkbox, something that just exists in the background. 

With this in mind, it's easier to see how OpenTelemetry solves the
instrumentation challenges posed in the last section. If it simply exists in
your libraries, runtimes, and kernels then the actual burden of instrumenting
(or re-instrumenting) is greatly reduced. If it exists at multiple layers, then
dynamically controlling the rate of telemetry generation isn't that hard -- you
just start listening for more data, or for different data. If you're getting
metrics, traces, and logs in a unified, context-aware format, then you aren't
going to have to try and force everything to be represented as just a metric, or
just a log, or just a trace, or some combination -- you'll have it all,
intertwined, and cross-referencable. 
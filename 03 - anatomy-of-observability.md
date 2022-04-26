![Chapter 3 - The Anatomy of Observability](./img/ch3_header.png)

# The Anatomy of Observability

A big reason that the definition of observability has become so polluted is
because popularized definitions are so broad as to be meaningless. Saying that
you can "answer questions you don't know to ask yet", or "find a needle in a
haystack" are -- for lack of a better phrase -- junky marketing. You know what I
can put a bunch of data into and answer arbitrary questions with? MySQL.

Observability, then, isn't simply defined by the desired end state of
"understanding a system by its outputs" (because there are no systems that can
be controlled otherwise; a system without output is one that might as well not
exist) -- it's a combination of tools and techniques, workflows and products,
bound together with a shared language of SLIs and SLOs.

The building blocks of observability are pretty straightforward in this model --
telemetry, persistence, and workflows; Each building on the other. Telemetry is
data and metadata about the behavior of our transactions and resources.
Persistence is how we collect and store that data. Workflows are how we analyze
and make sense of it. Let's look deeper into each, starting with telemetry!

## Telemetry

In the [prior chapter](./02%20-%20endusers-and-engineers.md), we discussed
three different telemetry signals that can be emitted by transactions and
resources. These signals -- logs, metrics, and traces -- are sometimes referred
to as the "three pillars" of observability. However, this is a misnomer borne
from the rough way that the observability space has matured backed up mostly by
product marketing rather than practical reality. These signals are all
interrelated, interconnected, and indispensible to observability tools.

Let's discuss each of them first, and how they can be used to understand
transactions and resources. Then, we'll discuss how signal quality and richness
impacts observability.

### Logs, Metrics, and Traces

**Logging** is the practice of writing human-readable text data to the console
or a file. This data can be structured into a predefined schema for processing
by a log management system, or unstructured free text -- usually, both, and
often inside the same component. Logs help engineers understand transaction or
resource state, especially in exceptional cases such as process failure.

**Metrics** are compact, numerical, statistic data designed for easy and cheap
transmission and storage. Metrics can be emitted by transactions or resources,
and are usually visualized and analyzed as a time-series plot. Metrics help
engineers understand aggregate system health, or track key indicators of
transaction performance.

**Traces** are a special form of structured, machine-readable, logging. While
different tracing systems can vary in their details, they all share the idea
of a transaction-level context -- this means that a single trace is mapped to a
single, logical request through a distributed system. A trace is comprised of
multiple spans, where each span represents a logical unit of work executed as a
part of the transaction.

All of these signals share a few commonalities -- they comprise some sort of
message or value, and a collection of metadata. The message varies based on
signal type; Logs emit a string of human-readable text, metrics contain a
number, and traces emit the duration and name of work performed. The metadata
tends to be similar in nature if not specifics -- everything from host names,
source or destination IP addresses, customer identifiers, version numbers, and
much more.

While transactions and resources can both emit any of these signals, there are
some that make more sense than others. The following table illustrates a few
examples of this:

<!-- markdownlint-disable line-length-->
| | Logs | Metrics | Traces |
| --- | --- | --- | --- |
| **Transactions** | A stack trace dumped during process crash | A counter of concurrent user sessions in a given service | A span that wraps a database call |
| **Resources** | Information about processes that started or stopped | The amount of memory in use per-process | The work being done by a forked shell script |
<!-- markdownlint-enable line-length -->

The challenge of cloud-native observability isn't simply in emitting each of
these signals, however -- we need to understand why they fail in isolation, and
how to relate and connect them to each other.

#### The Momentum Tax

As said in _Mass Effect_, Sir Issac Newton is the deadliest son-of-a-b*tch in
the galaxy. The same is unfortunately true in the realm of telemetry. I would
suggest that the overwhelming majority of complex systems in use today are built
on a rats nest of variegated telemetry signals with vastly different and often
conflicting users and stakeholders. If you believe this to be false, then I
invite you to count how many crucial business processes relating to the software
you maintain or build occur within Microsoft Excel.

The hard truth is that any observability practice is bound by the least
observable component of any of its critical paths. What does it mean to be
'observable'? Broadly, to have accurate, correct, and descriptive telemetry data
produced reliability and quickly by your components. As we'll see, extant
telemetry signals tend to fail at multiple parts of this test. As to why we
still _use_ these signals, well, look at the heading -- momentum. It's hard to
justify replacing your telemetry for anything other than a massive perceived
value, because the people who pay the tax are usually _engineers_ rather than
customers. Let's discuss some of the flaws of each signal as they're usually
implemented.

Logging tends to suffer the most from the momentum tax, as it's the easiest
thing to produce. The first thing most of us probably learned how to program was
an application that wrote "Hello, World!" to the console, after all.
Unfortunately, most developers education about how to log started and stopped at
this point. It's one of those skills that we don't really train except through
osmosis as we become more experienced engineers. It's a "damned if you do,
damned if you don't" signal, because it truly is the lowest common denominator
of telemetry; Everything can emit it, and everything can read it, if you put in
the work. In addition, logging has many suitors -- security teams, auditors,
finance and operations, developers, and many more.

Metrics suffer as well, but in a more pernicious way. Rather than a lack of
suitability, metrics tend to suffer from a lack of precision due to the
ever-present fear of [cardinality
explosions](https://www.robustperception.io/cardinality-is-key) or [confusing
and misleading](https://www.robustperception.io/on-the-naming-of-things) names
or attributes. Metrics tend to be underused in transaction telemetry
specifically -- the requisite variety required to produce highly detailed time
series at scale is, for most metric systems, unsustainable. A question like
"show me the latency of a particular endpoint by customer ID" is easy when you
have ten customers, hard when you have ten thousand, and nearly impossible when
you have ten million. Resource metrics suffer from similar scaling issues,
especially thanks to container orchestration and horizontal scaling.

Traces, being a 'newer' signal type, suffer less from this momentum tax.
Generally the biggest challenge experienced here is divergent or discordant
trace specifications and implementations as different teams implement tracing on
specific sub-systems without connecting them to each other. Additionally, APM
(Application Performance Monitoring) and RUM (Real User Monitoring) tools have
led many engineers to only understand tracing in the context of _profiling_ a
single process rather than holistic system health.

With all that said, what _should_ it look like?

### Cloud-Native Telemetry Generation

Our discussion of transactions and resources touched on a point -- transactions
consume resources, resource exhaustion makes transactions suffer. You can also
think of this in the inverse, that a collection of resources is a state machine
which produces desired transactions. This is an important distinction, because
it raises very real questions about where we _should_ generate telemetry, and
how we aggregate it.

I would suggest a rather facile response, which is that we do _both_ at the same
time through context mechanisms that allow for us to tie the telemetry signals
generated by our transactions and resources together. This necessarily means we
need tools that are capable of correlating signals regardless of their
component, and aggregating them based on the needs of operators rather than on
arbitrary distinctions driven by their source. Logs, metrics, and traces all
have a place -- and it's the same place, as an interrelated and interconnected
braid of data.

We'll address the exact mechanisms by which this data can be generated in the
[next chapter](./04%20-%20telemetry-creation-and-otel.md), but I want to bring
it up here before we start talking about what _good_ telemetry looks like.
As mentioned above, it's not enough to just be correct or accurate or
descriptive -- you need to achieve all of these simultaneously. It's certainly
true that all of the telemetry signals we previously discussed are convertible
-- you can turn a log file into a stream of metrics, for example. However, this
fails the other part of our equation, as adding processing delays for telemetry
conversion fails to make it available quickly to operators. Fundamentally,
though, **these signal types are just telemetry sugar**. Traces are a structured
form of logging, metrics can be generated from aggregate trace analysis, and
logs are just text-based metrics of state. Once we stop thinking of these
signals as independent from each other, or as driving unique and siloed analysis
experiences, then we can start to imagine different possibilities.

Later chapters will dive more deeply into this topic, but I want to preview them
here in order to frame the rest of this discussion. Some readers may ask why we
spend so much time on these individual signals but at the same time seem to
eschew them by saying they're all 'the same' -- it's because without getting
this part right, you're building on a foundation of sand. Here's some statements
about what 'correct' telemetry generation can get you --

* Accurate histograms and summaries generated from actual customer data rather
  than synthetic testing.
* Telemetry points that are automatically correlated through
  associativity with their respective transactions rather than time-windowing.
* Resource telemetry that can identify transaction dependency chains, such as
  specific jobs on a queue that are blocked by upstream consumer lag.
* Performance telemetry linked to user analytics, security analytics and logs,
  or any other data source.

> More information on telemetry resolution and other signals can be found in
> [this appendix](./001%20-%20appendices.md#telemetry-resolution)

### Telemetry Quality

Telemetry, in general, is the most important part of observability; If you don't
have telemetry, you can't do much else. It stands to reason, then, that the
quality and quantity of your telemetry data has a direct and measurable impact
on your observability practice in general. This doesn't simply impact the
software systems analyzing your telemetry, but the engineers trying to make
sense of it -- and it's those engineers who define what "quality" ultimately
means here.

For many of us, the quality of our telemetry data varies drastically, even
within a given signal. Consider the amount of varying logging libraries or
targets you're likely to find in a sufficiently large application, the variance
in metrics produced by different operating systems or frameworks, and
tracing data produced by different resources or half-implemented omnibus tracing
efforts.

This puts us in a bit of a pickle; We're generating more telemetry than ever,
but none of it in the same way. Even within the context of a single transaction,
there's no guarantee that we can associate the telemetry from multiple
components together except via time windows (or fairly rudimentary links across
shared attributes). Furthermore, transactional dependencies may emit critical
telemetry in differing formats that our own, necessitating ever-more complex ETL
pipelines in order to translate this data into a common format. The result is a
byzantine series of pipes, data lakes, and dashboards that introduce real
latency to the comprehension and value-generation aspects of telemetry
utilization.

A simple example -- how do you represent the host name of the server that ran a
piece of code? Is it the `hostname`, `host.name`, `HostName`, `host_name`, or
`hostName`? These are all linguistically valid, but if all of my telemetry can't
agree, then suddenly it becomes very difficult to group and filter on this
attribute. What about if the semantic meaning of `hostname` differs between
components or regions? This causes real, constant pain for developers today, and
it needs to be solved.

The solution here is fairly simple in theory, if not in practice -- but it's
this: we need standards, and they need to be open. A common API that can be
shared between LOB applications and their dependencies -- along with a common
format that both transactions and resources can emit telemetry data in -- goes a
long way towards solving this pain point. We'll discuss this more in the next
chapter, because thankfully, this solution exists.

## Persistence

We've come a long way from the "record everything and let God sort it out"
mindset of the 2010's. It's true that storage is [cheaper than
ever(<https://blog.dshr.org/2012/02/cloud-storage-pricing-history.html>)
], the increase in data volume easily negates the reduction in persistence
costs. Additionally, an increasingly complex legal and regulatory environment --
that only promises to get more complex -- demands sophistication in the question
of not only what data we store, but where we store it, how we store it, and how
we attribute changes to it.

This makes persistence a key consideration in application and system telemetry
data, and there's a lot to consider. Observability systems tend to suffer from a
mismatch between reads and writes; This is to say, the way that telemetry is
presented to the storage system is orthogonal to the way you'd like to read it
back from that system. When we ingest these signals, they're often batched and
mixed with a wide variety of metadata that covers multiple transactions or
resources; When we read them back, we usually only want information about one.
The individual data points are very small, but queries that you'd be interested
in might aggregate hundreds of thousands of points to return a result. This
presents many challenges in the design and operation of a production
observability system; You can't just take something off the shelf and go, it
really does require a lot of thought and effort!

The solutions we've seen to this problem usually involve highly specialized
databases for different signals. Projects such as
[Cortex](https://cortexmetrics.io), [OpenSearch](https://opensearch.org), and
[Cassandra](https://cassandra.apache.org/_/index.html) are all popular
time series or NoSQL databases for storing metrics, logs, and traces,
respectively. This fails a test, though -- observability is about more than just
having these signals available, it's about the _integration_ and _correlation_
between these signals. Keeping all of our data in separate buckets with
incompatible query languages and APIs (or torturously adapting metrics queries
to support traces, or trying to use SQL as a lingua franca...) means that we're
tilting our head and squinting at observability without really grasping the
point.

What observability requires is specialized persistance that natively supports
multiple types of telemetry, but also allows that telemetry data to be queried
in a consistent fashion regardless of type. This allows us to perform actual
cross-cutting queries and comparisons between signals, have a shared syntax to
express aggregations, groups, filters, and so forth, and reduce the cost and
maintenance burden required to operate the storage layer. This cost shouldn't
just be thought of as 'how much am I spending to keep this stuff operating',
either -- controlling how much you're spending to keep this data around, and
figuring out how useful it is, is key.

### Is everything an event?

There's an alternative theory about telemetry that agrees in spirit with the
above sentiment. In summary, it claims that metrics, logs, and spans can all be
generally classified as "events" -- a generic form of telemetry that represents
some distinct, observable occurrence in your system. The storage layer for these
events can then be optimized for one type of data, rather than for each
individually.

While spiritually similar to the argument above, the 'everything is an event'
position presents challenges at the persistence layer. Certain signals contain
implicit expectations about persistence that need to be addressed -- for
example, most people wouldn't expect a single trace to arbitrarily be kept for
over a month, but they would expect a time series to be (albeit in a compacted
form). Metrics exist in an extremely compact format to begin with; 'Up-scaling'
them into an event adds metadata that effectively increases their storage cost
over time. This also adds a burden to users, who need to pay special attention
to how much data is being emitted to the storage engine, as it doesn't
necessarily know about the different types of signals and can't make intelligent
decisions about how or where to store things (i.e., moving
useful-but-not-critical log or trace data to cold storage rather than keeping it
warm)

In practice, rather than trying to genericize telemetry signals, it can be more
helpful to think of the association between them via shared context at the point
of emission -- we don't need to turn everything into an event if we're able to
ensure that each signal is emitted with the appropriate attributes to ensure we
can correlate them later across types. This is something we'll touch on, again,
in the next chapter -- stay tuned.

## Workflows

Once you've got some data, and you've got it stored, you wanna do something with
it. Traditionally this has meant you pull up some sort of query builder or data
explorer and start running queries, then pulling those queries into a dashboard.
A link to that dashboard would be added to a runbook or wiki page, and it'd
mostly sit there unneeded until trouble came a-calling, at which point whoever
was on call would open it up and realize half the graphs didn't work any more
because someone renamed the metrics last month and didn't update the charts.

Realistically, everything we've talked about before accretes into some sort of
actual benefit over the existing approach. Observability isn't about simply
swapping out your storage layer with something smarter, or improving the data
quality of your telemetry signals, even if both of those are necessary for it.
Think back to our earlier chapter, where we talked about transactions and
resources -- our goal is to improve our ability to understand change by
monitoring critical signals. The workflows that observability enables are what
actually helps us do just that!

### Monitoring and Alerting

The practice of monitoring looks a lot like what we described in the
introduction to this section -- a bunch of dashboards that attempt to record
signals that describe every failure state of a system which you need to peruse
and interpret to determine why things failed. There's a lot of problems with
this; As I mentioned before, our underlying data can shift and change leading to
dashboard staleness, or a lack of breadth in the dashboard itself requires us
to click between multiple tabs and systems in order to get a complete picture of
the system state.

A better way to think of this is to change what we're actually monitoring.
Dashboards full of visualizations over a collection of time series shouldn't be
our primary interaction method with our observability tools. Monitoring needs to
very explicitly connect resource and transaction health with the business goals
that they support. This changes our frame of reference to monitoring our SLOs
rather than our specific SLIs.

One way to think about this is to borrow from the field of industrial control
and safety thinking[^safetyWhitepaper]. Safety-I focuses around incidents, and
what goes wrong. Safety-II is far more concerned with what goes _right_ in a
system, and learning from that. Observability workflows should, on the balance,
be more concerned with reporting and measuring acceptable outcomes in order to
shape our thinking about our system, and how to build it in a more resilient
way (aka 'model formation'). Traditional alerting and monitoring approaches are
caught in a Safety-I mindset, but SLOs are much more useful to drive Safety-II
approaches because they help you focus on what _should_ be happening rather than
what _is_ happening.

SLOs provide a convenient framework for alerting, as well. Traditionally, we'd
know 'something's wrong' because we would set alerts on our indicators
around things like 'is this value too high or low for too long', or 'is the
change from period to period greater than some arbitrary threshold'. While this
can tell you there's smoke, it doesn't necessarily tell you if there's fire.
What's worse in my mind is that these kind of alerts don't actually give you any
sort of context or rationale for how they're impacting things. Service owners
can set up a variety of alerts based on assumptions that were made in the past
that may not have any real connection to how the service is being used today or
it's current usage patterns.

SLO-based alerting, though, solves this problem neatly. An SLO is already going
to have a built-in threshold at which you should start caring about it (when
you're in violation) _and_ logically consistent breakpoints at which you should
be notified about changes (when you're burning down, when you've run out of
error budget, when you're not using enough budget, etc.) that don't really
change based on the context of the service you're using. An SLO is an SLO is an
SLO, after all.

SLO-based monitoring and alerting also can make a huge difference in how teams
outside of engineering interact with observability and reliability. SLOs are a
natural way to communicate between engineering and business stakeholders on the
tradeoffs between feature development and 'reliability engineering', or about
the benefits of architectural and structural improvements to a codebase.
Consistent alerting based on SLOs reduces on-call handoff errors and anxiety, as
there's a consistent set of things you'll be alerted on.

Of course, this is only half of the equation -- once you've been alerted that
you're burning down an SLO, you gotta figure out _why_.

### Investigating Change and Difference

I'm gonna say that there's no outage or incident that ever existed without
something changing to cause it. Maybe that change wasn't something _you_ did --
it could be a customer sending malformed payloads, or a dependent service
suddenly changing its API, or an underlying dependency being updated by a
well-meaning security team, or an unexpected cloud failure.

There's two types of change that we should care about -- intentional and
unintentional. These intentional changes are produced as we develop and push
code out to our systems. They're the result of our CI/CD process, or dependency
updates, or changes to our team structure. Observability acts as a flywheel on
all of these, making us able to move forward more quickly and more safely, by
ensuring we can identify the results of these deliberate changes. Did we
actually fix that bug? Are we actually getting more sign-ups? Did we reduce how
long it takes to load the page? While it's nice to muse about the high-minded
pursuit of 'data exploration', most of us don't want to become Magellan in order
to see the effect of merging a PR.

Unintentional changes are more pernicious, but they're equally important to
understand. These unintentional changes can be the result of accidental
dependency upgrades, emergent user behavior, certificate expiry, exercise of
dormant code paths, and much more. Helpfully, the actual process of detecting
change works about the same for both. What is different about unintentional
changes is that our context differs greatly. We know when we've deployed code
and are watching it roll out; We usually have a theory we're trying to prove or
disprove. Unintentional change happens when we least expect it, from vectors
we're unlikely to predict. In these cases, we need workflows that are
deliberately crafted in to quickly present accurate hypotheses, present relevant
correlations, and let us compare differences in broad system state across
arbitrary time windows using a blend of telemetry signals.

It's actually very straightforward if you think about it -- the big story about
observability has a lot more to do with how you think about monitoring than
anything else. It's not just about storing a bunch of different data, it's not
just about having traces and metrics and logs, it's not even about just using
SLOs. The promise of observability is a radically different approach to how you
interact with your telemetry data -- it turns you from being a statistician into
a troubleshooter. It democratizes both data, and outcomes, so that everyone
involved in a software business can understand how software health and business
outcomes are interrelated.

[^safetyWhitepaper]: For a more comprehensive discussion of Safety-I and
    Safety-II, [this
    whitepaper](https://www.england.nhs.uk/signuptosafety/wp-content/uploads/sites/16/2015/10/safety-1-safety-2-whte-papr.pdf)
    provides a decent overview.

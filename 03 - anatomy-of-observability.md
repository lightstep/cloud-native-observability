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
bound together with a shared language of SLI's and SLO's.

The building blocks of observability are pretty straightforward in this model --
telemetry, persistence, and workflows; Each building on the other. Let's break
it down, starting at the bottom, with telemetry.

> COMMENT: add a high-level description of telemetry, persistence, and workflows
> before diving into the details, so readers have a frame of reference. 

## Telemetry

> COMMENT: as mentioned in the last chapter, we should consolidate all of our 
> telemetry definitions into this section.

In our prior discussion of transactions and resources, we addressed the three
types of telemetry -- [metrics, logs, and
traces](./customers-and-engineers.md#transactions), and how transactions and
resources use them to emit signals about performance. Let's look at the
commonalities between these signals, and discuss how they act as a foundation
for observability itself.

Telemetry, in general, is the most important part of observability; If you don't
have telemetry, you can't do much else. It stands to reason, then, that the
quality and quantity of your telemetry data has a direct and measurable impact
on your observability practice in general. The problem that we need to overcome
is twofold, then -- first, telemetry generation itself, and then, the quality of
that telemetry.

### The Quiet Tyranny Of Logging

> COMMENT: When we move tracing over, we should also make the case here that 
> tracing and logging aren't really separate tools. 

Logging is one of the fundamental activities in programming, insofar as the
first thing you learn how to program is an application that prints 'Hello,
World!' to a console. Unfortunately, this is both the first and last
introduction most developers get to logging in their formal or informal education.
Logging is "easy" after all, so we don't tend to teach it. When you get into a
'real job', your application will probably already have a chosen logging library
that spits out some nicely structured log data (all built before you got there,
or maintained by people that aren't you) -- you don't really have much of an
impetus to do anything more, because everything just works!

This is the steady-state default for the overwhelming majority of professional
software developers; Nobody really ever 'learned' logging because none of us had
to. Sure, there's outliers -- but logging isn't one of those things that people
need or want to reinvent every time they crank open their IDE.

The problem, really, is that logs as we know them aren't a great observability
primitive. Your logs probably aren't as structured as you like, you might not
have context information embedded within them, and you never seem to have the
right log granularity. They're everywhere, though! Nobody is going to accept a
solution that requires rip-and-replace of the hundreds of thousands of logging
statements in a codebase. Cloud-Native Observability requires _enhancement_ of
existing logs into a format that allows them to be blended with other forms of
telemetry.

### Telemetry Quality

> COMMENT: maybe focus more on data consistency first, and separate out the 
> issue of vendor lock-in into a follow up section (which could also mention the
> dilemma OSS libraries find themselves in).

Indeed, there's quite a few existing ways to generate telemetry data outside of
just logs that are embedded within our software. You can probably find a dozen
versions of log4j in any given application (well, maybe fewer these days) along
with a few different metric libraries, maybe a custom tracing library or two.
Perhaps you're using a commercial solution, which means your telemetry APIs are
all controlled by a vendor's agent or SDK.

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
useful-but-not-critical log or trace data to cool storage rather than keeping it
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
understand. Helpfully, the actual process of detecting change works about the
same for both. What is different about unintentional changes is that our context
differs greatly. We know when we've deployed code and are watching it roll out;
We usually have a theory we're trying to prove or disprove. Unintentional change
happens when we least expect it, from vectors we're unlikely to predict. In
these cases, we need workflows that are deliberately crafted in to quickly
present accurate hypotheses, present relevant correlations, and let us compare
differences in broad system state across arbitrary time windows using a blend of
telemetry signals.

It's actually very straightforward if you think about it -- the big story about
observability has a lot more to do with how you think about monitoring than
anything else. It's not just about storing a bunch of different data, it's not
just about having traces and metrics and logs, it's not even about just using
SLOs. The promise of observability is a radically different approach to how you
interact with your telemetry data -- it turns you from being a statistician into
a troubleshooter. It democratizes both data, and outcomes, so that everyone
involved in a software business can understand how software health and business
outcomes are interrelated.

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

## Telemetry

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

Logging is one of the fundamental activities in programming, insofar as the
first thing you learn how to program is an application that prints 'Hello,
World!' to a console. Unfortunately, this is both the first and last
introduction most devs get to logging in their formal or informal education.
Logging is "easy" after all, so we don't tend to teach it. When you get into a
'real job', your application will probably already have a chosen logging library
that spits out some nicely structured log data (all built before you got there,
or maintained by people that aren't you) -- you don't really have much of an
impeteus to do anything more, because everything just works!

This is the steady-state default for the overwhelming majority of professional
software developers; Nobody really ever 'learned' logging because none of us had
to. Sure, there's outliers -- but logging isn't one of those things that people
need or want to reinvent every time they crank open their IDE. 

The problem, really, is that logs as we know them aren't a great observability
primitive. Your logs probably aren't as structured as you like, you might not
have context information embedded within them, and you never seem to have the
right log granularity. They're everywhere, though! Nobody is going to accept a
solution that requires rip-and-replace of the hundreds of thousands of loglines
in a codebase. Cloud-Native Observability requires _enhancement_ of existing
loglines into a format that allows them to be blended with other forms of
telemetry.

### Telemetry Quality

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

### Is everything an event?

## Workflows

### Monitoring and Alerting

### Investigating Change and Difference
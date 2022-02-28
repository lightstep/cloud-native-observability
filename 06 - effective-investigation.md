![Chapter 6 - Effective Investigation](./img/ch6_header.png)

# Effective Investigation

There's a common aphorism, 'the only constant in life is change'. This is
especially true of a software system. The magnitude and frequency of these
changes differ based on a variety of factors both internal (team size,
composition, development methodology) and external (userbase, seasons,
internet connectivity interruptions) but they all can have an impact.
Identifying, understanding, and mitigating the adverse impacts of these changes
is in many ways the 'primary use case' of observability tools.

"That seems obvious", you might say, but you wouldn't know it based on most
existing monitoring and observability tools. The overwhelming majority of
existing tools are focused around monitoring and dashboarding, with analytical
features being difficult to use or of little value. Why is this the case? Three
reasons come immediately to mind; The difficulty of building good
general-purpose analytical workflows, inconsistent data quality preventing
intelligent analysis workflows, and a lack of equity in telemetry data access or
interest amongst engineering staff.

This is why our observability tools all kinda look the same if you turn your
head and squint; The market forces optimization around 'table stakes' features
such as query builders and visualizations, creating a dillemma for new entrants
-- buyers don't want different unless you can prove that different will save
them money in the short term. This has lead to a great stagnation, where
innovation is leashed to short-term financial incentives, benefiting nobody.

Cloud-native observability asks us to rethink this and to separate out
investigation from monitoring, to really consider it as a standalone function.
Investigating change should be about more than being able to bang out some
really good queries or time-window correlations between different telemetry
types. It calls for general-purpose analytical workflows that let us compare and
contrast system state across transactions and resources at different times, in
different conditions, and at infinte layers of granularity in order to quickly
identify what changed, why it changed, and how it's impacting reliability and performance.

## Context -- What Ties Everything Together

The most challenging thing in any post-hoc (or real-time) investigation of an
incident is establishing context. We need to know the status of tens, hundreds,
or thousands of dependent and independent variables in order to accurately
reconstruct a mental model or reproducible sample of our failure. This presents
several challenges in not only capturing a sufficiently detailed snapshot of
system state and storing that snapshot for post-hoc analysis, but also in
analyzing them in order to discover the specific changes that lead to an
incident. Conventionally, this has meant making decisions about several
tradeoffs between snapshot breadth, depth, and fidelity.

This shouldn't be as hard as we've made it. Transactions contain within
themselves a sufficient picture of their internal state to aid in hypothesis
generation and disqualification, linked together by a shared request context.
Additionally, point-in-time or periodic measurement of a resource's
instrumentation carry enough data to allow for effective correlation between
a specific (or aggregate) transaction and the underlying resource state. 

What's been missing here is a _lingua franca_ for associating transactional and
resource telemetry together. OpenTelemetry solves this through it's context and
baggage concepts; Combined, they're a generalizable way to propagate
transaction-specific information to other processes or services involved in a
transaction. In concert with other OpenTelemetry tooling, this context and
baggage can be associated with resource telemetry such as logs and metrics, all
of which can then be emitted to an analysis tool capable of understanding it.

This contextual information opens up a new world. Rather than relying on
manual association through pre-shared tag values or simple time-window
correlation, shared context means that tools can build more useful and impactful
visualizations and analytical features. Rather than just looking at a single
transaction with an error, we can instead look at a graph of all services
involved in a transaction and compare those graphs against each other. Resource
consumption and limits can be overlaid on these graphs, quickly giving us visual
feedback on resource exhaustion or oversubscription, unusual usage patterns, and
so forth. 

With enough time, these tools can mature and begin to use machine learning
technologies to dynamically perform these comparisons and issue suggestions or
commands to a system, such as modifying throttling for specific clients to ease
pressure on a congested API that's approaching a SLO violation. 

## Guided Analysis vs. Data Exploration

While the future is full of possibility, there's decisions we have to make today
in this landscape. The fundamental tension in observability tooling can be
broadly summarized as "do you want a really good query/visualization tool, or do
you want something else?" It's not hard to understand why we often opt for the
former -- there's a strong sense of familiarity and control when presented with
this model of data exploration.

However, this model is insufficient to satisfy the needs of a cloud-native
observability program. Even extremely powerful and intuitive query builders and
data visualizers require a lot of buy-in from operators and developers. What
happens quite often is a bimodal distribution of usage, where people are
simply reading charts that someone else built or they're heavily building and
caring for those charts for others. This sucks for everyone -- it silos
knowledge into the hands of the observability sherpas, who quickly become single
points of failure when they're unavailable or after they leave a team; It stunts
growth and velocity as newer or more junior team members are unable to
self-serve to answer questions. 

We posit that data exploration is necessary but insufficient to satisfy the
needs of cloud-native observability. It isn't an "either-or", but the primary
way you interact with an observability tool shouldn't necessarily be a query
editor! It's certainly familiar, and it certainly has its place, but it sets us
up to think in a certain way -- we see there's a problem, and we start digging.
In lieu of this, we propose other 'observability primitives' such as service
graphs, aggregate trace comparisons, and histograms be where we start, then
drilling down and winnowing out possibilities from there.

The advantage of this winnowing process is that we don't have to rely on other
people so much -- people that might be half a world away, or asleep, or busy
with any number of other things. If we present users with rich visualizations
that present service health and status via their relationships (i.e., seeing a
service graph with an indicator about the volume of traffic or errors that are
being reported as part of a transaction, or a diff tool that shows us the change
in two bundles of traces) then we free ourselves from having to know what we're
interested in finding out before we actually can start figuring things out. This
democratizes problem-solving, as we don't need to be an expert to see where
things are wrong, we can quickly see if it's a problem with our service or
someone else's and go from there -- usually getting enough information in the
process to find the right person or team and tell them about it!

## Tagging and Cardinality

So if it's that simple, why don't we do it? Good question. As stated throughout
this document, data quality is one of the biggest things holding us back; The
sort of hand-held analysis mentioned earlier is extremely difficult to achieve
without significant amounts of instrumentation written in a standardized way,
embedded throughout a complex application from its earliest days and maintained
over the course of months or years. 

There's hope here, in the form of OpenTelemetry -- its semantic conventions
promise to make it easy to create standardized, high-quality telemetry data. In
a more general sense, however, we need to approach this question in a more
holistic fashion. What is both necessary and sufficient for the systems we want
to build and run?

To address this, we need to talk about cardinality and tagging. There's a lot
you can read about cardinality[^cardinalityDef] online, but to breifly review
what it is, it's the amount of unique values associated with a single attribute
on some piece of telemetry data like a log, metric, or span. High cardinality is
the bane of observability systems, as it either makes things very slow (such as
searching for a single unique value in a set of millions or billions), very
expensive (such as metrics 'cardinality explosions' where high cardinality
attributes can cause 10x, 100x, 1000x, or greater combinations of time-series to
be created), very expensive (such as the cost for indexes on high-cardinality
structured logs), and usually all of the above. It's an unavoidable
complication. It's also a requirement for observability tools, so this tension 
needs to be relieved somehow. 

We'll talk more about it in our section on [telemetry ROI](./telemetry-roi.md),
but to the end user, cardinality shouldn't really be a concern. What we can do,
however, is blunt its impact through improved tooling throughout the
observability pipeline. Traces, logs, and metrics should all contain
high-cardinality data, but those situations shouldn't ever cause significant
impacts to your observability system, as it should be able to dynamically adjust
the rate of collection and intelligently pre-aggregate and bucket data to avoid
cardinality explosions hitting your actual system and causing failures there. 

As we stated earlier, the cornerstone of effective investigation is that you can
capture as complete of a snapshot of system state as possible, to make it
feasible to perform post-hoc analysis of everything that was going on at the
time of a failure or fault. There's still some missing pieces to this formula,
though -- controlling costs, for one. That said, the key takeaway is that
performing an effective investigation is about a lot more than nice
visualizations and a great query experience, it's about workflows that guide you
up and down the relationship tree between your transactions and resources,
giving you the ability to quickly prove (or disprove) hypotheses and answer
questions about what caused a given change.
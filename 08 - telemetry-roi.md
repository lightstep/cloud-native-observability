![Chapter 8 - Telemetry ROI](./img/ch8_header.png)

# Telemetry ROI -- The Elephant In The Room

If everything in the prior seven chapters has sounded like a beautiful
crystalline pipe dream, this is where it all comes crashing down to earth. The
explosive cost of telemetry and observability tooling, storage, and analysis is
staggering. Organizations spend hundreds of millions of dollars a year (at the
high end) on collecting and storing telemetry data, much of which is never even
seen by a human being nor influences a decision.

The status quo is harmful to observability practice, practicioners, and
end-users alike. Curiously, it's also something we don't talk about much. While
there's a generalized acceptance that we pay too much for too little, there's
also a generalized malaise that this is simply the way the world works and we
must accept it.

In this section, we'll dive into a discussion about why telemetry costs keep
rising, and what to do about it. We'll also discuss the tradeoffs between
different cost management strategies, and where they should be applied.

## Costs Go Up, ROI Goes Down -- You Can Explain That

You only have to look at financials from major monitoring vendors to see that
the people selling shovels are doing pretty well indeed. Organizations pay more
than ever to collect even greater amounts of telemetry data each year. If this
cost was linked to a linear scaling function -- as in, you pay more because you
have more users or more traffic -- then it would at least make sense. However,
costs tend to increase even if usage is flat. Why is that the case?

Increases in system complexity throw a wrench into this logic, unfortunately.
The product of new orchestration systems or managed services, new feature work,
and sustaining engineering efforts causes a combinatorial explosion in the
amount of telemetry emitted by a system. This telemetry tends to bias towards
resources rather than transactions, as new foundational work (migrations to
Kubernetes or the cloud in general) can emit significantly larger tranches of
new telemetry data that requires more care and feeding -- more dashboards, more
point solutions for monitoring or understanding specific types of cloud systems,
and so forth. 

This leaves developers in a bit of a lurch. The telemetry they need in order to
understand and diagnose end-user issues tends to be high cardinality and
multi-variate, but the monitoring budget is being doubly-spent on things like
cloud metrics being fed into a different (or even multiple different) monitoring
systems. We've all got stories about not being able to add a new attribute to a
metric, because we're arbitrarily limited on the amount of custom attributes for
contractual purposes or because we can't afford to scale up Prometheus to handle
the potential cardinality. 

What we're left with, then, is a curious condition. We're spending more than
ever, but getting less value from every dollar we spend. It's not just the bias
towards resource telemetry that causes this, it's everything we've touched on
throughout each chapter. The implementation wall of improving data quality, the
maintenence burden of creating and maintaining dashboards and queries, the
morale and communication overhead of inconsistent or unclear alerts and
reliability metrics, etcetera and so forth. 

Compounding this problem is that we use data volume as a proxy for data quality.
As discussed in the SLO chapter, modern organizations collect and capture
unthinkable volumes of data about their processes and systems for analysis and
decision-making. Different stakeholders each have their own telemetry desires --
security teams demand access and flow logging, analysts demand usage and journey
analytics, SRE's need high-fidelity resource and transaction telemetry. The data
stores and tools each of these roles require are often disparate and
unintegrated, leading to a need to either warehouse vast quantities of data or
duplicate it to various endpoints. Managing this stream requires no small
investment in time and money, and often overlays complex beauracracy into what
should be trivial asks. 

In summary, we've arrived at a situation where we're generating more telemetry
from more places than ever before, sending it to more destinations with more
advanced features than we've previously had access to, but all of this has
failed to appreciably improve outcomes. We're spending more money, and time, to
eke out marginal reliability gains while suffering from a _perception_ that
software is less reliable than ever. Something's gotta give.

## The Predictive Value of Telemetry

The key to changing this negative feedback loop is to rethink the value
proposition of system telemetry. Rather than viewing telemetry as the input to
primarily reactive processes (failure recovery, etc.) we should model it as a
_proactive_ input into predictive processes.

A predictive model of telemetry seeks to thoroughly instrument and collect
telemetry from our systems in order to generate forward-looking insights into
the reliability of not only our system itself, but the organizational support
for iterating on that system. As mentioned back in the introductory chapters, we
don't just want to be able to build fast, we _need_ to be able to build fast.
Rapid iteration isn't just smart, it's safe. Lengthly verification cycles in
pre-production systems don't necessarily translate to higher quality features or
less disruptive deployments due to the inherent drift between production and
non-production environments. Observability can change this balance, as it allows
us the ability to safely monitor continous deployments into production and roll
back if things aren't working out like we hope. Telemetry is the input into this
prediction algorithm -- and calling it an algorithm is really just being fancy,
this is basically describing a canary rollout. Traditional canaries look at
limited amounts of telemetry, though -- simple "is it up? is it fast?"
questions. Judicious use of metadata can give us more fine-grained options for
canarying, allowing us to see if we're actually improving our SLO adherence or
achieving specific business goals as part of a rollout.

Predictive telemetry also helps in capacity planning, not just for resources,
but also for teams and organizations. Being able to predict future usage by
analyzing historical patterns is one example of this, but by unifying our
telemetry and our business data, we're able to improve our ability to forecast
on-call rotations, hiring, and even the cadence of feature work. Democratizing
access to this telemetry means these insights can come from anywhere in the
organization, rather than being siloed by teams with limited visibility into the
overall system health. 

## Cost Reduction Strategies

Even if we wish to use our telemetry data in a more predictive, rather than
reactive, fashion, we still need to control how much we send and spend on it.
Traditional methods of managing this spend tend to be proactive -- for example,
reducing the amount of telemetry generated by a given process, or not adding
additional attributes due to cost concerns.

This is, again, an anti-pattern. Telemetry generation at the process level does
have an overhead, but it makes a lot more sense to have this overhead be
relatively fixed rather than dynamic. That is to say, if you know that every
transaction will have a given amount of telemetry generated for it, then you can
plan for the amount of resources required to support that overhead. 

This isn't necessarily something that has a one-size-fits-all strategy to
overcome. There's a world of difference between the amount of telemetry
generated by planet-scale applications (such as Google Search) and even large
enterprise applications (such as a large financial firm's electronic banking
app). Different systems will have different requirements. How do we figure out
what we need to keep and what we need to discard -- and how do we discard it, anyway?

### SLO's and Deep Telemetry

Determining how much data to generate and keep should be a derived function of
the SLO's we set for a given transaction. This does mean that telemetry data
requirements can fluctuate over time; After all, SLO's aren't set in stone, they
can change and grow. That said, the amount of telemetry we need should always be
in support of measurable semantic structures like an SLO. Don't collect data
'just because' -- if you ain't using it, you don't need to keep it! Identify the
critical transactional and resource telemetry required to achieve SLO monitoring
and investigative workflows, then discard the rest.

That said, what do we do when things get weird? There's always going to be
circumstances where the amount of data you need to solve a problem is greater
than the amount of data you keep on-hand. This is where 'deep' telemetry comes
in. We use this to refer to any number of process or system level dumps of
telemetry data that wouldn't normally be collected; Think pprof profiles, kernel
dumps, or network flow logs. The role of your observability system should be to
make transitioning across the boundary (from steady-state collection of
high-level diagnostic information to low-level system information) painless.
The balance we should strive for is "just enough" information in our usual
telemetry to solve 80% of our problems (or more), with enough data to help us
narrow our search space for that last 20%.

### Sampling

Even if we know what data we need, and how to generate it, it's foolish to
suggest that we collect and keep all of it. Systems that are in a steady state
don't really have anything interesting to say in detail. If everything is good,
or everything is bad, then you're probably not getting any useful insights from
detailed telemetry. 

Most of the time, though, your system will be somewhere between these extremes.
This is why sampling is useful -- it's a way to ensure a representative amount
of telemetry is collected about your system, smoothed out over peaks and valleys
of demand. Traditional sampling methodologies rely heavily on "head-based" or
client-based sampling rates, where originators of transactions define a given
ratio of transactions to be traced or logged (such as 1-in-10, 1-in-100, etc.)
and only recording transactions that meet that demand. This approach leaves
something to be desired, as outliers can be easily missed. Further developments
have introduced heuristic-based approaches, that override sampling rates based
on indicators of state, such as prioritizing collection of errors or other
faults in the final sample.

In general, cloud-native observability places more value on dynamic sampling
approaches rather than static ones. The key difference is where this sampling is
configured, and how it's adjusted. Traditional methods of dynamic sampling
require bidirectional communication between telemetry clients to adjust
head-based sampling rates based on other factors such as operator intent or
independent signals, or by cache-and-forward approaches where a pool of
collector processes store transactional telemetry in its entirety to make a
decision about a group. 

While these approaches have value, there's still a lot of confusion and further
work needed. Static sampling is too proactive, dynamic sampling can be too
reactive. The key innovation required is moving sampling 'up' a layer, from
generation or collection to ingestion. If sampling decisions become less about
'what's generated' and more about 'what's persisted', then these decisions can
be made in the full context of not only a transaction, but also overall resource
state. This means the knobs and levers for controlling sampling will need to
move as well, away from low-level configuration in code, and towards management
planes and analysis tools that can control and report on the current level of
sampling at a process, cluster, or system level.

### Aggregation

The other major lever to control steady-state telemetry costs is the aggregation
of telemetry data. Sampling controls the flow of data into our observability
pipeline, and thus our ingress costs -- Aggregation is our lever to control
long-term storage costs. For the most part, solutions here are rather
non-controversial, but many of them aren't in wide use.

Metrics, logs, and traces all benefit from aggregation at storage and
query-time, but the method of aggregation differs slightly between them. Rather
than dwell on the strategies, we'll summarize them here and discuss how they
impact cost controls. Metric aggregation occurs at both ingest and query time,
as points are smoothed into regular buckets. Trace aggregation tends to occur
mostly during ingest, as attributes and metadata are converted into frequencies
and de-duplicated. Log aggregation is another ingest and query-time concern, as
messages are de-duplicated, with variable fields clipped out and stored
separately.

All of these existing approaches are useful, but there's some nuances we should
be aware of. What benefits cloud-native observability is a shared set of
aggregation formats and visualizations that allow us to use these disparate data
types in similar ways, and to not re-aggregate data that we don't need to.
Observability platforms that are aware of multiple forms of telemetry can,
in response to operator requests or heuristic-based algorithms, dynamically
adjust aggregation and persistence rates and methodologies based on desired
recall times, storage constraints, or numerous other factors.
![The Complete Guide to Cloud-Native Observability](./img/header.png)

# The Complete Guide to Cloud-Native Observability

Over the past five years, the concept of
[observability](https://lightstep.com/observability/) has gone through a
sustained hype cycle. From being a niche concept bandied around by academicians
in discussions of control theory, to an emerging market of next-generation
software monitoring tools, to utter confusion and co-option as incumbent,
insurgent, and newly-minted companies alike scrabble to lay claim to the crown
of 'observability solution'.

A couple of years ago, we wrote a [pretty exhaustive
report](https://go.lightstep.com/rs/260-KGM-472/images/observability-guide.pdf)
about this topic -- what observability was, what distinguished it from existing
monitoring solutions, and its benefits to developers. Since then, there's been a
few important developments, and a surprising amount of new entrants, both large
and small. With that in mind, we decided to put our heads together, do some
research, and create an updated resource for the cloud-native community and
anyone else that wonders "what the heck is observability, anyway?"

Welcome, to **The Complete Guide to Cloud-Native Observability**.

## Who Is This For?

<!-- TODO: This needs to be updated as we go along. -->

This document is currently in a draft state. If you're reading it --
congratulations! You're either very curious or someone told you about it.

Our intention is that the reader is broadly familiar with the current concepts
and practices around observability, and the jargon associated with it. A
[glossary](./glossary.md) is provided to aid in definining terms, but we don't
spend a ton of time going in-depth on concepts like sampling, traces,
aggregation, etc. The reader is expected to have a passing familiarity with
these.

The goal of this document is to advance an _opinion_ about how we should be
thinking about, and building, observability systems. We don't want to do it
alone, though, and we don't think that it's helpful for us to prognosticate in
an ivory tower and then plop out a PDF with our thoughts. The reason that this
is on GitHub is expressly so we can get comments, contributions, and changes
from the entire observability world; practicioners, implementors, advocates,
community leaders, whoever. This vision shouldn't just be any one of our's
vision, it should be all of ours vision.

---

## Table of Contents
<!-- TODO: Add subsection links to TOC -->

* [README](./README.md)
* [Introduction](./intro.md)
    * [What's the point of software development, anyway?](./intro.md#whats-the-point)
    * [Why Cloud-Native Matters](./intro.md#why-it-matters)
* [Customers and Engineers, Transactions and Resources](./customers-and-engineers.md)
    * [Transactions](./customers-and-engineers.md#transactions)
    * [Resources](./customers-and-engineers.md#resources)
    * [SLI's and SLO's](./customers-and-engineers.md#sli-slo)
* [The Anatomy of Observability](./anatomy-of-observability.md)
* [Telemetry Creation and OpenTelemetry](./telemetry-creation-and-otel.md)
* [Effective Monitoring](./effective-monitoring.md)
* [Effective Investigation](./effective-investigation.md)
* [Effective SLO's](./effective-slo.md)
* [Telemetry ROI -- The Elephant in the Room](./telemetry-roi.md)
* [Organizational Concerns](./organizational-concerns.md)


---

## Contributing

We gladly accept pull requests! Please see [CONTRIBUTING.md](./CONTRIBUTING.md)
for more.

## Special Thanks

This guide is made possible thanks to [Lightstep](https://lightstep.com).



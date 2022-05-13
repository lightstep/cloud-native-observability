![The Complete Guide to Cloud-Native Observability](./img/header.png)

![Status](https://img.shields.io/badge/status-draft-blue?style=for-the-badge)
![GitHub release (latest by date including
pre-releases)](https://img.shields.io/github/v/release/lightstep/cloud-native-observability?include_prereleases&style=for-the-badge)
![GitHub Repo stars](https://img.shields.io/github/stars/lightstep/cloud-native-observability?style=for-the-badge)

# About This Project

**What is the goal of this project?** To provide a clear, concise, and unbiased
overview of cloud-native observability.

**Why is that important?** Despite untold millions of marketing dollars spent in
and around observability (or maybe _because_ of the untold millions of marketing
dollars spent), cloud-native observability practice today is often inefficient,
ineffective, or both. It doesn't need to be that way.

**Who is this for?** Observability practitioners, SREs and DevOps engineers,
end-users of observability systems, and anyone involved in building and running
cloud-native software at scale.

**How can you help?** You can contribute in several ways --

* Adding your stories and learnings to [the "Real World Examples" directory](./real_world_examples/README.md)
* Helping refine and shape this repository through
  [contributions](./CONTRIBUTING.md)
* Tackling a [good first
  issue](https://github.com/lightstep/cloud-native-observability/labels/good%20first%20issue)
* Giving us a star and sharing the repository!

You can also view the [release cadence](./CONTRIBUTING.md#release-cadence). We
plan to continue iterating and refining this content throughout 2022.

## What We're Looking For

Currently, we would appreciate feedback (either via issues or pull requests) in
the following areas:

* Overall structure and flow of the document.
* Overall comprehension of the text/themes.
* Un-defined, under-defined, or over-defined themes/concepts/terms.
* General feedback on the ideas.
* Illustrations and explanatory diagrams.

---

## Table of Contents

<!-- TODO: Add subsection links to TOC -->
<!-- cspell:disable -->

* [README](./README.md)
* [Foreword](<./00 - foreword.md>)
* [Introduction](<./01 - intro.md>)
  * [What's the point of software development, anyway?](<./01 - intro.md#whats-the-point-of-software-development-anyway>)
  * [Why Cloud-Native Matters](<./01 - intro.md#why-cloud-native-matters>)
* [End-users and Engineers, Transactions and Resources](<./02 - end-users-and-engineers.md>)
  * [End-users](<./02 - end-users-and-engineers.md#end-users>)
  * [Engineers](<./02 - end-users-and-engineers.md#engineers>)
  * [Transactions](<./02 - end-users-and-engineers.md#transactions>)
  * [Resources](<./02 - end-users-and-engineers.md#resources>)
  * [SLIs and SLOs](<./02 - end-users-and-engineers.md#slis-and-slos>)
* [The Anatomy of Observability](<./03 - anatomy-of-observability.md>)
  * [Telemetry](<./03 - anatomy-of-observability.md#telemetry>)
  * [Persistence](<./03 - anatomy-of-observability.md#persistence>)
  * [Workflows](<./03 - anatomy-of-observability.md#workflows>)
* [Telemetry Creation and OpenTelemetry](<./04 - telemetry-creation-and-otel.md>)
  * [Instrumentation and Granularity](<./04 - telemetry-creation-and-otel.md#instrumentation-and-granularity>)
  * [OpenTelemetry and Commodity Telemetry](<./04 - telemetry-creation-and-otel.md#opentelemetry-and-commodity-telemetry>)
* [Effective Monitoring](<./05 - effective-monitoring.md>)
  * [Effective Dashboards](<./05 - effective-monitoring.md#effective-dashboards>)
  * [Effective Alerting](<./05 - effective-monitoring.md#effective-alerting>)
  * [Fantastic SLOs and Where To Find Them](<./05 - effective-monitoring.md#fantastic-slos-and-where-to-find-them>)
* [Effective Investigation](<./06 - effective-investigation.md>)
  * [Context -- What Ties Everything Together](<./06 - effective-investigation.md#context----what-ties-everything-together>)
  * [Guided Analysis vs. Data Exploration](<./06 - effective-investigation.md#guided-analysis-vs.-data-exploration>)
  * [Tagging and Cardinality](<./06 - effective-investigation.md#tagging-and-cardinality>)
* [Effective SLOs](<./07 - effective-slo.md>)
  * [Counting the Uncountable](<./07 - effective-slo.md#counting-the-uncountable>)
  * [Pushing the Envelope](<./07 - effective-slo.md#pushing-the-envelope>)
* [Telemetry ROI -- The Elephant in the Room](<./08 - telemetry-roi.md>)
  * [Costs Go Up, ROI Goes Down -- You Can Explain That](<./08 - telemetry-roi.md#costs-go-up-roi-goes-down----you-can-explain-that>)
  * [The Predictive Value of Telemetry](<./08 - telemetry-roi.md#the-predictive-value-of-telemetry>)
  * [Cost Reduction Strategies](<./08 - telemetry-roi.md#cost-reduction-strategies>)
* [Organizational Concerns](<./09 - organizational-concerns.md>)
  * [Combating the 'Three Pillars' ideology](<./09 - organizational-concerns.md#combating-the-three-pillars-ideology>)
  * [Build, Buy, and Everything Inbetween](<./09 - organizational-concerns.md#build-buy-and-everything-between>)
  * [Organizing Effective Observability Through Centralization](<./09 - organizational-concerns.md#organizing-effective-observability-through-centralization>)
* [Glossary](<./10 - glossary.md>)
<!-- cspell:enable -->
---

## Contributing

We gladly accept pull requests! Please see [CONTRIBUTING.md](./CONTRIBUTING.md)
for more.

## Special Thanks

This guide is made possible thanks to salary-paying by [Lightstep](https://lightstep.com).

We'd love for you to check out our report on [how OpenTelemetry drives the
future of observability](http://go.lightstep.com/register-oreilly-future-of-observability.html)
as a companion to this whitepaper.

---

This work is licensed under a
[Creative Commons Attribution-ShareAlike 4.0 International License][cc-by-sa].

[![CC BY-SA 4.0][cc-by-sa-image]][cc-by-sa]

[cc-by-sa]: http://creativecommons.org/licenses/by-sa/4.0/
[cc-by-sa-image]: https://licensebuttons.net/l/by-sa/4.0/88x31.png

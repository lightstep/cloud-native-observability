![The Complete Guide to Cloud-Native Observability](./img/header.png)

![Status](https://img.shields.io/badge/status-draft-blue?style=for-the-badge)
![GitHub release (latest by date including
pre-releases)](https://img.shields.io/github/v/release/lightstep/cloud-native-observability?include_prereleases&style=for-the-badge)
![GitHub Repo stars](https://img.shields.io/github/stars/lightstep/cloud-native-observability?style=for-the-badge)

# About This Project

Observability, as a term, is a victim of its own success. Unfortunately, that
success has led to it becoming almost entirely worthless as a descriptor, as it
has been polluted and over-indexed by marketing teams from dozens of companies.
We believe that observability is an essential practice for developers, but doing
so requires that we put forth a strong statement about what it is, and what it
is not.

The purpose of this repository is to collectively define that vision. We aren't
interested in selling any specific product, or in attacking specific people or
competitors. Our goal is to clearly define and state an editorial opinion about
what observability means for cloud-native developers, and the cloud-native
ecosystem. This vision, when realized, should inspire and shape the next
generation of observability tools and practices.

This project is in a _draft_ state. We are actively refining, polishing, and
iterating on the content. Our goal is to spend March 2022 doing refinement, then
make it generally available in April 2022.

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
* [Foreward](<./00 - foreword.md>)
* [Glossary](<./000 - glossary.md>)
* [Introduction](<./01 - intro.md>)
  * [What's the point of software development, anyway?](<./01 - intro.md#whats-the-point-of-software-development-anyway>)
  * [Why Cloud-Native Matters](<./01 - intro.md#why-cloud-native-matters>)
* [Customers and Engineers, Transactions and Resources](<./02 - customers-and-engineers.md>)
  * [Transactions](<./02 - customers-and-engineers.md#transactions>)
  * [Resources](<./02 - customers-and-engineers.md#resources>)
  * [SLI's and SLO's](<./02 - customers-and-engineers.md#slis-and-slos>)
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
  * [Fantastic SLO's and Where To Find Them](<./05 - effective-monitoring.md#fantastic-slos-and-where-to-find-them>)
* [Effective Investigation](<./06 - effective-investigation.md>)
  * [Context -- What Ties Everything Together](<./06 - effective-investigation.md#context----what-ties-everything-together>)
  * [Guided Analysis vs. Data Exploration](<./06 - effective-investigation.md#guided-analysis-vs.-data-exploration>)
  * [Tagging and Cardinality](<./06 - effective-investigation.md#tagging-and-cardinality>)
* [Effective SLO's](<./07 - effective-slo.md>)
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

<!-- cspell:enable -->
---

## Contributing

We gladly accept pull requests! Please see [CONTRIBUTING.md](./CONTRIBUTING.md)
for more.

## Special Thanks

This guide is made possible thanks to [Lightstep](https://lightstep.com).

---

This work is licensed under a
[Creative Commons Attribution-ShareAlike 4.0 International License][cc-by-sa].

[![CC BY-SA 4.0][cc-by-sa-image]][cc-by-sa]

[cc-by-sa]: http://creativecommons.org/licenses/by-sa/4.0/
[cc-by-sa-image]: https://licensebuttons.net/l/by-sa/4.0/88x31.png

![The Complete Guide to Cloud-Native Observability](./img/header.png)

![Status](https://img.shields.io/badge/status-draft-blue?style=for-the-badge)
![GitHub release (latest by date including
pre-releases)](https://img.shields.io/github/v/release/lightstep/cloud-native-observability?include_prereleases&style=for-the-badge)
![GitHub Repo stars](https://img.shields.io/github/stars/lightstep/cloud-native-observability?style=for-the-badge)

# About This Project

The goal of this repository is to present a statement, and a vision, about
observability -- specifically, observability for organizations building and
operating cloud-native software.

This project is in a _draft_ state. We are actively refining, polishing, and
iterating on the content. Our goal is to spend March 2022 doing refinement, then
make it generally available in April 2022.

## What We're Looking For

Currently, we would appreciate feedback (either via issues or pull requests) in
the following areas:

* Structure and Flow
* Comprehensability
* Undefined or Underdefined Concepts/Terms
* Underexplained/Overexplained Concepts/Terms
* General Feedback

---

## Table of Contents

<!-- TODO: Add subsection links to TOC -->

* [README](./README.md)
* [Foreward](<./00 - foreward.md>)
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
  * [Build, Buy, and Everything Inbetween](<./09 - organizational-concerns.md#build-buy-and-everything-inbetween>)
  * [Organizing Effective Observability Through Centralization](<./09 - organizational-concerns.md#organizing-effective-observability-through-centralization>)

---

## Contributing

We gladly accept pull requests! Please see [CONTRIBUTING.md](./CONTRIBUTING.md)
for more.

## Special Thanks

This guide is made possible thanks to [Lightstep](https://lightstep.com).

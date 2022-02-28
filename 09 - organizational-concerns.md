![Chapter 9 - Organizational Concerns](./img/ch9_header.png)

# Organizational Concerns of Observability

The previous chapters have more or less spelled out a vision of a new form of
observability; One that is built around new primitives, like SLO's, and
supported by centralized, multi-telemetry-aware platforms, built on open source
observability libraries. We understand this vision might be a hard sell -- after
all, it doesn't really exist in one place, anywhere.

This chapter will address a mixed bag of institutional concerns about the
feasibility of this vision, objections raised by orthodox practitioners, and the
requirements for successful implementation of cloud-native observability.

## Combating the 'Three Pillars' ideology

Objects in motion tend to stay in motion, this much is known. The momentum of
traditional 'three-pillars' monitoring is difficult to understate. An immense,
incalculable amount of development time and money have gone into platforms that
operate based on this ideology. Existing systems, instrumented for discrete
metrics/tracing/logging experiences, are loathe to change without provable,
exponential improvements in experience.

We argue that although change is hard, it is necessary, and it is also
inevitable. Three pillars isn't a hard and fast law of the universe, it's a
historical accident spurred by the existence of monitoring vendors; They
themselves accidents of a lack of first-party monitoring tools and polyglot
development practices. A fish might rot from the head, but ideology is
reinforced from the bottoms-up, and the primary way three pillars approaches are
institutionalized is through telemetry generation. If you rely on a proprietary
agent to create telemetry and send it off somewhere, you're going to get the
telemetry that is given to you. OpenTelemetry combats this directly, by
recognizing the heavily commodified nature of these agents, and moving telemetry
generation away from proprietary agents, towards direct integrations in
libraries, managed services, orchestration platforms, and so forth.

As OpenTelemetry becomes a de jure standard due to adoption in cloud-native
technologies, the newly found freedom of developers to port their data to other
analytical tools will act as a forcing function on the industry to modify their
products. This should lead to a period of intense competition and consolidation,
as new entrants and incumbents alike are no longer 'safe' due to vendor lock-in.

## Build, Buy, and Everything Between

In this brave new OpenTelemetry world, should you buy or build? The answer
depends on several factors. If we keep in mind that our telemetry data will be
highly portable, then we see the formation of three general camps --

_Pure Builders_: Highly specialized applications of telemetry will always
require highly specialized tooling that general-purpose tools won't provide.
This group will leverage customized, off-the-shelf databases with tailor-made
analysis tools in order to solve challenging, domain-specific problems.

_Mixed Use_: A larger group will choose to leverage one, or more, point
observability solutions in conjunction with managed long-term storage in order
to achieve a 'best of both worlds' approach. Different teams with different
needs may elect to use different paid or free tools, drawing down from a
centralized telemetry repository.

_Pure Buyers_: Possibly as large as the mixed-use group, these organizations
will leverage paid solutions for their overall telemetry needs. Generally,
they'll either be very small organizations that need to outsource expertise to a
vendor, or extremely large ones that prefer OpEx over CapEx.

## Organizing Effective Observability Through Centralization

Successful adoption of observability requires some level of ownership in the
organization. Implementing standards for telemetry quality, determining spend
thresholds and optimizing costs, and managing access and control of potentially
sensitive data are all better served through a centralized owner rather than
letting everyone fend for themselves.

It may be helpful to think of an observability team as a federalized role;
Making rules and guidelines and providing cross-cutting infrastructure that
provides a minimum guarantee for everyone else, rather than trying to embed in a
variety of disparate teams and dictate terms. Centralized teams can ensure
that tooling and processes exist to give service owners easy integration into
the observability stack, guarantees around data quality, dashboards and SLOs to
plug into, and overall visibility into the greater system.

In some organizations, this might be in the office of a CIO or CTO -- in others,
a cloud center of excellence. Regardless of where it sits, there is a lot of
value in having executive sponsorship for observability initiatives, as the most
successful ones tend to be mandated from the top-down. Larger organizations,
with more rigid service boundaries, require a lot more cross-cutting work to do
the heavy lifting of getting an observability framework mandated and implemented
in enough foundational resources to provide basic transaction-level visibility.

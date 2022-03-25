![Chapter 5 - Effective Monitoring](./img/ch5_header.png)

# Effective Monitoring

Traditional monitoring looks a lot like
['Safety-I'](./03%20-%20anatomy-of-observability.md#monitoring-and-alerting),
a model of system reliability that focuses on measuring and preventing _errors_.
This helps explain why most existing monitoring practices are characterized by
an "error-centric" design -- we don't have a great way to quantify what we're
monitoring, exactly, without looking at errors. You can see this manifest in the
acronyms and statements we make about reliability -- mean time to resolution,
root cause, etc. These are all error-centric, in that they focus on errors (and
the resolution of those errors) as the primary thing we should care about.

Cloud-native observability recognizes that errors and faults aren't something
you can prevent, because they're a normal part of system operations. Instead, it
seeks to redefine not only what we monitor, but _why_ we perform monitoring.
Monitoring isn't something we can throw away in its entirety, human operators
will always need the ability to inspect system state. What we need to change is
the instruments we use to monitor that state, and the processes we use to make
sense of it.

## Effective Dashboards

Current approaches to dashboards suffer from many issues, but most analysis of
these systems focus on the symptomatic impacts rather than the root cause of why
these issues occur in the first place. It's useful to think of the problem as a
disconnect between the loan-concept of a 'dashboard' and what we actually get.
In a vehicle, a dashboard displays crucial operator-controllable gauges and
alarms to aid in the operation of said vehicle. This includes fuel status,
speed, engine tachometer, temperature, and a cluster of lights to indicate
subsystem status. Comparatively, a dashboard for a piece of software like Kafka
can include over a dozen line graphs, several large counters, and a handful of
alarms.

The key distinction where these models break down is that everything on a
vehicle dashboard is designed to impart statuses that the operator is fully in
control over, or ones that they require in order to shift their operation in
response to. For example, the speedometer is something you can control as a
driver, the tachometer is critical if you're driving a manual transmission, and
the status of subsystems such as traction control allows you to make an educated
decision on how to control the vehicle in response to external, uncontrollable
events (such as the weather or road condition). Kafka[^dashboardOther]
dashboards, on the other hand, are completely disconnected from the transactions
that they are being used for. As a shared resource, the dashboard for Kafka will
invariably intermingle data from multiple transactions, making it difficult to
pinpoint failures or give actionable feedback to operators on _why_ things are
happening.

> **Design and Dashboards**
>
> Another crucial distinction between vehicular (plane, automobile, etc.)
> dashboards and software ones is that the vehicle dashboards are designed very
> specifically to be used while the vehicle is in _motion_. Items are displayed
> using distinct pictograms, with consistent color coding and placement (such as
> using yellow or red to impart meaning -- 'check engine' is a yellow alarm,
> 'parking brake' is a red one), and very little continuous wear information. A
> vehicle's TPMS (Tire Pressure Monitoring System) is usually in a sub-menu or
> on an infotainment display, for example. Explicitly, these dashboards are
> oriented around _what you need to know to operate the vehicle safely at any
> given moment_. This fundamental design distinction separates them from
> software dashboards, which often intermingle continuous health or state
> reporting with active status indicators, forcing operators to familiarize
> themselves with "what matters in an emergency" vs. "what matters when things
> sound weird". There's a difference between knowing if something is broken, and
> figuring out why it's broken, and our dashboards usually don't support this.

This fundamental mismatch isn't something that can necessarily be corrected by
'better' dashboards, it requires rethinking what we actually use them for, and
what we should be looking at. Currently, software dashboards look a lot like the
internal diagnostics display for a vehicle -- crammed full of hundreds or
thousands of data points that are both difficult to interpret without expertise
_and_ lacking the context and high-level guidance required for operators to
quickly and accurately identify the source of system failures.

Cloud-native observability would suggest that in lieu of these detailed,
diagnostic dashboards, our primary dashboard becomes one that's full of Service
Level Objectives. These SLOs provide useful and actionable feedback to
operators on _what's happening_, based on the actual business goals that our
software is trying to achieve. With dashboards of SLOs, we can immediately
understand the actual health of our transactions and understand how service
health is impacting wider objectives. These dashboards make it immediately
obvious as to the state of a system, not to mention letting us know how much
room we have for error.

## Effective Alerting

"But wait, how do I know when something broke?" you might be asking yourself,
after reading the prior section. Alerts are, traditionally, how we are informed
of system state interruptions that require human intervention. The lived reality
of most engineers is rather different -- alerts are set up at some point based
on recommendations or projections and then they sit stagnant until they annoy
you enough to modify them. This isn't to say alerts are useful or useless,
they're important -- but the amount of time we spend on grooming alert
thresholds is, frankly, ridiculous. If you don't do it, it's probably because
someone else is -- the invisible labor of 'DevOps'.

It follows from our earlier discussion that the most important thing to alert on
are SLO violations and status. What alerts should we focus on, outside of those?
Not a lot, actually -- alerts should be weighed against a three-part test, and
discarded if they don't stack up.

Alerts should be:

* Actionable (I can make a change to resolve this alert)
* Specific (This alert is tied to a logical resource or transaction, or
  aggregation thereof)
* Permanent (Without intervention, this state will not improve)

Non-actionable alerts are the bane of on-call rotations (especially transient,
non-actionable alerts). Being paged because something's broken that you can't
fix is an exercise in frustration! If the alert is tied to a resource or
transaction out of your control that does result in service impact, then that
status is better off tied to your SLO targets. Non-specific alerts are ones that
are too broad; They don't give you enough information to make a determination
about what changed or needs to be changed to resolve them. Non-permanent (or
transient) alerts are the type that resolve themselves without intervention;
Containers crashing (but not crash-looping), intermittent API failures, things
like that.

If your alerts don't pass all three tests, then you should reconsider them.
Often, a 'bad' alert can be made 'good' through better targeting/scoping, but
you should also be fairly liberal with turning off or muting
alerts[^mutingAlerts] and seeing how things go.

## Fantastic SLOs and Where To Find Them

Both of the above sections are written with the implicit and explicit idea that
you're using SLOs as the primary method of understanding reliability. There's a
reason for this -- they're an incredibly powerful and effective tool for not
only quantifying reliability, but also for communicating that reliability to
others. SLOs encapsulate another important property, and that's an acceptance of
errors. As discussed in the introduction to this chapter, traditional monitoring
approaches view errors as an exceptional case to be avoided -- this outlook
doesn't reflect reality. Even if you assume your system to be perfect and free
of bugs, there's things you can't control which can introduce errors through
emergent behavior or unexpected interactions. SLO-based monitoring frees us up
to treat errors as room for experimentation and improvement.

SLOs are a complex topic, with more nuance and detail than have been levied
here. They can appear as somewhat scary, I think -- you'll start to run into a
lot of fancy-looking math which can be a turn-off. If you'd like a much more
in-depth discussion, [Alex Hidalgo's
book](https://www.alex-hidalgo.com/the-slo-book) is a rather comprehensive
introduction and analysis of the topic. That said, we can summarize the benefits
of SLOs here, and how they fit into the bigger picture of cloud-native observability.

A good SLO means something, and it's a way to distill multiple functional
aspects of a software system into a convenient shorthand. The reason you should
be monitoring SLOs is because the SLO ties _meaningful user experiences_ back to
your team. What's another way to phrase a 'meaningful user experience'? It's a
transaction, of course! This is why SLOs map so well to observability, because
the thing we actually care about measuring is the same between them. A SLO
should measure the aggregate health of a particular type of transaction and give
you at-a-glance knowledge about how that transaction is operating in production.
This dramatically simplifies your monitoring workflow, as you're freed up to
focus on what matters instead of slapping down flapping alerts or staring at
stale dashboards.

Even better, since SLOs tie these transactions to business goals and
objectives, they're powerful tools to democratize reliability across an
organization and its customers. Incidents, especially a glut of them, tend to
shake confidence in products -- even amongst coworkers. Broadly communicated
SLOs help people outside of engineering not only understand the actual
performance and reliability of your service, but they can help build confidence
in products by demonstrating how the software relates to specific business
objectives. As the business grows and changes, so too do your SLOs, always
evolving to meet new challenges and opportunities.

For more reading on SLOs, see [Effective SLOs](./07%20-%20effective-slo.md)

[^dashboardOther]: Or 'Kubernetes', or any other sufficiently complex resource.

[^mutingAlerts]: This presupposes that you have defined SLOs for your services.
    Please don't actually start disabling production alerts arbitrarily.

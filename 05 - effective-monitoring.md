![Chapter 5 - Effective Monitoring](./img/ch5_header.png)

# Effective Monitoring

Traditional monitoring looks a lot like youth soccer; Bursts of frentic energy
seperated by long stretches of aimless plucking at blades of grass on a
field[^pitch]. Sometimes you've got two kids over in the corner kicking at a
dandelion they thought was the ball. Everyone gets orange slices and juice at
the end of the day, and we all walk away from a game tired and wondering what
the point was.

These bursts of energy are incidents, of course -- production is down, the
database is dying, VPs are starting to email us complaining about lost business.
These moments, however, aren't the majority of the game; They're bookended by
the liminal space of 'monitoring', the act that supposedly tells us when to
panic and when to rest.

Cloud-native observability promotes a radical new concept of monitoring, one
that moves us away from (as many) terror-inducing point failures and towards a
more holistic flow of understanding and accepting failure as a natural
consequence of operating complex and complicated systems. 

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
events (such as the weather or road condition). Kafka[^dashboardOther] dashboards, on the other
hand, are completely disconnected from the transactions that they are being used
for. As a shared resource, the dashboard for Kafka will invariably intermingle
data from multiple transactions, making it difficult to pinpoint failures or
give actionable feedback to operators on _why_ things are happening.

This fundamental mismatch isn't something that can necessarily be corrected by
'better' dashboards, it requires rethinking what we actually use them for, and
what we should be looking at. Most of the dashboards that are in use today for
software are too low-level for continous monitoring; They simply say too much at
once, and don't provide needed context for teams trying to operate cloud-native
software. 

Cloud-native observability would suggest that in lieu of these detailed,
diagnostic dashboards, our primary dashboard becomes one that's full of Service
Level Objectives. These SLO's provide useful and actionable feedback to
operators on _what's happening_, based on the actual business goals that our
software is trying to achieve. By dashboarding SLO's, we can immediately
understand the actual health of our transactions and understand how service
health is impacting wider objectives. These dashboards make it immediately
obvious as to the state of a system, not to mention letting us know how much
room we have for error.

## Effective Alerting

"But wait, how do I know when something broke?" you might be asking yourself,
after reading the prior section. Alerts are, traditionally, how we are informed
of system state interruptions that require human intervention. The lived reality
of most engineers is rather different -- alerts are set up at some point based
on reccomendations or projections and then they sit stagnant until they annoy
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
* Permanant (Without intervention, this state will continue)

Non-actionable alerts are the bane of on-call rotations (especially transient,
non-actionable alerts). Being paged because something's broken that you can't
fix is an exercise in frustration! If the alert is tied to a resource or
transaction out of your control that does result in service impact, then that
status is better off tied to your SLO targets. Non-specific alerts are ones that
are too broad; They don't give you enough information to make a determination
about what changed or needs to be changed to resolve them. Non-permanant (or
transient) alerts are the type that resolve themselves without intervention;
Containers crashing (but not crash-looping), intermittent API failures, things
like that.

If your alerts don't pass all three tests, then you should reconsider them.
Often, a 'bad' alert can be made 'good' through better targeting/scoping, but
you should also be fairly liberal with turning off or muting
alerts[^mutingAlerts] and seeing how things go.

## Fantastic SLO's and Where To Find Them

Both of the above sections are written with the implicit and explicit idea that
you're using SLOs as the primary method of understanding reliability. There's a
reason for this -- they're an incredibly powerful and effective tool for not
only quantifying reliability, but also for communicating that reliability to
others. You'd do well to do some [more
reading](https://www.alex-hidalgo.com/the-slo-book) about this topic, because
it's honestly too big to cover in its totality here. That said, we want to take
this opportunity to discuss how SLO's fit into this bigger observability
picture, and just why you should be adopting them.

A good SLO means something, and it's a way to distill multiple functional
aspects of a software system into a convenient shorthand. The reason you should
be monitoring SLOs is because the SLO ties _meaningful user experiences_ back to
your team. What's another way to phrase a 'meaningful user experience'? It's a
transaction, of course! This is why SLO's map so well to observability, because
the thing we actually care about measuring is the same between them. A SLO
should measure the aggregate health of a particular type of transaction and give
you at-a-glance knowledge about how that transaction is operating in production.
This dramatically simplifies your monitoring workflow, as you're freed up to
focus on what matters instead of slapping down flapping alerts or staring at
stale dashboards. 

Even better, since SLO's tie these transactions to business goals and
objectives, they're powerful tools to democratize reliability across an
organization and its customers. Incidents, especially a glut of them, tend to
shake confidence in products -- even amongst coworkers. Broadly communicated
SLOs help people outside of engineering not only understand the actual
performance and reliability of your service, but they can help build confidence
in products by demonstrating how the software relates to specific business
objectives. As the business grows and changes, so too do your SLOs, always
evolving to meet new challenges and opportunities.

For more reading on SLO's, see [Effective SLO's](./effective-slo.md)

[^pitch]: Or 'pitch', for our non-American friends.

[^dashboardOther]: Or 'Kubernetes', or any other sufficiently complex resource.

[^mutingAlerts]: This presupposes that you have defined SLOs for your services.
    Please don't actually start disabling production alerts willy-nilly.
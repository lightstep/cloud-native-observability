![Chapter 2 - Customers and Engineers, Transactions and Resources](./img/ch2_header.png)

# Customers and Engineers, Transactions and Resources

There's no useful technology that exists within a vacuum. Features, APIs,
platforms, all of these exist for _reasons_, even if those reasons are somewhat
nebulously defined. Observability, as well, exists for reasons. Some of these
are historical; You can view it as a maturation of existing monitoring
practices, a codification of formerly impractical or unorthodox ideas that
gained acceptance through proof. Observability is also an inevitable outcome of
the complexity of modern systems, and this complexity is also an inevitability
of the systems we live and work in, as bigger is generally thought of as better.
As a lever multiplies forces, observability seeks to increase the amount of
complexity humans running systems are capable of dealing with.

The most important reason for any technology to exist, however, isn't the
historical trajectory of prior technologies nor the reactionary contrivances
that drive innovation, but that there is an audience for it. People need to
benefit from technology for it to truly take root, and thrive. Even basic
algorithms have some human audience in mind that's trying to solve a problem
more efficiently.

Observability is about two distinct groups of people, and their concerns. These
groups are **customers** and **engineers**. Customers are anyone that uses a
system to do something useful. Engineers are anyone who are responsible for that
system. Within those extremely broad categories, there's an entire universe of
possibility; customers have different needs, wants, expectations, desires, and
tolerances for system failure and latency. Engineers, as well, have differing
levels of knowledge, ability, concern, priorities, and reactive capacity to
system failures. What both of them have in common is that they have a
fundamental need to utilize a software system, and they are limited in their
ability to utilize it by analyzing its outputs.

Customers have a deliberately narrowed scope of visibility into a system, and
relatively fewer tools to modify system behavior than an engineer. Engineers
have privileged access and the ability to probe, inspect, and modify a
system's behavior, but have a vastly larger surface area to cover in order to
understand individual failures that a customer may be experiencing. We can
characterize this by saying that customers mostly care about
**transactions**[^txnDisambiguation], whereas engineers must contend with
**resources**.

If customers and engineers are the who's of observability, transactions and
resources are the what's and the why's. These four groups not only help us
understand how observability 'works', but they help define the language that we
use to communicate about reliability in the context of a software system. Let's
start off by taking a deeper dive into transactions and resources, then get into
that shared language.

## Transactions

A transaction is a thing that happens in your system which results in some
useful work, usually crossing some sort of boundary. Transactions can be very
small and granular, such as a CPU instruction to perform some math and store the
result. They can also be extremely large and complex, like the end-to-end order
flow of an e-commerce page. All transactions are composed of many smaller
transactions, and as mentioned, you can get extremely granular with how you
define them.

The most common transactions that we think of when we talk about observability
tend to sit somewhere in the middle of this scale -- for instance, one service
talking to another via an RPC, or a server talking to a database in order to
commit some change or read a value. These transactions tend to be "just right"
in terms of monitoring granularity, because the act of observing transactions
has a cost in terms of emitted telemetry. This is why observing transactions
requires a variety of telemetry signals, which we'll discuss in the [next
chapter](./03%20-%20anatomy-of-observability.md).

### Addressing Complexity

If all transactions happened as a series of steps on a single, logical, thread
it'd be easier to monitor them. However, there are several confounding variables
that we must address -- trust, and scheduling. Both of these have many facets,
so we'll focus on just a few.

Any sufficiently complex transaction will involve the crossing of at least one,
and potentially many, secure boundaries. Since telemetry generation happens
'close' to where a the work of a transaction is being done, this also means that
user data can cross those boundaries as well. Telemetry that generates a shared,
single-transaction context (like a trace) starting from an untrusted client,
moving into a trusted server, can leave purchase for potential vulnerabilities.
In addition, different transactions in the same multi-tenant system can operate
at differing trust levels; even authorized privilege escalation within a client
can intermingle trusted and untrusted data in the same telemetry context.
Addressing this requires not only data hygiene and defensive coding practices,
but a strong understanding of security boundaries within a transaction.

Scheduling adds another wrinkle to transaction telemetry, as well. Asynchronous
dispatch of a transaction (consider multiple independent calls to a profile
service, a products service, an order history service in order to build a user
dashboard in a web client) leads to unbounded context growth, and requires us to
model our transactions to adapt. While to a user, this is one logical request --
"I want to view my dashboard" -- it's up to the developer of a system to decide
how this should be modelled from a telemetry perspective. Further complicating
this is the actual organization and how it's built -- if different teams, in
different time zones, own each part of this transaction independently, what's
the best way to model it so that teams can understand how changes to other parts
of the system affect theirs? The inverse of this is also true; if async dispatch
causes combinatorial growth, then batching causes context collapse as multiple
logical transactions fold into one.

Addressing these complexities requires innovation in telemetry generation and
analysis. Our tools must be able to understand links between different types of
telemetry, so that we can create traces and metrics that not only respect trust
boundaries, but allow us to isolate potentially untrustworthy data. We need to
be able to spatially visualize transactions as they fan-out or fan-in, without
becoming unmanageably complex and difficult to read.

## Resources

If transactions are things that do something, resources are what they consume to
power that work. A resource is durable -- it survives many transactions. Like a
transaction, a resource comes in many shapes, sizes, and scale. A VM, a Docker
container, a Kafka topic, a virtual CPU, a mutex lock... and even a AWS account
can be a resource. What do they all have in common? They're finite. They can be
exhausted. And when they over-saturate, transactions suffer.

We know the symptoms of resource exhaustion well -- database queries that used
to take moments now taking minutes, login services timing out that are normally
bulletproof, pods crash looping in OOMKilled state.

Often when we discuss 'resiliency' in the context of cloud-native systems, what
we really mean is 'resource resilience', not 'transaction resilience'.
Orchestration systems such as Kubernetes will blissfully attempt to restart
failed pods due to application crashes and errors, for example. Some of this is
due to an implicit bias towards 'ops' concerns in the SRE discipline (after all,
it's "site reliability" not "app reliability"), but much of it is because our
reliability model can't accurately capture or model the interactions between
transactions and resources.[^reliabilitySidebar]

### Layered Dependencies

The line between transactions and resources can get a bit blurry. Transactions
can manipulate resources, resource faults can modify
the flow of transactions. These aren't necessarily two frictionless,
non-interactive spheres. Let's illustrate through an example.

Suppose we have a transaction that depends on resource like Kafka. Most of the
time, a Kafka cluster will support multiple topics with a whole battery of
consumers reading and producers writing to them. The transaction is, for the
most part, ignorant about Kafka except through an abstraction layer buried
several libraries deep; It simply writes messages and then polls for a response.
This transaction is part of several other, broader transactions, that extend all
the way back to a command issued by an end-user through a CLI. Kafka itself is
performing transactions constantly, as message brokers receive and dispatch
messages to topics and requests come in for the replay of new statements on each
topic.

Again, we have transactions and resources, layered upon each other. Based on our
responsibilities, we might care more or less about certain parts of this entire
ball of wax, but at the end of the day there's only one thing everyone can agree
on -- someone is making a request, and if it doesn't work, they're not gonna be
terribly thrilled.

Now, suppose this transaction begins to take longer than it usually does. From
the end-user perspective, nothing's changed; They entered a command, and they're
waiting for a response. Our sub-transactions as well don't see anything
different, as they're operating more or less independently, writing and reading
messages. However, on the resource side, Kafka's `client.consumer.lag` metric
just spiked from a few milliseconds to nearly two minutes across all of our
hosts for that topic.

Descriptively, it's easy enough to explain why -- if a resource which a
transaction depends on starts suffering a performance anomaly, then our
transaction will necessarily suffer. If we are only able to look at these two
data sets independently, then that's where our investigation of this problem
would end. The Kafka (Ops) team notices consumer lag has increased, posts a
message to Slack that performance is degraded, and starts to comb through
log files to try and understand why. The users, developers, and others who
depend on that resource are left scratching their heads as to why performance
is impacted unless they happen to be responsible for the abstraction that talks
to Kafka, at which point they start to look at recent pushes to prod, to see if
they broke something.

How do we solve this problem? If you're in a completely disconnected world, it's
hard! Different teams own different parts of this problem, and maybe have
entirely different monitoring stacks. Even if you have all of this different
transactional and resource telemetry in the same tool, you've still got
disconnected experiences where at best you can line up time windows with some
deployment markers to try and narrow the search space.

Stepping back, this is really a bigger problem than it really seems! As your
system grows to include more transaction and resource dependencies, the
ownership of the health of those dependencies becomes more and more distributed.
The cloud is resilient, but it isn't bulletproof -- think about what happens
when us-east-1 suffers an adverse event. Heck, think about what happens when
ancillary parts of the software development toolchain have problems -- [GitHub
failures](https://www.theverge.com/2021/11/27/22805076/github-down-outage-service-issues),
[npm
outages](https://www.reddit.com/r/javascript/comments/q3dk0p/major_npm_outage/),
or simply Greg (the guy who knows how to fix the build) being on a hike with no
cell service. Not only do our transactions and resources have layered
dependencies, our entire development lifecycle has them, and a lot of them are
completely outside our control.

How do we make sense of this? How can we define reliability, quality, or even a
basic understanding of what we require from each party involved in this whole
mess? Our answer is in a couple of short acronyms.

## SLI's and SLO's

The Service Level Indicator (SLI) and Service Level Objective (SLO) have been
popularized by the Site Reliability Engineering (SRE) movement, a practice which
seeks to professionalize the highly technical and often invisible work of
building resilient systems.[^sre]

I won't dwell overmuch on redefining these terms, because you can find
exhaustive discussions of them in the SRE Handbook and other sources. I'm more
interested in discussing this problem within the world we just described, of
customers and engineers, resources and transactions.

An SLI is some quantitative measure of a service -- like request latency, error
rate, or throughput. They are also measurements of a _transaction_, mid-stream.
Another way to phrase this is that the combination of all relevant SLI's for
each sub-transaction in a transaction is the SLI of that transaction -- i.e., if
my user-facing transaction has twelve steps and I sum the duration it takes for
each step, I know the total duration. Indeed, instead of thinking about
transaction health as a trace, or a metric, or the lack of an error log, I can
think of each transaction as an SLI unto itself.

If we extend that metaphor a bit, then the SLO is an agreement about the
acceptable ranges for those transactional SLIs.

It follows that the SLO becomes a contract between transactions and resources to
define acceptable performance bounds, and thus the SLO is a contract between the
customers and engineers themselves. Instead of thinking about observability as
the ability to 'explore unknown unknowns' or 'quickly reduce search space during
incidents', we can begin to think of it as a way to define and iterate on SLI's
and use those to formalize SLO's with each other, setting and communicating
expectations between apps and servers, transactions and resources, customers and
engineers, managers and employees, teams and organizations. The SLO becomes a
mutually agreed upon instrument for talking about change, prioritizing work, or
assessing reliability standards -- and observability becomes our way to monitor
and understand these changes.

[^txnDisambiguation]: To be clear, 'transaction' is any interaction with a
    system and not just a 'database transaction'.
[^sre]: You can read more about Site Reliability Engineering in the [SRE
    Handbook](https://sre.google/sre-book/table-of-contents/) from Google.
[^reliabilitySidebar]: Chaos Engineering is perhaps the biggest counter-factual
    to this proposition, and many interesting examples of transaction/resource
    contention arise from its principles. For example, the
    [LinkedOut](https://www.infoq.com/news/2018/06/linkedout-failure-injection/
    ) failure injection pipeline that allows for individual faults to be bubbled
    into backend systems. That said, I would advance that Chaos Engineering is
    still somewhat niche outside of the most sophisticated teams.

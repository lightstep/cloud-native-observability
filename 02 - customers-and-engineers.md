![Chapter 2 - End-users and Engineers, Transactions and Resources](./img/ch2_header.png)

# End-users and Engineers, Transactions and Resources

## End-users

Cloud-native software applications ultimately exist **to serve their
end-users.** This is why all engineers – or at least all _good_ engineers
– obsess over the quality of their end-users' experiences with that software.
These end-users do _not_ care about Kubernetes, they do _not_ care about
microservices, and they do _not_ care about observability; rather, they care
that the application does its job, quickly and without getting in the way. The
end-users don't care about software, they care about what the software is doing
for them.

## Transactions

End-user interactions with cloud-native software ultimately boil down to
requests that begin in a client (e.g., a mobile phone, a browser, or an SDK),
propagate to the cloud-native services, do their work, then ultimately
terminate, successfully or otherwise. These distinct and distributed journeys
are called "transactions", and if all transactions are successful and
expedient, the software is doing its job for the end-users.

The journeys that these transactions take are often extraordinarily elaborate
in real-world cloud-native software. To make matters even more complicated,
these transactions are not independent: they commingle and interfere with one
another as they consume shared resources like databases, queueing systems (like
Kafka), and even in the network itself.

The work the transactions do can be modeled at many different granularities:
one could reasonably describe transactions in terms of nested [OSI Layer
7](https://en.wikipedia.org/wiki/OSI_model#Layer_7:_Application_layer) HTTP
calls, as function calls, or even as individual CPU instructions. All of these
are valid ways to think about transactions, though depending on the context
they may not be viable ways to sufficiently answer questions about the
transactions or the resources they consume along the way.

### Addressing complexity and interference effects

If all transactions could be represented as a series of distinct, independent
steps taken by a single, logical thread it'd be far easier to model them.
However, there are several confounding variables that we must address --
scheduling, interference effects, and trust.

Non-blocking scheduling is common in cloud-native apps but complicates
transaction tracing telemetry. Asynchronous dispatch of a transaction (consider
multiple independent calls to a profile service, a products service, an order
history service that powers a product purchase in a web client, and an async
job to send a user email confirmation) leads to context growth, and requires
adaptations to our transactional models. While to a user, this is one logical
request -- "I want to view my dashboard" -- it's up to the developer of a
system to decide how this should be represented in the telemetry itself.
Further complicating this is the actual organization and how it's built -- if
different teams, in different time zones, each own distinct segments of the
transaction lifecycle, what's the best way to model it so that teams can
understand how changes to other parts of the system affect theirs?

The inverse of this is also true; since async dispatch causes combinatorial
growth, batching causes context collapse as multiple logical transactions fold
into one. Batching is one of the most common ways to reduce the *resource*
footprint under heavy transactional load, but as soon as transactions are
batched, a trace-per-transaction model breaks down. This modeling issue with
"coalescing effects" has been a noted problem since the original Dapper tracing
paper, and remains problematic in the Span model.

Finally, any sufficiently complex transaction will involve the crossing of at
least one, and potentially many, secure boundaries. Since telemetry generation
happens close to where a the work of a transaction is being done, this also
means that user data can cross those boundaries as well. Telemetry that
generates a shared, single-transaction context (like a trace) starting from an
untrusted client, moving into a trusted server, can leave purchase for
potential vulnerabilities.  In addition, different transactions in the same
multi-tenant system can operate at differing trust levels; even authorized
privilege escalation within a client can intermingle trusted and untrusted data
in the same telemetry context.  Addressing this requires not only data hygiene
and defensive coding practices, but a strong understanding of security
boundaries within a transaction.

Addressing these complexities requires innovation in telemetry generation and
analysis. Our tools must be able to understand links between different types of
telemetry, so that we can create traces and metrics that not only respect trust
boundaries, but allow us to isolate potentially untrustworthy data. We need to
be able to spatially visualize transactions as they fan-out or fan-in, without
becoming unmanageably complex and difficult to read.

## Resources

As these distributed transactions hop from service to service, enter and exit
caches, make system calls, and generally do work, they consume _resources_.
Resources are nearly always _shared_ across multiple transactions, both over
time (i.e., in serial) and/or concurrently (i.e., in parallel). As with
transactions above, resources also can be modeled at many different
granularities: a mutex lock is a resource, CPU cycles are a resource, a
microservice is a resource, a Kafka queue is a resource, and even an AWS quota
can be thought of as a resource.

Crucially, resources are *finite*: you can peg a CPU, you can run out of
memory, and you can flood a Kafka queue. As anyone who's observed production
software can attest, resource exhaustion can get very ugly very quickly for the
transactions that are queueing up and/or failing as a result. We know these
common symptoms well -- database queries that used to take moments now taking
minutes, login services timing out that are normally bulletproof, pods
crash-looping in OOMKilled state.

Often when we discuss 'resiliency' in the context of cloud-native systems, what
we really mean is 'resource resilience', not 'transaction resilience'.
Orchestration systems such as Kubernetes will blissfully attempt to restart
failed pods due to application crashes and errors, for example. Some of this is
due to an implicit bias towards 'ops' concerns in the SRE discipline (after all,
it's "site reliability" not "app reliability"), but much of it is because our
reliability model can't accurately capture or model the interactions between
transactions and resources.[^reliabilitySidebar]

## Engineers

Finally we come to the engineers. For most readers here, "we're talking about you."

Engineers care about their end-users, and thus they certainly *care* about
transactions. That being said, _caring_ about something is different than
_controlling_ something: engineers cannot and **should not** directly control
the behavior of individual transactions, as nothing could be less scalable.
**Engineers only control resources,** not transactions.

## Engineers and End-Users: The Great Tragedy

And this brings us to the tragic relationship of well-meaning engineers and the end-users that depend on their cloud-native software application!

It's like this:

1. **End-users** only care about the **transactions**,
1. **Engineers** care about their **end-users**,
1. ... but engineers can only _control_ their **resources** – not the transactions that flow through them.

This is why we see engineers who care deeply about end-users spend their time staring at dashboards showing CPU usage and pod restart rates. Observability's challenge is to not only help engineers understand transactions and the end-user experience, but to automate the extremely challenging task of understanding how those transactions and the many layers of resources interact.

## Resources and Transactions: The Great Duality

The interdependencies between transactions and resources are manifold and often
complex. Transactions can manipulate resources, resource faults can modify the
flow of transactions. These aren't necessarily two frictionless,
non-interactive spheres. Let's illustrate through an example.

Suppose we have a transaction that depends on resource like Kafka. Most of the
time, a Kafka cluster will support multiple topics with a whole battery of
consumers reading and producers writing to them. The transaction is, for the
most part, ignorant about Kafka except through an abstraction layer buried
several libraries deep; It simply writes messages and then polls for a
response.  This transaction is part of several other, broader transactions,
that extend all the way back to a command issued by an end-user through a CLI.
Kafka itself is performing transactions constantly, as message brokers receive
and dispatch messages to topics and requests come in for the replay of new
statements on each topic.

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
ownership of the health of those dependencies becomes more and more
distributed.  The cloud is resilient, but it isn't bulletproof -- think about
what happens when us-east-1 suffers an adverse event. Heck, think about what
happens when ancillary parts of the software development toolchain have
problems -- [GitHub
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

## SLIs and SLOs

The Service Level Indicator (SLI) and Service Level Objective (SLO) have been
popularized by the Site Reliability Engineering (SRE) movement, a practice which
seeks to professionalize the highly technical and often invisible work of
building resilient systems.[^sre]

An SLI is some quantitative measure of a service -- like request latency, error
rate, or throughput. They are also measurements of a _transaction_, mid-stream.
Another way to phrase this is that the combination of all relevant SLIs for
each sub-transaction in a transaction is the SLI of that transaction -- i.e., if
my user-facing transaction has twelve steps and I sum the duration it takes for
each step, I know the total duration. Indeed, instead of thinking about
transaction health as a trace, or a metric, or the lack of an error log, I can
think of each transaction as an SLI unto itself.

If we extend that metaphor a bit, then the SLO describes the acceptable range
for those transactional SLIs.

It follows that the SLO becomes a contract between transactions and resources
to define acceptable performance bounds, and thus the SLO is a contract between
the end-users and engineers themselves. Instead of thinking about observability
as the ability to 'explore unknown unknowns' or 'quickly reduce search space
during incidents', we can begin to think of it as a way to define and iterate
on SLIs and use those to formalize SLOs with each other, setting and
communicating expectations between apps and servers, transactions and
resources, end-users and engineers, managers and employees, teams and
organizations. The SLO becomes a mutually agreed upon instrument for talking
about change, prioritizing work, or assessing reliability standards -- and
observability becomes our way to monitor and understand these changes.

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

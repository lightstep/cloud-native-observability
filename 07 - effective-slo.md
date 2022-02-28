![Chapter 7 - Effective SLOs](./img/ch7_header.png)

# Effective SLO's and You

There's a lot of ways to communicate reliability, but the SLO is the way that
'fits' into cloud-native observability. An artifact of the SRE Book, SLO's are
the simple and sane way to quantify things that are usually extremely difficult
to quantify. That doesn't mean they're _easy_, but they're essential because
they firmly orient everyone on your team around end-user experience, regardless
of who that end-user is.

What makes an effective SLO, though? Like, a _really_ effective SLO? The
association of SLO's with engineering teams and SRE practitioners has,
possibly unsurprisingly, made them feel somewhat unapproachable to people
outside those spheres. In this section, we'll discuss not how to make an SLO,
but how to use them as bridges between different parts of a business and use
them to drive organizational change by reevaluating how we think about incidents
and incident reporting.

## Counting the Uncountable

The concept of 'data-driven' organizations has become rather en vogue over the
past years. Driven in no small part by a bunch of businesses selling you data
shovels, we're encouraged to collect and quantify increasingly fine-grained data
about everything that happens in the course of doing business. Contacts with
customers, logins and logouts, session lengths, what links people click in
email, who downloads reports or responds to targeted advertising, how often
someone waits before asking for help, etc. This data obsession has also turned
inwards, as companies track ever finer details of their development, marketing,
production, staffing, and every other business process you can think of. This
data all gets sucked up into a variety of databases and spit back out into
reports, digested by managers and executives, and finally translated into broad
pronouncements about the health of the business or a particular initiative.

Tayloristic management practices were said to have fallen out of style, but it's
hard to really tell when you look at KPIs, OKRs, and the myriad of goal-setting
or objective-tracking systems that have become commonplace in organizations.
In fairness, there's a legitimate need to understand what's going on in the
workplace, in order to better allocate and assign resources (like money) and
people (like... people) to projects and initiatives.

Let's compare that, then, to how we report on reliability to the customers of a
service. It looks a lot like this:

| Service Name | Status |
| ------------ | ------ |
| Foobar       | Up     |
| Baz          | Down   |

There's a pretty big disconnect between the level of granularity we expect out
of goals that, frankly, are ill-defined and unquantifiable (such as "Ensure
we're the best place to work!") and systems that have an absolute wealth of
fine-grained performance and reliability data to work with.[^notAllDashboards]

Why does this disconnect exist? It's usually due to cultural reasons. There's an
oft-repeated, possibly apocryphal, anecdote that marking certain AWS services as
'down' on a public dashboard requires VP-level approval. Not all incidents are
created equal, after all, and if nobody was impacted (that you know of) by an
issue, well, did it really ever happen?

SLO's avoid this trap by being directly tied to specific customer experiences
and communicating _expected_ availability ahead of time. This is something that
everyone should understand; KPIs, OKRs, these all have built-in 'pressure
relief' valves that state "hey this is the goal, but we don't expect it to be
100%" -- OKRs, famously, are suggested that you target 30%+ over a 'normal'
result in order to push your team. SLOs understand that binary success or
failure isn't a useful way to communicate reliability.

## Pushing The Envelope

Cloud-native observability posits that SLOs are woefully under-adopted and
underappreciated in software organizations. Much of what you 'do' can be
expressed as an SLO, and those SLOs (or aggregations of a group of them) should
be the primary way reliability is communicated outward from engineering teams
to the rest of the organization, and to your end-users directly.

Expressing reliability via SLO's as the default is rather revolutionary from a
mindset perspective. You quickly realize that you were defaulting to a binary
model of 'up' or 'down', because that's the only real language you had to
communicate performance. SLO's break this, as they introduce not only the
concept of safe failure, but also _acceptable_ failure. This acceptable failure
is where you can improve and iterate -- pushing new features, or cleaning up
technical debt. It's also an affirmative signal to the rest of the organization
that this kind of acceptable failure is expected, and to think about failure not
as a terminal state, but as something that can be managed through process,
prioritization, and effective coping strategies.

That's not just hyperbole, either -- "move fast, break things" is a hyperbolic
expression of this! We accept a certain amount of failure, we even encourage it
in our planning and iteration processes, everywhere except our software. If we
can move towards an SLO-based understanding of reliability, then we can apply
the same level of psychological safety that these mantras try to instill to
people's feelings about our software and products themselves.

SLO's aren't just a way to keep track of how well you're meeting end user needs,
they're a radically transparent way to set boundaries and expectations about the
reliability of your software, your team, and your organization as a whole.

[^notAllDashboards]: It should be noted that there are organizations that
    communicate more fine-grained reliability data (such as availability over
    trailing 30 days) through their public status pages. We salute them.

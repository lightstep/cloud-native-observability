![The Complete Guide to Cloud-Native Observability](./img/header.png)

<!-- TODO: Create 00 - Foreword header -->
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

> COMMENT: Should we link to *Modern Observability with OpenTelemetry* instead of 
> this older report? It frames everything in terms of "three pillars" and I'd 
> like us to be consistent in moving away from that framing, and towards the
> "single braid" framing.


Welcome, to **The Complete Guide to Cloud-Native Observability**.

## Who Is This For?

Our intention is that the reader is broadly familiar with the current concepts
and practices around observability, and the jargon associated with it. A
[glossary](./glossary.md) is provided to aid in defining terms, but we don't
spend a ton of time going in-depth on concepts like sampling, traces,
aggregation, etc. The reader is expected to have a passing familiarity with
these.

The goal of this document is to advance an _opinion_ about how we should be
thinking about, and building, observability systems. We don't want to do it
alone, though, and we don't think that it's helpful for us to prognosticate in
an ivory tower and then plop out a PDF with our thoughts. The reason that this
is on GitHub is expressly so we can get comments, contributions, and changes
from the entire observability world; practitioners, implementors, advocates,
community leaders, whoever. This vision shouldn't just be any one of our's
vision, it should be all of ours vision.

![Chapter 1 - The Why's and Whats of Cloud-Native](./img/ch1_header.png)

# The Why's and Whats of Cloud-Native

In the beginning, there were servers, and we looked upon them and said "what if
Jeff Bezos could make so much money selling books and renting out time on other
people's computers that he could go to space?" Thus, the cloud was born.[^1]

Brief hagiographies aside, it's worth noting that the cloud has absolutely made
an unmistakable impact on the trajectory of not only software developers, but
business in general. If [every company is a tech
company](https://www.wsj.com/articles/every-company-is-now-a-tech-company-1543901207)
then I'd be remiss to speculate that the cloud is a massive reason why.

Fundamentally, the cloud acts as the ultimate 'hack' around the creation and
operation of a successful business, capital expenditure. There's only one reason
that companies go out of business, after all -- they run out of money.
Historically, the sums required to purchase, maintain, upgrade, and operate a
world-class data center were out of reach for most businesses -- and even if they
weren't for yours, you were still artificially gated on the capacity of those
data centers and your ability to grow them to meet demand. In many ways, you can
compare the cloud to the commodification of public infrastructure like water,
power, sewage, and transit. Before these public utilities existed, you may have
been required to drill a well for water, or build a mill for power --
commodification freed up businesses to plug into collective resources and get
straight to work. Similarly, the cloud provides a robust and reliable set of
computing resources for everyone to build on, usually at a fraction of the price
of building them ourselves.

This shift towards servers and compute resources as a commodity led to a new
method of developing and designing software, referred to as 'cloud-native'.
However, this shift has raised new and surprising questions, challenges, and
obstacles to developers who pursue it. To understand how observability relates
to cloud-native, we need to ask a more fundamental question -- why do we develop
software at all?

## What's the point of software development, anyway?

Personally, I got into coding because I was an extremely bullied child (this is
also why I got into musical theater). The ability to make a computer _do
something_ by knowing the right words felt magical.

Strategic business application development is a pretty far cry from the BASIC
databases I wrote to keep track of my baseball card collection, but even as a
child, I was confronted with a tradeoff that has remained true in the nearly
forty years hence: I want to add features, and increase reliability, but I need
to do both as efficiently as possible.

All challenging software development questions can be boiled down into the
rake's maxim of "cheap, fast, good -- pick two". Developing software, be it for
yourself, or for an audience of millions, is a reoccurring set of decisions that
boil down to picking the two that matter most at any given time. Complicating
this is that we live in a globally connected world full of [unexpected
events](https://www.history.com/this-day-in-history/first-confirmed-case-of-coronavirus-found-in-us-washington-state),
[disruptive innovation](https://en.wikipedia.org/wiki/Disruptive_innovation),
[rapid and sustained technological
progress](https://en.wikipedia.org/wiki/Moore%27s_law#/media/File:Moore's_Law_Transistor_Count_1970-2020.png),
and much more. The world keeps happening regardless of our personal desires, and
we're forced to be reactive on some level; If we don't, then we're dead.

To distill this a bit, then, the point of modern software development is **to
accelerate innovation and increase system reliability, for as little money as
possible**. We seek to deliver new features more quickly than not only our
competitors, but also more quickly than we have before. We strive to learn from
the past as our system matures, and improve the operational reliability of the
system. Finally, we want to do all of this on the cheap, so that we're less
fragile in the face of unexpected disruptions _or_ rapidly evolving technology
that forces fundamental changes.

## Why Cloud-Native Matters

If we accept the above premise, then cloud-native becomes a necessity rather
than a pleasant addendum to the way we think about building software. If
software requires money and time, cloud-native can act as a force multiplier to
one, or both, of these inputs. We can leverage the implicit expertise of other
developers and engineers who build these platforms, granting us greater
reliability. Our money is more flexible, as we don't have to invest upfront
capital in the care and feeding of servers to run our programs.

This benefit is so large that it extends across architectures -- analyst reports
find [cloud ROI to be over 4x
self-hosted](https://nucleusresearch.com/wp-content/uploads/2020/11/u176-Cloud-delivers-4.01-times-the-ROI-as-on-premises.pdf),
which is pretty amazing all on its own. Beyond the simple math, cloud-native
software bridges a solutions gap as well, giving you the means to leverage
expertise in _many_ fields to enhance what you can build. Need some
text-to-speech? There's managed cloud services for that. Want to know what's
going on with your software in production? A constellation of open source and
software as a service tools are out there for your perusal.

Beyond the economic arguments, though, cloud-native paints a wonderfully
collaborative picture of how software can benefit our lives and world. The
consolidation of data centers has created a [green
revolution](https://www.nytimes.com/2020/02/27/technology/cloud-computing-energy-usage.html),
where computing power has gone up but energy use has gone down. Open source
software, once regarded as a land of free-speech absolutists and cranks, has
been commercialized and funded by the world's largest companies to build even
more [cloud-native software](https://www.cncf.io), freely given to the next
generation of developers to solve the challenges of today and tomorrow. The
adaptive capacity and scaling benefits of the cloud were on display in full
effect as a global workforce moved from offices to work-from-home, and
[Zoom](https://www.zoom.com) became the lifeline for our personal and
professional lives. It's hard to imagine the future without the cloud, but it's
even harder to imagine the present.

That isn't to say it's all sunshine and roses. Despite the level of access we
have to the cloud, there's vast differences in how teams and organizations
approach and utilize it. There's vast, untapped reservoirs of work to be
accomplished on making it easier and safer to use. Very real concerns exist
around the consolidation of so much computing power within the hands of a few
private companies. That said, the benefits outweigh the risks in most cases for
most people, because the cloud lets you do infinitely more with much less.

[^1]: I use 'cloud' and 'cloud-native' here somewhat interchangeably, but to be
    definitional:
    'The Cloud' refers to highly-scalable, usually virtualized, programmatically
    configurable computing resources that are billed and metered through a
    consumption model. 'Cloud-Native' is software that is written and designed
    with the express benefits, and limitations, of this architecture in mind.

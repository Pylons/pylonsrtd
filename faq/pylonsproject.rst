Pylons Project FAQ
==================

How does "The Pylons Project" relate to Pylons-the-web-framework?
-----------------------------------------------------------------

#. The name Pylons, for some period of time, will continue to refer to the
   Pylons 1.0 web framework (the Python package named "Pylons").

#. At the same time, however, the name "Pylons" will begin to also mean a
   larger project "brand" which encompasses a collection of related
   technologies.

#. Over time, the intention is that the word "Pylons" comes to mean "the
   collection of technologies created by the Pylons community" instead of
   "the Pylons 1.0 web framework", as the Pylons 1.0 web framework will be
   placed into legacy status.

In short, Pylons is meant to become the project name for a collection of
related technologies.  The first new package which is part of the Pylons
Project, Pyramid, is a web framework.  Other packages to the collection will
be added over time, likely including higher-level components such as
applications and other frameworks which rely on a particular persistence
mechanism (Pyramid does not).

Ben Bangert, the Pylons project lead, had already been setting his aim at
layers higher than the core.  This aim, his participation as co-founder of
the new effort, the community of Pylons, and the established brand of Pylons
are all the primary ingredients at play here.  Thus, "the Pylons Project" was
chosen to reflect what's really new: an overall effort combining parts from
different projects.

Why not just continue developing the Pylons 1.0 code-base?
----------------------------------------------------------

Unfortunately, the Pylons 1.0 code-base has hit a point where further
development on it to increase its flexibility and extendability didn't work
out. Due to the use of `sub-classing
<http://be.groovie.org/post/1347858988/why-extending-through-subclassing-a-frameworks>`_
, attempts to re-factor or re-design the existing ``pylons`` core wasn't
working out.

Over the course of several months, several large effects were made to re-write
sections of the ``pylons`` core. Progress went fairly well, but after seeing
that Pylons users would have to port their existing applications to use Pylons
2, and seeing that repoze.bfg already looked extremely similar to the way
Pylons 2 was shaping up, it seemed like a waste of development effort to
recreate a large section of repoze.bfg.

Given this, Ben Bangert started collaborating with Chris McDonough to bring
some of the ``repoze.bfg`` routes functionality up to par with the features
that the stand-alone `Routes <http://routes.groovie.org>`_ has so that Pylons
users wouldn't lose features they depended on. After collaborating for awhile,
the thought shifted from just building Pylons 2 on top of BFG, to a full
merger.

What does the Pylons Project mean for Pylons-the-web-framework?
---------------------------------------------------------------

The Pylons web framework 1.x will continue to be maintained, though not
enhanced.  We will provide a package that allows Pylons 1.x applications and
Pyramid applications to run in the same interpreter.  Going forward, the
Pylons style of application development will be done in Pyramid, with Ben
leading the way.

What does the Pylons Project mean for repoze.bfg?
-------------------------------------------------

The Pyramid codebase is derived almost entirely from an older web framework
named repoze.bfg.  The BFG name is gone, and with the lovefest with Pylons,
it's now indisputable that the-artist-formerly-known-as-BFG is gaining a
wider audience.  BFG's community was primarily comprised of ex-Zope-users.

It's true that repoze.bfg drops its identity and becomes Pyramid, part of the
Pylons Project.  But that's a good thing.  All of us need something with
enough gravity and critical mass to take off and stay relevant for the
long-haul.  We would be doing the repoze.bfg community a disservice by not
taking this opportunity.  The Pylons people are wonderful and we both have
something meaningful to bring to the table.

What does this mean for the Repoze project?
-------------------------------------------

The Repoze project will continue to exist.  In a lot of ways, Repoze will
begin to regain its original focus, which was to "bring Zope technologies to
WSGI".  The popularity of BFG, as its own web framework, apart from Zope
technologies actually obscured this goal.

Why should I care about The Pylons Project?
-------------------------------------------

This really is a good question.  We hope that people are attracted at
first by the spirit of the thing.  It takes humility to sacrifice a
little sovereignty and work together.  Usually the opposite happens
and projects splinter.  We feel there is a limited amount of oxygen in
the space of "top-tier Python web frameworks" and we don't do the
Python community a service by fractal multiplication.

So the first answer is, the spirit of the thing.  We are a group of
project leaders with experience going back to the start of Python web
frameworks.  We aim to bring fresh ideas to classic problems projects
run into.  We also hope to combine a lot of hard-earned maturity into
a secure choice that developers and companies can bet on, along with
the humility and irreverence gained by surviving every stupid decision
that could be imagined.

Why is the Pylons Project different than other projects?
--------------------------------------------------------

Our mantra is: "Small, Documented, Tested, Extensible, Fast, Stable,
Friendly".  Everything we do, from the already-ready Pyramid to the various
batteries we want to do later in batteries-included projects, should retain
these qualities.

Specifically, if you like meat-and-potatoes stuff like insanely great and
up-to-date docs, a magnificently-tested web framework, and a slim execution
footprint, you'll like Pyramid.  If you're also curious in fresh new ideas
about extensibility, come hang out with us and throw your ideas into the mix.

What do you mean by "Friendly"?
-------------------------------

All of us have been around the block a few times.  We've seen good
communities and bad communities, effective communities and
dysfunctional communities, arrogant ones and irreverant ones.  We
pride ourselves on constructive listening, telling the truth even when
it makes us look bad, admitting when we're wrong, and lots of people
who actually like to help.

What does the Pylons Project mean for Zope and Plone?
-----------------------------------------------------

The repoze.bfg people came from the world of Zope and Plone.  Paul, for
example, was a co-founder of Zope and was at the first Python conference at
NIST.  Zope was a tremendous success in the first cycle, with some truly
fresh ideas and a large commercial ecosystem.  Plone continued that in a
second cycle, with an even larger ecosystem and an obvious, out-of-the-box
value proposition.

Since then, other projects are having their cycle.  We love our Zope roots,
the experience we gained helping establish the Zope Foundation and the Plone
Foundation, and consulting experience we have on very large projects.  But we
want to take these experiences, start fresh with one of the clear winners in
the Pylons brand, and work on something for the next cycle.

If you're doing Zope and Plone and have a project that fits their bulls-eye,
use them.  If you have something that could use those ideas for an alternate
need, keep an eye on what we're doing.

How do I participate?
---------------------

Join the Pylons-discuss and/or Pylons-dev maillists on google groups,
or join the #pylons IRC channel on freenode.net.

Where is the code?
------------------

https://github.com/organizations/Pylons


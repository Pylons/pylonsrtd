Pylons Project FAQ
==================

How does "The Pylons Project" relate to Pylons-the-web-framework?
-----------------------------------------------------------------

The Pylons Project was founded by the people behind the Pylons web framework
to develop web application framework technology in Python. Rather than
focusing on a single web framework, the Pylons Project will develop a
collection of related technologies.

The first package from the Pylons Project will be the Pyramid web framework.
Other packages will be added to the collection over time, including
higher-level components, applications and other frameworks which rely
on a particular persistence mechanism (Pyramid does not). Ben Bangert, the
Pylons web framework project lead, is already aiming to develop layers above
the core web framework.

"The Pylons Project" was chosen to reflect the shared core ethos with the
Pylons web framework: an overall effort combining the best parts from
different projects.

Why not just continue developing the Pylons 1.0 code-base?
----------------------------------------------------------

Unfortunately, the Pylons 1.0 code-base has hit a point of diminishing returns
in flexibility and extendability. Due to the use of `sub-classing
<http://be.groovie.org/post/1347858988/why-extending-through-subclassing-a-frameworks>`_
, extensive, sometimes confusing, use of Stacked Object Proxy globals, and
issues with configuration organization, attempts to re-factor or re-design the
existing ``pylons`` core weren't working out.

Over the course of several months, serious attempts were made to re-write
sections of the ``pylons`` core. After realizing that Pylons users would have
to put in extensive effort to port their existing applications, and that
Pylons 2 was looking more and more like ``repoze.bfg``, continued development
seemed a waste of development effort.

Ben Bangert started collaborating with Chris McDonough to bring the
``repoze.bfg`` routes functionality up to par with the stand-alone
`Routes <http://routes.groovie.org>`_ project. Further development showed that
the two projects had much in common, and the developers shifted from building
Pylons 2 on top of BFG and towards a full merger.

What does the Pylons Project mean for Pylons-the-web-framework?
---------------------------------------------------------------

The Pylons web framework 1.x line will continue to be maintained, though not
enhanced. We will provide a package that allows Pylons 1.x applications and
Pyramid applications to run in the same interpreter. The future of
Pylon-style web application development is Pyramid.

What does the Pylons Project mean for repoze.bfg?
-------------------------------------------------

The Pyramid codebase is derived almost entirely from ``repoze.bfg``. Some
changes have been made for the sake of Pylons compatibility, but those
used to development with ``repoze.bfg`` will find Pyramid very familiar. By
merging ``repoze.bfg`` with the philosophically-similar Pylons framework,
both gain a dramatically expanded audience.

What does this mean for the Repoze project?
-------------------------------------------

The Repoze project will continue to exist. Repoze will be able to regain its
original focus: bringing Zope technologies to WSGI. The popularity of BFG as
its own web framework hindered this goal.

Why should I care about The Pylons Project?
-------------------------------------------

This really is a good question. We hope that people are attracted at
first by the spirit of the thing. It takes humility to sacrifice a
little sovereignty and work together. The opposite, forking or splintering
of projects, is much more common in the open source world.  We feel there is a
limited amount of oxygen in the space of "top-tier Python web frameworks" and
we don't do the Python community a service by over-crowding.

We are a group of project leaders with experience going back to the start of
Python web frameworks.  We aim to bring fresh ideas to classic problems.  We
hope to combine a lot of hard-earned maturity into the development of a secure
choice that developers and companies can bet on. Couple this with the humility
and irreverence gained by surviving every stupid decision that could be
imagined, and you've got a good basis for a team of developers.

Why is the Pylons Project different than other projects?
--------------------------------------------------------

Our mantra is: "Small, Documented, Tested, Extensible, Fast, Stable,
Friendly". Everything we do, from Pyramid to the batteries we want to develop
for later "batteries-included" projects, should retain these qualities.

What do you mean by "Friendly"?
-------------------------------

All of us have been around the block a few times. We've seen good
communities and bad communities, effective communities and
dysfunctional communities, arrogant ones and irreverant ones. We
pride ourselves on constructive listening, telling the truth even when
it makes us look bad, admitting when we're wrong, and attracting lots of
people who actually like to help.

What does the Pylons Project mean for Zope and Plone?
-----------------------------------------------------

The repoze.bfg people came from the world of Zope and Plone. Paul, for
example, was a co-founder of Zope and was at the first Python conference at
NIST. Zope was a tremendous success in the first cycle, with some truly
fresh ideas and a large commercial ecosystem. Plone continued that in a
second cycle, with an even larger ecosystem and an obvious, out-of-the-box
value proposition.

Since then, the cycle has moved on and focus has shifted to other projects. We
love our Zope roots, the experience we gained helping establish the Zope
Foundation and the Plone Foundation, and consulting experience we have on
very large projects. But we want to take these experiences and start fresh
together with Pylons, one of the clear winners of the last cycle, to work on
something for the next cycle.

If you're doing Zope and Plone and have a project that fits their bulls-eye,
use them. If you have something that could use those ideas for an alternate
need, keep an eye on what we're doing.

How do I participate?
---------------------

Join the Pylons-discuss and/or Pylons-dev maillists on google groups,
or join the #pylons IRC channel on freenode.net.

Where is the code?
------------------

https://github.com/Pylons


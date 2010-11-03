Pyramid FAQ
===========

What is the difference between Pyramid and Pylons-the-web-framework?
--------------------------------------------------------------------

Pyramid is a new web framework.  It doesn't "share any DNA" with Pylons 1.0.
Existing Pylons 1.0 code will run "inside" Pyramid via the use of a fallback
handler that sends requests to an existing Pylons application.  When run
within the fallback handler, Pylons 1.0 applications may then be ported
piecemeal to Pyramid by converting each bit of functionality into Pyramid
code, allowing the fallback application to handle yet-to-be ported
functionality.

The web framework known as Pylons 1.x will still be maintained by The Pylons
Project indefinitely.  Upon the first non-alpha release of Pyramid, however,
it will go into "legacy" status.

What is the difference Pyramid and repoze.bfg?
----------------------------------------------

Pyramid *is* repoze.bfg, with:

- a new name and a new set of import locations.

- a few added features to meet the expectations of Pylons 1.0 users.

Changes do need to be made to port existing repoze.bfg applications to
Pyramid, however, it is possible to convert an existing repoze.bfg
application to Pyramid in a mostly-automated fashion.  See
http://docs.pylonshq.com/pyramid/dev/tutorials/bfg/index.html for more
information.

As a reference, KARL, using the provided automation, a very large
repoze.bfg application (> 70K lines of code), was ported in 30
minutes.

Why is Pyramid any different than the hundred other Python web frameworks?
--------------------------------------------------------------------------

It's small, documented, tested, extensible, fast, and friendly.  Its core
contributors are long-time Python web framework developers with lots of
experience.  It also has momentum based on a convergence of its (previously
divided) communities.

What do you mean by "Small"?
-----------------------------

Pyramid has roughly 5 thousand lines of code that has the potential to be
executed at runtime.  For more detail, see
http://docs.pylonshq.com/pyramid/dev/designdefense.html#pyramid-is-too-big.

What do you mean by "Documented"?
---------------------------------

Literally *nothing* in Pyramid is undocumented.  Every feature receives
documentation when (or very shortly after, but always before a release) it is
added, and every change and backwards incompatibility is documented in an
easily navigable "what's new" document between major releases.  We require
direct contributors to treat changing documentation as a task that is as
important as changing code.

When renderered to PDF form, the Pyramid documentation consumes more
than 600 pages.  Every "official" Pyramid add-on has a similar level
of documentation.

What do you mean by "Tested"?
-----------------------------

Every Pyramid release has 100% statement coverage via unit and
integration tests.  There is, on average, more than 2 lines of test
code for each line of code that may be executed during a Pyramid
application.

What do you mean by "Extensible"?
---------------------------------

Pyramid has well-documented plug points which allow for integration of
third-party templating systems, sessioning systems, traversal
strategies, url generation, request generation, and generic rendering
(e.g. JSON, et. al).

Pyramid is also a great framework upon which to build *other*
frameworks -- like a content management system -- because it provides
a set of tools and patterns that make it possible to create extensible
applications, such as a declarative configuration system.  See also:
http://docs.pylonshq.compyramid/dev/narr/extending.html

What do you mean by "Fast"?
----------------------------

Pyramid has an exceptionally shallow call stack, and routinely bests other
web frameworks in both speed and execution complexity.  It has been
engineered with speed as a very concrete goal.

What do you mean by "Stable"?
-----------------------------

The first release of Pyramid's predecessor, repoze.bfg, was made in
mid-2008. Over time, new releases of BFG have strived to retain backwards
compatibility with older releases.  Applications written using repoze.bfg
0.6.9 often work unchanged on repoze.bfg 1.3.  We like our users, so we try
to not (within the boundaries of reason and good taste) break backwards
compatibility capriciously.  When we do break backwards compatibility, the
steps to upgrade are always outlined in detail in the new release's "What's
New" document.


Pyramid FAQ
===========

What is the difference between Pyramid and Pylons-the-web-framework?
--------------------------------------------------------------------

Pyramid is a new web framework. It doesn't "share any DNA" with Pylons 1.0.
Existing Pylons 1.0 code will be able to run "inside" Pyramid via the use
of a fallback handler that sends requests to an existing Pylons application.
When run within the fallback handler, Pylons 1.0 applications may be ported
piecemeal to Pyramid. As each bit of functionality is translated into Pyramid
code, the fallback application will continue to handle yet-to-be ported
functionality.

The Pylons 1.x web framework will be maintained indefinitely by The Pylons
Project.  There may be a Pylons 1.1 release aimed at easing a transition to
Pyramid in the near future.  However, as of the release of Pyramid 1.0 on
January 31, 2011, the Pylons web framework has effectively been shifted into
"legacy" status.

.. _should_i_port:

Should I port my Pylons 1.0 project to Pyramid?
-----------------------------------------------

Pyramid 1.0 was released on Jan 31, 2011. 

A draft release of a Pylons-to-Pyramid migration guide is available at
https://bytebucket.org/sluggo/pyramid-docs/wiki/html/migration.html .

We've heard reports from several Pylons users that they have ported smaller
apps without too much difficulty.  For larger Pylons apps, you may want to
wait for the migration guide document to reach non-draft status before
attempting a port.

However, there are a few things you can do now to ease a later migration to
Pyramid:

1) Avoid the use of Pylons global objects except directly in action methods.
   There is no other well-known way to access them, unless 
   self._py_object.request has been implemented.
   
   Pylons global objects refer to 'request', 'session', 'cache', 'response', 
   'tmpl_context', 'config', 'url' objects that are imported from ``pylons``.
   
   This also affects your ability to use your domain models outside of a
   Pylons app (a command line script). Domain models shouldn't depend
   on Pylons globals to work, nor should you pass Pylons globals into class
   methods of your domain models. Pass variables that contain just the
   data the model needs.

2) Ensure all of your routes are explicit and named. All routes in Pyramid
   must be named (uniquely), and there is no minimization available.

If your Pylons app is already set up like this, then your domain models will
most likely require no changes at all. Templates might need slight
alterations and controllers will need some changes.

What is the difference between Pyramid and repoze.bfg?
------------------------------------------------------

Pyramid *is* repoze.bfg, with:

- a new name and a new set of import locations.

- a few added features to meet the expectations of Pylons 1.0 users.

Changes do need to be made to port existing repoze.bfg applications to
Pyramid. It is possible to automate most of the porting process. See
http://docs.pylonsproject.org/projects/pyramid/dev/tutorials/bfg/index.html 
for more information.

As a reference, KARL, a very large repoze.bfg application (> 70K lines of
code), was ported in 30 minutes using the provided automation.

repoze.bfg 1.3 (made November 1, 2010) will be its last major release. Minor
updates will be made for critical bug fixes (and so there may be a 1.3.1,
1.3.2, etc), but new feature development will take place in Pyramid.

Why is Pyramid any different than the hundred other Python web frameworks?
--------------------------------------------------------------------------

It's small, documented, tested, extensible, fast, and friendly. Its core
contributors are long-time Python web framework developers with lots of
experience. It also has momentum based on the combination of its (previously
discrete) communities.

If you like meat-and-potatoes stuff like insanely great and up-to-date docs,
a magnificently-tested web framework, and a slim execution footprint, you'll
like Pyramid. If you're curious in fresh new ideas about extensibility, come
hang out with us and throw your two cents into the mix.

Is a port to Python 3 planned? When?
------------------------------------

We are working on it. Since Pyramid has a few dependencies, we have been
working on helping them being ported first. We hope to be ready for Python 3
before 2011 ends.

What do you mean by "Small"?
-----------------------------

Pyramid has roughly 5 thousand lines of code that has the potential to be
executed at runtime. For more detail, see
http://docs.pylonsproject.org/projects/pyramid/dev/designdefense.html#pyramid-is-too-big.

What do you mean by "Documented"?
---------------------------------

Literally *nothing* in Pyramid is undocumented. Every feature receives
documentation when (or very shortly after, but always before a release) it is
added, and every change and backwards incompatibility is documented in an
easily navigable "what's new" document between major releases. We require
direct contributors to treat changing documentation as a task that is as
important as changing code.

When renderered to PDF form, the Pyramid documentation consumes more
than 600 pages. Every "official" Pyramid add-on has a similar level
of documentation.

What do you mean by "Tested"?
-----------------------------

Every Pyramid release has 100% statement coverage via unit and
integration tests. There is, on average, more than 2 lines of test
code for each line of code that may be executed during a Pyramid
application.

What do you mean by "Extensible"?
---------------------------------

Pyramid has well-documented plug points which allow for integration of
third-party templating systems, session management systems, traversal
strategies, url generation, request generation, and generic rendering
(e.g. JSON, et. al).

Pyramid is also a great framework upon which to build *other*
frameworks -- like a content management system -- because it provides
a set of tools and patterns that make it possible to create extensible
applications, such as its well-documented configuration system.

What do you mean by "Fast"?
----------------------------

Pyramid has an exceptionally shallow call stack, and routinely bests other
web frameworks in both speed and execution complexity. It has been
engineered with speed as a very concrete goal.

What do you mean by "Stable"?
-----------------------------

The first release of Pyramid's predecessor, repoze.bfg, was made in
mid-2008. Over time, new releases of BFG have strived to retain backwards
compatibility with older releases. Applications written using repoze.bfg
0.6.9 often work unchanged on repoze.bfg 1.3. We like our users, so we try
to not (within the boundaries of reason and good taste) break backwards
compatibility capriciously. When we do break backwards compatibility, the
steps to upgrade are always outlined in detail in the new release's "What's
New" document.


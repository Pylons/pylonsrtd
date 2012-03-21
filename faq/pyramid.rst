Pyramid FAQ
===========

What is the difference between Pyramid and Pylons-the-web-framework?
--------------------------------------------------------------------

Pyramid is a new web framework. It doesn't "share any DNA" with Pylons 1.0.
The Pylons 1.x web framework will be maintained indefinitely by The Pylons
Project.  There may be a Pylons 1.1 release aimed at easing a transition to
Pyramid eventually.  However, as of the release of Pyramid 1.0 on January 31,
2011, the Pylons web framework has effectively been shifted into "legacy"
status.

Existing Pylons 1.0 code will be able to run "inside" Pyramid via the use
of a fallback handler that sends requests to an existing Pylons application.
When run within the fallback handler, Pylons 1.0 applications may be ported
piecemeal to Pyramid. As each bit of functionality is translated into Pyramid
code, the fallback application will continue to handle yet-to-be ported
functionality.

.. _should_i_port:

Should I port my Pylons 1.0 project to Pyramid?
-----------------------------------------------

Pyramid 1.0 was released on Jan 31, 2011. 

A draft release of a Pylons-to-Pyramid migration guide is available at
https://bytebucket.org/sluggo/pyramid-docs/wiki/html/migration.html and a
Pyramid guide for users of Pylons is available at
http://docs.pylonsproject.org/projects/pyramid_cookbook/en/latest/pylons/index.html

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

See http://docs.pylonsproject.org/projects/pyramid/en/1.3-branch/narr/introduction.html#what-makes-pyramid-unique

Is a port to Python 3 planned? When?
------------------------------------

Pyramid 1.3a1+ runs on Python 3.2 and better.  Earlier versions run on Python
2 only.


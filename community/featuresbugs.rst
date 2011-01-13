.. _featuresbugs:

Adding Features and Dealing with Bugs
=====================================

Unfortunately no code is perfect, sometimes bugs will occur, or a feature is
desired. When reporting bugs, being as thorough as possible, and including
additional details makes a huge improvement. No one should feel discouraged in
attempting to fix a bug or suggest a feature that might be missing.

Reporting a Bug
---------------

Bugs with a Pylons Project package should be reported to the individual issue
tracker on GitHub_. First, some general guidelines on reporting a bug.

1) Create a GitHub account
!!!!!!!!!!!!!!!!!!!!!!!!!!

You will need to  `create a GitHub account <https://github.com/signup/free>`_
account to report and correspond regarding the bug you are reporting.

2) Determine if your bug is really a bug
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
   
You should not file a bug if you are:
   
* **Proposing features and ideas**: you should follow the policy below on 
  :ref:`proposing_features`.
* **Requesting support**: there are a variety of ways to request support,
  from the `mailing list <http://groups.google.com/group/pylons-devel>`_, 
  `Stackoverflow <http://stackoverflow.com/questions/tagged/pylons>`_, or IRC
  at ``#pylons`` on `FreeNode <http://freenode.net/>`_.

3) Make sure your bug hasn't already been reported
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!

Search through the appropriate Issue tracker on GitHub_ (see
:ref:`issue_trackers` below). If a bug like yours was found, check to see
if you have new information that could be reported to help the developers fix
it.

4) Collect information about the bug
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!

To have the best chance of having a bug fixed, we need to be able to easily
replicate the conditions that caused it. Most of the time this information
will be from a Python traceback message, though some bugs might be in design,
spelling, or other errors on the website/docs/code.

If the error is from a Python traceback (`see a Python traceback 
<http://pastebin.com/TyaPKpt9>`_), include it in the bug report being filed.
We will also need to know what platform you're running (Windows, OSX, Linux),
and which Python interpreter you're running if its not CPython (e.g. Jython, 
Google App Engine).

5) Submit the bug
!!!!!!!!!!!!!!!!!

By default GitHub_ will email you to let you know when new comments have been
made on your bug. In the event you've turned this feature off, you should
check back on occasion to ensure you don't miss any questions a developer
trying to fix the bug might ask.

.. _issue_trackers:

Issue Trackers
--------------

Bugs are reported and tracked on GitHub_'s issue trackers. Each Pylons Project
has their own:

* `pyramid issue tracker <https://github.com/Pylons/pyramid/issues>`_
* `pyramid_beaker issue tracker <https://github.com/Pylons/pyramid_beaker/issues>`_
* `pyramid_xmlrpc issue tracker <https://github.com/Pylons/pyramid_xmlrpc/issues>`_
* `pyramid_jinja2 issue tracker <https://github.com/Pylons/pyramid_jinja2/issues>`_
* `Pylons Project issue tracker <https://github.com/Pylons/pylonshq/issues>`_ (All
  bugs with the pylonshq.com/pylonsproject.org website should be reported here.)

Working on Code
---------------

To fix bugs or add features to a package managed by the Pylons project, an
account on GitHub_ is required. All Pylons Project packages are under the
`Pylons organization on github <http://github.com/Pylons>`_.

The general practice for contributing new features and bug fixes is to `fork
the package <http://help.github.com/forking/>`_ in question and make changes
there. Then send a `pull request <http://help.github.com/pull-requests/>`_.
This allows the developers to review the patches and accept them, or comment
on what needs to be addressed before the change sets can be accepted.

.. _proposing_features:

Proposing Features and Ideas
----------------------------



.. _GitHub: http://github.com/
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

When proposing an idea that isn't just a fix or a plain bug report, the best
place to do so is on the `Pylons-Devel maillist
<http://groups.google.com/group/pylons-devel>`_ .  Another reasonable venue
is IRC at ``#pylons`` on `FreeNode <http://freenode.net/>`_.

Pyramid Add-On Requirements
---------------------------

Along with the "100% test coverage, 100% documentation" requirements of all
packages that wish to be part of the Pylons Project, there are some more
specific guidelines for creating Pyramid add-ons. If you would like your
add-on to be considered for inclusion into the `Pyramid Add-Ons
<http://docs.pylonsproject.org/docs/pyramid.html#pyramid-add-ons>`_ section
of the Pylons Project web site, you should attempt to adhere to these
guidelines.

- An add-on is a package which relies on Pyramid itself.  If your add-on does
  not rely on Pyramid, it's not an add-on (just a library), and it will not
  be listed on the add-ons page.

- The package should be named ``pyramid_foo`` where ``foo`` describes the
  functionality of the package.  For example, ``pyramid_mailer`` is a great
  name for something that provides outbound mail service.  If the name you
  want has already been taken, try to think of another,
  e.g. ``pyramid_mailout``.  If the functionality of the package cannot
  easily be described with one word, or the name you want has already been
  taken and you can't think of another name related to functionality, ``foo``
  can be a codename, e.g. ``pyramid_akhet`` is a package that provides
  Pylons-like application templates: ``akhet`` is the Egyptian phrase
  describing the shape of a "pylon".

- If you are creating a higher-level framework atop the Pyramid codebase that
  contains "template" code (skeleton code rendered by a user via ``paster
  create -t foo``), for the purposes of uniformity with other packages that
  contain templates:

  * It should be possible to subsequently run ``paster serve
    development.ini`` to start any ``paster create`` -rendered application.

  * ``development.ini`` should include ``egg:WebError#evalerror`` middleware
    in the WSGI pipeline.

  * There should be a ``production.ini`` file that mirrors
    ``development.ini`` but disincludes ``egg:WebError#evalerror``.

  * The ``[server:main]`` section of both ``production.ini`` and
    ``development.ini`` should start ``paste.httpserver`` on port 6543, ala::

      [server:main]
      use = egg:Paste#http
      host = 0.0.0.0
      port = 6543

  * ``development.ini`` and ``production.ini`` should configure logging (see
    any existing template).

  * It should be possible to use ``paster pshell development.ini myapp`` to
    visit an interactive shell using a ``paster create``-rendered application.

  * Startup/configuration code should live in a function named ``main``
    within the ``__init__.py`` of the main package of the rendered template.
    This function should be linked within a ``paster.app_factory`` section in
    the template's ``setup.py`` like so::

      entry_points = """\
      [paste.app_factory]
      main = {{package}}:main
      """

    This makes it possible for users to use the following pattern
    (particularly ``use = egg:{{project}}``)::

      [app:{{project}}]
      use = egg:{{project}}
      reload_templates = true
      .. other config ..

  * Middleware configuration should not be inlined into imperative code
    within the ``main`` function.  Instead, middleware should be configured
    within a ``[pipeline:main]`` section in the configuration file, e.g.::

      [pipeline:main]
      pipeline =
          egg:WebError#evalerror
          tm
          {{project}}

- If your package provides "configuration" functionality, you will be tempted
  to create your own framework to do the configuration, ala::

    class MyConfigurationExtender(object):
        def __init__(self, config):
            self.config = config

        def doit(self, a, b):
            self.config.somedirective(a, b)

    extender = MyConfigurationExtender(config)
    extender.doit(1, 2)

  Instead of doing so, use the ``add_directive`` `method of a configurator
  <http://docs.pylonsproject.org/projects/pyramid/1.0/narr/advconfig.html#adding-methods-to-the-configurator-via-add-directive>`_
  ::

    def doit(config, a, b):
        config.somedirective(a, b)

    config.add_directive('doit', doit)

- If your add-on wants to provide some default behavior, provide an
  ``includeme`` method in your add-on's ``__init__.py``, so
  ``config.include('pyramid_foo')`` will pick it up.  See `Including
  Configuration From External Sources
  <http://docs.pylonsproject.org/projects/pyramid/1.0/narr/advconfig.html#including-configuration-from-external-sources>`_.

.. _GitHub: http://github.com/

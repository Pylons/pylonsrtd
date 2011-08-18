.. _addons_and_dev_envs:

Pyramid Add-On and Development Environment Guidelines
=====================================================

Along with the "100% test coverage, 100% documentation" requirements of all
packages that wish to be part of the Pylons Project, there are some more
specific guidelines for creating Pyramid add-ons and "development
environments". If you would like your add-on to be considered for inclusion
into the `Pyramid Add-Ons
<http://docs.pylonsproject.org/docs/pyramid.html#pyramid-add-ons>`_ or
`Development Environments
<https://docs.pylonsproject.org/docs/pyramid.html#pyramid-development-environment-documentation>`_
sections of the Pylons Project web site, you should attempt to adhere to
these guidelines.

An "add-on" is a package which relies on Pyramid itself.  If your add-on does
not rely on Pyramid, it's not an add-on (just a library), and it will not be
listed on the add-ons page.

A separate class of packages exist, which are not simply add-ons, but contain
opinions usually taking shape in the form of "Paster templates", which set up
a locally customized Pyramid application on behalf of users who like that set
of opinions.  These are referred to as "development environments".

Below, we talk about what makes a good add-on and what makes a good
development environment.

Making Good Add-Ons
-------------------

Add-on packages should be named ``pyramid_foo`` where ``foo`` describes the
functionality of the package.  For example, ``pyramid_mailer`` is a great
name for something that provides outbound mail service.  If the name you want
has already been taken, try to think of another, e.g. ``pyramid_mailout``.
If the functionality of the package cannot easily be described with one word,
or the name you want has already been taken and you can't think of another
name related to functionality, use a codename, e.g. ``pyramid_postoffice``.

If your package provides "configuration" functionality, you will be tempted
to create your own framework to do the configuration, ala::

    class MyConfigurationExtender(object):
        def __init__(self, config):
            self.config = config

        def doit(self, a, b):
            self.config.somedirective(a, b)

    extender = MyConfigurationExtender(config)
    extender.doit(1, 2)

Instead of doing so, use the ``add_directive`` method of a configurator as
documented at
http://docs.pylonsproject.org/projects/pyramid/1.0/narr/advconfig.html#adding-methods-to-the-configurator-via-add-directive
::

    def doit(config, a, b):
        config.somedirective(a, b)

    config.add_directive('doit', doit)

If your add-on wants to provide some default behavior, provide an
``includeme`` method in your add-on's ``__init__.py``, so
``config.include('pyramid_foo')`` will pick it up.  See `Including
Configuration From External Sources
<http://docs.pylonsproject.org/projects/pyramid/1.0/narr/advconfig.html#including-configuration-from-external-sources>`_.

Making Good Development Environments
------------------------------------

If you are creating a higher-level framework atop the Pyramid codebase that
contains "template" code (skeleton code rendered by a user via ``paster
create -t foo``), for the purposes of uniformity with other "development
environment" packages, we offer some guidelines below.  `Akhet
<https://docs.pylonsproject.org/projects/akhet/dev/>`_ is a good example of a
development environment that attempts to meet these guidelines.

* It should not be named with a ``pyramid_`` prefix.  For example, instead
  of ``pyramid_foo`` it should just be named ``foo``.  The ``pryamid_``
  prefix is best used for add-ons that plug some discrete functionality in
  to Pyramid, not for code that simply uses Pyramid as a base for a
  separate framework with its own "opinions".

* It should be possible to subsequently run ``paster serve
  development.ini`` to start any ``paster create`` -rendered application.

* ``development.ini`` should ensure that the ``pyramid_debugtoolbar``
  package is active.

* There should be a ``production.ini`` file that mirrors
  ``development.ini`` but disincludes ``pyramid_debugtoolbar``.

* The ``[server:main]`` section of both ``production.ini`` and
  ``development.ini`` should start ``paste.httpserver`` on port 6543, ala::

    [server:main]
    use = egg:Paste#http
    host = 0.0.0.0
    port = 6543

* ``development.ini`` and ``production.ini`` should configure logging (see
  any existing template).

* It should be possible to use ``paster pshell development.ini`` to visit
  an interactive shell using a ``paster create``-rendered application.

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

* WSGI middleware configuration should not be inlined into imperative code
  within the ``main`` function.  Instead, middleware should be configured
  within a ``[pipeline:main]`` section in the configuration file, e.g.::

    [pipeline:main]
    pipeline =
        egg:WebError#evalerror
        tm
        {{project}}


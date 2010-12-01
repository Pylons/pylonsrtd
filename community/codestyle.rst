.. _codestyle:

Coding Style and Standards
==========================

Projects under the Pylons Project scope have rigorous standards for both
coding style, testing, and documentation.

Documentation Styling
---------------------

Every project needs to have documentation built with `sphinx
<http://sphinx.pocoo.org/>`_ using the `pylons sphinx theme
<http://github.com/Pylons/pylons_sphinx_theme>`_ for consistency.

To build documentation using the Pylons theme, first checkout the theme from
github with:

.. code-block:: bash
    
    git clone git://github.com/Pylons/pylons_sphinx_theme.git

Then in the project that needs documentation built with it, under the docs dir
symlink in the pylons theme:

.. code-block:: bash
    
    ln -s /path/to/pylons_sphinx_theme _themes

Then in your conf.py file add the _themes directory to your python path,
assign it to html_theme_path and select you html_theme: 

.. code-block:: python
    :linenos:
    
    sys.path.append(os.path.abspath('_themes'))
    html_theme_path = ['_themes']
    html_theme = 'pylons'

This will then allow you to build the project utilizing the theme, and when
updates are made to the theme the docs can be rebuilt easily but pulling
changes from the git repository.

New Feature Code Requirements
-----------------------------

In order to add a feature to any Pylons Project package:

- The feature must be documented in both the API and narrative
  documentation (in ``docs/``).

- The feature must work fully on the following CPython versions: 2.4,
  2.5, 2.6, and 2.7 on both UNIX and Windows.

- The feature must not cause installation or runtime failure on Jython or App
  Engine. If it doesn't cause installation or runtime failure, but doesn't
  actually *work* on these platforms, that caveat should be spelled out in the
  documentation.

- The feature must not depend on any particular persistence layer (filesystem,
  SQL, etc).

- The feature must not add unnecessary dependencies (where "unnecessary" is of
  course subjective, but new dependencies should be discussed).

The above requirements are relaxed for paster template dependencies. If a
paster template has an install-time dependency on something that doesn't work
on a particular platform, that caveat should be spelled out clearly in *its*
documentation (within its ``docs/`` directory).

To determine if a feature should be added to an existing package, or deserves
a package of its own, feel free to talk to one of the developer teams.

Documentation Coverage
----------------------

If you fix a bug, and the bug requires an API or behavior modification, all
documentation in the package which references that API or behavior must change
to reflect the bug fix, ideally in the same commit that fixes the bug or adds
the feature.

Change Log
----------

Feature additions and bugfixes must be added to the ``CHANGES.txt`` file in
the prevailing style. Changelog entries should be long and descriptive, not
cryptic. Other developers should be able to know what your changelog entry
means.

Test Coverage
-------------

The codebase *must* have 100% test statement coverage after each commit. You
can test coverage via ``python setup.py nosetests --with-coverage`` (requires
the ``nose`` and ``coverage`` packages).

Testing code in a consistent manner can be difficult, to help developers learn
our style ("dogma") of testing we've made available a set of testing notes.

:ref:`testing_guidelines`

Coding Style
------------

All Python code should follow `PEP-8
<http://www.python.org/dev/peps/pep-0008/>`_ style guide-lines. Whitespace
rules are relaxed and it is not necessary to put 2 newlines between classes
(though that's just fine if you do). 80-column lines, in particular, are
mandatory.

* Single-line imports
  
  Do this:

  .. code-block:: python
    :linenos:
    
    import os
    import sys
  
  Do **not** do this:

  .. code-block:: python
    :linenos:
  
    import os, sys
  
  Importing a single item per line makes it easier to read patches and commit
  diffs.

* Import Order
  
  Imports should be ordered by their origin. Names should be imported in
  this order:

  #. Python standard library

  #. Third party packages

  #. Other modules from the current package

* Wildcard Imports
  
  Do *not* import all the names from a package, import just the ones that
  are needed. Single-line imports applies here as well, each name from the
  other package should be imported on its own line.

* No mutable objects as default arguments
  
  Remember that since Python only parses the default argument for a
  function/method just once, they cannot be safely used as default arguments.
  
  Do **not** do this:

  .. code-block:: python
    :linenos:
    
    def somefunc(default={}):
        if default.get(...):
            ...

  Either of these is fine:

  .. code-block:: python
    :linenos:
    
    def somefunc(default=None):
        default = default or {}

  .. code-block:: python
    :linenos:
    
    def somefunc(default=None):
        if default is None:
            default = {}

* Causing others to need to rely on import-time side effects is highly
  discouraged.

  Creating code that requires someone to import a module or package for the
  singular purpose of causing some module-scoped code to be run is highly
  discouraged.  It is only permissible to add such code to the core in paster
  templates, where it might be required by some other framework
  (e.g. SQLAlchemy "declarative base" classes must be imported to be
  registered).

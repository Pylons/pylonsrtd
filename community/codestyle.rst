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

This will then allow you to build the project utilizing the theme, and when
updates are made to the theme the docs can be rebuilt easily.

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

Coding Style
------------

All Python code should follow `PEP-8
<http://www.python.org/dev/peps/pep-0008/>`_ style guide-lines. Whitespace
rules are relaxed and it is not necessary to put 2 newlines between classes
(though that's just fine if you do). 80-column lines, in particular, are
mandatory.

* Single-line imports
  
  Do this::
    
    import os
    import sys
  
  Do **not** do this::
  
    import os, sys
  
  Importing a single item per line makes it easier to read patches and commit
  diffs.

* Import Order
  
  Imports should be ordered by their origin. Names should be imported in
  this order:
    1) Python standard library
    2) 3rd party packages
    3) Other modules from the current package

* No mutable objects as default arguments
  
  Remember that since Python only parses the default argument for a
  function/method just once, they cannot be safely used as default arguments.
  
  Do **not** do this::
    
    def somefunc(default={}):
        if default.get(...):

  Either of these is fine::
    
    def somefunc(default=None):
        default = default or {}

  .. code-block:: python
  
    def somefunc(default=None):
        if default is None:
            default = {}


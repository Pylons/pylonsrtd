.. _codestyle:

Coding Style and Standards
==========================

Projects under the Pylons Project scope have rigorous standards for both
coding style, testing, and documentation.

Documentation Styling
---------------------

Every project needs to have documentation built with `Sphinx
<http://www.sphinx-doc.org/>`_ using either the `Pylons or Pyramid Sphinx Theme
<https://github.com/Pylons/pylons-sphinx-themes>`_ for consistency.

PDF output
~~~~~~~~~~

Set the following values for ``latex_documents`` in ``docs/conf.py``::

    # Grouping the document tree into LaTeX files. List of tuples
    # (source start file, target name, title, author, document class [howto/manual]).
    latex_documents = [
    ('latexindex', 'pyramid_<project name>.tex',
    'Pyramid\_<project name>',
    'Author', 'manual'),
        ]

It is important to use \\_ to escape the underscore in the document
title to prevent a failure in LaTeX.

Comment the following line::

    #latex_logo = '_static/pylons_small.png'

Copy the folder ``pyramid/docs/_static`` (contains two .png files) and the
file ``pyramid/docs/convert_images.sh`` into your ``docs/`` folder.


ePub output
~~~~~~~~~~~

Make sure you have the following value for ``epub_exclude_files``
in ``docs/conf.py``::

    # A list of files that should not be packed into the epub file.
    epub_exclude_files = ['_static/opensearch.xml', '_static/doctools.js',
        '_static/jquery.js', '_static/searchtools.js', '_static/underscore.js',
        '_static/basic.css', 'search.html', '_static/websupport.js' ]

New Feature Code Requirements
-----------------------------

In order to add a feature to any Pylons Project package:

- The feature must be documented in both the API and narrative documentation
  (in ``docs/``).

- The feature must work fully on the CPython 2.6 and 2.7 on both UNIX and
  Windows and PyPy on UNIX.  Most Pylons Project packages now either run or
  want to run on Python 3; if you're working on such a package and it already
  runs on Python 3.2, it must continue to run under Python 3.2 after your
  change.  Some packages explicitly list Python 2.4 or Python 2.5 support;
  such support should be maintained if it exists.  The ``tox.ini`` of most
  Pylons Project packages indicates which versions the package is tested
  under.

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

Testing code in a consistent manner can be difficult, to help developers
learn our style ("dogma") of testing we've made available a set of testing
notes at :ref:`testing_guidelines`.

Coding Style
------------

All Python code should follow a derivation of `PEP-8
<http://www.python.org/dev/peps/pep-0008/>`_ style guide-lines. Whitespace
rules and other rules are relaxed (for example, it is not necessary to put 2
newlines between classes though that's just fine if you do).  Other rules are
relaxed too, such as spaces around operators, and other-such.  79-column lines,
however, are mandatory.

If you do use the ``pep8`` tool for automated checking, here is an invocation
that is close to what is required by the project (subject to change)::

  pep8 --ignore=E302,E261,E231,E123,E301,E226,E262,E225,E303,E125,E251,E201,E202,E128,E122,E701,E203,E222,W293,W291,W391 *.py

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

  If you need to import lots of names from a single package, use:

  .. code-block:: python

     from thepackage import (
         foo,
         bar,
         baz,
         )

  It may look unusual, but it has advantages:

  - It allows one to swap out the higher-level package ``foo`` for something else
    that provides the similar API. An example would be swapping out one database
    for another (e.g., graduating from SQLite to PostgreSQL).

  - Looks more neat in cases where a large number of objects get imported from
    that package.

  - Adding or removing imported objects from the package is quicker and results
    in simpler diffs.

* Import Order
  
  Imports should be ordered by their origin. Names should be imported in
  this order:

  #. Python standard library

  #. Third party packages

  #. Other modules from the current package

* Wildcard Imports
  
  Do *not* import all the names from a package (e.g. never use ``from package
  import *``), import just the ones that are needed. Single-line imports
  applies here as well, each name from the other package should be imported
  on its own line.

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

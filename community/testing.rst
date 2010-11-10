.. _testing_guidelines:

Testing Guidelines
==================

The Pylons Project rather rigorously follows a testing dogma along the lines
describe by Tres Seaver in `Avoiding Temptation: Notes on using 'unittest'
effectively
<http://palladion.com/home/tseaver/obzervationz/2008/unit_testing_notes-20080724>`_
which this document borrows heavily if not entirely from.


Tips for Avoiding Bad Unit Tests
--------------------------------

* Some folks have drunk the "don't repeat yourself" KoolAid: we agree that not
  repeating code is a virtue in most cases, but unit test code is an exception:
  cleverness in a test both obscures the intent of the test and makes a
  subsequent failure massively harder to diagnose.

* Others want to avoid writing both tests and documentation: they try to write
  test cases (almost invariably as "doctests") which do the work of real tests,
  while at the same time trying to make "readable" docs.

Most of the issues involved with the first motive are satisfactorily addressed
later in this document: refusing to share code between test modules makes most
temptations to cleverness go away. Where the temptation remains, the cure is to
look at an individual test and ask the following questions:

* Is the intent of the test clearly explained by the name of the testcase?

* Does the test follow the "canonical" form for a unit test? I.e., does it:
    * set up the preconditions for the method / function being tested.
    * call the method / function exactly one time, passing in the values
      established in the first step.
    * make assertions about the return value, and / or any side effects.
    * do absolutely nothing else.

Fixing tests which fail along the "don't repeat yourself" axis is usually
straightforward:

* Replace any "generic" setup code with per-test-case code. The classic case
  here is code in the setUp method which stores values on the self of the test
  case class: such code is always capable of refactoring to use helper methods
  which return the appropriately-configured test objects on a per-test basis.

* If the method / function under test is called more than once, clone (and
  rename appropriately) the test case method, removing any redundant setup /
  assertions, until each test case calls it exactly once.


Rewriting tests to conform to this pattern has a number of benefits:

* Each individual test case specifies exactly one code path through the method /
  function being tested, which means that achieving "100% coverage" means you
  really did test it all.

* The set of test cases for the method / function being tested define the
  contract very clearly: any ambiguity can be solved by adding one or more
  additional tests.

* Any test which fails is going to be easier to diagnose, because the
  combination of its name, its preconditions, and its expected results are going
  to be clearly focused.

Goals
-----

The goals of the kind of testing outlined here are simplicity, loose or no
coupling, and speed:

* Tests should be as simple as possible, while exercising the application- 
  under-test (AUT) completely.
* Tests should run as quickly as possible, to encourage running them
  frequently.
* Tests should avoid coupling with other tests, or with parts of the AUT which
  they are not responsible for testing.

Developers write such tests to verify that the AUT is abiding by the contracts
the developer specifies. While an instance this type of test case may be
illustrative of the contract it tests, such test cases do not take the place
of either API documentation or of narrative / "theory of operations"
documentation. Still less are they intended for end-user documentation.

Rule: Never import the module-under-test at test module scope.
--------------------------------------------------------------

Import failures in the module-under-test (MUT) should cause individual test
cases to fail: they should never prevent those tests from being run. Depending
on the testrunner, import problems may be much harder to distinguish at a
glance than normal test failures.

For example, rather than the following::

    # test the foo module
    import unittest

    from package.foo import FooClass

    class FooClassTests(unittest.TestCase):

        def test_bar(self):
            foo = FooClass('Bar')
            self.assertEqual(foo.bar(), 'Bar')

prefer::
    
    # test the foo module
    import unittest

    class FooClassTests(unittest.TestCase):

        def _getTargetClass(self):
            from package.foo import FooClass
            return FooClass

        def _makeOne(self, *args, **kw):
            return self._getTargetClass()(*args, **kw)

        def test_bar(self):
            foo = self._makeOne('Bar')
            self.assertEqual(foo.bar(), 'Bar')

Guideline: Minimize module-scope dependencies.
----------------------------------------------

Unit tests need to be runnable even in an enviornment which is missing some
required features: in that case, one or more of the testcase methods (TCMs)
will fail. Defer imports of any needed library modules as late as possible.

For instance, this example generates no test failures at all if the 'qux'
module is not importable::

    # test the foo module
    import unittest
    import qux

    class FooClassTests(unittest.TestCase):

        def _getTargetClass(self):
            from package.foo import FooClass
            return FooClass

        def _makeOne(self, *args, **kw):
            return self._getTargetClass()(*args, **kw)

        def test_bar(self):
            foo = self._makeOne(qux.Qux('Bar'))

while this example raises failures for each TCM which uses the missing
module::

    # test the foo module
    import unittest

    class FooClassTests(unittest.TestCase):

        def _getTargetClass(self):
            from package.foo import FooClass
            return FooClass

        def _makeOne(self, *args, **kw):
            return self._getTargetClass()(*args, **kw)

        def test_bar(self):
            import qux
            foo = self._makeOne(qux.Qux('Bar'))

It may be a reasonable tradeoff in some cases to import a module (but not the
MUT!) which is used widely within the test cases. Such a tradeoff should
probably occur late in the life of the TCM, after the pattern of usage is
clearly understood.

Rule: Make each test case method test Just One Thing.
-----------------------------------------------------

Avoid the temptation to write fewer, bigger tests. Ideally, each TCM will
exercise one set of preconditions for one method or function. For instance,
the following test case tries to exercise far too much::

    def test_bound_used_container(self):
        from AccessControl.SecurityManagement import newSecurityManager
        from AccessControl import Unauthorized
        newSecurityManager(None, UnderprivilegedUser())
        root = self._makeTree()
        guarded = root._getOb('guarded')

        ps = guarded._getOb('bound_used_container_ps')
        self.assertRaises(Unauthorized, ps)

        ps = guarded._getOb('container_str_ps')
        self.assertRaises(Unauthorized, ps)

        ps = guarded._getOb('container_ps')
        container = ps()
        self.assertRaises(Unauthorized, container)
        self.assertRaises(Unauthorized, container.index_html)
        try:
            str(container)
        except Unauthorized:
            pass
        else:
            self.fail("str(container) didn't raise Unauthorized!")

        ps = guarded._getOb('bound_used_container_ps')
        ps._proxy_roles = ( 'Manager', )
        ps()

        ps = guarded._getOb('container_str_ps')
        ps._proxy_roles = ( 'Manager', )
        ps()

This test has a couple of faults, but the critical one is that it tests too
many things (eight different cases).

In general, the prolog of the TCM should establish the one set of
preconditions by setting up fixtures / mock objects / static values, and then
instantiate the class or import the FUT. The TCM should then call the method /
function. The epilog should test the outcomes, typically by examining either
the return value or the state of one or more fixtures / mock objects.

Thinking about the separate sets of preconditions for each function or method
being tested helps clarify the contract, and may inspire a simpler / cleaner /
faster implementation.

Rule: Name TCMs to indicate what they test.
-------------------------------------------

The name of the test should be the first, most useful clue when looking at a
failure report: don't make the reader (yourself, most likely) grep the test
module to figure out what was being tested.

Rather than adding a comment::

    class FooClassTests(unittest.TestCase):

       def test_some_random_blather(self):
           # test the 'bar' method in the case where 'baz' is not set.

prefer to use the TCM name to indicate its purpose::

    class FooClassTests(unittest.TestCase):

       def test_getBar_wo_baz(self):
           #...

Guideline: Share setup via helper methods, not via attributes of 'self'.
------------------------------------------------------------------------

Doing unneeded work in the 'setUp' method of a testcase class sharply
increases coupling between TCMs, which is a Bad Thing. For instance, suppose
the class-under-test (CUT) takes a context as an argument to its constructor.
Rather than instantiating the context in 'setUp'::

    class FooClassTests(unittest.TestCase):

       def setUp(self):
           self.context = DummyContext()

       # ...

       def test_bar(self):
           foo = self._makeOne(self.context)

add a helper method to instantiate the context, and keep it as a local::

    class FooClassTests(unittest.TestCase):

       def _makeContext(self, *args, **kw):
           return DummyContext(*args, **kw)

       def test_bar(self):
           context = self.
           foo = self._makeOne(self.context)

This practice allows different tests to create the mock context differently,
avoiding coupling. It also makes the tests run faster, as the tests which
don't need the context don't pay for creating it.

Guideline: Make fixtures as simple as possible.
-----------------------------------------------

When writing a mock object, start off with an empty class, e.g.::
    
    class DummyContext:
        pass

Run the tests, adding methods only enough to the mock object to make the
dependent tests pass. Avoid giving the mock object any behavior which is not
necessary to make one or more tests pass.

Guideline: Use hooks and registries judiciously.
------------------------------------------------

If the application already allows registering plugins or components, take
advantage of the fact to insert your mock objects. Don't forget to cleanup
after each test!

It may be acceptable to add hook methods to the application, purely to allow
for simplicity of testing. For instance, code which normally sets datetime
attributes to "now" could be tweaked to use a module-scope function, rather
than calling 'datetime.now()' directly. Tests can then replace that function
with one which returns a known value (as long as they put back the original
version after they run).

Guideline: Use mock objects to clarify dependent contracts
----------------------------------------------------------

Keeping the contracts on which the AUT dependes as simple as possible makes
the AUT easier to write, and more resilient to changes. Writing mock objects
which supply only the simplest possible implementation of such contracts keeps
the AUT from acquiring "dependency creep."

For example, in a relational application, the the SQL queries used by the
application can be mocked up as a dummy implementation which takes keyword
parameters and returns lists of dictionaries::

    class DummySQL:

        def __init__(self, results):
            # results should be a list of lists of dictionaries
            self.called_with = []
            self.results = results

        def __call__(self, **kw):
            self.called_with.append(kw.copy())
            return results.pop(0)

In addition to keeping the dependent contract simple (in this case, the SQL
object should return a list of mappings, one per row), the mock object allows
for easy testing of how it is used by the AUT::

    class FooTest(unittest.TestCase):

       def test_barflies_returns_names_from_SQL(self):
           from foo.sqlregistry import registerSQL
           RESULTS = [[{'name': 'Chuck', 'drink': 'Guiness'},
                       {'name': 'Bob', 'drink': 'Knob Creek'},
                      ]]
           query = DummySQL(RESULTS[:])
           registerSQL('list_barflies', query)
           foo = self._makeOne('Dog and Whistle')

           names = foo.barflies()

           self.assertEqual(len(names), len(RESULTS))
           self.failUnless('NAME1' in names)
           self.failUnless('NAME2' in names)

           self.assertEqual(query.called_with, {'bar', 'Dog and Whistle'})

Rule: Don't share text fixtures between test modules.
-----------------------------------------------------

The temptation here is to save typing by borrowing mock objects or fixture
code from another test module. Once indulged, one often ends up moving such
"generic" fixtures to shared modules.

The rationale for this prohibition is simplicity: unit tests need to exercise
the AUT, while remaining as clear and simple as possible.

* Because they are not in the module which uses them, shared mock objects and
  fixtures makes impose a lookup burden on the reader.

* Because they have to support APIs used by multiple clients, shared fixtures
  tend grow to grow APIs / data structures needed only by one client: in the
  degenerate case, become as complicated as the application they are supposed
  to stand in for!

In some cases, it may be cleaner to avoid sharing fixtures even among test
case methods (TCMs) within the same module / class.

Conclusion
----------

Tests which conform to these rules and guidelines have the following properties:

* The tests are straightforward to write.
* The tests yield excellent coverage of the AUT.
* They reward the developer through predictable feedback (e.g., the growing
  list of dots for passed tests).
* They run quickly, and thus encourage the developer to run them frequently.
* Expected failures confirm missing / incomplete implementations.
* Unexpected failures are easy to diagnose and repair.
* When used as regression tests, failures help pinpoint the exact source of
  the regression (a changed contract, for instance, or an underspecified
  constraint).
* Writing such tests clarifies thinking about the contracts of the code they
  test, as well as the dependencies of that code.

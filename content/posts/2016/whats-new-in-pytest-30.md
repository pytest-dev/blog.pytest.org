Title: What's new in pytest 3.0
Date: 2016-07-12 12:00
Tags: whatsnew
Authors: nicoddemus
Status: draft
Summary: Summary of new the features in pytest 3.0, compared to 2.9. 

This blog post provides a short overview of some of the major features and changes in pytest 3.0. See the [CHANGELOG](http://doc.pytest.org/en/latest/changelog.html) for the complete list.

# Additional command alias: `pytest`

pytest came into existence as part of the [py](https://readthedocs.org/projects/pylib/) library, providing a tool called `py.test`.

Even after pytest was moved to a separate project, the `py.test` name for the command-line tool was kept to preserve backward compatibility with existing scripts and tools.

In pytest-3.0 the command-line tool is now called `pytest`, which is easier to type and prevents some common misunderstandings when tools with the same name are installed in the system.

Note that `py.test` still works.

# `approx()`: function for comparing floating-point numbers
 
The `approx` function makes it easy to perform floating-point comparisons using a syntax that's as intuitive and close to pytest's philosophy:

    :::python
    from pytest import approx
    
    def test_similar():
        v = 0.1
        assert (v + 0.2) == approx(0.3)
 
# `yield` statements in `@pytest.fixture`

Fixtures marked with `@pytest.fixture` can now use `yield` statements to provide values for test functions exactly like fixtures marked with `@pytest.yield_fixture` decorator:

    :::python
    import smtplib
    import pytest
    
    @pytest.fixture(scope="module")
    def smtp():
        smtp = smtplib.SMTP("smtp.gmail.com")
        yield smtp
        smtp.close()

This makes it easier to change an existing fixture that uses `return` to use `yield` syntax if teardown code is needed later. Also, many users consider this style more clearer and natural than the previous method of registering a finalizer function using `request.addfinalizer` because the flow of the test is more explicit. Consider:

    @pytest.fixture(scope="module")
    def smtp(request):
        smtp = smtplib.SMTP("smtp.gmail.com")
        request.addfinalizer(smtp.close)
        return smtp        

This change renders `@pytest.yield_fixture` deprecated and makes `@pytest.fixture` with `yield` statements the recommended way to write fixtures which require teardown code.

Note that registering finalizers in `request` is still supported and there's no intention of removing it in future pytest versions.

# `doctest_namespace` fixture
 
The `doctest_namespace` fixture can be used to inject items into the
namespace in which doctests run. It is intended to be used by
auto-use fixtures to provide names to doctests.

`doctest_namespace` is a standard `dict` object:

    :::python
    # content of conftest.py
    import numpy
    @pytest.fixture(autouse=True)
    def add_np(doctest_namespace):
        doctest_namespace['np'] = numpy

Doctests below the `conftest.py` file in the directory hierarchy can now use the `numpy` module directly:

    :::python
    # content of numpy.py
    def arange():
        """
        >>> a = np.arange(10)
        >>> len(a)
        10
        """
        pass 
 
# Fixture `name` parameter

Fixtures now support a `name` parameter which allow them to be accessed by a different name than the function which defines it:

    :::python
    @pytest.fixture(name='value')
    def fixture_value():
        return 10 
        
    def test_foo(value):
        assert value == 10

This solves the problem where the function argument shadows the argument name, which annoys pylint and might cause bugs if one forgets to pull a fixture into a test function.

# Disable capture within a test

`capsys` and `capfd` fixtures now support a `disabled` context-manager method which can be used to disable output capture temporarily within a test or fixture:

    :::python
    def test_disabling_capturing(capsys):
        print('this output is captured')
        with capsys.disabled():
            print('output not captured, going directly to sys.stdout')
        print('this output is also captured')

# `pytest.raises`: regular expressions and custom messages
 
The `ExceptionInfo.match` method can be used to check exception messages using regular expressions, similar to `TestCase.assertRaisesRegexp` method
from `unittest`:

    :::python
    import pytest
    
    def myfunc():
        raise ValueError("Exception 123 raised")
    
    def test_match():
        with pytest.raises(ValueError) as excinfo:
            myfunc()
        excinfo.match(r'.* 123 .*')

The regexp parameter is matched with the `re.search` function.

Also, the context manager form accepts a `message` keyword parameter to raise a custom message when the expected exception does not occur:

    :::python
    def check_input(val):
        if input == "b":
            raise KeyError  
            
    @pytest.mark.parametrize('val', ["a", "b", "c"])
    def test_inputs(val):
        with pytest.raises(KeyError, message="Key error not raised for {}".format(val)):
            check_input(val)

# New hooks

pytest-3.0 adds new hooks, useful both for plugin authors and local `conftest.py` plugins:

* `pytest_fixture_setup(fixturedef, request)`: executes the fixture setup; 
* `pytest_fixture_post_finalizer(fixturedef)`: called after the fixture's finalizer and has access to the fixture's result cache;
* `pytest_make_parametrize_id(config, val)`: allow plugins to provide user-friendly strings for custom types to be used by `@pytest.mark.parametrize` calls;

Complete documentation for all pytest hooks can be found in the [hooks documentation](doc.pytest.org/en/latest/writing_plugins.html).

# Parametrize changes

Parametrize ids can now be set to `None`, in which case the automatically generated id will be used:

    :::python
    import pytest
    @pytest.mark.parametrize(("a,b"), [(1,1), (1,1), (1,2)],
                             ids=["basic", None, "advanced"])  
    def test_function(a, b):
        assert a == b

This will result in the following tests:

* `test_function[basic]`
* `test_function[1-1]`
* `test_function[advanced]`

# New command line options

* `--fixtures-per-test`: 
    Shows which fixtures are being used for each selected test item. Features doc strings of fixtures by default. Can also show where fixtures are defined if combined with `-v`.
    
* `--setup-show`: 
    Performs normal test execution and additionally shows the setup and teardown of fixtures.    

* `--setup-plan`:
    Performs normal collection and reports the potential setups and teardowns, but does not execute fixtures and tests.

* `--setup-only`: 
    Performs normal collection, executes setup and teardown of fixtures and reports them.
     
* `--override-ini`/`-o`:
    Overrides values from the configuration file (`pytest.ini`). For example, `"-o xfail_strict=True"` will make all `xfail` markers act as `strict`, failing the test suite if they exit with `XPASS` status.

* `--pdbcls`:
    Allow passing a custom debugger class that should be used together with the `--pdb` option. Syntax is in the form `<module>:<class:`, for example `--pdbcls=IPython.core.debugger:Pdb`.
    
*  `--doctest-report`:
    Changes the diff output format for doctests, can be one of: `none`, `udiff`, `cdiff`, `ndiff` or `only_first_failure`.

# Documentation Restructuring

While pytest's documentation works as an excellent tutorial, users often ask for a more structured documentation.

XXX: add more, suggestions are welcome.


# Assert reinterpretation is gone

Assertion reinterpretation was the preferred method to display better assertion meessages before assertion rewriting was implemented. Assertion rewriting is safer because there is no risk of introducing subtle errors because of assertions with side-effects. The `--assert=reinterp` option has also been removed.

See this [excellent blog post](http://pybites.blogspot.com.br/2011/07/behind-scenes-of-pytests-new-assertion.html) by Benjamin Peterson for a great overview of the assertion-rewriting mechanism.

In addition to this change, assertion rewriting is now also applied automatically in `conftest.py` files and plugins installed using `setuptools`.

# Feature Deprecation

The pytest team took the opportunity to discuss how to handle feature deprecation during the [pytest 2016 sprint]({filename}pytest-development-sprint.md), intending to minimize impact on the users as well as slowly fade-out some outdated features. It was decided:

## Internal pytest-warnings now are displayed by default

Not showing them by default is confusing to users, as most people don't know how to display them in the first place. Also, it will help wider adoption of new ways of using pytest's features.

## Deadline for deprecated features: new major version

Following [semantic versioning](http://semver.org/), the pytest team will add deprecation warnings to features which are considered obsolete because there are better alternatives or usage is low while maintenance burden on the team is high. Deprecated features will only be removed on the next **major** release, for example pytest-4.0, giving users and plugin authors ample time to update. 

## Deprecated features

The following features have been considered officially deprecated in pytest-3.0, emitting pytest-warnings when used and scheduled for removal in 4.0:

* `yield-based` tests; use `@pytest.mark.parametrize` instead;
* `pytest_funcarg__` prefix to declare fixtures; use `@pytest.fixture` decorator instead;

# Small improvements and bug-fixes

As usual, this release includes a lot of small improvements and bug fixes. Make sure to checkout the full [CHANGELOG](http://doc.pytest.org/en/latest/changelog.html) for the complete list.


 
 
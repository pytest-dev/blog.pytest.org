Title: What's new in pytest 3.3
Date: 2017-11-27 18:00
Tags: whatsnew
Authors: nicoddemus
Summary: Summary of new the features in pytest 3.3, compared to 3.2. 

This blog post provides a short overview of some of the major features and changes in pytest 3.3. See the [CHANGELOG](http://doc.pytest.org/en/latest/changelog.html) for the complete list.

# Python 2.6 and 3.3 support dropped

Pytest 3.3 and onwards will no longer support Python 2.6 and 3.3. Those Python versions are EOL for some time now and incur maintenance and compatibility costs on the pytest core team, and following up with the rest of the community we decided that they will no longer be supported starting with this version. Users using those versions and with a modern enough `pip` should not be affected, otherwise they should pin pytest to < 3.3.


# Logging capture

Pytest now captures and displays output from the standard logging module. The user can control the logging level to be captured by specifying options in pytest.ini, the command line and also during individual tests using markers. Also, a `caplog` fixture is available that enables users to test the captured log during specific tests (similar to `capsys` for example). For more information, please see the [logging docs](https://docs.pytest.org/en/latest/logging.html). This feature was introduced by merging the popular [pytest-catchlog](https://pypi.org/project/pytest-catchlog/) plugin, thanks to [Thomas Hisch](https://github.com/thisch). Be advised that with this merge the backward compatibility interface with the defunct pytest-capturelog has been dropped. 


# Progress display

The pytest output now includes a percentage indicator. Here's the partial output of pytest's own test suite:

```
============================= test session starts =============================
platform win32 -- Python 3.6.3, pytest-3.3.0, py-1.5.2, pluggy-0.6.0
rootdir: C:\Users\appveyor\AppData\Local\Temp\1\devpi-test0\targz\pytest-3.3.0, inifile: tox.ini
plugins: hypothesis-3.38.5
collected 1985 items

testing\acceptance_test.py ...............s............................. [  2%]
...x.............                                                        [  3%]
testing\deprecated_test.py ........                                      [  3%]
testing\test_argcomplete.py ss                                           [  3%]
testing\test_assertion.py .............................................. [  5%]
...........................s.......                                      [  7%]
testing\test_assertrewrite.py ......................................s..s [  9%]
sss...........                                                           [ 10%]
```

The output can be changed back to its former style by configuring your `pytest.ini`:

```ini
[pytest]
console_output_style=classic
```


# New capfdbinary and capsysbinary fixtures

Those two new fixtures return their contents as `bytes` instead of `str`, making them suitable to interact with programs that write binary data
to stdout/stderr.


# pytest.skip() at module level

`pytest.skip` in 3.0 was changed so it could not be used at the module level; it was a common mistake to use it as a decorator in a test function,
when the user should have used `pytest.mark.skip` instead, which would cause the whole module to be skipped.

Some users missed this functionality, so `allow_module_level` can be passed to `pytest.skip` to skip an entire module:

```python
import pytest

pytest.skip('this whole module is TODO', allow_module_level=True)
```


# New dependencies

Pytest now depends on the following modules:

* [attrs](https://pypi.org/project/attrs/)
* [six](https://pypi.org/project/six/)
* [pluggy](https://pypi.org/project/pluggy/)

This change should not affect users.

# Improvements and bugfixes

As usual, this release includes a lot of small improvements and bug fixes. Read the full [CHANGELOG](http://doc.pytest.org/en/latest/changelog.html) for the complete list.


 
 

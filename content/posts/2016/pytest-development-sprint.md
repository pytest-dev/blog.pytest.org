Title: pytest development sprint
Date: 2016-07-07 09:00
Tags: sprint2016
Authors: pfctdayelise
Summary: Summary of happenings at the June 2016 development sprint.


We have just wrapped up the pytest development sprint, which was held 20-25 June 2016 in Freiburg, Germany. Other sprints have taken place at Python conferences, but this was the first dedicated standalone event. We had 27 participants from five continents!

It was funded by [an indiegogo campaign](https://www.indiegogo.com/projects/python-testing-sprint-mid-2016#/) which raised over US$12,000 with nearly 100 backers. Several companies generously donated $300 or more:

* [Dropbox](https://www.dropbox.com/home)
* [Dolby Laboratories](http://www.dolby.com/)
* [Optiver APAC](http://www.optiver.com/sydney/)
* [Splunk](http://www.splunk.com/)
* [Cobe.io](https://cobe.io/)
* [Personalkollen](https://personalkollen.se/)
* [FanDuel](https://www.fanduel.com/)
* [BrightSpec](http://brightspec.com/)

Participants travelled from around Europe and the UK, the US, Brazil, Australia and China.

Work started or finished included the following.

Discussions:

* what to include in pytest 3.0
* advanced intro sessions on plugin mechanics (pluggy) and the fixture system implementation
* tox roadmap
* [Started to conceptualize generalized package builds and virtualenv creation](https://bitbucket.org/hpk42/tox/issues/338/generalize-package-builds-and-virtualenv)
* process for removing deprecated features
* proposal for [combining parametrize with fixtures](https://github.com/pytest-dev/pytest/pull/1660)
* new 'fromcontext'/'invocation' scope for fixtures and some initial prototyping  
* lightning talks

![Post it notes for planning]({attach}images/sprint_postits.jpg){.img-rounded}

Major features:

* dropping pytest assert reinterpret code
* [rename binary from 'py.test' to 'pytest'](https://github.com/pytest-dev/pytest/issues/1629) (but don't worry, ``py.test`` will still work)
* [documentation restructuring](https://github.com/pytest-dev/pytest/wiki/Docs-refactor)
* [``--setup-only`` and ``--setup-plan`` flags which show how fixtures are initialised without actually running the tests](https://github.com/pytest-dev/pytest/pull/1647)
* Adding ``-o`` commandline option to override .ini config values

Other features:

* pytest-qt features - improvements towards a [2.0 release](https://github.com/pytest-dev/pytest-qt/blob/master/CHANGELOG.rst)
* pytest-bdd enhancements
* pytest-django: triaged issues in general and made some progress especially with regard to better handle DB access/setup.
* pytest-html enhancements
* pytest-factoryboy enhancements
* pytest-selenium enhancements
* pytest-repeat enhancements
* pytest-environment research
* cookiecutter-pytest-plugin improvements
* 4 projects [submitted](http://pytest.org/latest/contributing.html#submitting-plugins-to-pytest-dev) to pytest-dev organisation

Bugs fixed/in progress:

* [exit pytest on collection error](https://github.com/pytest-dev/pytest/issues/1421)
* [markers stain on all related classes](https://github.com/pytest-dev/pytest/issues/568)
* [removed deprecated command line options](https://github.com/pytest-dev/pytest/issues/1657)
* [incorrectly dropping brackets on display of assertions](https://github.com/pytest-dev/pytest/issues/925)
* [rename getfuncargvalue to getfixturevalue](https://github.com/pytest-dev/pytest/issues/1625)
* [warning if you use getfuncargvalue with parametrized fixtures](https://github.com/pytest-dev/pytest/issues/460)
* [terminal newlines in failed test output](https://github.com/pytest-dev/pytest/issues/1553)
* [escaping curly braces in a tox command doesn't work](https://bitbucket.org/hpk42/tox/issues/212)
* [Tox shouldn't call pip directly to avoid shebang limitations](https://bitbucket.org/hpk42/tox/issues/66)
* [missing evaluated value in report when asserting a boolean attribute](https://github.com/pytest-dev/pytest/issues/1503)
* [improved determination of rootdir](https://github.com/pytest-dev/pytest/pull/1621)


In total [35 pull requests were merged](https://github.com/pytest-dev/pytest/pulls?utf8=%E2%9C%93&q=is%3Apr%20is%3Amerged%20updated%3A2016-06-20..2016-06-27%20) to pytest, and [at least 12](https://bitbucket.org/hpk42/tox/pull-requests/?state=MERGED) to tox, not to mention many others to plugins.

(It is worth noting that this is not an exhaustive list of all features that will go into pytest 3.0; a separate post will the full release notes will be made once the release is done.)

|   | |
| ------------- | ------------- |
| ![Learning outside]({attach}images/sprint_workingoutside.jpg){.img-rounded} | ![Stickers 2.0]({attach}images/sprint_stickers.jpg){.img-rounded}   |
| ![Meeting a Black Forest local]({attach}images/sprint_horse.jpg){.img-rounded}  | ![Hiking in the sunny Black Forest]({attach}images/sprint_hiking.jpg){.img-rounded}    |
| ![Sampling the local delicacies]({attach}images/sprint_cake.jpg){.img-rounded} | ![Enjoying the beergarden]({attach}images/sprint_beergarden.jpg){.img-rounded} |


We also spent some time designing the [stickers](https://twitter.com/pytestdotorg/status/745528947736092672) and [t-shirts](https://github.com/kvas-it/pytest-design/blob/master/tshirt_example.png). Surveys went out to those who backed the fundraising campaign last week and the goods will be shipped very soon.

Thank you so much to everyone who supported the sprint. We have greatly improved our knowledge of pytest internals, expanded the pytest contributor community, and we're looking forward to bringing you pytest 3.0 very soon (aiming for before [EuroPython](https://ep2016.europython.eu/conference/talks/pytest-30))!

![Group photo]({attach}images/sprint_group.jpg){.img-rounded}
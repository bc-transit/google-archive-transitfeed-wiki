# Prerequisites

* Some familiarity with git tags and branches may be useful. [Git Basiscs - Tagging](http://git-scm.com/book/en/Git-Basics-Tagging) is quite good.
* A Microsoft Windows machine for testing and [[BuildingPythonWindowsExecutables]]; that subpage also contains help how to install Python, utilities, and a Subversion client under Windows.
* Access to update the [PyPI transitfeed](http://pypi.python.org/pypi/transitfeed) package.  That means, you need an account at that site, and an owner of the package needs to grant write access to you or do the update for you.

# Steps

* Ensure that all tests pass.
* Bump the version number in `transitfeed/version.py`
* Tag the version
Get a clean copy with `svn export https://googletransitdatafeed.googlecode.com/svn/branches/transitfeed-1.2.3` and run the tests in Linux and Windows with the nosetests command. 

Manually start the schedule viewer (using test/data/good_feed.zip, for example), click on a stop (an info window of next departures should open), click on a trip and its shape should be displayed with stop times. Try it in a variety of browsers, at least IE and Firefox.

Test `feedvalidator.py` on a large body of feeds to verify that it does not produce spurious error messages on good data (warnings may be acceptable).

## Create a change log

Find the revision of the last release with `svn ls -v https://googletransitdatafeed.googlecode.com/svn/branches`. Display a log of changes with `svn log -r xxx:HEAD https://googletransitdatafeed.googlecode.com/svn/trunk/python`. Use this to list the changes on TransitFeedDistribution. Subversion doesn't do a great job of tracking merge history so some detective work may be needed if your branch and trunk have diverged.

## Windows binary build

Build the pyexe files using the instructions at [BuildingPythonWindowsExecutables].

## Build and upload files

Using the exported tree build the source tar ball in unix with `python setup.py sdist`. Upload it to this project and set the tags `Type-Source` and `OpSys-All`.  Send an announcement to the googletransitfeed group.

## Finalize the release

Tag the release version:

`git tag -a transitfeed-x.y.z -m "transitfeed-x.y.z" [optional commit identifier]`

Move the label Featured from the previous to this release.

Update PyPI with {{{python setup.py register sdist}}}.

## Announce it

Send an email to googletransitdatafeed@googlegroups.com.
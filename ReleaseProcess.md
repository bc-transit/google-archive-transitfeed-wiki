# Prerequisites

You need a Microsoft Windows machine for testing and [BuildingPythonWindowsExecutables]; that subpage also contains help how to install Python, utilities, and a Subversion client under Windows.

Some familiarity with subversion tags and branches may be useful. [Version Control with Subversion](http://svnbook.red-bean.com/en/1.5/svn.branchmerge.html) is quite good.

For the final step, you need access to update the [PyPI transitfeed](http://pypi.python.org/pypi/transitfeed) package.  That means, you need an account at that site, and an owner of the package needs to grant write access to you or do the update for you.

# Steps

## Check that trunk is ready

Make sure `transitfeed.__version__` is set to the new version in trunk and all tests pass.

## Make a branch

`svn cp -m "make version 1.2.3 branch" https://googletransitdatafeed.googlecode.com/svn/trunk https://googletransitdatafeed.googlecode.com/svn/branches/transitfeed-1.2.3`

## Run tests

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

Freeze the release branch with a Subversion tag:
`svn cp -m "make version x.y.z tag" https://googletransitdatafeed.googlecode.com/svn/branches/transitfeed-1.2.3 https://googletransitdatafeed.googlecode.com/svn/tags/transitfeed-1.2.3`

Note: once you have created the release's Subversion tag, older versions of feedvalidator will tell users to upgrade.

Move the label Featured from the previous to this release.

Update PyPI with {{{python setup.py register sdist}}}.

## Announce it

Send an email to googletransitdatafeed@googlegroups.com. If you have access also put something on https://groups.google.com/group/google-transit-help-announcements .
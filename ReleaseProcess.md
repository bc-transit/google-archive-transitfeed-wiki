# Prerequisites

* Some familiarity with git tags and branches may be useful. [Git Basiscs - Tagging](http://git-scm.com/book/en/Git-Basics-Tagging) is quite good.
* A Microsoft Windows machine for testing and [[BuildingPythonWindowsExecutables]]; that subpage also contains help how to install Python, utilities, and a Subversion client under Windows.
* Access to update the [PyPI transitfeed](http://pypi.python.org/pypi/transitfeed) package.  That means, you need an account at that site, and an owner of the package needs to grant write access to you or do the update for you.

# Steps

* Ensure that all tests pass.
* Bump the version number in `transitfeed/version.py`
* Tag the version: `git tag -a X.Y.Z -m "Tag for X.Y.Z release."`
* Publish your tag: `git push origin X.Y.Z`
* Create a new Release in GitHub
  * Create release notes from the commits and issues that were included in the release.
* Build and test Windows binaries: [[BuildingPythonWindowsExecutables]]
  * Attach the zip file of binaries to the GitHub release.
* Publish the release to PyPI: `python setup.py register sdist upload`
* Update the release version string in [[LatestReleaseVersion]].
* Send an email to transitfeed@googlegroups.com.
Python and GitHub are great, but they're not very straightforward for non-programmers to use.  The sample applications that we've built in Python will be useful to more people if they can be downloaded as standalone Windows executables.  Here are some notes on how to use `py2exe` to build Windows apps.

# Initial Setup

* Install `python-2.7` from http://python.org/
* Install `py2exe` for `win32-py2.7` from http://www.py2exe.org/
* Install `setuptools` from http://pypi.python.org/pypi/setuptools/
  * You'll need to make sure the setuptools.egg has been unzipped as well, otherwise the py2exe tool won't be able to find the `pkg_resources` module and you'll get strange warnings about uncommon timzone names in the validator because the `pkg_resource` module is not available to `pytz` to find timezone files.
* `c:\Python27\Scripts\easy_install.exe --always-unzip pytz`
* `c:\Python27\Scripts\easy_install.exe --always-unzip simplejson`
* `c:\Python27\Scripts\easy_install.exe nose`
* Install `git` from http://git-scm.com/download/win

# Steps

Run one or two of the scripts directly to make sure they are not broken. Using nose (installed as above), run all automated tests (`\Python27\Scripts\nosetests.exe`). Make sure the same number of tests ran in linux. Note also issue 37.

Change into the python directory of the exported code and run:
`C:\Python27\python.exe setup.py py2exe`

This will create a directory `transitfeed-windows-binary-<version>`. (The name was picked so that it appears after the source tar.gz on the project home page. `easy_install transitfeed` fetches the first archive file it finds.) Put it into a new zip file with the name `transitfeed-windows-binary-<version>.zip` by right clicking on the folder and choosing Send To -> Compressed (zipped) Folder. Make sure the name includes the full version number.

Run all the exe files to make sure they work. Then upload the zip file to the project Downloads page with tags `Type-Archive` and `OpSys-Windows`.

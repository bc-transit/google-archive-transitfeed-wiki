Python and Subversion are great, but they're not very straightforward for non-programmers to use.  The sample applications that we've built in Python will be useful to more people if they can be downloaded as standalone Windows executables.  Here are some notes on how to use `py2exe` to build Windows apps.

# Ingredients

* [Python 2.5 for Windows](http://www.python.org/download/releases/) - py2exe doesn't recognize the Cygwin version, so you'll need to have this one around
* [py2exe](http://sourceforge.net/projects/py2exe/files/)
* [pytz (zip/gz)](http://pypi.python.org/pypi/pytz/) - py2exe doesn't understand Python eggs, so you'll need to [install this in unzipped form](http://www.py2exe.org/index.cgi/ExeWithEggs)
* [simplejson](http://pypi.python.org/pypi/simplejson/) - py2exe doesn't understand Python eggs, so you'll need to [install this in unzipped form](http://www.py2exe.org/index.cgi/ExeWithEggs)
* [nose](http://code.google.com/p/python-nose/)

You will also need a Subversion client to get hold of the code and Nose to run the tests.

*NOTE:* Make sure to use 32-bit versions of Python and py2exe.  A 64-bit release won't work for most users.

For Nov 2009 here is how I install these on a Windows XP machine. Some of them expect the Windows Installer to be available, but that should anyways be the case.

* Install python2.5.4 from http://python.org/
* Install py2exe-0.6.9.win32-py2.5.exe from http://www.py2exe.org/
* Install setuptools-0.6c11.win32-py2.5.exe from http://pypi.python.org/pypi/setuptools/
  * You'll need to make sure the setuptools.egg has been unzipped as well, otherwise the py2exe tool won't be able to find the `pkg_resources` module and you'll get strange warnings about uncommon timzone names in the validator because the `pkg_resource` module is not available to `pytz` to find timezone files.
* `c:\Python25\Scripts\easy_install.exe --always-unzip pytz` gave me pytz 2009r
* `c:\Python25\Scripts\easy_install.exe --always-unzip simplejson` gave me simplejson-2.0.9-py2.5-win32
* `c:\Python25\Scripts\easy_install.exe nose` gave me nose-0.11.1
* Install TortoiseSVN-1.6.6.17493-win32-svn-1.6.6 from http://tortoisesvn.tigris.org/ , which needed a reboot after installation.

# Steps

Make a new directory and do `svn export http://googletransitdatafeed.googlecode.com/svn/branches/transitfeed-<version>/`.  To do that with TortoiseSVN, you go into the directory with Explorer, right-click, and select TortoiseSVN >> Export.

Run one or two of the scripts directly to make sure they are not broken. Using nose (installed as above), run all automated tests (`\Python25\Scripts\nosetests.exe`). Make sure the same number of tests ran in linux. Note also issue 37.

Change into the python directory of the exported code and run:
`C:\Python25\python.exe setup.py py2exe`

This will create a directory `transitfeed-windows-binary-<version>`. (The name was picked so that it appears after the source tar.gz on the project home page. `easy_install transitfeed` fetches the first archive file it finds.) Put it into a new zip file with the name `transitfeed-windows-binary-<version>.zip` by right clicking on the folder and choosing Send To -> Compressed (zipped) Folder. Make sure the name includes the full version number.

Run all the exe files to make sure they work. Then upload the zip file to the project Downloads page with tags `Type-Archive` and `OpSys-Windows`.

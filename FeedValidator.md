# Introduction

The feedvalidator is a command line tool that checks a GTFS feed for problems and generates a HTML report.  Running it on your transit data feed and fixing the issues that it finds can save you from display and routing problems down the road.

For a list of the errors and warnings that FeedValidator outputs, see the FeedValidatorErrorsAndWarnings page.

You may run the validator on an uncompressed directory of GTFS txt files. I recommend you validate your final google_transit.zip just before publishing it to catch any mistakes creating the zip. If you have an script that generates your GTFS file consider making validation a final step and aborting if it exits with an error.

# Windows standalone version

We provide a Windows executable version for convenience; if you're running Windows, you should use this one.  To get it, download the latest `transitfeed-(version)-windows.zip` from the [project download page](...).

Once you have it downloaded and unzipped, there are a few ways to run it:

## Easiest

Drag a GTFS zip file or directory onto `feedvalidator.exe`.  A window will pop up with the result of the validation test:

[[validation-complete.png]]

## Intermediate

Double-click `feedvalidator.exe`.  A window will pop up and ask you to enter the location of your feed file or directory.  You can type it in, or just drag a GTFS file or directory onto the window and hit Enter.

## Expert
Go to the Windows command prompt, and type: `feedvalidator <name of feed file or directory>`.  If you want to avoid the prompt at the end of the validation, use the `--noprompt` parameter.

# Python source code version

Use this version if you're on Mac OS X or Linux.

Before you run the feed validator you must install Python. It is tested with versions 2.4 and 2.5 and should work with 2.6. You can download Python from http://www.python.org/download/

Fetch the transitfeed distribution from our source code repository (see Source tab above) or by downloading `transitfeed-(version).tar.gz` from the [project download page](...).

## Running

Run the feed validator as
```
feedvalidator.py <feed filename>
```

The warning "Timezone not checked (install pytz package for timezone validation)" is normal. feedvalidator can not check the spelling of the agency_timezone until you install the pytz package.


## Extra help for Windows XP/2000 users

Open a new Command Prompt window. Change into the directory containing feedvalidator.py (Hint: type "cd " without quotes and drag the icon of the folder into command prompt window). Run feedvalidator.py <feed filename>. If you don't want to type the feed filename try dragging it into the command prompt window.

# Command line options

```
Usage: feedvalidator.py [options] [<input GTFS.zip>]

Validates GTFS file (or directory) <input GTFS.zip> and writes a HTML
report of the results to validation-results.html.

If <input GTFS.zip> is ommited the filename is read from the console. Dragging
a file into the console may enter the filename.

For more information see
http://code.google.com/p/googletransitdatafeed/wiki/FeedValidator


Options:
  --version             show program's version number and exit
  -h, --help            show this help message and exit
  -n, --noprompt        do not prompt for feed location or load output in
                        browser
  -o FILE, --output=FILE
                        write html output to FILE or --output=CONSOLE to print
                        all errors and warnings to the command console
  -p, --performance     output memory and time performance (Availability: Unix
  -m, --memory_db       Use in-memory sqlite db instead of a temporary file.
                        It is faster but uses more RAM.
  -d, --duplicate_trip_check
                        Check for duplicate trips which go through the same
                        stops with same service and start times
  -l LIMIT_PER_TYPE, --limit_per_type=LIMIT_PER_TYPE
                        Maximum number of errors and warnings to keep of each
                        type
  --latest_version=LATEST_VERSION
                        a version number such as 1.2.1 or None to get the
                        latest version from code.google.com. Output a warning
                        if transitfeed.py is older than this version.
  --service_gap_interval=SERVICE_GAP_INTERVAL
                        the number of consecutive days to search for with no
                        scheduled service. For each interval with no service
                        having this number of days or more a warning will be
                        issued
```

# Revision History

See the TransitFeedDistribution page.
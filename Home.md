The TransitFeed library contains several parts, each with its own instructions:

* [[TransitFeed]] - a Python package for reading, writing, and validating feeds
* [[FeedValidator]] - a command line tool that checks a GTFS feed for problems
* [[ScheduleViewer]] - an application for exploring a feed on a map   
* [[KMLWriter]] - an application for plotting a feed's stops in a KML file for viewing in Google Earth
* [[Merge]] - combines two GTFS files into one
* [[GoogleRandomQueries]] - an example program. generates random queries for Google Maps trip planner.
* [[UnusualTripFilter]] - sets the trip_type depending on how often a pattern is used

# Download

You may download the transitfeed distribution as Windows executables, a source tar ball, or directly from our GitHub repository.

## Windows Executable

If you just want to run the tools, you may download prebuilt Windows programs from:

https://github.com/google/transitfeed/releases/latest

Download the latest version of `transitfeed-<version>-windows.zip` and unzip the contents into an empty directory.  Then see the [[FeedValidator]] and [[ScheduleViewer]] pages for further instructions.

## Install source distribution

To install the `transitfeed` Python library:

```
easy_install transitfeed
```

Some features of the distribution depend on pytz. You may need to install it manually.

If you don't have `easy_install` on your computer you may download [the latest source distribution](https://github.com/google/transitfeed/releases/latest) directly.

# Bugs and feature requests

Please create a new issue if you find bugs or have a feature requests. Patches released under the terms of the Apache 2.0 license are more than welcome!
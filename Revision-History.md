#### 1.2.12 (2012-07-30)

* Issue 326: Add South Sudan to language-subtag-registry.
* Issue 327: Fix for crash in FeedValidator when stop in stops.txt does not include location information and stop-station hierarchy is used.
* Issue 328: !ValidateNoDuplicateStopSequences produces a confusing error message.
* Issue 341: FeedValidator crashes when loading frequencies.txt with no header specified.
* Issue 343: GTFS Validation Problem in Merge Tool Causes Crash

#### 1.2.11 (2012-02-22)

* Issue 299: Fix FeedValidator crash for feeds that have trips with service_id values that are not defined in either calendar.txt or calendar_dates.txt.
* Issue 315: Add support to [KMLWriter] to show stop-station hierarchy
* Issue 316: Fix crash in [KMLWriter]
* Issue 319: Fix issue with Google Maps API key in ScheduleViewer
* Issue 320: Refactor stop_timezone validation code out of google extensions and into main FeedValidator

#### 1.2.10 (2011-11-04)
* Issue 307: Fix whitespace in schedule.py
* Issue 309: Update schedule viewer to show different arrival / departure time in stop label
* Issue 310: Bug in TooManyConsecutiveStopTimesWithSameTime check when stop times are undefined
* Issue 311: Add new_feed.zip, a artifact of unit tests, to svn:ignore
* Issue 312: Test feed good_feed expires on Dec 31, 2011 - 60 days from now - causing unexpected warnings and test failures
* Issue 313: Convert tabs to spaces in python/gtfsscheduleviewer/files/index.html

#### 1.2.9 (2011-10-27)
* Issue 305: Change "Too Many Consecutive !StopTimes with same departure/arrival time" from ERROR to WARNING

#### 1.2.8 (2011-10-21)
* Issue 296: Feed Validator should send a warning when there are multiple consecutive 0:00:00 stop_times
* Issue 300: frequencies.txt - exact_times is now an officially adopted proposal
* Issue 301: agency.txt - agency_fare_url is now officially part of the GTFS spec
* Issue 302: Add Eclipse project files to svn:ignore
* Issue 303: The feed_info.txt proposal has been officially adopted

#### 1.2.7 (2011-05-06)
* New features
  * Adding possibility to warn about "deprecated column names" (r1423, other related commits: r1463, r1521)
  * Adding the first FeedValidator extension: this “googletransit” extension adds and alters validations of feeds to be used by Google Transit (r1452, see also FeedValidatorExtensions)
      * extension of transitfeed.FareAttribute
      * validation of new file feedinfo.txt
      * extension of transitfeed.Schedule
      * extension of transitfeed.Frequency (r1455)
      * extension of transitfeed.Route and transitfeed.Stop (r1509)
      * extension of transitfeed.Agency (includes the BCP-47 language code validation) (r1521, other related commits: r1524)
      * adding feedvalidator_googletransit.py for using the googletransit extension without having to add the -extension flag (r1498)
* General validation changes
  * Removed date dependency from two tests (r1393)
  * Allowing trip_id to contain a hash '#' (r1399)
  * Avoiding duplicate errors from schedule.Validate due to validate_children=true (r1408)
  * Fix handling of empty string stop_lat and stop_lon values in stops.txt ( issue 258 , r1419)
  * Harmonizing and extending date validation in ServicePeriod. Dates must be between 1900 and 2100 (r1424)
* transitfeed API changes
  * Changes to the extensibility mechanism to make creating and testing of extensions easier (r1409 and r1438)
  * Code formatting changes: cleaned code and removed trailing whitespaces (r1420)
  * Code refactoring: harmonized the Expect... and Expect...InClosure functions in the ValidationTestCase (r1421)
  * Making the error level configurable on all problem types (r1439)
  * Making feedvalidator.py extensible (r1498)
* Other changes
  * Making transitfeed nosetests run on Windows 7 (r1458)
  * Adding a line to the output (both Console and HTML) saying which extension, if any, was used to validate the feed (r1497)

#### 1.2.6 (2010-08-05)
* New features
  * Introduction of an extensibility mechanism so validation can be altered without directly changing the base transitfeed code (r1296. Other related commits: r1319)
* Validation changes
  * Transfers are now ID'd by the pair (from_stop_id, to_stop_id). While not prohibited by GTFS duplicate IDs are still allowed, but a warning is issued (r1268)
  * Warning when two trips in the same block have overlapping stop time intervals, and when both trips run on the same date. (Change by Brian Ferris - issue 230, r1297)
  * Removed error when transfers = 0 and transfer_duration > 0, as it is allowed by GTFS (issue 232, r1298)
  * Added a warning when there are no exceptions defined in calendar_dates.txt (r1306)
  * Added detection of large transfers. Issues a warning if the stops are >2km appart, and an error if they are >10km appart. Also issues a warning if min_transfer_time + 120s is not enough to cover the distance when walking at 2m/s (very fast walking). (issue 234, r1307)
  * util.!FloatStringToFloat and util.!NonNegIntStringToInt are now more flexible, issuing a warning when the number formats are not recognized but can be parsed by Python. As this is implementation dependent, it should be moved to an error as soon as possible. (r1350)
* transitfeed API changes
  * Introduction of an object factory (r1281)
  * Introduction of the !HeadwayPeriod (later renamed to Frequency) and !GtfsFactory classes (r1281)
  * {{{Trip._FIELD_NAMES_HEADWAY}}} was removed. {{{Frequency._FIELD_NAMES}}} should be used instead. (r1296)
  * Invalid values in routes.txt's route_color and route_text_color are now replaced by the respective default values (r1281)
  * Schedule.!AddAgencyObject() no longer calls Validate() by default on the Agency object being added (r1281)
  * Schedule.!AddTripObject() no longer calls Validate() by default on the Trip object being added (r1281)
  * Schedule.!AddRouteObject() no longer calls Validate() on the Route object being added (r1281)
  * Instanciation of GTFS classes should not be done directly, but through !GtfsFactory instead (r1281)
  * Introduction of a !ShapePoint class (r1289)
  * Separation between problem reporting (what warnings and errors exist) and accumulation (how to handle them). (r1319)
  * Renamed a couple of classes to more closely mirror their respective GTFS names. Fare was renamed !FareAttribute and !HeadwayPeriod was renamed Frequency. GenericGTFSObject was also renamed to !GtfsObjectBase. (r1324)
* Other changes
  * !FeedValidator on Windows now deletes database file successfully (issue 206, r1257)
  * Introduction of a GenericGTFSObject (later renamed !GtfsObjectBase) from which all GTFS classes should inherit (r1258)
  * Moved each GTFS class to its own file and removed the big {{{transitfeed/_transitfeed.py}}}. The API was not changed. (issue 196, r1275)
  * Fixed google_random_queries.py usage info, added URL to usage of several tools (r1280)
  * Added single date selection to Schedule Viewer (issue 17, r1300)
  * Unit tests now pass in Python 2.6 (issue 237, r1323)
  * All classes, except !StopTime for performance reasons, go through the !GtfsFactory instead of directly instantiating classes. (r1321)

#### 1.2.5 (2009-12-15)
* Fixing the Windows binary distribution, which by mistake did not include the pytz package for timezone validation.

#### 1.2.4 (2009-12-11)
* Moved {{{transitfeed.py}}} to {{{transitfeed/_transitfeed.py}}} to package it properly with {{{transitfeed/util.py}}} and {{{transitfeed/shapelib.py}}}; provided {{{transitfeed/__init__.py}}} to make the contents of transitfeed available under the same names as before.
* Validation changes
  * Warn when there are several consecutive days without service in the feed validation (issue 188)
  * Warn when the successive stop-times have the same value of shape_dist_traveled and Error when they are decreasing (issue 125, issue 203)
  * The W3C formula for luminance comparison of line colors is used (partial fix for issue 108)
  * Warn when an input file has more than one column with the same name (issue 87)
  * Warn when start date or end date are set, but are not valid dates (issue 41)
  * For problem types that can be sorted by severity keep the N most severe instead of the first N found. These include !StopTooFarFromParentStation, !StopsTooClose, !StationsTooClose, !DifferentStationTooClose, !StopTooFarFromShapeWithDistTraveled, and !TooFastTravel. (issue 184)
* transitfeed API changes
  * Schedule.Validate() is now not run by Loader.Load() by default. To call it implicitly set extra_validation to True, or call it explicitly indicating the service_gap_interval.
  * For performance reasons !ServicePeriod.!IsActiveOn now takes both a date string and, optionally, a date object representing the same date as the string.
* Other changes
  * All tools now have a crash handler. The crash dump file is now called transitfeedcrash.txt
  * --help available in all tools
  * Make bottom panel in schedule viewer fixed height to partially address issue 45
  * Changes to make all tests pass in python 2.4

#### 1.2.3 (2009-11-05)
* Validation changes
  * Warn about duplicate calendar_dates.txt records (issue 82, Jiri)
  * Ignore warning about travel in 0 seconds for times rounded to nearest minute (issue 193)
* transitfeed API changes
  * Make `transitfeed.Route.AddTripObject` private
  * Only write agency.txt, stops.txt, routes.txt and trips.txt if they are created
* Other changes
  * Fix google_random_queries.py to output URLs with ISO 8601 date formats to avoid locale issues (Richard)
  * Fix broken tests in Windows
  * Fix kmlparser

#### 1.2.2 (2009-10-16)

* Validation changes
  * Warn if a stop is served by a route_type 1 and 3 (Subway and Bus). (Guangfan)
  * Warn if vehicles travel too fast between stops (Clancy)
  * Output warning or error if a stop is more than 100m or 1km from its parent (Fabien)
  * Warn if a schedule starts in the future, issue 159
  * Warn when a new version of transitfeed has been created (Guangfan)
  * Include summary of trips per day in validation-results.html
  * parent_station of stop X is not station Y but they are near changed from error to warning (issue 167)
  * Support HHH:MM:SS for really long distance trips
* New features
  * `feedvalidator.py --duplicate_trip_check` Check for duplicate trips which go through the same stops with same service and start times. (Guangfan)
  * `feedvalidator.py --limit_per_type=N` default is 5. only show the first N warnings or errors of each type. This saves memory when there are many problems. (issue 37)
  * `unusual_trip_filter.py --route_type` apply filter only to selected route type. (Jiri)
  * `feedvalidator.py -o CONSOLE` print all errors and warnings to the command console. This lets the validator output useful information before a crash and saves memory when there are many problems.
  * `location_editor.py` adds support for editing station locations to schedule_viewer (Jiri)
* Fix for
  * issue 57, sort of fixed. Added Schedule(load_stop_times=False) used by google_random_queries.py
  * issue 160, feedvalidator crash when given bad path
  * issue 153, merger crash when given wrong number of arguments
  * issue 156, merge raises error and crashes on input with warning or error
  * issue 177, Inconsistent line ends error message should say what file had the bad lines
  * `examples/google_random_queries.py` prints errors and warnings to console instead of crashing
* transitfeed.Schedule gets new method !GetServicePeriodsActiveEachDate
* The check for stops served by subway and bus and duplicate_trip_check use a second scan of stop_times, which increases validation time by ~10%. They may be disabled by default in a future release.

#### 1.2.1 (2009-05-26)

* Validation changes
  * Change unknown 'route_type' value in 'route.txt' from error into warning. (Jiri)
  * Change unknown 'location_type' value in 'stops.txt'from error into warning. (Leio)
  * Warn if shape_dist_traveled is larger than max shape_dist_traveled of its shape (Leio)
  * issue 114, issue an Error if a trip has frequencies defined, but no stop_times. (Clancy)
  * check that for a stop entrance with location_type=2, its parent_station must have location_type=1. (Leio)
  * Stricter checking for route_long_name containing route_short_name. (Leio)
  * Some checking that shape and stop location are within 1000m when using shape_dist_traveled (Leio)
* [Merge Merger] change
  * Use a temporary file instead of memory for stops_times. This allows feeds to be merged with less RAM but is slower. If you have small files or lots of RAM the option --memory_db reverts to the previous in-memory storage.
  * Fixed issue 165, merge when schedules have the same trip_id
* Fix for
  * issue 143, issue 144 Unicode related crashes
  * issue 148, crash on badly formatted shape_dist_traveled (Clancy)
  * issue 115, assert stop_id and !IndexError crash hit when column isn't found
  * issue 120, "%s" isn't a recognized time zone; improved error message
* ScheduleViewer tweaks
  * Marks patterns with unusual trips (those having non-zero trip_type) with gray background. (Jiri)
  * Add support for mouse wheel zooming (Jiri)
  * Displays station in red as opposed to stops in blue. (Jiri)
  * Add a new option --host to support ditu.google.com maps (Leio)
* New tool unusual_trip_filter.py finds infrequent patterns of routes, setting a GTFS attribute 'trip_type' of the corresponding trips to value '1'. No Windows binary yet.

#### 1.2.0 (2009-02-27)

* `GetTimeInterpolatedStops` uses polyline connecting stops instead of straight-line between timepoints. (Thanks to William Lachance)
* slightly stricter time format validation. minutes and second must start with 0-5 instead of any digit (Thanks to Clancy Childs)
* add crash handler to merge.py
* protect crash handler from objects that crash when you try to convert them to a string
* Modify shape_importer to parse shapefiles with polylines between stops instead of for entire trips (Thanks to Dennis Lee)
* Support for arbitrary columns in routes.txt, trips.txt
* ScheduleViewer tweaks:
  * lat/lng in the station bubble (Thanks to Dennis Lee)
  * trip_id search box, issue 96 (Thanks to Clancy Childs)
* Fix for
  * Issue 67 (and duplicates): bad date format causes feedvalidator crash
  * Issue 136 (half of it): zip files written are now compressed
  * Issue 110: Check that times are in order of sequences in stop_times (Thanks to Clancy Childs)
  * Issue 112: Warn about unexpected file names in feed (Thanks to Clancy Childs)
  * Issue 119: Warn about unexpected subdirectories in feed (Thanks to Clancy Childs)
  * Issue 113: Don't raise a RuntimeError for new location_type (Thanks to Clancy Childs)

#### v1.1.9 (2008-12-01)====
* Fix kmlwriter, which was broken in 1.1.8 (Thanks to PK for the fix)

#### v1.1.8 (2008-11-25)====
* Support for transfers.txt (Thanks to Ivan)
* Make Problem reporter more robust when given a mix of unicode and basic strings
* new command line option for kmlwriter: `--date_filter` "Restrict to trips active on date YYYYMMDD"
* transitfeed.Trip gets new method `ReplaceStopTimeObject`
* Fix for
  * Issue 75: validate stop_latitude and lng are in format -?[1-9]\d+\.\d+
  * Issue 106: TypeError: unsupported operand type(s) for -: 'NoneType' and 'NoneType' in Schedule.Validate
  * Issue 118: error in transfers.txt file does not appear in validation-results.html document

#### v1.1.7
* Ignore whitespace before csv fields, warn about whitespace in header fields
* Validate station/stop hierarchy
* Add methods GetHeadwayStopTimes and GetHeadwayStartTimes (Thanks to Juangui)
* Support arbitrary columns in agency.txt and agency_phone
* Fix for
  * Issue 36 : Feed parsing doesn't deal with quoted values correctly when surrounded by space
  * Issue 83 : FeedValidator complains of new feature agency_phone in agency.txt
  * Issue 72 : warning about stops near each other should include distance
  * Issue 92 : Enforce location_type=0 for stops used in stop_times.txt
  * Issue 88 : Validation - location_type, parent_station
  * Issue 101 : feedvalidator rejects blank stop.stop_url

#### v1.1.6
* Store stop_times table in sqlite. This greatly reduces the memory requirements. 
* schedule_viewer no longer requires --key or --feed_filename when connecting to localhost
* added option to kmlwriter to show each shape point
* added new shape parsing library, shapelib.py, but not used by any tools yet
* new methods `ServicePeriod.IsActiveOn` and `ServicePeriod.ActiveDates`
* merge stops with station hierarchy (location_type and parent_station)
* fix some bugs

Measured in linux 2.6.22 on an Intel Core 2 Duo CPU T7300 at 2GHz laptop using `feedvalidator.py -p`.

| Version  | Dataset  | stop_time rows | Memory (MB) | Time (s) |
|----------|----------|----------------|-------------|----------|
| 1.1.5    | bart     |    33k         |   31        |   12     |
|          | portland |  1134k         |  984.2      |  551     |
| 1.1.6    | bart     |    33k         |   29.5      |   20     |
|          | portland |  1134k         |  149.0      |  746     |
| 1.1.6 --memory_db | portland |  1134k         |  235.8      |  738     |

#### v1.1.5
* Add examples/filter_unused_stops.py that loads a gtfs file, modifies some things and saves the changes to a new file
* Fix for
  * Issue 27: feed validator should check line endings
  * Issue 37: transitfeed installed as an egg can interfere with tests
  * Issue 54: `AttributeError: 'NoneType' object has no attribute 'strip'`
  * Issue 55: feedvalidator.exe error: `TypeError: 'NoneType' object is not iterable` (crash handler in py2exe)

#### v1.1.4
* add merge.py as a script; merge.exe is now part of windows binary distribution
* add examples/google_random_queries.py: outputs a list of random query urls, using stop lat,lng
* add a crash handler to feedvalidator. this provides a friendly message to users and should make it much easier for us to debug problems
* unicode names in a feed could make merge.py crash
* new method Stop.GetTrips
* fix for some things that could make feedvalidator crash, including issue 40

#### v1.1.3
* added a new tool, merge.py that merges two feed files
* fix for issue 46, "Color contrast issue in FeedValidator should be a warning, not an error."
* FeedValidator outputs a warning for unknown columns
* KMLWriter optionally draws trips with altitude for time
* small changes to validator html output. fix some html, add internal link to warnings 
* recognize the stop_code field

#### v1.1.2
* Changed #! from python2.4 to python2.5
* Check for rows missing cells. Contributed by Clancy
* fixed typo in FareRule
* kmlwriter much enhanced by Timothy. For each route output the shapes, patterns of stops and optionally a line for each trip

#### v1.1.1
* Schedule Viewer changes:
  * More accurate display of route names
  * Better support for unicode IDs
  * Stop time interpolation
* FeedValidator changes:
  * FeedValidator now categorizes issues as either errors or warnings
  * Checks for missing start & end times on trips
  * Warning if the feed has expired or will in the next month
  * Better handling of UTF-8/16 and other file encoding issues
  * Allows more lenient sequence numbers now that the spec allows them
  * Added warning for poor route color contrast

#### v1.1.0
* Schedule Viewer now displays shapes when available.
* FeedValidator now shows feed metadata at the top of its output and highlights the relevant column for problems.
* Added `--output` switch to set HTML output path and made `-n` switch disable loading of output in browser in FeedValidator.
* Validation changes:
  * Duplicate schedule_id validation now works
  * Error on stops that don't pick up, drop off, or act as a timepoint
  * Error when day in service period isn't set to "1" or "0"
  * More thorough checking of date values
  * Error when a stop time departs before it arrives
  * Error when a stop time specifies either arrival or departure time, but not both

#### v1.0.9
* FeedValidator now provides nicer HTML output in a browser instead of hard-to-read text.
* Added a directory of example Python scripts using transitfeed.
* kmlparser converts KML lines into GTFS shape.

#### v1.0.8
Move schedule_viewer data files and marey_graph into package gtfsscheduleviewer so that setup.py installs them in a sensible place. Add a hack so py2exe schedule_viewer can find its data files. schedule_viewer --key option will get key from a file if it exists.

#### v1.0.7
Adds kmlparser and kmlwriter utilities for partially converting feeds from and to Google Earth KML format.  Also includes several validation improvements, particularly to stop times and route_short_name.

#### v1.0b6
Adds a Windows executable for schedule_viewer.

#### v1.0b5
Adds more validation of service_ids.

#### v1.0b4
Further improves line wrapping, improves ID display, limits the number of unused stops shown.

#### v1.0b3
Adds better line wrapping to make the output more pleasant to read.

#### v1.0b2
Adds testing to identify stops that probably refer to the same location because they're close enough together.

#### v1.0b1
First standalone Windows version.
 
 

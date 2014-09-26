unusual_trip_filter sets the `trip_type` for trips that have an unusual pattern for a route. The input GTFS must pass the feedvalidator without any warnings or errors.

# Details

The pattern of a trip is the sequence of stops it visits. By default a trip is marked as unusual if its pattern is used by less than 10% of the trips of that route. See http://groups.google.com/group/gtfs-changes/browse_frm/thread/557dd991c453807a for more information about `trip_type`.

## Installation
 
unusual_trip_filter is part of the transitfeed distribution. Installation instructions for all operating systems can be found on the TransitFeedDistribution page.

## Command line usage

```
Usage: unusual_trip_filter.py [options] <GTFS.zip>

Sets the trip_type for trips that have an unusual pattern for a route.
<GTFS.zip> is overwritten with the modifed GTFS file unless the --output
option is used.

Options:
  --version             show program's version number and exit
  -h, --help            show this help message and exit
  -o FILE, --output=FILE
                        Name of the output GTFS file (writing to input feed if
                        omitted).
  -m, --memory_db       Force use of in-memory sqlite db.
  -t THRESHOLD, --threshold=THRESHOLD
                        Frequency threshold for considering pattern as non-
                        regular.
  -r ROUTE_TYPE, --route_type=ROUTE_TYPE
                        Filter only selected route type (specified by numberor
                        one of the following names: Cable Car, Tram, Bus,
                        Rail, Funicular, Subway, Gondola, Ferry).
  -f, --override_trip_type
                        Forces overwrite of current trip_type values.
  -q, --quiet           Suppress information output.
```
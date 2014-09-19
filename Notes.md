Brain dump while thinking about the structure of transitfeed.py

# Unicode handling

GTFS says that all strings should be encoded with utf-8. Python can represent these as a a str containing utf-8 encoded bytes or a unicode string. A str containing utf-8 can only be appended to a unicode string if it is pure ascii (all characters <= 127). Appending a utf-8 with characters > 127 to a unicode raises a "UnicodeDecodeError: 'ascii' codec can't decode byte ..." as python tries to convert the str to a unicode.

Currently all strings are converted to the python unicode type when loaded from csv and attributes are encoded to utf-8 as they are written back to csv. Invalid utf-8 sequences in inputs are replaced with `REPLACEMENT CHARACTER` (U+FFFD) in `_ReadCsvDict` and `_ReadCSV`.

# Things to validate

Here is a list of things that should be validated, divided into sections that can be checked at the same time. This startn with low level byte things, working towards high level where relations between multiple rows or objects matter.

## File format
* correct eol
* valid utf-8
* correct csv format
* same number of columns on each line

## GTFS tables
* all required tables present
* in each present table the required columns are present
* warn about unknown tables
* warn about unknown columns

## Individual values

These can be checked when parsing a row from a table or when assigning a value to an attribute.
* correct format for type (time, date, float etc)
* in valid range, abs(latitude) < 90, abs(longitude) < 180

Instances of classes such as `Stop` can be in many states:
* Initialized from a list of unicode strings in a file
* No attributes set, for example after calling `Stop()`
* Attribute assigned in the normal python manner, `stop.stop_lat = 45.832`
* Attribute assigned as a string (probably doesn't need to be supported)

The advantage of storing a string for each value and lazily converting to a native type later is that __init__ is fast and doesn't fail. Also if __getitem__ always returns a string one can treat the object as a dict, which feels clean. But if the strings are saved in __dict__ I need to use __getattribute__ to intercept requests so that, for example, stop.stop_lat returns a float instead of unicode. __getattribute__ is slow because it is called a lot, even for self.__dict__ and self.Validate(). Strings could be saved in a separate dict which could be consulted by __getattr__ but then I'd need to make sure the string and attribute stay in sync.

When an invalid value is assigned to an attribute what happens? It should probably be saved anyway to improve forwards compatibility.

## Consistent row/object

These could be checked during __init__, if it is given all values which happens in the common case of loading from a file. On the other hand it is good to decouple validation from object creation to avoid revalidating when recreating a !StopTime from sqlite. Checking in __setattr__ doesn't seem like a great idea because the order that attributes are set may determine if a problem is reported. For example location_type and parent_id would need to be changed atomically.

* arr and dep time both present or absent
* stop not near 0,0
* parent_id only set with correct location_type
* route_short_name not in route_long_name
* All required fields are set
* warn about fields that are not in spec?

## An object within a Schedule

Some checks can only be done within the context of a schedule, for example references between tables and the speed of a trip (because it needs all stop_time rows for a trip). At the moment some of these checks are done while loading the schedule and others are done once loading is finished.

## While loading

If tables are loaded in an appropriate order many checks can be done on a partial schedule. For example, transitfeed loads routes.txt before trips.txt so that the route_id in trips.txt can be checked while loading. The full context of the source file is available at this time. If everything was in a SQL DB it might make sense to check cross-table references using SQL instead of in each object's Validate method. Here is a subset of things that can be checked while loading:
* No duplicate stop_id in stops, trip_id in trips etc
* No duplicate (trip_id, stop_sequence) tuples in stop_times
* trips: route_id, shape_id, service_id
* stops: parent_id is the id of another stop
* stop_times: the current sqlite branch does not check that all stop_times entries contain a valid stop_id and trip_id and there is no test

## After loading

Some checks can not be done while loading a file. For example, parent_station in stops.txt and anything that needs all the times of a trip. It will take a lot of extra memory to preserve the original file context of every row for these checks. If the raw files were dumped into an sqlite db with the context memory wouldn't be such a problem. Here are some checks easiest to run after loading:

* For a trip
  * first and last stop_time have a time
  * speeding legs, a sign of teleportation
  * frequency periods, if present, are disjoint
* warn about stops within 2m that are not related in stop hierarchy
* illogical fares
* warn about unused stops


# Making it easier to extend

An elegant way to support GTFS extension is very desirable. I think we can support many types of extensions with only a few changes to the existing transitfeed package.

## Create a factory for objects

The classes which represent objects, mostly subclasses of `GenericGTFSObject`, are currently direct members of the module. There is no way to tell the `Loader` to create `Stop`, `Route`, etc objects with a different implementation. The `Loader` should be modified to get new objects from a factory. An extension can than provide a factory to inject objects that support the new behaviors. All other code that creates new objects, such as `Schedule.AddStop` will also need to be changed to use a factory. For simplicity I suggest modifying the Schedule to support factory methods. The classes which represent objects can be made class attributes of `Schedule`. An extension will be created by creating a subclass of `Schedule` that overrides some of these class attributes.

## Make `GenericGTFSObject` subclasses easier to subclass

We need to make sure that it is easy to make a subclass of `Stop`, `Route`, etc. For example GenericGTFSObject accesses the class attribute _FIELD_NAMES. (too be continued)
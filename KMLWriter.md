The kmlwriter is a command line tool that plots the stops, trips and shapes defined in a Google Transit Feed as a KML file that can be viewed in Google Earth or any other application that uses KML files.  Using it can help confirm that the stops and shapes are located in the right place. It also provides a 3d visualization of your trips that can help you find certain types of errors in your stop_times file.

# Installing

kmlwriter is part of the transitfeed distribution. Installation instructions for all operating systems can be found on the TransitFeedDistribution page. kmlwriter depends on !ElementTree, which you might need to install manually.


# Command line usage

Run the feed validator as
```
Usage: kmlwriter.py [options] <input GTFS.zip> [<output.kml>]

Reads GTFS file or directory <input GTFS.zip> and creates a KML file
<output.kml> that contains the geographical features of the input. If
<output.kml> is omitted a default filename is picked based on
<input GTFS.zip>. By default the KML contains all stops and shapes.


Options:
  --version             show program's version number and exit
  -h, --help            show this help message and exit
  -t, --showtrips       include the individual trips for each route
  -a ALTITUDE_PER_SEC, --altitude_per_sec=ALTITUDE_PER_SEC
                        if greater than 0 trips are drawn with time axis set
                        to this many meters high for each second of time
  -s, --splitroutes     split the routes by type
  -d DATE_FILTER, --date_filter=DATE_FILTER
                        Restrict to trips active on date YYYYMMDD
  -p, --display_shape_points
                        shows the actual points along shapes
```


# Viewing

To view the resulting KML file, open Google Earth and choose the File > Open... command from the menu. Navigate to the saved .kml file and Google Earth will plot the stops as place marks on the globe.

# Revision History

See the TransitFeedDistribution page.
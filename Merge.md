In many cases GTFS users have a need to combine two feeds that represent different service periods for their service. For example, an agency that changes their schedule quarterly might have one GTFS file for the January-March period and another for the April-June period. The merge tool helps users combine these two files so they can publish a GTFS file without any gaps in service.

# Using Merge

The merge tool is intended to merges schedules. New stops will be handled but some changes like moving stops may cause problems. 

## Installing and running the Windows binary

First download the compiled version of the TransitFeedDistribution for Windows from [downloads](...). Unpack the zip file to some location - for the purposes of this document let's assume the zip file is unpacked to `C:\` so all the files are in `C:\transitfeed-windows-binary-1.2.0`. The merge tool is `merge.exe`.

Open a command line window by going to the Start menu, selecting "Run...", typing `cmd` and press enter. Go to the directory where you have the GTFS files you want to merge using the `cd` command. If the files are in `c:\gtfs_files` type `cd \gtfs_files'. 

To merge two feed files, you pass the name of the two files on the command line along with the name of the new merged GTFS file. 

For example, if the two feed files are `jan_schedule.zip` and `feb_schedule.zip` then you use the following command line:

`c:\transitfeed-windows-binary-1.2.0\merge jan_schedule.zip feb_schedule.zip merged_schedule.zip`

If there are no errors you will have a new file, `merged_schedule.zip` which will contain the schedule for both January and February.

If you have small files or lots of RAM you can try the option --memory_db which runs much faster.

## Running from Python source

After you have installed the TransitFeedDistribution source simply run `python merge.py old_schedule.zip new_schedule.zip merged_schedule.zip`. For additional options try `python merge.py --help`.

If you have small files or lots of RAM you can try the option --memory_db which runs much faster.

# Command line usage

```
Usage: merge.py [options] <input GTFS a.zip> <input GTFS b.zip> <output GTFS.zip>

Merges <input GTFS a.zip> and <input GTFS b.zip> into a new GTFS file
<output GTFS.zip>.


Options:
  --version             show program's version number and exit
  -h, --help            show this help message and exit
  --cutoff_date=CUTOFF_DATE
                        a transition date from the old feed to the new feed in
                        the format YYYYMMDD
  --largest_stop_distance=LARGEST_STOP_DISTANCE
                        the furthest distance two stops can be apart and still
                        be merged, in metres
  --largest_shape_distance=LARGEST_SHAPE_DISTANCE
                        the furthest distance the endpoints of two shapes can
                        be apart and the shape still be merged, in metres
  --html_output_path=HTML_OUTPUT_PATH
                        write the html output to this file
  --no_browser          prevents the merge results from being opened in a
                        browser
  -m, --memory_db       Use in-memory sqlite db instead of a temporary file.
                        It is faster but uses more RAM.
```
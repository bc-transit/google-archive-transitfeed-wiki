# Introduction

`transitfeed.py` is a Python module for reading, validating, and writing transit schedule information in the [GTFS](https://developers.google.com/transit/gtfs) format.  Python programmers are encouraged to make use of this functionality when building programs that work with transit data.

# Sample Usage

A small sample program for creating an (incomplete) feed is shown below. The [examples directory](https://github.com/google/transitfeed/tree/master/examples) contains more sample scripts.

```
import transitfeed


schedule = transitfeed.Schedule()
schedule.AddAgency("Fly Agency", "http://iflyagency.com",
                   "America/Los_Angeles")

service_period = schedule.GetDefaultServicePeriod()
service_period.SetStartDate("20070101")
service_period.SetEndDate("20080101")
service_period.SetWeekdayService(True)
service_period.SetDateHasService('20070704', False)

stop1 = schedule.AddStop(lng=-122, lat=37.2, name="Suburbia")
stop2 = schedule.AddStop(lng=-122.001, lat=37.201, name="Civic Center")

route = schedule.AddRoute(short_name="22", long_name="Civic Center Express",
                          route_type="Bus")

trip = route.AddTrip(schedule, headsign="To Downtown")
trip.AddStopTime(stop1, stop_time='09:00:00')
trip.AddStopTime(stop2, stop_time='09:15:00')

trip = route.AddTrip(schedule, headsign="To Suburbia")
trip.AddStopTime(stop1, stop_time='17:30:00')
trip.AddStopTime(stop2, stop_time='17:45:00')

schedule.Validate()
schedule.WriteGoogleTransitFeed('google_transit.zip')
```
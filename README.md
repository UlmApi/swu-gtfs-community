# SWU GTFS Feed, Community edition

This is a community-maintained version of Stadtwerke Ulm/Neu-Ulm Verkehr's [official GTFS feed](https://www.swu.de/privatkunden/service/mobilitaet/gtfs-daten/) ([Transitfeeds mirror](https://transitfeeds.com/p/swu-verkehr-gmbh/512)).

**This is not an official GTFS feed, use at own risk**

## Creation

* Fix newlines with `dos2unix *.txt
* Add `feed_start_date` and `feed_end_date` to feed_info.txt
* Remove superflous `feed_id` column in trips.txt (eg. with [xsv](https://github.com/BurntSushi/xsv): `xsv select '!feed_id' trips.txt | sponge trips.txt`)
* Clean up the feed by merging stops and removing deadheads. See https://stefan.bloggt.es/2019/03/cleaning-up-the-swu-gtfs-feed-with-pygtfs-and-sqlite3/
	* For exporting the following commands are used:

	```
	sqlite3 -header -csv gtfs.db 'SELECT * FROM stops' > stops.txt
	sqlite3 -header -csv gtfs.db 'SELECT * FROM trips' > trips.txt

	for I in stops.txt trips.txt; do xsv select '!feed_id' $I | sponge $I; done

	sqlite3 -header -csv gtfs.db 'SELECT trip_id, printf("%02d",(strftime("%s", "arrival_time") / (60*60))) || ":" || printf("%02d",(strftime("%s", "arrival_time") % (60*60)) / 60) || ":" || printf("%02d",(strftime("%s", "arrival_time") % 60) % 60) AS arrival_time, printf("%02d",(strftime("%s", "departure_time")) / (60*60)) || ":" || printf("%02d",(strftime("%s", "departure_time")  % (60*60)) / 60) || ":" || printf("%02d",(strftime("%s", "departure_time") % 60) % 60) AS departure_time, stop_id, stop_sequence, stop_headsign, pickup_type, drop_off_type, shape_dist_traveled, timepoint FROM stop_times' > stop_times.txt
	```
* Add `-community` to `feed_version` in feed_info.txt
* Create `shapes.txt` with [pfaedle](https://github.com/ad-freiburg/pfaedle)
	* Download OSM Extracts for [Tübingen](https://download.geofabrik.de/europe/germany/baden-wuerttemberg/tuebingen-regbez.html) and [Schwaben](https://download.geofabrik.de/europe/germany/bayern/schwaben.html) from Geofabrik
	* Merge them together with osmium-tool: `osmium merge tuebingen-regbez-latest.osm.pbf schwaben-latest.osm.pbf -o tuebingen-schwaben.osm.xml`
	* run pfaedle: `pfaedle -x tuebingen-schwaben.osm.xml .`

## License

This GTFS feed was originally made available by SWU Verkehr and re-published with changes by ulmAPI contributors under the Open Database License: http://opendatacommons.org/licenses/odbl/1.0/.
`shapes.txt` is based on Data from [OpenStreetMap Contributors](https://www.openstreetmap.org/copyright). Used OSM Extracts published by [Geofabrik GmbH](https://www.geofabrik.de).
Any rights in individual contents of the database are licensed under the Database Contents License: http://opendatacommons.org/licenses/dbcl/1.0/

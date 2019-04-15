# SWU GTFS Feed, Community edition

This is a community-maintained version of Stadtwerke Ulm/Neu-Ulm Verkehr's [official GTFS feed](https://www.swu.de/privatkunden/service/mobilitaet/gtfs-daten/) ([Transitfeeds mirror](https://transitfeeds.com/p/swu-verkehr-gmbh/512)).

**This is not an official GTFS feed, use at own risk**

## Creation

* Clean up the feed by merging stops and removing deadheads. See https://stefan.bloggt.es/2019/03/cleaning-up-the-swu-gtfs-feed-with-pygtfs-and-sqlite3/
* Create `shapes.txt` with [pfaedle](https://github.com/ad-freiburg/pfaedle)
	* Download OSM Extracts for [Tübingen](https://download.geofabrik.de/europe/germany/baden-wuerttemberg/tuebingen-regbez.html) and [Schwaben](https://download.geofabrik.de/europe/germany/bayern/schwaben.html) from Geofabrik
	* Merge them together with osmium-tool: `osmium merge tuebingen-regbez-latest.osm.pbf schwaben-latest.osm.pbf -o tuebingen-schwaben.osm.xml`
	* run pfaedle: `pfaedle -x tuebingen-schwaben.osm.xml .`

## License

This GTFS feed was originally made available by SWU Verkehr and re-published with changes by ulmAPI contributors under the Open Database License: http://opendatacommons.org/licenses/odbl/1.0/.
`shapes.txt` is based on Data from [OpenStreetMap Contributors](https://www.openstreetmap.org/copyright). Used OSM Extracts published by [Geofabrik GmbH](https://www.geofabrik.de).
Any rights in individual contents of the database are licensed under the Database Contents License: http://opendatacommons.org/licenses/dbcl/1.0/

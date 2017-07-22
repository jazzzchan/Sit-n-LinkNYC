# Sit-n-LinkNYC

> Finding Locations where you can "Sit" and access Link NYC wifi. 
> Integrates LinkNYC location + range data with Open Street Map
> "amenity" tag

Process

1. **Obtain LINK-NYC location data** -`https://data.cityofnewyork.us/Social-Services/LinkNYC-Locations/s4kf-3yrf`
   * This from Official NYC Open Data portal. Carto connects and syncs with this source.
   * [July 2017] Currently synced in CARTO every month using the link - https://data.cityofnewyork.us/api/views/s4kf-3yrf/rows.csv?accessType=DOWNLOAD
          
 2. **Obtain Open Street Map "amenity" data** by `echo '[out:json];(node["amenity"](40.495992,-74.257159,40.915568,-73.699215););out;>;out skel qt;' | query-overpass --flat-properties >>query-overpass-export-MASTER.geojson`
	 * This uses the OverPass API and Query-Overpass API to obtain a GEOJSON output
	 * Add this GEOJSON to Carto instance.
	 * Try updating the location info  to `area[admin_level=5]["name"="New York City"][boundary=administrative]->.boundaryarea;`

 3. **Create buffers** around every LinkNYC site with radius = 61meters (200 ft - the claimed range of each Link)
    * Ensure Buffers are `dissolved`
    
 4. **"Filter points in polygons"** on the Buffered LinkNYC sites from #3 where Base Polygons = Link Buffers and point layer = OSM amenity nodes (Step #2)
 5. **Re add the Link NYC layer and add the buffer from #1
 6. **Add widgets**

This gives you all the OSM amenities within the connection range of every LINK NYC.
From here on you can add Widgets and legends to customize your view.

My view is [here](https://nyu.carto.com/u/varun-cusp2/builder/fa2fc615-cae5-4c2f-9f69-3a2b13704ce2/embed)


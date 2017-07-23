# Sit-n-LinkNYC

We will be creating a map that shows selected "amenities" (as contained in Open Street Map). 

[LINK to MAP](https://nyu.carto.com/u/varun-cusp2/builder/fa2fc615-cae5-4c2f-9f69-3a2b13704ce2/embed)

1. **LINK-NYC location data** -`https://data.cityofnewyork.us/Social-Services/LinkNYC-Locations/s4kf-3yrf`
   * This from Official NYC Open Data portal.
   * You can sync this dataset with CARTO so that it updates every hour/day/week/month. Currently synced in my CARTO account every month using the link - https://data.cityofnewyork.us/api/views/s4kf-3yrf/rows.csv?accessType=DOWNLOAD
          
 2. **Install query-pass** from https://github.com/perliedman/query-overpass
   * **Using your Terminal, obtain Open Street Map "amenity" data** 
      * ```echo '[out:json];area[admin_level=5]["name"="New York City"][boundary=administrative]->.boundaryarea;(  node["amenity" ~ "coffee"](area.boundaryarea);node["amenity" ~ "cafe"](area.boundaryarea);node["amenity" ~ "restaurant"](area.boundaryarea);node["amenity" ~ "library"](area.boundaryarea);node["amenity" ~ "school"](area.boundaryarea);node ["public_transport"](area.boundaryarea););out;>;out skel qt;' | query-overpass --flat-properties >>`date +%B%Y`-query-overpass-export-MASTER.geojson```
      * This uses the OverPass API and Query-Overpass API to obtain a GEOJSON output
      * Add this GEOJSON to Carto instance.
      
## CARTO Steps

 3. **Setup map environment with the following datasets**
    * LinkNYC locations from #1 
    * QueryyOverpass geojson output from #2
    
 4. **Create Areas of Influence** aka Buffers around every LinkNYC site
    * We will call this ***LinkNYC buffers***.
    * Ensure Buffers are `dissolved`
    * Radius = 61meters (200 ft - the claimed range of each Link)
    
 5. **"Filter points in polygons"** on the ***LinkNYC buffers*** from #4 where
    * Base Polygons = LinkNYC Buffers
    * Point layer = QueryyOverpass geojson output (Step #3)
    
 6. **Re add the original Link NYC layer and redo #4
 
 7. **Add widgets as needed**

This gives you all the OSM amenities within the connection range of every LINK NYC.
From here on you can ustomize your view as needed.


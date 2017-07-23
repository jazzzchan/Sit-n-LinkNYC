# Sit-n-LinkNYC

![](https://user-images.githubusercontent.com/4397663/28500555-5a11f710-6f98-11e7-83d4-13f1039f5a2a.png)

This is the process to create a digital map that shows selected "amenities" where the public can sit down and access LinkNYC Wifi in New York City. 

I have used [NYC Open Data](opendata.cityofnewyork.us), [CARTO](www.carto.com), and the open-source [Overpass API](http://wiki.openstreetmap.org/wiki/Overpass_API) that uses data from [Open Street Map](http://openstreetmap.org)

[LINK to Sit-n-LinkNYC MAP](https://nyu.carto.com/u/varun-cusp2/builder/fa2fc615-cae5-4c2f-9f69-3a2b13704ce2/embed)
![](https://user-images.githubusercontent.com/4397663/28500542-0bf6503a-6f98-11e7-86f7-4c1011505733.png)

***[LinkNYC](https://link.nyc/) is a first-of-its-kind communications network that will replace over 7,500 pay phones across the five boroughs with new structures called Links. Each Link provides superfast, free public Wi-Fi, phone calls, device charging and a tablet for access to city services, maps and directions.***

## Data preparation

1. **LINK-NYC location data** -`https://data.cityofnewyork.us/Social-Services/LinkNYC-Locations/s4kf-3yrf`
   * This from Official NYC Open Data portal.
   * You can sync this dataset with CARTO so that it updates every hour/day/week/month. Currently synced in my CARTO account every month using the link - https://data.cityofnewyork.us/api/views/s4kf-3yrf/rows.csv?accessType=DOWNLOAD
          
 2. **Install query-pass** from https://github.com/perliedman/query-overpass
   * **Using your Terminal, obtain Open Street Map "amenity" data** 
      * ```echo '[out:json];area[admin_level=5]["name"="New York City"][boundary=administrative]->.boundaryarea;(  node["amenity" ~ "coffee"](area.boundaryarea);node["amenity" ~ "cafe"](area.boundaryarea);node["amenity" ~ "restaurant"](area.boundaryarea);node["amenity" ~ "library"](area.boundaryarea);node["amenity" ~ "school"](area.boundaryarea);node ["public_transport"](area.boundaryarea););out;>;out skel qt;' | query-overpass --flat-properties >>`date +%B%Y`-query-overpass-export-MASTER.geojson```
      * This uses the OverPass API and Query-Overpass API to obtain a GEOJSON output
      * Add this GEOJSON to Carto instance.
      
## Map preparation

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


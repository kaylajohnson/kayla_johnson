---
title: "A short intro to personal map art with R"
subtitle: ""
excerpt: "Learn to make a custom map in R"
date: 2021-04-23
author: "Kayla Johnson"
draft: false
images:
series:
tags:
categories:
layout: single
---

This post is brought to you by the presentation I gave for R Ladies East Lansing's "Fun with R" event in April of 2021. There are many things that are fun in R (to me), but I wanted to learn something new to present and had long admired the beautiful maps I saw other people making in R. There are actually _many_ ways to make maps in R (see the [R Graph Gallery](https://www.r-graph-gallery.com/index.html/) and scroll down to "Map" to check out 6 different types of map charts and the multiple ways to create them by clicking on a type), so my goal here was just to be able to create a nice map of a place of significance to me and learn enough to know how to customize it a bit. 

## Packages I am using

```r
library(osmdata)
library(tigris)
library(sf)
library(tidyverse)
```
I have loaded `osmdata`, `tigris`, `sf`, and `tidyverse`. Each of these can be installed from CRAN with `install.packages()` if you do not already have these packages. A brief introduction of each package:

* The `tidyverse` package is a collection of open source packages for reading, cleaning, manipulating, tidying, and visualizing data. If you are not already familiar with the `tidyverse`, the `tidyverse` website has learning resources listed [here](https://www.tidyverse.org/learn/).
* The `sf` package provides simple features access for R. It makes working with irregular geometries in R easier, and here I will be using it to help plot our maps. Learn more about this really cool package on the `sf` [website](https://r-spatial.github.io/sf/index.html).
* The `osmdata` package is an R package for downloading and using data from [OpenStreetMap](https://www.openstreetmap.org/#map=4/38.01/-95.84) (OSM). OSM is an open access map of the world created by interested people and is free to use. `osmdata` provides access to the vector data underlying OSM and puts it into a format we can easily use with `sf`. For a more in-depth introduction, see the [vignette](https://cran.r-project.org/web/packages/osmdata/vignettes/osmdata.html).
* The `tigris` package allows us to download and use TIGER/Line [shapefiles](https://www.census.gov/geographies/mapping-files/time-series/geo/tiger-line-file.html) from the US Census Bureau. Shapefiles contain legal boundaries and names as of the first of the year. `tigris` functions return simple features objects with a default year of 2019. There is some overlap between `tigris` and `osmdata` packages in terms of data types you could choose to use. The advantage of `tigris` is that it is pulling from the US Census Bureau data, which is very high quality and tracked by year. Learn more about the `tigris` package [here](https://www.rdocumentation.org/packages/tigris/versions/1.0).

## Getting started
Let's see what kind of data OSM has. We can use the `available_features()` function from `osmdata` or check out the [OSM Map features wiki](https://wiki.openstreetmap.org/wiki/Map_features). The wiki includes more information on how features are tagged.


```r
available_features()
```

```
##   [1] "4wd_only"                "abandoned"              
##   [3] "abutters"                "access"                 
##   [5] "addr"                    "addr:city"              
##   [7] "addr:conscriptionnumber" "addr:country"           
##   [9] "addr:county"             "addr:district"          
##  [11] "addr:flats"              "addr:full"              
##  [13] "addr:hamlet"             "addr:housename"         
##  [15] "addr:housenumber"        "addr:inclusion"         
##  [17] "addr:interpolation"      "addr:place"             
##  [19] "addr:postbox"            "addr:postcode"          
##  [21] "addr:province"           "addr:state"             
##  [23] "addr:street"             "addr:subdistrict"       
##  [25] "addr:suburb"             "addr:unit"              
##  [27] "admin_level"             "aeroway"                
##  [29] "agricultural"            "alt_name"               
##  [31] "amenity"                 "area"                   
##  [33] "atv"                     "backward"               
##  [35] "barrier"                 "basin"                  
##  [37] "bdouble"                 "bicycle"                
##  [39] "bicycle_road"            "biergarten"             
##  [41] "boat"                    "border_type"            
##  [43] "boundary"                "bridge"                 
##  [45] "building"                "building:colour"        
##  [47] "building:fireproof"      "building:flats"         
##  [49] "building:levels"         "building:material"      
##  [51] "building:min_level"      "building:part"          
##  [53] "building:soft_storey"    "bus_bay"                
##  [55] "busway"                  "castle_type"            
##  [57] "change"                  "charge"                 
##  [59] "construction"            "construction#Railways"  
##  [61] "covered"                 "craft"                  
##  [63] "crossing"                "crossing:island"        
##  [65] "cuisine"                 "cutting"                
##  [67] "cycleway"                "denomination"           
##  [69] "destination"             "diet"                   
##  [71] "diplomatic"              "direction"              
##  [73] "dispensing"              "disused"                
##  [75] "disused:shop"            "drive_in"               
##  [77] "drive_through"           "ele"                    
##  [79] "electric_bicycle"        "electrified"            
##  [81] "embankment"              "embedded_rails"         
##  [83] "emergency"               "end_date"               
##  [85] "entrance"                "est_width"              
##  [87] "fee"                     "fire_object:type"       
##  [89] "fire_operator"           "fire_rank"              
##  [91] "foot"                    "footway"                
##  [93] "ford"                    "forestry"               
##  [95] "forward"                 "frequency"              
##  [97] "fuel"                    "gauge"                  
##  [99] "golf_cart"               "goods"                  
## [101] "hazmat"                  "healthcare"             
## [103] "healthcare:counselling"  "healthcare:speciality"  
## [105] "height"                  "hgv"                    
## [107] "highway"                 "historic"               
## [109] "horse"                   "ice_road"               
## [111] "incline"                 "industrial"             
## [113] "inline_skates"           "inscription"            
## [115] "internet_access"         "junction"               
## [117] "kerb"                    "landuse"                
## [119] "lanes"                   "lanes:bus"              
## [121] "lanes:psv"               "layer"                  
## [123] "leaf_cycle"              "leaf_type"              
## [125] "leisure"                 "lhv"                    
## [127] "lit"                     "location"               
## [129] "man_made"                "maxaxleload"            
## [131] "maxheight"               "maxlength"              
## [133] "maxspeed"                "maxstay"                
## [135] "maxweight"               "maxwidth"               
## [137] "military"                "minspeed"               
## [139] "mofa"                    "moped"                  
## [141] "motor_vehicle"           "motorboat"              
## [143] "motorcar"                "motorcycle"             
## [145] "motorroad"               "mountain_pass"          
## [147] "mtb_scale"               "mtb:description"        
## [149] "mtb:scale:imba"          "name"                   
## [151] "name:left"               "name:right"             
## [153] "narrow"                  "natural"                
## [155] "noexit"                  "non_existent_levels"    
## [157] "note"                    "nudism"                 
## [159] "office"                  "official_name"          
## [161] "old_name"                "oneway"                 
## [163] "opening_hours"           "operator"               
## [165] "organic"                 "oven"                   
## [167] "overtaking"              "parking:condition"      
## [169] "parking:lane"            "passing_places"         
## [171] "place"                   "power"                  
## [173] "priority_road"           "produce"                
## [175] "proposed"                "protected_area"         
## [177] "psv"                     "public_transport"       
## [179] "railway"                 "railway:preserved"      
## [181] "railway:track_ref"       "recycling_type"         
## [183] "ref"                     "religion"               
## [185] "residential"             "roadtrain"              
## [187] "route"                   "sac_scale"              
## [189] "service"                 "service_times"          
## [191] "shelter_type"            "shop"                   
## [193] "short_name"              "sidewalk"               
## [195] "site"                    "ski"                    
## [197] "smoothness"              "social_facility"        
## [199] "sorting_name"            "speed_pedelec"          
## [201] "start_date"              "step_count"             
## [203] "substation"              "surface"                
## [205] "tactile_paving"          "tank"                   
## [207] "tidal"                   "toilets:wheelchair"     
## [209] "toll"                    "tourism"                
## [211] "tracks"                  "tracktype"              
## [213] "traffic_calming"         "traffic_sign"           
## [215] "trail_visibility"        "trailblazed"            
## [217] "trailblazed:visibility"  "tunnel"                 
## [219] "turn"                    "type"                   
## [221] "usage"                   "vehicle"                
## [223] "vending"                 "voltage"                
## [225] "water"                   "wheelchair"             
## [227] "wholesale"               "width"                  
## [229] "winter_road"             "wood"
```

So, with over 200 available features, there is a lot here. The tag that is probably going to be one of the most popular is "highway", so we'll use that in a moment. First, we need to declare a "boundary box" to tell `osmdata` which location we want the data pulled from. As I am a member of R-Ladies East Lansing, we'll start with East Lansing, MI.

We can use `getbb()` to define a boundary box for a place, or define it manually with minimum and maximum latitude and longitude values. We will call our boundary box `bbx`.

```r
bbx <- getbb("East Lansing, MI")
bbx
```

```
##         min       max
## x -84.51350 -84.43887
## y  42.69726  42.80098
```

With `bbx` defined, we can now use `opq()` to query OSM for data in that location. The `add_osm_feature()` specifies the type of data we want pulled by the query, and `osmdata_sf()` ensures that the data returned to us is formatted in a way that `sf` can use to help us plot.

<style>
div.blue { background-color:#e6f0ff; border-radius: 5px; padding: 5px;}
</style>
<div class = "blue">

Warning: Some queries are larger than others. This particular query would take some time to complete for a large city such as New York City or London. Each feature such as "highway" can have multiple subdivisions and it may be prudent to only select some of these values if you are querying for a large area or large city. I will show an example of this selection in the next query.

</div>


```r
highways <- bbx %>%
  opq() %>%
  add_osm_feature(key = "highway") %>%
  osmdata_sf()
```

Let's see what `highways` looks like. We need `ggplot()` to create any plot, then we use `geom_sf()` to plot a map rather than say, `geom_bar()` for a bar plot. The plotting data is stored in the `highways` object we created, specifically in the `osm_lines` part which we can access with `$`. An example of another option would be `osm_polygons` for storing shapes rather than lines for storing roads. I also colored the roads by `highway`, a variable stored in the OSM data, so we can see the many different types of roads. 

```r
ggplot() +
  geom_sf(data = highways$osm_lines,
          aes(color = highway),
          size = 0.4,
          alpha = 0.65) +
  theme_void() #theme void won't plot non-data (like axes) to give us a blank background
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-5-1.png" width="672" />

The road map is pretty dense using all types of roads. Sometimes selecting only certain values of a feature is better for visualization. I can check which values are available for the "highway" feature using `available_tags()`.

```r
available_tags("highway")
```

```
##  [1] "bridleway"              "bus_guideway"           "bus_stop"              
##  [4] "busway"                 "construction"           "corridor"              
##  [7] "crossing"               "cycleway"               "elevator"              
## [10] "emergency_access_point" "emergency_bay"          "escape"                
## [13] "footway"                "give_way"               "living_street"         
## [16] "milestone"              "mini_roundabout"        "motorway"              
## [19] "motorway_junction"      "motorway_link"          "passing_place"         
## [22] "path"                   "pedestrian"             "platform"              
## [25] "primary"                "primary_link"           "proposed"              
## [28] "raceway"                "residential"            "rest_area"             
## [31] "road"                   "secondary"              "secondary_link"        
## [34] "service"                "services"               "speed_camera"          
## [37] "steps"                  "stop"                   "street_lamp"           
## [40] "tertiary"               "tertiary_link"          "toll_gantry"           
## [43] "track"                  "traffic_mirror"         "traffic_signals"       
## [46] "trailhead"              "trunk"                  "trunk_link"            
## [49] "turning_circle"         "turning_loop"           "unclassified"
```

I am going to choose the largest/main roads, plus "residential". In a very large city, "residential" may be unnecessary to get the density you want.

```r
highways <- bbx %>%
  opq() %>%
  add_osm_feature(key = "highway", 
                  value = c("motorway", "trunk",
                            "primary","secondary",
                            "tertiary","motorway_link",
                            "trunk_link","primary_link",
                            "secondary_link", "tertiary_link",
                            "residential")) %>%
  osmdata_sf()
```

Plotting the smaller query gives a less crowded map. I have also switched to using `theme()` with many of the arguments set to match `theme_void()` but with the addition of `legend.position = "none"` so that the legend is also not plotted on our map.

```r
ggplot() +
  geom_sf(data = highways$osm_lines,
          aes(color = highway),
          size = 0.4,
          alpha = 0.65) +
  theme(axis.line = element_blank(),
        axis.text.x = element_blank(),
        axis.text.y = element_blank(),
        axis.ticks = element_blank(),
        axis.title.x = element_blank(),
        axis.title.y = element_blank(),
        legend.position = "none",
        panel.background = element_blank(),
        panel.border = element_blank(),
        panel.grid.major = element_blank(),
        panel.grid.minor = element_blank(),
        plot.background = element_blank()) #theme_void made explicit so I can remove legend
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-8-1.png" width="672" />

## Getting more specific
Above, I used `getbb()` to set the boundary box for East Lansing, MI. However, we are not always interested in an entire city or an easily named area. In these cases, we set our own boundary box. Here I am choosing coordinates that bound MSU's campus, which I have taken from Google Maps (if you click a location in Google Maps, it will give you an address as well as the latitude and longitude coordinates).

```r
min_lon <- -84.493675; max_lon <- -84.462183
min_lat <- 42.711814; max_lat <- 42.735481
campus_bbx <- rbind(x = c(min_lon, max_lon), y = c(min_lat, max_lat))
colnames(campus_bbx) <- c("min", "max")
```

I make two separate queries here for roads and sidewalks on campus so that I can plot them in different sizes and alphas. 

```r
campus_sidewalks <- campus_bbx %>% 
  opq() %>% 
  add_osm_feature(key = "footway") %>%
  osmdata_sf()
campus_roads <- campus_bbx %>% 
  opq() %>% 
  add_osm_feature(key = "highway") %>% 
  osmdata_sf()
```

I want a particular color for my roads and sidewalks, so I am setting that here with `rgb()`, although I could have used built-in R colors or hex codes.

```r
color_roads <- rgb(0.42,0.449,0.488)
```

Here I am plotting the roads and sidewalks of campus, using `coord_sf()` to cut off any roads that extend outside the map boundaries.

```r
ggplot() +
  geom_sf(data = campus_sidewalks$osm_lines,
          col = color_roads,
          size = 0.4,
          alpha = 0.65) +
  geom_sf(data = campus_roads$osm_lines,
          col = color_roads,
          size = 0.6,
          alpha = 0.8) +
  coord_sf(xlim = c(min_lon, max_lon),
           ylim = c(min_lat, max_lat),
           expand = FALSE) + #expand = FALSE will cut off any roads that extend outside your border
  theme(axis.line = element_blank(),
        axis.text.x = element_blank(),
        axis.text.y = element_blank(),
        axis.ticks = element_blank(),
        axis.title.x = element_blank(),
        axis.title.y = element_blank(),
        legend.position = "none",
        panel.background = element_blank(),
        panel.border = element_blank(),
        panel.grid.major = element_blank(),
        panel.grid.minor = element_blank(),
        plot.background = element_rect(fill = NULL))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-12-1.png" width="672" />

## Adding water with tigris
We have plotted the roads and sidewalks, but MSU's campus features the Red Cedar River, so let's see if we can get that on the map.

This code chunk gets us county lines for the state of Michigan using `counties()` and crops to our boundary box with `st_crop()`. In such a small area this pretty much equates to no lines, but it does give us a background and a location to add water to later.

```r
counties_MI <- counties(state = "MI", cb = TRUE, class = "sf",)
counties_MI <- st_crop(counties_MI,
                       xmin = min_lon, xmax = max_lon,
                       ymin = min_lat, ymax = max_lat)
```

A quick look at our non-existent county lines on the MSU campus. Just a background, as promised.

```r
ggplot() + 
  geom_sf(data = counties_MI, fill = "gray", lwd = 0) +
  coord_sf(xlim = c(min(campus_bbx[1,]), max(campus_bbx[1,])), 
           ylim = c(min(campus_bbx[2,]), max(campus_bbx[2,])),
           expand = FALSE) +
  theme(legend.position = F) + 
  theme_void()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-14-1.png" width="672" />

To get the water in our location, we need some custom code, which I thank Dr. Esteban Moro for including in his blog post [here](http://estebanmoro.org/post/2020-10-19-personal-art-map-with-r/). 

```r
get_water <- function(county_GEOID){
  area_water("MI", county_GEOID, class = "sf")
}
water <- do.call(rbind, 
                 lapply(counties_MI$COUNTYFP, get_water))
water <- st_crop(water,
                 xmin = min_lon, xmax = max_lon,
                 ymin = min_lat, ymax = max_lat)
```

If we plot our newly created `water` variable/polygon, we can see the Red Cedar River, which I have chosen to plot in red.

```r
ggplot() + 
  geom_sf(data = counties_MI) +
  geom_sf(data = water,
          inherit.aes = F,
          col = "red") +
  coord_sf(xlim = c(min(campus_bbx[1,]), max(campus_bbx[1,])), 
         ylim = c(min(campus_bbx[2,]), max(campus_bbx[2,])),
         expand = FALSE) +
  theme(legend.position = FALSE) + 
  theme_void()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-16-1.png" width="672" />

We can now use the `water` polygon we created to carve the water area out of our `counties` polygon, again with a custom function slightly modified from Esteban Moro's blog post. 

```r
st_erase <- function(x, y) {
  st_difference(st_union(x), st_union(y))
}
counties_MI <- st_erase(counties_MI, water)
```

The water feature has now been carved out of our geographical background.

```r
ggplot() + 
  geom_sf(data = counties_MI,
          lwd = 0)+
  coord_sf(xlim = c(min(campus_bbx[1,]), max(campus_bbx[1,])), 
         ylim = c(min(campus_bbx[2,]), max(campus_bbx[2,])),
         expand = FALSE)+
  theme(legend.position = FALSE) + 
  theme_void()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-18-1.png" width="672" />

As with all ggplots, colors are easily customizable. Because we carved the water area out of our geographical background, I am actually setting the color of the water by setting the `panel.background` in `theme()` - the panel background color shows through the carved out area.

```r
ggplot() + 
  geom_sf(data = counties_MI,
          inherit.aes = FALSE,
          lwd = 0.0, fill = rgb(0.203,0.234,0.277)) + # the dark gray color
  coord_sf(xlim = c(min(campus_bbx[1,]), max(campus_bbx[1,])), 
         ylim = c(min(campus_bbx[2,]), max(campus_bbx[2,])),
         expand = FALSE) +
  theme(legend.position = FALSE) + 
  theme_void() +
  theme(panel.background =
          element_rect(fill = rgb(0.92,0.679,0.105))) # the yellow color
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-19-1.png" width="672" />

## Putting it all together

We have all of our parts, let's put them together.

```r
ggplot() + 
  geom_sf(data = counties_MI,
          inherit.aes = FALSE,
          lwd = 0.0, fill = rgb(0.203,0.234,0.277)) +
  geom_sf(data = campus_sidewalks$osm_lines,
          inherit.aes = FALSE,
          color = color_roads,
          size = 0.4,
          alpha = 0.65) +
  geom_sf(data = campus_roads$osm_lines,
          inherit.aes = FALSE,
          color=color_roads,
          size = 0.6,
          alpha = 0.65) +
  coord_sf(xlim = c(min(campus_bbx[1,]), max(campus_bbx[1,])), 
           ylim = c(min(campus_bbx[2,]), max(campus_bbx[2,])),
           expand = FALSE) +
  theme(legend.position = FALSE) + 
  theme_void() +
  theme(panel.background =
          element_rect(fill = rgb(0.92,0.679,0.105)))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-20-1.png" width="672" />

One last thing, to mark a special place on your map. Here I am adding the coordinates of an MSU favorite, The MSU Dairy Store. The coordinates are again from Google Maps.

```r
# x is longitude, y is latitude 
dairy_store <- data.frame(x = -84.478404, y = 42.724459)
```

Then I can use `geom_point()` to mark the place! Of course you could choose any color here and choose a different shape in `geom_point()` if you'd like something other than a filled circle. If you like the idea of a star marking your destination, I suggest checking out the `ggstar` package vignette [here](https://cran.r-project.org/web/packages/ggstar/vignettes/ggstar.html). The `ggstar` package has a function `geom_star()` that works like `geom_point()` except has different built-in shapes, so you can use stars or hearts to mark things. 

```r
ggplot() + 
  geom_sf(data = counties_MI,
          inherit.aes = FALSE,
          lwd = 0.0, fill = rgb(0.203,0.234,0.277)) +
  geom_sf(data = campus_sidewalks$osm_lines,
          inherit.aes = FALSE,
          color = color_roads,
          size = .4,
          alpha = .65) +
  geom_sf(data = campus_roads$osm_lines,
          inherit.aes = FALSE,
          color=color_roads,
          size = .6,
          alpha = .65) +
  geom_point(data = dairy_store,
             inherit.aes = FALSE,
             aes(x = x, y = y),
             color = rgb(0.92,0.679,0.105)) +
  coord_sf(xlim = c(min(campus_bbx[1,]), max(campus_bbx[1,])), 
           ylim = c(min(campus_bbx[2,]), max(campus_bbx[2,])),
           expand = FALSE) +
  theme(legend.position = F) + theme_void()+
  theme(panel.background =
          element_rect(fill = rgb(0.92,0.679,0.105)))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-22-1.png" width="672" />

## Saving your map
We may want to print our map, add text, or overlap images in other programs such as Google Slides, ImageMagick, or InkScape. To do so, we need to save in high resolution. First, we will create a variable to store our map, creatively named `final_map`.

```r
final_map <- ggplot() + 
  geom_sf(data=counties_MI,
          inherit.aes= FALSE,
          lwd=0.0,fill=rgb(0.203,0.234,0.277)) +
  geom_sf(data = campus_sidewalks$osm_lines,
          inherit.aes = FALSE,
          color=color_roads,
          size = .4,
          alpha = .65) +
  geom_sf(data = campus_roads$osm_lines,
          inherit.aes = FALSE,
          color=color_roads,
          size = .6,
          alpha = .65) +
  geom_point(data = dairy_store,
             inherit.aes = FALSE,
             aes(x = x, y = y),
             color = rgb(0.92,0.679,0.105)) +
  coord_sf(xlim = c(min(campus_bbx[1,]), max(campus_bbx[1,])), 
           ylim = c(min(campus_bbx[2,]), max(campus_bbx[2,])),
           expand = FALSE) +
  theme(legend.position = FALSE) + 
  theme_void() +
  theme(panel.background =
          element_rect(fill = rgb(0.92,0.679,0.105)))
```

Next, we will use the `ggsave()` function to save our map to file. The width and height will be measured in terms of whatever the `units` argument is set to. In this case I use inches. Also very important is the `dpi` argument. Here it is set to 500, which means the image will be saved with high resolution at 500 dots per inch. This prevents us from printing large, blurry maps! I have not used it here, but `limitsize` may become an argument you need to use if you are trying to save a file larger than 50 inches by 50 inches.

```r
ggsave(final_map, 
       filename = "~/personal_map.png",
       width = 24, 
       height = 24, 
       units = "in",
       dpi = 500)
```

Thanks for following along! If this has interested you, I suggest checking out these other short blog posts: 
* http://estebanmoro.org/post/2020-10-19-personal-art-map-with-r/
* https://ggplot2tutor.com/tutorials/streetmaps
* https://dominicroye.github.io/en/2018/accessing-openstreetmap-data-with-r/

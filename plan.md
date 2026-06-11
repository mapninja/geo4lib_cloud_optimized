Workshop Repo



https://pmtiles.io/

World as PMtile

https://purl.stanford.edu/hf224mw4004

https://stacks.stanford.edu/file/druid:hf224mw4004/20231116.pmtiles

108.07 GiB

prepare

1. install dependencies and ibraries
2. imports
3. clone repo and extract data to colab data folder

Convert the vector data to pmtiles

1. use GDAL (new version) to examine data/RoadCenterLine_20260610/geo_export_71410037-f1cd-48fb-a6ed-f63e86f313c2.shp
2. use GDAL (new version) to convert data/RoadCenterLine_20260610/geo_export_71410037-f1cd-48fb-a6ed-f63e86f313c2.shp > flatgeobuf > santa_clara_roads.pmtiles

Convert the raster data to COG

1. use GDAL to examine data/santa_clara_1896.tif
2. use GDAL to convert data/santa_clara_1896.tif to COG
3. Download the new data, locally.
4. upload the index.html template and images to github

   Explain Crafting stacks URLs to reference the data directly through HTTP range requests, with a template URL
5. edit the html in Github, replacing placeholder text with Github Stacks URLS to the Cloud Optimized formats.

Github Pages

1. enable github pages
2. browse to the index.html and confirm functionlity

index.html should 👍

1. be a complete working html file, other than the placeholder text for the COG and PMtiles

create a style file for the PMTiles file that styles the roads pmtiles it as 2pt red

# Exploring Cloud-Optimized Geospatial Formats from the Stanford Digital Repository

This workshop is a simple, browser-based introduction to cloud-optimized geospatial data. Instead of downloading data or running a conversion workflow, we will explore public datasets from the Stanford Digital Repository (SDR) directly in a web map.

The goal is to understand why formats such as Cloud-Optimized GeoTIFF and PMTiles are useful for web mapping: they let a browser request only the pieces of a large dataset needed for the current map view.

## Learning Objectives

By the end of this workshop, participants should be able to:

1. Explain the difference between raster data and vector tile data.
2. Open public cloud-optimized geospatial files in a browser-based map viewer.
3. Describe why HTTP range requests matter for large geospatial files.
4. Identify COG and PMTiles as formats designed for efficient web access.
5. Recognize FlatGeobuf as another web-friendly vector format.
6. Compare GeoJSON, FlatGeobuf, PMTiles, and COG as web-map data sources.
7. Connect a web-accessible file URL back to its SDR deposit, collection record, or repository source.

## Why Cloud-Optimized Formats Matter

Traditional geospatial files can be very large. If a browser had to download an entire orthophoto or a global OpenStreetMap dataset before drawing anything, web mapping would be slow or impossible for many teaching and research uses.

Cloud-optimized formats solve this by organizing the data so software can ask for only the part it needs.

For example:

```text
The map opens.
The viewer reads a small index from the remote file.
The viewer requests only the image tiles or vector tiles visible on screen.
As the user pans and zooms, the viewer requests additional pieces as needed.
```

This is the basic idea behind HTTP range requests. The browser or mapping library can request a byte range from a remote file instead of downloading the whole file.

## Workshop Platform

We will use GeoLibre Viewer:

https://viewer.geolibre.app/

GeoLibre Viewer provides a simple interface for opening many geospatial data formats in a web map context. For this workshop, it lets us explore remote cloud-optimized files by pasting in public URLs.

## Datasets

### 1. Orthophoto of O'Donohue Family Stanford Educational Farm

SDR deposit:

https://purl.stanford.edu/vq494qx9344

Cloud-Optimized GeoTIFF:

https://stacks.stanford.edu/file/vq494qx9344/odm_orthophoto_COG_d.tif

This is an orthophoto, which is an aerial image that has been corrected so it lines up with map coordinates. In this workshop, it represents the raster example: a pixel-based dataset where each location is represented by image values.

The file is a Cloud-Optimized GeoTIFF, often shortened to COG. A COG is still a GeoTIFF, but it is organized internally so web tools can read small parts of the file over HTTP instead of downloading the entire image first.

### 2. OpenStreetMap PMTiles

SDR deposit:

https://purl.stanford.edu/hf224mw4004

PMTiles file:

https://stacks.stanford.edu/file/hf224mw4004/20231116.pmtiles

This dataset contains OpenStreetMap data packaged as PMTiles. OpenStreetMap is a global, community-created map dataset containing roads, buildings, water, land use, boundaries, places, and many other mapped features.

PMTiles is a single-file vector tile format. Instead of storing every map tile as a separate file, PMTiles stores tiles and their index together in one file. A web map can request only the tile bytes it needs for the current zoom level and map extent.

### 3. NAIP Imagery of the Castle Fire Site - Prefire

SDR collection:

https://purl.stanford.edu/kv186fx7335

GeoTIFF file:

https://stacks.stanford.edu/file/kv186fx7335/m_3611854_se_11_060_20200726.tif

This dataset is National Agriculture Imagery Program (NAIP) aerial imagery for the Castle Fire site before the fire. NAIP imagery is useful for seeing vegetation, roads, clearings, and other landscape conditions in high-resolution aerial photographs.

In this workshop, it gives us a second raster example. Comparing the O'Donohue Farm orthophoto with the Castle Fire prefire NAIP image helps show that raster data can support many different kinds of interpretation, from campus-scale land use to landscape-scale fire context.

### 4. Clowns of America Membership Point Dataset as FlatGeobuf

GitHub raw file URL:

https://raw.githubusercontent.com/mapninja/geo4lib_cloud_optimized/main/stanford-vx572wx7854-geojson.fgb

Original SDR record:

http://purl.stanford.edu/vx572wx7854

Source GeoJSON file converted for this repository:

`stanford-vx572wx7854-geojson.geojson`

This point dataset was created from an anonymized Clowns of America International membership database obtained in 2007 from Clowns of America, International, for teaching use at Stanford's Geospatial Center.

The original table was geocoded by ZIP Code using OpenRefine and the Geonames.org PostalCodes API. Attributes include fields from the original table, such as `City`, `ZipCode`, `Clown_Name`, and `Country`, along with geocoding fields such as `admname1`, `adm1`, `adm2`, `placename`, `longitude`, and `latitude`.

The attribute `Clown-Na_1` stores cleaned name values produced with OpenRefine's "Cluster and Edit" operation. For example, spelling variants such as "Co Co" and "Co-Co" could be collapsed into a shared value such as "CoCo" for name frequency analysis.

For this workshop, the original GeoJSON has been converted to FlatGeobuf. FlatGeobuf is a cloud-friendly vector format that can be read efficiently by web mapping tools, which makes it useful for exploring point features directly from a single remote file.

Citation:

Maples, Stacey D. (2015). Clowns of America, International Membership Point GeoJSON (anonymized). Stanford Digital Repository. Available at: http://purl.stanford.edu/vx572wx7854.

### 5. Stanford Campus Sample Data from This Repository

This repository also includes a small Stanford campus data bundle in the `data/` folder. These files are useful for comparing how GeoLibre Viewer handles common web-mapping inputs from the same place.

Stanford campus boundary GeoJSON:

https://raw.githubusercontent.com/mapninja/geo4lib_cloud_optimized/main/data/stanford_campus.geojson

Stanford public art GeoJSON:

https://raw.githubusercontent.com/mapninja/geo4lib_cloud_optimized/main/data/stanford_public_art.geojson

Stanford campus IRG Cloud-Optimized GeoTIFF:

https://raw.githubusercontent.com/mapninja/geo4lib_cloud_optimized/main/data/stanford_campus_irg.tif

The campus boundary is a single polygon feature. The public art layer contains 65 point features. The IRG raster is a three-band Cloud-Optimized GeoTIFF with internal overviews, so it can be explored in the same browser-based workflow as the SDR raster examples.


## Key Terms

Cloud-Optimized GeoTIFF (COG):
A GeoTIFF arranged so software can efficiently read small parts of the raster over the web.

GeoJSON:
A text-based vector format commonly used for sharing points, lines, and polygons on the web.

PMTiles:
A single-file archive for vector map tiles, designed so web maps can request only the tiles needed for the current view.

FlatGeobuf:
A single-file vector format that stores features in a compact binary layout with a spatial index for efficient map access.

Raster data:
Grid-based data made of pixels. Examples include aerial photographs, satellite imagery, scanned maps, and elevation models.

Vector data:
Coordinate-based data made of points, lines, and polygons. Examples include roads, building footprints, rivers, boundaries, and place labels.

HTTP range request:
A web request for a specific byte range within a remote file. This is one of the mechanisms that makes large cloud-optimized files practical to use in browser maps.

Stanford Digital Repository (SDR):
Stanford's repository for preserving, describing, and sharing scholarly and library-managed digital objects.

## Workshop Flow

1. Introduce the idea of cloud-optimized geospatial files.
2. Open GeoLibre Viewer.
3. Load the SDR COG orthophoto.
4. Discuss raster data and efficient image access.
5. Load the SDR Castle Fire prefire NAIP imagery.
6. Discuss raster interpretation at different scales.
7. Load the FlatGeobuf point dataset from GitHub.
8. Discuss feature datasets and web-friendly vector formats.
9. Load the Stanford campus GeoJSON and COG files from the repository `data/` folder.
10. Compare point, polygon, raster, and tile-based layers.
11. Load the SDR PMTiles OpenStreetMap dataset.
12. Discuss vector tiles and efficient feature rendering.
13. Connect each direct file URL back to its SDR deposit, collection record, or repository source.

## Troubleshooting

If a dataset does not appear:

- Confirm the URL was pasted exactly as shown in this README.
- Check whether GeoLibre Viewer expects a specific source type or add-layer option.
- Try refreshing the viewer and adding the source again.
- Open the SDR deposit link to confirm the source record is available.
- If the file is very large, give the viewer a moment to read the remote index.

For the FlatGeobuf example, use the `raw.githubusercontent.com` URL exactly:

https://raw.githubusercontent.com/mapninja/geo4lib_cloud_optimized/main/stanford-vx572wx7854-geojson.fgb

Do not use the GitHub web redirect form:

https://github.com/mapninja/geo4lib_cloud_optimized/raw/refs/heads/main/stanford-vx572wx7854-geojson.fgb

The GitHub web route redirects to the raw file, but some browser-based geospatial tools cannot follow that redirect because the redirect response does not provide the CORS headers they need.

If the map appears but the data is hard to find:

- Use the viewer's zoom-to-layer or fit-to-data option if available.
- For the orthophoto, zoom near the Stanford Educational Farm.
- For the Castle Fire NAIP imagery, use the viewer's fit-to-data option if available because the site is not on the Stanford campus.
- For the FlatGeobuf point dataset, use fit-to-data because the features cover a broad geographic extent.
- For the Stanford campus data files, zoom near Stanford University or use fit-to-data for the campus boundary.
- For OpenStreetMap PMTiles, try zooming into a familiar city or campus area.

## Source Links

- GeoLibre Viewer: https://viewer.geolibre.app/
- Orthophoto SDR deposit: https://purl.stanford.edu/vq494qx9344
- Orthophoto COG file: https://stacks.stanford.edu/file/vq494qx9344/odm_orthophoto_COG_d.tif
- Castle Fire NAIP SDR collection: https://purl.stanford.edu/kv186fx7335
- Castle Fire prefire NAIP file: https://stacks.stanford.edu/file/kv186fx7335/m_3611854_se_11_060_20200726.tif
- Clowns of America membership SDR record: http://purl.stanford.edu/vx572wx7854
- Clowns of America FlatGeobuf file: https://raw.githubusercontent.com/mapninja/geo4lib_cloud_optimized/main/stanford-vx572wx7854-geojson.fgb
- Source GeoJSON converted in this repository: `stanford-vx572wx7854-geojson.geojson`
- Stanford campus boundary GeoJSON: https://raw.githubusercontent.com/mapninja/geo4lib_cloud_optimized/main/data/stanford_campus.geojson
- Stanford public art GeoJSON: https://raw.githubusercontent.com/mapninja/geo4lib_cloud_optimized/main/data/stanford_public_art.geojson
- Stanford campus IRG COG: https://raw.githubusercontent.com/mapninja/geo4lib_cloud_optimized/main/data/stanford_campus_irg.tif
- OpenStreetMap SDR deposit: https://purl.stanford.edu/hf224mw4004
- OpenStreetMap PMTiles file: https://stacks.stanford.edu/file/hf224mw4004/20231116.pmtiles
- PMTiles project: https://github.com/protomaps/PMTiles
- FlatGeobuf project: https://flatgeobuf.org/
- Cloud-Optimized GeoTIFF overview: https://www.cogeo.org/

## License and Use

This workshop material is provided for educational use. When reusing or redistributing data, refer to the rights and access information in the individual Stanford Digital Repository deposit records.

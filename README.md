# Exploring Cloud-Optimized Geospatial Formats from the Stanford Digital Repository

This workshop is a simple, browser-based introduction to cloud-optimized geospatial data. Instead of downloading data or running a conversion workflow, we will explore public datasets from the Stanford Digital Repository (SDR) directly in a web map.

The goal is to understand why formats such as Cloud-Optimized GeoTIFF and PMTiles are useful for web mapping: they let a browser request only the pieces of a large dataset needed for the current map view.

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

## Learning Objectives

By the end of this workshop, participants should be able to:

1. Explain the difference between raster data and vector tile data.
2. Open public cloud-optimized geospatial files in a browser-based map viewer.
3. Describe why HTTP range requests matter for large geospatial files.
4. Identify COG and PMTiles as formats designed for efficient web access.
5. Connect a web-accessible file URL back to its SDR deposit or collection record.

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

## Activity 1: Explore the Orthophoto COG

1. Open GeoLibre Viewer:

   https://viewer.geolibre.app/

2. Add the COG URL:

   https://stacks.stanford.edu/file/vq494qx9344/odm_orthophoto_COG_d.tif

3. Zoom and pan around the image.

4. Observe what happens as the viewer loads imagery at different zoom levels.

Discussion prompts:

- What details become visible as you zoom in?
- Why is this file a raster dataset rather than a vector dataset?
- What would be difficult about using this image if the viewer had to download the whole file first?

## Activity 2: Explore the OpenStreetMap PMTiles Dataset

1. Open GeoLibre Viewer:

   https://viewer.geolibre.app/

2. Add the PMTiles URL:

   https://stacks.stanford.edu/file/hf224mw4004/20231116.pmtiles

3. Navigate to different parts of the world.

4. Change zoom levels and observe how the map detail changes.

Discussion prompts:

- What types of features appear at low zoom levels?
- What additional features appear as you zoom in?
- Why are vector tiles useful for a dataset as large as OpenStreetMap?

## Activity 3: Explore the Castle Fire Prefire NAIP Image

1. Open GeoLibre Viewer:

   https://viewer.geolibre.app/

2. Add the NAIP imagery URL:

   https://stacks.stanford.edu/file/kv186fx7335/m_3611854_se_11_060_20200726.tif

3. Zoom and pan around the image.

4. Compare the image content with the O'Donohue Farm orthophoto.

Discussion prompts:

- What landscape features can you identify before the fire?
- How is this raster image similar to the farm orthophoto?
- How is this raster image different in scale, subject, or interpretation?
- Why might prefire imagery be useful for environmental analysis?

## Activity 4: Compare COG, GeoTIFF, and PMTiles

Use the examples to compare raster and vector cloud-optimized formats.

| Question | O'Donohue Farm COG | Castle Fire Prefire NAIP | OpenStreetMap PMTiles |
| --- | --- | --- | --- |
| What kind of data is it? | Raster imagery | Raster imagery | Vector tiles |
| What does the map draw? | Pixels from an aerial image | Pixels from aerial imagery | Points, lines, and polygons |
| What is it good for? | Campus-scale visual interpretation and basemaps | Prefire landscape interpretation | Roads, buildings, labels, boundaries, analysis layers |
| How does it support web access? | Internal tiling and overviews | Remote GeoTIFF access in the viewer | Internal tile index and tile archive |
| What SDR record describes it? | https://purl.stanford.edu/vq494qx9344 | https://purl.stanford.edu/kv186fx7335 | https://purl.stanford.edu/hf224mw4004 |

## Key Terms

Cloud-Optimized GeoTIFF (COG):
A GeoTIFF arranged so software can efficiently read small parts of the raster over the web.

PMTiles:
A single-file archive for vector map tiles, designed so web maps can request only the tiles needed for the current view.

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
7. Load the SDR PMTiles OpenStreetMap dataset.
8. Discuss vector tiles and efficient feature rendering.
9. Compare the raster and vector examples.
10. Connect each direct file URL back to its SDR deposit or collection record.

## Troubleshooting

If a dataset does not appear:

- Confirm the URL was pasted exactly as shown in this README.
- Check whether GeoLibre Viewer expects a specific source type or add-layer option.
- Try refreshing the viewer and adding the source again.
- Open the SDR deposit link to confirm the source record is available.
- If the file is very large, give the viewer a moment to read the remote index.

If the map appears but the data is hard to find:

- Use the viewer's zoom-to-layer or fit-to-data option if available.
- For the orthophoto, zoom near the Stanford Educational Farm.
- For the Castle Fire NAIP imagery, use the viewer's fit-to-data option if available because the site is not on the Stanford campus.
- For OpenStreetMap PMTiles, try zooming into a familiar city or campus area.

## Source Links

- GeoLibre Viewer: https://viewer.geolibre.app/
- Orthophoto SDR deposit: https://purl.stanford.edu/vq494qx9344
- Orthophoto COG file: https://stacks.stanford.edu/file/vq494qx9344/odm_orthophoto_COG_d.tif
- Castle Fire NAIP SDR collection: https://purl.stanford.edu/kv186fx7335
- Castle Fire prefire NAIP file: https://stacks.stanford.edu/file/kv186fx7335/m_3611854_se_11_060_20200726.tif
- OpenStreetMap SDR deposit: https://purl.stanford.edu/hf224mw4004
- OpenStreetMap PMTiles file: https://stacks.stanford.edu/file/hf224mw4004/20231116.pmtiles
- PMTiles project: https://github.com/protomaps/PMTiles
- Cloud-Optimized GeoTIFF overview: https://www.cogeo.org/

## License and Use

This workshop material is provided for educational use. When reusing or redistributing data, refer to the rights and access information in the individual Stanford Digital Repository deposit records.

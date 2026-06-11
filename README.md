# Cloud-Optimized Geospatial Data for Web Mapping

A workshop on preparing geospatial data in cloud-optimized formats and serving them through HTTP range requests for efficient web-based visualization.

## Concept

This workshop demonstrates how to transform traditional geospatial data formats into cloud-optimized alternatives that enable efficient remote access via HTTP. The resulting web application can fetch only the data needed to render the current map view, dramatically reducing bandwidth and improving performance.

**Key advantages:**
- **Reduced bandwidth**: Only fetch data for the current viewport
- **HTTP range requests**: Access specific portions of files without full download
- **No spatial index required**: Cloud-optimized formats include embedded indices
- **Open standards**: Works with existing web standards and libraries

## Technical Overview

### Cloud-Optimized Formats

**PMTiles**: A cloud-optimized vector tile format that bundles all tiles into a single file. Tiles are indexed internally, allowing HTTP range requests to fetch only the needed tiles.
- Ideal for: Vector data (roads, boundaries, features)
- Source: [pmtiles.io](https://pmtiles.io/)

**COG (Cloud-Optimized GeoTIFF)**: A standard GeoTIFF organized with overviews and tiled layout to enable efficient remote access.
- Ideal for: Raster data (imagery, elevation, historical maps)
- Benefits: Full GDAL/GIS software compatibility

### Data Pipeline

```
Original Data → Examination → Format Conversion → Cloud Storage → Web Access
  (Shapefiles)   (GDAL Info)   (PMTiles/COG)    (HTTP URLs)    (MapLibre)
```

## Getting Started

### Prerequisites
- Python 3.8+
- GDAL (4.0+) - handles both raster and vector formats
- Web browser with support for HTTP range requests
- Git (for pushing to GitHub and enabling Pages)

### Data Preparation

<a target="_blank" href="https://colab.research.google.com/github/mapninja/geo4lib_cloud_optimized/blob/main/data-preparation.ipynb">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>
</a>

Use the provided Jupyter notebook (`data-preparation.ipynb`) to:

1. **Examine existing data**: Inspect shapefile and raster metadata
2. **Convert vector data**: Transform shapefiles → FlatGeoBuf → PMTiles
3. **Convert raster data**: Optimize GeoTIFFs to COG format
4. **Download processed data**: Save locally for uploading to GitHub

The notebook includes detailed explanations and can be run in Google Colab or locally.

### Web Application

The `index.html` file provides a complete MapLibre-based web map that:
- Loads a basemap (OpenStreetMap tiles)
- Adds cloud-optimized layers via HTTP URLs
- Includes full inline documentation
- Supports zoom/pan interactions
- Demonstrates performance with remote data access

### Deployment

The simplest deployment path uses GitHub:

1. Push all files to a GitHub repository
2. Enable GitHub Pages (Settings → Pages → Main branch)
3. Access your map at `https://[username].github.io/[repo]/`

GitHub serves files with proper HTTP headers, enabling range requests directly from a GitHub Pages URL.

## File Structure

```
.
├── README.md                    # This file
├── index.html                   # Complete web map application
├── data-preparation.ipynb       # Data conversion workflow
├── data/
│   ├── pmtiles-roads.json       # Style definition for PMTiles layer
│   ├── stanford_campus.geojson  # Sample vector data
│   ├── stanford_public_art.geojson
│   ├── RoadCenterLine_20260610/ # Source shapefile (Santa Clara roads)
│   └── santa_clara_roads.pmtiles # Output: cloud-optimized vector tiles
└── scratch/                     # Working directory for intermediate files
```

## The Workflow

### 1. Prepare Data (Jupyter Notebook)

```bash
# The notebook will:
- Install gdal-utils and dependencies
- Examine your shapefile with ogrinfo
- Convert shapefile to FlatGeoBuf (an intermediate format)
- Create PMTiles from FlatGeoBuf
- Optimize any rasters to COG format
```

### 2. Upload to GitHub

Place your cloud-optimized files in GitHub and note their raw content URLs:
- Example: `https://raw.githubusercontent.com/[user]/[repo]/main/data/roads.pmtiles`

### 3. Reference in HTML

Update the `index.html` placeholder URLs with your actual GitHub URLs to load the data.

### 4. Enable GitHub Pages

Once enabled, your map is live at the repository's GitHub Pages URL.

## HTTP Range Requests: How It Works

Traditional approach:
```
Browser → Download entire 1GB PMTiles file → Extract tiles → Render
```

Cloud-optimized approach:
```
Browser → HTTP GET /tiles.pmtiles Range: bytes=0-1023 → Receive 1KB index
       → HTTP GET /tiles.pmtiles Range: bytes=50000-51000 → Receive tile
       → Render
```

The cloud-optimized format includes a directory at the beginning, allowing the browser to request only the bytes containing relevant tiles.

## MapLibre GL JS

The `index.html` uses [MapLibre GL JS](https://maplibre.org/), an open-source web map library that supports:
- Vector tiles (PMTiles)
- Raster tiles (COG, XYZ)
- GeoJSON sources
- Flexible styling with JSON stylesheets
- Full zoom/pan control

All map styling is configured in JavaScript, with comprehensive inline comments explaining each layer and style property.

## Performance Considerations

- **Initial load**: Minimal - only map tiles and viewport data are fetched
- **Pan/zoom**: Additional HTTP range requests fetch needed tiles on demand
- **Caching**: Browser cache and CDN support reduce repeated requests
- **Bandwidth**: Typical map view uses 100-500KB instead of gigabytes

## Troubleshooting

**Map doesn't load:**
- Check browser console (F12) for CORS/fetch errors
- Verify GitHub raw content URLs are accessible
- Ensure `index.html` is in a public repository with Pages enabled

**Slow performance:**
- Check HTTP response headers include `Accept-Ranges: bytes`
- Verify network tab shows range requests (`206 Partial Content`)
- Consider downsampling or tiling strategy for very large datasets

**Data not visible:**
- Verify tile sources match your actual data extent
- Check layer visibility and style definitions
- Review MapLibre documentation for style syntax

## Example: Santa Clara County Road Network

This repository includes Santa Clara County road network data prepared as:
- **Vector source**: Santa Clara County GIS data ([RoadCenterLine dataset](https://data.sccgov.org/Transportation/RoadCenterLine/amyq-ahrs))
- **Cloud-optimized**: `santa_clara_roads.pmtiles` (after running notebook)
- **Style**: `data/pmtiles-roads.json` (2pt red line styling)

### Data Sources

- **Road Network (Vector)**: [Santa Clara County Transportation - RoadCenterLine](https://data.sccgov.org/Transportation/RoadCenterLine/amyq-ahrs/about_data) - Public GIS data from Santa Clara County via Socrata Open Data portal
- **Reference Implementation**: [Stanford Digital Repository](https://purl.stanford.edu/hf224mw4004) - World PMTiles dataset (108 GB) demonstrating cloud-optimized formats at scale
- **Historical Maps (Optional Raster)**: [David Rumsey Historical Map Collection](https://www.davidrumsey.com/luna/servlet/detail/RUMSEY~8~1~1578~170036:San-Mateo,-Santa-Cruz,-Santa-Clara) - Georeferenced historical maps of Santa Clara County, example of raster COG data
- **Workshop Template**: This repository provides a simplified, manageable example suitable for learning and local experimentation

## Further Learning

- [PMTiles Specification](https://github.com/protomaps/PMTiles)
- [Cloud Optimized GeoTIFF](https://www.cogeo.org/)
- [MapLibre GL JS Docs](https://maplibre.org/maplibre-gl-js/)
- [GDAL Data Format Guide](https://gdal.org/)
- [HTTP Range Requests](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Range)

## License

This workshop material is provided as-is for educational purposes. Ensure compliance with the licenses of any included data sources.

---

**Last updated**: June 2026

For questions or suggestions, open an issue on GitHub or refer to the inline documentation in the code files.

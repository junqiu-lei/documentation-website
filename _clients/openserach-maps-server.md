---
layout: default
title: OpenSearch Maps Server
nav_order: 200
has_children: false
---

# OpenSearch Maps Server

This project helps to create a self-host maps server for OpenSearch Dashboard, for these who are not able to access to OpenSearch default maps service on air-gapped environments.

The server provides maps manifest url(including both maps tiles and vectors), maps tiles url and maps vectors url which are compatible with OpenSearch.

## Get docker image

```
docker pull opensearch/opensearch-maps-server
```

## Set up server
We need prepare map tiles set before start the server, there are 2 options:

Option1: Use the planet tiles set provided by OpenSearch maps service

* Create a docker volume to hold the tiles set:
```
docker volume create tiles-data
```
* Then download tiles set from OpenSearch maps service, we now have zoom 0-8 and 0-10 planet tiles set available generated from OpenStreetMap.
  * https://maps.opensearch.org/offline/planet-osm-default-z0-z8.tar.gz
  * https://maps.opensearch.org/offline/planet-osm-default-z0-z10.tar.gz

```
docker run \
-e DOWNLOAD_TILES=https://maps.opensearch.org/offline/planet-osm-default-z0-z8.tar.gz \
-v tiles-data:/usr/src/app/public/tiles/data/ \
opensearch/opensearch-maps-server \
import
```

Option2: Use the tiles set by yourself, you could generate the raster tiles images set(in z/x/y folder) by our [raster tile generation pipeline](https://github.com/opensearch-project/maps/tree/main/tiles-generation/cdk), and then use the x/y/z tile set absolute path as a volume to start the server.

## Run server
Use created docker volume ``tiles-data``:
```
docker run \
-v tiles-data:/usr/src/app/public/tiles/data/ \
-e HOST_URL='http://localhost' \
-p 8080:8080 \
opensearch/opensearch-maps-server \
run
```

Use your own tiles set by:
```
docker run \
-v /absolute/path/to/tiles/:/usr/src/app/dist/public/tiles/data/ \
-p 8080:8080 \
opensearch/opensearch-maps-server \
run
```
Then you can check:
* Map manifest: http://localhost:8080/manifest.json
* Map tiles: http://localhost:8080/tiles/data/{z}/{x}/{y}.png
* Map tiles demo: http://localhost:8080

## Use in OpenSearch-Dashboard
* Use the manifest url in Dashboard config yml file by 
```
map.opensearchManifestServiceUrl: "http://localhost:8080/manifest.json"
```

* Use the tiles url on Dashboard WMS options: [OpenSearch WMS document](https://opensearch.org/docs/latest/dashboards/maptiles/)
# README

## Quick-start

First, generate vector tiles from geojson:

```bash
docker run -it --rm \
-v ${PWD}/data:/data \
tippecanoe:latest \
tippecanoe --output-to-directory=/data/tiles/ --force --maximum-zoom=11 --drop-densest-as-needed --extend-zooms-if-still-dropping --no-tile-compression /data/obs.geojson
```

On this example, it will generate the tiles in a folder called `tiles`, whithin `data`.

Then run the docker-composition (uncomment `createBukets`):

```bash
docker-compose up -d
```

The tiles are available here:

```bash
ogrinfo MVT:http://localhost:9000/obs/0/0/0.pbf
```

This will make sure to initialize the bucket on minio, and move the data there. If you need to start the composition again, but already have the bucket, please comment the `createBuckets` service.

This will run the container on a network called `minioNet`. Be sure to attach to it, when running the pygeoapi container, or you won't be able to access the vector tiles.



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

Then run the docker-composition:

```bash
docker-compose up -d
```

The `minio` server is available here: `http://localhost:9000/`

These are the steps to publish the tiles on a data volume:

```bash
docker run --network host --add-host="host.docker.internal:127.0.0.1" \
--entrypoint=/bin/sh minio/mc -c \
'mc config host add pygeoapi http://127.0.0.1:9000 pygeoapi pygeoapi; mc ls pygeoapi; exit 0'
```

```bash
docker run --network host --add-host="host.docker.internal:127.0.0.1" \
-v ${PWD}/data/tiles:/data --entrypoint=/bin/sh minio/mc -c \
'mc config host add pygeoapi http://127.0.0.1:9000 pygeoapi pygeoapi; mc policy set public pygeoapi/obs; mc ls pygeoapi; exit 0'
```

The tiles are available here:

```bash
ogrinfo MVT:http://localhost:9000/obs/0/0/0.pbf
```



# Docker image for OSRM

This project lets you prepare and run a docker container with OSRM and the map of your choice.

## Run
There are two methods using this docker container. The first one is to add a PBF resource url that will be fetched on startup. The other option is to use a data container. The first method is recommend and easier to get started with.

### Without data container

You only have to define a `.osm.pbf` file as a `PBF_RESOURCE` environment.

I recommend you to select a file from [GeoFabrik.de](http://download.geofabrik.de/).

Run the container like

```
docker run \
  -d
  -p 5000:5000
  -e PBF_RESOURCE="http://download.geofabrik.de/europe/luxembourg-latest.osm.pbf"
  michaelkrog/osrm-docker
```

### Using a data container

First you'll need to prepare the url to your `.osm.pbf` source file.

Run your data container. The data container will keep your map files even if you restart your OSRM server.

```
docker run \
    -v /data \
    --name osrm-data \
    michaelkrog/osrm-docker:latest \
    echo "running data container..."
```

Now you can run your osrm server with any map.

```
docker run \
    -d \
    --volumes-from osrm-data \
    -p 5000:5000 \
    michaelkrog/osrm-docker:latest \
    ./run.sh \
        Barcelona \
        "https://s3.amazonaws.com/metro-extracts.mapzen.com/barcelona_spain.osm.pbf"
```

The first argument is the name you want to give to the map. It's used mostly as a file name in the data storage.
The second argument is the URL to your source map file.

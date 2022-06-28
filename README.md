# pb-osmr

## Tools and resrouces

### Docker containers

https://hub.docker.com/r/osrm/osrm-backend/
https://hub.docker.com/r/mediagis/nominatim

### Open Street Map data

Download osm.pbf files from https://download.geofabrik.de/

### Tools

Osmosis: https://wiki.openstreetmap.org/wiki/Osmosis#Linux
`apt-get install osmosis

https://git-lfs.github.com/ to manage large files

## Merging .osm.pbf files and genarating .osrm file

Run this from the data directory

```
osmosis --read-pbf netherlands-latest.osm.pbf --read-pbf belgium-latest.osm.pbf --read-pbf nordrhein-westfalen-latest.osm.pbf --merge --merge --write-pbf nl_be_de-nw.osm.pbf
sudo docker run -t -v "${PWD}:/data" osrm/osrm-backend osrm-extract -p /opt/car.lua /data/nl_be_de-nw.osm.pbf
sudo docker run -t -v "${PWD}:/data" osrm/osrm-backend osrm-partition /data/nl_be_de-nw.osrm
sudo docker run -t -v "${PWD}:/data" osrm/osrm-backend osrm-customize /data/nl_be_de-nw.osrm
```

## Running docker with correct data

```
sudo docker run -t -i -p 5000:5000 -v "${PWD}:/data" osrm/osrm-backend osrm-routed --algorithm mld /data/nl_be_de-nw.osrm
sudo docker run -d --restart unless-stopped -t -i -p 5000:5000 -v "${PWD}:/data" osrm/osrm-backend osrm-routed --algorithm mld /data/nl_be_de-nw.osrm
```

## Testing

```
http://localhost:5000/route/v1/driving/4.67470510,51.78983750;4.94419,52.33294

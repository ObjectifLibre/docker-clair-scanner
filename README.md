# docker-clair-scanner

[![Build Status](https://travis-ci.org/ObjectifLibre/docker-clair-scanner.svg?branch=master)](https://travis-ci.org/ObjectifLibre/docker-clair-scanner)

This is a docker container for clair-scanner https://github.com/arminc/clair-scanner.

## Quick how-to

```bash
docker network create scanning
docker run -p 5432:5432 -d --net=scanning --name db arminc/clair-db:$(date -d "yesterday" '+%Y-%m-%d')
docker run -p 6060:6060  --net=scanning --link db:postgres -d --name clair arminc/clair-local-scan:v2.0.1
docker run --net=scanning --rm --name=scanner --link=clair:clair -v '/var/run/docker.sock:/var/run/docker.sock'  objectiflibre/clair-scanner --clair="http://clair:6060" --ip="scanner" -t Medium <Image to scan>

```

## Example with generated json report and date formated for Osx

```bash
docker network create scanning
docker run -p 5432:5432 -d --net=scanning --name db arminc/clair-db:$(date +%Y-%m-%d)
docker run -p 6060:6060  --net=scanning --link db:postgres -d --name clair arminc/clair-local-scan:v2.0.1
docker run --net=scanning --name=scanner --link=clair:clair -v '/var/run/docker.sock:/var/run/docker.sock'  objectiflibre/clair-scanner --clair="http://clair:6060" --ip="scanner" -r report.json <Image to scan>
docker container cp scanner:report.json ./report.json
docker container rm scanner
```
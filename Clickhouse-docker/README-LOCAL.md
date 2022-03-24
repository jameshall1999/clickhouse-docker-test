## Build latest 22 version
docker build -f Dockerfile-22 --label ch-22 --tag ch-22 .

## Build latest 19 version
docker build -f Dockerfile-19 --label ch-19 --tag ch-19 .

## Run version 19
docker run --rm -d -v ${PWD}/data:/var/lib/clickhouse ch-19
*reminder to create data directory in github

## Exec into container
docker exec -it <container id> clickhouse client
create database test
use test
<create and populate table with test data>

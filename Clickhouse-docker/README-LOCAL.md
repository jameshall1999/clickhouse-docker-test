## Build latest 22 version
```
docker build -f Dockerfile-22 --label ch-22 --tag ch-22 .
```

## Build latest 19 version
```
docker build -f Dockerfile-19 --label ch-19 --tag ch-19 .
```

## Run version 19
```
docker run --rm -d -v ${PWD}/data:/var/lib/clickhouse ch-19
```
reminder to create data directory in github

## Exec into container
```
docker exec -it <container id> clickhouse client
create database test
use test
<create table>
example: 
CREATE TABLE test.graphite (`Path` String, `Value` Float64, `Time` UInt32, `Date` Date, `Timestamp` UInt32) ENGINE = GraphiteMergeTree('graphite_rollup') PARTITION BY toMonday(Date) ORDER BY (Path, Time) TTL Date + toIntervalMonth(25) SETTINGS index_granularity = 8192
note: 
graphite_rollup config is required for this
<copy existing data>
ssh to doner server
clickhouse client -q 'select * from graphite.graphite limit 1000000 format CSV' | gzip > graphite.csv.gz
scp file to localhost and into the data directory
enter running container using docker exec to run bash
import data:
zcat graphite.csv.gz | clickhouse client -q 'insert into test.graphite format CSV'
```

# elastic-search-lerning

## environment

```
$ docker version
Client:
 Version:           24.0.7
 API version:       1.43
 Go version:        go1.21.1
 Git commit:        24.0.7-0ubuntu2~22.04.1
 Built:             Wed Mar 13 20:23:54 2024
 OS/Arch:           linux/amd64
 Context:           default
permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/version": dial unix /var/run/docker.sock: connect: permission denied
$ docker compose version
Docker Compose version v2.29.7
```

## how to set up

```
sudo apt reinstall docker-compose-plugin
```

```
sudo docker compose up
```


```
sudo docker exec -it elasticdump sh
```

```
sudo docker compose down
```

## error handling


```
elastic-search-lerning$ id -u
1000
elastic-search-lerning$ sudo chown -R 1000:root elasticsearch/
```

## check cluster status

```
# default password is changeme
$ curl -u elastic http://127.0.0.1:9200/_cat/health
Enter host password for user 'elastic':
1728987448 10:17:28 docker-cluster yellow 1 1 2 2 0 0 2 0 - 50.0%
```

## list index

```
curl -s http://localhost:9200/_cat/indices
```

## insert test data


```shell:insert-index.sh
#!/bin/bash

for i in {1..365}; do
	d=$(date '+%Y.%m.%d' --date "$i days ago 2024-11-01")
	index="$d"
	curl -X PUT "localhost:9200/$index/tweet/1?op_type=create&pretty" -H 'Content-Type: application/json' -d '{"test": "test"}'
done
```

## close index

```shell:close-index.sh
#!/bin/bash
set -eu

INDEXES=()

## close対象のindexを作成
for i in {30..365}; do
	d=$(date '+%Y.%m.%d' --date "$i days ago 2024-11-01")
	index="$d"
    INDEXES+=($index)
done

date
for index in ${INDEXES[@]}; do
    echo curl -X POST "http://localhost:9200/${index}/_close?pretty"
    curl -X POST http://localhost:9200/${index}/_close?pretty
done
date
```

## open index

```sh
curl -X POST "localhost:9200/my_index/_open?pretty"
```

## delete index

```sh
curl -X DELETE "http://localhost:9200/${index}"
```

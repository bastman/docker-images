# java8-buildtools

- java8
- jq, rq
- docker-ce, docker-compose
- make

## dockerhub
- bastman77/java8-buildtools:<SERVICE_VERSION>
- https://hub.docker.com/r/bastman77/java8-buildtools/tags/

## commands
```
$ make -C java8-buildtools clean
$ make -C java8-buildtools build
$ make -C java8-buildtools run
$ make -C java8-buildtools dockerhub.push
$ make -C java8-buildtools dockerhub.pull

# use a custom version (adhoc)
$ make -C java8-buildtools dockerhub.pull SERVICE_VERSION=2018-04-26T07.27.46Z
$ make -C java8-buildtools run SERVICE_VERSION=2018-04-26T07.27.46Z

# use a custom version (persistent)
$ make -C java8-buildtools version.set NEW_SERVICE_VERSION=my-custom-version
$ make -C java8-buildtools dockerhub.pull
$ make -C java8-buildtools run


```


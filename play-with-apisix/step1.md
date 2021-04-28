# Install Apache APISIX

Thanks to Docker, we could launch the Apache APISIX and enable the Admin API by executing the following commands:

```sh
$ git clone https://github.com/apache/apisix-docker.git
$ cd apisix-docker/example
$ docker-compose -p docker-apisix up -d
```

It will take some time to download all needed files, and this depends on your network, please be patient. Once this step gets done, we could curl our Admin API to tell if the Apache APISIX launchs successfully.

```sh
# NOTE: Please curl on the machine which you run above Docker commands.
$ curl "http://127.0.0.1:9080/apisix/admin/services/" -H 'X-API-KEY: edd1c9f034335f136f87ad84b625c8f1'
```

We expect the following data to be returned:

```json
{
  "node": {
    "createdIndex": 6,
    "modifiedIndex": 6,
    "key": "/apisix/services",
    "dir": true
  },
  "action": "get"
}
```

# Create a Route

## Before we continue#

Do you know? Apache APISIX provides the powerful Admin API and a Dashboard for us to use, but we will use Admin API here in this guide. Let's go!

We could create one Route and target it to our backend services (We call them Upstream usually), when one Request reaches Apache APISIX, Apache APISIX will see where this Request should go.

Then how does Apache APISIX know this? That's because we have a list of rules configured with Route. Here is a sample Route data:

```json
{
  "methods": ["GET"],
  "host": "example.com",
  "uri": "/services/users/*",
  "upstream": {
    "type": "roundrobin",
    "nodes": {
      "httpbin.org:80": 1
    }
  }
}
```

This Route means all inbound requests will be forwarded to the httpbin.org:80 Upstream when they meets ALL these rules(matched requests):

* Request's HTTP method is GET;
* Request has Host Header, and its value is example.com;
* Request's path matches /services/users/*, * means all subpaths, like /services/users/getAll?limit=10.


After this Route is created, we could use Apache APISIX's address to access our backend services(Upstream actually):

```sh
$ curl -i -X GET "http://{APISIX_BASE_URL}/services/users/getAll?limit=10" -H "Host: example.com"
```

This will be forward to `http://httpbin.org:80/getAll?limit=10` by Apache APISIX.

## Create an Upstream

After reading the above section, we know we have to set the Upstream for Route. Just executing the following command to create one:

```sh
$ curl "http://127.0.0.1:9080/apisix/admin/upstreams/50" -H 'X-API-KEY: edd1c9f034335f136f87ad84b625c8f1' -X PUT -d '
{
  "type": "roundrobin",
  "nodes": {
    "httpbin.org:80": 1
  }
}'
```

We use roundrobin as our load balancer mechanism, and set httpbin.org:80 as our Upstream target(backend server), and its ID is 50. For more fields, please refer to Admin API.

NOTE: Create an Upstream is not required actually, because we could use Plugin to interceptor requests then response directly, but let's assume we need to set at least one Upstream in this guide.


## Bind Route with Upstreams

We just created an Upstream(Reference to our backend services), let's bind one Route with it!

```sh
$ curl "http://127.0.0.1:9080/apisix/admin/routes/5" -H 'X-API-KEY: edd1c9f034335f136f87ad84b625c8f1' -X PUT -d '
{
  "uri": "/get",
  "host": "httpbin.org",
  "upstream_id": "50"
}'
```

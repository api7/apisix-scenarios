# Verification

Congratulations once more! We have created one Route and Upstream, also we bind them together. Now let's call Apache APISIX to test the created route.

```sh
$ curl -i -X GET "http://127.0.0.1:9080/get?foo1=bar1&foo2=bar2" -H "Host: httpbin.org"
```

Wow! It will return data from our Upstream(httpbin.org actually), it works as expected!

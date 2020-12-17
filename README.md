# local-gloo
Please refer to the following link for me detail regarding the configuration of gloo when not using k8s:
https://docs.solo.io/gloo-edge/latest/installation/gateway/development/docker-compose-file/

## Installing Gloo Edge (control plane + data plane):

```bash
docker-compose up 
```


## Installing Gloo Edge control plane only: 

```bash
docker-compose -f gloo-edge-control-plane.yaml up
```
keep the hostname or the ip of the machine, it is need for the next step.

## Installing Gloo Edge data plane only:

before installing Gloo Edge data plane, update the file: data/envoy-config.yaml

```
      endpoints:
      - lb_endpoints:
        - endpoint:
            address:
              socket_address:
                address: gloo # use the control plane address (ip or hostname) from previous step 
                port_value: 9977
    http2_protocol_options: {}
```

```bash
export LICENSE_KEY='your license key'
export GLOO_ADDR='gloo control plane address from previous setep'
docker-compose -f gloo-edge-control-plane.yaml up
```



## Validating a configuration:
here is an axample of how to validate a config using the gateway:

```
curl -k -XPOST -d '{"request":{"uid":"1234","kind":{"version":"v1","kind":"List"},"resource":{"group":"","version":"","resource":""},"name":"invalid-vs-list","namespace":"gloo-system","operation":"CREATE","userInfo":{},"object":{"apiVersion":"v1","kind":"List","items":[{"apiVersion":"gateway.solo.io/v1","kind":"VirtualService","metadata":{"name":"invalid-vs","namespace":"gloo-system"},"spec":{"virtualHost":{"routes":[{"matchers":[{"prefix":"/"}],"delegateAction":{"name":"i-dont-exist-rt","namespace":"gloo-system"}}]}}}]}}}' -H 'Content-Type: application/json' https://localhost:8443/validation
```


## Mtls







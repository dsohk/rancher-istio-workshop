# LAB 2 - Deploy Microservices

## Step 1 - Deploy the microservices application

1. Navigate to Rancher rke2-cluster (Cluster Exlorer view)
2. Click the "Import YAML" button on the top
3. Open a browser tab, navigate to this URL: https://github.com/istio/istio/blob/master/samples/bookinfo/platform/kube/bookinfo.yaml
4. copy the content into Rancher import yaml dialog window.

## Step 2 - Deploy gateways

5. navigation to this URL: https://github.com/istio/istio/blob/master/samples/bookinfo/networking/bookinfo-gateway.yaml
6. copy the content into Rancher import yaml dialog window.

## Step 3 - Deploy destination rules

1. Navigate to this url: https://github.com/istio/istio/blob/master/samples/bookinfo/networking/destination-rule-all.yaml

2. copy the content into Rancher import yaml dialog window

## Step 4 - find the gateway URL

Find the URL for gateway service
7. Cluster explorer > Services > (Switch to istio-system namespace) -> click istio-ingressgateway service, switch to the node ports page.

The gateway URL = http://<public ip from neuvector>:<http2 port>

## Step 5 - deploy traffic generation app

Create namespace `loadtest` under Default project in rancher

Import this yaml into Rancher

Edit the file to use the bookinfo url

```
apiVersion: v1
kind: Pod
metadata:
  name: traffic-generator
  namespace: loadtest
spec:
  containers:
  - command: ['sh', '-c', 'while true; do sleep $INTERVAL; curl -sk $URL; done']
    env:
    - name: URL
      value: "http://20.219.17.7.sslip.io:31380/productpage"
    - name: INTERVAL
      value: "1"
    image: radial/busyboxplus:curl
    name: curl
```



# LAB 2 - Deploy Microservices

## Step 1 - Deploy the microservices application

Navigate to Rancher rke2-cluster (Cluster) 

`Cluster` > Cluster Dashboard`> Import YAML`  

![import-yaml-app-bookinfo](../images/import-yaml-app-bookinfo.png)

Open a browser tab, navigate to this URL: https://github.com/istio/istio/blob/master/samples/bookinfo/platform/kube/bookinfo.yaml

Copy the content into Rancher import yaml dialog window.

![app-bookinfo-yaml](../images/app-bookinfo-yaml.png)

This will create various Kubernetes Objects as per the deployment yaml for BookInfo Application. You can see deployment is updating which indicated container are getting provisioned. Within 1 mins or less you should be able to the see the deployment rollout successfully. 

![14-kuberbetes-objects-from-deployment](../images/14-kuberbetes-objects-from-deployment.png)

`Cluster` > `Workload` > `Deployments`  select appropriate namespace `default`

![deployment-bookinfo-ready](../images/deployment-bookinfo-ready.png)

`Cluster` > `Workload` > `Pods`  select appropriate namespace `default`

![deployment-pods](../images/deployment-pods.png)

`Cluster` > `Workload`  `Services` > select appropriate namespace `default`

![deployment-services](../images/deployment-services.png)



## Step 2 - Deploy gateways

In the previous step, we were looking at the Service for the application which would be typically taking advantage of Ingress Controller to expose as Service, however we are using Istio. Istio uses it's own Load Balancer to route the Istio traffic. 

For the application to take advantage of Istio, we will need to create an Istio Gateways Service for our BookInfo Application. 

Navigate to Rancher rke2-cluster (Cluster) 

`Cluster` > Cluster Dashboard`> Import YAML`  

![import-yaml-app-bookinfo](../images/import-yaml-app-bookinfo.png)

Navigation to this URL: https://github.com/istio/istio/blob/master/samples/bookinfo/networking/bookinfo-gateway.yaml

Copy the content into Rancher import yaml dialog window.

![gateway-yaml-bookinfo-app](../images/gateway-yaml-bookinfo-app.png)

`Cluster` > `Istio` >  `Gateways` 

![cluster-istio-gateways](../images/cluster-istio-gateways.png)





The gateway URL = http://<public ip from neuvector>:<http2 port>

## Step 3 - find the gateway URL

We now have our Gateway Services for Istio set-up. We now need to utilze it. 

Let's find out the URL for our Gateway Services. 

`Cluster Explorer` > `Services` > switch to `Isito-system namespace`

![services-istio-istio-ingressgateway](../images/services-istio-istio-ingressgateway.png)

Click on the Name `istio-ingressgateway` and take a note of the `NodePort` as we will need it. As sample my `NodePort` value is `31380`

You will have your own respective `NodePort` so make a note of the port. The value will be used in next step. 

![services-istio-ingress-gateway-nodeport](../images/services-istio-ingress-gateway-nodeport.png)

Next things is will will need the Public IP of your RKE2 downstream cluster.  

The IP has been already shared with you.  Look for `neuvector_webui_url` . Append the `Nodeport` & `productpage`. URL should look like below. 

URL = http://<public ip from neuvector>:<Nodeport>/productpage. 

`http://40.80.87.0:31380/productpage`

Sample output below

![bookinfo-app-exposed-via-istio](../images/bookinfo-app-exposed-via-istio.png)

Congratulation! you have successfully deployed the BookInfo App. 

## Step 4 - Deploy destination rules

In this steps, we are setting up some destination rule which will allow traffic between the various pod used by Bookinfo app. 

Navigate to Rancher rke2-cluster (Cluster) 

`Cluster` > Cluster Dashboard`> Import YAML`  

![import-yaml](../images/import-yaml.png)

Navigate to this url: https://github.com/istio/istio/blob/master/samples/bookinfo/networking/destination-rule-all.yaml

copy the content into Rancher import yaml dialog window and hit `Import`

![destinationrule](../images/destinationrule.png)

Destination Rule created, you can hit `close`

![destinationrule-created-active](../images/destinationrule-created-active.png)

![Istio-desintationrule](../images/Istio-desintationrule.png)







## Step 5 - deploy traffic generation app

Navigate to Rancher rke2-cluster (Cluster) 

`Cluster` > Cluster Dashboard`> Import YAML`  

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


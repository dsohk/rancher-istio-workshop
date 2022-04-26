# Lab 2 - Deploy Sample BookInfo Microservices

With Istio (Service Mesh) and Prometheus/Grafana (Monitoring) components deployed on the RKE2, let's deploy a sample microservices-based application on to the `default` namespace of this cluster.



## Step 1 - Enable Auto-injection in default namespace

Before we deploy microservices app into default namespace, we need to make sure Istio can automatically inject an envoy proxy sidecar sitting next to teach cicroservices. To achieve this, we need to enable auto-injection.

1. Navigate to `Istio` on the left pane menu.

2. Choose `Kiali` application
3. You will be presented a home page 



## Step 2 - Deploy the microservices application called BookInfo

From the Rancher server, navigate to the cluster `rke2-cluster`

At the cluster dashboard view, go to the option `Import Yaml` at the top right hand corner.

![import-yaml-app-bookinfo](../images/import-yaml-app-bookinfo.png)

Open a browser tab, navigate to this URL: https://github.com/istio/istio/blob/master/samples/bookinfo/platform/kube/bookinfo.yaml

Copy the contents into Rancher import yaml dialog window.
Ensure that `default` is being selected as the `Default Namespace`. Click on `Import` to start the deployment.

![app-bookinfo-yaml](../images/app-bookinfo-yaml.png)

This will create various Kubernetes Objects as decribed in the the deployment yaml for BookInfo Application. You can see deployment is updating which indicates container are getting provisioned. Within 1 min you should be able to the see the deployment rollout successfully. 

![14-kuberbetes-objects-from-deployment](../images/14-kuberbetes-objects-from-deployment.png)

You can check out the various kubernetes objects that have been deployed. For Deployments, you can go to `Workload` > `Deployments`. You can also filter the namespace accordingly from the drop-down menu at the top right hand corner. For this case, you can select the namespace `default`.

![deployment-bookinfo-ready](../images/deployment-bookinfo-ready.png)

For Pods, you can go to `Workload` > `Pods`. Again, you can select the namespace `default`.

![deployment-pods](../images/deployment-pods.png)

For Services, you can go to `Service Discovery` > `Services`. You can also select the namespace `default`

![deployment-services](../images/deployment-services.png)



## Step 2 - Deploy Istio gateway for BookInfo application

In the previous step, we were looking at the Services that has been deployed when we installed the Bookinfo application. These Services would typically make use of the Ingress Controller in order to be exposed externally outside of the cluster. 
However, for this workshop, we would be using Istio, which will make use of it's own Load Balancer to route the Istio traffic instead. 

In order for the application to take advantage of Istio, we will need to create an Istio Gateway Service for the BookInfo Application. 

From the Rancher server, navigate to the cluster `rke2-cluster`
At the cluster dashboard view, go to the option `Import Yaml` at the top right hand corner. 

![import-yaml-app-bookinfo](../images/import-yaml-app-bookinfo.png)

Navigation to this URL: https://github.com/istio/istio/blob/master/samples/bookinfo/networking/bookinfo-gateway.yaml

Copy the contents into Rancher import yaml dialog window.
Ensure that `default` is being selected as the `Default Namespace`. Click on `Import` to start the deployment.

![gateway-yaml-bookinfo-app](../images/gateway-yaml-bookinfo-app.png)

From the menu on the left, select `Istio` >  `Gateways` 

![cluster-istio-gateways](../images/cluster-istio-gateways.png)






## Step 3 - Find the BookInfo Gateway URL

We now have our Gateway Services for Istio set-up. We now need to utilze it. 

Let's find out the URL for our Gateway Services. 

From the menu on the left, select `Services Discovery` > `Services`. Select the namespace `Istio-system`

![services-istio-istio-ingressgateway](../images/services-istio-istio-ingressgateway.png)

Click on the Name `istio-ingressgateway` and select the tab `Ports`. Take note of the `NodePort` corresponding to `http2` and jot it down. Typically, `http2` would be mapped to the `NodePort` value `31380`. This value will be used in the next step.

![services-istio-ingress-gateway-nodeport](../images/services-istio-ingress-gateway-nodeport.png)

Next, you will need to have the Public IP of your RKE2 downstream cluster.  

This IP has already been shared with you. Look out for the IP corresponding to `neuvector_webui_url` . Append the `Nodeport` & `productpage` accordingly. The final URL should look like below. 

`http://40.80.87.0:31380/productpage`

Sample output below

![bookinfo-app-exposed-via-istio](../images/bookinfo-app-exposed-via-istio.png)

Congratulation! you have successfully deployed the BookInfo App. 



## Step 4 - Deploy destination rules

We will be setting up destination rules which will allow traffic to happen between the various pod used by Bookinfo app. 

From the Rancher server, navigate to the cluster `rke2-cluster`
At the cluster dashboard view, go to the option `Import Yaml` at the top right hand corner. 

![import-yaml](../images/import-yaml.png)

Navigate to this url: https://github.com/istio/istio/blob/master/samples/bookinfo/networking/destination-rule-all.yaml

Copy the content into Rancher import yaml dialog window.
Ensure that `default` is being selected as the `Default Namespace`. Click on `Import` to start the deployment.

![destinationrule](../images/destinationrule.png)

Destination Rule created, you can hit `close`

![destinationrule-created-active](../images/destinationrule-created-active.png)

![Istio-desintationrule](../images/Istio-desintationrule.png)

We have successfully deployed the destination rule.

Next step is to generate traffic to the bookinfo app. In order to acheive this we will be deploying a pod which will be polling the BookInfo app every second.

## Step 5 - deploy traffic generation app

For simplicity sake, we will deploy a simple pod which will curl into BookInfo app. The pod will be created in it's own namespace, so let first create a project and a namespace. 

`Cluster` > `Project/Namespaces` > `Create Project`

![create-project-namespace](../images/create-project-namespace.png)

Name the project `loadtest` and then click on `create`

![project-loadtest](../images/project-loadtest.png)

At the bottom of the page we see the Project `loadtest` created succesfully

![project-loadtest-created-success](../images/project-loadtest-created-success.png)

Name step is to create namespace with the project. Click on `Create Namespace`

![namespace-loadtest](../images/namespace-loadtest.png)

Name the namespace `loadtest` and then click on `create`

![namespace-loadtest-created-success](../images/namespace-loadtest-created-success.png)

We are now ready to create the pod which will communicate with bookinfo app. 

From the Rancher server, navigate to the cluster `rke2-cluster`
At the cluster dashboard view, go to the option `Import Yaml` at the top right hand corner. 

![import-yaml](../images/import-yaml.png)

Copy the contents of the yaml specificiation below into Rancher import yaml dialog window
Sample yaml definition. 

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
      value: "http://20.219.17.7:31380/productpage"
    - name: INTERVAL
      value: "1"
    image: radial/busyboxplus:curl
    name: curl
```

**Important fields** in the yaml definition 

1 - **Namespace** `loadtest`

2 - **URL** `http://40.80.87.0:31380/productpage`. URL should be within the quotes symbol `" "`

![loadtest-yaml-modified-to-unique-rke-url](../images/loadtest-yaml-modified-to-unique-rke-url-16508833232382.png)

Let's check if the pod is up & running

`Cluster` > `Workload` > `Pod` > Under Namespace select `loadtest` 

![loadtest-pod-success](../images/loadtest-pod-success.png)

With this we have successfully completed step 2 of the workshop.  

To summarize, we have deployed the micro-services, enabled Istio, injected Istio sidecars to our application, configured Istio Gateway & destination rule to access application and it's application traffic. Finally we have deployed a container to access the application. 

In the next steps, we will observe the application traffic via Kiali & Jaeger. 

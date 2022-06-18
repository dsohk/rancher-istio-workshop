# Lab 2 - Deploy Sample BookInfo Microservices

With Istio (Service Mesh) and Prometheus/Grafana (Monitoring) components deployed on the RKE2, let's deploy a sample microservices-based application on to the `default` namespace of this cluster.



## Step 1 - Enable Auto-injection in default namespace

Before we deploy microservices app into default namespace, we need to make sure Istio can automatically inject an envoy proxy sidecar sitting next to teach cicroservices. To achieve this, we need to enable auto-injection.

Click `Istio` on the left pane menu and Click `Kiali`

Choose Default namespace, click `3 dots vertical line` and click on `Enable Auto Injection`

![01-rke2cluster1-istio-kiali-enable-auto-injection](../images/01-rke2cluster1-istio-kiali-enable-auto-injection.png)

Upon success you move your mouse on `Labels` you will see the injection successfully done 

![rke2cluster1-istio-kiali-enable-auto-injection-success](../images/rke2cluster1-istio-kiali-enable-auto-injection-success.png)



## Step 2 - Deploy the microservices application called BookInfo

![Bookinfo Application](https://istio.io/latest/docs/examples/bookinfo/withistio.svg)



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



## Step 3 - Deploy Istio gateway for BookInfo application

In the previous step, we were looking at the Services that has been deployed when we installed the Bookinfo application. These Services would typically make use of the Ingress Controller in order to be exposed externally outside of the cluster. 
However, for this workshop, we would be using Istio, which will make use of it's own Load Balancer to route the Istio traffic instead. 

In order for the application to take advantage of Istio, we will need to create an Istio Gateway Service for the BookInfo Application. 

From the Rancher server, ensure that you are at `rke2-cluster` and then go to the option `Import Yaml` at the top right hand corner. 

![import-yaml-app-bookinfo](../images/import-yaml-app-bookinfo.png)

Navigation to this URL: https://github.com/istio/istio/blob/master/samples/bookinfo/networking/bookinfo-gateway.yaml

Copy the contents into Rancher import yaml dialog window.
Ensure that `default` is being selected as the `Default Namespace`. Click on `Import` to start the deployment.

![gateway-yaml-bookinfo-app](../images/gateway-yaml-bookinfo-app.png)

From the menu on the left, select `Istio` >  `Gateways` 

![cluster-istio-gateways](../images/cluster-istio-gateways.png)



## Step 4 - Deploy destination rules

We will be setting up destination rules which will allow traffic to happen between the various pod used by Bookinfo app. 

From the Rancher server, ensure that you are at `rke2-cluster` and then go to the option `Import Yaml` at the top right hand corner. 

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



**Skip Step 5** 

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Step 5 - Find the BookInfo Gateway URL

We now have our Gateway Services for Istio set-up. We now need to utilze it. 

Let's find out the URL for our Gateway Services. 

From the menu on the left, select `Services Discovery` > `Services`. Select the namespace `Istio-system`

![services-istio-istio-ingressgateway](../images/services-istio-istio-ingressgateway.png)

Click on the Name `istio-ingressgateway` and select the tab `Ports`. Take note of the `NodePort` corresponding to `http2` and jot it down. Typically, `http2` would be mapped to the `NodePort` value `31380`. This value will be used in the next step.

![services-istio-ingress-gateway-nodeport](../images/services-istio-ingress-gateway-nodeport.png)

Next, you will need to have the Public IP of your RKE2 downstream cluster.  

This IP has already been shared with you. Look out for the IP corresponding to `neuvector_webui_url` . Append the `Nodeport` & `productpage` accordingly. The final URL should look like below. 

`http://40.80.87.0:31380/productpage`

**IMPORTANT: You must take down your unique URL. It will be required in STEP 6**







You should be able to access the BookInfo web front from youe unique URL. In case you are working behind a proxy server or corprate firewall that blocks you from accessing any URL with ports other than 80 or 443, then you need to setup an nginx ingress proxy for the application.

** Add nginx ingress for Bookinfo product page **

![bookinfo-app-exposed-via-istio](../images/bookinfo-app-exposed-via-istio.png)

Congratulation! you have successfully deployed the BookInfo App. 



--------

## Step 6 - Expose BookInfo Application to Users 

To expose the BookInfo Application, we will need to create create a Ingress Services so that using the Ingress Service, Application would be exposed.

`Cluster` > `Service Discovery` > `Ingress`  > `Create`

Fill the below detail in the `Ingress` Page

`namespace` = `istio-system`

`name` = `bookinfo`

Under `Rules` Page 

 `Request Host`  `=`  `bookinfo.<IP-of-NeuVector>.sslip.io`

`Path` = `ImplementationSpecific`

`Target Service` = `Istio-ingressgateway`  

`Port` = `80`

Note **Please use your own unique IP of NeuVector Server**

![april27-Ingress-bookinfo](../images/april27-Ingress-bookinfo.png)

Click on the URL under `Target` Column for  Application bookinfo. The page should error out (404 Page not found)`. 

Add `/productpage` at the end of the URL & the page#bookinfo-app-exposed-via-istio should open up as below.

![april27-ingress-controller-bookinfo-app](../images/april27-ingress-controller-bookinfo-app.png)

![april27-book-info-app-opening-success](../images/april27-book-info-app-opening-success.png)



------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------



## Step 7 - deploy traffic generation app

For simplicity sake, we will deploy a simple pod which will curl into BookInfo app. The pod will be created in it's own namespace, so let first create a project and a namespace. 

`Cluster` > `Project/Namespaces` > `Create Project`

![create-project-namespace](../images/create-project-namespace.png)

Name the project `loadtest` and then click on `create`

![project-loadtest](../images/project-loadtest.png)

At the bottom of the page we see the Project `loadtest` created successfully

![project-loadtest-created-success](../images/project-loadtest-created-success.png)

Next step is to create namespace with the project. Click on `Create Namespace`

![namespace-loadtest](../images/namespace-loadtest.png)

Name the namespace `loadtest` and then click on `create`

![namespace-loadtest-created-success](../images/namespace-loadtest-created-success.png)

We are now ready to create the pod which will communicate with bookinfo app. 

From the Rancher server, ensure that you are at `rke2-cluster` and then go to the option `Import Yaml` at the top right hand corner. 

![import-yaml](../images/import-yaml.png)

Copy the contents of the sample yaml definition below into Rancher import yaml dialog window.

**IMPORTANT: You must ensure that the value of URL is modified according to the one that you have taken down in STEP 5.
This URL must be within the quotes symbol `" "`**

Ensure that `loadtest` is being selected as the `Default Namespace`. Click on `Import` to start the deployment.

`Sample yaml definition` 

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

![loadtest-yaml-modified-to-unique-rke-url](../images/loadtest-yaml-modified-to-unique-rke-url-16508833232382.png)

Let's check if the pod is up and running by going to `Workload` > `Pods`. You might need to select the namespace `loadtest`.

![loadtest-pod-success](../images/loadtest-pod-success.png)

With this we have successfully completed step 2 of the workshop.  

To summarize, we have deployed the micro-services, enabled Istio, injected Istio sidecars to our application, configured Istio Gateway & destination rule to access application and it's application traffic. Finally we have deployed a container to access the application. 

In the next steps, we will observe the application traffic via Kiali & Jaeger. 
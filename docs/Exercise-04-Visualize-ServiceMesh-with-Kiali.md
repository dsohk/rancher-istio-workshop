# Lab 04 - Visualize Service Mesh with Kiali

Once you have deployed the microservices app into `bookinfo` namespace and the traffic generator has been running to constantly visiting the application endpoint, we can make use of Kiali to visualize your service mesh. 

To access Kiali, 

`Cluster` > `Istio` > `Kaili`

![01-rke2cluster-istio-kiali-and-jaeger](../images/01-rke2cluster-istio-kiali-and-jaeger.png)

![Kiali-homepage](../images/Kiali-homepage.png)

If you have more namespace where Istio is configured, you may want to use filter & set Kiali to show particular namespace only. 

Click on `Graph`  to view the data plane Istio creates for our application. 

![data-plane-app-bookinfo](../images/data-plane-app-bookinfo-16555770380292.png)

Click on the drop down menu for `Display` & adapt the visualization to your requirement. 

![Selecting-labels-app-bookinfo](../images/Selecting-labels-app-bookinfo.png)

You can also check out `Istio Legends`

![Istio-Legends](../images/Istio-Legends-16508866338812.png)

By taking note of the `legends`, it is easier to see how your traffic is flowing between various components.

You can also toggle between various  topologies to change the layout visualization.

Check out option in `Traffic`

![kiali-traffic-protocol-choice](../images/kiali-traffic-protocol-choice.png)

Check out the option in `graph` 

![kiali-graph-choices](../images/kiali-graph-choices.png)



You can also narrow down to view communication between 2 micro services. 

![kiali-productpage-to-details-focused-view](../images/kiali-productpage-to-details-focused-view.png)

Click on `Details`  to look into details for application traffic between Productpage & Details 

![kiali-productpage-to-details](../images/kiali-productpage-to-details.png)

Inbound traffic details for `Detail`

![kiali-details-inbound-traffic-details](../images/kiali-details-inbound-traffic-details.png)

![kiali-details-inbound-metrics](../images/kiali-details-inbound-metrics.png)

You can also play around with the different options on the left menu. Select `Application Page`

![Istio-Application](../images/Istio-Application.png)

If you click on `productpage` under `Name` column you will see the traffic flow. 

![Application-productpage](../images/Application-productpage.png)



From the left menu, select `Workload` and then under `Name` column, click on `ratings-v1`,

![workload-rating1](../images/workload-rating1.png)

Similarly, from the left menu, select `Service`, and then click on `productpage` under `Name` column, you will see the Service flow. 

![istio-service-bookinfo-flow](../images/istio-service-bookinfo-flow.png)

Finally, from the left menu, select `Istio Config` and you will see all Istio component that you can view & configure. 

![Istio-config](../images/Istio-config.png)

To summarize, we have viewed how our Istio data plane look like. We can visualize our application traffic is going between each micro-services application that constitute our bookinfo application. 

In the next exercise 05, we will take a look at Jaeger which help us with distributed tracing

Click on this link to move to [Exercise-05-Explore Distributed Tracing with Jaeger](https://github.com/dsohk/rancher-istio-workshop/blob/main/docs/Exercise-05-ExploreDistributedTracingwithJaeger.md)



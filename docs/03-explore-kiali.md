# Lab 3 - Visualize Your Service Mesh

Once you have deployed the microservices app into `default` namespace and the traffic generator has been running to constantly visiting the application endpoint, we can make use of Kiali to visualize your service mesh. 

To access Kiali, 

`Cluster` > `Istio` > `Kaili`

![01-rke2cluster-istio-kiali-and-jaeger](../images/01-rke2cluster-istio-kiali-and-jaeger.png)

We have only configured Istio on namespace `default`, hence this will be the only namespace that you will see a green tick besides `Istio Config`

![kiali-ui-Istio-configured-for-default-ns](../images/kiali-ui-Istio-configured-for-default-ns.png)

Click on 3 Vertial dots in the namespace `default` and then select `graph`

![Istio-Graph](../images/Istio-Graph.png)



![Istio-traffic-visual-namespace-default](../images/Istio-traffic-visual-namespace-default.png)



Click on `Display` and you can tick the check boxes to play around the graphics.

![Istio-Traffic-Display-Show-Edge-Labels](../images/Istio-Traffic-Display-Show-Edge-Labels.png)

You can also check out `Istio Legends`

![Istio-Legends](../images/Istio-Legends-16508866338812.png)

By taking note of the `legends`, it is easier to see how your traffic is flowing between various components.

 ![http-communciation-between-istiogateway-product-app](../images/http-communciation-between-istiogateway-product-app.png)



For example, on the screenshot below, we can see the traffic between productpage Service & productpage Application. We can also tell this is a `http` traffic with 2.04 kps speed between the

![traffice-between-service-product-page-to-app-product-app](../images/traffice-between-service-product-page-to-app-product-app.png)

Next we can also see the traffic `productpage` to `reviews`. We can tell that the communication between the microservice is secured as it has been encrpyted.

![istioingress-2-productpage-reviews](../images/istioingress-2-productpage-reviews.png)



The screenshot below shows the communication between `reviews` to `rating`.

![review-2-rating](../images/review-2-rating.png)



You can also toggle between 3 topologies to change the layout visualization.

`Default Layout`

![Istio-default-layout](../images/Istio-default-layout.png)

Layout -1 - cose-bilkent

![Istio-layout-1-cose-bilkent](../images/Istio-layout-1-cose-bilkent.png)

Layout 2 - Cola 

![Istio-layout2-cola](../images/Istio-layout2-cola.png)

You can change the traffic metric refresh rate by adjusting the value of your choice.

![Istio-traffice-refresh-rate-options](../images/Istio-traffice-refresh-rate-options.png)

New value

![istio-traffic-refresh-changes](../images/istio-traffic-refresh-changes.png)



Application Page 

![Istio-Application](../images/Istio-Application.png)

If you click on `productpage` under `Name` column you will see the traffic flow. 

![Application-productpage](../images/Application-productpage.png)



Under `Workload` if you click on `ratings-v1`,

![workload-rating1](../images/workload-rating1.png)





Similarly Under `Service` Tab if you click on `productpage` under `Name` column you will see the Service flow. ![istio-service-bookinfo-flow](../images/istio-service-bookinfo-flow.png)

Finally, under `Istio Config` tab you will see all Istio component that you can view & configure. 

![Istio-config](../images/Istio-config.png)

To summarize, we have viewed how our application traffic is going from Istio Ingress gateway to various microservices.








# Explore Kiali

Show graph with traffic animation

To access Kiali, 

`Cluster` > `Istio` > `Kaili`

![01-rke2cluster-istio-kiali-and-jaeger](../images/01-rke2cluster-istio-kiali-and-jaeger.png)

We have only configured Istio with namespace `default` and hence you will only see Istio 

![kiali-ui-Istio-configured-for-default-ns](../images/kiali-ui-Istio-configured-for-default-ns.png)

Click on 3 Vertial dots in the namespace `Default`  

![Istio-Graph](../images/Istio-Graph.png)



![Istio-traffic-visual-namespace-default](../images/Istio-traffic-visual-namespace-default.png)



Click on `Display` and under `Show Edge Labels` select Labels of your choice.

![Istio-Traffic-Display-Show-Edge-Labels](../images/Istio-Traffic-Display-Show-Edge-Labels.png)

Istio Legends

![Istio-Legends](../images/Istio-Legends-16508866338812.png)

At this stage selecting legends, you will be easily able to identify how your traffic is flowing.

 ![http-communciation-between-istiogateway-product-app](../images/http-communciation-between-istiogateway-product-app.png)



Traffic between productpage Service & productpage Application. We can also tell this is a `http` traffic with 2.04 kps speed between the

![traffice-between-service-product-page-to-app-product-app](../images/traffice-between-service-product-page-to-app-product-app.png)

Next we can also see the traffic `productpage` to `reviews`. We can tell that the communication between the microservice is secured as it's encrpted.

![istioingress-2-productpage-reviews](../images/istioingress-2-productpage-reviews.png)



review to rating communication

![review-2-rating](../images/review-2-rating.png)



Various different Layout for visibility. You can click on 3 topology like icon to change the layout.

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

To summarize, we have viewed how our application traffic is going from Istio Ingress gateway to various microservices.








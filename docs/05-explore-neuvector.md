# Explore Neuvector

Access your Neuvector Server using the URL & credentials provided (over email or other communication.....).

Click on advance `Proceed to NeuvectorIP.sslip.io`

![neuvector-url](../images/neuvector-url.png)

Upon clicking on proceed to Neuvector, you will routed to Neuvector Login page. Provide your Neuvector Credential. 

![neuvector-login-page](../images/neuvector-login-page.png)

Accept the `End User License agreement` to proceed to Neuvector Homepage. 

![Neuvector-homepage](../images/Neuvector-homepage.png)

You get a good high level overview of 

1. `Security Risk Score` for `Nodes & Pods`
2. `Service Connection Risk`. 
   - 3 different modes. 1) `Discover` , 2) `Monitor` and 3) `Protect`
3. `Ingress/Egress Exposure Risk` 
   -  3 different modes. 1) `Discover` , 2) `Monitor` and 3) `Protect`
4. `Vulnerability Exploit Risk`
   -  3 different modes. 1) `Discover` , 2) `Monitor` and 3) `Protect`

 

## Container Vulnerability Scanning

When we deploy containers, we need to make sure we have acceptable level to image vulnerability so that it can be put to production. 

`Asset`  > `Containers`  >  `Auto-Scan`

![Neuvector-Assets-Containers-Autoscan-Off](../images/Neuvector-Assets-Containers-Autoscan-Off.png)

Upon turning `Auto Scan`  `= ` `on`you shall see the scan status `scheduled`

![Neuvector-Assets-Containers-Autoscan-Turned-ON](../images/Neuvector-Assets-Containers-Autoscan-Turned-ON.png)

![Neuvector-Assets-Containers-Autoscan-Status-Finished](../images/Neuvector-Assets-Containers-Autoscan-Status-Finished.png)

You can easily filter pod with their name to narrow you investigation. You can click on different tab such as `Compliance`, `Vulnerabilities`, `Process` and finally `Container Stats`

![Neuvector-Assets-Containers-Scan-Result](../images/Neuvector-Assets-Containers-Scan-Result.png)

Sample of Compliance for details Containers  ![Neuvector-Conatiner-Details-Complaince](../images/Neuvector-Conatiner-Details-Complaince.png)

Sample Vulnerabilities for details Container

![Neuvector-Conatiner-Details-Vulnerabilities](../images/Neuvector-Conatiner-Details-Vulnerabilities.png)

## Visualize Network Activity in Learning Mode

Match Rules: default namespace only

Hidden Namespace: 
- cattle-monitoring-system
- cattle-fleet-system
- cattle-system
- calico-system


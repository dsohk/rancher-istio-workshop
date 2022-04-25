# Lab 1 - Setup Istio



## Step 1 - Access your Rancher Cluster

Access your Rancher Server using the URL & credentials provided (over email or other communication.....). Click on advance `Proceed to RancherIP.sslip.io`

![01-rancher-url](../images/01-rancher-url.png)

![01-rancher-url-insecure-message](../images/01-rancher-url-insecure-message.png)

You will be presented with Rancher Server Login Page. Enter the Rancher Credentials.

![01-rancher-credentials](../images/01-rancher-credentials-16507073634331.png)

2. In Rancher Home page, click the `3-line` icon next to Rancher logo on the top left corner. Click on `Explore Cluster` > `Cluster Management`

![01-rancher-cluster-management-rke2cluster1](../images/01-rancher-cluster-management-rke2cluster1.png)

Click on `Explore` button on the right hand for Cluster `rke2-cluster1`

![](../images/01-rancher-cluster-management-rke2cluster1-explore.png)

You will be presented with `Cluster Dashboard` view for Cluster `rke2-cluster1`

![01-rke2cluster1-cluster-dashboard](../images/01-rke2cluster1-cluster-dashboard.png)

## Step 2 - Enable monitoring on RKE2

Before installing Istio, you need to enable Monitoring (Prometheus and Grafana) in the cluster tool of RKE2 cluster.

Click on `Cluster Tools` at the bottom of the left hand side menu pane. 

![01-rke2cluster1-cluster-dashboard](../images/01-rke2cluster1-cluster-dashboard.png)

Click `Install`  button of the `Monitoring` application.

![01-rke2cluster1-dashboard-cluster-tools](../images/01-rke2cluster1-dashboard-cluster-tools.png)

Choose `System` in `Install Into Project` selection box, and then click `Next`.

![01-rke2cluster1-monitoring-selecting-project-default](../images/01-rke2cluster1-monitoring-selecting-project-default.png)

Ideally you will leave the Prometheus values default, however for our lab since we are using 4 vCPU's let adjust our resource request as below

Resource Limits 

`Requested CPU = 250m`

`Requested Memory = 500Mi`

Rest all default

![01-rk2cluster1-monitoring-adjusting-promethus-value](../images/01-rk2cluster1-monitoring-adjusting-promethus-value.png)

Once Prometheus is successfully installed, you should success message as below. 

![01-rkecluster1-monitoring-installation-complete](../images/01-rkecluster1-monitoring-installation-complete.png)

Once Monitoring is installed, you will see the `Monitoring` as available left hand side menu pane. You can click on `Grafana` which will open browser window `Grafana Dashboard`

![01-rke2cluster1-monitoring-homepage](../images/01-rke2cluster1-monitoring-homepage.png)



## Lab 3 - Setup Istio with Rancher

After Monitoring add-on is installed, navigate to the `Cluster Tools` page, this time choose `Istio` and click on `Install` button.

![01-rke2cluster1-dashboard-cluster-tools](../images/01-rke2cluster1-dashboard-cluster-tools.png)

Choose `System` in `Install Into Project` selection box, and then click `Next`.

![01-rke2cluster1-istio-install-project-selection-value-default](../images/01-rke2cluster1-istio-install-project-selection-value-default.png)

On the `components` tab, check the box next to `Enabled CNI` and ` Enable Jaeger Tracing` to select the appropriate Istio components. 

![01-rke2cluster1-istio-component-selections](../images/01-rke2cluster1-istio-component-selections.png)

On the `Custom Overlay File` ta, add a custom overlay file like below to specify the path for `cniBinDir` and `cniConfDir`.

See Notes: https://rancher.com/docs/rancher/v2.6/en/istio/configuration-reference/rke2/

```yaml
apiVersion: install.istio.io/v1alpha1
kind: IstioOperator
spec:
  components:
    cni:
      enabled: true
      k8s:
        overlays:
        - apiVersion: "apps/v1"
          kind: "DaemonSet"
          name: "istio-cni-node"
          patches:
          - path: spec.template.spec.containers.[name:install-cni].securityContext.privileged
            value: true
  values:
    cni:
      image: rancher/mirrored-istio-install-cni:1.9.3
      excludeNamespaces:
        - istio-system
        - kube-system
      logLevel: info
      cniBinDir: /opt/cni/bin
      cniConfDir: /etc/cni/net.d
```

Your Custom Overlay file should look as below

![01-rke2cluster1-istio-customer-overlay-file](../images/01-rke2cluster1-istio-customer-overlay-file.png)

![01-Istio-Kiali-enable-auto-injection-final](../images/01-Istio-Kiali-enable-auto-injection-final.png)

Click `Install` button to start deploying Istio on RKE2 cluster.

![01-rke2-cluster1-istio-install-initated](../images/01-rke2-cluster1-istio-install-initated.png)

On successfully install of Istio, you should see below success message.

![01-rkecluster2-cluster-tools-istio-install-completed](../images/01-rkecluster2-cluster-tools-istio-install-completed.png)

You should now see `Istio` on left hand side menu pane & if you click on the drop down under Istio you should see `Kiali` and `Jaeger`

![01-rke2cluster-istio-kiali-and-jaeger](../images/01-rke2cluster-istio-kiali-and-jaeger.png)

You can click on `Kiali` and `Jaeger` to look at thier respective dashboard

![01-rke2cluster2-istio-kiali-Ui](../images/01-rke2cluster2-istio-kiali-Ui.png)

![01-rke2cluster1-istio-jaeger-ui](../images/01-rke2cluster1-istio-jaeger-ui.png)

# 

## Step 4 - Open Grafana and check Istio monitoring

Click on `Monitoring` and click on `Grafana` . 

Under Grafana Dashboards, you will see various Istio Dashboard options to view (Istio ControlPlane Dashboard,  Istio Mesh Dashboard, Istio Performance Dashboard, Istio Service Dashboard, etc...... )

![01-rke1cluster1-monitoring-dashboard-istio-options](../images/01-rke1cluster1-monitoring-dashboard-istio-options.png)

Next step is to enable Istio sidecare injection in the namespace where we would like to run our application. 

## Step 5 - Enable sidecar injection in default workload

Click `Istio` on the left pane menu and Click `Kiali`

Choose Default namespace, click `3 dots vertical line` and click on `Enable Auto Injection`

![01-rke2cluster1-istio-kiali-enable-auto-injection](../images/01-rke2cluster1-istio-kiali-enable-auto-injection.png)

Upon success you move your mouse on `Labels` you will see the injection successfully done 

![rke2cluster1-istio-kiali-enable-auto-injection-success](../images/rke2cluster1-istio-kiali-enable-auto-injection-success.png)

To summarize, we have successfully deployed Monitoring and Istio.

In the next exercise, we will deploy our micro-services application to take advantage of Istio. 

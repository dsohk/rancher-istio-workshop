# Lab 1 - Access your Rancher Server

1. Access your Rancher Server using the URL & credentials provided (over email or other communication.....)

   ![01-rancher-url](../../Images/01-rancher-url-16507062249782.png)

# Lab 2 - Setup Istio with Rancher

Access your 

Login to your instance of Rancher Server prepared for you in the lab environment. Then, follow the steps below to setup Istio with Rancher on its downstream RKE2 cluster.



## Step 1 - Navigate to Cluster Tools of RKE2 cluster

1. In Rancher Home page, click the 3-line icon next to Rancher logo on the top left corner.
2. Click `rke2-cluster` under Explorer section of the left side pane. This is the downstream RKE2 cluster we will be running Istio.
3. At the bottom of the left side menu pane, click `Cluster Tools` button.



## Step 2 - Enable monitoring on RKE2

Before installing Istio, you need to enable Monitoring (Prometheus and Grafana) in the cluster tool of RKE2 cluster.

1. Click `Install` button of the `Monitoring` application.
2. Choose `System` in `Install Into Project` selection box, and then click `Next`.
3. Accept the default settings of Prometheus and Grafana, click `Install` to start installation.



## Step 3 - Install Istio on RKE2

After Monitoring addon is installed, navigate to the `Cluster Tools` page and install Istio on the RKE2 cluster

See Notes: https://rancher.com/docs/rancher/v2.6/en/istio/configuration-reference/rke2/

1. Navigate to the `Cluster Tools` page again, this time, choose `Istio` to be installed on the RKE2 cluster, click `Install` button.
2. Choose `System` in `Install Into Project` selection box, and then click `Next`.
3. On the `components` tab, check the box next to `Enabled CNI`.
4. On the `Custom Overlay File` tab, add a custom overlay file like below to specify the path for `cniBinDir` and `cniConfDir`.

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

5. Click `Install` button to start deploying Istio on RKE2 cluster.



With this, both Istio and Prometheus are installed.

## Step 4 - Open Grafana and check Istio monitoring

Check Istio-related dashboard

## Step 5 - Enable sidecar injection in default workload

1. Click `Istio` on the left pane menu
2. Click `Kiali`
3. Choose Default namespace, click "..." icon
4. Check `Enable Auto Injection` in popup menu.


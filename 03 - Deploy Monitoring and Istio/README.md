# Enable Cluster Monitoring and Istio Service Mesh

## Enable Monitoring

In the Rancher UI...

1. Click In the Istio cluster.
1. Click on Enable Cluster monitoring.

With kubectl in the UI run the following command to view progress.
```
kubectl get pod -A
```
## Enable Istio

In the Rancher UI...

1. Click In the Istio cluster.
1. Click on tools then Istio.
1. Enable Ingress Gateway with default
1. Click Enable at the bottom of the page.

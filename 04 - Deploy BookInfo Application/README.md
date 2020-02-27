# Deploy BookInfo

### Start the application services

The default Istio installation uses automatic sidecar injection. Label the namespace that will host the application with istio-injection=enabled:
```
kubectl label namespace default istio-injection=enabled
```

Deploy application using the kubectl command:
```
kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.4/samples/bookinfo/platform/kube/bookinfo.yaml
```

Confirm all services and pods are correctly defined and running:
```
kubectl get services
```
and...
```
kubectl get pods
```

To confirm that the Bookinfo application is running, send a request to it by a curl command from some pod, for example from ratings:
```
kubectl exec -it $(kubectl get pod -l app=ratings -o jsonpath='{.items[0].metadata.name}') -c ratings -- curl productpage:9080/productpage | grep -o "<title>.*</title>"
```

><title>Simple Bookstore App</title>

### Determine the ingress IP and port

Define the ingress gateway for the application:
```
kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.4/samples/bookinfo/networking/bookinfo-gateway.yaml
```
Set the INGRESS_HOST and INGRESS_PORT variables for accessing the gateway.
```
export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].nodePort}')
export SECURE_INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="https")].nodePort}')
export INGRESS_HOST=$(kubectl get po -l istio=ingressgateway -n istio-system -o jsonpath='{.items[0].status.hostIP}')
```

Set GATEWAY_URL:
```
export GATEWAY_URL=$INGRESS_HOST:$INGRESS_PORT
```
### Confirm the app is accessible from outside the cluster

To confirm that the Bookinfo application is accessible from outside the cluster, run the following curl command:
```
curl -s http://${GATEWAY_URL}/productpage | grep -o "<title>.*</title>"
```
><title>Simple Bookstore App</title>

Run the following command to create default destination rules for the Bookinfo services:
```
kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.4/samples/bookinfo/networking/destination-rule-all.yaml
```

### Check destination rules...

You can display the destination rules with the following command:
```
kubectl get destinationrules -o yaml
```

## Create Ingress from the Rancher UI...

1. In the UI select the `default` namespace for the Istio cluster.
1. Then select Resources and click on `Workloads`.
1. Select `Loadbalancing` and click `Add Ingress`.
1. Give the ingress a name.
1. In the rules section select the target `productpage-v1` on port 9080
1. Hit `Save`.

Once the ingress becomes active then click on the xip.io URL in the Targets Section.

Pat Self On Back!!!!

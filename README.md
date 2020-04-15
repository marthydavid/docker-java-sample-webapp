docker-java-sample-webapp
=========================

## Problems with OpenShift 3.10.
* Without proper ServiceAccount configuration the pod never started with mounted/env used secrets.
* Haproxy-Route url rewrite capability missing.
* Haproxy-Route missing 80 to 443 redirect.
* Missing logging, monitoring for pods.

## Files:
* src/main/docker/Dockerfile used for building the container
* kubernetes.yaml file for deploying the app with predefined ingress host and secret


## Usage
Edit kubernetes.yml with the correct url and secret name then: `kubectl apply -f kubernetes.yaml`

## Test it:
Check if pod is running:

* `kubectl get pods --field-selector=status.phase=Running -l app=java-sample`
* `kubectl run -i -t busybox --image=busybox --restart=Never`
* `curl -v http://java-sample:8080/?name=World`
* `curl -v http://java-sample:8080/docker-java-sample-webapp-1.0-SNAPSHOT?name=World`

Check if rewrite is working through ingress
* `export INGRESS_URL=$(kubectl get ingress pod-service -ojsonpath='{.spec.rules[*].host}')`
* `curl -vL http://$INGRESS_URL/?name=World` or `open http://$INGRESS_URL/?name=World`
* `curl -vL http://$INGRESS_URL/docker-java-sample-webapp-1.0-SNAPSHOT?name=World` or `open http://$INGRESS_URL/docker-java-sample-webapp-1.0-SNAPSHOT?name=World`

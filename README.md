# OpenTelemetry
    OpenTelemetry (OTel) is an open-source observability framework that allows development teams to generate, process, and transmit telemetry data in a single, unified format.OpenTelemetry provides a common framework for collecting telemetry data and exporting it to an Observability back end of your choice. It uses a set of standardized, vendor-agnostic APIs, SDKs, and tools for ingesting, transforming, and transporting data.
    
## Implement OpenTelemetry in Kubernetes with Observability Tools
- Prometheus for metrics and Grafana for metrics visualizer
- Elasticsearch to store app logs and trace logs and visualze using Kibana
- Jaeger to visualize the distributed tracing.

## Note:
Sidecar annotations need to be added in all the main container for emitting the telemetry signals.

```sh
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.13.1/cert-manager.yaml

kubectl create namespace observability

// Elasticsearch
kubectl apply -f https://download.elastic.co/downloads/eck/2.9.0/crds.yaml

kubectl apply -f elastic-operator.yml -n observability

kubectl apply -f elasticsearch.yml -n observability

//  Kibana
kubectl apply -f kibana.yml -n observability

// Jaeger
kubectl apply -f https://github.com/jaegertracing/jaeger-operator/releases/download/v1.42.0/jaeger-operator.yaml -n observability

// we need to generate the jaeger password using elasticsearch password
kubectl get secret -n observability quickstart-es-elastic-user -o jsonpath='{.data}'
sample output: `{"elastic":"dWQ0OXc5cTlLdzE3cjhaeE8ybDJrN3Vp"}`

kubectl create secret generic jaeger-secret --from-literal=ES_PASSWORD=<1O817JM9DlbBzL4ML1C89Cg5> --from-literal=ES_USERNAME=elastic

kubectl apply -f simplest.yml -n observability

// Prometheus
kubectl apply -f prom-config-map.yml -n observability

kubectl apply -f prometheus-deployment.yml -n observability

kubectl apply -f prometheus-service.yml -n observability

// Grafana
kubectl apply -f grafana-datasources.yml -n observability

kubectl apply -f deployment.yml -n observability

kubectl apply -f service.yml -n observability

//OpenTelemetry Collector
helm repo add open-telemetry https://open-telemetry.github.io/opentelemetry-helm-charts

helm install opentelemetry-collector open-telemetry/opentelemetry-operator -n observability

//Otel instrumentation
kubectl apply -f otel-instrumentation.yml -n observability

kubectl apply -f valuesconfigmap.yml -n observability

//Sample Application
kubectl apply -f deployment.yml -n observability

kubectl apply -f service.yml -n observability
```
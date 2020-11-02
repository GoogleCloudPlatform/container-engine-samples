# Setting up HTTP(S) Load Balancing with Ingress example

This example shows how to run a web application behind
an [external HTTP(S) load balancer](https://cloud.google.com/load-balancing/docs/https)
using the [Ingress](https://cloud.google.com/kubernetes-engine/docs/concepts/ingress) resource.

Visit https://cloud.google.com/kubernetes-engine/docs/tutorials/http-balancer to follow the tutorial.

This directory contains:

- `basic-ingress.yaml` defines an Ingress resource
- `basic-ingress-static.yaml` defines an Ingress resource that uses a reserved IP address
- `fanout-ingress.yaml` defines an Ingress resource that routes requests to different Services by path

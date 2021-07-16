# Hello Application example

[![Open in Cloud Shell](https://gstatic.com/cloudssh/images/open-btn.svg)](https://ssh.cloud.google.com/cloudshell/editor?cloudshell_git_repo=https://github.com/GoogleCloudPlatform/kubernetes-engine-samples&cloudshell_tutorial=cloudshell/tutorial.md&cloudshell_workspace=hello-app)

This example shows how to build and deploy a containerized Go web server
application using [Kubernetes](https://kubernetes.io).

Visit https://cloud.google.com/kubernetes-engine/docs/tutorials/hello-app
to follow the tutorial and deploy this application on [Google Kubernetes
Engine](https://cloud.google.com/kubernetes-engine).

This directory contains:

- `main.go` contains the HTTP server implementation. It responds to all HTTP
  requests with a  `Hello, world!` response.
- `Dockerfile` is used to build the Docker image for the application.

This application is available as two Docker images, which respond to requests
with different version numbers:

- `gcr.io/google-samples/hello-app:1.0`
- `gcr.io/google-samples/hello-app:2.0`

This example is used in many official/unofficial tutorials, some of them
include:
- [Kubernetes Engine Quickstart](https://cloud.google.com/kubernetes-engine/docs/quickstart)
- [Kubernetes Engine - Deploying a containerized web application](https://cloud.google.com/kubernetes-engine/docs/tutorials/hello-app) tutorial
- [Kubernetes Engine - Setting up HTTP Load Balancing](https://cloud.google.com/kubernetes-engine/docs/tutorials/http-balancer) tutorial

# Hello Application example

## 

This tutorial shows how to build and deploy a containerized Go web server application using [Google Kubernetes Engine (GKE)](https://cloud.google.com/kubernetes-engine).

You'll learn how to create a cluster, and how to deploy the application to the cluster so that it can be accessed by users.

Let's get started!


## Set your GCP project

You can use an existing Google Cloud Platform project for this tutorial, or you can [create a project](https://cloud.google.com/resource-manager/docs/creating-managing-projects#creating_a_project).

Open the <walkthrough-editor-spotlight spotlightId="menu-terminal">terminal</walkthrough-editor-spotlight> and follow the steps below:

### 1. Set the default project    
```bash
gcloud config set project PROJECT_ID
```     
Replace PROJECT_ID with your [project id](https://support.google.com/cloud/answer/6158840).

### 2. Set the default compute/zone    
```bash
gcloud config set compute/zone COMPUTE_ZONE
```    
Replace COMPUTE_ZONE with your [compute zone](https://cloud.google.com/compute/docs/regions-zones#available), such as `us-west1-a`.


## Create a GKE cluster
A cluster consists of at least one cluster control plane machine and multiple worker machines called nodes. Nodes are [Compute Engine virtual machine (VM) instances](https://cloud.google.com/compute/docs/instances) that run the Kubernetes processes necessary to make them part of the cluster. 

GKE offers two [modes of operation](https://cloud.google.com/kubernetes-engine/docs/concepts/types-of-clusters#modes) for clusters: Standard and Autopilot. For this tutorial, we'll use Standard mode.

### 1. Create a Standard GKE cluster

Run the command below in your terminal to create a one-node Standard cluster named `hello-cluster`:
```
gcloud container clusters create hello-cluster --num-nodes=1
```

It might take several minutes to finish creating the cluster.


### 2. Get authentication credentials

After creating your cluster, you need to get authentication credentials to interact with the cluster.

```
gcloud container clusters get-credentials hello-cluster
```

This command configures `kubectl` to use the cluster you created.    


Next, let's deploy an app to the cluster.

## Deploy an application to the cluster

Now that you have created a cluster, you can deploy a [containerized application](https://cloud.google.com/kubernetes-engine/docs/concepts/kubernetes-engine-overview#workloads) to it. For this tutorial, let's deploy the [example web application](https://github.com/GoogleCloudPlatform/kubernetes-engine-samples/tree/master/hello-app) `hello-app`.

GKE uses Kubernetes objects to create and manage your cluster's resources. This example has two types of Kubernetes objects:
- a [Deployment](https://cloud.google.com/kubernetes-engine/docs/concepts/deployment) object, which deploys stateless applications like web servers 
- a [Service](https://cloud.google.com/kubernetes-engine/docs/concepts/service) object, which defines rules and load balancing for accessing your application from the internet

### Create the Deployment

To run `hello-app` in your cluster, run the following command:
```
kubectl create deployment hello-server --image=gcr.io/google-samples/hello-app:1.0
```

Let's break down what this command is doing:
- The Kubernetes command [`kubectl create deployment`](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create) creates a Deployment named `hello-server`. The Deployment's [Pod](https://cloud.google.com/kubernetes-engine/docs/concepts/pod) runs the `hello-app` container image.

- The command flag `--image` specifies a container image to deploy. In this case, the command pulls the example image from a [Container Registry](https://cloud.google.com/container-registry/docs) bucket, `gcr.io/google-samples/hello-app`. The suffix `:1.0` indicates the specific image version to pull. (If you don't specify a version, the latest version is used.)

## Expose the Deployment

After deploying the application, you need to expose it to the internet so that users can access it. You can expose your application by creating a Service, a Kubernetes resource that exposes your application to external traffic.

To expose your application, run the following [`kubectl expose`](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#expose) command:
```
kubectl expose deployment hello-server --type LoadBalancer --port 80 --target-port 8080
```

- Passing in the `--type LoadBalancer` flag creates a Compute Engine load balancer for your container. 

- The `--port` flag initializes public port 80 to the internet and the `--target-port` flag routes the traffic to port 8080 of the application.

Load balancers are billed per Compute Engine's [load balancer pricing](https://cloud.google.com/compute/pricing#lb).

Now let's inspect the Kubernetes resources and access your deployed web app.

## Inspect and view the application

1. Inspect the Pods by running [`kubectl get pods`](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get)
```bash
kubectl get pods
```

You should see one `hello-server` Pod running on your cluster.

2. Inspect the `hello-server` Service by running [`kubectl get service`](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get)
```bash
kubectl get service hello-server
```
From this command's output, copy the Service's external IP address from the `EXTERNAL-IP` column. (It may take a minute for the external IP address to show up. If the output reads `<pending>`, retry the command.)

3. View the application from your web browser by using the external IP address with the exposed port:
```
http://EXTERNAL_IP
```

You should see a message that reads `Hello, World!`, the container image version, and the Kubernetes Pod name.

## Conclusion

<walkthrough-conclusion-trophy></walkthrough-conclusion-trophy>

Congratulations! You've successfully deployed a containerized web application to GKE!

Don't forget to delete your cluster using the <walkthrough-editor-spotlight spotlightId="cloud-code-gke-explorer">GKE Explorer</walkthrough-editor-spotlight> to avoid any unwanted charges. Just right-click your cluster and then click **Delete Cluster**.

If you created a project specifically for this tutorial, you can delete it (and associated resources, including any clusters) using the [Projects page](https://console.cloud.google.com/cloud-resource-manager) in the Cloud Console.

<walkthrough-inline-feedback></walkthrough-inline-feedback>

##### What's next?
Try a [Cloud Shell GKE tutorial](https://shell.cloud.google.com/?walkthrough_tutorial_url=https%3A%2F%2Fwalkthroughs.googleusercontent.com%2Fcontent%2Fgke_cloud_code_create_app%2Fgke_cloud_code_create_app.md&show=ide&environment_deployment=ide) that shows you how to use the <walkthrough-editor-spotlight spotlightId="cloud-code-k8s-explorer">Kubernetes Explorer</walkthrough-editor-spotlight> to manage resources.

Read more about [managing GKE clusters](https://cloud.google.com/kubernetes-engine/docs/how-to/cluster-admin-overview), [deploying workloads](https://cloud.google.com/kubernetes-engine/docs/how-to/deploying-workloads-overview) to GKE, and recommended [best practices](https://cloud.google.com/kubernetes-engine/docs/best-practices) for working with GKE.

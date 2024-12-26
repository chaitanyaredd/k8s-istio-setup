# Kubernetes & Istio Setup for Python Web App on AWS EKS

## Pre-requisites
1. **aws-cli**: Command Line Interface for AWS.
2. **eksctl**: CLI tool for creating and managing EKS clusters.
3. **kubectl**: CLI tool for controlling Kubernetes clusters.
4. **istioctl**: CLI tool for managing Istio service mesh.
5. **Docker**: Platform for developing, shipping, and running applications in containers.

## Create Docker Image for Python Web App
1. Build the Docker image:
    ```sh
    docker build -t <your-dockerhub-username>/python-web-app:latest .
    ```
2. Push the Docker image to DockerHub:
    ```sh
    docker push <your-dockerhub-username>/python-web-app:latest
    ```

## Steps to Setup Cluster & Connect to Cluster
1. **Configure AWS CLI**:
    ```sh
    aws configure
    ```
    This command will prompt you to enter your AWS Access Key ID, Secret Access Key, region, and output format.

2. **Create EKS Cluster**:
    ```sh
    eksctl create cluster --name my-eks-cluster --region us-east-1 --nodes 1 --nodegroup-name service-nodes
    ```
    This command creates an EKS cluster named `my-eks-cluster` in the `us-east-1` region with one node in the `service-nodes` node group.

3. **Update kubeconfig to Connect to the Cluster**:
    ```sh
    aws eks --region us-east-1 update-kubeconfig --name my-eks-cluster
    ```
    This command updates your kubeconfig file to allow `kubectl` to interact with your EKS cluster.

4. **Delete the EKS Cluster (if needed)**:
    ```sh
    eksctl delete cluster --name my-eks-cluster --region us-east-1
    ```
     commandThis deletes the EKS cluster named `my-eks-cluster` in the `us-east-1` region.

## K8s Resources Setup
1. **Deploy Python Web Application**:
    ```sh
    kubectl apply -f deployment.yaml
    ```
    This command deploys your Python web application using the configuration in [deployment.yaml](http://_vscodecontentref_/0).

2. **Create Cluster-IP Service**:
    ```sh
    kubectl apply -f service.yaml
    ```
    This command creates a Cluster-IP service using the configuration in [service.yaml](http://_vscodecontentref_/1).

3. **Install Istio in EKS K8s Cluster**:
    ```sh
    istioctl install --set profile=demo -y
    ```
    This command installs Istio with the demo profile in your EKS Kubernetes cluster.

4. **Create Gateway**:
    ```sh
    kubectl apply -f gateway.yaml
    ```
    This command creates an Istio Gateway using the configuration in [gateway.yaml](http://_vscodecontentref_/2).

5. **Create VirtualService**:
    ```sh
    kubectl apply -f virtualservice.yaml
    ```
    This command creates an Istio VirtualService using the configuration in [virtualservice.yaml](http://_vscodecontentref_/3).

6. **Access the Application**:
    Access the application at `http://<external-ip>/demo`. Replace `<external-ip>` with the external IP address of your Istio ingress gateway.

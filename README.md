Kubernetes & Istio Setup for Python Web App on AWS EKS
========================
Pre-requisites
========================
1. aws-cli
2. eksctl
3. kubectl
4. istioctl
5. Docker

Create docker image for python-web-app

Push the docker image to dockerhub


================================================
Steps to setup cluster & connect to cluster
================================================
1. Configure AWS CLI:
    ```sh
    aws configure
    ```

2. Create EKS cluster:
    ```sh
    eksctl create cluster --name my-eks-cluster --region us-east-1 --nodes 1 --nodegroup-name service-nodes
    ```

3. Update kubeconfig to connect to the cluster:
    ```sh
    aws eks --region us-east-1 update-kubeconfig --name my-eks-cluster
    ```

4. Delete the EKS cluster (if needed):
    ```sh
    eksctl delete cluster --name my-eks-cluster --region us-east-1
    ```


======================
K8s resources setup
======================
1. Deploy python web application:
    ```sh
    kubectl apply -f deployment.yaml
    ```

2. Create Cluster-IP service:
    ```sh
    kubectl apply -f service.yaml
    ```

3. Install Istio in EKS k8s cluster:
    ```sh
    istioctl install --set profile=demo -y
    ```

4. Create gateway:
    ```sh
    kubectl apply -f gateway.yaml
    ```

5. Create virtualservice:
    ```sh
    kubectl apply -f virtualservice.yaml
    ```

6. Access the application at http://<external-ip>/demo
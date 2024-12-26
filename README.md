========================
Pre-requisites
========================
1. aws-cli
2. eksctl
3. kubectl
4. istioctl


================================================
Steps to setup cluster & connect to cluster
================================================
aws configure
eksctl create cluster --name my-eks-cluster --region us-east-1 --nodes 1 --nodegroup-name service-nodes
aws eks --region us-east-1 update-kubeconfig --name my-eks-cluster
eksctl delete cluster --name my-eks-cluster --region us-east-1


======================
K8s resources setup
======================
1. Deploy python web application
2. Create Cluster-IP service
3. Install Istio in EKS k8s cluster
4. Create gateway
5. Create virtualservice

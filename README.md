 EKS Demo README

This README provides step-by-step instructions to create an Amazon EKS cluster using Fargate launch type in the US East (N. Virginia) region.

 Prerequisites

Before you begin, ensure that you have the following:

- AWS CLI installed and configured with the necessary credentials.
- `eksctl` command-line tool installed.
- Basic familiarity with AWS services and Kubernetes concepts.

 Step-by-Step Instructions
1) Open a terminal and execute the following command to create the EKS cluster:


  eksctl create cluster --name demo-cluster --region us-east-1 --fargate --profile default 


2)
  eksctl create fargateprofile \
      --cluster demo-cluster \
      --region us-east-1 \
      --name alb-sample-app \
      --namespace game-2048



3) kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.5.4/docs/examples/2048/2048_full.yaml

4) add oidc

    export cluster_name=demo-cluster
  eksctl utils associate-iam-oidc-provider --cluster $cluster_name --approve

5)Download and create a iam policy 
   1)curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.5.4/docs/install/iam_policy.json
   2)aws iam create-policy \
    --policy-name AWSLoadBalancerControllerIAMPolicy \
    --policy-document file://iam_policy.json
   3)
   ![image](https://github.com/yash161/eks-demo/assets/62762507/6522ad18-eca4-46cd-9aa6-f4378d3528fb)
   4)changing role-eksctl create iamserviceaccount \
  --cluster=<your-cluster-name> \
  --namespace=kube-system \
  --name=aws-load-balancer-controller \
  --role-name AmazonEKSLoadBalancerControllerRole \
  --attach-policy-arn=arn:aws:iam::<your-aws-account-id>:policy/AWSLoadBalancerControllerIAMPolicy \
  --approve
  ![image](https://github.com/yash161/eks-demo/assets/62762507/2258b7ef-bde0-44bc-897e-e421f0194912)

  5)helm
    helm repo add eks https://aws.github.io/eks-charts
    helm repo update eks
    helm install aws-load-balancer-controller eks/aws-load-balancer-controller \
    -n kube-system \
    --set clusterName=demo-cluster \
    --set serviceAccount.create=false \
    --set serviceAccount.name=aws-load-balancer-controller \
    --set region=us-east-1 \
    --set vpcId=vpc-0245e5465b4d73832

  6)
   kubectl get ingress -n game-2048
  ![image](https://github.com/yash161/eks-demo/assets/62762507/655eedd2-1fe5-4067-b47d-16e34c687712)


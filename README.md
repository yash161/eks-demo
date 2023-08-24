# EKS Demo README

This README provides step-by-step instructions to create an Amazon EKS cluster using Fargate launch type in the US East (N. Virginia) region.

## Prerequisites

Before you begin, ensure that you have the following:

- AWS CLI installed and configured with the necessary credentials.
- `eksctl` command-line tool installed.
- Basic familiarity with AWS services and Kubernetes concepts.

## Step-by-Step Instructions

1. Open a terminal and execute the following command to create the EKS cluster:

```bash
eksctl create cluster --name demo-cluster --region us-east-1 --fargate --profile default

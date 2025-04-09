# AWS EKS Auto Mode with eksctl

> _"EKS Auto Mode extends AWS management of Kubernetes clusters beyond the cluster itself, to allow AWS to also set up and manage the infrastructure
 that enables the smooth operation of your workloads. You can delegate key infrastructure decisions and leverage the expertise of AWS for day-to-day
 operations. Cluster infrastructure managed by AWS includes many Kubernetes capabilities as core components, as opposed to add-ons, such as compute
 autoscaling, pod and service networking, application load balancing, cluster DNS, block storage, and GPU support."_
>
> Source: [Automate cluster infrastructure with EKS Auto Mode](https://docs.aws.amazon.com/eks/latest/userguide/automode.html)

## Assumptions

- Familiarity with AWS.
- `eksctl` is [installed](https://eksctl.io/installation/).
- You have an AWS IAM user account.
  - `eksctl` can access your AWS account credentials via environment variables or `~/.aws/credentials`.
- You have `kubectl` installed.
  - `kubectl` can access your AWS account credentials via environment variables or `~/.aws/credentials`.

## Resources

- [eksctl - The official CLI for Amazon EKS](https://eksctl.io/)
- [Automate cluster infrastructure with EKS Auto Mode](https://docs.aws.amazon.com/eks/latest/userguide/automode.html)
  - [Create an EKS Auto Mode Cluster with the eksctl CLI](https://docs.aws.amazon.com/eks/latest/userguide/automode-get-started-eksctl.html)
  - [Learn how EKS Auto Mode works](https://docs.aws.amazon.com/eks/latest/userguide/auto-reference.html)
  - [View Kubernetes resources in the AWS Management Console](https://docs.aws.amazon.com/eks/latest/userguide/view-kubernetes-resources.html)

## Create a Cluster

We'll use `eksctl` to create the cluster. It will take into account all the resources needed to stand up the cluster and create them for you.
 You can also use resources you've already created by applying the appropriate flags. See `eksctl --help` for a full list.

```txt
eksctl create cluster \
  --name <CLUSTER_NAME> \
  --version <VERSION> \
  --region <AWS_REGION> \
  --tags <KV_PAIR_LIST> \
  --enable-auto-mode
```

Replace `<CLUSTER_NAME>` with your cluster name, `<VERSION>` with the desired Kubernetes version to run, `<AWS_REGION>` with the AWS region where you want
 to create the cluster, and `<KV_PAIR_LIST>` with any resource tags you'd like add.

Example:

```txt
eksctl create cluster \
  --name eks-test-cluster \
  --version 1.31 \
  --region us-east-1 \
  --tags createdBy=john.doe@example.com,costCode=123ABC \
  --enable-auto-mode
```

You'll see output similar to this:

```txt
2024-12-03 09:02:43 [ℹ]  eksctl version 0.196.0
2024-12-03 09:02:43 [ℹ]  using region us-east-1
2024-12-03 09:02:43 [ℹ]  setting availability zones to [us-east-1a us-east-1b]
2024-12-03 09:02:43 [ℹ]  subnets for us-east-1a - public:192.168.0.0/19 private:192.168.64.0/19
2024-12-03 09:02:43 [ℹ]  subnets for us-east-1b - public:192.168.32.0/19 private:192.168.96.0/19
2024-12-03 09:02:43 [ℹ]  using Kubernetes version 1.30
2024-12-03 09:02:43 [ℹ]  creating EKS cluster "eks-test-cluster" in "us-east-1" region with 
2024-12-03 09:02:43 [ℹ]  if you encounter any issues, check CloudFormation console or try 'eksctl utils describe-stacks --region=us-east-1 --cluster=eks-test-cluster'
2024-12-03 09:02:43 [ℹ]  Kubernetes API endpoint access will use default of {publicAccess=true, privateAccess=false} for cluster "eks-test-cluster" in "us-east-1"
2024-12-03 09:02:43 [ℹ]  CloudWatch logging will not be enabled for cluster "eks-test-cluster" in "us-east-1"
2024-12-03 09:02:43 [ℹ]  you can enable it with 'eksctl utils update-cluster-logging --enable-types={SPECIFY-YOUR-LOG-TYPES-HERE (e.g. all)} --region=us-east-1 --cluster=eks-test-cluster'
2024-12-03 09:02:43 [ℹ]  2 sequential tasks: { create cluster control plane "eks-test-cluster", wait for control plane to become ready }
2024-12-03 09:02:43 [ℹ]  building cluster stack "eksctl-eks-test-cluster-cluster"
2024-12-03 09:02:43 [ℹ]  deploying stack "eksctl-eks-test-cluster-cluster"
2024-12-03 09:03:13 [ℹ]  waiting for CloudFormation stack "eksctl-eks-test-cluster-cluster"
2024-12-03 09:03:44 [ℹ]  waiting for CloudFormation stack "eksctl-eks-test-cluster-cluster"
2024-12-03 09:04:44 [ℹ]  waiting for CloudFormation stack "eksctl-eks-test-cluster-cluster"
2024-12-03 09:05:44 [ℹ]  waiting for CloudFormation stack "eksctl-eks-test-cluster-cluster"
2024-12-03 09:06:44 [ℹ]  waiting for CloudFormation stack "eksctl-eks-test-cluster-cluster"
2024-12-03 09:07:44 [ℹ]  waiting for CloudFormation stack "eksctl-eks-test-cluster-cluster"
2024-12-03 09:08:44 [ℹ]  waiting for CloudFormation stack "eksctl-eks-test-cluster-cluster"
2024-12-03 09:09:44 [ℹ]  waiting for CloudFormation stack "eksctl-eks-test-cluster-cluster"
2024-12-03 09:10:45 [ℹ]  waiting for CloudFormation stack "eksctl-eks-test-cluster-cluster"
2024-12-03 09:11:45 [ℹ]  waiting for CloudFormation stack "eksctl-eks-test-cluster-cluster"
2024-12-03 09:12:45 [ℹ]  waiting for CloudFormation stack "eksctl-eks-test-cluster-cluster"
2024-12-03 09:14:46 [ℹ]  waiting for the control plane to become ready
2024-12-03 09:14:47 [✔]  saved kubeconfig as "/Users/fakeuser/.kube/config"
2024-12-03 09:14:47 [ℹ]  no tasks
2024-12-03 09:14:47 [✔]  all EKS cluster resources for "eks-test-cluster" have been created
2024-12-03 09:14:47 [ℹ]  kubectl command should work with "/Users/fakeuser/.kube/config", try 'kubectl get nodes'
2024-12-03 09:14:47 [✔]  EKS cluster "eks-test-cluster" in "us-east-1" region is ready
```

Note that `eksctl` uses [CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html) to create the cluster and
 associated resources. You can view the CloudFormation Stack to see all of the resources that were created. You should see the EKS cluster, IAM role(s),
 EC2 security groups, EC2 internet gateways, EC2 NAT gateways, EC2 elastic IPs, EC2 routes, EC2 route tables, EC2 subnets, and EC2 VPC needed to run the
 cluster.

> [!IMPORTANT]  
> EKS clusters and resources created with `eksctl` must _always_ be managed via `eksctl` going forward. Modifying resources or making
 changes to the cluster manually will cause the CloudFormation stack to drift and make further updates with `eksctl` difficult or impossible.

`eksctl` will automatically create or update `~/.kube/config`, so you should be able to access the cluster with `kubectl`.

## Cluster Access via AWS Console

If you created your EKS cluster with a user other than the one you access the AWS Console with, or you want to allow other users to view the cluster via
 the AWS Console, you must map an IAM role to the Kubernetes cluster. This example assumes the IAM role has sufficient permissions, and that the user/role is
 allowed access to all Kubernetes namespaces. See
 [View Kubernetes resources in the AWS Management Console](https://docs.aws.amazon.com/eks/latest/userguide/view-kubernetes-resources.html) for more details.

### Create ClusterRole and ClusterRoleBinding

```txt
kubectl apply -f https://s3.us-west-2.amazonaws.com/amazon-eks/docs/eks-console-full-access.yaml
```

### Map IAM Role to Cluster Group

```txt
eksctl create iamidentitymapping \
  --cluster <CLUSTER_NAME> \
  --region <AWS_REGION> \
  --arn <ROLE_ARN> \
  --group eks-console-dashboard-full-access-group \
  --no-duplicate-arns
```

Replace `<CLUSTER_NAME>` with the name of your EKS cluster, `<AWS_REGION>` with the AWS region where the cluster is located, and `<ROLE_ARN>` with the ARN of the role you want
 to grant access to.

Example:

```txt
eksctl create iamidentitymapping \
  --cluster eks-test-cluster \
  --region us-east-1 \
  --arn arn:aws:iam::123456789000:role/ExampleRoleName \
  --group eks-console-dashboard-full-access-group \
  --no-duplicate-arns
```

IAM users with access to that role should now be able to view cluster resources via the AWS Console.

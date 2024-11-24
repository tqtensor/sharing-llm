# LiteLLM Proxy

## EKS Deployment

Step1. Create an EKS cluster with the following spec

```bash
eksctl create cluster \
  --name litellm \
  --region ap-southeast-1 \
  --node-type t3.small \
  --nodes 2 \
  --nodes-min 1 \
  --nodes-max 3 \
  --managed

# Reference: https://docs.aws.amazon.com/eks/latest/userguide/ebs-csi.html
eksctl utils associate-iam-oidc-provider \
  --cluster litellm \
  --region ap-southeast-1 \
  --approve

eksctl create addon \
  --cluster litellm \
  --name aws-ebs-csi-driver \
  --service-account-role-arn arn:aws:iam::$(aws sts get-caller-identity --query Account --output text):role/AmazonEKS_EBS_CSI_DriverRole \
  --region ap-southeast-1 \
  --force
```

Step 2. Mount LiteLLM proxy config on kubernetes cluster

This will mount your local file called `proxy_config.yaml` on kubernetes cluster

```bash
kubectl create configmap litellm-config --from-file=artifacts/litellm_config.yaml
```

Step 3. Create LiteLLM secrets

```bash
sops --decrypt artifacts/secret.yaml | kubectl apply -f -
```

Step 4. Apply deployments locally

```bash
kubectl apply -f artifacts/psql_deployment.yaml
kubectl apply -f artifacts/litellm_deployment.yaml
```

## Secrets Management

For secure handling of secrets, use AWS KMS with SOPS. Install the `signageos.signageos-vscode-sops` VSCode extension to edit encrypted secrets.

1. Create a symetric key in AWS KMS
2. Fill in the ARN of the key in `.sops.yaml`
3. Encrypt secrets with `sops --encrypt --in-place artifacts/secret.yaml`

## NGINX Ingress Controller

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.8.2/deploy/static/provider/aws/deploy.yaml
```

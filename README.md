# VProfile Infra — EKS (Terraform) & Argo CD

Infrastructure and GitOps configuration for the VProfile capstone: an Amazon **EKS** cluster provisioned with **Terraform**, and **Argo CD** deploying the app from Git.

## What this repo does

- **Terraform** provisions the EKS cluster, VPC, and supporting AWS resources (Infrastructure as Code), with remote state in S3
- Includes the **AWS Load Balancer Controller IAM policy** for ALB ingress
- **Argo CD** watches the Helm repo and auto-syncs the app to EKS

## End-to-end flow

```
GitHub Actions (build, test, SonarQube, push to ECR)   [vprofile-app-public]
        |
        v
Helm values.yaml updated in Git                        [vprofile-helm-public]
        |
        v
Argo CD detects the change and syncs to the cluster    [vprofile-infra-public]
        |
        v
Amazon EKS (provisioned by Terraform) -> app running
```

## Contents

- `main.tf`, `variables.tf`, `outputs.tf`, `backend.tf` — Terraform (EKS)
- `iam_policy.json` — AWS Load Balancer Controller policy
- `argocd-ingress.yaml` — Argo CD ingress

## Related repositories

- **[vprofile-app-public](https://github.com/thomasjosey540/vprofile-app-public)** — app + CI/CD
- **[vprofile-helm-public](https://github.com/thomasjosey540/vprofile-helm-public)** — Helm chart
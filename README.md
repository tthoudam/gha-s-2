# Terraform Reusable GitHub Actions Workflow

This GitHub Actions **reusable workflow** provides an automated and secure way to run Terraform `plan` or `apply` commands in a Google Cloud environment using **Workload Identity Federation**.

## 📌 Features

- Validates the requested Terraform command (`plan` or `apply`)
- Authenticates to Google Cloud using Workload Identity Federation
- Uses `hashicorp/setup-terraform` for Terraform CLI setup
- Supports configurable working directory and GCP project settings
- Designed to be reused across multiple repositories or workflows

## 🛠️ Usage

To use this workflow in another workflow, add a `workflow_call` reference:

```yaml
name: Deploy Terraform

on:
  push:
    branches: [main]

jobs:
  deploy:
    uses: your-org/your-repo/.github/workflows/solution_caller_workflow.yml@main
    with:
      command: apply
      work_dir: ./infra
      project_id: your-gcp-project-id
      workload_identity_provider: projects/123456789/locations/global/workloadIdentityPools/my-pool/providers/my-provider
      service_account: terraform-deployer@your-gcp-project.iam.gserviceaccount.com
    secrets:
      TF_API_TOKEN: ${{ secrets.TF_API_TOKEN }}
```

## 🔧 Inputs
| Name                     | Description                                      | Required | Default |
|--------------------------|--------------------------------------------------|----------|---------|
| `command`                | The Terraform command to run (`plan` or `apply`) | ✅ Yes  | —       |
| `work_dir`               | The Terraform working directory                  | ✅ Yes  | `.`     |
| `project_id`             | Google Cloud project ID                          | ✅ Yes  | —       |
| `workload_identity_provider` | The GCP Workload Identity Provider           | ✅ Yes  | —       |
| `service_account`        | The GCP service account to assume                | ✅ Yes  | —       |

## 🔐 Secrets

| Name           | Description                                                             | Required |
|----------------|-------------------------------------------------------------------------|----------|
| `TF_API_TOKEN` | Terraform Cloud API token (optional if not using TFC CLI configuration) | ✅ Yes   |


## 📚 Reference
- **Setting up Workload Identity Federation (WIF) on Google Cloud:**
  [Use GitHub Actions with Workload Identity Federation](https://github.com/google-github-actions/auth)

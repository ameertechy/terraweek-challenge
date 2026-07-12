# Day 1 (Sunday) — Introduction to Terraform

**TrainWithShubham #TerraWeek Challenge**

> Understand the basics of Infrastructure as Code, learn Terraform's key concepts, set up Terraform locally, and create your first configuration.

## What I built

A single [`main.tf`](./main.tf) that provisions real AWS infrastructure:
- **Provider** — `hashicorp/aws ~> 6.0`, region `us-west-2`
- **S3 bucket** — `aws_s3_bucket` with tags
- **AMI data source** — looks up the latest Ubuntu 24.04 image (Canonical) instead of hardcoding an ID
- **EC2 instance** — `t3.micro` that depends (implicitly) on the AMI lookup

Ran the full lifecycle: `init → plan → apply → state inspection → destroy`.

## Infrastructure as Code — in my own words

IaC means defining infrastructure in version-controlled text files that a tool executes, instead of clicking through a console or running a runbook by hand. It matters because infrastructure becomes **repeatable, reviewable, and auditable** — the same file yields the same environment every time, and `git diff` shows exactly what changed.

- **vs manual console work:** no click-ops drift, no undocumented snowflake servers — the code *is* the documentation.
- **Terraform vs others:** CloudFormation is AWS-only (Terraform is cloud-agnostic); Ansible configures existing machines while Terraform provisions the machines themselves; Pulumi does the same job in general-purpose languages while Terraform uses declarative HCL.
- **Declarative:** I describe the desired end state; Terraform computes the steps. **Cloud-agnostic:** one workflow provisions AWS, Azure, GCP, and more via providers.

## Key concepts learned

- **The reconciliation loop** — every run, Terraform diffs three things: my `.tf` (desired), the state file (last-known), and the live AWS API (actual). `plan` shows that diff before anything changes.
- **Implicit dependencies** — referencing `data.aws_ami.ubuntu.id` in the instance is what tells Terraform to resolve the AMI first. Order comes from references, not manual sequencing.
- **The state file** — `terraform.tfstate` maps resource names to real AWS IDs, can hold secrets in plain text, and must never be committed (it's git-ignored) or hand-edited. Remote backends with locking (Day 4) are the team-safe fix.

## Command reference

| Command | What it does |
|---|---|
| `terraform init` | Downloads providers into `.terraform/`, writes `.terraform.lock.hcl` |
| `terraform plan` | Dry run — shows the diff (`+` add, `~` change, `-` destroy) |
| `terraform apply` | Executes the plan after `yes`; records to state |
| `terraform destroy` | Removes all managed resources |
| `terraform state list` / `show` | Inspect what Terraform is tracking |

## Screenshots

| Step | Proof |
|---|---|
| `apply` — S3 bucket created | [01-apply-s3.png](./screenshots/01-apply-s3.png) |
| Bucket verified in AWS | [02-aws-s3-verify.png](./screenshots/02-aws-s3-verify.png) |
| `apply` — EC2 instance created | [03-apply-ec2.png](./screenshots/03-apply-ec2.png) |
| `terraform state list` | [04-state-list.png](./screenshots/04-state-list.png) |
| `terraform destroy` — all clean | [05-destroy.png](./screenshots/05-destroy.png) |

---

*Day 1 of the TrainWithShubham #TerraWeek Challenge · [back to index](../README.md)*

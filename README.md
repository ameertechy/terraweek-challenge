# TerraWeek Challenge — Terraform / Infrastructure as Code

> **TrainWithShubham #TerraWeek Challenge** · 7 days · July 12–17, 2026

A 7-day, hands-on journey through Terraform — from first `apply` to multi-resource, modular, state-managed infrastructure on AWS. Every day is a self-contained folder with runnable `.tf` code, a write-up, and screenshots of real AWS resources being created and destroyed.

## About me

I'm an infrastructure engineer with 7+ years of hands-on experience, using this challenge to sharpen **Infrastructure as Code** as I move deeper into cloud/platform engineering. The framing throughout these write-ups maps Terraform to real operational experience — because IaC isn't a new *idea* to me, but expressing it as code is the skill that matters now.

## Day-wise index

| Day | Topic | Folder |
|-----|-------|--------|
| 1 (Sun) | Introduction to Terraform — IaC, setup, first AWS infra | [day-01-introduction](./day-01-introduction) |
| 2 (Mon) | Terraform Configuration Language (HCL) — variables, types, expressions | [day-02-hcl](./day-02-hcl) |
| 3 (Tue) | Managing Resources — resource types, dependencies, provisioners, lifecycle | [day-03-managing-resources](./day-03-managing-resources) |
| 4 (Wed) | State Management — local vs remote, S3 backend, locking | [day-04-state-management](./day-04-state-management) |
| 5 (Thu) | Modules — reusable, composable infrastructure, versioning | [day-05-modules](./day-05-modules) |
| 6 (Fri) | Providers — configuration, authentication, multi-cloud | [day-06-providers](./day-06-providers) |
| 7 (Sat) | Advanced Topics — workspaces, best practices, CI/CD, registry | [day-07-advanced](./day-07-advanced) |

## Tools

- **Terraform** v1.15+
- **AWS** (region `us-west-2`) — provider `hashicorp/aws ~> 6.0`
- **Ubuntu 24.04** laptop for all runs
- Cost discipline: free-tier resources, `terraform destroy` after every session

## How to run any day

```bash
cd day-0X-topic
terraform init
terraform plan
terraform apply     # creates real AWS resources
terraform destroy   # always clean up
```

---

*Part of my [#90DaysOfDevOps](https://github.com/ameertechy/90DaysOfDevOps) journey · Built for the TrainWithShubham #TerraWeek Challenge.*

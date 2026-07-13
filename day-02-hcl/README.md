# Day 2 (Monday) — Terraform Configuration Language (HCL)

**TrainWithShubham #TerraWeek Challenge**

> Explore HCL syntax; learn variables, data types, and expressions; practice writing configurations with them.

## What I built

Day 1's infrastructure, rebuilt as a **fully parameterized, environment-aware config** — no hardcoded values left:

| File | Role |
|---|---|
| [`variables.tf`](./variables.tf) | 8 inputs across all five types (`string`, `number`, `bool`, `list(number)`, `map(string)`); `project_name` is required (no default); `environment` has a `validation` rule (dev/staging/prod only) |
| [`main.tf`](./main.tf) | Provider region from a variable; AMI + AZs via data sources; a security group whose ingress rules are **generated from a port list** (`dynamic` block); `count`-based EC2 |
| [`locals.tf`](./locals.tf) | `name_prefix` interpolation, `merge()`d common tags, and a conditional: monitoring is forced on in prod |
| [`outputs.tf`](./outputs.tf) | Instance IDs/IPs/DNS (splat expressions), SG ID, resolved AMI |
| [`terraform.tfvars`](./terraform.tfvars) / [`prod.tfvars`](./prod.tfvars) | dev vs prod value sets — same code, different environment |

## What I proved hands-on

- **Required variables stop autopilot** — with no value for `project_name`, `terraform plan` prompts and refuses to proceed.
- **The precedence chain** (lowest → highest): `default` → `TF_VAR_*` env vars → `terraform.tfvars` → `*.auto.tfvars` → `-var-file` / `-var` (last flag wins). Watched the instance type change t3.micro → t3.small → t2.nano across three plans without touching `.tf` code.
- **Lists become firewall rules** — `allowed_ports = [22, 80, 443]` generated exactly three ingress rules via `dynamic "ingress"`. New port in the list = new rule, zero code edits.
- **Expressions encode policy** — `var.environment == "prod" ? true : var.enable_detailed_monitoring` (verified `monitoring = false` in the dev apply).
- **Outputs are the config's API** — `terraform output -json` gives machine-readable results for scripts and pipelines.

Applied for real (1× t3.micro + SG in `us-west-2`, default VPC), verified outputs, then `terraform destroy` — state list empty, account clean.

## Screenshots

| Step | Proof |
|---|---|
| Required variable — plan prompts for `project_name` | [01-plan-prompt-required-var.png](./screenshots/01-plan-prompt-required-var.png) |
| `-var` CLI flag overriding both tfvars files | [02-precedence-cli-override.png](./screenshots/02-precedence-cli-override.png) |
| Apply complete + all outputs printed | [03-apply-outputs.png](./screenshots/03-apply-outputs.png) |
| `terraform output` / `-json` queries | [04-terraform-output.png](./screenshots/04-terraform-output.png) |

## Run it

```bash
terraform init
terraform plan                          # dev (terraform.tfvars auto-loads)
terraform plan -var-file="prod.tfvars"  # prod values
terraform apply
terraform output -json
terraform destroy
```

---

*Day 2 of the TrainWithShubham #TerraWeek Challenge · [back to index](../README.md)*

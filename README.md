# Voting App — AWS (CSW PoV)

![Visitors](https://visitor-badge.laobi.icu/badge?page_id=chandrapati.Voting_app&left_text=visitors)

Three-tier voting demo (web / app / SQL Server) on AWS with **VPC flow logs to S3** for the **Cisco Secure Workload AWS connector**.

> **Recommended:** use the Terraform stack in `terraform/` (same as [voting-app-aws](https://github.com/chandrapati/voting-app-aws)). Legacy zip + shell installers in this repo are kept for manual lab installs only.

---

## Quick start (Terraform + CSW flow logs)

```bash
cd terraform
cp terraform.tfvars.example terraform.tfvars
# generate SSH key in terraform/: ssh-keygen -t ed25519 -f voting-app-key -N ""
terraform init
terraform apply
```

Flow logs are **on by default** (`enable_vpc_flow_logs = true`):

- **Format:** CSW-required fields including `version`, `pkt-srcaddr`, `pkt-dstaddr`
- **Destination:** S3 plain-text, hourly partitions
- **Retention:** 7 days (lifecycle expiration)

```bash
terraform output vpc_flow_logs_s3_bucket
terraform output -raw csw_flow_log_bucket_name
bash ../scripts/verify-flow-logs.sh
```

Point the **CSW AWS connector** at that bucket (Flow Log Ingestion for this VPC).

---

## Legacy manual install

The root-level `setup-voting*.sh` scripts and `votingweb.zip` / `votingdata.zip` bundles install the app on existing VMs **without** VPC flow logs. For CSW agentless flow visibility, deploy with Terraform above.

---

## Related repo

[chandrapati/voting-app-aws](https://github.com/chandrapati/voting-app-aws) — primary mirror of the Terraform stack; changes are kept in sync with this repo.

# HCP Terraform — Complete Beginner's Guide

> A practical introduction to HCP Terraform (formerly Terraform Cloud), how it differs from Terraform CLI, its core components, pricing, and run workflows.

---

## Table of Contents

1. [What is HCP Terraform?](#1-what-is-hcp-terraform)
2. [Is HCP Terraform the Same as Terraform Cloud?](#2-is-hcp-terraform-the-same-as-terraform-cloud)
3. [HCP Terraform vs Terraform CLI](#3-hcp-terraform-vs-terraform-cli)
4. [Main Components and Features](#4-main-components-and-features)
5. [HCP Terraform Account and Pricing](#5-hcp-terraform-account-and-pricing)
6. [HCP Terraform Workflows](#6-hcp-terraform-workflows)
7. [Workflow 1 — UI/VCS-Driven](#7-workflow-1--uivcs-driven)
8. [Workflow 2 — CLI-Driven](#8-workflow-2--cli-driven)
9. [Workflow 3 — API-Driven](#9-workflow-3--api-driven)
10. [Workflows Comparison](#10-workflows-comparison)
11. [HCP Terraform vs CI/CD](#11-hcp-terraform-vs-cicd)
12. [Overall Architecture](#12-overall-architecture)
13. [Recommended Learning Path](#13-recommended-learning-path)

---

# 1. What is HCP Terraform?

**HCP Terraform** is HashiCorp's managed platform for running Terraform collaboratively.

It provides a centralized place where teams can:

- Store and manage Terraform state
- Run Terraform plans and applies
- Manage Terraform workspaces
- Store variables and sensitive values
- Connect Terraform projects to Git repositories
- Control team permissions
- Enforce policies
- Maintain run history
- Share private Terraform modules
- Automate Terraform through APIs

The easiest way to understand it is:

> **Terraform is the Infrastructure as Code tool, while HCP Terraform is the managed platform that helps teams use Terraform together.**

### Terraform CLI without HCP Terraform

A simple Terraform setup may look like:

```text
Your Laptop
     |
     | terraform plan / apply
     v
Terraform CLI
     |
     v
AWS
```

Terraform code and execution are primarily managed from your local machine.

### Terraform with HCP Terraform

With HCP Terraform:

```text
                  HCP Terraform
                +---------------+
Git / CLI ----->|  Workspaces   |
                |  Remote State |
                |  Runs         |
                |  Variables    |
                |  Permissions  |
                |  Policies     |
                +-------+-------+
                        |
                        v
                       AWS
```

HCP Terraform provides the collaboration and management layer around Terraform.

---

# 2. Is HCP Terraform the Same as Terraform Cloud?

## Yes.

**Terraform Cloud** was the previous name for **HCP Terraform**.

HashiCorp renamed Terraform Cloud to HCP Terraform in April 2024.

Therefore, you may still find older tutorials, videos, blog posts, or documentation referring to:

```text
Terraform Cloud
```

while newer documentation uses:

```text
HCP Terraform
```

For practical purposes, they refer to the same HashiCorp cloud product.

---

# 3. HCP Terraform vs Terraform CLI

This is one of the most important concepts to understand.

## Terraform CLI

Terraform CLI is the command-line tool you install on your computer.

Examples:

```bash
terraform init
terraform validate
terraform plan
terraform apply
terraform destroy
terraform state
```

A normal local workflow looks like:

```text
                    Your Laptop

              +-------------------+
              |  Terraform CLI    |
              +---------+---------+
                        |
                        |
                        v
                     AWS APIs
                        |
                        v
                       AWS
```

Terraform CLI is the tool that executes Terraform operations.

---

## HCP Terraform

HCP Terraform is a managed service around Terraform.

It provides capabilities such as:

- Remote Terraform execution
- Remote state
- Workspaces
- Variables
- Secrets
- VCS integration
- Teams and permissions
- Run history
- Policy enforcement
- Private module registry
- APIs
- Run tasks
- Agents

An important concept is:

> **HCP Terraform does not replace Terraform itself. It provides a managed platform for running and managing Terraform.**

HCP Terraform can run Terraform remotely on your behalf.

```text
                 HCP Terraform
        +---------------------------+
        |                           |
        |       Workspace           |
        |            |              |
        |            v              |
        |     Terraform execution   |
        |            |              |
        |            v              |
        |     terraform plan/apply  |
        |                           |
        +-------------+-------------+
                      |
                      v
                     AWS
```

---

## Simple Analogy

Think of:

```text
Terraform CLI
     =
The Terraform engine/tool you use
```

and:

```text
HCP Terraform
     =
A managed platform that organizes, controls, and runs Terraform for teams
```

---

## Local Terraform vs HCP Terraform

### Without HCP Terraform

```text
Your Laptop
 |
 +-- Terraform CLI
 +-- Terraform code
 +-- AWS credentials
 +-- State/backend
 |
 v
AWS
```

### With HCP Terraform

```text
Your Laptop / GitHub
        |
        v
 HCP Terraform
        |
        +-- Terraform execution
        +-- Remote State
        +-- Variables
        +-- Secrets
        +-- Permissions
        +-- Policies
        +-- Run history
        |
        v
       AWS
```

---

# 4. Main Components and Features

There are many HCP Terraform features, but the following are the most important ones to understand first.

---

## 4.1 Organization

An **Organization** is the top-level container in HCP Terraform.

For example:

```text
HCP Terraform Organization
        |
        +-- Teams
        +-- Workspaces
        +-- Policies
        +-- Variables
        +-- Private Registry
```

A company or team can use an organization to manage its Terraform infrastructure.

For learning purposes, you could have:

```text
Organization:
raosharif-labs
```

---

## 4.2 Workspace

A **Workspace** is one of the most important concepts in HCP Terraform.

A workspace manages a Terraform configuration and its associated state.

Think of it as a central location for one Terraform infrastructure project.

For example:

```text
Organization
     |
     +-- AWS-3tier
     |
     +-- AWS-networking
     |
     +-- AWS-monitoring
```

A workspace can contain/manage:

```text
Workspace
   |
   +-- Terraform Configuration
   +-- State
   +-- Variables
   +-- Secrets
   +-- Runs
   +-- Permissions
   +-- Terraform Version
```

---

## 4.3 Remote State

Terraform needs state to keep track of infrastructure.

A traditional setup may use an S3 backend:

```text
Terraform CLI
      |
      v
     S3
      |
      +-- terraform.tfstate
```

HCP Terraform can manage Terraform state remotely:

```text
Terraform CLI
      |
      v
HCP Terraform
      |
      v
Remote State
```

This is particularly useful for teams because the state is centralized instead of being maintained separately on every engineer's machine.

HCP Terraform also provides state history/versioning capabilities.

---

## 4.4 Remote Terraform Execution

Normally, you might run:

```bash
terraform plan
terraform apply
```

directly on your laptop.

With HCP Terraform, Terraform can execute remotely:

```text
Your Laptop
     |
     | terraform plan
     v
HCP Terraform
     |
     v
Remote Terraform Execution
     |
     v
AWS
```

HCP Terraform uses remote execution environments by default.

For private environments, HCP Terraform also supports **HCP Terraform Agents**, which allow Terraform runs to execute in infrastructure you control.

---

## 4.5 Variables and Secrets

Instead of keeping sensitive values inside local files such as:

```text
terraform.tfvars
```

you can manage variables through HCP Terraform.

For example:

```text
Workspace
   |
   +-- AWS_REGION
   +-- DB_USERNAME
   +-- DB_PASSWORD
   +-- API_KEY
```

Sensitive variables can be protected so their values are not exposed in normal UI/API views.

This becomes very useful for production environments.

---

## 4.6 VCS Integration

HCP Terraform can integrate with Git-based version control systems such as:

- GitHub
- GitLab
- Bitbucket

A typical workflow can look like:

```text
GitHub
   |
   | git push / Pull Request
   v
HCP Terraform
   |
   v
Terraform Plan
   |
   v
Policy Checks
   |
   v
Approval
   |
   v
Terraform Apply
   |
   v
AWS
```

A workspace can be connected to a VCS repository.

When changes are pushed to the configured branch, HCP Terraform can automatically create Terraform runs.

---

## 4.7 Teams and Permissions

HCP Terraform provides team-based access control.

For example:

```text
Organization
      |
      +-- DevOps Team
      |      |
      |      +-- Write Access
      |
      +-- Developers
      |      |
      |      +-- Read Access
      |
      +-- Security
             |
             +-- Policy / Governance Access
```

This is important in real-world environments because not every engineer should have the same Terraform permissions.

---

## 4.8 Private Module Registry

Organizations often create reusable Terraform modules.

For example:

```text
Private Registry

+-- VPC
+-- ALB
+-- EC2
+-- RDS
+-- EKS
```

Instead of every team creating these modules from scratch, they can consume approved internal modules.

Example:

```hcl
module "vpc" {
  source = "app.terraform.io/raosharif-labs/vpc/aws"
}
```

This encourages standardization and reuse.

---

## 4.9 Policies

Policies allow organizations to enforce infrastructure rules.

For example:

```text
Policy:
No public S3 buckets
```

or:

```text
Policy:
Production resources must use encryption
```

or:

```text
Policy:
EC2 instances cannot have public IP addresses
```

This moves Terraform from simply:

```text
Create infrastructure
```

toward:

```text
Create infrastructure
+
Govern infrastructure
+
Enforce security rules
```

HCP Terraform supports policy enforcement capabilities such as Sentinel and Open Policy Agent (depending on the plan and configuration).

---

## 4.10 Runs

Every Terraform execution in HCP Terraform is represented as a **Run**.

A run can include:

```text
Run
 |
 +-- Terraform Configuration
 |
 +-- Plan
 |
 +-- Policy Checks
 |
 +-- Approval
 |
 +-- Apply
```

HCP Terraform maintains run history, making it easier to understand what Terraform changes were executed and when.

---

# 5. HCP Terraform Account and Pricing

## Do I need an account?

Yes.

To use the hosted HCP Terraform service, you create an HCP Terraform account and an organization.

---

## Is HCP Terraform paid?

HCP Terraform has a **Free** tier as well as paid plans.

The current plan structure includes:

```text
Free
   |
   v
Essentials
   |
   v
Standard
   |
   v
Premium
```

The Free plan is suitable for learning and small projects.

According to HashiCorp's current documentation, the Free plan supports up to **500 managed resources** and includes many core HCP Terraform capabilities.

Paid plans provide additional capabilities and larger limits.

> **For learning HCP Terraform, you do not need to start with a paid plan.**

Always check HashiCorp's current pricing/documentation before using HCP Terraform for a production organization because plan limits and pricing can change.

---

# 6. HCP Terraform Workflows

HCP Terraform provides three main run workflows:

1. **UI/VCS-driven workflow**
2. **CLI-driven workflow**
3. **API-driven workflow**

These workflows determine how a Terraform run gets started.

---

# 7. Workflow 1 — UI/VCS-Driven

## Recommended workflow to learn first

This is one of the most important workflows for real-world team environments.

A typical architecture looks like:

```text
                       GitHub
                          |
                          | Push / Pull Request
                          v
                 +------------------+
                 |  HCP Terraform   |
                 |                  |
                 |    Workspace     |
                 +--------+---------+
                          |
                          v
                    Terraform Plan
                          |
                          v
                    Policy Checks
                          |
                          v
                       Approval
                          |
                          v
                    Terraform Apply
                          |
                          v
                         AWS
```

---

## Example

Suppose you have a GitHub repository:

```text
terraform-infrastructure/
|
+-- main.tf
+-- variables.tf
+-- outputs.tf
+-- modules/
+-- README.md
```

Your HCP Terraform workspace is connected to this repository.

A developer creates a branch:

```bash
git checkout -b feature/add-rds
```

They modify Terraform code.

Then:

```bash
git push origin feature/add-rds
```

They create a Pull Request.

HCP Terraform can generate a **speculative plan** for the Pull Request.

Example:

```text
Pull Request #42

Terraform Plan

+ aws_db_instance.production

Plan:
1 to add
0 to change
0 to destroy
```

The team reviews the Pull Request.

After approval, the change can be merged.

---

## Main branch workflow

```text
Developer
    |
    v
Feature Branch
    |
    v
Pull Request
    |
    v
HCP Terraform
    |
    v
Speculative Plan
    |
    v
Code Review
    |
    v
Merge to main
    |
    v
Terraform Plan
    |
    v
Policy Checks
    |
    v
Approval
    |
    v
Terraform Apply
    |
    v
AWS
```

This is a very common model for team-based Terraform workflows.

---

# 8. Workflow 2 — CLI-Driven

This workflow is especially useful if you prefer working with the Terraform CLI.

You still use commands such as:

```bash
terraform plan
terraform apply
```

But Terraform can be configured to use HCP Terraform as the backend and execution environment.

Instead of:

```text
terraform plan
      |
      v
Your Laptop
      |
      v
AWS
```

you can have:

```text
terraform plan
      |
      v
HCP Terraform
      |
      v
Remote Terraform Execution
      |
      v
AWS
```

The command still feels familiar:

```bash
terraform plan
```

but HCP Terraform handles the remote execution.

---

## Simplified CLI workflow

```text
Developer
    |
    | terraform plan
    v
HCP Terraform
    |
    v
Workspace
    |
    v
Remote Terraform Execution
    |
    v
AWS
```

This is a useful transition from traditional local Terraform to HCP Terraform.

---

# 9. Workflow 3 — API-Driven

The API-driven workflow is more automation-oriented.

Instead of a developer or VCS directly starting the run, another system can communicate with HCP Terraform through its API.

For example:

```text
Custom CI/CD
     |
     | HCP Terraform API
     v
HCP Terraform
     |
     v
Workspace
     |
     v
Terraform Run
     |
     v
AWS
```

The external automation could be:

```text
GitHub Actions
Jenkins
GitLab CI
Custom Automation
```

Example:

```text
GitHub
   |
   v
GitHub Actions
   |
   | HCP Terraform API
   v
HCP Terraform
   |
   v
Terraform
   |
   v
AWS
```

The API-driven workflow is powerful, but it is better to learn the VCS-driven and CLI-driven workflows first.

---

# 10. Workflows Comparison

| Workflow | Run Started By | Typical Use | Learning Priority |
|---|---|---|---|
| **UI/VCS-driven** | VCS or HCP Terraform UI | Team GitOps-style Terraform | ⭐⭐⭐⭐⭐ |
| **CLI-driven** | `terraform plan/apply` | Developers using CLI with HCP Terraform | ⭐⭐⭐⭐ |
| **API-driven** | External automation/API | Advanced CI/CD and automation | ⭐⭐⭐ |

### Simple way to remember

```text
UI/VCS-driven
GitHub
   ↓
HCP Terraform
   ↓
Terraform
   ↓
AWS
```

```text
CLI-driven
Terraform CLI
   ↓
HCP Terraform
   ↓
Terraform
   ↓
AWS
```

```text
API-driven
CI/CD / Automation
   ↓
HCP Terraform API
   ↓
Terraform
   ↓
AWS
```

---

# 11. HCP Terraform vs CI/CD

Do not think of HCP Terraform as simply another GitHub Actions.

They solve different problems.

## General CI/CD

GitHub Actions can handle:

```text
GitHub
   |
   v
GitHub Actions
   |
   +-- Build
   +-- Test
   +-- Security Scanning
   +-- Docker Build
   +-- Push Image
   +-- Deployment
```

It is a general-purpose CI/CD platform.

---

## HCP Terraform

HCP Terraform is specifically designed around Terraform:

```text
GitHub
   |
   v
HCP Terraform
   |
   +-- Terraform Plan
   +-- Remote State
   +-- Policy Checks
   +-- Approval
   +-- Terraform Apply
   +-- Run History
   +-- Workspace Management
```

Therefore:

> **GitHub Actions = general CI/CD automation**

> **HCP Terraform = Terraform-focused infrastructure management and execution platform**

They can also work together.

For example:

```text
GitHub
   |
   v
GitHub Actions
   |
   +-- Tests
   +-- Security Scans
   +-- Validation
   |
   v
HCP Terraform
   |
   +-- Plan
   +-- Policy
   +-- Approval
   +-- Apply
   |
   v
AWS
```

---

# 12. Overall Architecture

The following architecture is a useful mental model for HCP Terraform:

```text
                         +------------------+
                         |     GitHub       |
                         |                  |
                         | Terraform Repo   |
                         +--------+---------+
                                  |
                             Push / PR
                                  |
                                  v
                 +----------------------------------+
                 |          HCP Terraform            |
                 |                                  |
                 |  +----------------------------+  |
                 |  |       Organization         |  |
                 |  +-------------+--------------+  |
                 |                |                 |
                 |  +-------------v--------------+  |
                 |  |          Workspace         |  |
                 |  |                            |  |
                 |  |  Terraform Configuration   |  |
                 |  |  Variables / Secrets      |  |
                 |  |  Remote State              |  |
                 |  |  Runs                      |  |
                 |  |  Permissions               |  |
                 |  |  Policies                  |  |
                 |  +-------------+--------------+  |
                 |                |                 |
                 |                v                 |
                 |       Terraform Execution       |
                 |                |                 |
                 |          Plan / Policy           |
                 |                |                 |
                 |             Approval             |
                 |                |                 |
                 |              Apply               |
                 +----------------+-----------------+
                                  |
                                  v
                         +------------------+
                         |       AWS        |
                         |                  |
                         | VPC              |
                         | EC2              |
                         | ALB              |
                         | RDS              |
                         +------------------+
```

---

# 13. Recommended Learning Path

If you already understand Terraform CLI, AWS, Terraform state, modules, and remote backends, a good HCP Terraform learning sequence is:

```text
1. Create HCP Terraform account
             |
             v
2. Create Organization
             |
             v
3. Create Workspace
             |
             v
4. Understand Workspace Settings
             |
             v
5. Connect GitHub
             |
             v
6. Understand Remote State
             |
             v
7. Run Terraform through HCP Terraform
             |
             v
8. Learn VCS-driven Workflow
             |
             v
9. Learn Variables & Secrets
             |
             v
10. Learn Plan / Apply / Approval
             |
             v
11. Learn Teams & Permissions
             |
             v
12. Learn CLI-driven Workflow
             |
             v
13. Learn Private Module Registry
             |
             v
14. Learn Policies
             |
             v
15. Learn Run Tasks
             |
             v
16. Learn HCP Terraform Agents
             |
             v
17. Learn API-driven Workflow
             |
             v
18. Integrate HCP Terraform with CI/CD
```

---

# Practical Project Recommendation

A very good way to learn HCP Terraform is to migrate an existing Terraform project instead of creating a completely new project.

For example, if you already have a Terraform AWS 3-tier application:

```text
Current Setup
------------------------------------------------

Developer Laptop
       |
       v
Terraform CLI
       |
       v
S3 Backend
       |
       v
AWS
```

You can migrate it to:

```text
HCP Terraform Setup
------------------------------------------------

GitHub
   |
   v
HCP Terraform
   |
   +-- Workspace
   +-- Remote State
   +-- Variables
   +-- Terraform Runs
   +-- Policies
   +-- Permissions
   |
   v
AWS
```

This is an excellent exercise because most of your existing Terraform knowledge remains useful.

You are primarily learning:

- How Terraform is managed centrally
- How remote execution works
- How workspaces work
- How remote state is handled
- How GitHub integrates with Terraform
- How plans and applies are managed
- How approvals work
- How teams collaborate
- How Terraform governance works

---

# Quick Summary

| Concept | Meaning |
|---|---|
| **Terraform** | Infrastructure as Code tool |
| **Terraform CLI** | Command-line interface used to run Terraform |
| **HCP Terraform** | Managed platform for teams using Terraform |
| **Terraform Cloud** | Previous name for HCP Terraform |
| **Organization** | Top-level HCP Terraform container |
| **Workspace** | Manages a Terraform configuration and its state |
| **Remote State** | Centralized Terraform state managed by HCP Terraform |
| **Remote Execution** | Terraform runs in HCP Terraform instead of only locally |
| **VCS Integration** | Connect GitHub/GitLab/Bitbucket to Terraform |
| **Teams/RBAC** | Control who can access and operate Terraform |
| **Private Registry** | Share reusable Terraform modules |
| **Policies** | Enforce infrastructure and security rules |
| **Runs** | Individual Terraform executions |
| **Agents** | Run Terraform in private infrastructure |
| **API** | Programmatically control Terraform runs |

---

# Final Mental Model

The simplest way to remember everything is:

```text
Terraform CLI
     |
     | "I can run Terraform"
     v

HCP Terraform
     |
     +-- "I can run Terraform remotely"
     +-- "I can store/manage state"
     +-- "I can manage workspaces"
     +-- "I can manage variables/secrets"
     +-- "I can connect GitHub"
     +-- "I can manage teams/permissions"
     +-- "I can enforce policies"
     +-- "I can track Terraform runs"
     +-- "I can share modules"
     +-- "I can integrate with automation"
     |
     v
AWS / Azure / GCP / Other Providers
```

The key idea is:

> **Terraform is the IaC engine. HCP Terraform is the managed platform that makes Terraform easier to run, collaborate on, secure, govern, and operate at team scale.**

---

## Official Documentation

- [HCP Terraform Documentation](https://developer.hashicorp.com/terraform/cloud-docs)
- [Terraform Documentation](https://developer.hashicorp.com/terraform/docs)
- [HCP Terraform Workflows](https://developer.hashicorp.com/terraform/cloud-docs/workspaces/run)
- [Terraform Cloud / HCP Terraform CLI Integration](https://developer.hashicorp.com/terraform/cli/cloud)

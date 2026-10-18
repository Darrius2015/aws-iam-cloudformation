# AWS IAM Infrastructure with CloudFormation

## Project Overview

This project demonstrates how AWS Identity and Access Management (IAM) resources can be created, managed and updated using AWS CloudFormation.

As part of my cloud engineering training, I used Infrastructure as Code (IaC) to deploy IAM resources through a YAML template rather than creating each resource manually in the AWS Management Console.

The project also demonstrates how to verify deployed resources using the AWS CLI and inspect IAM permissions and trust relationships.

## Technologies Used

- AWS CloudFormation
- AWS Identity and Access Management (IAM)
- AWS Command Line Interface (CLI)
- YAML
- Visual Studio Code
- Git and GitHub

## Project Architecture

The CloudFormation stack contains four IAM resources:

| Resource | Description |
|----------|-------------|
| IAM User | Creates a user for the training environment |
| IAM Group | Creates a developer group |
| IAM Policy | Grants permission to list S3 buckets |
| IAM Role | Creates an EC2 role with an EC2 trust relationship |

All four resources are managed through a single CloudFormation stack.

## Implementation

### 1. Creating the CloudFormation Template

I created a YAML template defining the IAM resources required for the project.

The template included:

- An IAM user
- A developer IAM group
- An IAM policy
- An EC2 IAM role

CloudFormation uses the template to provision the resources in AWS.

### 2. Deploying and Updating the Stack

I deployed the infrastructure using AWS CloudFormation.

Rather than creating a separate stack for every change, I updated the existing stack when additional resources or configuration changes were required.

This demonstrated how CloudFormation can manage the lifecycle of infrastructure through a single template.

### 3. Configuring IAM Permissions

I configured an IAM policy granting the following permission:

```yaml
Effect: Allow
Action:
  - s3:ListAllMyBuckets
Resource: '*'
```

This permission allows the associated IAM group to list S3 buckets.

The public version of the template excludes the broad administrative permission used during the original training exercise.

### 4. Creating an EC2 IAM Role

I created an IAM role named:

`ec2-practice-role-cfm`

The role contains a trust policy allowing the EC2 service to assume it.

```yaml
Principal:
  Service: ec2.amazonaws.com

Action: sts:AssumeRole
```

This demonstrates the distinction between:

- Trust policies, which determine who or what can assume a role.
- Permissions policies, which determine what actions the role can perform.

The role was created without an attached permissions policy.

### 5. Verifying the Infrastructure

After deployment, I used the AWS CLI to inspect the IAM resources.

#### Verify IAM Group Membership

```bash
aws iam get-group \
  --group-name developer-cfm
```

#### Verify IAM Group Policies

```bash
aws iam list-group-policies \
  --group-name developer-cfm
```

#### Verify the EC2 Role

```bash
aws iam get-role \
  --role-name ec2-practice-role-cfm
```

I also verified the deployed resources through the AWS Management Console.

The CloudFormation stack reached:

`UPDATE_COMPLETE`

The stack contained four successfully deployed IAM resources.

## Security Considerations

Security was an important part of this exercise.

Key considerations included:

- Understanding the difference between IAM users, groups, roles and policies.
- Reviewing the permissions granted to IAM identities.
- Understanding EC2 role trust relationships.
- Avoiding unnecessary administrative permissions.
- Keeping sensitive credentials and local configuration files out of the public repository.

The original training exercise included an IAM user with administrative permissions.

For the public GitHub version, that administrative permission was removed from the template.

The public template is intended as a learning example and should be reviewed before deployment in another AWS environment.

## Lessons Learned

Through this project, I developed a better understanding of:

1. Creating AWS IAM resources using Infrastructure as Code.
2. Structuring CloudFormation YAML templates.
3. Updating existing CloudFormation stacks.
4. Managing IAM group permissions.
5. Understanding IAM roles and trust policies.
6. Verifying AWS resources using the AWS CLI.
7. Applying security considerations when publishing infrastructure code.

## Repository Structure

```text
aws-iam-cloudformation/
├── iam.yaml
├── README.md
└── .gitignore
```

### Files

- `iam.yaml` — Public CloudFormation IAM template.
- `README.md` — Project documentation.
- `.gitignore` — Excludes local files that should not be committed.

## Project Status

Completed as part of my practical AWS Cloud Engineering training.

The project demonstrates foundational AWS IAM, CloudFormation and AWS CLI skills.

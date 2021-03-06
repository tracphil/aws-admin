# TTS-wide AWS Account Adminstration

This repository contains AWS cross-account management for the [Technology Transform Service (TTS)](http://www.gsa.gov/portal/category/25729) and is managed by the [TTS Technology Portfolio](https://handbook.18f.gov/tech-portfolio/) within the [General Services Administration](http://gsa.gov).

## Setup

1. [Set up AWS credentials](https://blog.gruntwork.io/authenticating-to-aws-with-the-credentials-file-d16c0fbcbf9e) for the AWS account `133032889584`
1. [Install Terraform](https://learn.hashicorp.com/terraform/getting-started/install.html)
1. Clone this repository
1. Set up Terraform

   ```sh
   cd aws-admin/terraform
   terraform init
   ```

1. Confirm the AWS connection works

   ```sh
   terraform plan
   ```

## Cross-account access

_Based on [these steps](https://docs.aws.amazon.com/en_pv/IAM/latest/UserGuide/tutorial_cross-account-with-roles.html)._

**Source account: `133032889584`**

### Signing in to destination accounts

1. [Log in to the source account using IAM](https://133032889584.signin.aws.amazon.com/console)
1. Use the `Switch role URL` from the [AWS accounts list](https://docs.google.com/spreadsheets/d/1DedSCiU9AsCAAVvAFZT0_Ii7AFIKlI-JNifzlpHNbDg/edit#gid=0)

[More info.](https://docs.aws.amazon.com/en_pv/IAM/latest/UserGuide/id_roles_use_switch-role-console.html)

## Budgets

Budgets are listed by business unit in two places:

- [The AWS accounts spreadsheet](https://docs.google.com/spreadsheets/d/1DedSCiU9AsCAAVvAFZT0_Ii7AFIKlI-JNifzlpHNbDg/edit#gid=1269506691)
- AWS Systems Manager Parameter Store

To add a new one:

1. Sign into the payer account
1. Go to the [Parameter Store](https://console.aws.amazon.com/systems-manager/parameters/?region=us-east-1&tab=Table#list_parameter_filters=Name:BeginsWith:%2Ftts%2Faws-budget)
1. Create a parameter
   1. For `Name`, use `/tts/aws-budget/<BUSINESS UNIT>`
      - Make `<BUSINESS UNIT>` lower-case, alphanumeric, with hyphens
   1. For `Value`, enter the monthly budget as an integer
1. Mimic [use of the `business_unit` module](terraform/organization.tf)

_Parameter Store is used to keep the values private._

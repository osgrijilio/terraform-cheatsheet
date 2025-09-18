# Terraform cheatsheet

## Install - deinstall

```bash
brew uninstall terraform
unlink /usr/local/bin/terraform
brew unlink terraform
```

## Cleanup state

```bash
find . -name .terraform.lock.hcl -exec rm -fr {} \;
find . -name ".terraform.*" -exec rm -fr {} \;
find . -name ".terraform.tfstate" -exec rm -fr {} \;
find . -name "*terraform.tfstate.backup" -exec rm -fr {} \;\n
```

## Status

```bash
cat .terraform.lock.hcl
cat terraform.tfstate | grep out
terraform fmt
terraform init
terraform lint
terraform output -raw admin_user_encrypted_console_password
terraform output -raw admin_user_encrypted_console_password| base64 --decode| gpg --decrypt
terraform output -raw developer_access_key_id
terraform output -raw developer_secret_access_key
terraform output -var="account_name=corobici" -var="account_id=609274416783"
terraform show
terraform show | grep -A5 -B5 "create_login_profile"\n\n
```

## Apply

```bash
terraform apply -var aws_region="us-west-2" -auto-approve
terraform apply -var="aws_region=us-west-2" -auto-approve
terraform apply -var="account_name=cariari" -var="account_id=704461208717" -auto-approve
terraform apply -var="account_name=corobici" -var="account_id=609274416783" -auto-approve
terraform apply <<< "yes"
TF_VAR_region=us-west-2 terraform apply
```

## Destroy

```bash
terraform destroy -auto-approve
terraform destroy -target=aws_cognito_user_pool.pool -var="aws_region=us-west-2" -auto-approve
terraform destroy -var aws_region="us-west-2" -auto-approve
terraform destroy -var="account_name=corobici" -var="account_id=609274416783"
terraform destroy -var="account_name=corobici" -var="account_id=609274416783" --auto-approve
terraform refresh
terraform refresh -var="account_name=corobici" -var="account_id=609274416783"
touch terraform.vars
```

## Plan

```bash
terraform plan -refresh-only
terraform plan -var aws_region="us-west-2"
terraform plan -var aws_region="us-west-2" -auto-approve
terraform plan -var region="us-west-2"
terraform plan -var tags='["tag1", "tag2"]'
terraform plan -var tags=["tag1", "tag2"]
terraform plan -var="account_name=cariari" -var="account_id=704461208717"
terraform plan -var="account_name=corobici" -var="account_id=609274416783"
terraform plan -var="account_name=corobici" -var="account_id=609274416783" -auto-approve
terraform plan -var="aws_region=us-west-2"
```

## Other

```bash
terraform console
terraform deploy
terraform validate
terraform version
terraform workspace create development
terraform workspace list
terraform workspace new development
terraform workspace new production
terraform workspace select development
terraform workspace select production
```

## Passing values to variables

### Define in __terraform.tfvars__ file

```hcl
aws_region = "us-west-2"
```

### Using environment variables

Terraform automatically picks up AWS_REGION or AWS_DEFAULT_REGION:

```bash
export AWS_REGION=us-west-2
terraform apply
```

### Using a shared AWS config/profile

in the AWS config (~/.aws/config):

```ini
[profile myprofile]
region = us-east-2
```

Then, in Terraform:

```hcl
provider "aws" {
  profile = "myprofile"
}
```

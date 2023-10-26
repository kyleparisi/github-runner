# Github Runner

Semi-automated setup of GitHub Action runners

## Prerequisites

- A valid `$GITHUB_TOKEN` in your current terminal (needs: `admin:org` permissions)
- A valid `$GITHUB_NAMESPACE` in your current terminal (i.e. `orgs/MyOrg` or `repos/kyleparisi/repo`)
- A valid `$GITHUB_USER` in your current terminal (i.e. `kyleparisi`)
- An AWS account with proper IAM permissions to create an ec2 launch template
- AWS CLI
- `jq` if you need to update the template from a previous version

## Getting Started

```shell
# edit cloud-config.tmpl to fit your needs
# create your cloud-config and config.json
./bin/render-templates
# us-east-1 has poor spot prices so pick a different region...
export AWS_DEFAULT_REGION=us-east-2
# add the EC2 template to your aws account
./bin/create-launch-template

# if you need to update the template, edit one or both of the templates
./bin/render-templates
./bin/create-launch-template-version
```

### Details



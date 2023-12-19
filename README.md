# Github Runner

Semi-automated setup of GitHub Action runners

## Prerequisites

- A valid `$GITHUB_TOKEN` in your current terminal (needs: `admin:org` permissions)
- A valid `$GITHUB_REGISTER_NAMESPACE` in your current terminal (i.e. `orgs/MyOrg` or `repos/kyleparisi/repo`)
- A valid `$GITHUB_RUNNER_NAMESPACE` in your current terminal (i.e. `MyOrg` or `kyleparisi/repo`)
- A valid `$GITHUB_USER` in your current terminal (i.e. `kyleparisi`)
- An AWS account with proper IAM permissions to create an ec2 launch template
- AWS CLI
- `jq` if you need to update the template from a previous version

## Getting Started

```shell
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


After you deploy your template you should be able to start a spot instance fleet using this template in AWS 
[here](https://us-east-2.console.aws.amazon.com/ec2/home?region=us-east-2#SpotInstancesLaunch:).  Now every time a spot
instance is terminated, your fleet will spin up a new fresh instance as a runner.  It is worth noting that this could 
happen in the middle of your github action.  That should be rare though.

The instance deployed from this template is rather minimal.  Ideally you can do everything you need using standard
docker [compose] patterns.

If you need to debug your runner, the github user you specified will be able to ssh into the machine per their public
keys on github ([example](https://github.com/kyleparisi.keys)).

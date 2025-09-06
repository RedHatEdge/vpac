# 02-Provision
This section covers how to deploy the open telemetry backend that leverages bootc images we built earlier

## Deploying to AWS

Copy public key of SSH key you want to use to SSH into instance using command below

```sh
pbcopy < ~/.ssh/<replace with pub key file name>
```

```sh
ansible-vault create ./vars/secrets.yaml
```

Add yaml snippet below into the editor as shown below and save and exit

```yaml
admin_user: ec2-user
ssh_key: '<replace with pub key copied>'
```

Set the vault secret in an environment variable as shown below

```sh
export VAULT_SECRET=<redacted>
```

Login to your AWS console and create a Amazon VPC. Note down the `subnet_id` and `security_group_id` of the public subnet and update [influx.yaml](../../../ansible/vars/influx.yaml) and [loki.yaml](../../../ansible/vars/loki.yaml). Additionally update the ami and the ssh key name to match your environment.

#### Provisioning Influx
At this point we are now ready to provision the Influx instance. Run the ansible playbook as shown below

```sh
cd ansible

ansible-playbook --vault-password-file <(echo "$VAULT_SECRET") launch-ec2.yaml -e @vars/influx.yaml
```

#### Provisioning Grafana/Loki
Next we will provision Grafana/Loki instance. Run the ansible playbook as shown below.

```sh
cd ansible

ansible-playbook --vault-password-file <(echo "$VAULT_SECRET") launch-ec2.yaml -e @vars/loki.yaml
```

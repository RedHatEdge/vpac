# Configure AWS account
Before we can leverage Bootc Image Builder to build AMI images we need to configure AWS account. This section details the steps. These steps are also automated using an Ansible playbook. You can manually run these steps or execute the ansible playbook as shown in the command below

```sh
cd ansible

ansible-playbook configure-aws.yaml
```


## Create an S3 bucket
Create an S3 bucket to store images using the command below

```sh
aws s3api create-bucket \
    --bucket=bootc-amis \
    --create-bucket-configuration LocationConstraint=$AWS_DEFAULT_REGION
```
## Create vmimport service role
Create vmimport service role by running command below

```sh
aws iam create-role \
  --role-name=vmimport \
  --assume-role-policy-document='{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": "vmie.amazonaws.com"
            },
            "Action": "sts:AssumeRole",
            "Condition": {
                "StringEquals": {
                    "sts:Externalid": "vmimport"  
                }
            }
        }
    ]}' \
--query='Role.Arn' \
--output=text | pbcopy
```
Note down the Role ARN: arn:aws:iam::102214807189:role/vmimport

Create role policy
Create an IAM policy for vmimport role

```sh
aws iam create-policy \
    --policy-name=vmimport_service_role_policy \
    --policy-document='{
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action":  [
                    "s3:GetBucketLocation",
                    "s3:GetObject",
                    "s3:PutObject"
                ],
                "Resource": [
                    "arn:aws:s3:::bootc-amis",
                    "arn:aws:s3:::bootc-amis/*"
                ]
            },
            {
                "Effect": "Allow",
                "Action": [
                    "s3:GetBucketLocation",
                    "s3:GetObject",
                    "s3:ListBucket",
                    "s3:PutObject",
                    "s3:GetBucketAcl"
                ],
                "Resource": [
                    "arn:aws:s3:::bootc-amis",
                    "arn:aws:s3:::bootc-amis/*"
                ]
            },
            {
                "Effect": "Allow",
                "Action": [
                    "ec2:ModifySnapshotAttribute",
                    "ec2:CopySnapshot",
                    "ec2:RegisterImage",
                    "ec2:Describe*"
                ],
                "Resource": "*"
            }
        ]}' \
--query='Policy.Arn' \
--output=text | pbcopy
```
Note down the policy ARN: arn:aws:iam::102214807189:policy/vmimport_service_role_policy

#### Attach the Policy to Role
Attach the IAM policy to the IAM role

```sh
aws iam attach-role-policy \
  --role-name=vmimport \
  --policy-arn=arn:aws:iam::102214807189:policy/vmimport_service_role_policy
```
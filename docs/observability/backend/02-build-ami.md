# 02-build-ami
This section will cover how to deploy the telemetry backend in AWS. First we need to build AMI images using Bootc Image Builder (BiB)
Before we can execute the commands here we need to configure the AWS account. If this is the first time, follow instructions [here](../../configure-aws.md) to configure the AWS account.

## Build AMI Image for deploying Influx on AWS
In this section we will pull the influx image tagged with `aws` from the registry and use Bootc Image Builder (BiB) tool to build an AMI image which can be used to provision EC2 instances in AWS

```sh
sudo podman pull $REGISTRY/$REGISTRY_USER/influxdb-bootc:aws

sudo podman run \
    --authfile=$PULL_SECRET \
    --rm \
    --privileged \
    --security-opt label=type:unconfined_t \
    --env AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID} \
    --env AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY} \
    --env AWS_DEFAULT_REGION=${AWS_DEFAULT_REGION} \
    --env AWS_ACCOUNT_ID=${AWS_ACCOUNT_ID} \
    -v /var/lib/containers/storage:/var/lib/containers/storage \
    registry.redhat.io/rhel9/bootc-image-builder:latest \
    --local \
    --type ami \
    --aws-ami-name influxdb-x86_64 \
    --aws-bucket bootc-amis \
    --aws-region us-west-2 \
    $REGISTRY/$REGISTRY_USER/influxdb-bootc:aws
```

## Build AMI image for deploying Loki on AWS
In this section we are going to pull the loki image tagged with `aws` from the registry will build AMI image for deploying grafana and loki using Bootc Image Builder (BiB) tool.

```sh
sudo podman pull $REGISTRY/$REGISTRY_USER/loki-bootc:aws

sudo podman run \
    --authfile=$PULL_SECRET \
    --rm \
    --privileged \
    --security-opt label=type:unconfined_t \
    --env AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID} \
    --env AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY} \
    --env AWS_DEFAULT_REGION=${AWS_DEFAULT_REGION} \
    --env AWS_ACCOUNT_ID=${AWS_ACCOUNT_ID} \
    -v /var/lib/containers/storage:/var/lib/containers/storage \
    registry.redhat.io/rhel9/bootc-image-builder:latest \
    --local \
    --type ami \
    --aws-ami-name loki-x86_64 \
    --aws-bucket bootc-amis \
    --aws-region us-west-2 \
    $REGISTRY/$REGISTRY_USER/loki-bootc:aws
```    

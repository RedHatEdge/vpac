# Virtualized Protection Automation and Control (vPAC) 
This repo contains all artifacts built to demonstrate virtualized protection automation and control (vpac) with RedHat Enterprise linux (RHEL) with Prempt RT Kernel and KVM providing the hypervisor layer for running virtualizes pac workloads. Currently we are using ABB's SSC600 SW platform but this can also be solutions provided by other vendors in the electrification space.

Fork this repo if you want to try the demos.


## Repository Variables
Table below lists repository variables leveraged in the Github actions workflow.

| Variable Name | Purpose | Default |
| ------------- | ------- | ------- |
| REGISTRY_USER | Change this if you want to push the images to your user in quay.io | rgopinat |

## Repository Secrets
Table below lists repository secrets leveraged in Github actions workflow. Be sure to obtain fresh activation keys and update secret before running the demo.

| Secret Name | Purpose |
| ------------- | ------- |
| INFLUX_ACTIVATION_KEY | Activation Key generated from console.redhat.com |
| LOKI_ACTIVATION_KEY | Activation key generated from console.redhat.com |
| ORGID | Organization ID from console.redhat.com |
| REGISTRY_USER_PASSWORD | Password to login to quay.io registry |

## Observability with Open Telemetry
Leverage Opentelemetry to forward metrics and logs to telemetry backend.

### Building Telemetry backend
For this demo we are going to use Influxdb to aggregate all the metrics and Loki for aggregating logs from RHEL hosts.



### Installing the Open Telemetry Collector
To install Otel collector on the RHEL hosts run an ansible playbook as shown in command below. Be sure to update the values in vars/otelcol.yaml to match to your specific config before running the playbook.


```sh
cd ansible
ansible-playbook ansible/install-configure-otelcol.yaml
```


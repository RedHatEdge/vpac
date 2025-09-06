# deploy-otel-collector
Deploy otel collector to collect metrics and logs and forward to telemetry backend.

Update [otelcol.yaml](../../ansible/vars/otelcol.yaml) vars file 

```yaml
loki:
  endpoint: http://<replace with public dns of loki host>:3100
influxdb:
  endpoint: http://<replace with public dns of influx host>:8086
  org: redhat # replace if needed
  bucket: vpac-metrics # replace if needed
```

## Inventory
Create an inventory file, see sample below

```yaml
---
all:
  hosts:
    vpac:
      ansible_host: "<replace with public dns of vpac rhel host"
      ansible_port: 22
      ansible_user: "ec2-user"
```

## Deploy collector
At this point we can now deploy the collector to the host. Run ansible playbook as shown below

```sh
ansible-playbook -i inventory install-configure-otelcol.yaml -e @vars/otelcol.yaml
```sh


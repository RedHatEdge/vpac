# Observability with Open Telemetry
This section covers how to build an observability solution using Open Telemetry. There are basically two key steps involved.

* Build the telemetry backend stack
* Deploy Otel Collector onto vpac RHEL hosts

## Building Telemetry backend
For this demo we are going to use Influxdb to aggregate all the metrics and Loki for aggregating logs from RHEL hosts.

1. [Building Images](./backend/01-building-images.md)
2. [Build AMI](./backend/02-build-ami.md)
3. [Provision Telemetry Backend](./backend/03-provision.md)

## Deploy Otel Collector
1. [Deploy Otel Collector](./deploy-otel-collector.md)


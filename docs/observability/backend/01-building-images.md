# 01-Building Images
In this section we will build image mode containers used for deploying the telemetry backend which includes Influxdb for storing metrics data and Loki for log aggregation.

Follow steps below to build the influxdb and loki bootc images. 

* Fork this repo into your github account. 
* Update the repository variable REGISTRY_USER to set it to your user account in quay.
* Login to [console.redhat.com](https://console.redhat.com) with your redhat credentials and obtain two activation keys
* Set INFLUX_ACTIVATION_KEY and LOKI_ACTIVATION_KEY secrets with the activation keys you created earlier.
* Set REGISTRY_USER and REGISTRY_USER_PASSWORD secrets with your Red Hat credentials. This is used to login to registry.redhat.io to pull down base RHEL 9 bootc image
* Run Github actions workflows to build and push images to container registry


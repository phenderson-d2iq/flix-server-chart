# Flix Server

This chart will deploy a Flix Server instance in Kubernetes.

## TL;DR;

```console
$ helm install d2iq-demo-show flix-server/flix-server
```

> **Warning**: You must have a Flix Database ready to use, external-dns set up with Kommander, as well as a shared storage solution like NFS, or rook-ceph.

## Introduction

This chart installs a set of flix servers using your current configuration.

## Installing the Chart

To install the chart with the release name `d2iq-demo-show`:

```console
$ helm install flix-server/flix-server --name d2iq-demo-show
```

You must pass in values.

## Uninstalling the Chart

To uninstall/delete the `d2iq-demo-show` deployment:

```console
$ helm delete d2iq-demo-show
```

The command removes all the Kubernetes components associated with the chart and
deletes the release.

## Configuration

The following table lists the required values needed for the chart:

| Parameter               | Description                                                                                                                     | Required |
|:------------------------|:--------------------------------------------------------------------------------------------------------------------------------|:---------|
| `domainName`                     | This is the domain name that was set up with external-dns                                                              | Yes |
| `externalDNS.enabled`           | Enable or Disable external-dns                                                                                         | Yes |
| `replicaCount`                   | The number of Flix servers to spawn                                                                                    | Yes |
| `flix.instanceName`             | This will be the subdomain of your instances.                                                                          | Yes |
| `flix.config`                    | This will be the Flix config you have with your current set up                                                                  | Yes |
| `service.port`                   | This is the service port Flix Server listens on                                                                                 | Yes |
| `sharedVolume`                  | This is the shared storage solution required for assests. This is in YAML format and follows the k8s offical docs for volumes.  | Yes |
| `awsNLB.enabled`      | Enable or Disable AWS Network LB instead of classic LB                                                                        | No |
| `customAnnotations.enabled`      | Enable or Disable extra annotations for the service                                                                        | Yes |
| `customAnnotations.annotations`  | List of additional annotations you want for you service. If you are using AWS NLB make sure to include the required annotations            | Only when using awsNLB  |


## Example format of flix.config:

The following is a example of how to format you current flix config in the values.yaml:

```
flix:
  instance_name: d2iq-demo-show
  config:
    floating_license_hostname: 20.98.109.175
    floating_license_port: 4101
    shared_storage: true
    mysql_hostname: flix-database-svc.flixdemo.svc.cluster.local
    mysql_username: flix
    mysql_password: qGQGSLu6BM3hth
    mysql_database: flix-db
    mysql_max_connections: 140
``` 

---

## Full values.yaml example:

```
# This is the domain name you have configured for External-DNS
domainName: in-hyper.space

# If you have enabled external-dns with your kommander install you can enable automatic DNS registration for your services
externalDns:
  enabled: false

# This is the number of Flix Instances you want in your namespace
replicaCount: 3

# Flix Specific configurations:
# instance_name -  This is the value that will be used for the hostname for the flix servers. Note that this is incremented starting at 0. IE flix-server-0, flix-server-1, etc
# config - This is the configuration for your Flix Instance. Make sure its in proper yaml format. See commented out example at bottom of page.
flix:
  instanceName: d2iq-demo
  config:
    floating_license_hostname: 20.98.109.175
    floating_license_port: 4101
    shared_storage: true
    mysql_hostname: flix-database-svc
    mysql_username: flix
    mysql_password: qGQGSLu6BM3hth
    mysql_database: flix-db
    mysql_max_connections: 140

# This will be the port Flix client communicates on. You can also disable services if you want to use a hostNetwork option
service:
  enabled: true
  port: 8080

# Enables host network, tieing each flix server to a single node and BINDing the service port to the node
hostNetwork:
  enabled: false

# This is a kubernetes volume that has RWX capabilties that will be used for the assests folder.
sharedVolume:
  nfs: 
    server: 10.98.169.56
    path: /export/pvc-d9c9126f-f6a2-481d-ae38-71debacbfb62

# Enable AWS Network Load Balancer instead of Classic Load Balancer

awsNLB:
  enabled: false

# Add custom annotations if needed to the service created in your cluster
customAnnotations:
  enabled: false
  annotations:
    service.beta.kubernetes.io/aws-load-balancer-additional-resource-tags: "Name=nginx-svc,Environment=test,ManagedByRole=,ManagedBy=K8s,owner=phenderson,expiration=3 2h,Application=dkp"
    service.beta.kubernetes.io/aws-load-balancer-scheme: "internet-facing" 
    service.beta.kubernetes.io/aws-load-balancer-nlb-target-type: "instance" 
    service.beta.kubernetes.io/aws-load-balancer-type: "external" 
    service.beta.kubernetes.io/aws-load-balancer-nlb-target-type: "instance"
```
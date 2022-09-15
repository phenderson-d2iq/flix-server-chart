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
| `domainName`            | This is the domain name that was set up with external-dns                                                                       | Yes |
| `external-dns.enabled`  | Enable or Disable external-dns                                                                                                  | Yes |
| `replicaCount`          | The number of Flix servers to spawn                                                                                             | Yes |
| `flix.instance_name`    | This will be the subdomain of your instances.                                                                                   | Yes |
| `flix.config`           | This will be the Flix config you have with your current set up                                                                  | Yes |
| `service.port`          | This is the service port Flix Server listens on                                                                                 | Yes |
| `shared_volume`         | This is the shared storage solution required for assests. This is in YAML format and follows the k8s offical docs for volumes.  | Yes |


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
domainName: in-hyper.space

external-dns:
  enabled: true

# This is the number of Flix Instances you want in your namespace
replicaCount: 3

# Flix Specific configurations:
# instance_name -  This is the value that will be used for the hostname for the flix servers. Note that this is incremented starting at 0. IE flix-server-0, flix-server-1, etc
# config - This is the configuration for your Flix Instance. Make sure its in proper yaml format. See commented out example at bottom of page.
flix:
  instance_name: flintstones
  config:
    floating_license_hostname: 20.98.109.175
    floating_license_port: 4101
    shared_storage: true
    mysql_hostname: flix-database-svc.flixdemo.svc.cluster.local
    mysql_username: flix
    mysql_password: qGQGSLu6BM3hth
    mysql_database: flix-db
    mysql_max_connections: 140

# This will be the port Flix client communicates on
service:
  port: 8080

# This is a kubernetes volume that has RWX capabilties that will be used for the assests folder.
shared_volume:
  persistentVolumeClaim:
    claimName: flix-assest-pv-claim
```
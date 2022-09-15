# Flix Server

This chart will deploy a Flix Server instance in Kubernetes.

## TL;DR;

```console
$ helm install d2iq-demo-show flix-server/flix-server
```

> **Warning**: You must have a Flix Database ready to use, as well as a shared storage solution like NFS, or rook-ceph.

## Introduction

This chart installs a set of flix servers using your current configuration.

## Installing the Chart

To install the chart with the release name `d2iq-demo-show`:

```console
$ helm install flix-server/flix-server --name d2iq-demo-show
```

You must pass in values.

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```console
$ helm delete d2iq-demo-show
```

The command removes all the Kubernetes components associated with the chart and
deletes the release.

## Configuration

The following table lists the required values needed for the chart:

| Parameter                       | Description                                                                 | Required |
|:--------------------------------|:----------------------------------------------------------------------------|:---------|
| `mysqlDatabase.rootDBpassword`  | Password for the root MySQL User                                            | Yes |
| `mysqlDatabase.dbUser`          | User for the Flix Application                                               | Yes |
| `mysqlDatabase.dbPassword`      | Password for the Flix DB User                                               | Yes |
| `mysql_db_pvc.storageClassName` | StorageClass to use for MySQL backend                                       | Yes |
| `mysql_db_pvc.size`             | Size of MySQL DB                                                            | Yes |
| `flix.config`                   | This will be the basic Flix config to get a server running (license server) | Yes |


## Example format of flix.config:

The following is a example of how to format you current flix config in the values.yaml:

```
flix:
  config:
    floating_license_hostname: 20.98.109.175
    floating_license_port: 4101
    shared_storage: true
``` 

---

## Full values.yaml example:

```
mysqlDatabase:
  rootDBpassword: "JyXiP7JofmBer9"
  dbUser: "flix"
  dbPassword: "qGQGSLu6BM3hth"

mysql_db_pvc:
  storageClassName: ebs-sc
  size: 50Gi

# Flix Specific configurations:
# config - This is the configuration for your Flix Instance. Make sure its in proper yaml format. See commented out example at bottom of page.
flix:
  config:
    floating_license_hostname: 20.98.109.175
    floating_license_port: 4101
    shared_storage: true
```
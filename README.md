# Ansible AWX

Helm deployement of Ansible AWX on Kubernetes

## Introduction

This chart bootstraps an [AWX](https://github.com/ansible/awx) deployment on
a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh)
package manager.

## Installing the Chart

To install the chart with the release name `my-release`:

```console
helm dep up ./awx
helm install --name my-release ./awx
```

The command deploys AWX on the Kubernetes cluster in the default configuration.
The [configuration](#configuration) section lists the parameters that can be configured
during installation.

This charts embeds chart dependencies specified in the requirements.yaml file:

- postgresql
- memcached
- rabbitmq

**Note**: Currently, this chart is not ready to be used with external postgresql,
memcached or rabbitmq. PR welcomed.

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```console
helm delete my-release
```

The command removes all the Kubernetes components associated with the chart
and deletes the release.

## Configuration

The following table lists the configurable parameters of the
awx chart and their default values.
Postgresql, memcached, rabbitmq charts values can be overridden in
awx/values.yaml

Parameter | Description | Default
--------- | ----------- | -------
`replicaCount` | Pod replica count | `1`
`awx_web.image.repository` |  | `ansible/awx_web`
`awx_web.image.tag` |  | `2.1.2`
`awx_web.image.pullPolicy` |  | `IfNotPresent`
`awx_task.image.repository` |  | `ansible/awx_task`
`awx_task.image.tag` |  | `2.1.2`
`awx_task.image.pullPolicy` |  | `IfNotPresent`
`awx_secret_key` |  | `awxsecret`
`default_admin_user` |  | `admin`
`default_admin_password` |  | `password`
`deployment.annotations` |  | `{}`
`service.internalPort` |  | `8052`
`service.externalPort` |  | `8052`
`ingress.enabled` |  | `false`
`memcached.install` | Install memcached chart | `true`
`rabbitmq.install` | Install rabbitmq chart | `true`
`rabbitmq.rabbitmq.username` | Rabbitmq username | `awx`
`rabbitmq.rabbitmq.password` | Rabbitmq password| `awx`
`rabbitmq.rabbitmq.configuration` | Rabbitmq configuration file| cf values.yaml
`postgresql.install` | Install postgresql chart | `true`
`postgresql.postgresqlUsername` | postgresql username | `postgres`
`postgresql.postgresqlPassword` | postgresql password | `awx`
`postgresql.postgresqlDatabase` | postgresql database | `awx`
`postgresql.persistence.enabled` | postgresql persistence | `true`


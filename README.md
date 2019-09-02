# Ansible AWX

Helm deployement of Ansible AWX on Kubernetes

## Introduction

This chart bootstraps an [AWX](https://github.com/ansible/awx) deployment on
a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh)
package manager.

## Installing the Chart

To install the chart with the release name `my-release`:

```console
helm repo add rfy-awx https://raw.githubusercontent.com/rfyio/ansible-awx-helm-chart/master/
helm repo update
helm install --name my-release rfy-awx/awx
```

To install the development version:

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
`metrics.enabled` | Start a side-car prometheus exporter | `false`
`metrics.image.registry` | Exporter image registry | `docker.io`
`metrics.image.repository` | Exporter image name | `bitnami/rabbitmq-exporter`
`metrics.image.tag` | Exporter image tag | `{TAG_NAME}`
`metrics.image.pullPolicy` | Exporter image pull policy | `IfNotPresent`
`metrics.serviceMonitor.enabled` | Create ServiceMonitor Resource for scraping metrics using PrometheusOperator | `false`
`metrics.serviceMonitor.namespace` | Namespace where servicemonitor resource should be created | `nil`
`metrics.serviceMonitor.interval` | Specify the interval at which metrics should be scraped | `30s`
`metrics.serviceMonitor.scrapeTimeout`| Specify the timeout after which the scrape is ended | `nil`
`metrics.serviceMonitor.relabellings`| Specify Metric Relabellings to add to the scrape endpoint | `nil`
`metrics.serviceMonitor.honorLabels` | honorLabels chooses the metric's labels on collisions with target labels. | `false`
`metrics.serviceMonitor.additionalLabels`| Used to pass Labels that are required by the Installed Prometheus Operator | `{}`
`metrics.port` | Prometheus metrics exporter port | `9419`
`metrics.env` | Exporter [configuration environment variables](https://github.com/kbudde/rabbitmq_exporter#configuration) | `{}`
`metrics.resources` | Exporter resource requests/limit | `nil`
`metrics.capabilities` | Exporter: Comma-separated list of extended [scraping capabilities supported by the target RabbitMQ server](https://github.com/kbudde/rabbitmq_exporter#extended-rabbitmq-capabilities) | `bert,no_sort`

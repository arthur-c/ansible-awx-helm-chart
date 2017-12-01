# Ansible AWX

Helm deployement of Ansible AWX on Kubernetes

## Requirements

- Kubernetes config
- Helm
- Nginx ingress controller
- Kube-lego controller (Let's encrypt)
- kube2iam (awx role for AWS - mostly for ec2 discovery)
- Kubernetes storage class (AWS example in storageclass.yaml) 

## Configuration

Review helm packages configuration files.

### Postgresql (postgresql_parameters.yaml):
  - storageClass
  - persistentvolumeclaims
  - credentials

### RabbitMQ (rabbitmq_parameters.yaml):
  - storageClass
  - persistentvolumeclaims
  - credentials
  - vhost

### awx-web-task (awx-web-task_parameters.yaml):
  - docker image
  - hostname
  - awx credentials ([init script currently broken](https://github.com/ansible/awx/blob/devel/installer/image_build/files/launch_awx_task.sh), admin/password by default, change in the awx
    interface)
  - postgres host and credentials
  - rabbitmq host and credentials
  - memcached host


## Installation

Deployment in the default namespace.

Note: Please use the rabbitmq chart in this repos, waiting for a merge request
on the stable repository (https://github.com/kubernetes/charts/pull/2853)

```
helm install --name awx-postgresql -f postgresql_parameters.yaml stable/postgresql
helm install --name awx-rabbitmq -f rabbitmq_parameters.yaml rabbitmq
helm install --name awx-memcached  --set memcached.verbosity=v stable/memcached
helm install --name awx-web-task -f awx-web-task_parameters.yaml awx-web-task
```

## Backup

Backup not defined by default. If the k8s-snapshot controller, you can edit
your persistent volume:

```
kubectl patch pv pvc-606a8142-d5de-11e7-b985-0aac771b5ba8 -p '{"metadata": {"annotations": {"backup.kubernetes.io/deltas": "P0.5D P7D"}}}'
```

More info: https://github.com/miracle2k/k8s-snapshots

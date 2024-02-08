# pantheon-backup-helm

A helm chart to backup your Pantheon sites to S3 or SFTP from Kubernetes

## How it works

This chart provisions a cronjob which runs the [`ten7/pantheon-backup`](https://hub.docker.com/repository/docker/ten7/pantheon-backup) container. It uses the [`terminus`](https://pantheon.io/docs/terminus) command to create and download backups to the container's ephemeral storage. Once downloaded, the backup is them uploaded to zero or more remote servers. Each "remote" can be either and S3 bucket, or an SFTP server.

## Features

* Uses Pantheon's own tools to perform the backup.
* Can create the backup, or simply download the most recent one.
* Supports multiple S3 and SFTP destinations.
* A number of backups can be retained, and old backups are deleted automatically.
* Can be installed multiple times in the same namespace, with different schedules, allowing you to make hourly/daily/weekly/monthly backups.

## Requirements

* The container must have sufficient local storage to save the backup while downloading and uploading.
* A `terminus` Machine Token must be provisioned for the site.
* Connection and credentials for target S3 and/or SFTP providers.

## Installation

```shell
helm repo add pantheon-backup https://ten7.github.io/pantheon-backup-helm/
helm repo update
helm upgrade --install pantheon-backup pantheon-backup/pantheon-backup --namespace=my-namespace -f path/to/my-values.yml
```

## Persistence
Some versions of managed Kubernetes, such as GKE, have a hard limit on the amount of ephemeral storage you can request. In this cases, you may wish to enable persistence on the chart:

```yaml
persistence:
  enabled: true
  resources:
    requests:
      storage: 10Gi
```

This will mount the disk in the container at /tmp, alllowing larger backups to be processed.

## Configuration

For a full list of values, see [values.yaml](https://raw.githubusercontent.com/ten7/pantheon-backup-helm/main/charts/pantheon-backup/values.yaml).

## License

Gitea Backup is licensed under GPLv3. See [LICENSE](https://raw.githubusercontent.com/ten7/pantheon-backup-helm/main/LICENSE) for the complete language.

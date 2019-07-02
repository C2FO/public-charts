# pg-dba Helm Chart

- [pg-dba Helm Chart](#pg-dba-Helm-Chart)
  - [Chart Details](#Chart-Details)
  - [Installing the Chart](#Installing-the-Chart)
  - [Configuration](#Configuration)
  - [Examples](#Examples)
    - [Kick off a 1 time pg-dba job](#Kick-off-a-1-time-pg-dba-job)
  - [Running as a kubernetes `CronJob` on a schedule](#Running-as-a-kubernetes-CronJob-on-a-schedule)


## Chart Details

This deploys a kubernetes job that runs [pg-dba](https://github.com/jonstacks/pg-dba)
to help perform common database maintenance.

## Installing the Chart

To install the chart with the release name `my-pg-dba`:

```bash
$ helm install --name my-pg-dba c2fo-public/pg-dba
```

## Configuration

The following table lists the configuration parameters of the pg-dba Chart and their default values.


| Parameter               | Description                                                | Default                     |
| ----------------------- | ---------------------------------------------------------- | --------------------------- |
| `hooks.enabled`         | Whether or not to enable helm hooks to clean up the job    | false (due to a helm bug)   |
| `image.repository`      | The image repository to pull from                          | jonstacks/pg-dba            |
| `image.tag`             | The image tag to pull                                      | 0.1.0                       |
| `image.pullPolicy`      | The pull policy for the image                              | IfNotPresent                |
| `postgres.host`         | The postgres host to connect to                            | localhost                   |
| `postgres.db`           | The postgres db to connect to                              | postgres                    |
| `postgres.user`         | The user to connect to postgres as                         | postgres                    |
| `postgres.pass`         | The password to use when connecting to postgres            | ''                          |
| `postgres.sslMode`      | The SSL Mode to use for postgres(require, disable, etc...) | require                     |
| `resources`             | The resources to give to the pg-dba container              | {}                          |
| `timeouts.analyze`      | The timeout in seconds for running an analyze.             | `600`                       |
| `timeouts.vacuum`       | The timeout in seconds for running a vacuum.               | `600`                       |
| `timeouts.fullVacuum`   | The timeout in seconds for running a full vacuum.          | `600`                       |


## Examples

### Kick off a 1 time pg-dba job

```
helm install --name my-pg-dba c2fo-public/pg-dba \
     --set postgres.host=db \
     --set postgres.db=mydb \
     --set postgres.user=myuser \
     --set postgres.pass=mypass \
     --set postgres.sslMode=disable \
     --set timeouts.analyze=900 \
     --set timeouts.vacuum=900 \
     --set timeouts.fullVacuum=900
```

## Running as a kubernetes `CronJob` on a schedule

```
helm install --name my-pg-dba c2fo-public/pg-dba \
     --set postgres.host=db \
     --set postgres.db=mydb \
     --set postgres.user=myuser \
     --set postgres.pass=mypass \
     --set postgres.sslMode=disable \
     --set cronJob.enabled=true \
     --set cronJob.schedule="0 2 * * 6"
```
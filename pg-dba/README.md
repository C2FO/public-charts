# pg-dba Helm Chart


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


## Examples

```
helm install --name my-pg-dba c2fo-public/pg-dba \
     --set postgres.host=db \
     --set postgres.db=mydb \
     --set postgres.user=myuser \
     --set postgres.pass=mypass \
     --set postgres.sslMode=disable
```
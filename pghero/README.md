# pghero Helm Chart

- [Chart Details](#chart-details)
- [Installing the Chart](#installing-the-chart)
- [Configuration](#configuration)
- [Examples](#examples)
  - [Deploying pghero with an ingress](#deploying-pghero-with-an-ingress)

## Chart Details

This deploys a kubernetes job that runs [pghero](https://github.com/ankane/pghero)
to help with tuning a postgres database.

## Installing the Chart

To install the chart with the release name `my-pghero`:

```bash
$ helm install --name my-pghero c2fo-public/pghero --set databaseURL="postgres://<user>:<password>@<host>:<port>/<db-name>"
```

## Configuration

The following table lists the configuration parameters of the pg-dba Chart and their default values.


| Parameter               | Description                                                | Default                     |
| ----------------------- | ---------------------------------------------------------- | --------------------------- |
| `databaseURL`           | The connection string to be passed to pgheero              | '' (must be supplied)       |
| `image.repository`      | The image repository to pull from                          | jonstacks/pg-dba            |
| `image.tag`             | The image tag to pull                                      | 0.1.0                       |
| `image.pullPolicy`      | The pull policy for the image                              | IfNotPresent                |
| `ingress.annotations`   | Annotations for the ingress                                | {}                          |
| `ingress.enabled`       | Whether or not to deploy an ingress for pghero             | `false`                     |
| `ingress.hosts`         | The hosts for the ingress                                  | ['chart-example.local']     |
| `ingress.paths`         | The paths for the ingress                                  | ['/']                       |
| `ingress.tls`           | The TLS configuration for the ingress                      | []                          |
| `resources`             | The resources to give to the pg-dba container              | {}                          |
| `service.type`          | The service type                                           | ClusterIP                   |
| `service.port`          | The service port                                           | 80                          |

## Examples

### Deploying pghero with an ingress

```
helm install --name my-pghero c2fo-public/pghero \
     --set databaseURL=postgres://postgres:postgres@my-db.domain.com:5432/dbname \
     --set ingress.enabled=true \
     --set ingress.annotations."kubernetes\.io/ingress\.class"=nginx-internal \
     --set ingress.annotations."kubernetes\.io/tls-acme"=true \
     --set ingress.hosts[0]=my-pghero.endpoint.com \
     --set ingress.tls[0].secretName=pghero-cert \
     --set ingress.tls[0].hosts[0]=my-pghero.endpoint.com
```
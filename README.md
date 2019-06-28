# Tenants

[tenants](https://github.com/qlik-trial/tenants) stores and returns tenant information.

## Introduction

This chart bootstraps an tenants deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Installing the Chart

To install the chart with the release name `my-release`:

```console
helm install --name my-release qlik/tenants
```

The command deploys `tenants` on the Kubernetes cluster in the default configuration. The [configuration](#configuration) section lists the parameters that can be configured during installation.

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```console
helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following tables lists the configurable parameters of the `tenants` chart and their default values.

| Parameter                         | Description                                                                         | Default                                                                                |
| --------------------------------- | ----------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| `global.imageRegistry`            | Global image registry (overrides `images.registry`)                                 | `nil`                                                                                  |
| `image.registry`                  | image registry                                                                      | `qliktech-docker.jfrog.io`                                                             |
| `image.repository`                | image name without registry                                                         | `tenants`                                                                              |
| `image.tag`                       | image version                                                                       | `1.3.8`                                                                                |
| `image.pullPolicy`                | image pull policy                                                                   | `Always` if `image.tag` is `latest`, else `IfNotPresent`                               |
| `imagePullSecrets`                | a list of secret names for accessing private image registries                       | `[{name: "artifactory-registry-secret"}]`                                              |
| `replicaCount`                    | number of tenants replicas                                                          | `1`                                                                                    |
| `deployment.annotations`          | deployment annotations                                                              | `{}`                                                                                   |
| `service.type`                    | service type                                                                        | `ClusterIP`                                                                            |
| `service.port`                    | tenants listen port                                                                 | `8080`                                                                                 |
| `ingress.class`                   | the `kubernetes.io/ingress.class` to use                                            | `nginx`                                                                                |
| `ingress.authURL`                 | The URL to use for nginx's `auth-url` configuration to authenticate `/api` requests | `http://{.Release.Name}-edge-auth.{.Release.Namespace}.svc.cluster.local:8080/v1/auth` |
| `resources.requests.cpu`          | CPU reservation                                                                     | 9m                                                                                     |
| `resources.requests.memory`       | Memory reservation                                                                  | `100M`                                                                                 |
| `resources.limits.cpu`            | CPU limit                                                                           | 150m                                                                                   |
| `resources.limits.memory`         | Memory limit                                                                        | `150M`                                                                                 |
| `metrics.prometheus.enabled`      | enable annotations for prometheus scraping                                          | `true`                                                                                 |
| `mongodb.enabled`                 | enable Mongodb as a chart dependency                                                | `true`                                                                                 |
| `mongodb.uri`                     | URI to tenants DB. If the mongodb chart dependency isn't used, specify the URI path to mongo |                                                                               |
| `mongodb.uriWebIntegrations`      | URI to webintegrations DB. If the mongodb chart dependency isn't used, specify the URI path to mongo |                                                                            |
| `mongodb.uriSecretName`           | name of secret to mount for mongo URI. The secret must have the `mongodb-uri` key   | `{release.Name}-mongoconfig`                                                           |
| `mongodb.ssl`                     | Enables/disables ssl for the MongoDB connection (`true` or `false`). Can be overridden using ssl query parameter in the URI (?ssl=true or ?ssl=false). | `true`                                                   |
| `mongodb.sslValidate`             | Validate mongo server certificate against CA. Untrusted certificates will be rejected. | `true`                                                   |
| `mongodb.checkServerIdentity`     | Enforces that mongo server certificate CN matches mongo URI hostname/IP address.       | `true`                                                   |
| `jwks.uri`                        | URI where the JWKS to validate JWTs is located                                      | `http://{{ .Release.Name }}-keys:8080/v1/keys/qlik.api.internal`                                   |
| `privateKey`                      | PEM formated private key to sign service identity JWT                               |                                                                                        |
| `keyId`                           | The key id that matches the kid in the JWKS                                         | `Mf9AcMRQ-bLHduxLoE-POyozm6Ze6yfvW1Pyq3Fv1Bo`                                          |
| `configmaps.blacklistedHostnames` | Tenants configuration for reserved or blacklisted hostnames                         | `[]`                                                                                   |
| `config.logLevel`                 | Log level values (silly|debug|verbose|info|warn|error)                              | `verbose`                                                                                 |
| `config.resources.users`          | uri for users resource                                                              | `http://{.Release.Name}-users:8080/v1`                                                 |
| `config.resources.license`        | uri for license resource                                                            | `http://{.Release.Name}-licenses:9200/v1`                                              |
| `config.resources.internalTokens` | uri for internal-token resource                                                     | `http://{.Release.Name}-edge-auth:8080/v1`                                             |
| `config.resources.featureFlags`   | uri for feature flags resource                                                      | `http://{.Release.Name}-feature-flags:8080/v1`                                         |
| `environment`                     | The environment name                                                                | `example`                                                                              |
| `datacenter`                      | Deployed datacenter                                                                 | `example`                                                                              |
| `rollbar.enabled`                 | Enable rollbar to track server errors                                               | `nil`                                                                                  |
| `rollbar.token`                   | The rollbar token                                                                   | `nil`                                                                                  |
| `nats.enabled`                    | Enable NATS?                                                                        | `false`                                                                                |
| `nats.url`                        | NATS URL                                                                            | `http://{.Release.Name}-nats-client:4222`                                              |
| `nats.clusterId`                  | NATS cluster Id                                                                     | `{nats.release}-nats-streaming-cluster`                                               |
| `nats.clientId`                   | A unique NATS client id for this instance of the service                            | `nil`                                                                                  |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`.

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

```console
helm install --name my-release -f values.yaml qlik/tenants
```

> **Tip**: You can use the default [values.yaml](values.yaml)

# Turtl

[Turtl](https://traefik-forward-authapp.com/) is a self hosted collaborative notebook

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
$ helm install halkeye/traefik-forward-auth --name my-release
```

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the
release.

## Configuration

The following table lists the configurable parameters of the Turtl chart and their default values.

| Parameter                              | Description                                                                                                                  | Default                                           |
| -------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------- |
| `fullnameOverride`                     | Override the full resource names                                                                                             | `{release-name}-traefik-forward-auth` (or traefik-forward-auth if release-name is traefik-forward-auth) |
| `image`                                | Turtl image name                                                                                                           | `halkeye/traefik-forward-auth`                                         |
| `imageTag`                             | The version of the official Turtl image to use                                                                             | `latest`                                           |
| `serviceType`                          | A valid Kubernetes service type                                                                                              | `LoadBalancer`                                    |
| `loadBalancerIP`                       | An available static IP you have reserved on your cloud platform                                                              | None                                              |
| `startupArguments`                       | A list of startup arguments which are passed to traefik                                                              | `[]`                                              |
| `loadBalancerSourceRanges`             | List of IP CIDRs allowed access to load balancer (if supported)                                                              | None                                              |
| `externalIP`                           | Static IP for the service                                                                                                    | None                                              |
| `whiteListSourceRange`                 | Enable IP whitelisting at the entrypoint level.                                                                              | `false`                                           |
| `externalTrafficPolicy`                | Set the externalTrafficPolicy in the Service to either Cluster or Local                                                      | `Cluster`                                         |
| `replicas`                             | The number of replicas to run; __NOTE:__ Full Traefik clustering with leader election is not yet supported, which can affect any configured Let's Encrypt setup; see Clustering section | `1` |
| `podDisruptionBudget`                  | Pod disruption budget                                                                                                        | `{}`                                              |
| `rootCAs`                              | Register Certificates in the RootCA. These certificates will be use for backends calls. __NOTE:__ You can use file path or cert content directly | `[]`                                              |
| `cpuRequest`                           | Initial share of CPU requested per Traefik pod                                                                               | `100m`                                            |
| `memoryRequest`                        | Initial share of memory requested per Traefik pod                                                                            | `20Mi`                                            |
| `cpuLimit`                             | CPU limit per Traefik pod                                                                                                    | `200m`                                            |
| `memoryLimit`                          | Memory limit per Traefik pod                                                                                                 | `30Mi`                                            |
| `rbac.enabled`                         | Whether to enable RBAC with a specific cluster role and binding for Traefik                                                  | `false`                                           |
| `deploymentStrategy`                   | Specify deployment spec rollout strategy                                                                                     | `{}`                                              |
| `nodeSelector`                         | Node labels for pod assignment                                                                                               | `{}`                                              |
| `affinity`                             | Affinity settings                                                                                                            | `{}`                                              |
| `tolerations`                          | List of node taints to tolerate                                                                                              | `[]`                                              |
| `proxyProtocol.enabled`                | Enable PROXY protocol support.                                                                                               | `false`                                           |                                                               | `""`                                           |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example:

```bash
$ helm install --name my-release --namespace kube-system \
  --set dashboard.enabled=true,dashboard.domain=traefik.example.com stable/traefik
```

The above command enables the Traefik dashboard on the domain `traefik.example.com`.

Alternatively, a YAML file that specifies the values for the parameters can be provided while
installing the chart. For example:

```bash
$ helm install --name my-release --namespace kube-system --values values.yaml stable/traefik
```

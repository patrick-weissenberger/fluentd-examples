# Fluentd examples

The purpose of this repository is to show different input, processing and output scenarios that can be achieved with Fluentd.

## Deployment: Docker

The following configurations are mostly Docker related and can be ignored if you are using Fluentd on a Virtual Machine or in Kubernetes.

### Ports

The ports that will be opened in (docker-compose.yaml)[./docker-compose.yaml] and what data will be received on them.

| Port | Protocol | Data received |
| .-- | .-- | .-- |
| 5100 | TCP/ UDP | Raw |
| 5110 | TCP/ UDP | Syslog |

### Volumes

The volumes and their uses.

| Local path | Container path | Usecase |
| .-- | .-- | .-- |
| ./config/fluentd.conf | /fluentd/etc/fluentd.conf:ro | Fluentd configuration |
| ./logs/ | /fluentd/log/ | Persisiting generated logs |

### Commands

The default (Fluentd Docker Image)[https://hub.docker.com/_/fluentd] comes bundled with a default configuration file called <i>fluent.conf</i>. If you search the web for help configuring Fluentd, you will find the community standard is to access the <i>fluentd.conf</i> file.

| Command | Purpose |
| .-- | .-- |
| fluentd -c /fluentd/etc/fluentd.conf | Tell Fluentd to use this configuration file ((More information)[/fluentd/etc/fluentd.conf]) |

## Fluentd

The example streams are documented in the (fluentd.conf)[./config/fluentd.conf] file itself.
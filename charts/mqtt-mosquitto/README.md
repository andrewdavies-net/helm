Mosquitto
=========
Mosquitto MQTT server on Kubernetes

Current chart version is `0.0.1`

Source code can be found [here](https://mosquitto.org/)

Leveraging the [Mosquitto Docker Image](https://github.com/eclipse/mosquitto)

## Addtional info
This is a basic mosquitto chart that was initially created for my home purpose, here is a few notes to those that come across it. It exposes on two ports 1883 & 8883. 8883 has tls enabled. The certificate is generated using a certmanager with a letsencrypt cluster issuer then mounted onto the pod where its added to the listener. The healthcheck is a TCP based one and will cause TCP failures in the mosquitto logs. I currently expose both ports although this should ideally be made an opt in a future release of this chart. If you don't wish to use certmanager you could just provide a kubernetes secret with the certs inside of it.

## Chart Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` |  |
| fullnameOverride | string | `""` |  |
| image.pullPolicy | string | `"IfNotPresent"` |  |
| image.repository | string | `"eclipse-mosquitto"` |  |
| image.tag | string | `"1.6.9"` |  |
| imagePullSecrets | list | `[]` |  |
| mosquitto.certmanager.clusterIssuer | string | `"letsencrypt-prod"` |  |
| mosquitto.certmanager.dnsname | string | `"host.example.com"` |  |
| mosquitto.certmanager.enabled | bool | `true` |  |
| mosquitto.config | string | `Line based properties for mosquitto` |  |
| nameOverride | string | `""` |  |
| nodeSelector | object | `{}` |  |
| persistence.enabled | bool | `true` |  |
| persistence.size | string | `"1Gi"` |  |
| podSecurityContext.fsGroup | int | `1883` |  |
| replicaCount | int | `1` |  |
| resources | object | `{}` |  |
| securityContext.runAsNonRoot | bool | `true` |  |
| securityContext.runAsUser | int | `1883` |  |
| service.mqtt.port | int | `1883` |  |
| service.mqtt.tlsport | int | `8883` |  |
| service.type | string | `"ClusterIP"` |  |
| tolerations | list | `[]` |  |

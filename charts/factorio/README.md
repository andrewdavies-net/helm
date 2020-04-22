factorio
========
Factorio dedicated server.

Current chart version is `2.0.0`

Source code can be found [here](https://www.factorio.com/)

Leveraging the [Factorio Docker Image](https://github.com/factoriotools/factorio-docker)

## Install;

```shell
$ helm repo add andrewdaviesnet https://charts.andrewdavies.net
$ helm install andrewdaviesnet/factorio
```

Alternatively, a YAML file that specifies the values for the above parameters can be provided while installing the chart. For example,

```console
helm install --name my-release -f values.yaml andrewdaviesnet/factorio
```
## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```console
helm delete my-release --purge
```


## Chart Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` |  |
| factorioServer.port | int | `34197` |  |
| factorioServer.rcon.enabled | bool | `false` |  |
| factorioServer.rcon.port | int | `27015` |  |
| fullnameOverride | string | `""` |  |
| image.pullPolicy | string | `"IfNotPresent"` |  |
| image.repository | string | `"factoriotools/factorio"` |  |
| image.tag | string | `"stable"` |  |
| imagePullSecrets | list | `[]` |  |
| nameOverride | string | `""` |  |
| nodeSelector | object | `{}` |  |
| persistence.enabled | bool | `true` |  |
| persistence.size | string | `"1Gi"` |  |
| podSecurityContext | object | `{}` |  |
| replicaCount | int | `1` |  |
| resources | object | `{}` |  |
| securityContext | object | `{}` |  |
| service.type | string | `"LoadBalancer"` |  |
| tolerations | list | `[]` |  |


# Setup
Once installed you need to configure the server configuration. This helm chart is only a wrapper.


# Finding The Factorio Pod Name
To find the name of the pod running your Factorio server execute:

```
kubectl -n factorio get pods
```

# Accessing Factorio Data
The container which runs the Factorio server stores data in `/factorio`.  

The `kubectl cp` command can be used to transfer files to / from the server:

## Save File Example
Example of transferring a save file to the server:

```
kubectl cp ./factorio-save.zip factorio/<factorio pod name>:/factorio/saves/
```

## Factorio Configuration File Example
The same command can be used to tweak a Factorio server's 
`server-settings.json` file.  

First download the config file to your local computer:

```
kubectl cp factorio/<factorio pod name>:/factorio/config/server-settings.json .
```

Edit the downloaded `server-settings.json` file which is now in your current 
working directory.  

Then upload the edited config file back up to your Factorio server:

```
kubectl cp factorio/<factorio pod name>:/factorio/config/ ./server-settings.json
```

# Viewing Server Logs
To view the Factorio server's logs run the following command on your local 
computer:

```
kubectl -n factorio logs -l app=factorio --timestamps
```

# Running A Terminal On The Server
To gain more detailed insight or simply to perform maintenance tasks it may be 
useful to run a shell on the machine hosting your Factorio server.  

To do so run the following on your local computer:

```
kubectl -n factorio exec -it <factorio pod name> -- /bin/sh
```

To exit this shell hit Ctrl + d.

# Gracefully Shutting Down The Server
When Kubernetes shuts down a pod it sends the SIGTERM signal to the main 
process (In our case the Factorio server).  

The Factorio server is built to save the game and exit when it receives a 
SIGTERM signal.  

Because of this no extra steps must be taken for a Factorio server to be 
shut down gracefully.

# Thanks 
This helm chart is a combination of the following charts:
* https://github.com/helm/charts/tree/master/stable/factorio
* https://github.com/Noah-Huppert/k8s-factorio-server
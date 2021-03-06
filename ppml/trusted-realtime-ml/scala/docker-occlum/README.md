# Trusted Cluster Serving with occlum

Please pay attention to IP and path etc., they should be changed to your own server IP/path.

## How To Build

Before run the following command, please modify the pathes in the `build-docker-image.sh` file at first. Then build docker image by running this command:

```bash
./build-docker-image.sh
```

## How To Run

### Prepare the keys

PPML in analytics zoo need secured keys to enable Flink TLS, https and TLS enabled Redis, you need to prepare secure keys and keystores.

This script is under `analytics-zoo/ppml/scripts`:

```bash
../../../generate-keys.sh
```

You also need to store password you used in previous step in a secured file:

This script is also under `/analytics-zoo/ppml/scripts`:

```bash
../../../generate-password.sh used_password_when_generate_keys
```

For example:

```bash
../../../generate-password.sh 1234qwer
```

### Start Trusted Clsuter Serving with PPML Docker image

#### Local mode (Single container)

In this mode, all components, redis, Flink & http front end, are running in single container.

Before run the following command, please modify pathes in the `start-local-cluster-serving.sh` file at first. Then Start Trusted Clsuter Serving with following command:

```bash
./start-local-cluster-serving.sh
```

##### Troubleshooting
You can run the script `/opt/check-status.sh` in the docker container to check whether the components have been correctly started.
Note that this only works for local cluster serving (for now).

To test a specific component, pass one or more argument to it among the following:
"redis", "flinkjm", "flinktm", "frontend", and "serving". For example, run the following command to check the status of the Flink job manager.

```bash
docker exec -it trusted-cluster-servinglocal bash /opt/check-status.sh flinkjm
```

To test all components, you can either pass no argument or pass the "all" argument.

```bash
docker exec -it trusted-cluster-servinglocal bash /opt/check-status.sh
```
If all is well, the following results should be displayed:

```
Detecting redis status...
Redis initilization successful.
Detecting Flink job manager status...
Flink job manager initilization successful.
Detecting Flink task manager status...
Flink task manager initilization successful.
Detecting http frontend status. This may take a while.
Http frontend initilization successful.
Detecting cluster-serving-job status...
cluster-serving-job initilization successful.
```

It is suggested to run this script once after starting local cluster serving to verify that all components are up and running.

#### Distributed mode (Multi-containers/Multi-nodes)

In this mode, all components, redis, Flink & http front end, are running in different containers, some of them can be distributed to multi-nodes. 

Pre-requests:

1. Setup `no password ssh login` between all nodes.
2. Modify IP/pathes in the `environments.sh` file. 

```bash
nano environments.sh
```

##### start the distributed cluster serving

```bash
./start-distributed-cluster-serving.sh
```

##### stop the distributed cluster serving

```bash
./stop-distributed-cluster-serving.sh
```

# Frequently Asked Questions

## Docker compose volume warning

It is normal to see:
```bash
WARN[0297]
volume "open5gs_db_data" already exists but was not created for project "<deployment_name>".
volume "open5gs_db_config" already exists but was not created for project "<deployment_name>".
```

It is only a warning, telling you that the Docker volume exists.

## No connectivity to an external network

Run in the host:
```bash
sudo sysctl -w net.ipv4.conf.all.forwarding=1

sudo iptables -P FORWARD ACCEPT

# Extracted from:
# enable forwarding from Docker containers to the outside world
# https://docs.docker.com/network/bridge/#enable-forwarding-from-docker-containers-to-the-outside-world
```

## Environment variables not expanding

If you are seeing an error related to the environment variables in Docker compose not expanding, check where the `.env` file is located. To avoid errors, it is easier to specify the path using the `--env-file` command option (even though, if you are using v1 format with `docker-compose`, it is not needed).
```bash
# Example using the basic deployment with docker compose (v2)
docker compose -f compose-files/basic/docker-compose.yaml --env-file=.env up -d

# Example using the basic deployment with docker-compose (v1)
docker-compose -f compose-files/basic/docker-compose.yaml --env-file=.env up -d
```

## mongo version > 5 Docker image not working

If the Docker image for mongo is higher than the 5.0 and it is crashing with a message regarding illegal AVX instruction and your host running Docker is running on a VM, specify the CPU type of the VM as the same CPU of the host.

> source: [docker-library/mongo GitHub issue](https://github.com/docker-library/mongo/issues/485#issuecomment-1028308997)

## The Docker Compose version mess

Let's start differentiating the Compose (aka the Docker Compose executable) from the Compose files.

Compose comes in two flavors:
- Compose V1 is written in Python and invoked with `docker-compose`. This Compose is not supported since June 2023.
- Compose V2 is written in Go and invoked with `docker compose`.

Compose files come in four different flavors:
- Compose file version 1 (deprecated)
- Compose file version 2.x
- Compose file version 3.x (cross compatible between Docker Compose and Docker Swarm)
- Compose specification

>Note: The current (June 2023) and recommended versions for Compose and Compose files are `Compose V2` with the `Compose specification`.

Examples for Compose file version 2.x and Compose file version 3.x will be kept under `misc/`.

If you want to run them using Compose V2 or Compose V1:
```bash
# Run the basic deployment with docker compose (V2) and Compose file version 3.x
docker compose -f misc/basic-compose-file-v3.x/docker-compose.yaml --env-file=.env up -d

# Tear down the basic deployment with docker compose (V2) and Compose file version 3.x
docker compose -f misc/basic-compose-file-v3.x/docker-compose.yaml --env-file=.env down

# or

# Run the basic deployment with docker-compose (V1) and Compose file version 2.x
docker-compose -f misc/basic-compose-file-v2.x/docker-compose.yaml up -d

# Tear down the basic deployment with docker-compose (V1) and Compose file version 2.x
docker-compose -f misc/basic-compose-file-v2.x/docker-compose.yaml down
```
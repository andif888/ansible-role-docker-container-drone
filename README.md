# ansible-role-docker-container-drone

Role to run Drone.io in a docker container. This role runs also one drone runner.

## Table of content

- [Default Variables](#default-variables)
  - [docker_container_drone_env](#docker_container_drone_env)
  - [docker_container_drone_image](#docker_container_drone_image)
  - [docker_container_drone_labels](#docker_container_drone_labels)
  - [docker_container_drone_name](#docker_container_drone_name)
  - [docker_container_drone_networks](#docker_container_drone_networks)
  - [docker_container_drone_ports](#docker_container_drone_ports)
  - [docker_container_drone_restic_enable](#docker_container_drone_restic_enable)
  - [docker_container_drone_restic_retention](#docker_container_drone_restic_retention)
  - [docker_container_drone_restic_s3_bucket_name](#docker_container_drone_restic_s3_bucket_name)
  - [docker_container_drone_restic_s3_endpoint](#docker_container_drone_restic_s3_endpoint)
  - [docker_container_drone_restic_s3_repo](#docker_container_drone_restic_s3_repo)
  - [docker_container_drone_restic_s3_repo_access_key](#docker_container_drone_restic_s3_repo_access_key)
  - [docker_container_drone_restic_s3_repo_password](#docker_container_drone_restic_s3_repo_password)
  - [docker_container_drone_restic_s3_repo_secret_key](#docker_container_drone_restic_s3_repo_secret_key)
  - [docker_container_drone_restic_tag](#docker_container_drone_restic_tag)
  - [docker_container_drone_volume_dir](#docker_container_drone_volume_dir)
  - [docker_container_drone_volumes](#docker_container_drone_volumes)
  - [docker_container_dronerunner_env](#docker_container_dronerunner_env)
  - [docker_container_dronerunner_image](#docker_container_dronerunner_image)
  - [docker_container_dronerunner_labels](#docker_container_dronerunner_labels)
  - [docker_container_dronerunner_name](#docker_container_dronerunner_name)
  - [docker_container_dronerunner_networks](#docker_container_dronerunner_networks)
  - [docker_container_dronerunner_volume_dir](#docker_container_dronerunner_volume_dir)
  - [docker_container_dronerunner_volumes](#docker_container_dronerunner_volumes)
  - [docker_image_drone_name](#docker_image_drone_name)
  - [docker_image_drone_pull](#docker_image_drone_pull)
  - [docker_image_dronerunner_name](#docker_image_dronerunner_name)
  - [docker_image_dronerunner_pull](#docker_image_dronerunner_pull)
  - [docker_network_drone_name](#docker_network_drone_name)
  - [docker_network_dronerunner_name](#docker_network_dronerunner_name)
- [Discovered Tags](#discovered-tags)
- [Dependencies](#dependencies)
- [License](#license)
- [Author](#author)

---

## Default Variables

### docker_container_drone_env

Dictionery of key,value pairs for docker environment variables to configure drone.

#### Default value

```YAML
docker_container_drone_env:
  DRONE_GIT_ALWAYS_AUTH: 'true'
  DRONE_GITEA_SERVER: http://localhost:3000
  DRONE_RPC_SECRET: UAv3Y67LpLL3NrsbbSaV2qhHkSjoJARF
  DRONE_SERVER_PROTO: http
  DRONE_TLS_AUTOCERT: 'false'
  DRONE_AGENTS_ENABLED: 'true'
  DRONE_USER_CREATE: username:gitadmin,admin:true
  DRONE_LOGS_PRETTY: 'true'
  DRONE_LOGS_COLOR: 'true'
  DRONE_S3_ENDPOINT: '{{ docker_container_drone_s3_endpoint | default(omit) }}'
  DRONE_S3_BUCKET: '{{ docker_container_drone_s3_bucket | default(omit) }}'
  DRONE_S3_PATH_STYLE: 'true'
  AWS_ACCESS_KEY_ID: '{{ docker_container_drone_aws_access_key_id | default(omit)
    }}'
  AWS_SECRET_ACCESS_KEY: '{{ docker_container_drone_aws_secret_access_key | default(omit)
    }}'
  AWS_DEFAULT_REGION: us-east-1
  AWS_REGION: us-east-1
```

### docker_container_drone_image

Repository path and tag used to create the container. If an image is not found or pull is true, the image will be pulled from the registry. If no tag is included, latest will be used.

#### Default value

```YAML
docker_container_drone_image: '{{ docker_image_drone_name }}'
```

### docker_container_drone_labels

Dictionary of key value pairs for container labels.

Example:

```yaml

docker_container_drone_labels:

traefik.enable: "true"

```

#### Default value

```YAML
docker_container_drone_labels: {}
```

### docker_container_drone_name

Name for the container

#### Default value

```YAML
docker_container_drone_name: drone
```

### docker_container_drone_networks

List of networks the container belongs to.

#### Default value

```YAML
docker_container_drone_networks:
  - name: '{{ docker_network_drone_name }}'
```

### docker_container_drone_ports

List of ports to publish from the container to the host.

#### Default value

```YAML
docker_container_drone_ports:
  - 3001:80
```

### docker_container_drone_restic_enable

Enable restic backup for the container's mounted volumes.

#### Default value

```YAML
docker_container_drone_restic_enable: false
```

### docker_container_drone_restic_retention

Retention settions for `restic forget` after the `restic backup`.

#### Default value

```YAML
docker_container_drone_restic_retention:
  keep_last: 1
  keep_daily: 7
  keep_weekly: 4
```

### docker_container_drone_restic_s3_bucket_name

Minio S3 bucket name for restic backup storage.

#### Default value

```YAML
docker_container_drone_restic_s3_bucket_name: restic-{{ docker_container_drone_name
  }}
```

### docker_container_drone_restic_s3_endpoint

Minio S3 endpoint for restic backup storage.

Example:

```yaml

docker_container__base__restic_s3_endpoint: "https://minio.{{ dns_domain }}"

docker_container_drone_restic_s3_endpoint: "{{ docker_container__base__restic_s3_endpoint }}"

```

#### Default value

```YAML
docker_container_drone_restic_s3_endpoint: '{{ docker_container__base__restic_s3_endpoint
  }}'
```

### docker_container_drone_restic_s3_repo

Minio S3 repo URL for restic backup storage.

#### Default value

```YAML
docker_container_drone_restic_s3_repo: s3:{{ docker_container_drone_restic_s3_endpoint
  }}/{{ docker_container_drone_restic_s3_bucket_name }}
```

### docker_container_drone_restic_s3_repo_access_key

Minio S3 repo access key for restic backup storage.

#### Default value

```YAML
docker_container_drone_restic_s3_repo_access_key: '{{ docker_container__base__restic_s3_repo_access_key
  }}'
```

### docker_container_drone_restic_s3_repo_password

Minio S3 repo password for restic backup storage.

#### Default value

```YAML
docker_container_drone_restic_s3_repo_password: '{{ docker_container__base__restic_s3_repo_password
  }}'
```

### docker_container_drone_restic_s3_repo_secret_key

Minio S3 repo secret key for restic backup storage.

#### Default value

```YAML
docker_container_drone_restic_s3_repo_secret_key: '{{ docker_container__base__restic_s3_repo_secret_key
  }}'
```

### docker_container_drone_restic_tag

Tag for the `restic backup` command

#### Default value

```YAML
docker_container_drone_restic_tag: '{{ docker_container_drone_name }}'
```

### docker_container_drone_volume_dir

Volume mount host directory, where Treafik config files are stored.

#### Default value

```YAML
docker_container_drone_volume_dir: '{{ docker_container__base__volume_dir }}/{{ docker_container_drone_name
  }}'
```

### docker_container_drone_volumes

List of volumes to mount within the container.

#### Default value

```YAML
docker_container_drone_volumes:
  - '{{ docker_container_drone_volume_dir }}/var/lib/drone:/var/lib/drone'
  - '{{ docker_container_drone_volume_dir }}/data:/data'
```

### docker_container_dronerunner_env

Dictionery of key,value pairs for docker environment variables to configure drone runner.

#### Default value

```YAML
docker_container_dronerunner_env:
  DRONE_RPC_PROTO: http
  DRONE_RPC_HOST: drone-server
  DRONE_RPC_SECRET: UAv3Y67LpLL3NrsbbSaV2qhHkSjoJARF
  DRONE_RUNNER_CAPACITY: '2'
  DRONE_RUNNER_NAME: localhost
```

### docker_container_dronerunner_image

Repository path and tag used to create the container. If an image is not found or pull is true, the image will be pulled from the registry. If no tag is included, latest will be used.

#### Default value

```YAML
docker_container_dronerunner_image: '{{ docker_image_dronerunner_name }}'
```

### docker_container_dronerunner_labels

Dictionary of key value pairs for container labels.

Example:

```yaml

docker_container_dronerunner_labels:

traefik.enable: "true"

```

#### Default value

```YAML
docker_container_dronerunner_labels: {}
```

### docker_container_dronerunner_name

Name for the container

#### Default value

```YAML
docker_container_dronerunner_name: dronerunner
```

### docker_container_dronerunner_networks

List of networks the container belongs to.

#### Default value

```YAML
docker_container_dronerunner_networks:
  - name: '{{ docker_network_drone_name }}'
    links:
      - '{{ docker_container_drone_name }}:drone-server'
```

### docker_container_dronerunner_volume_dir

Volume mount host directory, where Treafik config files are stored.

#### Default value

```YAML
docker_container_dronerunner_volume_dir: '{{ docker_container__base__volume_dir }}/{{
  docker_container_drone_name }}'
```

### docker_container_dronerunner_volumes

List of volumes to mount within the container.

#### Default value

```YAML
docker_container_dronerunner_volumes:
  - /var/run/docker.sock:/var/run/docker.sock
```

### docker_image_drone_name

Repository path and tag for the container image.

#### Default value

```YAML
docker_image_drone_name: drone/drone:2
```

### docker_image_drone_pull

Indicate to always pull the docker image.

#### Default value

```YAML
docker_image_drone_pull: no
```

### docker_image_dronerunner_name

Repository path and tag for the container image.

#### Default value

```YAML
docker_image_dronerunner_name: drone/drone-runner-docker:1
```

### docker_image_dronerunner_pull

Indicate to always pull the docker image.

#### Default value

```YAML
docker_image_dronerunner_pull: no
```

### docker_network_drone_name

Name of the docker network created for drone.

#### Default value

```YAML
docker_network_drone_name: '{{ docker_container_drone_name }}_backend'
```

### docker_network_dronerunner_name

Name of the docker network created for drone runner.

#### Default value

```YAML
docker_network_dronerunner_name: '{{ docker_container_drone_name }}_backend'
```

## Discovered Tags

**_docker-container-backup-all_**\
&emsp;Backup all containers' volume mounts.

**_docker-container-backup-drone_**\
&emsp;Backup drone volume mounts.

**_docker-container-backup-init-all_**\
&emsp;Run init backup task for all container.

**_docker-container-backup-init-drone_**\
&emsp;Run init backup task for drone if restic is enabled.

**_docker-container-backup-list-all_**\
&emsp;List all containers' backups.

**_docker-container-backup-list-drone_**\
&emsp;List drone backups.

**_docker-container-prereq-all_**\
&emsp;Ensure all pre-requisites are installed

**_docker-container-prereq-drone_**\
&emsp;Ensure all pre-requisites for drone are installed

**_docker-container-purge-all_**\
&emsp;Remove all containers and delete volume mounts.

**_docker-container-purge-drone_**\
&emsp;Remove drone and delete volume mounts.

**_docker-container-remove-all_**\
&emsp;Remove all containers.

**_docker-container-remove-drone_**\
&emsp;Remove drone.

**_docker-container-restore-all_**\
&emsp;Run restic restore for all restic enabled containers.

**_docker-container-restore-drone_**\
&emsp;Run restic restore for drone if restic is enabled.

**_docker-container-setup-all_**\
&emsp;Run setup task for all containers.

**_docker-container-setup-drone_**\
&emsp;Run setup task for drone.

**_never_**


## Dependencies

None.

## License

license (GPL-2.0-or-later, MIT, etc)

## Author

andif888

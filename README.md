# kairos-distros

[Kairos](https://github.com/kairos-io/kairos) container images for running
in my [Kubernetes cluster](https://github.com/allenporter/k8s-gitops) based on
[tyzbit's kairos-distros](https://github.com/tyzbit/kairos-distros)

## Building a container

```bash
$ IMAGE_NAME=kairos-ubuntu-intel-nuc:test
$ docker build -t ${IMAGE_NAME} ./ubuntu-intel-nuc
```

## Building an iso

Note: This can't connect to the local docker instance when run from in docker.

```bash
$ IMAGE_NAME=kairos-ubuntu-intel-nuc:test
$ docker run -v "${PWD}/build:/tmp/auroraboot" \
    --rm -ti quay.io/kairos/auroraboot:v0.2.7 \
    --set "container_image=docker://${IMAGE_NAME}" \
    --set "disable_http_server=true" \
    --set "disable_netboot=true" \
    --set "state_dir=/tmp/auroraboot" \
    --debug
```

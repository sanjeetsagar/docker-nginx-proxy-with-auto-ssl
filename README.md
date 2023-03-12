# Nginx-proxy + Acme-companion
A repository for starting nginx-proxy and acme-companion with Docker Compose.
Adding a container to the `nginx-proxy` network automatically builds a reverse proxy for that container and supports SSL/TLS.

It is not used by itself, but is basically used in conjunction with other containers.

## Required operating environment
- Docker `>= 19.03.0`
- Docker Compose `>= 1.27.0`

Reference: [Compose file - Docker documentation](https://matsuand.github.io/docs.docker.jp.onthefly/compose/compose-file/)

## How to use
As a basic flow, first start Docker Compose in this repository, then start the container that is the original connection destination and add it to the `nginx-proxy` network.

### Starting nginx-proxy and acme-companion
``` bash
docker-compose up -d
```

### Starting a container targeted by a reverse proxy
If you include the following in the environment variables of the target container of the reverse proxy, the reverse proxy will be built automatically.
There are various other settings, but here are the minimum required items, so please refer to the reference for details.

- `VIRTUAL_HOST`: hostname corresponding to this container
- `LETSENCRYPT_HOST`: Host name for SSL/TLS (basically same as `VIRTUAL_HOST`)
- `LETSENCRYPT_EMAIL`: Email address to be contacted when SSL/TLS certificate is about to expire

#### WordPress example
See [sanjeetsagar/docker-compose-wordpress](https://github.com/sanjeetsagar/docker-compose-wordpress).
A WordPress repository to used with this repository.

## Reference
### Docker Hub
[nginxproxy/nginx-proxy](https://hub.docker.com/r/nginxproxy/nginx-proxy)
[nginxproxy/acme-companion](https://hub.docker.com/r/nginxproxy/acme-companion)
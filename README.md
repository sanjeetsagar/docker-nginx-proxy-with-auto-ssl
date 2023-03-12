# Nginx-proxy + Acme-companion
A repository for starting nginx-proxy and acme-companion with Docker Compose.
Adding a container to the `nginx-proxy` network automatically builds a reverse proxy for that container and supports SSL/TLS.

It is not used by itself, but is basically used in conjunction with other containers.

## Required operating environment
- Docker `>= 19.03.0`
- Docker Compose `>= 1.27.0`

Reference: [Compose file - Docker documentation](https://matsuand.github.io/docs.docker.jp.onthefly/compose/compose-file/)

## How to use
- Clone project
- Create global nginx-proxy network: `docker network create nginx-proxy`
- Build container using `docker-compose up -d`


### Documentation:

- In `conf/custom.conf` you can add any custom configuration for nginx.


### Starting a container targeted by a reverse proxy
If you include the following in the environment variables of the target container of the reverse proxy, the reverse proxy will be built automatically.
There are various other settings, but here are the minimum required items, so please refer to the reference for details.

- `VIRTUAL_HOST`: hostname corresponding to this container
- `LETSENCRYPT_HOST`: Host name for SSL/TLS (basically same as `VIRTUAL_HOST`)
- `LETSENCRYPT_EMAIL`: Email address to be contacted when SSL/TLS certificate is about to expire

Example on how to use on containers:

    services:
        web:
            ...
            ports:
                - 80
            ...
            environment:
                VIRTUAL_HOST: example.com
                VIRTUAL_PORT: 80
                LETSENCRYPT_HOST: example.com
                LETSENCRYPT_EMAIL: mail@example.com
            ...
            networks:
                ...
                - nginx-proxy

You can use multiple domains/subdomains:

    services:
        web:
            ...
            build:
                context: ./
                dockerfile: web/Dockerfile
            ports:
                - 80
            ...
            environment:
                VIRTUAL_HOST: example.com,sub.example.com,example2.com
                VIRTUAL_PORT: 80
                LETSENCRYPT_HOST: example.com,sub.example.com,example2.com
                LETSENCRYPT_EMAIL: mail@example.com
            ...
            networks:
                ...
                - nginx-proxy

- In `web/Dockerfile` you can include a conf where you define your servers, wildcards are not yet supported by acme companion.
- When using in local environment the ssl certificates won't be created and a fallback to http will be created automatically.


#### WordPress example
See [sanjeetsagar/docker-compose-wordpress](https://github.com/sanjeetsagar/docker-compose-wordpress).
A WordPress repository to used with this repository.

## Reference
### Docker Hub
- [nginxproxy/nginx-proxy](https://hub.docker.com/r/nginxproxy/nginx-proxy)

- [nginxproxy/acme-companion](https://hub.docker.com/r/nginxproxy/acme-companion)

_Happy coding!_
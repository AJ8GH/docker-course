# Notes

## build

build a docker image

* `-t` : tag image, optionally with version - `<tag>:<version>`
* `--build-arg` : pass arg to build, to be accessed in dockerfile


## pull

pull an image from a repository to run later

## push

## run

run a container from an image

* `--rm` : remove container once stopped
* `--name` : name container
* `-d` : run in detached mode
* `-p` : publish port `<port-to-publish>:<container-port-to-map-to>`
* `--env` : pass env var to container, to be accessed in dockerfile or in the container / application code. can also be set with ENV in dockerfile
* `--env-file` : specify .env file e.g. `--env-file ./.env`
* `--networks` : run container as part of a network
* `-v` : attach a volume


## volumes

attach volumes when running containers for data persistence

anonymous volume:
* `-v /path/in/container`
* removed when container is removed
* can be used to overwrite parts of a bind mount

named volume:
* `-v <name>:/path/in/container`
* kept when container is removed
* persist data long term on host machine

bind mount
* `-v <absolute-path-to-dir-on-local-machine>:/path/in/container`
* binds a local dir to a container dir
* changes made in local dir are realised in container, without having to rebuild image
* for development / testing, not production

read only:
* `-v <name>/path:ro`
* ensures volume not writeable by container


## Networking

* outbound traffic - enabled by default
* localhost - replace local host with `host.docker.internal`
* container to container -
  * sol 1: docker inspect target container to get ip, use ip to make requests
  * sol 2: use docker networks with `--networks`
    * create with `docker network create <name>`
    * run all containers inside network
    * use container name as host for any urls/requests

## Compose

Commands:
* `docker-compose up` : run a docker compose file
* `docker-compose down` : tear down a docker compose environment + containers
* `docker-compose run <service> <command>` : run service from compose file and exec the command

file name: `docker-compose.yaml`

File Structure
* `version:` string version number of docker compose
* `services:` services to run, keyed by name
  * `image:` image to use
  * `build:` instructions for image build. Can be just file path to build dir, or more complex using other properties
  * `ports:` ports to expose
  * `volumes:` volumes to create / attach
  * `env_file:` env file to use
  * `depends_on:`: dependencies on other services
  * `stdin_open:` open stdin - boolean
  * `tty:` interactive shell - boolean
* `volumes`:
  * list of named volumes - must include them all, with name followed by colon, e.g.
```yaml
volumes:
  data:
  logs-backend:
  logs-frontend:
```

## running commands

* run a container with a specific environment to exec your own commands
* ways to run commands:
  * `docker run -it <image> <command>`
  * `docker run -it -d <image> && docker exec -it <container> <command>`
  * `docker-compose run <service> <command>` : run service from compose file and exec the command
* run container with bind mount and exec commands to create / edit files on host machine:
  * `docker run -it -v <bind-mount:path-mapping> <image> <command>`
* `ENTRYPOINT` in docker file - specify an executable to run all commands with - restricts commands that can be run, by prepending the executable

### Entry point vs CMD
cmd is overwritten by docker run commands, entry point is prepended to docker run commands

## Deployments

* Don't use bind-mounts in production


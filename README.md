# docker-autocompose

Generates a docker-compose yaml definition from a docker container.

For building this project [uv](https://astral.sh/uv) is required.

Required Modules:

* [pyaml](https://pypi.python.org/project/pyaml/)
* [docker](https://pypi.python.org/project/docker)

Install them:

    uv install

Example Usage:

    uv run autocompose <container ids>

Generate a compose file for multiple containers together:

    uv run autocompose apache-test mysql-test

The script defaults to outputting to compose file version 3, but use "-v 1" to output to version 1:

    uv run autocompose -v 1 apache-test

Outputs a docker-compose compatible yaml structure:

[docker-compose reference](https://docs.docker.com/compose/)

[docker-compose yaml file specification](https://docs.docker.com/compose/compose-file/)

While experimenting with various docker containers from the Hub, I realized that I'd started several containers with complex options for volumes, ports, environment variables, etc. and there was no way I could remember all those commands without referencing the Hub page for each image if I needed to delete and re-create the container (for updates, or if something broke).

With this tool, I can easily generate docker-compose files for managing the containers that I've set up manually.

## Native installation

System-wide installation is discouraged. If you really need to, you can run `pip install --user --break-system-packages .` (use at your own discretion).

There are unofficial packages available in the Arch User Repository:

* [Stable](https://aur.archlinux.org/packages/docker-autocompose)
* [Development (follows the master branch)](https://aur.archlinux.org/packages/docker-autocompose-git)

**AUR packages are provided by a third party and are not tested or updated by the maintainer(s) of the docker-autocompose project.**

## Docker Usage

You can use this tool from a docker container by either cloning this repo and building the image or using the [automatically generated image on GitHub](https://github.com/Red5d/docker-autocompose/pkgs/container/docker-autocompose)

Pull the image from GitHub (supports both x86 and ARM)

    docker pull ghcr.io/red5d/docker-autocompose:latest

Use the new image to generate a docker-compose file from a running container or a list of space-separated container names or ids:

    docker run --rm -v /var/run/docker.sock:/var/run/docker.sock ghcr.io/red5d/docker-autocompose <container-name-or-id> <additional-names-or-ids>...

To print out all containers in a docker-compose format:

    docker run --rm -v /var/run/docker.sock:/var/run/docker.sock ghcr.io/red5d/docker-autocompose $(docker ps -aq)
    
## Contributing

When making changes, please validate the output from the script by writing it to a file (docker-compose.yml or docker-compose.yaml) and running "docker-compose config" in the same folder with it to ensure that the resulting compose file will be accepted by docker-compose.

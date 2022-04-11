# Jupyter Lap Container

## Requirements

  * Docker Engine 19.03.0+


## Usage

Standard build with the default settings:

> docker-compose build

The following build arguments do exist:

  * `PYTHON_VERSION`, e.g. `3.9.7` (default) - any version from https://hub.docker.com/r/jupyter/minimal-notebook/tags?page=1&name=python
  * `REQUIREMENTS`. e.g. `tu-nlp4web` (default) - selects with `pip` packages are installed. Possible values: `tu-nlp4web`

For example, if you want to have a build with `REQUIREMENTS` `tu-nlp4web`:

> docker-compose build --build-arg REQUIREMENTS=tu-nlp4web

Finally, start the setup with

> docker-compose up

Jupyter Lab should be now reachable under http://127.0.0.1:8080/lab.
You should also be able to connect an IDE like Visual Studio Code to this container to use Jupyter Notebooks inside your IDE.

To shut down everything:

> docker-compose down

Removal of the containers (probably a good idea before you rebuild with different settings):

> docker-compose rm

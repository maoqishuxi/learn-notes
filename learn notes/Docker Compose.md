# Docker Compose

## Overview(概述)

Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application's services. Then, with a single command, you create and start all the services from your configuration.

Using Compose is basically a three-step process:

1. Define your app's environment with a `Dockerfile` so it can be reproduced anywhere.
2. Define the services that make up your app in `docker-compose.yml` so they can be run together in an isolated environment.
3. Run `docker compose up` and the [Docker compose command](https://docs.docker.com/compose/#compose-v2-and-the-new-docker-compose-command) starts and runs your entire app. You can alternatively run `docker-compose up` using Compose standalone(`docker-compose` binary).

Example A `docker-compose.yml` look like this:

```
version: "3.9"  # optional since v1.27.0
services:
  web:
    build: .
    ports:
      - "8000:5000"
    volumes:
      - .:/code
      - logvolume01:/var/log
    depends_on:
      - redis
  redis:
    image: redis
volumes:
  logvolume01: {}
```

Compose has commands for managing the whole lifecycle of your application:

- Start, stop, and rebuild service
- View the status of running services
- Stream the log output of running services
- Run a one-off command on a service

### Features

The features of Compose that make it effective are:

- Multiple isolated environment on a single host
- Preserve volume data when containers are created
- Only recreate containers that have changed
- Variables and moving a composition between environments

#### Multiple isolated environments on a single host

Compose uses a project name to isolate environments from each other. You can make use of this project name in several different contexts:

- on a dev host, to create multiple copies of a single environment, such as when you want to run a stable copy for each feature branch of a project
- on a CI server, to keep builds from interfering with each other, you can set the project name a unique build number
- on a shared host or dev host, to prevent different projects, which may use the same service names, from interfering with each other

The default project name is the `basename` of the project directory. You can set a custom project name by using the `-p` command line option or the `COMPOSE_PROJECT_NAME` [environment variable](https://docs.docker.com/compose/reference/envvars/#compose_project_name).(更改项目名称)

The default project directory is the base directory of the Compose file. A custom value  for it can be defined with the `--project-directory` command line option. (更改项目路径)

#### Preserve volume data when containers are created

Compose preserves all volumes used by your services. When `docker compose up` runs, if it finds any containers from previous runs, it copies the volumes from the old container to the new container. This process ensures that any data you've created in volumes isn't lost. (保存volume数据)

#### Only recreate containers that have changed

Compose caches the configuration used to create a container. When you restart a service that has not changed, Compose re-uses the existing containers. Re-using containers means that you can make changes to your environment very quickly. (仅重新创建更改过的容器)

#### Variables and moving a composition between environments

Compose supports variables in the Compose file. You can use these variables to customize your composition for different environment, or different users.

### Development environments

When you're developing software, the ability to run an application in an isolated environment and interact with it is crucial. The Compose command line tool can be used to create the environment and interact with it.

## Install on Linux

### Install Compose

To install Compose:

- Option 1: Set up Docker's repository on your Linux system.
- Option 2: Install Compose manually.

#### Install using the repository

If you have already set up the Docker repository, jump to step 2.

1. Set up the repository. Find distro-specific instructions in:
   [Ubuntu](https://docs.docker.com/engine/install/ubuntu/#set-up-the-repository) | [Debian](https://docs.docker.com/engine/install/debian/#set-up-the-repository)

2. Update the package index, and install the *latest version* of Docker Compose:

   - Ubuntu, Debian:
     ```
      sudo apt-get update
      sudo apt-get install docker-compose-plugin
     ```

3. Verify that Docker Compose is installed correctly by checking the version.
   ```
   docker compose version
   ```

#### Update Compose

- Ubuntu, Debian:
  ```
   sudo apt-get update
   sudo apt-get install docker-compose-plugin
  ```

### Uninstall Docker Compose

#### Uninstalling the Docker Compose CLI plugin

Ubuntu, Debian:

```
 sudo apt-get remove docker-compose-plugin
```

#### Remove for all users

Or, if you have installed Compose for all users, run:

```
 rm /usr/local/lib/docker/cli-plugins/docker-compose
```

## Get started with Docker Compose

### [Step 1: Setup](https://docs.docker.com/compose/gettingstarted/#step-1-setup)

### Step 2: Create a `Dockerfile`

In this step, you write a `Dockerfile` that builds a Docker image. The image contains all the dependencies the Python application requires, including Python itself.

```
# syntax=docker/dockerfile:1
FROM python:3.7-alpine
WORKDIR /code
ENV FLASK_APP=app.py
ENV FLASK_RUN_HOST=0.0.0.0
RUN apk add --no-cache gcc musl-dev linux-headers
COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt
EXPOSE 5000
COPY . .
CMD ["flask", "run"]
```

###  Step 3: Define services in a Compose file

Create a file called `docker-compose.yml` in your project directory and paste the following:

```
version: "3.9"
services:
  web:
    build: .
    ports:
      - "8000:5000"
  redis:
    image: "redis:alpine"
```

This Compose file defines two services: `web` and `redis` .

#### Web service

The `web` service uses an image that's built from the `Dockerfile` in the current directory. It then binds the container and the host machine to the exposed port, `8000` . This example service uses the default port for the Flask web server, `5000` .

#### Redis service

The `redis` service uses a public `Redis` image pulled from the Docker Hub registry.

### Step 4: Build and run your app with Compose

start up your application by running `docker compose up` .

### Step 5: Edit the Compose file to add a bind mount

Edit `docker-compose.yml` in your project directory to add a bind mount for the `web` service:

```
version: "3.9"
services:
  web:
    build: .
    ports:
      - "8000:5000"
    volumes:
      - .:/code
    environment:
      FLASK_DEBUG: True
  redis:
    image: "redis:alpine"
```

The new `volumes` key mounts the project directory (current directory) on the host to `/code` inside the container, allowing you to modify the code on the fly, without having to rebuild the image. The `environment` key sets the `FLASK_ENV` environment variable, which tells `flask run` to run in development mode and reload the code on change. This mode should only be used in development.

### Step 6: Re-build and run the app with Compose

type `docker compose up` to build the app with the updated Compose file, and run it.

### Step 8: Experiment with some other commands

If you want to run your services in the background, you can pass the `-d` flag (for "detached" mode) to `docker compose up` and use `docker compose ps` to see what is currently running.

The `docker compose run` command allows you to run one-off commands(一次性命令) for your services. For example, to see what environment variables are available to the `web` service: (查看service环境变量)

```
docker compose run web env
```

See `docker compose --help` to see other available commands.

If you started Compose with `docker compose up -d` , stop your services once you've finished with them:

```
docker compose stop
```

You can bring everything down, removing the containers entirely, with the `down` command. Pass `--volumes` to also remove the data volume used by the Redis container:

```
docker compose down --volumes
```

## Environment variables in Compose

### Substitute environment variables in Compose files

It's possible to use environment variables in your shell to populate values inside a Compose file:

```
web:
  image: "webapp:${TAG}"
```

If you have multiple environment variables, you can substitute them by adding them to a default environment variable file named `.env` or by providing a path to your environment variables files using the `--env-file` command line option.

Your configuration options can contain environment variables. Compose uses the variable values from the shell environment in which `docker-compose` is run. For example, suppose the shell contains `POSTGRES_VERSION=9.3` and you supply this configuration:

```
db:
  image: "postgres:${POSTGRES_VERSION}"
```

if an environment variable is not set, Compose substitutes with an empty string. 

You can set default values for environment variables using a `.env` file, which Compose automatically looks for in project directory  (parent folder of your Compose file). (Compose 会自动寻找.env文件) Values set in the shell environment override those set in the `.env` file.

```
tips
The "".env file" feature only works when you use the "docker-compose up" command and does not work with "docker stack deploy".
```

Both `$VARIABLE` and `${VARIABLE}` syntax are supported. Additionally when using the [2.1 file format](https://docs.docker.com/compose/compose-file/compose-versioning/#version-21) , it is possible to provide inline default values using typical shell syntax:

- `${VARIABLE:-default}` evaluates to `default` if `VARIABLE` is unset or empty in the environment.
- `${VARIABLE-default}` evaluates to `default` only if `VARIABLE` is unset in the environment.

Similarly, the following syntax allows you to specify mandatory variables:

- `${VARIABLE:?err}` exits with an error message containing `err` if `VARIABLE` is unset or empty in the environment.
- `${VARIABLE?err}` exits with an error message containing `err` if `VARIABLE` is unset in the environment.

You can use a `$$` (double-dollar sign) when your configuration needs a literal dollar sign.

```
web:
  build: .
  command: "$$VAR_NOT_INTERPOLATED_BY_COMPOSE"
```

### Using the “--env-file” option

By passing the file as an argument, you can store it anywhere and name it appropriately, for example, `.env.ci`, `.env.dev`, `.env.prod`. Passing the file path is done using the `--env-file` option:

```
$ docker compose --env-file ./config/.env.dev up
```

### The “env_file” configuration option

You can pass multiple environment variables from an external file through to a service’s containers with the [‘env_file’ option](https://docs.docker.com/compose/compose-file/compose-file-v3/#env_file), just like with `docker run --env-file=FILE ...`:

```
web:
  env_file:
    - web-variables.env
```

### Set environment variables with ‘docker compose run’

Similar to `docker run -e`, you can set environment variables on a one-off container with `docker compose run -e`:

```
$ docker compose run -e DEBUG=1 web python console.py
```

You can also pass a variable from the shell by not giving it a value:

```
$ docker compose run -e DEBUG web python console.py
```

The value of the `DEBUG` variable in the container is taken from the value for the same variable in the shell in which Compose is run.

## Using profiles with Compose

### Assigning profiles to services

Services are associated with profiles through the `profiles` attribute which takes an array of profile names:

```
version: "3.9"
services:
  frontend:
    image: frontend
    profiles: ["frontend"]

  phpmyadmin:
    image: phpmyadmin
    depends_on:
      - db
    profiles:
      - debug

  backend:
    image: backend

  db:
    image: mysql
```

Here the services `frontend` and `phpmyadmin` are assigned to the profiles `frontend` and `debug` respectively and as such are only started when their respective profiles are enabled.

Services without a `profiles` attribute will *always* be enabled, i.e. in this case running `docker compose up` would only start `backend` and `db` .

> Note
>
> The core services of your application should not be assigned `profiles` so they will always be enabled and automatically started.

###  Enabling profiles

To enable a profile supply the `--profile` command-line option or use the `COMPOSE_PROFILES` environment variable:

```
$ docker compose --profile debug up
$ COMPOSE_PROFILES=debug docker compose up
```

Multiple profiles can be specified by passing multiple `--profile` flags or a comma-separated list for the `COMPOSE_PROFILES` environment variable:

```
$ docker compose --profile debug up
$ COMPOSE_PROFILES=debug docker compose up
```

### Auto-enabling profiles and dependency resolution

When a service with assigned `profiles` is explicitly targeted on the command line its profiles will be enabled automatically so you don’t need to enable them manually. This can be used for one-off services and debugging tools. As an example consider this configuration:

```
version: "3.9"
services:
  backend:
    image: backend

  db:
    image: mysql

  db-migrations:
    image: backend
    command: myapp migrate
    depends_on:
      - db
    profiles:
      - tools
```

```
# will only start backend and db
$ docker compose up -d

# this will run db-migrations (and - if necessary - start db)
# by implicitly enabling profile `tools`
$ docker compose run db-migrations
```

But keep in mind that `docker compose` will only automatically enable the profiles of the services on the command line and not of any dependencies. This means that all services the targeted service `depends_on` must have a common profile with it, be always enabled (by omitting `profiles`) or have a matching profile enabled explicitly:

```
version: "3.9"
services:
  web:
    image: web

  mock-backend:
    image: backend
    profiles: ["dev"]
    depends_on:
      - db

  db:
    image: mysql
    profiles: ["dev"]

  phpmyadmin:
    image: phpmyadmin
    profiles: ["debug"]
    depends_on:
      - db
```

```
# will only start "web"
$ docker compose up -d

# this will start mock-backend (and - if necessary - db)
# by implicitly enabling profile `dev`
$ docker compose up -d mock-backend

# this will fail because profile "dev" is disabled
$ docker compose up phpmyadmin
```

Although targeting `phpmyadmin` will automatically enable its profiles - i.e. `debug` - it will not automatically enable the profile(s) required by `db` - i.e. `dev`. To fix this you either have to add the `debug` profile to the `db` service:

```
db:
  image: mysql
  profiles: ["debug", "dev"]
```

or enable a profile of `db` explicitly:

```
# profile "debug" is enabled automatically by targeting phpmyadmin
$ docker compose --profile dev up phpmyadmin
$ COMPOSE_PROFILES=dev docker compose up phpmyadmin
```

## Networking in Compose

By default Compose sets up a single network for your app. Each container for a service joins the default network and is both *reachable* by other containers on that network, and *discoverable* by them at a hostname identical to the container name.

> Note
>
> Your app's network is given a name based on the "project name", which is based on the name of the directory it live in.

For example, suppose your app is in a directory called `myapp`, and your `docker-compose.yml` looks like this:

```
version: "3.9"
services:
  web:
    build: .
    ports:
      - "8000:8000"
  db:
    image: postgres
    ports:
      - "8001:5432"
```

When you run `docker compose up`, the following happens:

1. A network called `myapp_default` is created.
2. A container is created using `web`’s configuration. It joins the network `myapp_default` under the name `web`.
3. A container is created using `db`’s configuration. It joins the network `myapp_default` under the name `db`.
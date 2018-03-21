# [example_amber_hello](https://github.com/drhuffman12/example_amber_hello)

This project is powered by [Amber Framework](https://amberframework.org/).

## Installation

1. [Install required dependencies](https://github.com/amberframework/online-docs/blob/master/getting-started/quickstart/zero-to-deploy.md#install-crystal-and-amber)
2. Run `shards install`

## Usage

To setup your database edit `database_url` inside `config/environments/development.yml` file.

To edit your production settings use `amber encrypt`. [See encrypt command guide](https://github.com/amberframework/online-docs/blob/master/getting-started/cli/encrypt.md#encrypt-command)

To run amber server in a **development** enviroment:

```
amber db create migrate
amber watch
```

To build and run a **production** release:

1. Add an environment variable `AMBER_ENV` with a value of `production`
2. Run these commands (Note using `--release` is optional and may take a long time):

```
npm run release
amber db create migrate
shards build --production --release
./bin/example_amber_hello
```

## Docker Compose

To set up the database and launch the server:

```
docker-compose up -d
```

To view the logs:

```
docker-compose logs -f
```

> **Note:** The Docker images are compatible with Heroku.

## Contributing

1. Fork it ( https://github.com/drhuffman12/example_amber_hello/fork )
2. Create your feature branch (git checkout -b my-new-feature)
3. Commit your changes (git commit -am 'Add some feature')
4. Push to the branch (git push origin my-new-feature)
5. Create a new Pull Request

## Contributors

- [drhuffman12](https://github.com/drhuffman12) drhuffman12 - creator, maintainer

----

# Prior notes


## Installation Dependancies
----

For Docker-compose, see:

* https://docs.docker.com/compose/install/#install-compose

For Amber, see:

* https://github.com/amberframework/amber

### Example local setup

```sh
sudo apt-get install build-essential libreadline-dev libsqlite3-dev libpq-dev libmysqlclient-dev libssl-dev

# pip uninstall docker-py; pip uninstall docker; pip install docker
sudo curl -L https://github.com/docker/compose/releases/download/1.17.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version # => docker-compose version 1.17.1, build 6d101fb

cd $LOCAL_DEV_FOLDER_ROOT
mkdir amberframework
cd amberframework

git clone git@github.com:amberframework/amber.git
cd amber/

# Crystal version matching what is noted in ![.shard.yml](.shard.yml)
#  e.g.: one of the below echo commands
echo "0.23.1" > .crystal-version # ... if you have 0.23.1 installed via crenv
echo "system" > .crystal-version # and have Crystal v 0.23.1 installed outside crenv

shards install
make install

```

## Initial creation of your Amber-scaffolded application
----

```sh
cd $LOCAL_DEV_FOLDER_ROOT
cd $MY_PROJECTS_SUB_ROOT

amber new example_app_amber -d pg -t slang --deps
cd example_app_amber

# Crystal version matching what is noted in ![.shard.yml](.shard.yml)
#  e.g.: one of the below echo commands
echo "0.23.1" > .crystal-version # ... if you have 0.23.1 installed via crenv
echo "system" > .crystal-version # and have Crystal v 0.23.1 installed outside crenv

shards install
```

## Dockerized
----

### Build Docker images

```sh
docker-compose build

docker-compose

```

### (As needed) Cleanup ALL docker images and containers

**WARNING, this is for "ALL"**

You might have to run these more than once until each command says something like `"docker <cmd>" requires at least 1 argument(s).`

#### Stop ALL containers

```sh
docker kill $(docker ps -q)
```

#### Remove all containers

```sh
docker rm $(docker ps -a -q)
```

#### Remove all docker images

```sh
docker rmi -f $(docker images -q)
```

#### (or all together)

```sh
docker kill $(docker ps -q)
docker rm $(docker ps -a -q)
docker rmi -f $(docker images -q)
```

* View current services:

```sh
docker-compose config --services
```

### Launch the Docker containers

#### If you want to watch the STDIO of the docker containers:

In a separate terminal, type:

```sh
docker-compose up
```

* CTRL-c at any time should shut down the containers.

#### If you don't care to watch the STDIO of the docker containers:

In a terminal, type (to background it):

```sh
docker-compose up -d
```

* To shutdown the docker containers, type:

```sh
docker-compose stop app
```

### Verify that your docker containers are up and running

To view the running containers, type:

```sh
$ docker-compose ps
```

This should show something like:

```sh
           Name                          Command               State            Ports         
----------------------------------------------------------------------------------------------
exampleamberhello_app_1       amber watch                      Up       0.0.0.0:3000->3000/tcp
exampleamberhello_db_1        docker-entrypoint.sh postgres    Up       5432/tcp              
exampleamberhello_migrate_1   bash -c while ! nc -q 1 db ...   Exit 1                         
```

### Re-take ownership of the Docker-generated files

```sh
sudo chown -R ${USER}:${USER} .
# ... then type your pw when prompted
```



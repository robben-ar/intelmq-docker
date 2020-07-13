
# Intelmq in Docker: This repo is for instance imtelmq using docker.

## We split the project in 2 parts:

### Part 1: The intelmq itself

* Intelmq source code: we clone the source code frome https://github.com/certtools/intelmq and use it in our image.

### Part 2: The bots and configuration

* This image is building using another repo: https://github.com/CERTUNLP/intelmq-bots wich contains default intelmq configuration for bots.
* There are default intelmq bots there, we made this new repo to make our life easier when we develop our own bots. You can fork this repo to build yours.


# Requirements

- Docker
- Docker-compose

# Installation

```
git clone https://github.com/CERTUNLP/intelmq-docker
cd intelmq-docker
docker-compose build
docker-compose up

```
# Use it:

http://localhost:8081

# Setting your own bots.

In docker-compose is and ENV setting that tell intelmq where he need to go to find his config and bots, in the example is:

	REPO_UPDATE: https://github.com/CERTUNLP/intelmq-bots.git

But you can use yours or get this option off to prevent intelmq to update bots.

First time you run the docker-compose: bots and conf are going to be downloaded from the REPO_UPDATE, next time you boot the image only bots are going to be updated if REPO_UPDATE is in docker.compose.yml.  

# Develop mode


First clone your intelmq-bots (or default repo):

```
REPOBOTS=https://github.com/CERTUNLP/intelmq-bots.git
git clone $REPOBOTS volumes/intelmq-bots

```

Once you clone the repository just start it:

```
docker-compose -f docker-compose-dev.yml up
```

And work with your src files in volumes/intelmq-bots/

Note: there is a enviroment variable yo need to set if you want the docker to container to install wathever is in yours bot repository

```
    DEV: "true"
```

To test your changes you just need to reload the running bots in intelmqmanager with:

`docker exec -it intelmq-docker_intelmq_1 update`


# Using it with elasticsearch and grafana

There is another docker-compose-eg.yml wich give you ElasticSearch and Grafana, wich you can use to build another chain of output.

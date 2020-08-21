
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

# Command update

 * Inside the running container you could use **update** when you want to install new bots, upgrade exinting or install dependencies for yours bots. It uses source code from container filesystem folder /intelmq-bots/bots/* wich is mounted by our docker-compose.yml from volumes/intelmq-bots/ (yours repo).
After your changes are applied intelmq-manager is reloaded.

# Other docker environment variables

* REPO_UPDATE: https://github.com/CERTUNLP/intelmq-bots.git if it is defined every time the container is rebooted it will be cloned

* DEV: "true" when it is configured call update command when the container start and dismiss REPO_UPDATE configuration, so the repo is never cloned by the container entrypoint.

* We introduce a change, so you could make intelmq to send you the errors by email instead of only log it in file

```
LOG_MAIL_ENABLED: "true"
LOG_MAIL_LEVEL: "logging.ERROR"
LOG_MAIL_MAILHOST: "mail.example.unlp.edu.ar"
LOG_MAIL_PORT: 25
LOG_MAIL_FROMADDR: "intelmq@examplefeeds.unlp.edu.ar"
LOG_MAIL_TOADDR: "support@example.unlp.edu.ar"
LOG_MAIL_SUBJECT: "[INTELMQ] Application Error"
LOG_MAIL_CREDENTIALS: None #tuple (username, password)
LOG_MAIL_SECURE: None
```

* Another thing, you could make your bots to be running when container startup, just setting ENABLE_BOTNET_AT_BOOT: "true"



# Using it with elasticsearch and grafana

There is another docker-compose-eg.yml wich give you ElasticSearch and Grafana, wich you can use to build another chain of output.

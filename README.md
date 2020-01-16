Docker for Intelmq in develop mode.

# Requirements

- Docker
- Docker-compose

# Installation

```
git clone https://github.com/CERTUNLP/intelmq-docker
cd intelmq-docker/intelmq
git clone https://github.com/CERTUNLP/intelmq-bots
cd ..
docker-compose build
docker-compose up
```

http://localhost:8081

You can create your own bots at intelmq-bots/bots.

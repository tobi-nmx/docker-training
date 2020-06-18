# docker-training

This repository is providing snipplets for an internal training and most likely isn't useful for anyone else.

1) Startup to your playground: https://labs.play-with-docker.com/
1) Click `+ ADD NEW INSTANCE`

### Create directory and docker-compose file for stack
type this into the terminal (or use copy & paste)
```
mkdir mywebapp && cd mywebapp
touch docker-compose.yml
```

Now you're all set and the training can start.


### Some useful commands:
```
docker-compose up -d
docker-compose ps
docker-compose logs [service]
docker-compose restart [service]
docker-compose down

git clone https://github.com/tobi-nmx/docker-training.git
```

### Some usefull links
- [Docker Hub](https://hub.docker.com/)
- [YAML](https://rollout.io/blog/yaml-tutorial-everything-you-need-get-started/)
- [Traefik](https://docs.traefik.io/)
- [Wordpress](https://wordpress.org/)
- [MariaDB primer](https://mariadb.com/kb/en/a-mariadb-primer/)


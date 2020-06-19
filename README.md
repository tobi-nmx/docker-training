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


### Docker images used
- https://hub.docker.com/_/wordpress
- https://hub.docker.com/_/mariadb
- https://hub.docker.com/_/traefik


### Some useful commands:
```
docker-compose up -d
docker-compose ps
docker-compose logs [service]
docker-compose restart [service]
docker-compose down
docker-compose exec [container] sh

git clone https://github.com/tobi-nmx/docker-training.git
```
### Security: ARA
##### Secure network design
- dedicated ingress-proxy
- required ports only exposed on proxy
- use of multiple zones
- encryption in transit (TLS1.2)
- GAP: firewall (2nd layer of defense) - use netfilter, but only on ext. interface
- GAP: WAF - mitigation plan: install "Wordfence" plugin)
- GAP: management dashboard exposed to the internet without protection
##### Secure system configuration
- use of official quality assured images provided by Docker Inc.
- GAP: updates must be installed manuall. Option to consider: use of "watchtower"
##### Vulnerability management
- use of official Docker images, scanned on pull
- GAP: regular scan of running containers
##### all other control areas
- missing. But it's not so bad anyhow, actually. Try to hack it and you'll see.
  

### Some usefull links
- [Docker Hub](https://hub.docker.com/)
- [YAML Tutorial](https://rollout.io/blog/yaml-tutorial-everything-you-need-get-started/)
- [Traefik Docs](https://docs.traefik.io/)
- [Wordpress Docs](https://developer.wordpress.org/)
- [MariaDB Primer](https://mariadb.com/kb/en/a-mariadb-primer/)
- [Git - the simple guide](https://rogerdudler.github.io/git-guide/)
- [Github Cheat Sheet](https://github.github.com/training-kit/downloads/github-git-cheat-sheet.pdf)


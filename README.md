# docker-training

This repository is providing snipplets for an internal training and most likely isn't useful for anyone else.

1) Startup to your playground: https://labs.play-with-docker.com/
1) Click `+ ADD NEW INSTANCE`
1) hit Alt+Enter in the terminal window for fullscreen

Now the fun can begin.

### Create directory and docker-compose file
type this into the terminal (or use copy & paste)
```
mkdir mywebapp && cd mywebapp
touch docker-compose.yml
```

### Create the stack
1) load the file `docker-compose.yml` into the built-in web-editor (or use vi)
1) copy & paste the content of `stack.yml` from https://hub.docker.com/_/wordpress/ into your file
1) save the file
1) deploy the stack:
```
docker-compose up
```
1) click on `80`next to `OPEN PORT` and shortly enjoy your website before we
1) press CTRL+c to shutdown the containers and start them again, propery in the background ("detached")
```
docker-compose up -d
```

### now let's make this more secure
1) define networks
2) add proxy and enable TLS

Changes are applied by the "up" same command:
```
docker-compose up -d
```

---

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
docker-compose pull && docker-compose up -d
```

### Security: ARA
##### Secure network design
- dedicated ingress-proxy
- required ports only exposed on proxy
- use of multiple network zones
- encryption in transit (TLS1.2)
- GAP: firewall (2nd layer of defense) - use netfilter, but only on ext. interface
- GAP: WAF - mitigation plan: install "Wordfence" plugin)
- GAP: management dashboard exposed to the internet without protection (but ro)
##### Secure system configuration
- use of official quality assured images provided by Docker Inc.
- GAP: updates must be installed manually. Option to consider: use of "watchtower"
##### Vulnerability management
- use of official Docker images, scanned on pull
- GAP: regular scan of running containers
##### IAM
- GAP: enable MFA in Wordpress!!! (e.g. Duo Plugin)
##### all other control areas
- missing. But it's not so bad anyhow, actually. Try to hack it and you'll see.
  

### Cheating: finishing under 2 minutes
```
# pull repository
git clone https://github.com/tobi-nmx/docker-training.git

# enter stack directory
cd docker-training/mywebsite/

# pull, create and start containers
docker-compose up -d

# Now you need to copy the URL from the "443" link the next to "OPEN PORT"
# then replace the link inside the file "docker-compose.yml" line 30
# by using vi, the built-in editor or the following command.
# Remove the http:// and trailing slash - keep the hostname only!
sed -i 's/MYURL/XXX/g' docker-compose.yml

# Apply changes
docker-compose up -d

# now you can enter the hostname in your browser, change to https, accept the risk
```

### Some usefull links
- [Docker Hub](https://hub.docker.com/)
- [YAML Tutorial](https://rollout.io/blog/yaml-tutorial-everything-you-need-get-started/)
- [Traefik Docs](https://docs.traefik.io/)
- [Wordpress Docs](https://developer.wordpress.org/)
- [MariaDB Primer](https://mariadb.com/kb/en/a-mariadb-primer/)
- [Git - the simple guide](https://rogerdudler.github.io/git-guide/)
- [Github Cheat Sheet](https://github.github.com/training-kit/downloads/github-git-cheat-sheet.pdf)


# docker-training

This repository is providing snipplets for an internal training and is most likely not useful for anyone else.

### Some basics
- [Docker Architecture](https://docs.docker.com/get-started/overview/#docker-architecture)

### Create your Docker host
1) Create your Docker account: https://hub.docker.com/signup (you can use a [temporary e-mail address](https://www.byom.de/trashmails/) if you like)
1) Login and startup to your playground: https://labs.play-with-docker.com/
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
1) open the file `docker-compose.yml` using the built-in web-editor (or use vi)
1) copy & paste the content of `stack.yml` from https://hub.docker.com/_/wordpress/ into your file
1) save the file
1) deploy the stack:
```
docker-compose up
```
1) click on `80`next to `OPEN PORT`, go through the basic Wordpress setup (set the password!) and shortly enjoy your website before we
1) press CTRL+c to shutdown the containers and start them again, this time propery in the background ("detached")
```
docker-compose up -d
```

### now let's make this more secure
Look at all the [Open Security Issues](https://github.com/tobi-nmx/docker-training/issues?q=is%3Aopen+is%3Aissue+label%3Asecurity). Then filter for [Milestone "today"](https://github.com/tobi-nmx/docker-training/issues?q=is%3Aopen+is%3Aissue+label%3Asecurity+milestone%3Atoday).

#### work on the two open tickets for our milestone
1) [segregate networks](https://github.com/tobi-nmx/docker-training/issues/1)
1) [implement ingres proxy with HTTPS](https://github.com/tobi-nmx/docker-training/issues/2)

The solution is documented in the comments of the issues.

#### apply changes
```
docker-compose up -d
```

#### access the url
use the link "443" on the webpage. What do you see?

#### let's fix the URL in the proxy settings
1) replace `MYURL` with the hostname from the url (without `https://` and trailing `/`)
2) apply your changes: `docker-compose up -d`

#### configure Wordpress
Now go to the URL (make sure it's https), look at the certificate, accept the risk. What went wrong? Find our by yourself:
```
docker-compose logs traefik
```

<details>
  <summary>Result (spoiler)</summary>
  
  *Yes, TLS-certificate generation fails due to the 64 characters hostname limit of LetsEncrypt. There is nothing you can do about this (even if you setup a CNAME the play-with-docker ingress router won't find your site anymore). It works on a "normal" vserver or at home if your server is reachable from the internet via port 443.*  
</details>


#### check out the Proxy's dashboard
by clicking at the "9999" link

#### create your first post in Wordpress
Your new website should be online now. Congratulations!

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


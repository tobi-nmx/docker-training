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

### Security Assessment
| Control area   | control   | solution approach   | planned mitigation timeframe |
|:-|:-|:-|:-|
| **Secure network design** |
|| network segmentation | *use of multiple network zones* | :clock9: comming next |
|| network access controls | *expose only required ports via dedicated ingress proxy* | :clock9: comming next |
|| encryption in transit  | *TLS1.2 via ingress proxy* * | :clock9:  comming next |
|| firewall (2nd layer of defense) | use netfilter (ext. inteface, only) | sometime later |
|| WAF | install "Wordfence" plugin | sometime later | 
|| access control for management dashboard exposed to the internet | add TLS and basic auth or bind to localhost | sometime later |
| **Secure system configuration** |
|| use of approved software | use of official quality assured images provided by Docker Inc. | :heavy_check_mark: |
|| patch management | use of "watchtower" or manual process | sometime later |
| **Vulnerability management** |
|| vuln. scan at time of deployment | use of official images scanned by Docker Inc | :heavy_check_mark: |
|| regular vuln. scan | workaround: regular redeployment (see patch mgmt) | probably never |
| **Encryption at rest** |
|| wordpress data | use Linux dm-crypt for OS-level encryption of block storage | probably never |
|| key management | could use KMS or Hashicorp vault | probably never |
| **IAM** |
|| Web-application | enable MFA in Wordpress (e.g. Duo Plugin) | sometime later |
|| OS level | disable SSH password login, mandate password-protection of SSH keys, change SSH port | sometime later |
| **Monitoring** |
|| Application security monitoring | via Wordfence Plugin | sometime later |
|| Availability monitoring | use SaaS, e.g. https://uptimerobot.com/ | sometime later |
|| Observability and log management | Prometheus, Grafana, ELK | sometime later |
| **Malware protection** |
|| application integrity | use "Wordfence" plugin | sometime later |
| **Change Management** |
|| Version control | use Github | sometime later |
| **Backup & DR** |
|| regular backups | use mysqldump and rdiff-backup of docker volumes (offsite) | sometime later |
|| DR concept | re-deploy on any virtual server using backup within minutes | sometime later |
| **other control areas** |
|| * | * | probably never |

&ast; the play-with-docker environment has some technical limitations concerning these controls

### now let's make this more secure
#### define networks
add this to your docker-compose.yml (e.g. before "services:")
```
networks:
  proxy:
  database:
```
#### attach services to the networks
add this to the wordpress definition (somewhere, e.g. below the "volumes:" section)
```
    networks:
      - proxy
      - database
```

add this to the database definition (somewhere, e.g. below the "volumes:" section)
```
    networks:
      - database
```

#### add proxy and enable TLS
add this to your docker-compose.yml
```
  traefik:
    image: traefik:latest
    restart: always
    ports:
      - "80:80"
      - "443:443"
      - "9999:9999"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - ./traefik/acme:/etc/traefik/acme
    networks:
      - proxy
    command:
      - "--log.level=DEBUG"
      - "--providers.docker=true"
      - "--providers.docker.exposedbydefault=false"
      - "--providers.docker.network=mywebsite_proxy"
      - "--entrypoints.http.address=:80"
      - "--entrypoints.https.address=:443"
      - "--entrypoints.traefik.address=:9999"
      - "--api.dashboard=true"
      - "--api.insecure=true"
      - "--certificatesresolvers.letsencrypt.acme.tlschallenge=true"
      - "--certificatesresolvers.letsencrypt.acme.email=wordpress-4452543@byom.de"
      - "--certificatesresolvers.letsencrypt.acme.storage=/etc/traefik/acme/acme.json"
      # remove for production
      - "--certificatesresolvers.letsencrypt.acme.caserver=https://acme-staging-v02.api.letsencrypt.org/directory"
    labels:
      # global redirect to https
      - "traefik.enable=true"
      - "traefik.http.routers.http-catchall.entrypoints=http"
      - "traefik.http.routers.http-catchall.rule=hostregexp(`{host:[a-z0-9-.]+}`)"
      - "traefik.http.routers.http-catchall.middlewares=redirect-to-https"
      - "traefik.http.middlewares.redirect-to-https.redirectscheme.scheme=https"
      - "traefik.http.middlewares.redirect-to-https.redirectscheme.permanent=true"
```

#### enable proxy for wordpress
remove exposed ports from "wordpress" service by removing these lines (or add # in front)
```
    #ports:
    #  - 8080:80
```

and add the proxy labels:
```
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.mywebsite.entrypoints=https"
      - "traefik.http.routers.mywebsite.rule=Host(`MYURL`)"
      - "traefik.http.routers.mywebsite.tls.certresolver=letsencrypt"
```
`MYURL` should actually be changed but let's first find out our hostname.

#### apply changes
```
docker-compose up -d
```

#### access the url
use the link `443` on the webpage. What do you see?

#### let's fix the URL in the proxy settings
1) replace `MYURL` with the hostname from the url (without `https://` and trailing `/`)
2) apply your changes: `docker-compose up -d`

#### configure Wordpress
Now go to the URL (make sure it's https), accept the risk. TLS-certificate generation fails due to a 64 characters limit of LetsEncrypt. It works at home if your server is accessible from the internet on port 443. See for yourself:
```
docker-compose logs traefik
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


## Security concept
| Control area   | control   | solution approach   | planned mitigation timeframe |
|:-|:-|:-|:-|
| **Secure network design** |
|| network segmentation | *use of multiple network zones* | :clock9: comming next |
|| network access controls | *expose only required ports via dedicated ingress proxy | :clock9: comming next |
|| encryption in transit  | *TLS1.2 via ingress proxy* | :clock9:  comming next |
|| firewall (2nd layer of defense) | use netfilter (ext. inteface, only) | sometime later |
|| WAF | install "Wordfence" plugin | sometime later | 
|| access control for management dashboard exposed to the internet | add TLS and basic auth or bind to localhost | sometime later |
| **Secure system configuration** |
|| use of approved software | use of official quality assured images provided by Docker Inc. | :heavy_check_mark: |
| **Patch management** |
|| patch mgmt of container images | use of "watchtower" or manual process | sometime later |
|| patch mgmt of Wordpress incl plugins | enable auto-upgrade and schedule cronjob "wp plugin update --all" | sometime later |
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

*The play-with-docker environment has some technical limitations so not all controls will actually work here*

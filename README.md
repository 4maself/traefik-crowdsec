# traefik-crowdsec
This is a template to deploy a Traefik proxy protected by crowdsec. I'm mainly using this as a quick way to configure and deploy a secure proxy in case I need it. 
It includes Prometheus and Grafana to collect and display a variety of information of your Traefik and Crowdsec containers.

I've mainly chosen Grafana here because of all the community-made dashboards for all kinds of services to help you monitor and observe your important containers.

## Preperations
### Configure the DNS
If your services need to be accessible to the public, you'll need to own and configure a domain. Configuration is done through a DNS of your choosing.
Example:
You can use Cloudflare to configure your A record to point to your IP address and a CNAME record to point a subdomain (e.g. for the Grafana dashboard) to _@_ (root domain) or a completely seperate IP. This is needed to route your domain to the right service and get valid certificates through LetsEncrypt.

### Port forward to your host
Forward ports 80 and 443 to your host. This is done through your router and is different in every environment. Don't forget to put your host IP address on a static lease, so you don't lose your services all of a sudden.

## Update the Prometheus configurations
Update the config file in _prometheus/config_ and rename the Traefik host and crowdsec machine label to something that suits your environment.

## Update the Traefik configuration
Adjust the following in the _traefik/traefik.yml_ file:
- Change the email to something you own. LetsEncrypt will send you email notifications when certificates are expiring.

## Configuration
### Register your bouncer
First, we'll need to register our bouncer to Crowdsec, so we can block unwanted guests.
In the root directory of this repository, execute the following. Of course, you can rename _traefik-bouncer_ to whatever you like:
- `docker-compose exec crowdsec cscli bouncers add traefik-bouncer`
Copy the served key and paste it in the compose behind the _CROWDSEC_BOUNCER_API_KEY_
Next, restart the Traefik bouncer with the following:
- `docker-compose restart bouncer`


## Other notes
### Folder/file permissions
In case Prometheus or grafana don't start correctly, it could be due to file permissions.
To fix this, you can use the following (as root) in the repository root:
`chown 472:472 -R grafana && chown 1000:1000 -R prometheus`


## Credits
Check out the primary repositories/website for the awesome services created!
Ordered as in the docker-compose:

### Traefik
Github repository:
- https://github.com/traefik/traefik
Primary website:
- https://traefik.io/

### Traefik Certs Dumper
Github repository:
- https://github.com/ldez/traefik-certs-dumper

### Crowdsec
Github repository:
- https://github.com/crowdsecurity/crowdsec
Primary website:
- https://crowdsec.net

### Crowdsec Traefik bouncer
Github repository:
- https://github.com/fbonalair/traefik-crowdsec-bouncer

### Prometheus:
Github repository:
- https://github.com/prometheus/prometheus
Primary website:
- https://prometheus.io/

### Grafana:
Github repository:
- https://github.com/grafana/grafana
Primary website:
- https://grafana.com
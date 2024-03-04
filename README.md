# Ansible Role Certbot

This Ansible role requests SSL certificates from Let's Encrypt using Docker and the Certbot Docker image, specifically for domains managed via DigitalOcean's DNS. The role will request a certificate for the apex domain name as well as the wildcard (`*.example.com`) domain name.

## Requirements

- Ansible 2.12 or newer.
- Docker installed on the target host.
- Access to a DigitalOcean account with API access for DNS verification.

## Role Variables

| Variable                    | Description                                        | Default                         |
|-----------------------------|----------------------------------------------------|---------------------------------|
| `letsencrypt_email`         | Email address used for Let's Encrypt registration. | `default_email@domain.com`      |
| `digitalocean_secrets_file` | Path to the DigitalOcean secrets file.             | `/path/to/default/secrets.ini`  |
| `domain_name`               | Domain name for the SSL certificate.               | `example.com`                   |
| `docker_certbot_proxy_container`    | Name of the proxy Docker container used to copy certificates from the cert volume to the host.. | `certbot-proxy-container`       |
| `docker_certbot_container` | Name of the Docker container handling the Let's Encrypt certbot process | `certbot` |
| `docker_certs_volume` | Name of the Docker volume which will contain the SSL certificates | `certs` |
| `ssl_certs_hosts_location`  | Host machine location where certs will be saved.   | `/tmp/`                         |
| `force_cert_copy`           | Determines whether to copy the certs to the host regardless if the certs are eligible for renewal | `False` |

## Example Playbook

Including an example of how to use your role in a playbook:

```yaml
- hosts: servers
  roles:
    - role: ansible_role_certbot
      vars:
        letsencrypt_email: name@example.com
        digitalocean_secrets_file: ~/.secrets/certbot/digitalocean.ini
        domain_name: example.com
        docker_certbot_proxy_container: docker-certbot-proxy-container
        docker_certbot_container: certbot
        docker_certs_volume: certs
        ssl_certs_hosts_location: /tmp/
        force_cert_copy: True
```

### requirements.yml

```yaml
# requirements.yml
roles:
  - name: ansible_role_certbot
    src: https://github.com/ryanreece/ansible-role-certbot.git
    version: main
    scm: git
```

## To Do
- [ ] Add support for AWS Route53 managed domains
- [ ] Make releases for role versions.

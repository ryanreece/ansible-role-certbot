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
| `docker_certs_container`    | Name of the temporary Docker container.            | `default-certs-container`       |
| `ssl_certs_hosts_location`  | Host machine location where certs will be saved.   | `/tmp/`                         |

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
        docker_certs_container: temp-certs-container
        ssl_certs_hosts_location: /tmp/
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

# Cert Manager

Install Cert Manager on a Kubernetes cluster. Includes three certificate issuers: Self-Signed, Let's Encrypt Staging, and Let's Encrypt Production. The Let's Encrypt issuers use a Cloudflare DNS challenge.

[![Ansible Galaxy](https://img.shields.io/badge/ansible--galaxy-cert_manager-blue.svg)](https://galaxy.ansible.com/ui/standalone/roles/rmasters270/cert_manager)

## Requirements

### Localhost

The role is intended to run from the Ansible controller.  If the playbook is executed on a different host it will fail because the templates must be copied to the target host.

### Kube Config

The host and user running the playbook must have kube config configured.

### Helm

The host must have the Helm package manager installed.

### Cloudflare

The Let's Encrypt ACME certificate issuers use a Cloudflare API token to invoke a DNS challenge.  If you are using a different DNS provider Let's Encrypt may not work.

The Let's Encrypt API token must have `Zone:Read, DNS:Edit` permissions for the requested domain.

## Role Variables

| Variable                  | Required | Default                      | Comments                                                                    |
| ------------------------- | -------- | ---------------------------- | --------------------------------------------------------------------------- |
| cert_manager_namespace    | yes      | cert-manager                 | Kubernetes namespace                                                        |
| cert_manager_repo_name    | yes      | jetstack                     | Helm repository name                                                        |
| cert_manager_repo_url     | yes      | <https://charts.jetstack.io> | Helm repository URL                                                         |
| cert_manager_repo_version | yes      | v1.8.0                       | [Helm chart version](https://github.com/cert-manager/cert-manager/releases) |
| cloudflare_email          | yes      | <user@example.com>           | Cloudflare email account                                                    |
| cloudflare_token          | yes      | Your-Cloudflare-Token        | Cloudflare token (recommended) or key                                       |
| letsencrypt_email         | yes      | <user@example.com>           | Lets Encrypt email address                                                  |
| letsencrypt               | no       |                              | Lets Encrypt prod and staging urls                                          |

## Dependencies

Use `rmasters270.helm` role or install `kubernetes cli` and `helm` manually on the host.

Setup `kube config` for the user account and host.

## Example Playbook

```yaml
- hosts: localhost

  vars:
    cloudflare_email: email.address@example.com
    cloudflare_token: o9Sp1wtRZ8waDPZZP8-ZPYwquO7S5GgDaAx-q06d
    letsencrypt_email: email.address@example.com

  roles:
    - rmasters270.helm
    - rmasters270.cert_manager
```

## License

MIT

## Author Information

Ryan Masters

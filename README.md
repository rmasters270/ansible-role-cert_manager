# Cert Manager

Install Cert Manager on a Kubernetes cluster. Includes three certificate issuers: Self-Signed, Let's Encrypt Staging, and Let's Encrypt Production. The Let's Encrypt issuers use a Cloudflare DNS challenge.

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

### cert_manager_namespace

Change the Kubernetes namespace for Cert Manager.

default: `cert-manager`

### cert_manager_repo_name

Name of the Helm repository.

default: `jetstack`

### cert_manager_repo_url

Url pointing to the Helm repository.

default: `https://charts.jetstack.io`

### cert_manager_repo_version

Chart version in the repository.

The default value is pinned to the latest version at the time of writing.  Use `helm search repo cert-manager` to list all versions of the chart.

default: `v1.8.0`

### cloudflare_email

Email address for your cloudflare account.

### cloudflare_key

The Clouflare account key or token.  For security purposes a token is recommended.

### letsencrypt_email

Email address for Let's Encrypt.

### letsencrypt

Holds Let's Encrypt environment values for the cluster issuer and the respective server.

```yaml
  letsencrypt:
    prod:
      cluster_issuer: letsencrypt-prod
      server: https://acme-v02.api.letsencrypt.org/directory
    staging:
      cluster_issuer: letsencrypt-staging
      server: https://acme-staging-v02.api.letsencrypt.org/directory
```

## Dependencies

Use `rmasters270.helm` role or install `kubernetes cli` and `helm` manually on the host.

Setup `kube config` for the user account and host.

## Example Playbook

```yaml
- hosts: localhost

  vars:
    cloudflare_email: email.address@example.com
    cloudflare_key: o9Sp1wtRZ8waDPZZP8-ZPYwquO7S5GgDaAx-q06d
    letsencrypt_email: email.address@example.com

  roles:
    - rmasters270.helm
    - rmasters270.cert_manager
```

## License

MIT

## Author Information

Ryan Masters

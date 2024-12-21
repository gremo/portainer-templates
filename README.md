## Portainer Templates

![Banner](https://github.com/user-attachments/assets/a45314dd-20b1-4d39-b3dd-70cd10744e92)

[![GitHub Actions Workflow Status](https://img.shields.io/github/actions/workflow/status/gremo/portainer-templates/.github/workflows/ci.yaml?label=CI&style=flat-square)](https://github.com/gremo/portainer-templates/actions/workflows/ci.yaml)
[![GitHub last commit](https://img.shields.io/github/last-commit/gremo/portainer-templates?style=flat-square)](https://github.com/gremo/portainer-templates/commits/main)
[![GitHub issues](https://img.shields.io/github/issues/gremo/portainer-templates?style=flat-square)](https://github.com/gremo/portainer-templates/issues)
[![GitHub pull requests](https://img.shields.io/github/issues-pr/gremo/portainer-templates?style=flat-square)](https://github.com/gremo/portainer-templates/pulls)

A bounce of Portainer templates, pre-configured for use with Traefik and sometimes with [Diun](https://github.com/crazy-max/diun) and [Docker Autoheal](https://github.com/willfarrell/docker-autoheal). Simple, no frills, just made to be up and running in a few minutes.

## ✨ Quick start

In **Portainer web UI**, under Settings → General → App Templates, use the following URL:

```txt
https://raw.githubusercontent.com/gremo/portainer-templates/refs/heads/main/templates.json
```

Then in Home → Templates → Applications you should see all the **available applications**.

## 🛟 Traefik tips

Generally, templates only allow you to specify the **Traefik network and endpoint** to use. Configuration of the endpoint, certificates, and redirections are your responsibility (see below).

If you use the **following defaults**, no major changes are required when deploying a container or a stack:

- Traefik network named `traefik`
- Default secure Traefik endpoint named `websecure`

### Network

Containers and stacks using Traefik must specify the **Traefik network**, which **must be created beforehand as external**. Ensure that Traefik itself is included in this network.

### Secure entrypoints

For stacks requiring a **Traefik secure entrypoint**, make sure to configure the `websecure` endpoint on port `443`:

```txt
--entrypoints.web.address=:80
--entrypoints.websecure.address=:443
```

You probably want an **automatic certificate issuance**. This example shows a Let's Encrypt resolver configured with the TLS challenge:

```txt
--entrypoints.websecure.http.tls.certresolver=letsencrypt
--certificatesresolvers.letsencrypt.acme.email=your-email@example.com
--certificatesresolvers.letsencrypt.acme.tlschallenge=true
--certificatesresolvers.letsencrypt.acme.storage=/letsencrypt/acme.json
```

You may also want to enable automatic **redirection from HTTP to HTTPS**:

```txt
--entrypoints.web.http.redirections.entryPoint.to=websecure
--entrypoints.web.http.redirections.entrypoint.permanent=true
--entrypoints.web.http.redirections.entryPoint.scheme=https
```

Finally, make sure that both ports `80` (HTTP) and `443` (HTTPS) are exposed when running Traefik and are accessible through the firewall to ensure Traefik can handle incoming traffic properly.

## ❤️ Contributing

All kinds of contributions are welcome and appreciated. See the [contributing](.github/CONTRIBUTING.md) guidelines, the community looks forward to your contributions!

## 📘 License

Released under the terms of the [ISC License](LICENSE).

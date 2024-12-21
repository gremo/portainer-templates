## Portainer Templates

A collection of Portainer app templates, focused on those compatible with Traefik.

## ✨ Quick start

In **Portainer web UI**, under Settings → General → App Templates, use the following URL:

```txt
https://raw.githubusercontent.com/gremo/portainer-templates/refs/heads/main/templates.json
```

Then in Home → Templates → Applications you should see all the **available applications**.

## 🛟 Traefik tips

Templates only allow specifying an **existing Traefik network and endpoint**. Endpoint configuration, certificates, and redirections are up to you.

### Network

Stacks that use Traefik need to specify the **Traefik network**, which **must be set as external**. Make sure that Traefik itself is included in this network.

### Secure entrypoints

For stacks requiring a **Traefik secure entrypoint**, make sure to configure the `websecure` endpoint on port `443`:

```txt
--entrypoints.web.address=:80
--entrypoints.websecure.address=:443
```

You probably want an **automatic certificate issuance**. This example shows a Let's Encrypt resolver configured with the HTTP challenge, using the `web` entrypoint:

```txt
--entrypoints.websecure.http.tls.certresolver=letsencrypt
--certificatesresolvers.letsencrypt.acme.email=your-email@example.com
--certificatesresolvers.letsencrypt.acme.httpchallenge=true
--certificatesresolvers.letsencrypt.acme.httpchallenge.entrypoint=web
--certificatesresolvers.letsencrypt.acme.storage=/letsencrypt/acme.json
```

You may also want to enable automatic **redirection from HTTP to HTTPS**:

```txt
--entrypoints.web.http.redirections.entryPoint.to=websecure
--entrypoints.web.http.redirections.entrypoint.permanent=true
--entrypoints.web.http.redirections.entryPoint.scheme=https
```

Finally, ensure that both ports `80` (for HTTP) and `443` (for HTTPS) are accessible to allow Traefik to handle incoming traffic correctly.

### Additional entrypoints

For stacks requiring a **Traefik specific entrypoint**, make sure to configure the additional entrypoint with the specified port. MySQL example:

```txt
--entrypoints.mysql.address=:3306
```

Ensure the additional port is accessible for Traefik to function properly.

## ❤️ Contributing

All kinds of contributions are welcome and appreciated. See the [contributing](.github/CONTRIBUTING.md) guidelines, the community looks forward to your contributions!

## 📘 License

Released under the terms of the [ISC License](LICENSE).

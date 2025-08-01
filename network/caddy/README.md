<!-- omit in toc -->
# Using Caddy on Docker as a Secure Reverse Proxy
Reverse Proxy for Uhstray.io Web Applications

<!-- omit in toc -->
## Table of Contents

- [Caddy Documentation](#caddy-documentation)
  - [Original Documentation Links](#original-documentation-links)
  - [Testing Caddy Locally](#testing-caddy-locally)
- [Automated Setup Script](#automated-setup-script)
- [NextCloud Configuration](#nextcloud-configuration)


## Caddy Documentation

### Original Documentation Links

- [Caddy Webserver and Monitoring Guide](https://betterstack.com/community/guides/web-servers/caddy/)
- [Caddy Server Docker Compose Setup](https://caddyserver.com/docs/running#docker-compose)
- [Caddy Conventions](https://caddyserver.com/docs/conventions)
- [Configuring Caddy Files](https://caddyserver.com/docs/caddyfile/concepts)
- [Caddy Docker Hub](https://hub.docker.com/_/caddy)
- [Caddy Reverse Proxy](https://caddyserver.com/docs/quick-starts/reverse-proxy)
- [Caddy HTTPS Configuration](https://caddyserver.com/docs/quick-starts/https)
- [Caddy File Tutorial](https://caddyserver.com/docs/caddyfile-tutorial)
- [Caddy Cloudflare DNS Module](https://github.com/caddy-dns/cloudflare)
- [Caddy Provider Modules Documentation](https://caddy.community/t/how-to-use-dns-provider-modules-in-caddy-2/8148)

### Testing Caddy Locally

Make sure go is installed:

Install xcaddy
```bash
go install github.com/caddyserver/xcaddy/cmd/xcaddy@latest
```

Build Caddy with the Cloudflare DNS module
```bash
xcaddy build latest --with https://github.com/caddy-dns/cloudflare
```

## Automated Setup Script

Use `start-caddy.sh` to automatically configure and start Caddy with your applications using environment variables:

```bash
./start-caddy.sh -k <cloudflare_api_key> [application_options]
```

**Available Applications:**
- `-n` - NocoDB (ip:domain[:port])
- `-w` - N8N workflow (ip:domain[:port])  
- `-p` - Postiz (ip:domain[:port])
- `-s` - Superset (ip:domain[:port])
- `-o` - O11Y observability (ip:domain[:port])
- `-m` - Mixpost (ip:domain[:port])
- `-b` - Wisbot (ip:domain[:port])
- `-c` - Cloud/Collabora (ip:domain[:port])

**Default Ports** (used if not specified):
- NocoDB: 8080
- N8N: 5678
- Postiz: 5000
- Superset: 8088
- O11Y: 3000
- Mixpost: 9095
- Wisbot: 8080
- Cloud: 11000 (main), 3002 (websocket), 9980 (collabora)

**Examples:**

Using default ports:
```bash
./start-caddy.sh -k your_cf_api_key \
  -n 192.168.1.100:nocodb.example.com \
  -w 192.168.1.101:n8n.example.com \
  -p 192.168.1.102:postiz.example.com
```

Using custom ports:
```bash
./start-caddy.sh -k your_cf_api_key \
  -n 192.168.1.100:nocodb.example.com:8081 \
  -w 192.168.1.101:n8n.example.com:5679 \
  -p 192.168.1.102:postiz.example.com:5001
```

The script sets environment variables for the existing Caddyfile and starts the service automatically.

## NextCloud Configuration

- [AIO Reverse Proxy Setup](https://github.com/nextcloud/all-in-one/blob/main/reverse-proxy.md#adapting-the-sample-web-server-configurations-below)
- [Docker Compose Example](https://github.com/nextcloud/all-in-one/discussions/575)

**Configuration Note**
> On a different server (in container or not)
Use the <mark>private ip-address</mark> of the host that shall be running AIO. So e.g. `private.ip.address.of.aio.server:$APACHE_PORT` instead of `localhost:$APACHE_PORT`. If you are not sure how to retrieve that, you can run: `ip a | grep "scope global" | head -1 | awk '{print $2}' | sed 's|/.*||'` on the server that shall be running AIO (the commands only work on Linux).
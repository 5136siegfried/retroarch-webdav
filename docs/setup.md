# Setup Guide — RetroArch WebDAV Sync Server

## Prerequisites

- VPS running Ubuntu 22.04+ (or Debian equivalent)
- Docker + Docker Compose installed
- Nginx installed
- A domain pointing to your VPS (e.g. `webdav.yourdomain.com`)
- Certbot installed

---

## 1. Directory Structure

Create the working directories on your VPS:

```bash
mkdir -p /opt/webdav/data/retroarch/{saves,states,config}
mkdir -p /opt/webdav/config
cd /opt/webdav
```

---

## 2. Docker Compose

Create `/opt/webdav/docker-compose.yml`:

```yaml
services:
  webdav:
    image: bytemark/webdav
    restart: always
    environment:
      AUTH_TYPE: Digest
      USERNAME: your_username
      PASSWORD: your_strong_password
    volumes:
      - ./data:/var/lib/dav/data
    ports:
      - "127.0.0.1:8080:80"
```

> **Note:** Use Digest authentication (not Basic) — RetroArch Cloud Sync requires it.

Start the container:

```bash
docker compose up -d
docker compose logs -f   # verify it started correctly
```

---

## 3. Nginx Reverse Proxy

Create `/etc/nginx/sites-available/webdav.yourdomain.com`:

```nginx
server {
    listen 80;
    server_name webdav.yourdomain.com;
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    server_name webdav.yourdomain.com;

    ssl_certificate /etc/letsencrypt/live/webdav.yourdomain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/webdav.yourdomain.com/privkey.pem;

    # Allow large save states (adjust if needed)
    client_max_body_size 200M;

    location / {
        proxy_pass http://127.0.0.1:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        # Required for WebDAV
        proxy_pass_header Authorization;
        proxy_set_header Destination $http_destination;

        # WebDAV methods
        proxy_method $request_method;
    }
}
```

Enable and reload:

```bash
ln -s /etc/nginx/sites-available/webdav.yourdomain.com /etc/nginx/sites-enabled/
certbot --nginx -d webdav.yourdomain.com
nginx -t && systemctl reload nginx
```

---

## 4. Verify WebDAV is Working

Test from your local machine:

```bash
curl -u your_username:your_strong_password \
  --digest \
  -X PROPFIND \
  https://webdav.yourdomain.com/retroarch/ \
  -H "Depth: 1"
```

You should get an XML response listing the directory. If you get a 401, check your credentials. If you get a 404, the path doesn't exist yet — create it manually inside the container or via the data volume.

---

## 5. Automatic Backup (Optional)

Create a cron job to back up the WebDAV data directory daily:

```bash
# /etc/cron.d/webdav-backup
0 3 * * * root tar -czf /opt/backups/webdav-$(date +\%Y\%m\%d).tar.gz /opt/webdav/data && find /opt/backups -name "webdav-*.tar.gz" -mtime +30 -delete
```

```bash
mkdir -p /opt/backups
```

---

## Next Steps

- [Configure RetroArch on Apple TV](retroarch-appletv.md)
- [Configure Android with FolderSync](android-foldersync.md)
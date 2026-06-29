# retroarch-webdav-sync

Self-hosted WebDAV server to synchronize RetroArch saves and states across multiple devices (Apple TV, Android, desktop) using Docker and Nginx on a VPS.

## Stack

- **WebDAV** — bytemark/webdav (Docker)
- **Reverse proxy** — Nginx + Let's Encrypt (HTTPS)
- **Sync client (Apple TV / iOS)** — RetroArch built-in Cloud Sync
- **Sync client (Android)** — FolderSync

## Features

- Centralized save/state storage on your own infrastructure
- HTTPS with Digest authentication
- Compatible with RetroArch WebDAV Cloud Sync (tvOS/iOS)
- Android sync via FolderSync
- Easily extensible to any WebDAV-compatible client

## Structure

\`\`\`
/opt/webdav/
├── docker-compose.yml
├── data/
│   └── retroarch/
│       ├── saves/
│       └── states/
└── config/
\`\`\`

## Requirements

- VPS with Docker and Docker Compose
- Nginx
- A domain pointing to your VPS
- Certbot (Let's Encrypt)

## Quick Start

See [docs/setup.md](docs/setup.md) for full installation and configuration guide.

## Clients Configuration

| Device | Method | Doc |
|--------|--------|-----|
| Apple TV / iOS | RetroArch Cloud Sync (built-in) | [docs/retroarch-appletv.md](docs/retroarch-appletv.md) |
| Android | FolderSync | [docs/android-foldersync.md](docs/android-foldersync.md) |

## License

GNU
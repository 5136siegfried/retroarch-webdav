# RetroArch Cloud Sync — Apple TV (tvOS)

## Prerequisites

- RetroArch installed on Apple TV (tvOS)
- WebDAV server running and accessible (see [setup.md](setup.md))
- Your WebDAV URL, username, and password ready

---

## 1. Enable Cloud Sync

On your Apple TV, open RetroArch and navigate to:

```
Settings → Saving → Cloud Sync
```

Configure the following fields:

| Setting | Value |
|--------|-------|
| **Cloud Sync Enable** | ON |
| **Cloud Sync Driver** | WebDAV |
| **WebDAV URL** | `https://webdav.yourdomain.com/retroarch/` |
| **Username** | your_username |
| **Password** | your_strong_password |
| **Cloud Sync Mode** | Automatic |

> **Note:** Make sure the URL ends with a trailing slash.

---

## 2. Initial Sync

After configuring, trigger a manual sync to upload your existing saves and states:

```
Settings → Saving → Cloud Sync → Sync Now
```

Wait for the sync to complete before launching any game.

---

## 3. What Gets Synced

RetroArch Cloud Sync synchronizes the following directories by default:

| Directory | Description |
|-----------|-------------|
| `saves/` | In-game save files (.srm, .sav, etc.) |
| `states/` | Save states (.state files) |
| `config/` | RetroArch configuration (optional) |

BIOS files and ROMs are **not** synced by default (too large).

---

## 4. Sync Modes

| Mode | Behavior |
|------|----------|
| **Automatic** | Syncs on launch, on resume from background, and on close |
| **Manual** | Sync only when you trigger `Sync Now` manually |

Automatic is recommended for daily use.

---

## 5. Important Usage Rules

- **Always close content before quitting RetroArch.**
  Use `Close Content` from the Quick Menu, then return to the main menu before exiting. Quitting while a game is running may skip the final sync.

- **Never play on two devices simultaneously.**
  If you play on Apple TV and Android at the same time with the same save file, a conflict will occur.

- **Always let sync complete on one device before starting on another.**
  Wait for the sync indicator to finish before picking up your phone.

---

## 6. Conflict Resolution

If a conflict is detected (same file modified on two devices before syncing):

- RetroArch will **not overwrite** either version automatically
- The conflict is logged, and the file is excluded from sync until resolved

**To keep the server version:**
Delete or rename the local file, then trigger `Sync Now` — the server version will be downloaded.

**To keep the local version:**
Edit the manifest file at `[core_assets_directory]/manifest.local` and update the conflicting file's hash to match the server. On next sync, the local file will be uploaded.

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Sync fails with 401 | Check username/password, ensure AUTH_TYPE is Digest |
| Sync fails with 404 | Verify the URL path exists on the server |
| Files not appearing after sync | Check that `Sync Now` completed without errors |
| Saves missing after switching devices | Ensure sync completed on the previous device before starting |
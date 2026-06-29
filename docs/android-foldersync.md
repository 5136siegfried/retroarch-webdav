# Android Sync — FolderSync

Since RetroArch on Android does not natively support WebDAV Cloud Sync (as of 2025), the recommended approach is to use **FolderSync** to keep the local RetroArch directories in sync with your WebDAV server.

## Prerequisites

- RetroArch installed on Android
- FolderSync installed ([Play Store](https://play.google.com/store/apps/details?id=dk.tacit.android.foldersync.lite))
- Your WebDAV server running and accessible
- Know your RetroArch save/state paths on Android

---

## 1. Find Your RetroArch Directories

Open RetroArch on Android and go to:

```
Settings → Directory
```

Note the paths for:

| Directory | Typical Default Path |
|-----------|---------------------|
| **Save Files** | `/storage/emulated/0/RetroArch/saves` |
| **Save States** | `/storage/emulated/0/RetroArch/states` |

These may differ depending on your installation. Always verify before configuring FolderSync.

---

## 2. Add WebDAV Account in FolderSync

1. Open FolderSync
2. Go to **Accounts** → tap **+**
3. Select **WebDAV**
4. Fill in:

| Field | Value |
|-------|-------|
| **Name** | RetroArch VPS (or any label) |
| **Server address** | `https://webdav.yourdomain.com` |
| **Username** | your_username |
| **Password** | your_strong_password |
| **Authentication** | Digest |

5. Tap **Test connection** — you should see a success message
6. Save the account

---

## 3. Create Sync Pairs

You need two sync pairs: one for saves, one for states.

### Saves Sync Pair

1. Go to **Folderpairs** → tap **+**
2. Configure:

| Setting | Value |
|---------|-------|
| **Account** | RetroArch VPS |
| **Remote folder** | `/retroarch/saves/` |
| **Local folder** | `/storage/emulated/0/RetroArch/saves` |
| **Sync type** | Two-way |
| **Sync on startup** | ON |

### States Sync Pair

Repeat the above with:

| Setting | Value |
|---------|-------|
| **Remote folder** | `/retroarch/states/` |
| **Local folder** | `/storage/emulated/0/RetroArch/states` |

---

## 4. Sync Workflow

### Before playing on Android

1. Open FolderSync
2. Tap **Sync All** (or sync each pair individually)
3. Wait for sync to complete
4. Open RetroArch and play

### After playing on Android

1. Return to the main menu in RetroArch (do not force-quit mid-game)
2. Open FolderSync
3. Tap **Sync All**
4. Wait for sync to complete before opening RetroArch on Apple TV

---

## 5. Optional — Scheduled Auto-Sync

In each sync pair settings, you can enable **Scheduled sync**:

- Frequency: every 15 minutes, hourly, or on Wi-Fi connection
- Useful if you want hands-off sync without opening FolderSync manually

> **Warning:** Avoid scheduled sync if you play on multiple devices — it can cause conflicts if a sync runs while a game is open on another device.

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Connection refused | Check that the VPS is reachable and Nginx is running |
| Auth error | Verify Digest authentication is selected, not Basic |
| Files not syncing | Check FolderSync logs for errors (tap the sync pair → History) |
| Wrong save loaded | Make sure sync completed **before** launching the game |
| Conflict between devices | Stop playing on both, sync one device first, then the other |

---

## Notes

- FolderSync Lite (free) supports WebDAV but limits scheduled sync frequency. The Pro version ($3–5 one-time) removes all limits.
- An alternative to FolderSync is **Autosync for WebDAV** (same developer, more focused on WebDAV).
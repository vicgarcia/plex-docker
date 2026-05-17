# Plex Docker

[Plex Media Server](https://github.com/linuxserver/docker-plex) (LinuxServer image) running via Docker Compose with host networking.

## Setup

Copy the env template and fill in values:

```bash
cp env.template .env
```

| Variable | Description |
|---|---|
| `TZ` | Timezone (e.g. `America/New_York`) |
| `PUID` | UID of the user that owns your media files |
| `PGID` | GID of the user that owns your media files |
| `PLEX_MEDIA_DIR` | Host path to your media library |

To find the right UID/GID, run `id` as the user that owns the media files on the host.

Then start:

```bash
docker compose up -d
```

## Media Library

`PLEX_MEDIA_DIR` is bind-mounted into the container at `/data`. Plex will see the entire directory tree under that path, so structure it however you like on the host:

```
/your/media/
├── movies/
├── tv/
└── music/
```

In the Plex UI, add libraries pointing to `/data/movies`, `/data/tv`, etc.

Plex runs as the user specified by `PUID`/`PGID` — this must match the owner of the files on the host, otherwise Plex won't be able to read them.

## Storage

| Path | Description |
|---|---|
| `./config` | Plex database, metadata, and preferences — persists across restarts |
| `/data` | Bind-mounted media library (read) |
| `/transcode` | Temporary transcode files — stored in RAM (tmpfs), cleared on restart |

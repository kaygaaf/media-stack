# Media Server Stack

## Services
| Service | Port | Purpose |
|---------|------|---------|
| Jellyfin | 8096 | Media playback (Plex alternative) |
| Sonarr | 8989 | TV show management |
| Radarr | 7878 | Movie management |
| Prowlarr | 9696 | Indexer management |
| qBittorrent | 8080 | Download client |
| Bazarr | 6767 | Subtitle management |
| Jellyseerr | 4535 | Request management UI |

## Folder Structure
```
media-stack/
├── docker-compose.yml
├── jellyfin/
├── sonarr/
├── radarr/
├── prowlarr/
├── qbittorrent/
├── bazarr/
├── jellyseerr/
├── media/          # Your actual media files
│   ├── movies/
│   ├── tv/
│   └── music/
└── downloads/      # Incoming downloads
```

## Coolify Setup

1. In Coolify → Add Resource → Docker Compose
2. Paste the contents of `docker-compose.yml`
3. Set the required environment variables if prompted (PUID/PGID = 1000)
4. Deploy

## First-Time Setup

### 1. qBittorrent
- Web UI: http://your-vps:8080
- Default login: admin / adminadmin
- Go to Settings → Download → Set download path to `/downloads`
- Enable automatic torrent management

### 2. Prowlarr
- Web UI: http://your-vps:9696
- Add indexers (Jackett, public trackers)
- Connect to Sonarr/Radarr via "Apps" tab

### 3. Jellyfin
- Web UI: http://your-vps:8096
- Add media library pointing to `/media`
- Map: movies → `/media/movies`, tv → `/media/tv`

### 4. Sonarr
- Connect to Prowlarr (Settings → Indexers → Add Prowlarr)
- Connect to qBittorrent (Settings → Download Clients)
- Set root folder to `/media/tv`

### 5. Radarr
- Connect to Prowlarr
- Connect to qBittorrent
- Set root folder to `/media/movies`

### 6. Bazarr
- Connect to Sonarr/Radarr
- Point to `/media` folder

### 7. Jellyseerr
- Connect to Jellyfin
- Users request movies/TV via this UI

## Permissions
If you get permission errors, run on the VPS:
```bash
sudo chown -R 1000:1000 ./media ./downloads
```

## Updating
```bash
cd media-stack
docker compose pull
docker compose up -d
```

# Jellyfin media stack

A stack of self-hosted media managers and streamer along with VPN.

Stack includes jellyfin, Radarr, Sonarr, Jackett, qBitTorrent and Wireguard for PIA support

## Requirements

- Docker version 23.0.5 and above
- Docker compose version v2.17.3 and above
- It may also work on some of lower versions, but its not tested.

## Install media stack

Copy the .env.example, name it .env and enter the required details in the file

```
# Create network
docker network create fuji-docker-network

# Install the stack
docker compose up -d
```

## Configure Transmission / qBittorrent

For qBitTorrent, 

- Open qBitTorrent at http://localhost:5080. Default username:password is admin:adminadmin
- Go to Tools --> Options --> WebUI --> Change password

## Add indexer to Jackett

- Open Jackett UI at http://localhost:9117
- Add indexer
- Search for torrent indexer
- Add selected

## Configure Radarr

- Open Radarr at http://localhost:7878
- Settings --> Media Management --> Check mark "Movies deleted from disk are automatically unmonitored in Radarr" under File management section --> Save
- Settings --> Indexers --> Add --> Torznab --> Follow steps from Jackett to add indexer
- Settings --> Download clients --> Transmission --> Add Host (transmission / qbittorrent) and port (9091 / 5080) --> Username and password if added --> Test --> Save **Note: If VPN is enabled, then transmission / qbittorrent is reachable on vpn's service name**
- Settings --> General --> Enable advance setting --> Select AUthentication and add username and password

## Add a movie

- Movies --> Search for a movie --> Add Root folder (/downloads) --> Quality profile --> Add movie
- Go to transmission (http://localhost:9091) and see if movie is getting downloaded.

## Configure Jellyfin

- Open Jellyfin at http://localhost:8096
- Configure as it asks for first time.
- Add media library folder and choose /data/movies/

## Configure Jackett

- Add admin password

## Configure Prowlarr

- Open Prowlarr at http://localhost:9696
- Settings --> General --> Authentications --> Select AUthentication and add username and password
- Add Indexers, Indexers --> Add Indexer --> Search for indexer --> Choose base URL --> Test and Save
- Add application, Settings --> Apps --> Add application --> Choose Sonarr or Radarr or any apps to link --> Prowlarr server (http://localhost:9696) --> Radarr server (http://localhost:7878) --> API Key --> Test and Save
- This will add indexers in respective apps automatically.
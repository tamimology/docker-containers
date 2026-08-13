

This document is intended to provide a `docker-compose` of the Docker Containers I am using on my Synology DS1513+ and 2 Mini PCs as my home server. If you are using a different device, that should be fine as well, just make sure you define the `$PERSIST` location in your `.env` file that corresponds to the docker folder on your host device.

If you are not familiar with how to use a compose file, you can change the compose to docker CLI command by using a [docker-compose Converter](https://bucherfa.github.io/dcc-web/)
 

Below are some ports that I have found to be reserved and cannot be used on my host, I always avoid using them

##### A CONSOLIDATED `docker-compose.yml` FILE CAN BE FOUND [HERE](docker-compose.yml) , AND TO START, JUST TYPE IN `docker-compose up -d` AND ALL CONTAINERS WILL START. NOTE THAT SOME CONTAINERS REQUIRES CONFIGURATION BEFORE STARTING, AS WELL AS MAKING SURE ALL REQUIRED FOLDERS ARE CREATED ON THE HOST MACHINE WITH THE CORRESPONDING NAMES AS IN THE COMPOSE 


 #### _RESERVED PORTS NOT BE USED IN ANY CONTAINER_
 <details open="open">
  <summary>
  </summary>

 ```
 80        HTTP
 443       HTTPS
 1234      SSH
 1986      DSM HTTP
 1991      DSM HTTPS
 5050      Unknown Port Used
 5432      Unknown Port Used
 7575      Reserved for the Docker Dashboard (personal choice)
 8080      TCP/UDP
```
</details>


# DATABASES
 
| Container                                                              | Port        | Description                                                 | Docker Image                             |
| :------------                                                          | :----       | :-------                                                    | :---                                     |
| <a href="#authelia-redis">Authelia-Redis</a>                           | 6379        | Used for Authelia                                           | `redis:alpine`                           |
| <a href="#bazarr-postgres">Bazarr-Postgres</a>                         | -           | Used for Bazarr                                             | `postgres:18-alpine`                     |
| <a href="#databases-backup">Databases-Backup</a>                       | 6379        | Backups All Redis, MariaDB and Postgres using CRON          | `tiredofit/db-backup:latest`             |
| <a href="#dockhand-postgres">DockHand-Postgres</a>                     | -           | Used for DockHand                                           | `postgres:18-alpine`                     |
| <a href="#homeassistant-mariadb">HomeAssistant-MariaDB</a>             | -           | Used for HomeAssistant                                      | `jbergstroem/mariadb-alpine:latest`      |
| <a href="#immich-redis">Immich-Redis</a>                               | -           | Used for Immich                                             | `redis:alpine`                           |
| <a href="#immich-postgres">Immich-Postgres</a>                         | -           | Postgres extension provides vector similarity search functions | `ghcr.io/immich-app/postgres:16-vectorchord0.3.0-pgvectors0.3.0` |
| <a href="#influxdb">InfluxDB</a>                                       | 3004        | Time series database                                        | `influxdb:alpine`                        |
| <a href="#invidious-postgres">Invidious-Postgres</a>                   | -           | Used for Invidious                                          | `postgres:alpine`                        |
| <a href="#loki">Loki</a>                                               | 3002        | Multi-tenant log aggregation system                         | `grafana/loki:latest`                    |
| <a href="#manyfold-postgres">Manyfold-Postgres</a>                     | -           | Used for Manyfold                                           | `postgres:18-alpine`                     |
| <a href="#manyfold-redis">Manyfold-Redis</a>                           | -           | Used for Manyfold                                           | `redis:alpine`                           |
| <a href="#nextcloud-mariadb">NextCloud-MariaDB</a>                     | -           | Used for NextCloud (long-term-support)                      | `mariadb:lts`                            |
| <a href="#nextcloud-redis">NextCloud-Redis</a>                         | -           | Used for NextCloud                                          | `redis:alpine`                           |
| <a href="#mariadb">MariaDB</a>                                         | 3306        | MariaDB Database (MySQL Clone)                              | `jbergstroem/mariadb-alpine:10.6.13`     |
| <a href="#paperless-ngx-postgres">Paperless-NGX-Postgres</a>           | -           | Used for Paperless-NGX                                      | `postgres:16-alpine`                     |
| <a href="#paperless-ngx-redis">Paperless-NGX-Redis</a>                 | -           | Used for Paperless-NGX                                      | `redis:alpine`                           |
| <a href="#pastefy-mariadb">Pastefy-MariaDB</a>                         | -           | Used for Pastefy                                            | `jbergstroem/mariadb-alpine:latest`      |
| <a href="#projectsend-mariadb">ProjectSend-MariaDB</a>                 | -           | Used for ProjectSend                                        | `jbergstroem/mariadb-alpine:latest`      |
| <a href="#promtail">Promtail</a>                                       | -           | Sending log data to Loki                                    | `grafana/promtail:latest`                |
| <a href="#prawlarr-postgres">Prawlarr-Postgres</a>                     | -           | Used for Prawlarr                                           | `postgres:18-alpine`                     |
| <a href="#radarr-postgres">Radarr-Postgres</a>                         | -           | Used for Radarr                                             | `postgres:18-alpine`                     |
| <a href="#seerr-postgres">Seerr-Postgres</a>                           | -           | Used for Seerr                                              | `postgres:18-alpine`                     |
| <a href="#sonarr-postgres">Sonarr-Postgres</a>                         | -           | Used for Sonrr                                              | `postgres:18-alpine`                     |
| <a href="#traccar-mariadb">TracCar-MariaDB</a>                         | 3307        | Used for TracCar                                            | `jbergstroem/mariadb-alpine:latest`      |
| <a href="#tracearr-postgres">Tracearr-Postgres</a>                     | -           | Used for Tracearr                                           | `timescale/timescaledb-ha:pg18.1-ts2.25.0` |
| <a href="#tracearr-redis">Tracearr-Redis</a>                           | -           | Used for Tracearr                                           | `redis:alpine`                           |
 


# DOCKER RELATED

| Container                                                              | Port        | Description                                                 | Docker Image                             |
| :------------                                                          | :----       | :-------                                                    | :---                                     |
| <a href="#backrest">Backrest</a>                                       | 9898        | A web UI and orchestrator for restic backup                 | `garethgeorge/backrest:latest`           |
| <a href="#diun">Diun</a>                                               | -           | Docker images status monitor and notifier using CRON        | `crazymax/diun:latest`                   |
| <a href="#socket-proxy">Docker Socket Proxy</a>                        | 2375        | A security-enhanced proxy for the Docker Socket             | `tecnativa/docker-socket-proxy:latest`   |
| <a href="#dockhand">DockHand</a>                                       | 9000        | All-in-One Docker Management Tool replacing Portainer       | `fnsys/dockhand:latest`                  |
| <a href="#monocker">Monocker</a>                                       | -           | Live MONitor doCKER with notifications                      | `petersem/monocker:latest`               |
| <a href="#portainer-ee">Portainer-EE</a>                               | 9000,9443,8000 | Docker management tool (Business Edition)                | `portainer/portainer-ee:alpine`          |
| <a href="#prunemate">PruneMate</a>                                     | 7676        | Docker image & resource cleanup helper, on a schedule!      | `anoniemerd/prunemate:latest`            |
| <a href="#watchtower">WatchTower</a>                                   | -           | Auto-Update containers with latest images                   | `nickfedor/watchtower:latest`            |



# LINKS AND PAGE ORGANISATION

| Container                                                              | Port        | Description                                                 | Docker Image                             |
| :------------                                                          | :----       | :-------                                                    | :---                                     |
| <a href="#homarr">Homarr</a>                                           | 5050        | Links manager                                               | `ghcr.io/homarr-labs/homarr:latest`      |
| <a href="#karakeep">Karakeep</a>                                       | 3421        | A self-hostable bookmark-everything app with a touch of AI  | `ghcr.io/karakeep-app/karakeep:latest`   |



# MEDIA PLAYING

| Container                                                              | Port        | Description                                                 | Docker Image                             |
| :------------                                                          | :----       | :-------                                                    | :---                                     |
| <a href="#audiobookshelf">AudioBookShelf</a>                           | 13378       | Self-hosted audiobook and podcast server                    | `ghcr.io/advplyr/audiobookshelf:latest`  |
| <a href="#ersatztv">ErsatzTV</a>                                       | 8407        | Transform media library into a personalized live TV experience | `ghcr.io/ersatztv/legacy:latest`      |
| <a href="#jellyfin">Jellyfin</a>                                       | 8096        | Media server                                                | `jellyfin/jellyfin:latest`               |
| <a href="#navidrome">NaviDrome</a>                                     | 4533        | Web-based music collection server and streamer              | `ghcr.io/navidrome/navidrome:latest`     |
| <a href="#maloja">Maloja</a>                                           | 42010       | Music scrobble for personal listening statistics            | `krateng/maloja:latest`                  |
| <a href="#subsyncarr-plus">SubSyncarr-Plus</a>                         | 3333        | An automated subtitle synchronization tool                  | `tomtomw123/subsyncarr-plus:latest`      |
| <a href="#tracearr">Tracearr</a>                                       | 3300        | Real-time monitoring for Plex, Jellyfin, and Emby servers   | `ghcr.io/connorgallopo/tracearr:latest`  |



# NETWORKING AND SECURITY

| Container                                                              | Port        | Description                                                 | Docker Image                             |
| :------------                                                          | :----       | :-------                                                    | :---                                     |
| <a href="#authelia">Authelia</a>                                       | 9091        | 2Factor Authentication                                      | `authelia/authelia:latest`               |
| <a href="#cloudflared">Cloudflared</a>                                 | -           | A secure way to publibly connect to local network           | `cloudflare/cloudflared:latest`          |
| <a href="#cloudflared-mon">Cloudflared-Mon</a>                         | -           | Cloudflare Zero Tunnel health monitor                       | `techblog/cloudflared-mon:latest`        |
| <a href="#dashdot">Dash.</a>                                           | 7512        | Modern server dashboard monitor for the host                | `mauricenino/dashdot:latest`             |
| <a href="#netalertx">NetAlertX</a>                                     | 20211       | Monitoring WIFI/LAN and alerting of new devices             | `jokobsk/netalertx:latest`               |
| <a href="#peanut">PeaNUT</a>                                           | 8999        | A tiny dashboard for Network UPS Tools                      | `brandawg93/peanut:latest`               |
| <a href="#speedtest-tracker">Speedtest-Tracker</a>                     | 8765,8766   | A self-hosted internet performance tracking application     | `lscr.io/linuxserver/speedtest-tracker:latest` |
| <a href="#uptime-kuma">Uptime-Kuma</a>                                 | 3001        | Monitoring tool for local DNS                               | `louislam/uptime-kuma:latest`            |
| <a href="#vaultwarden-backup">VaultWarden-Backup</a>                   | -           | Backs up database for VaultWarden                           | `bruceforce/vaultwarden-backup:latest`   |



# PROGRAMMING

| Container                                                              | Port        | Description                                                 | Docker Image                             |
| :------------                                                          | :----       | :-------                                                    | :---                                     |
| <a href="#esphome">ESPHome</a>                                         | 6052,6123   | Programming tool for ESP82 chipsets                         | `esphome/esphome:latest`                 |
| <a href="#flaresolverr">FlareSolverr</a>                               | 8191        | A proxy server to bypass Cloudflare and DDoS-GUARD protection | `ghcr.io/flaresolverr/flaresolverr:latest` |
| <a href="#karakeep-meilisearch">Karakeep-MeiliSearch</a>               | 7700        | A lightning-fast search engine                              | `getmeili/meilisearch:v1.13.3`           |
| <a href="#jellysearch-meilisearch">JellySearch-MeiliSearch</a>         | 7700        | A lightning-fast search engine                              | `getmeili/meilisearch:v1.33.3`           |
| <a href="#mqtt">MQTT</a>                                               | 1883,9001   | Mosquitto broker                                            | `eclipse-mosquitto:latest`               |
| <a href="#octoprint">OctoPrint</a>                                     | 3015        | A snappy web interface for your 3D printer!                 | `octoprint/octoprint:latest`             |
| <a href="#tasmoadmin">TasmoAdmin</a>                                   | 9999        | Manages Sonoff Devices flashed with Tasmota                 | `raymondmm/tasmoadmin:latest`            |
| <a href="#tasmobackup">TasmoBackup</a>                                 | 8259        | Backups/manages Sonoff Devices flashed with Tasmota         | `danmed/tasmobackupv1:latest`            |
| <a href="#vscodium-web">VSCodium-Web</a>                               | 8000        | Visual Studio Code editor                                   | `lscr.io/linuxserver/vscodium-web:latest` |
| <a href="#zigbee2mqtt">Zigbee2MQTT</a>                                 | 9002        | Convert Zigbee protocol to Mosquitto                        | `koenkk/zigbee2mqtt:latest`              |



# SYSTEM MONITORING AND MANAGEMENT

| Container                                                              | Port        | Description                                                 | Docker Image                             |
| :------------                                                          | :----       | :-------                                                    | :---                                     |
| <a href="#filebrowser">FileBrowser</a>                                 | 7888        | A modern web-based file browser                             | `gtstef/filebrowser:stable`              |
| <a href="#homeassistant">HomeAssistant</a>                             | 8123        | Smart home monitoring and management                        | `ghcr.io/home-assistant/home-assistant:stable` |



# SELF-HOSTED

| Container                                                              | Port        | Description                                                 | Docker Image                             |
| :------------                                                          | :----       | :-------                                                    | :---                                     |
| <a href="#apprise">Apprise</a>                                         | 8001        | Push notification using POST                                | `lscr.io/linuxserver/apprise-api:latest` |
| <a href="#bazarr">Bazarr</a>                                           | 6767        | Automating subtitles requests                               | `lscr.io/linuxserver/bazarr:latest`      |
| <a href="#checkrr">Checkrr</a>                                         | 8585        | Scans your library files for corrupt media                  | `aetaric/checkrr:latest`                 |
| <a href="#degoog">DeGoog</a>                                           | 4444        | Search aggregator that queries multiple engines             | `ghcr.io/fccview/degoog:latest`          |
| <a href="#dumbassets">DumbAssets</a>                                   | 3000        | Asset tracker for physical assets, components, and warranties | `dumbwareio/dumbassets:latest`         |
| <a href="#excalidraw">ExcaliDraw</a>                                   | 3765        | Virtual whiteboard for sketching hand-drawn like diagrams   | `excalidraw/excalidraw:latest`           |
| <a href="#gotify">Gotify</a>                                           | 9999        | A self-hosted push notification service replacing Pushover  | `gotify/server:latest`                   |
| <a href="#hauk">Hauk</a>                                               | 9435        | A fully open source self-hosted location sharing service    | `bilde2910/hauk:latest`                  |
| <a href="#immich">Immich</a>                                           | 8212        | A high performance self-hosted photo and video backup solution | `ghcr.io/immich-app/immich-server:release` |
| <a href="#immich-folder-album-creator">Immich Folder Album Creator</a> | -           | Automatically create albums in Immich from a folder structure  | `salvoxia/immich-folder-album-creator:latest` |
| <a href="#immich-machine-learning">Immich-Machine-Learning</a>         | -           | ML that alleviate performance issues on low-memory systems  | `ghcr.io/immich-app/immich-machine-learning:release` |
| <a href="#kavita">Kavita</a>                                           | 8778        | A rocket fueled self-hosted digital library for ebook       | `jvmilazz0/kavita:latest`                |
| <a href="#manyfold">Manyfold</a>                                       | 7214        | Web application for managing a collection of 3D models      | `ghcr.io/manyfold3d/manyfold:latest`     |
| <a href="#mozhi">Mozhi</a>                                             | 6455        | An alternative frontend for many translation engines        | `codeberg.org/aryak/mozhi:latest`        |
| <a href="#nextcloud">NextCloud</a>                                     | 8082        | A safe home for all your data                               | `nextcloud:latest`                       |
| <a href="#nextcloud">NextCloud-Cron</a>                                | -           | Apache Web server for NextCloud                             | `nextcloud:apache`                       |
| <a href="#pairdrop">PairDrop</a>                                       | 3005        | Local file sharing in your web browser inspired by AirDrop  | `lscr.io/linuxserver/pairdrop:latest`    |
| <a href="#paperless-ngx">Paperless-NGX</a>                             | 8777        | A document management system                                | `ghcr.io/paperless-ngx/paperless-ngx:latest` |
| <a href="#paperless-ngx-gotenberg">Paperless-NGX-Gotenberg</a>         | -           | API for converting numerous document formats into PDF files | `gotenberg/gotenberg:latest`             |
| <a href="#paperless-ngx-tika">Paperless-NGX-Tika</a>                   | -           | Detects and extracts metadata/text for different file types | `docker.io/apache/tika:latest`           |
| <a href="#pastefy">PasteFy</a>                                         | 9980        | Pastebin                                                    | `interaapps/pastefy:latest`              |
| <a href="#projectsend">ProjectSend</a>                                 | 8516        | Clients-oriented, private file sharing web application      | `lscr.io/linuxserver/projectsend:latest` |
| <a href="#prowlarr">Prowlarr</a>                                       | 9696        | Indexer manager/proxy to integrate with your various PVR apps | `lscr.io/linuxserver/prowlarr:latest`  |
| <a href="#qr-code">QR Code</a>                                         | 8895        | UI to generate a QR Code from a provided URL (supports Wifi SSID) | `bizzycolah/qrcode-generator:latest` |
| <a href="#quillnote-server">Quillnote-Server</a>                       | 3020        | Self hosated and deployed NextCloud DB server for Quillpad  | `arunk140/quillnote-server`              |
| <a href="#radarr">Radarr</a>                                           | 7878        | Automating movies requests                                  | `lscr.io/linuxserver/radarr:latest`      |
| <a href="#seerr">Seerr</a>                                             | 5055        | Managing requests for your media library                    | `ghcr.io/seerr-team/seerr:latest`        |
| <a href="#sonarr">Sonarr</a>                                           | 8989        | Automating TV shows requests                                | `lscr.io/linuxserver/sonarr:latest`      |
| <a href="#squoosh">Squoosh</a>                                         | 7701        | Ultimate image optimiser with compress and compare          | `dko0/squoosh:latest`                    |
| <a href="#syncthing">SyncThing</a>                                     | 8384        | Syncing platfrom between Mobile-NAS                         | `syncthing/syncthing:1.26`               |
| <a href="#smtp-to-gotify">SMTP-to-Gotify</a>                           | 2525        | Forwards SMTP messages (emails) to Gotify (mainly used for Synology) | `imoshtokill/smtp-to-gotify-docker:latest` |
| <a href="#tinymediamanager">TinyMediaManager</a>                       | 4000,5900   |Organize movie and TV show collections                       | `tinymediamanager/tinymediamanager:latest` |
| <a href="#traccar">TracCar</a>                                         | 8082,5055   | GPS Tracking System                                         | `traccar/traccar:alpine`                 |
| <a href="#transmission">Transmission</a>                               | 9091        | A lightweight BitTorrent client                             | `lscr.io/linuxserver/transmission:latest` |
| <a href="#vaultwarden">VaultWarden</a>                                 | 8089,3012   | Password management application                             | `vaultwarden/server:alpine`              |
| <a href="#wdosg">wDOSg</a>                                             | 3003        | (web DOS games) Centralized DOS game library                | `soulraven1980/wdosg:latest`             |
| <a href="#ytptube">YTPTube</a>                                         | 8081        | A WebUI for yt-dlp with concurrent downloads support        | `ghcr.io/arabcoders/ytptube:latest`      |




To begin with, I defined my own networks to use, which makes my setup easier for IP allocation of some containers.

```
version: '3'

networks:

# Bridge Network
  my_bridge:
    name: my_bridge
    driver: bridge
    attachable: true
    ipam:
     config:
       - subnet: $BRIDGE_SUBNET
         gateway: $BRIDGE_GATEWAY

# MacVLAN Network
  dockervlan:
    name: dockervlan
    driver: macvlan
    driver_opts:
      parent: $ETHCARD
      macvlan_mode: bridge
    ipam:
      config:
        - subnet: "$SUBNET"
          gateway: "$MACVLAN_GATEWAY"
          ip_range: "$MACVLAN_RANGE"

services:
```


Now assuming that every `docker-compose` file you use, will have the standard parameters on top, then you just paste each compose below that corresponds to your need. I have included those in the network definition above.

You can add them to each file separately if needed. I assume that you will be using 1 file for all of the containers, and then `docker-compose up -d CONTAINER_NAME` to start every single container. You can add as many CONTAINER_NAME as needed i.e.

`docker-compose up -d adguard` will start the Adguard container only. If you use `docker-compose up -d` then all containers in the compose file will be started in one command.

```
version: '3'

# name: $STACK_NAME

services:
```



# AppRise
<details>
  <summary>
  </summary>

```
  apprise:
    container_name: apprise-api
    restart: $ALWAYS_ON_POLICY
    hostname: apprise
    environment:
      - PUID=$PUID
      - PGID=$PGID
      - TZ=$TZ
    networks:
      my_bridge:
    ports:
      - 8001:8000
    volumes:
      - $PERSIST/apprise:/config
    image: 'lscr.io/linuxserver/apprise-api:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# AudioBookShelf
<details>
  <summary>
  </summary>

```
  audiobookshelf:
    container_name: audiobookshelf
    restart: $RESTART_POLICY
    hostname: audiobookshelf
    environment:
      - TZ=$TZ
    volumes:
      - $MEDIA_PATH/AudioBooks:/audiobooks
      - $MEDIA_PATH/Podcasts:/podcasts
      - $PERSIST/audiobookshelf/config:/config
      - $PERSIST/audiobookshelf/metadata:/metadata
    ports:
      - 13378:80
    networks:
      my_bridge:
    image: 'ghcr.io/advplyr/audiobookshelf:latest'
```
</details>

[🔼 Back to top](#media-playing)


# Authelia
<details>
  <summary>
  </summary>

```
  authelia:
    container_name: authelia    
    restart: $ALWAYS_ON_POLICY
    hostname: authelia
    environment:
      - TZ=$TZ
    volumes:
      - $PERSIST/authelia/config:/config
      - $PERSIST/authelia/log:/var/log/authelia
      - $PERSIST/authelia/assets:/etc/authelia/assets
    ports:
      - 9091:9091
    networks:
      my_bridge:
    healthcheck:
      disable: true
    depends_on:
      authelia-redis:
        condition: service_started
    labels: 
      monocker.enable: $MONOCKER_ENABLE
    image: 'authelia/authelia:latest'
```
</details>

[🔼 Back to top](#networking-and-security)


# Authelia-Redis
<details>
  <summary>
  </summary>

```
  authelia-redis:
    container_name: authelia-redis
    restart: $ALWAYS_ON_POLICY
    hostname: authelia-redis
    environment:
      - TZ=$TZ
    volumes:
      - $PERSIST/authelia/redis:/data
    expose:
      - 6379
    networks:
      my_bridge:
    healthcheck:
      test: redis-cli ping
      interval: 30s
      timeout: 5s
      retries: 2
    labels: 
      autoheal: $AUTOHEAL_RESTART
      monocker.enable: $MONOCKER_ENABLE
    image: 'redis:alpine'
```
</details>

[🔼 Back to top](#databases)


# Backrest
<details>
  <summary>
  </summary>

```
  backrest:
    container_name: backrest
    restart: $RESTART_POLICY
    hostname: backrest
    environment:
      - BACKREST_DATA=/data
      - BACKREST_CONFIG=/config/config.json
      - XDG_CACHE_HOME=/cache
      - TMPDIR=/tmp
      - TZ=$TZ
      - DOCKER_HOST=$DOCKER_HOST
      - RESTIC_PASSWORD=$RESTIC_PASSWORD
      - ISSUE_1139_FIX_PASSWORDS=true # https://github.com/garethgeorge/backrest/issues/1139
    volumes:
      - $PERSIST/backrest/data:/data
      - $PERSIST/backrest/config:/config
      - $PERSIST/backrest/cache:/cache
      - $PERSIST/backrest/tmp:/tmp
      - $PERSIST:/docker_volumes:ro # Mount local paths to backup
      - $PERSIST/backrest/restores:/restores # Mount for restored content
      - $BACKUPS:/repos # Mount local repos (optional for remote storage)
      # - /var/run/docker.sock:/var/run/docker.sock:ro
    ports:
      - 9898:9898
    networks:
      my_bridge:
    image: 'garethgeorge/backrest:latest'
```
</details>

[🔼 Back to top](#docker-related)


# Bazarr
<details>
  <summary>
  </summary>

```
  bazarr:
    container_name: bazarr
    restart: $RESTART_POLICY
    hostname: bazarr
    environment:
      - PUID=$PUID
      - PGID=$PGID
      - TZ=$TZ
      - POSTGRES_ENABLED=true
      - POSTGRES_HOST=bazarr-postgres
      - POSTGRES_PORT=5432
      - POSTGRES_DATABASE=bazarr
      - POSTGRES_USERNAME=bazarruser
      - POSTGRES_PASSWORD=$DB_PASSWORD
    volumes:
      - $PERSIST/bazarr/config:/config
      - $MEDIA_PATH/Movies/English:/movies #optional
      - $MEDIA_PATH/Shows/English:/tv #optional
    ports:
      - 6767:6767
    networks:
      my_bridge:
    depends_on:
      bazarr-postgres:
          condition: service_healthy
    image: 'lscr.io/linuxserver/bazarr:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Bazarr-Postgres
<details>
  <summary>
  </summary>

```
  bazarr-postgres:
    container_name: bazarr-postgres
    hostname: bazarr-postgres
    restart: $RESTART_POLICY
    environment:
      POSTGRES_DB: bazarr
      POSTGRES_USER: bazarruser
      POSTGRES_PASSWORD: $DB_PASSWORD
    volumes:
      - $PERSIST/bazarr/db:/var/lib/postgresql:rw
    networks:
      my_bridge:
    labels: 
      com.centurylinklabs.watchtower.enable: false
      com.centurylinklabs.watchtower.monitor-only: true
    healthcheck:
      test: ["CMD", "pg_isready", "-q", "-d", "bazarr", "-U", "bazarruser"]
      timeout: 45s
      interval: 10s
      retries: 10
    image: 'postgres:18-alpine'
```
</details>

[🔼 Back to top](#databases)


# Checkrr
<details>
  <summary>
  </summary>

```
  checkrr:
    container_name: checkrr
    restart: $RESTART_POLICY
    hostname: checkrr
    volumes:
      - $PERSIST/checkrr/checkrr.yaml:/etc/checkrr.yaml
      - $PERSIST/checkrr/checkrr.db:/checkrr.db
      - $MEDIA_PATH:/media
    ports:
      - 8585:8585
    networks:
      my_bridge:
    image: 'aetaric/checkrr:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# CloudFlared
<details>
  <summary>
  </summary>

```
  cloudflared:
    container_name: cloudflared
    restart: $ALWAYS_ON_POLICY
    hostname: cloudflared
    user: root
    environment:
      - NO_AUTOUPDATE=true
      - TUNNEL_TOKEN=$TUNNEL_TOKEN
    networks:
      my_bridge:
         ipv4_address: $BRIDGE_NET.201
    command: 'tunnel --no-autoupdate run' # 'tunnel --config /etc/tunnel/config.yml run' 
    labels: 
      monocker.enable: $MONOCKER_ENABLE
    image: 'cloudflare/cloudflared:latest'
```
</details>

[🔼 Back to top](#networking-and-security)


# Cloudflared-Mon
<details>
  <summary>
  </summary>

```
  cloudflared-mon:
    container_name: cloudflared-mon
    restart: $RESTART_POLICY
    hostname: cloudflared-mon
    environment:
      - CHECK_INTERVALS=10
      - NOTIFIERS=pover://$PUSHOVER_USER_KEY@$PUSHOVER_CLOUDFLARED_MON_API
      - CF_TOKEN=$CLOUDFLARED_MON_TOKEN
      - CF_EMAIL=$GM_USER
      - CF_ACCOUNT_ID=$ACCOUNT_ID
    volumes:
      - $PERSIST/cloudflared-mon:/app/db
    networks:
      my_bridge:
    image: 'techblog/cloudflared-mon:latest'
```
</details>

[🔼 Back to top](#networking-and-security)


# DashDot
<details>
  <summary>
  </summary>

```
  dashdot:
    container_name: dashdot
    restart: $RESTART_POLICY
    hostname: dashdot
    environment:
      - DASHDOT_SHOW_HOST=true
      #- DASHDOT_ENABLE_STORAGE_SPLIT_VIEW=true #all drives must be mounted in the container
      - DASHDOT_PAGE_TITLE=My Server Monitor
      #- DASHDOT_ACCEPT_OOKLA_EULA=true
      #- DASHDOT_USE_NETWORK_INTERFACE=bond0 # use (ifconfig -a | sed 's/[ \t].*//;/^$/d') in SSH
      - DASHDOT_ENABLE_CPU_TEMPS=true
      - DASHDOT_DISABLE_TILT=true
      - DASHDOT_DISABLE_HOST=true
      - DASHDOT_OVERRIDE_OS=Synology DSM
      - DASHDOT_OVERRIDE_RAM_BRAND=A-Tech
      - DASHDOT_OVERRIDE_RAM_TYPE=DDR3
      - DASHDOT_OVERRIDE_RAM_FREQUENCY=1600
    volumes:
      - /:/mnt/host:ro
    ports:
      - 7512:3001
    networks:
      my_bridge:
    privileged: false
    image: 'mauricenino/dashdot:latest'
```
</details>

[🔼 Back to top](#networking-and-security)


# Databases-Backup
<details>
  <summary>
  </summary>

```
  databases-backup:
    container_name: databases-backup
    restart: $RESTART_POLICY
    hostname: databases-backup
    volumes:
      - $BACKUPS:/backup
      #- ./post-script.sh:/assets/custom-scripts/post-script.sh
    environment:
      - TIMEZONE=$TZ
      - USER_DBBACKUP=$PUID
      - GROUP_DBBACKUP=$PGID
      - CONTAINER_ENABLE_MONITORING=FALSE
      - LOG_PATH=/logs/logs
      - ENABLE_NOTIFICATIONS=true # only on job failure
      - NOTIFICATION_TYPE=email
      - MAIL_FROM=$GM_USER
      - MAIL_TO=$PUSHOVER_EMAIL_DATABASES_BACKUP
      - SMTP_HOST=$GM_HOST
      - SMTP_PORT=$GM_PORT

    # redis-backup
      - DB01_TYPE=redis
      - DB01_HOST=redisdb
      - DB01_PORT=6379
      - DB01_BACKUP_LOCATION=FILESYSTEM
      - DB01_FILESYSTEM_PATH=/backup/redis/
      - DB01_BACKUP_INTERVAL=1440 #once per day
      - DB01_BACKUP_BEGIN="2355" # @23:55 midnight
      - DB01_CLEANUP_TIME=8640 # keep for 6 days
      - DB01_CHECKSUM=SHA1
      - DB01_COMPRESSION=ZSTD
      - DB01_COMPRESSION_LEVEL=10
      - DB01_SPLIT_DB=false
      - DB01_MYSQL_SINGLE_TRANSACTION=true

    # mariadb-backup
      - DB02_TYPE=mariadb
      - DB02_HOST=mariadb
      - DB02_PORT=3306
      - DB02_NAME=mariadb-name
      - DB02_USER=mariadbuser
      - DB02_PASS=$DB_PASSWORD
      - DB02_BACKUP_LOCATION=FILESYSTEM
      - DB02_FILESYSTEM_PATH=/backup/mariadb/
      - DB02_BACKUP_INTERVAL=1440 #once per day
      - DB02_BACKUP_BEGIN="0000" # @00:00 midnight
      - DB02_CLEANUP_TIME=8640 # keep for 6 days
      - DB02_CHECKSUM=SHA1
      - DB02_COMPRESSION=ZSTD
      - DB02_COMPRESSION_LEVEL=10
      - DB02_SPLIT_DB=false
      - DB02_MYSQL_SINGLE_TRANSACTION=true

    # postgres-backup
      - DB03_TYPE=pgsql
      - DB03_HOST=postgresdb
      - DB03_USER=postgres-user
      - DB03_AUTH=postgres-user
      - DB03_PASS=$DB_PASSWORD
      - DB03_NAME=postgresdb
      - DB03_BACKUP_LOCATION=FILESYSTEM
      - DB03_FILESYSTEM_PATH=/backup/postgres/
      - DB03_SPLIT_DB=false
      - DB03_BACKUP_INTERVAL=1440 #once per day
      - DB03_BACKUP_BEGIN="0030" # @00:30 midnight
      - DB03_CLEANUP_TIME=8640 # keep for 6 days
      - DB03_COMPRESSION=ZSTD
      - DB03_COMPRESSION_LEVEL=10
      - DB03_CHECKSUM=SHA1
      - DB03_MYSQL_SINGLE_TRANSACTION=true

    networks:
      my_bridge:
    links:
      - redisdb
      - mariadb
      - postgresdb
    labels: 
      monocker.enable: $MONOCKER_ENABLE
    image: 'tiredofit/db-backup:latest'
```
</details>

[🔼 Back to top](#databases)


# Diun
<details>
  <summary>
  </summary>

```
  diun:
    container_name: diun
    restart: $RESTART_POLICY
    hostname: diun
    environment:
      - TZ=$TZ
      - LOG_LEVEL=info
      - LOG_JSON=false
    volumes:
      - $PERSIST/diun:/data
      - $PERSIST/diun/diun.yml:/diun.yml:ro
    networks:
      my_bridge:
    command: serve
    image: 'crazymax/diun:latest'
```
</details>

[🔼 Back to top](#docker-related)


# DeGoog
<details>
  <summary>
  </summary>

```
  degoog:
    container_name: degoog
    hostname: degoog
    restart: $RESTART_POLICY
    volumes:
      - $PERSIST/degoog:/app/data
    ports:
      - 4444:4444
    networks:
      my_bridge:
    image: 'ghcr.io/fccview/degoog:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# DockHand
<details>
  <summary>
  </summary>

```
  dockhand:
    container_name: dockhand
    hostname: dockhand
    restart: $RESTART_POLICY
    environment:
      TZ: $TZ
      DOCKER_HOST: $DOCKER_HOST
      DATABASE_URL: postgres://dockhand:$DB_PASSWORD@dockhand-postgres:5432/dockhand
    volumes:
      # - /var/run/docker.sock:/var/run/docker.sock
      - $PERSIST/dockhand/data:/app/data
    networks:
      my_bridge:
    ports:
      - 9000:3000
    depends_on:
      - dockhand-postgres
    image: 'fnsys/dockhand:latest'
```
</details>

[🔼 Back to top](#docker-related)


# DockHand-Postgres
<details>
  <summary>
  </summary>

```
  dockhand-postgres:
    container_name: dockhand-postgres
    hostname: dockhand-postgres
    restart: $RESTART_POLICY
    environment:
      POSTGRES_USER: dockhand
      POSTGRES_DB: dockhand
      POSTGRES_PASSWORD: $DB_PASSWORD
    volumes:
      - $PERSIST/dockhand/db:/var/lib/postgresql:rw
    networks:
      my_bridge:
    healthcheck:
      test: ["CMD", "pg_isready", "-q", "-d", "dockhand", "-U", "dockhand"]
      timeout: 45s
      interval: 10s
      retries: 10
    image: 'postgres:18-alpine'
```
</details>

[🔼 Back to top](#databases)


# DumbAssets
<details>
  <summary>
  </summary>

```
  dumbassets:
    container_name: dumbassets
    restart: $RESTART_POLICY
    hostname: dumbassets
    environment:
      TZ: $TZ
      # NODE_ENV: production
      # DEBUG: true
      SITE_TITLE: DumbAssets
      # BASE_URL: http://localhost:3000
      # DUMBASSETS_PIN: 1234
      # ALLOWED_ORIGINS: '*'
      # APPRISE_URL: 
      CURRENCY_CODE: AUD
      CURRENCY_LOCALE: en-GB
    volumes:
      - $PERSIST/dumbassets:/app/data
    networks:
      my_bridge:
    ports: 
      - 3000:3000
    image: 'dumbwareio/dumbassets:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# ESPHome
<details>
  <summary>
  </summary>

```
  esphome:
    container_name: esphome
    restart: $RESTART_POLICY
    hostname: esphome
    privileged: true
    environment:
      - DEBUG=true
      - CLEANUP=false
      - PUID=$PUID
      - PGID=$PGID
      - TZ=$TZ
      - ESPHOME_DASHBOARD_USE_PING=true
    ports:
      - 6052:6052/tcp
      - 6123:6123/tcp
    networks:
      my_bridge:
    volumes:
      - $PERSIST/esphome/:/config:rw
    # devices:
    #   - /dev/ttyUSB0
    working_dir: /config
    image: 'esphome/esphome:latest'
```
</details>

[🔼 Back to top](#programming)


# ErsatzTV
<details>
  <summary>
  </summary>

```
  ersatztv:
    container_name: ersatztv
    hostname: ersatztv
    restart: $RESTART_POLICY
    environment:
      - TZ=$TZ
    ports:
      - 8409:8409
    networks:
      my_bridge:
    volumes:
      - $PERSIST/ersatztv:/config
      - $MEDIA_PATH:/media:ro
    tmpfs:
      - /transcode
    image: 'ghcr.io/ersatztv/legacy:latest'
```
</details>

[🔼 Back to top](#media-playing)


# ExcaliDraw
<details>
  <summary>
  </summary>

```
  excalidraw:
    container_name: excalidraw
    restart: $RESTART_POLICY
    hostname: excalidraw
    ports:
      - 3765:80
    networks:
      my_bridge:
    healthcheck:
     test: curl -f http://localhost:80/ || exit 1
    stdin_open: true
    environment:
      - NODE_ENV=production
    image: 'excalidraw/excalidraw:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# FileBrowser
<details>
  <summary>
  </summary>

```
  filebrowser:
    container_name: filebrowser
    hostname: filebrowser
    restart: $RESTART_POLICY
    volumes:
      # - /:/host # Do not use a root "/" directory or include the "/var" folder
      - $PERSIST:/docker
      - $PERSIST/filebrowser/data:/home/filebrowser/data
      - $PERSIST/filebrowser/cache:/tmp  # Mount cache directory
      # - $PERSIST/filebrowser/config.yaml:/home/filebrowser/data/config.yaml # https://github.com/gtsteffaniak/filebrowser/blob/main/frontend/public/config.generated.yaml
    ports:
      - 7888:80
    networks:
      my_bridge:
    image: 'gtstef/filebrowser:stable'
```
</details>

[🔼 Back to top](#system-monitoring-and-management)


# FlareSolverr
<details>
  <summary>
  </summary>

```
  flaresolverr:
    container_name: flaresolverr
    hostname: flaresolverr
    restart: $RESTART_POLICY
    environment:
      - LOG_LEVEL=info
      - LOG_FILE=none
      - LOG_HTML=false
      - CAPTCHA_SOLVER=hcaptcha-solver #none
      - TZ=$TZ
    volumes:
      - $PERSIST/flaresolver:/config
    ports:
      - 8191:8191
    networks:
      my_bridge:
    image: 'ghcr.io/flaresolverr/flaresolverr:latest'
```
</details>

[🔼 Back to top](#programming)


# Gotify
<details>
  <summary>
  </summary>

```
  gotify:
    container_name: gotify
    hostname: gotify
    restart: $ALWAYS_ON_POLICY
    environment:
      - TZ=$TZ
      - GOTIFY_PLUGIN_DIR=/app/plugins
      - GOTIFY_DEFAULTUSER_NAME=admin
      - GOTIFY_DEFAULTUSER_PASS=$GOTIFY_USER_PASS
      # - GOTIFY_DATABASE_DIALECT=postgres
      # - GOTIFY_DATABASE_CONNECTION=host=gotify-postgres port=5432 user=gotify dbname=gotifydb password=$DB_PASSWORD sslmode=disable
      # - GOTIFY_DATABASE_DIALECT=postgres # sqlite3, mysql, postgres
      # - GOTIFY_DATABASE_CONNECTION=postgres=host=localhost port=5432 user=gotify dbname=gotifydb password=secret sslmode=disable
    volumes:
      - $PERSIST/gotify:/app/data
    networks:
      my_bridge:
    ports:
      - 9999:80
    labels: 
      monocker.enable: $MONOCKER_ENABLE
    healthcheck:
      test: timeout 10s bash -c ':> /dev/tcp/127.0.0.1/80' || exit 1
      interval: 10s
      timeout: 5s
      retries: 3
      start_period: 90s
    image: 'gotify/server:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Hauk
<details>
  <summary>
  </summary>

```
  hauk:
    container_name: hauk
    restart: $RESTART_POLICY
    hostname: hauk
    volumes:
      - $PERSIST/hauk:/etc/hauk:rw
    ports:
      - 9435:80
    networks:
      my_bridge:
    healthcheck:
      test: curl -f http://localhost:80/ || exit 1
    image: 'bilde2910/hauk:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Homarr
<details>
  <summary>
  </summary>

```
  homarr:
    container_name: homarr
    restart: $RESTART_POLICY
    hostname: homarr
    environment:
      - TZ=$TZ
      - PASSWORD=$DASH_PWD
      - SECRET_ENCRYPTION_KEY=$HOMARR_SECRET_ENCRYPTION_KEY # openssl rand -hex 32
      - DOCKER_HOSTNAMES=192.168.1.10,192.168.1.11,192.168.1.14
      - DOCKER_PORTS= 2375,2375,2375
      - DB_DRIVER=better-sqlite3
      - DB_URL=/appdata/db/db.sqlite
      - BASE_URL=https://homarr.$DOMAINNAME/
      # - DOCKER_HOST=$DOCKER_HOST
      - EDIT_MODE_PASSWORD=$DASH_PWD
      - DEFAULT_COLOR_SCHEME=dark
      # - DISABLE_EDIT_MODE=TRUE
    volumes:
      - $PERSIST/homarr/data:/appdata
    ports:
      - 7575:7575
    networks:
      my_bridge:
    labels: 
      monocker.enable: $MONOCKER_ENABLE
    image: 'ghcr.io/homarr-labs/homarr:latest'
```
</details>

[🔼 Back to top](#links-and-page-organisation)


# HomeAssistant
<details>
  <summary>
  </summary>

```
  homeassistant:
    container_name: homeassistant
    restart: $ALWAYS_ON_POLICY
    hostname: homeassistant
    privileged: true
    environment:
      - DOCKER_HOST=$SOCKET
      - PUID=$PUID
      - PGID=$PGID
      - TZ=$TZ
    volumes:
      - $LOCAL_TIME:/etc/localtime:ro
      - $PERSIST/homeassistant:/config:rw
      - $MEDIA_PATH:/config/media/synology:ro
      # - /run/dbus:/run/dbus:ro
    #devices:
    #  - /dev/ttyUSB0:/dev/ttyUSB0
    #  - /dev/ttyUSB1:/dev/ttyUSB1
    #  - /dev/ttyACM0:/dev/ttyACM0
    #  - /dev/bus/usb:/dev/bus/usb
    ports:
      - 8123:8123
    labels: 
      monocker.enable: $MONOCKER_ENABLE
    healthcheck:
      test: curl -fSs http://127.0.0.1:8123 || exit 1
      start_period: 90s
      timeout: 10s
      interval: 5s
      retries: 3
    network_mode: host
    working_dir: /config
      homeassistant-mariadb:
        condition: service_healthy
    image: 'ghcr.io/home-assistant/home-assistant:stable'
```
</details>

[🔼 Back to top](#system-monitoring-and-management)


# HomeAssistant-MariaDB
<details>
  <summary>
  </summary>

```
  homeassistant-mariadb:
    container_name: homeassistant-mariadb
    restart: $ALWAYS_ON_POLICY
    hostname: homeassistant-mariadb
    environment:
      MARIADB_ROOT_PASSWORD: $DB_PASSWORD
      MARIADB_USER: homeassistant
      MARIADB_PASSWORD: $DB_PASSWORD
      MARIADB_DATABASE: homeassistant
      MARIADB_ROOT_HOST: 192.168.1.10
      MARIADB_AUTO_UPGRADE: "1"
      MARIADB_DISABLE_UPGRADE_BACKUP: "1"
      MARIADB_INITDB_SKIP_TZINFO: "1"
      PGID: $PGID
      PUID: $PUID
      TZ: $TZ
      # SKIP_INNODB: "yes" #only with jbergstroem/mariadb-alpine:latest, but not recommended
    volumes:
        - $PERSIST/homeassistant/db:/var/lib/mysql
    ports:
      - 3306:3306
    networks:
      my_bridge:
    tty: true
    labels: 
      monocker.enable: $MONOCKER_ENABLE  
    image: 'jbergstroem/mariadb-alpine:latest' #10.6.13'
```
</details>

[🔼 Back to top](#databases)


# Immich Folder Album Creator
<details>
  <summary>
  </summary>
```
  immich-folder-album-creator:
    container_name: immich-folder-album-creator
    restart: $RESTART_POLICY
    hostname: immich-folder-album-creator
    environment:
      API_URL: http://192.168.1.10:8212/api
      API_KEY: $IMMICH_API
      ROOT_PATH: /mnt/photos # to match the IMMICH inside-of-container mount path 
      CRON_EXPRESSION: "0 4 * * *" # Daily At 04:00, Immich scans library daily at 3am
      ALBUM_LEVELS: 2
      ALBUM_SEPARATOR: " - "
      MODE: CREATE
      DELETE_CONFIRM: True
      TZ: $TZ
      LOG_LEVEL: INFO
    networks:
      my_bridge:
    image: 'salvoxia/immich-folder-album-creator:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Immich-Machine-Learning 
<details>
  <summary>
  </summary>
```
  immich-machine-learning:
    container_name: immich-machine-learning
    restart: $ALWAYS_ON_POLICY
    hostname: immich-machine-learning
    environment:
      - IMMICH_ENV=production
      - IMMICH_LOG_LEVEL=log
    volumes:
      - $MEDIA_PATH/Photos/immich/cache:/cache
    networks:
      my_bridge:
    depends_on:
      immich-postgres:
        condition: service_started
    # extends: # uncomment this section for hardware acceleration - see https://immich.app/docs/features/ml-hardware-acceleration
    #   file: hwaccel.ml.yml
    #   service: cpu # set to one of [armnn, cuda, openvino, openvino-wsl] for accelerated inference - use the `-wsl` version for WSL2 where applicable
    image: 'ghcr.io/immich-app/immich-machine-learning:release'
```
</details>

[🔼 Back to top](#self-hosted)


# Immich-Postgres 
<details>
  <summary>
  </summary>

```
  immich-postgres:
    container_name: immich-postgres
    restart: $ALWAYS_ON_POLICY
    hostname: immich-postgres
    environment:
      TZ: $TZ
      POSTGRES_DB: immich
      POSTGRES_USER: immichuser
      POSTGRES_PASSWORD: $DB_PASSWORD
      POSTGRES_INITDB_ARGS: '--data-checksums'
      DB_STORAGE_TYPE: 'HDD'
    volumes:
      - $PERSIST/immich/db:/var/lib/postgresql/data
    networks:
      my_bridge:
    labels: 
      com.centurylinklabs.watchtower.enable: false
      com.centurylinklabs.watchtower.monitor-only: true
    shm_size: 128mb
    image: 'ghcr.io/immich-app/postgres:16-vectorchord0.3.0-pgvectors0.3.0'

```
</details>

[🔼 Back to top](#databases)


# Immich-Redis
<details>
  <summary>
  </summary>

```
  immich-redis:
    container_name: immich-redis
    restart: $ALWAYS_ON_POLICY
    hostname: immich-redis
    environment:
      - TZ=$TZ
    volumes:
      - $PERSIST/immich/redis:/data
    networks:
      my_bridge:
    healthcheck:
      test: redis-cli ping || exit 1
      interval: 30s
      timeout: 5s
      retries: 2
    image: 'redis:alpine'
```
</details>

[🔼 Back to top](#databases)


# Immich
<details>
  <summary>
    To migrate from Google Photos to Immich use <a href=https://github.com/simulot/immich-go>this repo</a href>
  </summary>

```
  immich:
    container_name: immich-server
    restart: $ALWAYS_ON_POLICY
    hostname: immich-server
    environment:
      - UPLOAD_LOCATION=./library
      - TZ=$TZ
      - IMMICH_ENV=production
      - IMMICH_LOG_LEVEL=log
      - DB_HOSTNAME=immich-postgres
      - DB_USERNAME=immichuser
      - DB_PASSWORD=$DB_PASSWORD
      - DB_DATABASE_NAME=immich
      - REDIS_HOSTNAME=immich-redis
    volumes:
      - $MEDIA_PATH/Photos/immich/upload:/usr/src/app/upload
      - $MEDIA_PATH/Photos:/mnt/photos:ro
      - $LOCAL_TIME:/etc/localtime:ro
    ports:
      - 8212:3001
    networks:
      my_bridge:
    depends_on:
      immich-redis:
        condition: service_healthy
      immich-postgres:
        condition: service_started
      immich-machine-learning:
        condition: service_started
    healthcheck:
      disable: false
    # devices: # https://bhtechhub.com/how-to-check-gpu-on-linux/
    #   - /dev/dri:/dev/dri
    # extends:
    #   file: hwaccel.transcoding.yml
    #   service: cpu # set to one of [nvenc, quicksync, rkmpp, vaapi, vaapi-wsl] for accelerated transcoding
    image: 'ghcr.io/immich-app/immich-server:release'
```
</details>

[🔼 Back to top](#self-hosted)


# InfluxDB
<details>
  <summary>
  </summary>

```
  influxdb:
    container_name: influxdb
    restart: $RESTART_POLICY
    hostname: influxdb
    ports:
       - 3004:8086
       - 8088:8088
    expose:
      - 8088
    environment:
       - TZ=$TZ
       - DOCKER_INFLUXDB_INIT_MODE=setup
       - DOCKER_INFLUXDB_INIT_USERNAME=inlfuxdb
       - DOCKER_INFLUXDB_INIT_PASSWORD=$DB_PASSWORD
       - DOCKER_INFLUXDB_INIT_ORG=influxdb
       - DOCKER_INFLUXDB_INIT_BUCKET=homeassistant
       - INFLUXDB_BIND_ADDRESS=:8088
    volumes:
       - $PERSIST/influxdb/data:/var/lib/influxdb2
       - $PERSIST/influxdb/config:/etc/influxdb2
       - $LOCAL_TIME:/etc/localtime:ro
    networks:
      my_bridge:
    image: 'influxdb:alpine'
```
</details>

[🔼 Back to top](#databases)


# Jellyfin
<details>
  <summary>
  </summary>

```
  jellyfin:
    container_name: jellyfin
    restart: $RESTART_POLICY
    hostname: jellyfin
    environment:
      - JELLYFIN_PublishedServerUrl=https://jellyfin.$DOMAINNAME  # Optional - alternative address used for autodiscovery
      - JELLYFIN_playlists:allowDuplicates=False
    volumes:
      - $PERSIST/jellyfin/config:/config
      - $PERSIST/jellyfin/cache:/cache
      - $MEDIA_PATH:/media
    ports:
      - 8096:8096
    networks:
      my_bridge:
    labels: 
      monocker.enable: $MONOCKER_ENABLE
    # network_mode: 'host'
    # Optional - may be necessary for docker healthcheck to pass if running in host network mode
    # extra_hosts:
    #   - "host.docker.internal:host-gateway"
    user: $PUID:$PGID
    image: 'jellyfin/jellyfin:latest'
```
</details>

[🔼 Back to top](#media-playing)


# JellySearch-MeiliSearch
<details>
  <summary>
    To be integrated with its Jellyfin <a href=https://github.com/arnesacnussem/jellyfin-plugin-meilisearch/>plugin</a href>
  </summary>

```
  jellysearch-meilisearch:
    container_name: jellysearch-meilisearch
    restart: $RESTART_POLICY
    hostname: jellysearch-meilisearch
    environment:
      TZ: $TZ
      MEILI_MASTER_KEY: $JELLYSEARCH_MEILI_KEY
    volumes:
      - $PERSIST/jellyfin/jellysearch-meilisearch:/meili_data
    ports:
      - 7700:7700
    networks:
      my_bridge:
    image: 'getmeili/meilisearch:v1.33.0'
```
</details>

[🔼 Back to top](#programming)


# Karakeep
<details>
  <summary>
  </summary>

```
  karakeep:
    container_name: karakeep
    hostname: karakeep
    restart: $RESTART_POLICY
    environment:
      MEILI_ADDR: http://karakeep-meilisearch:7700
      MEILI_MASTER_KEY: $KARAKEEP_MEILI_KEY
      DATA_DIR: /data
      NEXTAUTH_URL: http://localhost:3000
      NEXTAUTH_SECRET: $KARAKEEP_AUTH_SECRET
      # BROWSER_WEB_URL: http://chrome:9222 #USED FOR CRAWLING WEBSITES
      # OPENAI_API_KEY: ...
    volumes:
      - $PERSIST/karakeep:/data
    ports:
      - 3421:3000
    networks:
      my_bridge:
    depends_on:
      karakeep-meilisearch:
        condition: service_started
    image: 'ghcr.io/karakeep-app/karakeep:latest'
```
</details>

[🔼 Back to top](#links-and-page-organisation)


# Karakeep-MeiliSearch
<details>
  <summary>
  </summary>

```
  karakeep-meilisearch:
    container_name: karakeep-meilisearch
    hostname: karakeep-meilisearch
    restart: $RESTART_POLICY
    environment:
      MEILI_NO_ANALYTICS: "true"
    volumes:
      - $PERSIST/karakeep/meilisearch:/meili_data
    networks:
      my_bridge:
    image: 'getmeili/meilisearch:v1.13.3'
```
</details>

[🔼 Back to top](#programming)


# Kavita
<details>
  <summary>
  </summary>

```
  kavita:
    container_name: kavita
    restart: $ALWAYS_ON_POLICY
    hostname: kavita
    volumes:
        - $DOCUMENTS/books:/books     # ebooks should be in a folder named "library", Use as many as you want with different mapping on both sides
        - $PERSIST/kavita:/kavita/config
    environment:
        - TZ=$TZ
    ports:
        - 8778:5000
    networks:
        my_bridge:
    image: 'jvmilazz0/kavita:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Loki
<details>
  <summary>
  </summary>

```
  loki:
    container_name: loki
    restart: $RESTART_POLICY
    hostname: loki
    volumes:
      - $PERSIST/loki/config.yaml:/etc/loki/local-config.yaml
      - $PERSIST/loki/data:/data/loki
    ports:
      - 3002:3100
    networks:
      my_bridge:
    command: -config.file=/etc/loki/local-config.yaml
    image: 'grafana/loki:latest'
```
</details>

[🔼 Back to top](#databases)


# Maloja
<details>
  <summary>
  </summary>

```
  maloja:
    container_name: maloja
    restart: $RESTART_POLICY
    hostname: maloja
    environment:
      - PUID=$PUID
      - PGID=$PGID
      - MALOJA_DATA_DIRECTORY=/data
      - MALOJA_FORCE_PASSWORD=$MALOJA_PWD
      - MALOJA_NAME=My Music
      - MALOJA_CHARTS_DISPLAY_TILES=true
      - MALOJA_DISPLAY_ART_ICONS=true
    volumes:
      - $PERSIST/maloja:/data
    networks:
      my_bridge:
    ports:
      - 42010:42010
    image: 'krateng/maloja:latest'
```
</details>

[🔼 Back to top](#media-playing)


# Manyfold
<details>
  <summary>
  </summary>

```
  manyfold:
    container_name: manyfold
    hostname: manyfold
    restart: $ALWAYS_ON_POLICY
    environment:
      REDIS_URL: redis://:$DB_PASSWORD@manyfold-redis:6379/1
      DATABASE_URL: postgresql://manyfolduser:$DB_PASSWORD@manyfold-postgres/manyfold?pool=5
      SECRET_KEY_BASE: $APP_KEY
      PUID: $PUID
      PGID: $PGID
    volumes:
      - $PERSIST/manyfold/libraries:/models:rw
    ports:
      - 7214:3214
    networks:
      my_bridge:
    security_opt:
      - no-new-privileges:true
    depends_on:
      manyfold-redis:
        condition: service_healthy
      manyfold-postgres:
        condition: service_healthy
    labels:
      - "docktail.service.enable=true"
      - "docktail.service.name=3dmodels"
      - "docktail.service.port=3214"  # CONTAINER port (RIGHT side of "8080:80")
      - "docktail.service.service-port=443"
    image: 'ghcr.io/manyfold3d/manyfold:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Manyfold-Postgres
<details>
  <summary>
  </summary>

```
  manyfold-postgres:
    container_name: manyfold-postgres
    hostname: manyfold-postgres
    restart: $RESTART_POLICY
    environment:
      POSTGRES_DB: manyfold
      POSTGRES_USER: manyfolduser
      POSTGRES_PASSWORD: $DB_PASSWORD
    volumes:
      - $PERSIST/manyfold/db:/var/lib/postgresql:rw
    networks:
      my_bridge:
    healthcheck:
      test: ["CMD", "pg_isready", "-q", "-d", "manyfold", "-U", "manyfolduser"]
      timeout: 45s
      interval: 10s
      retries: 10
    image: 'postgres:18-alpine'
```
</details>

[🔼 Back to top](#databases)


# Manyfold-Redis
<details>
  <summary>
  </summary>

```
  manyfold-redis:
    container_name: manyfold-redis
    hostname: manyfold-redis
    restart: $ALWAYS_ON_POLICY
    environment:
      TZ: $TZ
    volumes:
      - $PERSIST/manyfold/redis:/data:rw
    networks:
      my_bridge:
    command:
      - /bin/sh
      - -c
      - redis-server --requirepass $DB_PASSWORD
    read_only: true
    user: $PUID:$PGID
    healthcheck:
      test: ["CMD-SHELL", "redis-cli ping || exit 1"]
    image: 'redis:alpine'
```
</details>

[🔼 Back to top](#databases)


# Monocker
<details>
  <summary>
  </summary>

```
  monocker:
    container_name: monocker
    restart: $RESTART_POLICY
    hostname: monocker
    environment:
      DOCKER_HOST: $DOCKER_HOST
      MESSAGE_PLATFORM: 'pushover@$PUSHOVER_USER_KEY@$PUSHOVER_MONOCKER_API'
      LABEL_ENABLE: 'true'
      ONLY_OFFLINE_STATES: 'false'
      EXCLUDE_EXITED: 'false'      
    # volumes:
    #   - $DOCKER_SOCKET:/var/run/docker.sock:ro
    networks:
      my_bridge:
    image: 'petersem/monocker:latest'
```
</details>

[🔼 Back to top](#docker-related)


# Mozhi
<details>
  <summary>
    Get the android app from <a href=https://github.com/you-apps/TranslateYou>here</a href>
  </summary>

```
  mozhi:
    container_name: mozhi
    restart: $RESTART_POLICY
    hostname: mozhi
    ports:
      - 6455:3000
    networks:
      my_bridge: 
    healthcheck:
      test: wget -nv --tries=1 --spider http://127.0.0.1:3000/api/version || exit 1
      interval: 30s
      timeout: 5s
      retries: 2
    labels: 
      monocker.enable: $MONOCKER_ENABLE  
    image: 'codeberg.org/aryak/mozhi:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# MQTT
<details>
  <summary>
  </summary>

```
  mqtt:
    container_name: mqtt
    restart: $RESTART_POLICY
    hostname: mqtt
    volumes:
      - $PERSIST/mqtt/config:/mosquitto/config
      - $PERSIST/mqtt/data:/mosquitto/data
      - $PERSIST/mqtt/log:/mosquitto/log
      - $PERSIST/mqtt/config/mosquitto.conf:/mosquitto/config/mosquitto.conf
    ports:
      - 1883:1883
      - 9001:9001
    networks:
      my_bridge:
    labels: 
      monocker.enable: $MONOCKER_ENABLE
    image: 'eclipse-mosquitto:latest'
```
</details>

[🔼 Back to top](#programming)


# NaviDrome
<details>
  <summary>
  </summary>

```
  navidrome:
    container_name: navidrome
    restart: $ALWAYS_ON_POLICY
    hostname: navidrome
    environment:
      ND_SCANSCHEDULE: 1h
      ND_AUTHREQUESTLIMIT: 5
      ND_AUTHWINDOWLENGTH: 30s
      ND_LASTFM_ENABLED: false
      ND_ENABLEEXTERNALSERVICES: true
      ND_LISTENBRAINZ_ENABLED: true
      ND_LISTENBRAINZ_BASEURL: http://192.168.1.10:42010/apis/listenbrainz/1/
      ND_LISTENBRAINZ_APIKEY: $LISTENBRAINZ_APIKEY
    volumes:
      - $PERSIST/navidrome:/data
      - $MEDIA_PATH/Music:/music:ro
    ports:
      - 4533:4533
    networks:
      my_bridge:
    labels: 
      monocker.enable: $MONOCKER_ENABLE
    user: $PUID$:$PGID # should be owner of volumes
    image: 'ghcr.io/navidrome/navidrome:latest'
```
</details>

[🔼 Back to top](#media-playing)


# NetAlertX
<details>
  <summary>
  </summary>

```
  netalertx:  
    container_name: netalertx
    restart: $ALWAYS_ON_POLICY
    hostname: netalertx
    environment:
      - TZ=$TZ
      - PORT=20211
      - HOST_USER_ID=$PUID
      - HOST_USER_GID=$PGID
    volumes:
      - $PERSIST/netalertx:/app/config
      - $PERSIST/netalertx/app.db:/app/db/app.db
      - $PERSIST/netalertx/logs:/app/front/log
    network_mode: host
    labels: 
      monocker.enable: $MONOCKER_ENABLE
    security_opt:
      - no-new-privileges:true
    healthcheck:
      test: curl -f http://localhost:20211/ || exit 1
    image: 'jokobsk/netalertx:latest'
```
</details>

[🔼 Back to top](#networking-and-security)


# NextCloud
<details>
  <summary>
  </summary>

```
  nextcloud:
    container_name: nextcloud
    restart: $RESTART_POLICY
    hostname: nextcloud
    environment:
      # - NEXTCLOUD_ADMIN_USER=admin
      # - NEXTCLOUD_ADMIN_PASSWORD=$DB_PASSWORD
      - REDIS_HOST=nextcloud-redis
      - NEXTCLOUD_TRUSTED_DOMAINS=192.168.1.11 nextcloud.$DOMAINNAME
      - TRUSTED_PROXIES=192.168.1.11 nextcloud.$DOMAINNAME
      - OVERWRITEHOST=nextcloud.$DOMAINNAME
      - OVERWRITEPROTOCOL=https
      - MYSQL_PASSWORD=$DB_PASSWORD
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextclouduser
      - MYSQL_HOST=nextcloud-mariadb
      - SMTP_HOST=$GM_HOST
      - SMTP_SECURE=ssl
      - SMTP_PORT=$GM_PORT
      - SMTP_AUTHTYPE=LOGIN
      - SMTP_NAME=$GM_USER
      - SMTP_PASSWORD=$GM_PSW
      - MAIL_FROM_ADDRESS=$GM_USER_MAIL
      - MAIL_DOMAIN=gmail.com
    volumes:
      - $PERSIST/nextcloud/config:/var/www/html:rw
    ports:
      - 8082:80
    networks:
      my_bridge:
    healthcheck:
      test: curl -f http://localhost:80/ || exit 1
    depends_on:
      nextcloud-mariadb:
        condition: service_started
      nextcloud-redis:
        condition: service_healthy
      nextcloud-cron:
        condition: service_started
    image: 'nextcloud:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# NextCloud-Cron
<details>
  <summary>
  </summary>

```
  nextcloud-cron:
    container_name: nextcloud-cron
    restart: $ALWAYS_ON_POLICY
    hostname: nextcloud-redis
    volumes:
      - $PERSIST/nextcloud/config:/var/www/html:rw
    networks:
      my_bridge:
    entrypoint: /cron.sh
    depends_on:
      nextcloud-mariadb:
        condition: service_started
      nextcloud-redis:
        condition: service_started
    image: 'nextcloud:apache'
```
</details>

[🔼 Back to top](#self-hosted)


# NextCloud-MariaDB
<details>
  <summary>
  </summary>

```
  nextcloud-mariadb:
    container_name: nextcloud-mariadb
    restart: $RESTART_POLICY
    hostname: nextcloud-mariadb
    environment:
      - MYSQL_ROOT_PASSWORD=$DB_PASSWORD
      - MYSQL_PASSWORD=$DB_PASSWORD
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextclouduser
      - TZ=$TZ
    volumes:
      - $PERSIST/nextcloud/db:/var/lib/mysql:rw
    networks:
      my_bridge:
    user: $PUID:$PGID
    security_opt:
      - no-new-privileges:false
    command: --transaction-isolation=READ-COMMITTED
    image: 'mariadb:lts'
```
</details>

[🔼 Back to top](#databases)


# NextCloud-Redis
<details>
  <summary>
  </summary>

```
  nextcloud-redis:
    container_name: nextcloud-redis
    restart: $ALWAYS_ON_POLICY
    hostname: nextcloud-redis
    environment:
      TZ: $TZ
    volumes:
      - $PERSIST/nextcloud/redis:/data:rw
    expose:
      - 6379
    networks:
      my_bridge:
    user: $PUID:$PGID
    healthcheck:
      test: ["CMD-SHELL", "redis-cli ping || exit 1"]
    image: 'redis:alpine'
```
</details>

[🔼 Back to top](#databases)


# OctoPrint
<details>
  <summary>
  </summary>

```
  octoprint: # Android app: OctoRemote
    container_name: octoprint
    hostname: octoprint
    restart: $RESTART_POLICY
    # uncomment the lines below to ensure camera streaming is enabled when you add a video device
    #environment:
    #  - ENABLE_MJPG_STREAMER=true
    volumes:
     - $PERSIST/octoprint:/octoprint
    networks:
      my_bridge:
    ports:
      - 3015:80
    devices:
    # use `python3 -m serial.tools.miniterm` to see what the name is of the printer, this requires pyserial
     - /dev/ttyUSB0:/dev/ttyUSB0
    #  - /dev/video0:/dev/video0
    labels: 
      com.centurylinklabs.watchtower.enable: false
      com.centurylinklabs.watchtower.monitor-only: true
    image: 'octoprint/octoprint:latest'
```
</details>

[🔼 Back to top](#programming)


# PairDrop
<details>
  <summary>
  </summary>

```
  pairdrop:
    container_name: pairdrop
    hostname: pairdrop
    restart: $RESTART_POLICY
    environment:
        - PUID=$PUID
        - PGID=$PGID
        - WS_FALLBACK=false # Set to true to enable websocket fallback if the peer to peer WebRTC connection is not available to the client.
        - RATE_LIMIT=false # Set to true to limit clients to 1000 requests per 5 min.
        - RTC_CONFIG=/config/rtc_config.json #false # Set to the path of a file that specifies the STUN/TURN servers.
        - DEBUG_MODE=false # Set to true to debug container and peer connections.
        - TZ=$TZ
    volumes:
      - $PERSIST/pairdrop:/config
    ports:
        - 3005:3000
    networks:
      my_bridge:
    image: 'lscr.io/linuxserver/pairdrop:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Paperless-NGX
<details>
  <summary>
  </summary>

```
  paperless-ngx:
    container_name: paperless-ngx
    restart: $RESTART_POLICY
    hostname: paperless-ngx
    environment:
      PAPERLESS_REDIS: redis://:$DB_PASSWORD@paperless-ngx-redis:6379
      PAPERLESS_DBENGINE: postgresql
      PAPERLESS_DBHOST: paperless-ngx-postgres
      PAPERLESS_DBNAME: paperless
      PAPERLESS_DBUSER: paperless
      PAPERLESS_DBPASS: $DB_PASSWORD 
      PAPERLESS_TRASH_DIR: ../trash
      PAPERLESS_FILENAME_FORMAT: '{created_year}/{document_type}/{title}'
      PAPERLESS_OCR_ROTATE_PAGES_THRESHOLD: 6
      PAPERLESS_TASK_WORKERS: 1
      USERMAP_UID: $PUID
      USERMAP_GID: $PGID
      PAPERLESS_TIME_ZONE: $TZ
      PAPERLESS_ADMIN_USER: admin
      PAPERLESS_ADMIN_PASSWORD: $PAPERLESS_PWD
      PAPERLESS_URL: https://paperless.$DOMAINNAME
      PAPERLESS_CSRF_TRUSTED_ORIGINS: https://paperless.$DOMAINNAME
      PAPERLESS_OCR_LANGUAGES: ara eng # check what is installed by  sudo docker exec -it paperless-ngx tesseract --list-langs
      PAPERLESS_TIKA_ENABLED: 1
      PAPERLESS_TIKA_GOTENBERG_ENDPOINT: http://paperless-ngx-gotenberg:3000
      PAPERLESS_TIKA_ENDPOINT: http://paperless-ngx-tika:9998
      PAPERLESS_EMAIL_HOST: $GM_HOST
      PAPERLESS_EMAIL_PORT: $GM_PORT
      PAPERLESS_EMAIL_HOST_USER: $GM_USER
      PAPERLESS_EMAIL_FROM: $GM_USER
      PAPERLESS_EMAIL_HOST_PASSWORD: $GM_PSW
      PAPERLESS_EMAIL_USE_SSL: true
    volumes:
      - $PERSIST/paperless-ngx/data:/usr/src/paperless/data:rw
      - $DOCUMENTS:/usr/src/paperless/media:rw
      - $PERSIST/paperless-ngx/export:/usr/src/paperless/export:rw
      - $PERSIST/paperless-ngx/consume:/usr/src/paperless/consume:rw
      - $PERSIST/paperless-ngx/trash:/usr/src/paperless/trash:rw
    ports:
      - 8777:8000
    networks:
      my_bridge:
    healthcheck:
      test: ["CMD", "curl", "-fs", "-S", "--max-time", "2", "http://localhost:8000"]
      interval: 30s
      timeout: 10s
      retries: 5
    depends_on:
      paperless-ngx-postgres:
        condition: service_healthy
      paperless-ngx-redis:
        condition: service_healthy
      paperless-ngx-tika:
        condition: service_started
      paperless-ngx-gotenberg:
        condition: service_started
    image: 'ghcr.io/paperless-ngx/paperless-ngx:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Paperless-NGX-Gotenberg
<details>
  <summary>
  </summary>

```
  paperless-ngx-gotenberg:
    container_name: paperless-ngx-gotenberg
    restart: $RESTART_POLICY
    hostname: paperless-ngx-gotenberg
    networks:
      my_bridge:
    user: $PUID:$PGID
    command:
      - "gotenberg"
      - "--chromium-disable-javascript=true"
      - "--chromium-allow-list=file:///tmp/.*"
    image: 'docker.io/gotenberg/gotenberg:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Paperless-NGX-Postgres
<details>
  <summary>
  </summary>

```
  paperless-ngx-postgres:
    container_name: paperless-ngx-postgres
    hostname: paperless-ngx-postgres
    restart: $RESTART_POLICY
    environment:
      POSTGRES_DB: paperless
      POSTGRES_USER: paperless
      POSTGRES_PASSWORD: $DB_PASSWORD
    volumes:
      - $PERSIST/paperless-ngx/db:/var/lib/postgresql/data:rw
    networks:
      my_bridge:
    healthcheck:
      test: ["CMD", "pg_isready", "-q", "-d", "paperless", "-U", "paperless"]
      timeout: 45s
      interval: 10s
      retries: 10
    image: 'postgres:16-alpine'
```
</details>

[🔼 Back to top](#databases)


# Paperless-NGX-Redis
<details>
  <summary>
  </summary>

```
  paperless-ngx-redis:
    restart: $RESTART_POLICY
    container_name: paperless-ngx-redis
    hostname: paperless-ngx-redis
    environment:
      TZ: $TZ
    volumes:
      - $PERSIST/paperless-ngx/redis:/data:rw
    networks:
      my_bridge:
    command:
      - /bin/sh
      - -c
      - redis-server --requirepass $DB_PASSWORD
    read_only: true
    user: $PUID:$PGID
    healthcheck:
      test: ["CMD-SHELL", "redis-cli ping || exit 1"]
    image: 'redis:alpine'
```
</details>

[🔼 Back to top](#databases)


# Paperless-NGX-Tika
<details>
  <summary>
  </summary>

```
  paperless-ngx-tika:
    restart: $RESTART_POLICY
    container_name: paperless-ngx-tika
    hostname: paperless-ngx-tika
    networks:
      my_bridge:
    user: $PUID:$PGID
    image: 'docker.io/apache/tika:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# PasteFy
<details>
  <summary>
  </summary>

```
  pastefy:
    container_name: pastefy
    restart: $RESTART_POLICY
    hostname: pastefy
    environment:
      HTTP_SERVER_PORT: 80
      HTTP_SERVER_CORS: "*"
      DATABASE_DRIVER: mysql
      DATABASE_NAME: pastefy
      DATABASE_USER: pastefy
      DATABASE_PASSWORD: $DB_PASSWORD
      DATABASE_HOST: pastefy-mariadb
      DATABASE_PORT: 3306
      SERVER_NAME: https://paste.$DOMAINNAME
      AUTH_PROVIDER: $PROVIDER
      OAUTH2_INTERAAPPS_CLIENT_ID: $CLIENT_ID
      OAUTH2_INTERAAPPS_CLIENT_SECRET: $CLIENT_SECRET
      PASTEFY_INFO_CUSTOM_NAME: My Home Server
      PASTEFY_INFO_CUSTOM_FOOTER: Say My Name
      #PASTEFY_INFO_CUSTOM_LOGO: https://urltoimage
      PASTEFY_LOGIN_REQUIRED: false
      PASTEFY_LOGIN_REQUIRED_CREATE: true
      PASTEFY_LOGIN_REQUIRED_READ: false
      PASTEFY_ENCRYPTION_DEFAULT: false
    ports:
      - 9980:80
    networks:
      my_bridge:
    depends_on:
      - pastefy-mariadb
    image: "interaapps/pastefy:latest"
```
</details>

[🔼 Back to top](#self-hosted)


# PasteFy-MariaDB
<details>
  <summary>
  </summary>

```
  pastefy-mariadb:
    container_name: pastefy-mariadb
    restart: $ALWAYS_ON_POLICY
    hostname: pastefy-mariadb
    environment:
      MARIADB_ROOT_PASSWORD: $DB_PASSWORD
      MARIADB_ROOT_HOST: 192.168.1.10
      MARIADB_AUTO_UPGRADE: "1"
      MARIADB_DISABLE_UPGRADE_BACKUP: "1"
      MARIADB_INITDB_SKIP_TZINFO: "1"
      PGID: $PGID
      PUID: $PUID
      TZ: $TZ
      # SKIP_INNODB: "yes" #only with jbergstroem/mariadb-alpine:latest, but not recommended
    volumes:
        - $PERSIST/pastefy/db:/var/lib/mysql
    networks:
      my_bridge:
    tty: true
    labels: 
      monocker.enable: $MONOCKER_ENABLE  
    image: 'jbergstroem/mariadb-alpine:latest''
```
</details>

[🔼 Back to top](#databases)


# PeaNUT
<details>
  <summary>
    This was a challenging one on Ubuntu, refer <a href=https://github.com/tamimology/nut-ubuntu>this guide</a href> for more details
  </summary>

```
  peanut:
    container_name: peanut
    hostname: peanut
    restart: $RESTART_POLICY
    environment:
      - NUT_HOST=192.168.1.11
      - NUT_PORT=3493
      - WEB_PORT=8080
      - WEB_USERNAME=$PEANUT_USR
      - WEB_PASSWORD=$PEANUT_PWD
    volumes:
      - $PERSIST/peanut:/config
    networks:
      my_bridge:
    ports:
      - 8999:8080
    user: $PUID:$PGID
    image: 'brandawg93/peanut:latest'
```
</details>

[🔼 Back to top](#networking-and-security)


# Portainer-EE
<details>
  <summary>
  </summary>
  
```
  portainer:
    container_name: portainer-ee
    restart: $ALWAYS_ON_POLICY
    hostname: portainer
    privileged: true
    command: -H $DOCKER_HOST --tlsskipverify
    environment:
      DOCKER_HOST: $SOCKET
      TZ: $TZ
    volumes:
      - $DOCKER_SOCKET:/var/run/docker.sock
      - $PERSIST/portainer:/data
    ports:
      - 8000:8000
      - 9000:9000
      - 9443:9443
    networks:
      my_bridge:
    image: 'portainer/portainer-ee:alpine'
```
</details>

[🔼 Back to top](#docker-related)


# ProjectSend
<details>
  <summary>
  </summary>

```
  projectsend:
    container_name: projectsend
    restart: $RESTART_POLICY
    hostname: projectsend
    environment:
      - PUID=$PUID
      - PGID=$PGID
      - TZ=$TZ
      - MAX_UPLOAD=20000
      - MYSQL_DATABASE=projectsend
      - MYSQL_USER=projectsend
      - MYSQL_PASSWORD=$DB_PASSWORD
      - MYSQL_PORT=3306
    volumes:
      - $PERSIST/projectsend/config:/config
      - $PERSIST/projectsend/data:/data
    ports:
      - 8516:80
    networks:
      my_bridge:
    depends_on:
      projectsend-mariadb:
        condition: service_healthy
    healthcheck:
      test: curl -f http://localhost:80/ || exit 1
    image: 'lscr.io/linuxserver/projectsend:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# ProjectSend-MariaDB
<details>
  <summary>
  </summary>

```
  projectsend-mariadb:
    container_name: projectsend-mariadb
    restart: $ALWAYS_ON_POLICY
    hostname: projectsend-mariadb
    environment:
      MARIADB_ROOT_PASSWORD: $DB_PASSWORD
      MARIADB_ROOT_HOST: 192.168.1.10
      MARIADB_AUTO_UPGRADE: "1"
      MARIADB_DISABLE_UPGRADE_BACKUP: "1"
      MARIADB_INITDB_SKIP_TZINFO: "1"
      PGID: $PGID
      PUID: $PUID
      TZ: $TZ
      # SKIP_INNODB: "yes" #only with jbergstroem/mariadb-alpine:latest, but not recommended
    volumes:
        - $PERSIST/projectsend/db:/var/lib/mysql
    networks:
      my_bridge:
    tty: true
    labels: 
      monocker.enable: $MONOCKER_ENABLE  
    image: 'jbergstroem/mariadb-alpine:latest' #10.6.13'
```
</details>

[🔼 Back to top](#databases)


# Promtail
<details>
  <summary>
  </summary>

```
  promtail:
    container_name: promtail
    restart: $RESTART_POLICY
    hostname: promtail
    volumes:
      - $PERSIST/promtail/config.yml:/etc/promtail/config.yml
    networks:
      my_bridge:
    depends_on:
      - loki
    command: -config.file=/etc/promtail/config.yml
    image: 'grafana/promtail:latest'
```
</details>

[🔼 Back to top](#databases)


# Prowlarr
<details>
  <summary>
  </summary>

```
  prowlarr:
    container_name: prowlarr
    restart: $RESTART_POLICY
    hostname: prowlarr
    environment:
      - PUID=$PUID
      - PGID=$PGID
      - TZ=$TZ
    volumes:
      - $PERSIST/prowlarr/config:/config
    ports:
      - 9696:9696
    networks:
      my_bridge:
    depends_on:
      prowlarr-postgres:
          condition: service_healthy
    image: 'lscr.io/linuxserver/prowlarr:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Prawlarr-Postgres
<details>
  <summary>
  </summary>

```
  prowlarr-postgres:
    container_name: prowlarr-postgres
    hostname: prowlarr-postgres
    restart: $RESTART_POLICY
    environment:
      POSTGRES_DB: prowlarr
      POSTGRES_USER: prowlarruser
      POSTGRES_PASSWORD: $DB_PASSWORD
    volumes:
      - $PERSIST/prowlarr/db:/var/lib/postgresql:rw
    networks:
      my_bridge:
    healthcheck:
      test: ["CMD", "pg_isready", "-q", "-d", "prowlarr", "-U", "prowlarruser"]
      timeout: 45s
      interval: 10s
      retries: 10
    image: 'postgres:18-alpine'
```
</details>

[🔼 Back to top](#databases)


# PruneMate
<details>
  <summary>
  </summary>

```
  prunemate:
    container_name: prunemate
    restart: $RESTART_POLICY
    hostname: prunemate
    volumes:
    #   - $DOCKER_SOCKET:/var/run/docker.sock
      - $PERSIST/prunemate/logs:/var/log
      - $PERSIST/prunemate/config:/config
    environment:
    #   - DOCKER_HOST= $DOCKER_HOST
      - PRUNEMATE_TZ=$TZ
      - PRUNEMATE_TIME_24H=true #false for 12-Hour format (AM/PM)
    networks:
      my_bridge:
    ports:
      - 7676:8080
    image: 'anoniemerd/prunemate:latest'
```
</details>

[🔼 Back to top](#docker-related)


# Quillnote-Server
<details>
  <summary>
    Self built docker contianer from
    
    https://github.com/arunk140/quillnote-server
    
    SSH into host then execute:
    `git clone https://github.com/arunk140/quillnote-server.git`
    `cd quillnote-server`

    `docker build -f "Dockerfile" -t quillnoteserver:latest "."`
    
    SSH into container and then execute:
    `./server user add [username] [password]`
  </summary>

```
  quillnote-server:
    container_name: quillnote-server
    hostname: quillnote-server
    restart: $RESTART_POLICY
    privileged: true
    environment:
      - PUID=$PUID
      - PGID=$PGID
    volumes:
      - $PERSIST/quillnote-server/notes.db:/app/notes.db
    networks:
      my_bridge:
    ports:
      - 3020:3000
    cap_add:
      - NET_ADMIN
    labels: 
      diun.enable: false
      com.centurylinklabs.watchtower.enable: false
    image: 'quillnoteserver:local'
```
</details>

[🔼 Back to top](#self-hosted)


# QR-code
<details>
  <summary>
  </summary>

```
  qrcode:
    container_name: qrcode
    restart: $RESTART_POLICY
    hostname: qrcode
    networks:
      my_bridge:
    ports:
      - 8895:80
    healthcheck:
     test: curl -f http://localhost:80/ || exit 1
    security_opt:
      - no-new-privileges:true
    image: 'bizzycolah/qrcode-generator:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Radarr
<details>
  <summary>
  </summary>

```
  radarr:
    container_name: radarr
    restart: $RESTART_POLICY
    hostname: radarr
    environment:
      - PUID=$PUID
      - PGID=$PGID
      - TZ=$TZ
    volumes:
      - $PERSIST/radarr/config:/config
      - $MEDIA_PATH/Movies/English:/movies #optional
      - $DOWNLOADS:/downloads #optional
    ports:
      - 7878:7878
    networks:
      my_bridge:
    depends_on:
      prowlarr:
          condition: service_started
      transmission:
          condition: service_started
      radarr-postgres:
          condition: service_healthy
    image: 'lscr.io/linuxserver/radarr:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Radarr-Postgres
<details>
  <summary>
  </summary>

```
  radarr-postgres:
    container_name: radarr-postgres
    hostname: radarr-postgres
    restart: $RESTART_POLICY
    environment:
      POSTGRES_DB: radarr
      POSTGRES_USER: radarruser
      POSTGRES_PASSWORD: $DB_PASSWORD
    volumes:
      - $PERSIST/radarr/db:/var/lib/postgresql:rw
    networks:
      my_bridge:
    healthcheck:
      test: ["CMD", "pg_isready", "-q", "-d", "radarr", "-U", "radarruser"]
      timeout: 45s
      interval: 10s
      retries: 10
    image: 'postgres:18-alpine'
```
</details>

[🔼 Back to top](#databases)


# Seerr
<details>
  <summary>
  </summary>

```
  seerr:
    container_name: seerr
    restart: $RESTART_POLICY
    hostname: seerr
    environment:
      - LOG_LEVEL=debug
      - TZ=$TZ
      - PORT=5055 #optional
      - DB_TYPE=postgres # Which DB engine to use, either sqlite or postgres. The default is sqlite.
      - DB_HOST=seerr-postgres
      - DB_PORT=5432
      - DB_USER=seerruser
      - DB_PASS=$DB_PASSWORD
      - DB_NAME=seerr
      - DB_LOG_QUERIES=false # (optional) Whether to log the DB queries for debugging. The default is "false".
    volumes:
      - $PERSIST/seerr/config:/app/config
    ports:
      - 5055:5055
    networks:
      my_bridge:
    init: true
    healthcheck:
      test: wget --no-verbose --tries=1 --spider http://localhost:5055/api/v1/status || exit 1
      start_period: 20s
      timeout: 3s
      interval: 15s
      retries: 3
    depends_on:
      transmission:
          condition: service_started
      sonarr:
          condition: service_started
      radarr:
          condition: service_started
      seerr-postgres:
          condition: service_healthy
    image: 'ghcr.io/seerr-team/seerr:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Seerr-Postgres
<details>
  <summary>
  </summary>

```
  seerr-postgres:
    container_name: seerr-postgres
    hostname: seerr-postgres
    restart: $RESTART_POLICY
    environment:
      POSTGRES_DB: seerr
      POSTGRES_USER: seerruser
      POSTGRES_PASSWORD: $DB_PASSWORD
    volumes:
      - $PERSIST/seerr/db:/var/lib/postgresql:rw
    networks:
      my_bridge:
    healthcheck:
      test: ["CMD", "pg_isready", "-q", "-d", "seerr", "-U", "seerruser"]
      timeout: 45s
      interval: 10s
      retries: 10
    image: 'postgres:18-alpine'
```
</details>

[🔼 Back to top](#databases)


# SMTP-to-Gotify
<details>
  <summary>
  </summary>

```
  smtp-to-gotify:
    container_name: smtp-to-gotify
    hostname: smtp-to-gotify
    restart: $ALWAYS_ON_POLICY
    environment:
    - GOTIFY_URL=$HTTPS_GOTIFY_URL
    - GOTIFY_TOKEN=$GOTIFY_SYNOLOGY_TOKEN
    - API_KEY=$APP_KEY
    network_mode: "host"
    ports:
      - 2525:2525
    labels: 
      monocker.enable: $MONOCKER_ENABLE
    depends_on:
      gotify:
        condition: service_healthy
    image: 'imoshtokill/smtp-to-gotify-docker:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Socket-Proxy
<details>
  <summary>
  </summary>

```
  socket:
    container_name: socketproxy
    restart: $ALWAYS_ON_POLICY
    hostname: socketproxy
    environment:
        PUID: $PUID
        PGID: $PGID
        LOG_LEVEL: err # debug,info,notice,warning,err,crit,alert,emerg
        ## Variables match the URL prefix (i.e. AUTH blocks access to /auth/* parts of the API, etc.).
        # 0 to revoke access.
        # 1 to grant access.
        ## Granted by Default
        EVENTS: 1
        PING: 1
        VERSION: 1
        ## Revoked by Default
        # Security critical
        AUTH: 0
        SECRETS: 0
        POST: 1 # HA, Watchtower
        # Not always needed
        BUILD: 1 # HA
        COMMIT: 1 # HA
        CONFIGS: 1 # HA
        CONTAINERS: 1 # HA, Container-Mon, Portainer
        DISTRIBUTION: 1 #HA
        EXEC: 1 # HA, ContainerWebTTy
        IMAGES: 1 # HA, Portainer, Container-Mon
        INFO: 1 # HA, Portainer
        NETWORKS: 1 # HA, Portainer
        NODES: 1 # HA
        PLUGINS: 1 # HA
        SERVICES: 1 # HA, Portainer
        SESSION: 1 # HA
        SWARM: 1 # HA
        SYSTEM: 0
        TASKS: 1 # Portainer
        VOLUMES: 1 # Portainer
    volumes:
        - $DOCKER_SOCKET:/var/run/docker.sock
        - /dev/log:/dev/log
    ports:
        - 2375:2375
    network_mode: host
    labels: 
      monocker.enable: $MONOCKER_ENABLE
    privileged: true
    image: 'tecnativa/docker-socket-proxy:latest'
```
</details>

[🔼 Back to top](#docker-related)


# Sonarr
<details>
  <summary>
  </summary>

```
  sonarr:
    container_name: sonarr
    restart: $RESTART_POLICY
    hostname: sonarr
    environment:
      - PUID=$PUID
      - PGID=$PGID
      - TZ=$TZ
    volumes:
      - $PERSIST/sonarr/config:/config
      - $MEDIA_PATH/Shows/English:/tv #optional
      - $DOWNLOADS:/downloads #optional
    ports:
      - 8989:8989
    networks:
      my_bridge:
    depends_on:
      prowlarr:
          condition: service_started
      transmission:
          condition: service_started
      sonarr-postgres:
          condition: service_healthy
    image: 'lscr.io/linuxserver/sonarr:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Sonarr-Postgres
<details>
  <summary>
  </summary>

```
  sonarr-postgres:
    container_name: sonarr-postgres
    hostname: sonarr-postgres
    restart: $RESTART_POLICY
    environment:
      POSTGRES_DB: sonarr
      POSTGRES_USER: sonarruser
      POSTGRES_PASSWORD: $DB_PASSWORD
    volumes:
      - $PERSIST/sonarr/db:/var/lib/postgresql:rw
    networks:
      my_bridge:
    healthcheck:
      test: ["CMD", "pg_isready", "-q", "-d", "sonarr", "-U", "sonarruser"]
      timeout: 45s
      interval: 10s
      retries: 10
    image: 'postgres:18-alpine'
```
</details>

[🔼 Back to top](#databases)


# Speedtest-Tracker
<details>
  <summary>
    This has a native integration in Home Assistant, hence I use it, and not any other
  </summary>

```
  speedtest-tracker:
    container_name: speedtest-tracker
    restart: $RESTART_POLICY
    hostname: speedtest-tracker
    environment:
        - PUID=$PUID
        - PGID=$PGID
        - APP_KEY=base64:$ST_APP_KEY
        - DB_CONNECTION=sqlite
        - APP_NAME=My Speedtest Tracker
        - APP_TIMEZONE=$TZ
        - DISPLAY_TIMEZONE=$TZ
        - ADMIN_NAME=Administrator
        - ADMIN_EMAIL=$GM_USER
        - ADMIN_PASSWORD=$SPEEDTEST_PWD
        - SPEEDTEST_SCHEDULE=*/5 * * * * #At every 5th minute #0 * * * * #At every hour
        # - SPEEDTEST_INTERFACE=eth0
    volumes:
        - $PERSIST/speedtest-tracker/data:/config
        - $PERSIST/speedtest-tracker/ssl-keys:/config/keys
    networks:
      my_bridge:
    ports:
        - 8765:80
        - 8766:443
    image: 'lscr.io/linuxserver/speedtest-tracker:latest'
```
</details>

[🔼 Back to top](#networking-and-security)


# Squoosh
<details>
  <summary>
  </summary>

```
  squoosh:
    container_name: squoosh
    restart: $RESTART_POLICY 
    hostname: squoosh
    ports:
      - 7701:8080
    networks:
      my_bridge:
    image: 'dko0/squoosh:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# SubSyncarr-Plus
<details>
  <summary>
  </summary>

```
  subsyncarr-plus:
    container_name: subsyncarr-plus
    hostname: subsyncarr-plus
    restart: $RESTART_POLICY
    environment:
      - TZ=$TZ
      - PUID=$PUID
      - PGID=$PGID
      - CRON_SCHEDULE=0 0 * * * # Runs every day at midnight by default
      - SCAN_PATHS=/movies,/tv # Comma-separated paths to scan
      # - EXCLUDE_PATHS=/movies/temp,/tv/downloads # Optional: exclude directories
      - MAX_CONCURRENT_SYNC_TASKS=1 # Number of parallel processing tasks
      - INCLUDE_ENGINES=ffsubsync,autosubsync,alass # Engines to use
      - SYNC_ENGINE_TIMEOUT_MS=1800000
      - WEB_PORT=3000                 # Port for web UI (default: 3000)
      - WEB_HOST=0.0.0.0              # Host to bind to (default: 127.0.0.1, use 0.0.0.0 for all interfaces)
      # - DB_PATH=/app/data/subsyncarr-plus.db # SQLite database location (default: /app/data/subsyncarr-plus.db)
    volumes:
      - $PERSIST/subsyncarr:/app/data
      - $MEDIA_PATH/Movies/English:/movies
      - $MEDIA_PATH/Shows/English:/tv
    ports:
    - 3333:3000
    networks:
      my_bridge:
    user: '$PUID:$PGID'
    deploy:
      resources:
        limits:
          memory: 768M # Hard limit
        reservations:
          memory: 128M # Minimum guaranteed memory
    image: 'tomtomw123/subsyncarr-plus:latest'
```
</details>

[🔼 Back to top](#media-playing)


# SyncThing
<details>
  <summary>
  </summary>

```
  syncthing:
    container_name: syncthing
    restart: $ALWAYS_ON_POLICY
    hostname: synchthing
    environment:
      - PUID=$PUID
      - PGID=$PGID
      - TZ=$TZ
    volumes:
      - $PERSIST/syncthing:/var/syncthing
      - $PERSIST/syncthing/config:/var/syncthing/config
    ports:
      - 8384:8384
      - 22000:22000/tcp
      - 22000:22000/udp
      - 21027:21027/udp
    networks:
      my_bridge:
    labels: 
      monocker.enable: $MONOCKER_ENABLE
    image: 'syncthing/syncthing:1.26' #latest'
```
</details>

[🔼 Back to top](#self-hosted)


# TasmoAdmin
<details>
  <summary>
  </summary>

```
  tasmoadmin:
    container_name: tasmoadmin
    restart: $RESTART_POLICY
    hostname: tasmoadmin
    volumes:
      - $PERSIST/tasmoadmin:/data:rw
    ports:
      - 0.0.0.0:9999:80/tcp
    networks:
      my_bridge:
    image: 'raymondmm/tasmoadmin:latest'
```
</details>

[🔼 Back to top](#programming)


# TasmoBackup
<details>
  <summary>
  </summary>

```
  tasmobackup:
    container_name: tasmobackup
    restart: $RESTART_POLICY
    hostname: tasmobackup
    environment:
      - TZ=$TZ
      - PUID=$PUID
      - PGID=$PGID
      - DBTYPE=mysql #sqlite
      - MYSQL_SERVER=192.168.1.10
      - DBNAME=tasmobackupdb #data/tasmobackup
      - MYSQL_USERNAME=tasmobackup
      - MYSQL_PASSWORD=$DB_PASSWORD
    volumes:
      - $PERSIST/tasmobackup:/var/www/html/data
    ports:
      - 8259:80
    networks:
      my_bridge:
    image: 'danmed/tasmobackupv1:latest'
```
</details>

[🔼 Back to top](#programming)


# TinyMediaManager
<details>
  <summary>
  </summary>

```
  tinymediamanager:
    container_name: tinymediamanager
    restart: $RESTART_POLICY
    hostname: tinymediamanager
    environment:
      - USER_ID=$PUID
      - GROUP_ID=$PGID
      - ALLOW_DIRECT_VNC=true
      - LC_ALL=en_US.UTF-8 # force UTF8
      - LANG=en_US.UTF-8   # force UTF8
      - TZ=$TZ
      - LC_TIME=C.UTF-8 #for 24 hour format
      # - PASSWORD=<password>
    volumes:
      - $PERSIST/tinymediamanager:/data
      - $PERSIST/tinymediamanager/addons:/app/addons:rw
      - $MEDIA_PATH:/media
      # - $MEDIA_PATH/Movies/English:/media/movies
      # - $MEDIA_PATH/Shows/English:/media/tv_shows
    ports:
      - 5900:5900 # VNC port
      - 4000:4000 # Webinterface
      - 5800:5800
    networks:
      my_bridge:
    image: 'tinymediamanager/tinymediamanager:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# TracCar
<details>
  <summary>
  </summary>

```
  traccar:
    container_name: traccar
    restart: $ALWAYS_ON_POLICY
    hostname: traccar
    environment:
      - TZ=$TZ
    volumes:
      - $LOCAL_TIME:/etc/localtime:ro
      - $PERSIST/traccar/logs:/opt/traccar/logs:rw
      - $PERSIST/traccar/traccar.xml:/opt/traccar/conf/traccar.xml:ro
    ports:
      - 8082:8082
      - 5055:5055/tcp # https://www.traccar.org/osmand/
      - 5055:5055/udp # https://www.traccar.org/osmand/
    depends_on: #https://www.traccar.org/mysql/
      traccar-mariadb:
        condition: service_healthy
    network_mode: host #to be able to integrate in HA
    labels: 
      monocker.enable: $MONOCKER_ENABLE
    image: 'traccar/traccar:alpine'
```
</details>

[🔼 Back to top](#self-hosted)


# TracCar-MariaDB
<details>
  <summary>
  </summary>

```
  traccar-mariadb:
    container_name: traccar-mariadb
    restart: $ALWAYS_ON_POLICY
    hostname: traccar-mariadb
    environment:
      MARIADB_ROOT_PASSWORD: $DB_PASSWORD
      MARIADB_ROOT_HOST: 192.168.1.10
      MARIADB_AUTO_UPGRADE: "1"
      MARIADB_DISABLE_UPGRADE_BACKUP: "1"
      MARIADB_INITDB_SKIP_TZINFO: "1"
      PGID: $PGID
      PUID: $PUID
      TZ: $TZ
      # SKIP_INNODB: "yes" #only with jbergstroem/mariadb-alpine:latest, but not recommended
    volumes:
        - $PERSIST/traccar/db:/var/lib/mysql
    ports:
      - 3307:3306
    networks:
      my_bridge:
    tty: true
    labels: 
      monocker.enable: $MONOCKER_ENABLE  
    image: 'jbergstroem/mariadb-alpine:latest'
```
</details>

[🔼 Back to top](#databases)


# Tracearr
<details>
  <summary>
  </summary>

```
  tracearr:
    container_name: tracearr
    hostname: tracearr
    restart: $RESTART_POLICY
    environment:
      - NODE_ENV=production
      - PORT=3000
      - HOST=0.0.0.0
      - TZ=$TZ
      - DATABASE_URL=postgres://tracearruser:$DB_PASSWORD@tracearr-timescale-db/tracearr
      - REDIS_URL=redis://tracearr-redis:6379
      - JWT_SECRET=$TRACEARR_JWT_SECRET # openssl rand -hex 32
      - COOKIE_SECRET=$TRACEARR_COOKIE_SECRET # openssl rand -hex 32
      - CORS_ORIGIN=*
      - LOG_LEVEL=info
      # - DATABASE_POOL_MAX=50  # Database connection pool size (default: 50)
    volumes:
      - $PERSIST/tracearr/backups:/data/backup
    ports:
      - 3300:3000
    networks:
      my_bridge:
    depends_on:
      tracearr-timescale-db:
        condition: service_healthy
      tracearr-redis:
        condition: service_healthy
    image: 'ghcr.io/connorgallopo/tracearr:latest'
```
</details>

[🔼 Back to top](#media-playing)


# Tracearr-Postgres
<details>
  <summary>
  </summary>

```
  tracearr-timescale-db:
    container_name: tracearr-timescale-db
    hostname: tracearr-timescale-db
    restart: $RESTART_POLICY
    environment:
      - POSTGRES_USER=tracearruser
      - POSTGRES_PASSWORD=$DB_PASSWORD
      - POSTGRES_DB=tracearr
    volumes:
      - $PERSIST/tracearr/db/timescale:/home/postgres/pgdata/data
    networks:
      my_bridge:
    command: postgres -c timescaledb.max_tuples_decompressed_per_dml_transaction=0 -c max_locks_per_transaction=4096 -c timescaledb.telemetry_level=off
    healthcheck:
      test: ["CMD", "pg_isready", "-q", "-d", "tracearr", "-U", "tracearruser"]
      interval: 10s
      timeout: 5s
      retries: 5
    shm_size: 512mb  # Required for PostgreSQL shared memory (increase for large imports)
    ulimits:
      nofile:
        soft: 65536
        hard: 65536  # TimescaleDB chunks require many file descriptors
    image: 'timescale/timescaledb-ha:pg18.1-ts2.25.0'

```
</details>

[🔼 Back to top](#databases)


# Tracearr-Redis
<details>
  <summary>
  </summary>

```
  tracearr-redis:
    container_name: tracearr-redis
    hostname: tracearr-redis
    restart: $RESTART_POLICY
    volumes:
      - $PERSIST/tracearr/db/redis:/data
    command: redis-server --appendonly yes
    networks:
      my_bridge:
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 10s
      timeout: 5s
      retries: 5
    image: 'redis:alpine'

```
</details>

[🔼 Back to top](#databases)


# Transmission
<details>
  <summary>
  </summary>

```
  transmission:
    container_name: transmission
    restart: $RESTART_POLICY
    hostname: transmission
    environment:
      - PUID=$PUID
      - PGID=$PGID
      - TZ=$TZ
      - USER=admin
      - PASS=$TRANSMISSION_PWD
    volumes:
      - $PERSIST/transmission:/config
      - $DOWNLOADS:/downloads
    ports:
      - 9091:9091
      - 51413:51413
      - 51413:51413/udp
    networks:
      my_bridge:
    image: 'lscr.io/linuxserver/transmission:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Uptime-Kuma
<details>
  <summary>
  </summary>

```
  uptime-kuma:
    container_name: uptime-kuma
    restart: $ALWAYS_ON_POLICY
    hostname: uptime-kuma
    environment:
      - TZ=$TZ
      - UPTIME_KUMA_CLOUDFLARED_TOKEN=$UPTIME_KUMA_TUNNEL_TOKEN
      - PUID=$PUID
      - PGID=$PGID
    volumes:
      - $PERSIST/uptime-kuma:/app/data
    ports:
      - 3001:3001
    labels: 
      monocker.enable: $MONOCKER_ENABLE
    network_mode: host
    image: 'louislam/uptime-kuma:latest' #alpine' CloudFlared not working (depracted)
```
</details>

[🔼 Back to top](#networking-and-security)


# VaultWarden
<details>
  <summary>
  </summary>

```
  vaultwarden:
    container_name: vaultwarden
    restart: $ALWAYS_ON_POLICY
    hostname: vaultwarden
    environment: #https://github.com/dani-garcia/vaultwarden/blob/main/.env.template#L98
      - LOG_FILE=/data/vaultwarden.log
      - PUID=$PUID
      - PGID=$PGID
      - TZ=$TZ
      - DATABASE_URL=data/db.sqlite3 # SQLite
      # - DATABASE_URL=mysql://user:password@host[:port]/database_name #MySQL    https://docs.diesel.rs/diesel/mysql/struct.MysqlConnection.html
      - SIGNUPS_ALLOWED=false
      - INVITATIONS_ALLOWED=true
      - LOG_LEVEL=warn
      - EXTENDED_LOGGING=true
      - DOMAIN=https://vw.$DOMAINNAME
      - ROCKET_PORT=8089
      - WEBSOCKET_ENABLED=true
      - ADMIN_TOKEN=$VW_TOKEN
      - SMTP_HOST=$GM_HOST
      - SMTP_FROM=$GM_USER
      - SMTP_PORT=$GM_PORT
      - SMTP_SECURITY=force_tls #starttls (SMTP_SSL=true), force_tls (SMTP_EXPLICIT_TLS=true)
      - SMTP_USERNAME=$GM_USER
      - SMTP_PASSWORD=$GM_PSW
    volumes:
      - $PERSIST/vaultwarden:/data
      - $LOCAL_TIME:/etc/localtime:ro
    ports:
      - 8089:8089
      - 3012:3012
    networks:
      my_bridge:
    user: $PUID:$PGID
    labels: 
      monocker.enable: $MONOCKER_ENABLE
    image: 'vaultwarden/server:alpine'
```
</details>

[🔼 Back to top](#self-hosted)


# VaultWarden-Backup
<details>
  <summary>
  </summary>

```
  vaultwarden-backup:
    container_name: vaultwarden-backup
    restart: $RESTART_POLICY
    hostname: vaultwarden-backup
    environment:
      - CRON_TIME=0 0 * * 6 # At 00:00 on Saturday
      - TZ=$TZ
      - GID=$PGID
      - UID=$PUID
      - TIMESTAMP=true
      - DELETE_AFTER=21
      - BACKUP_DIR=/backups/
      - BACKUP_ADD_DATABASE=true
      - BACKUP_ADD_ATTACHMENTS=true
      - BACKUP_ADD_CONFIG_JSON=true
      - BACKUP_ADD_ICON_CACHE=true
      - BACKUP_ADD_RSA_KEY=true
      - LOG_LEVEL=INFO #WARN 
      - LOG_DIR=/logs/
      - LOG_DIR_PERMISSIONS=-1
    volumes:
      - $PERSIST/vaultwarden:/data/
      - $PERSIST/vaultwarden-backup/logs:/logs/
      - $BACKUPS/vaultwarden-backup:/backups/
    networks:
      my_bridge:
    init: true
    depends_on:
      - vaultwarden
    labels: 
      autoheal: $AUTOHEAL_RESTART
      monocker.enable: $MONOCKER_ENABLE
    image: 'bruceforce/vaultwarden-backup:latest'
```
</details>

[🔼 Back to top](#networking-and-security)


# VSCodium-Web
<details>
  <summary>
  </summary>

```
  vscodium-web:
    container_name: vscodium-web
    restart: $RESTART_POLICY
    hostname: vscodium-web
    environment:
      - PUID=$PUID
      - PGID=$PGID
      - TZ=$TZ
      - DEFAULT_WORKSPACE=/home/coder/project #optional
      # - CONNECTION_TOKEN= #optional
      # - CONNECTION_TOKEN_FILE= #optional
      # - SUDO_PASSWORD=password #optional
      # - SUDO_PASSWORD_HASH= #optional
      # - CODE_ARGS= #optional
    volumes:
      - $PERSIST/vscodium-web:/config
      - $PERSIST:/home/coder/project:rw
    ports:
      - 8000:8000
    networks:
      my_bridge:
    image: 'lscr.io/linuxserver/vscodium-web:latest'
```
</details>

[🔼 Back to top](#programming)


# WatchTower
<details>
  <summary>
  </summary>

```
  watchtower:
    container_name: watchtower
    restart: $RESTART_POLICY
    hostname: watchtower
    environment:
      DOCKER_HOST: $DOCKER_HOST
      TZ: $TZ
      WATCHTOWER_REMOVE_VOLUMES: "true"
      WATCHTOWER_CLEANUP: "true"
      WATCHTOWER_LABEL_ENABLE: "false"
      WATCHTOWER_NO_STARTUP_MESSAGE: "true"
      WATCHTOWER_LOG_LEVEL: "info"
      WATCHTOWER_WARN_ON_HEAD_FAILURE: "never"
      WATCHTOWER_INCLUDE_STOPPED: "false" # Update stopped containers
      WATCHTOWER_INCLUDE_RESTARTING: "true" # Restart containers after update
      WATCHTOWER_ROLLING_RESTART: "false"
      WATCHTOWER_LOG_FORMAT: "pretty"
      WATCHTOWER_SCHEDULE: 0 0/15 1 * * ? #Daily (?) at every 15th minute (0/15) from 0 (0) through 59 past hour 1am (1)
      WATCHTOWER_TIMEOUT: 15s
      WATCHTOWER_NOTIFICATIONS_LEVEL: info
      WATCHTOWER_NOTIFICATIONS_HOSTNAME: "My host"
    networks:
      my_bridge:
    depends_on:
      socket:
        condition: service_started
    labels: 
      monocker.enable: $MONOCKER_ENABLE
    image: 'nickfedor/watchtower:latest'
```
</details>

[🔼 Back to top](#docker-related)


# wDOSg
<details>
  <summary>
    Games can be downloaded from <a href=https://dosgames.com/> here </a href>
  </summary>

```
  wdosg:
    container_name: wdosg
    restart: $RESTART_POLICY
    hostname: wdosg
    environment:
      - TOKEN_SECRET=$APP_KEY
      - TWITCH_CLIENT_ID=$TWITCH_CLIENT_ID # Your IGDB (Twitch) client ID
      - TWITCH_CLIENT_SECRET=$TWITCH_CLIENT_SECRET #Your IGDB (Twitch) client secret
      - EMAIL_SERVICE=gmail
      - EMAIL_USER=$GM_USER 
      - EMAIL_PASS=$GM_PSW
      - SERVER_FRIENDLY_URL=https://wdosg.$DOMAINNAME
      # - TWITCH_APP_ACCESS_TOKEN= # Your IGDB (Twitch) Token - **NOT your secret**
    volumes:
      - $MEDIA_PATH/Games:/app/wdosglibrary # directory containing your library
      - $PERSIST/wdosg:/app/database # directory containing your database
    ports:
      - 3003:3001
    networks:
      my_bridge:
    image: 'soulraven1980/wdosg:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# YTPTube
<details>
  <summary>
  </summary>
  
```
  ytptube:
    container_name: ytptube
    hostname: ytptube
    restart: $RESTART_POLICY
    environment:
      - TZ=$TZ
      - YTP_INSTANCE_TITLE="MyTube"
      - YTP_TEMP_PATH=/downloads/tmp
      - YTP_DOWNLOAD_PATH=/downloads
      - YTP_FLARESOLVERR_URL=http://192.168.1.11:8191/v1
      - YTP_AUTH_USERNAME=admin
      - YTP_AUTH_PASSWORD=$YTPTUBE_PASSWORD
    networks:
      my_bridge:
    ports:
      - 8081:8081
    volumes:
      - $PERSIST/ytptube:/config:rw
      - $DOWNLOADS/YouTube:/downloads:rw
    user: $PUID:$PGID
    image: 'ghcr.io/arabcoders/ytptube:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Zigbee2MQTT
<details>
  <summary>
  </summary>

```
  zigbee2mqtt:
    container_name: zigbee2mqtt
    restart: $RESTART_POLICY
    hostname: zigbee2mqtt
    environment:
      - TZ=$TZ
    volumes:
      - $PERSIST/zigbee2mqtt:/app/data
      # - /run/udev:/run/udev:ro
    ports:
      - 9002:8080
    networks:
      my_bridge:
    # devices:
    #   - /dev/ttyUSB0:/dev/ttyUSB0
    depends_on:
      - mqtt
    labels: 
      monocker.enable: $MONOCKER_ENABLE
    image: 'koenkk/zigbee2mqtt:latest'
```
</details>

[🔼 Back to top](#programming)

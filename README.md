

This document is intended to provide a `docker-compose` of the Docker Containers I am using on my Synology DS1513+ home server. If you are using a differnet device, that should be fine as well, just make sure you define the `$PERSIST` location in your `.env` file that corresponds to the docker folder on your host device.

If you are not familiar on how to use a compose file, you can change the compose to docker CLI command by using a [docker-compose Converter](https://bucherfa.github.io/dcc-web/)
 

Below are some ports that I have found to be reserved and cannot be used on my host, I always avoid using them

##### A CONSOLIDATED `docker-compose.yml` FILE CAN BE FOUND [HERE](docker-compose.yml) , AND TO START, JUST TYPE IN `docker-compose up -d` AND ALL CONTAINERS WILL START. NOTE THAT SOME CONTAIENRS REQUIRES CONFIGURATION BEFORE STARTTING, AS WELL AS MAKING SURE ALL REQUIRED FOLDERS ARE CREATED ON THE HOST MACHINE WITH THE CORRESPONDING NAMES AS IN THE COMPOSE 


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
 
| Container                                                          | Port            | Description                                          | Docker Image                           |
| :------------                                                      | :----           | :-------                                             | :---                                   |
| <a href="#adminer">Adminer</a>                                     | 3330            | Full-featured database management tool               | `adminer:latest`                       |
| <a href="#authelia-redis">Authelia-Redis</a>                       | 6379            | Used for Authelia                                    | `redis:alpine`                         |
| <a href="#databases-backup">Databases-Backup</a>                   | 6379            | Backups All Redis, MariaDB and Postgres using CRON   | `tiredofit/db-backup:latest`           |
| <a href="#focalboard-postgres">FocalBoard-Postgres</a>             | -               | Used for FocalBoard                                  | `postgres:alpine`                      |
| <a href="#influxdb">InfluxDB</a>                                   | 3004            | Time series database                                 | `influxdb:alpine`                      |
| <a href="#invidious-postgres">Invidious-Postgres</a>               | -               | Used for Invidious                                   | `postgres:alpine`                      |
| <a href="#jellystat-postgres">JellyStat-Postgres</a>               | -               | Used for JellyStat                                   | `postgres:15.2`                        |
| <a href="#loki">Loki</a>                                           | 3002            | Multi-tenant log aggregation system                  | `grafana/loki:latest`                  |
| <a href="#mariadb">MariaDB</a>                                     | 3306            | MariaDB Database (MySQL Clone)                       | `jbergstroem/mariadb-alpine:10.6.13`   |
| <a href="#promtail">Promtail</a>                                   | -               | Sending log data to Loki                             | `grafana/promtail:latest`              |



# DOCKER RELATED

| Container                                                          | Port            | Description                                          | Docker Image                           |
| :------------                                                      | :----           | :-------                                             | :---                                   |
| <a href="#auto-heal">Autoheal</a>                                  | -               | Monitor and restart unhealthy docker containers      | `modem7/docker-autoheal:latest`        |
| <a href="#diun">Diun</a>                                           | -               | Docker images status monitor and notifier using CRON | `crazymax/diun:latest`                 |
| <a href="#socket-proxy">Docker Socket Proxy</a>                    | 2375            | A security-enhanced proxy for the Docker Socket      | `tecnativa/docker-socket-proxy:latest` |
| <a href="#dozzle">Dozzle</a>                                       | 9900            | Container log aggregator                             | `pamir20/dozzle:latest`                |
| <a href="#duplicati">Duplicati</a>                                 | 8200            | Backup tool for local files (used for docker mainly) | `ghcr.io/imagegenius/duplicati:latest` |
| <a href="#yopass-memcached">Yopass-MemCached</a>                   | 11211           | General-purpose caching system (mainly for YoPass)   | `memcached:alpine`                     |
| <a href="#monocker">Monocker</a>                                   | -               | Live MONitor doCKER with notifications               | `petersem/monocker:latest`             |
| <a href="#portainer-ee">Portainer-EE</a>                           | 9000,9443,8000  | Docker management tool (Business Edition)            | `portainer/portainer-ee:alpine`        |
| <a href="#watchtower">WatchTower</a>                               | -     | Auto-Update containers with latest images                      | `containrrr/watchtower:latest`         |
| <a href="#container-webtty">Container-WebTTY</a>                   | 8818,8090       | SContainer WebTTY terminal access                    | `wrfly/container-web-tty:0.1.10`       |



# LINKS AND PAGE ORGANISATION

| Container                                                          | Port            | Description                                          | Docker Image                           |
| :------------                                                      | :----           | :-------                                             | :---                                   |
| <a href="#homarr">Homarr</a>                                       | 5050            | Links manager                                        | `ghcr.io/ajnart/homarr:latest`         |



# MEDIA PLAYING

| Container                                                          | Port            | Description                                          | Docker Image                           |
| :------------                                                      | :----           | :-------                                             | :---                                   |
| <a href="#jellyfin">Jellyfin</a>                                   | 8096            | Media server                                         | `jellyfin/jellyfin:latest`             |
| <a href="#jellystat">JellyStat</a>                                 | 6555            | Stat web interface for Jellyfin Media server         | `cyfershepard/jellystat:unstable`      |
| <a href="#navidrome">NaviDrome</a>                                 | 4533            | Web-based music collection server and streamer       | `ghcr.io/navidrome/navidrome:latest`   |
| <a href="#maloja">Maloja</a>                                       | 42010           | Music scrobble for personal listening statistics     | `krateng/maloja:latest`        |



# NETWORKING AND SECURITY

| Container                                                          | Port            | Description                                          | Docker Image                           |
| :------------                                                      | :----           | :-------                                             | :---                                   |
| <a href="#adguard">AdGuardHome</a>                                 | 80, 3000        | Network-wide software for blocking ads & tracking    | `adguard/adguardhome:latest`           |
| <a href="#authelia">Authelia</a>                                   | 9091            | 2Factor Authentication                               | `authelia/authelia:latest`             |
| <a href="#cloudflared">Cloudflared</a>                             | -               | A secure way to publibly connect to local network    | `cloudflare/cloudflared:latest`        |
| <a href="#cloudflared-mon">Cloudflared-Mon</a>                     | -               | Cloudflare Zero Tunnel health monitor                | `techblog/cloudflared-mon:latest`      |
| <a href="#dashdot">Dash.</a>                                         | 7512            | Modern server dashboard monitor for the host         | `mauricenino/dashdot:latest`         |
| <a href="#ddns-updater">DDNS Updater</a>                           | 8002            | Dynamic DNS updater for mulit-DDNS services in one   | `qmcgaw/ddns-updater:latest`           |
| <a href="#kea">Kea</a>                                             | -               | A DHCP server to assign static IPs (no UI)           | `jonasal/kea-dhcp4:2`                  |
| <a href="#uptime-kuma">Uptime-Kuma</a>                             | 3001            | Monitoring tool for local DNS                        | `louislam/uptime-kuma:latest`          |
| <a href="#vaultwarden-backup">VaultWarden-Backup</a>               | -               | Backs up database for VaultWarden                    | `bruceforce/vaultwarden-backup:latest` |



# PROGRAMMING

| Container                                                          | Port            | Description                                          | Docker Image                           |
| :------------                                                      | :----           | :-------                                             | :---                                   |
| <a href="#code-server">Code-Server</a>                             | 8181            | Visual Studio Code editor                            | `lscr.io/linuxserver/code-server:latest` |
| <a href="#esphome">ESPHome</a>                                     | 6052,6123       | Programming tool for ESP82 chipsets                  | `ghcr.io/imagegenius/esphome:latest`   |
| <a href="#tasmoadmin">TasmoAdmin</a>                               | 9999            | Manages Sonoff Devices flashed with Tasmota          | `raymondmm/tasmoadmin:latest`          |
| <a href="#tasmobackup">TasmoBackup</a>                             | 8259            | Backups/manages Sonoff Devices flashed with Tasmota  | `danmed/tasmobackupv1:latest`          |



# SYSTEM MONITORING AND MANAGEMENT

| Container                                                          | Port            | Description                                          | Docker Image                           |
| :------------                                                      | :----           | :-------                                             | :---                                   |
| <a href="#cloud-commander">Cloud Commander</a>                     | 4569            | A file manager for the web with console and editor   | `coderaiser/cloudcmd:latest-alpine`    |
| <a href="#homeassistant">HomeAssistant</a>                         | 8123            | Smart home monitoring and management                 | `ghcr.io/home-assistant/home-assistant:stable`|
| <a href="#scrutiny">Scrutiny</a>                                   | 6080            | WebUI for S.M.A.R.T monitoring                       | `ghcr.io/analogj/scrutiny:master-omnibus` |



# SELF-HOSTED

| Container                                                          | Port            | Description                                          | Docker Image                           |
| :------------                                                      | :----           | :-------                                             | :---                                   |
| <a href="#apprise">Apprise</a>                                     | 8001            | Push notification using POST                         | `lscr.io/linuxserver/apprise-api:latest` |
| <a href="#dufs">Dufs</a>                                           | 4688            | Utility file server that supports webdav             | `sigoden/dufs:latest`                  |
| <a href="#excalidraw">ExcaliDraw</a>                               | 3765            | Virtual whiteboard for sketching hand-drawn like diagrams | `excalidraw/excalidraw:latest`    |
| <a href="#focalboard">FocalBoard</a>                               | 5374            | A project management tool                            | `mattermost/focalboard:latest`         |
| <a href="#gitea">Gitea</a>                                         | 222,3333        | Git with a cup of tea!                               | `gitea/gitea:latest`                   |
| <a href="#invidious">Invidious</a>                                 | 7601            | An open source alternative front-end to YouTube      | `quay.io/invidious/invidious:latest`   |
| <a href="#librey">LibreY</a>                                       | 8245            | Free privacy respecting meta search engine Google Alternative | `ghcr.io/ahwxorg/librey:latest` |
| <a href="#memos">Memos</a>                                         | 5230            | Capture and share your great thoughts                | `neosmemo/memos:latest`                |
| <a href="#metube">MeTube</a>                                       | 8081            | Web GUI for youtube-dl with playlist support         | `ghcr.io/alexta69/metube:latest`       |
| <a href="#pastefy">PasteFy</a>                                     | 9980            | Pastebin                                             | `interaapps/pastefy:latest`            |
| <a href="#pialert">PiAlert</a>                                     | 20211           | Monitoring WIFI/LAN and alerting of new devices      | `jokobsk/pi.alert:latest`              |
| <a href="#projectsend">ProjectSend</a>                             | 8516            | Clients-oriented, private file sharing web application | `lscr.io/linuxserver/projectsend:latest` |
| <a href="#qr-code">QR Code</a>                                     | 8895            | UI to generate a QR Code from a provided URL         | `bizzycolah/qrcode-generator:latest`   |
| <a href="#squoosh">Squoosh</a>                                     | 7701            | Ultimate image optimiser with compress and compare   | `dko0/squoosh:latest`                  |
| <a href="#syncthing">SyncThing</a>                                 | 8384            | Syncing platfrom between Mobile-NAS                  | `syncthing/syncthing:1.26 `            |
| <a href="#traccar">TracCar</a>                                     | 8082,5055       | GPS Tracking System                                  | `traccar/traccar:alpine`               |
| <a href="#vaultwarden">VaultWarden</a>                             | 8089,3012       | Password management application                      | `vaultwarden/server:alpine`            |
| <a href="#yopass">YoPass</a>                                       | 8180            | Share Secrets Securely                               | `jhaals/yopass:latest`                 |



To begin with, I defined my own networks to use, that makes my setup easier for IP allocation of some containers.

```
version: '3'

networks:

# Bridge Network
  tamimology_bridge:
    name: tamimology_bridge
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


No assuming that every `docker-compose` file you use, will ghave the standard parameters on top, then you just paste each compose below that corresponds to your need. I have included those into the network definition above.

You can add them to each file seperatley if needed. I assume that you will be using 1 file for all of the containers, and then `docker-compose up -d CONTAINER_NAME` to start every single contaniner. You can add as many CONTAINER_NAME as needed i.e.

`docker-compose up -d adguard` will start the Adguard container only. If you use `docker-compose up -d` then all containers in the compose file will be started in one command.

```
version: '3'

services:
```

# AdGuard

<details>
  <summary>
  </summary>

```
  adguard:
    container_name: adguard
    restart: $ALWAYS_ON_POLICY
    hostname: adguard
    environment:
      - TZ=$TZ
    volumes:
      - $PERSIST/adguard/conf:/opt/adguardhome/conf 
      - $PERSIST/adguard/work:/opt/adguardhome/work 
      - $PERSIST/adguard/certs:/certs
    ports:
      - 53/udp 
      - 67/udp 
      - 68/tcp 
      - 68/udp 
      - 80/tcp 
      - 443/tcp 
      - 853/tcp 
      - 3000/tcp 
    networks:
      tamimology_bridge:
         ipv4_address: $BRIDGE_NET.202
      dockervlan:
        ipv4_address: $MACVLAN_NET.5 # IP address inside defined $MACVLAN_RANGE range
    security_opt:
      - no-new-privileges:true
    labels: 
      autoheal: $AUTOHEAL_RESTART
      monocker.enable: $MONOCKER_ENABLE
    image: 'adguard/adguardhome:latest'
```
</details>

[🔼 Back to top](#networking-and-security)


# Adminer

<details>
  <summary>
  </summary>

```
  adminer:
    container_name: adminer
    restart: $RESTART_POLICY
    hostname: adminer
    environment:
      ADMINER_DEFAULT_DB_DRIVER: mysql
      ADMINER_DEFAULT_SERVER: mariadb #172.18.0.5 #mariadb ip address
      ADMINER_DESIGN: 'nette' #'esterka'
      ADMINER_PLUGINS: 'tables-filter tinymce frames'
    ports:
      - 3330:8080
    networks:
      tamimology_bridge:
    depends_on:
      - mariadb
    image: 'adminer:latest'
```
</details>

[🔼 Back to top](#databases)


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
      tamimology_bridge:
    ports:
      - 8001:8000
    volumes:
      - $PERSIST/apprise:/config
    image: 'lscr.io/linuxserver/apprise-api:latest'
```
</details>

[🔼 Back to top](#self-hosted)


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
      tamimology_bridge:
    healthcheck:
      disable: true
    depends_on:
      authelia-redis:
        condition: service_started
      # - mariadb #enable if using mariadb in config.yaml
    labels: 
      monocker.enable: $MONOCKER_ENABLE
    logging:
      driver: loki
      options:
        loki-url: "http://$LOCAL_HOST:3002/loki/api/v1/push"
    image: 'authelia/authelia:latest' #4.36'
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
      tamimology_bridge:
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


# Auto-Heal
<details>
  <summary>
  </summary>

```
  autoheal:
    container_name: autoheal
    restart: $ALWAYS_ON_POLICY
    hostname: autoheal
    environment:
      - AUTOHEAL_CONTAINER_LABEL=autoheal #all
      - DOCKER_SOCK=$DOCKER_HOST
      - APPRISE_URL=http://$LOCAL_HOST:8001/notify/autoheal
      - AUTOHEAL_INTERVAL=5   # check every 5 seconds
      - AUTOHEAL_START_PERIOD=600   # wait 10 minutes before first health check
      - AUTOHEAL_DEFAULT_STOP_TIMEOUT=60   # Docker waits max 10 seconds (the Docker default) for a container to stop before killing during restarts (container overridable via label, see below)
      - CURL_TIMEOUT=30     # --max-time seconds for curl requests to Docker API
      # - POST_RESTART_SCRIPT=""    # Run the specified script if a container was re
    volumes:
      - $LOCAL_TIME:/etc/localtime:ro
    networks:
      tamimology_bridge:
    image: 'modem7/docker-autoheal:latest'
```
</details>

[🔼 Back to top](#docker-related)


# Cloud Commander
<details>
  <summary>
  </summary>

```
  cloudcmd:
    container_name: cloudcmd
    restart: $RESTART_POLICY
    hostname: cloudcmd
    environment:
      NAME: "Tamimology"
    volumes:
      - /volume1/:/volume1
      - /volumeUSB1/:/USB1
      # - /:/mnt/fs
    working_dir: /volume1
    tty: true
    ports:
      - 4569:8000
    networks:
      tamimology_bridge:
    image: 'coderaiser/cloudcmd:latest-alpine'
```
</details>

[🔼 Back to top](#system-monitoring-and-management)


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
      tamimology_bridge:
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
      tamimology_bridge:
    image: 'techblog/cloudflared-mon:latest'
```
</details>

[🔼 Back to top](#networking-and-security)


# Code-Server
<details>
  <summary>
  </summary>

```
  code-server:
    container_name: code-server
    restart: $RESTART_POLICY
    hostname: code-server
    environment:
      - PUID=$PUID
      - PGID=$PGID
      - TZ=$TZ
      - SUDO_PASSWORD_HASH=$PASSWORD_HASH #https://www.symbionts.de/tools/hash/sha256-hash-salt-generator.html
      - PROXY_DOMAIN=code-server.$DOMAINNAME
      - DEFAULT_WORKSPACE=/home/coder/project #optional
    volumes:
      - $PERSIST/code-server:/config
      - $PERSIST/homeassistant:/home/coder/project:rw
    ports:
      - 8181:8443
    networks:
      tamimology_bridge:
    image: 'lscr.io/linuxserver/code-server:latest'
```
</details>

[🔼 Back to top](#programming)


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
      - DASHDOT_PAGE_TITLE=Tamimology Server Monitor
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
      tamimology_bridge:
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

    # authelia-redis-backup
      - DB01_TYPE=redis
      - DB01_HOST=authelia-redis
      - DB01_PORT=6379
      - DB01_BACKUP_LOCATION=FILESYSTEM
      - DB01_FILESYSTEM_PATH=/backup/redis/authelia
      - DB01_BACKUP_INTERVAL=1440 #once per day
      - DB01_BACKUP_BEGIN="2355" # @23:55 midnight
      - DB01_CLEANUP_TIME=8640 # keep for 6 days
      - DB01_CHECKSUM=SHA1
      - DB01_COMPRESSION=ZSTD
      - DB01_COMPRESSION_LEVEL=10
      - DB01_SPLIT_DB=false
      - DB01_MYSQL_SINGLE_TRANSACTION=true

    # homeassistant-mariadb-backup
      - DB02_TYPE=mariadb
      - DB02_HOST=mariadb
      - DB02_PORT=3306
      - DB02_NAME=homeassistant
      - DB02_USER=homeassistant
      - DB02_PASS=$DB_PASSWORD
      - DB02_BACKUP_LOCATION=FILESYSTEM
      - DB02_FILESYSTEM_PATH=/backup/mariadb/homeassistant
      - DB02_BACKUP_INTERVAL=1440 #once per day
      - DB02_BACKUP_BEGIN="0000" # @00:00 midnight
      - DB02_CLEANUP_TIME=8640 # keep for 6 days
      - DB02_CHECKSUM=SHA1
      - DB02_COMPRESSION=ZSTD
      - DB02_COMPRESSION_LEVEL=10
      - DB02_SPLIT_DB=false
      - DB02_MYSQL_SINGLE_TRANSACTION=true

    # mariadb-sys-backup
      - DB03_TYPE=mariadb
      - DB03_HOST=mariadb
      - DB03_PORT=3306
      - DB03_NAME=all
      - DB03_NAME_EXCLUDE=homeassistant,pastefy,projectsend,traccar,tasmobackupdb
      - DB03_USER=root
      - DB03_PASS=$DB_PASSWORD
      - DB03_BACKUP_LOCATION=FILESYSTEM
      - DB03_FILESYSTEM_PATH=/backup/mariadb/sys
      - DB03_BACKUP_INTERVAL=1440 #once per day
      - DB03_BACKUP_BEGIN="0005" # @00:05 midnight
      - DB03_CLEANUP_TIME=8640 # keep for 6 days
      - DB03_CHECKSUM=SHA1
      - DB03_COMPRESSION=ZSTD
      - DB03_COMPRESSION_LEVEL=10
      - DB03_SPLIT_DB=false
      - DB03_MYSQL_SINGLE_TRANSACTION=true

    # pastefy-mariadb-backup
      - DB04_TYPE=mariadb
      - DB04_HOST=mariadb
      - DB04_PORT=3306
      - DB04_NAME=pastefy
      - DB04_USER=pastefy
      - DB04_PASS=$DB_PASSWORD
      - DB04_BACKUP_LOCATION=FILESYSTEM
      - DB04_FILESYSTEM_PATH=/backup/mariadb/pastefy
      - DB04_BACKUP_INTERVAL=1440 #once per day
      - DB04_BACKUP_BEGIN="0010" # @00:10 midnight
      - DB04_CLEANUP_TIME=8640 # keep for 6 days
      - DB04_CHECKSUM=SHA1
      - DB04_COMPRESSION=ZSTD
      - DB04_COMPRESSION_LEVEL=10
      - DB04_SPLIT_DB=false
      - DB04_MYSQL_SINGLE_TRANSACTION=true

    # projectsend-mariadb-backup
      - DB05_TYPE=mariadb
      - DB05_HOST=mariadb
      - DB05_PORT=3306
      - DB05_NAME=projectsend
      - DB05_USER=projectsend
      - DB05_PASS=$DB_PASSWORD
      - DB05_BACKUP_LOCATION=FILESYSTEM
      - DB05_FILESYSTEM_PATH=/backup/mariadb/projectsend
      - DB05_BACKUP_INTERVAL=1440 #once per day
      - DB05_BACKUP_BEGIN="0015" # @00:15 midnight
      - DB05_CLEANUP_TIME=8640 # keep for 6 days
      - DB05_CHECKSUM=SHA1
      - DB05_COMPRESSION=ZSTD
      - DB05_COMPRESSION_LEVEL=10
      - DB05_SPLIT_DB=false
      - DB05_MYSQL_SINGLE_TRANSACTION=true

    # traccar-mariadb-backup
      - DB06_TYPE=mariadb
      - DB06_HOST=mariadb
      - DB06_PORT=3306
      - DB06_NAME=traccar
      - DB06_USER=traccar
      - DB06_PASS=$DB_PASSWORD
      - DB06_BACKUP_LOCATION=FILESYSTEM
      - DB06_FILESYSTEM_PATH=/backup/mariadb/traccar
      - DB06_BACKUP_INTERVAL=1440 #once per day
      - DB06_BACKUP_BEGIN="0020" # @00:20 midnight
      - DB06_CLEANUP_TIME=8640 # keep for 6 days
      - DB06_CHECKSUM=SHA1
      - DB06_COMPRESSION=ZSTD
      - DB06_COMPRESSION_LEVEL=10
      - DB06_SPLIT_DB=false
      - DB06_MYSQL_SINGLE_TRANSACTION=true

    # tasmobackup-mariadb-backup
      - DB07_TYPE=mariadb
      - DB07_HOST=mariadb
      - DB07_PORT=3306
      - DB07_NAME=tasmobackupdb
      - DB07_USER=tasmobackup
      - DB07_PASS=$DB_PASSWORD
      - DB07_BACKUP_LOCATION=FILESYSTEM
      - DB07_FILESYSTEM_PATH=/backup/mariadb/tasmobackup
      - DB07_BACKUP_INTERVAL=1440 #once per day
      - DB07_BACKUP_BEGIN="0025" # @00:25 midnight
      - DB07_CLEANUP_TIME=8640 # keep for 6 days
      - DB07_CHECKSUM=SHA1
      - DB07_COMPRESSION=ZSTD
      - DB07_COMPRESSION_LEVEL=10
      - DB07_SPLIT_DB=false
      - DB07_MYSQL_SINGLE_TRANSACTION=true

    # invidious-postgres-backup
      - DB08_TYPE=pgsql
      - DB08_HOST=invidious-postgres
      - DB08_USER=invidious
      - DB08_AUTH=invidious
      - DB08_PASS=$DB_PASSWORD
      - DB08_NAME=invidious
      - DB08_BACKUP_LOCATION=FILESYSTEM
      - DB08_FILESYSTEM_PATH=/backup/postgres/invidious
      - DB08_SPLIT_DB=false
      - DB08_BACKUP_INTERVAL=1440 #once per day
      - DB08_BACKUP_BEGIN="0030" # @00:30 midnight
      - DB08_CLEANUP_TIME=8640 # keep for 6 days
      - DB08_COMPRESSION=ZSTD
      - DB08_COMPRESSION_LEVEL=10
      - DB08_CHECKSUM=SHA1
      - DB08_MYSQL_SINGLE_TRANSACTION=true

    # jellystat-postgres-backup
      - DB09_TYPE=pgsql
      - DB09_HOST=jellystat-postgres
      - DB09_USER=jellystat
      - DB09_AUTH=jellystat
      - DB09_PASS=$DB_PASSWORD
      - DB09_NAME=jfstat
      - DB09_BACKUP_LOCATION=FILESYSTEM
      - DB09_FILESYSTEM_PATH=/backup/postgres/jellystat
      - DB09_SPLIT_DB=false
      - DB09_BACKUP_INTERVAL=1440 #once per day
      - DB09_BACKUP_BEGIN="0035" # @00:35 midnight
      - DB09_CLEANUP_TIME=8640 # keep for 6 days
      - DB09_COMPRESSION=ZSTD
      - DB09_COMPRESSION_LEVEL=10
      - DB09_CHECKSUM=SHA1
      - DB09_MYSQL_SINGLE_TRANSACTION=true

    # focalboard-postgres-backup
      - DB10_TYPE=pgsql
      - DB10_HOST=focalboard-postgres
      - DB10_USER=focalboard
      - DB10_AUTH=focalboard
      - DB10_PASS=$DB_PASSWORD
      - DB10_NAME=focalboard
      - DB10_BACKUP_LOCATION=FILESYSTEM
      - DB10_FILESYSTEM_PATH=/backup/postgres/focalboard
      - DB10_SPLIT_DB=false
      - DB10_BACKUP_INTERVAL=1440 #once per day
      - DB10_BACKUP_BEGIN="0040" # @00:40 midnight
      - DB10_CLEANUP_TIME=8640 # keep for 6 days
      - DB10_COMPRESSION=ZSTD
      - DB10_COMPRESSION_LEVEL=10
      - DB10_CHECKSUM=SHA1
      - DB10_MYSQL_SINGLE_TRANSACTION=true
    networks:
      tamimology_bridge:
    links:
     - authelia-redis
     - mariadb
     - invidious-postgres
    #  - jellystat-postgres
    #  - focalboard-postgres
    labels: 
      # autoheal: $AUTOHEAL_RESTART
      monocker.enable: $MONOCKER_ENABLE
    image: 'tiredofit/db-backup:latest'
```
</details>

[🔼 Back to top](#databases)


# DDNS-Updater
<details>
  <summary>
  </summary>

```
  ddns-updater:
    container_name: ddns-updater
    restart: $ALWAYS_ON_POLICY
    hostname: ddns-updater
    environment:
      - TZ=$TZ
      - PUID=$PUID
      - PGID=$PGID
      # - USER="0" #run the container as root 
      - PERIOD=5m
      - UPDATE_COOLDOWN_PERIOD=5m
      - PUBLICIP_FETCHERS=all
      - PUBLICIP_HTTP_PROVIDERS=all
      - PUBLICIPV4_HTTP_PROVIDERS=all
      - PUBLICIPV6_HTTP_PROVIDERS=all
      - PUBLICIP_DNS_PROVIDERS=all
      - PUBLICIP_DNS_TIMEOUT=3s
      - HTTP_TIMEOUT=10s
      - LISTENING_PORT=8000
      - HEALTH_SERVER_ADDRESS=127.0.0.1:9999
      - ROOT_URL=/
      - BACKUP_PERIOD=24h # 0 to disable
      - BACKUP_DIRECTORY=/updater/data
      - LOG_LEVEL=info
      - LOG_CALLER=hidden
      - SHOUTRRR_ADDRESSES=pushover://shoutrrr:$PUSHOVER_DDNS_API@$PUSHOVER_USER_KEY
    volumes:
      - $PERSIST/ddns-updater:/updater/data
    networks:
      tamimology_bridge:
    ports:
      - 8002:8000/tcp
    user: $PUID:$PGID
    labels: 
      autoheal: $AUTOHEAL_RESTART
      autoheal.stop.timeout: 30
      monocker.enable: $MONOCKER_ENABLE
    image: 'qmcgaw/ddns-updater:latest'
```
</details>

[🔼 Back to top](#networking-and-security)


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
      tamimology_bridge:
    command: serve
    image: 'crazymax/diun:latest'
```
</details>

[🔼 Back to top](#docker-related)


# Dozzle
<details>
  <summary>
  </summary>

```
  dozzle:
    container_name: dozzle
    restart: $RESTART_POLICY
    hostname: dozzle
    environment:
      DOCKER_HOST: $DOCKER_HOST
      DOZZLE_LEVEL: info
      DOZZLE_TAILSIZE: 300
      DOZZLE_FILTER: "status=running"
      DOZZLE_USERNAME: admin
      DOZZLE_PASSWORD: $DOZZLE_PWD
      # DOZZLE_FILTER: "label=log_me" # limits logs displayed to containers with this label
    # volumes:
    #   - $DOCKER_SOCKET:/var/run/docker.sock
    ports:
      - 9900:8080
    networks:
      tamimology_bridge:
    healthcheck:
      test: [ "CMD", "/dozzle", "healthcheck" ]
      interval: 3s
      timeout: 30s
      retries: 5
      start_period: 30s
    labels: 
      autoheal: $AUTOHEAL_RESTART
    image: 'amir20/dozzle:latest'
```
</details>

[🔼 Back to top](#docker-related)


# Dufs
<details>
  <summary>
  </summary>

```
  dufs:
    container_name: dufs
    restart: $RESTART_POLICY
    hostname: dufs
    volumes:
      - $PERSIST/dufs:/data
    networks:
      tamimology_bridge:
    ports:
      - 4688:5000
    command: /data -A
    tty: true
    image: 'sigoden/dufs:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Duplicati
<details>
  <summary>
  </summary>

```
  duplicati:
    container_name: duplicati
    restart: $ALWAYS_ON_POLICY
    hostname: duplicati
    environment:
      - PUID=$PUID
      - PGID=$PGID
      - TZ=$TZ
      # https://forum.duplicati.com/t/how-to-configure-automatic-email-notifications-via-gmail-for-every-backup-job/869/19
      - CLI_ARGS= --send-mail-url=smtp://$GM_HOST:587/?starttls=when-available
                  --send-mail-any-operation=true
                  --send-mail-subject=Duplicati %PARSEDRESULT%, %OPERATIONNAME% report for %backup-name%
                  --send-mail-to=$PUSHOVER_EMAIL_DUPLICATI
                  --send-mail-username=$GM_USER
                  --send-mail-password=$GM_PSW
                  --send-mail-from=Tamimology Server <$GM_USER>
                  --send-mail-level=Success,Error
      - PRIVILEGED=true
    volumes:
      - $PERSIST/duplicati:/config
      - $PERSIST/:/source/docker
      - $BACKUPS/:/source/backups
    ports:
      - 8200:8200
    networks:
      tamimology_bridge:
    labels: 
      monocker.enable: $MONOCKER_ENABLE
    image: 'ghcr.io/imagegenius/duplicati:latest'
```
</details>

[🔼 Back to top](#docker-related)


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
      tamimology_bridge:
    volumes:
      - $PERSIST/esphome/:/config:rw
   #devices:
   #  - /dev/ttyUSB0
    working_dir: /config
    image: 'ghcr.io/imagegenius/esphome:latest' #alpine version
```
</details>

[🔼 Back to top](#programming)


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
      tamimology_bridge:
    labels: 
      autoheal: $AUTOHEAL_RESTART
    healthcheck:
     test: curl -f http://localhost:80/ || exit 1
    stdin_open: true
    environment:
      - NODE_ENV=production
    image: 'excalidraw/excalidraw:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# FocalBoard
<details>
  <summary>
  </summary>

```
  focalboard:
    container_name: focalboard
    restart: $RESTART_POLICY
    hostname: focalboard
    environment:
      - VIRTUAL_HOST=localhost
      - VIRTUAL_PORT=8000
      - VIRTUAL_PROTO=http
    volumes:
      - $PERSIST/focalboard/data:/opt/focalboard/data:rw
      - $PERSIST/focalboard/config/config.json:/opt/focalboard/config.json
    ports:
      - 5374:8000
    networks:
      tamimology_bridge:
    security_opt:
      - no-new-privileges:true
    depends_on:
      focalboard-postgres:
        condition: service_healthy
    image: 'mattermost/focalboard:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# FocalBoard-Postgres
<details>
  <summary>
  </summary>

```
  focalboard-postgres:
    container_name: focalboard-postgres
    restart: $RESTART_POLICY
    hostname: focalboard-postgres
    environment:
      - TZ=$TZ
      - POSTGRES_DB=focalboard
      - POSTGRES_USER=focalboard
      - POSTGRES_PASSWORD=$DB_PASSWORD
      - PGDATA=/var/lib/postgresql/data
    volumes:
      - $PERSIST/focalboard/db:/var/lib/postgresql/data:rw
    networks:
      tamimology_bridge:
    security_opt:
      - no-new-privileges:true
    healthcheck:
      test: ["CMD", "pg_isready", "-q", "-d", "focalboard", "-U", "focalboard"]
      timeout: 45s
      interval: 10s
      retries: 10
    user: $PUID:$PGID
    labels: 
      autoheal: $AUTOHEAL_RESTART
    image: 'postgres:alpine'
```
</details>

[🔼 Back to top](#databases)


# Gitea
<details>
  <summary>
  </summary>

```
  gitea:
    container_name: gitea
    restart: $RESTART_POLICY
    hostname: gitea
    environment:
      - USER_UID=$PUID
      - USER_GID=$PGID
      - ROOT_URL=https://gitea.$DOMAINNAME
    volumes:
      - $PERSIST/gitea:/data
      - $LOCAL_TIME:/etc/localtime:ro
      - $TIME_ZONE:/etc/TZ:ro
    ports:
      - 3333:3000
      - 222:22
    networks:
      tamimology_bridge:
    healthcheck:
      test: wget --no-verbose --tries=1 --spider http://localhost:3000/ || exit 1
    labels: 
      autoheal: $AUTOHEAL_RESTART
      monocker.enable: $MONOCKER_ENABLE
    image: 'gitea/gitea:latest'
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
      - DOCKER_HOST=$DOCKER_HOST
      - EDIT_MODE_PASSWORD=$DASH_PWD
      - DEFAULT_COLOR_SCHEME=dark
      # - DISABLE_EDIT_MODE=TRUE
      - BASE_URL=https://homarr.$DOMAINNAME/ # to access via https://mydomain.net/BASE_URL instead of https://BASE_URL.mydomain.net, mainly for reverse proxies
    volumes:
      - $PERSIST/homarr/configs:/app/data/configs
      - $PERSIST/homarr/data:/data
      - $PERSIST/homarr/icons:/app/public/icons
      # - $DOCKER_SOCKET:/var/run/docker.sock:ro
    ports:
      - 7575:7575
    networks:
      tamimology_bridge:
    labels: 
      autoheal: $AUTOHEAL_RESTART
      monocker.enable: $MONOCKER_ENABLE
    image: 'ghcr.io/ajnart/homarr:latest'
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
      # - $DOCKER_SOCKET:/var/run/docker.sock
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
      # autoheal: $AUTOHEAL_RESTART
      # autoheal.stop.timeout: 240 # 4min #900 #15 min
      monocker.enable: $MONOCKER_ENABLE
    healthcheck:
      test: curl -fSs http://127.0.0.1:8123 || exit 1
      start_period: 90s
      timeout: 10s
      interval: 5s
      retries: 3
    network_mode: host
    working_dir: /config
    depends_on:
       - mariadb
       - influxdb
    logging:
      driver: loki
      options:
        loki-url: "http://$LOCAL_HOST:3002/loki/api/v1/push"
    image: 'ghcr.io/home-assistant/home-assistant:stable'
```
</details>

[🔼 Back to top](#system-monitoring-and-management)


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
       - DOCKER_INFLUXDB_INIT_ORG=Tamimology
       - DOCKER_INFLUXDB_INIT_BUCKET=homeassistant
       - INFLUXDB_BIND_ADDRESS=:8088
    volumes:
       - $PERSIST/influxdb/data:/var/lib/influxdb2
       - $PERSIST/influxdb/config:/etc/influxdb2
       - $LOCAL_TIME:/etc/localtime:ro
    networks:
      tamimology_bridge:
    image: 'influxdb:alpine'
```
</details>

[🔼 Back to top](#databases)


# Invidious
<details>
  <summary>
  </summary>

```
  invidious:
    container_name: invidious
    restart: $ALWAYS_ON_POLICY
    hostname: invidious
    environment:
      # Please read the following file for a comprehensive list of all available
      # configuration options and their associated syntax:
      # https://github.com/iv-org/invidious/blob/master/config/config.example.yml
      INVIDIOUS_CONFIG: |
        db:
          dbname: invidious
          user: invidious
          password: $DB_PASSWORD
          host: invidious-postgres
          port: 5432
        check_tables: true
        external_port: 443
        domain: invidious.$DOMAINNAME
        https_only: true
        log_level: Warn
        popular_enabled: false
        statistics_enabled: true
        registration_enabled: false
        login_enabled: true
        captcha_enabled: true
        jobs:
          clear_expired_items:
            enable: true
          refresh_channels:
            enable: true
          refresh_feeds:
            enable: true
        captcha_api_url: https://api.anti-captcha.com
        captcha_key: $ANTICAPTCHA_API
        admins: ["$GM_USER"]
        banner: "TamimologyTube"
        hmac_key: $APP_KEY
        default_user_preferences:
          locale: en-US
          region: AU
          captions: ["English", "English (auto-generated)", "Arabic"]
          dark_mode: "dark"
          feed_menu: ["Trending", "Subscriptions", "Playlists"]
          player_style: invidious
          default_home: Trending
          related_videos: true
          autoplay: true
          quality: dash
          quality_dash: auto
          volume: 50
          save_player_pos: true
    ports:
      - 7602:3000
    networks:
      tamimology_bridge:
    user: $PUID:$PGID
    security_opt:
      - no-new-privileges:true
    healthcheck:
      test: wget -nv --tries=1 --spider http://127.0.0.1:3000/api/v1/comments/jNQXAC9IVRw || exit 1
      interval: 30s
      timeout: 5s
      retries: 2
    labels: 
      autoheal: $AUTOHEAL_RESTART
      monocker.enable: $MONOCKER_ENABLE
    depends_on:
      invidious-postgres:
        condition: service_started
    image: 'quay.io/invidious/invidious:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Invidious-Postgres
<details>
  <summary>
  </summary>

```
  invidious-postgres:
    container_name: invidious-postgres
    restart: $ALWAYS_ON_POLICY
    hostname: invidious-postgres
    volumes:
      - $PERSIST/invidious/db:/var/lib/postgresql/data
    environment:
      TZ: $TZ
      POSTGRES_DB: invidious
      POSTGRES_USER: invidious
      POSTGRES_PASSWORD: $DB_PASSWORD
    networks:
      tamimology_bridge:
    security_opt:
      - no-new-privileges:true
    user: $PUID:$PGID
    healthcheck:
      test: ["CMD", "pg_isready", "-q", "-d", "invidious", "-U", "invidious"]
      timeout: 45s
      interval: 10s
      retries: 10
    labels: 
      autoheal: $AUTOHEAL_RESTART
    image: 'postgres:alpine'
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
      tamimology_bridge:
    labels: 
      # autoheal: $AUTOHEAL_RESTART
      monocker.enable: $MONOCKER_ENABLE
    logging:
      driver: loki
      options:
        loki-url: "http://$LOCAL_HOST:3002/loki/api/v1/push"
    # network_mode: 'host'
    # Optional - may be necessary for docker healthcheck to pass if running in host network mode
    # extra_hosts:
    #   - "host.docker.internal:host-gateway"
    user: $PUID:$PGID
    image: 'jellyfin/jellyfin:latest'
```
</details>

[🔼 Back to top](#media-playing)


# JellyStat
<details>
  <summary>
  </summary>

```
  jellystat:
    container_name: jellystat
    restart: $ALWAYS_ON_POLICY
    hostname: jellystat
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: $DB_PASSWORD
      POSTGRES_IP: jellystat-postgres
      POSTGRES_PORT: 5432
      JWT_SECRET: $APP_KEY
    volumes:
      - $PERSIST/jellyfin/stat/data:/app/backend/backup-data
    ports:
      - 6555:3000 #Server Port
      - 6556:3004 #Websocket port
    networks:
      tamimology_bridge:
    depends_on:
      jellystat-postgres:
        condition: service_healthy
    image: 'cyfershepard/jellystat:latest'
```
</details>

[🔼 Back to top](#media-playing)


# Jellystat-Postgres
<details>
  <summary>
  </summary>

```
  jellystat-postgres:
    container_name: jellystat-postgres
    restart: $ALWAYS_ON_POLICY
    hostname: jellystat-postgres
    volumes:
      - $PERSIST/jellyfin/stat/db:/var/lib/postgresql/data
    environment:
      TZ: $TZ
      POSTGRES_DB: jfstat
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: $DB_PASSWORD
    networks:
      tamimology_bridge:
    security_opt:
      - no-new-privileges:true
    user: $PUID:$PGID
    healthcheck:
      test: ["CMD", "pg_isready", "-q", "-d", "jfstat", "-U", "postgres"]
      timeout: 45s
      interval: 10s
      retries: 10
    labels: 
      autoheal: $AUTOHEAL_RESTART
    image: 'postgres:alpine'
```
</details>

[🔼 Back to top](#databases)


# Kea
<details>
  <summary>
  </summary>

```
  kea:
    container_name: kea-dhcp4
    restart: $ALWAYS_ON_POLICY
    hostname: kea
    network_mode: host
    command: -c /kea/config/dhcp4.json
    volumes:
      - $PERSIST/kea/config:/kea/config
      - $PERSIST/kea/sockets:/kea/sockets
      - $PERSIST/kea/leases:/kea/leases
      - $PERSIST/kea/logs:/kea/logs
    labels: 
      monocker.enable: $MONOCKER_ENABLE
    image: 'jonasal/kea-dhcp4:2' # do not use the alpine as it does not support routing to Adguard
```
</details>

[🔼 Back to top](#networking-and-security)


# LibreY
<details>
  <summary>
  </summary>

```
  librey:
    container_name: librey
    restart: $ALWAYS_ON_POLICY
    hostname: librey
    environment:
      - CONFIG_GOOGLE_DOMAIN=com
      - CONFIG_LANGUAGE=en
      - CONFIG_NUMBER_OF_RESULTS=10
      - CONFIG_INVIDIOUS_INSTANCE=https://invidious.$DOMAINNAME
      - CONFIG_DISABLE_BITTORRENT_SEARCH=false
      - CONFIG_HIDDEN_SERVICE_SEARCH=false #if true it hides the TOR tab
      - CONFIG_INSTANCE_FALLBACK=true
      - CONFIG_RATE_LIMIT_COOLDOWN=25
      - CONFIG_CACHE_TIME=20
      - CONFIG_TEXT_SEARCH_ENGINE=google
      - CURLOPT_PROXY_ENABLED=false
    volumes:
      - $PERSIST/librey/nginx:/var/log/nginx
      - $PERSIST/librey/logs:/var/log/php7
    ports:
      - 8245:8080
    networks:
      tamimology_bridge:
    image: 'ghcr.io/ahwxorg/librey:latest'
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
      tamimology_bridge:
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
      - MALOJA_NAME=Tamimology Music
      - MALOJA_CHARTS_DISPLAY_TILES=true
      - MALOJA_DISPLAY_ART_ICONS=true
    volumes:
      - $PERSIST/maloja:/data
    networks:
      tamimology_bridge:
    ports:
      - 42010:42010
    image: 'krateng/maloja:latest'
```
</details>

[🔼 Back to top](#media-playing)


# MariaDB
<details>
  <summary>
  </summary>

```
  mariadb:
    container_name: mariadb
    restart: $ALWAYS_ON_POLICY
    hostname: mariadb
    environment: # https://mariadb.com/kb/en/mariadb-docker-environment-variables/
      MARIADB_ROOT_PASSWORD: $DB_PASSWORD
      MARIADB_USER: homeassistant
      MARIADB_PASSWORD: $DB_PASSWORD
      MARIADB_DATABASE: homeassistant
      MARIADB_ROOT_HOST: 192.168.1.10
      MARIADB_AUTO_UPGRADE: "1"
      MARIADB_DISABLE_UPGRADE_BACKUP: "1"
      MARIADB_INITDB_SKIP_TZINFO: "1"
      # MYSQL_ROOT_PASSWORD: $DB_PASSWORD
      # MYSQL_USER: authelia
      # MYSQL_PASSWORD: $DB_PASSWORD
      # MYSQL_DATABASE: authelia
      # MYSQL_ROOT_USER: authelia
      PGID: $PGID
      PUID: $PUID
      TZ: $TZ
      # SKIP_INNODB: "yes" #only with jbergstroem/mariadb-alpine:latest, but not recommended
    volumes:
       - $PERSIST/mariadb:/var/lib/mysql
  ports:
      - 3306:3306
    networks:
      tamimology_bridge:
    tty: true
    labels: 
      autoheal: $AUTOHEAL_RESTART
      autoheal.stop.timeout: 30
      monocker.enable: $MONOCKER_ENABLE  
    logging:
      driver: loki
      options:
        loki-url: "http://$LOCAL_HOST:3002/loki/api/v1/push"
    image: 'jbergstroem/mariadb-alpine:10.6.13' # latest'
```
</details>

[🔼 Back to top](#databases)


# Memos
<details>
  <summary>
  </summary>

```
  memos:
    container_name: memos
    restart: $RESTART_POLICY
    hostname: memos
    volumes:
      - $PERSIST/memos:/var/opt/memos
    ports:
      - 5230:5230
    networks:
      tamimology_bridge:
    labels: 
      # autoheal: $AUTOHEAL_RESTART
      monocker.enable: $MONOCKER_ENABLE
    image: 'neosmemo/memos:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# MeTube
<details>
  <summary>
  </summary>

```
  metube:
    container_name: metube
    restart: $RESTART_POLICY
    hostname: metube
    volumes:
      - $DOWNLOADS/YouTube:/downloads
      - $DOWNLOADS/YouTube/cookies:/cookies
    environment:
      - UID=$PUID
      - GID=$PGID
      - DOWNLOAD_DIR=/downloads/video
      - AUDIO_DOWNLOAD_DIR=/downloads/audio
      - DEFAULT_THEME=dark
      - URL_PREFIX=/
      - YTDL_OPTIONS={"cookiefile":"/cookies/cookies.txt"} #,"writesubtitles":true,"subtitleslangs":["en","-live_chat"],"postprocessors":[{"key":"Exec","exec_cmd":"chmod 0664","when":"after_move"},{"key":"FFmpegEmbedSubtitle","already_have_subtitle":false},{"key":"FFmpegMetadata","add_chapters":true}]}
    ports:
      - 8081:8081
    networks:
      tamimology_bridge:
    # healthcheck:
    #  test: curl -f http://localhost:8081/ || exit 1
    labels: 
      monocker.enable: $MONOCKER_ENABLE
    image: 'ghcr.io/alexta69/metube:  latest'
```
</details>

[🔼 Back to top](#self-hosted)


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
      tamimology_bridge:
    image: 'petersem/monocker:latest'
```
</details>

[🔼 Back to top](#docker-related)


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
      tamimology_bridge:
    labels: 
      autoheal: $AUTOHEAL_RESTART
      monocker.enable: $MONOCKER_ENABLE
    user: 1035:$PGID # should be owner of volumes
    image: 'ghcr.io/navidrome/navidrome:latest'
```
</details>

[🔼 Back to top](#media-playing)


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
      DATABASE_HOST: mariadb
      DATABASE_PORT: 3306
      SERVER_NAME: https://paste.$DOMAINNAME
      AUTH_PROVIDER: $PROVIDER
      OAUTH2_INTERAAPPS_CLIENT_ID: $CLIENT_ID
      OAUTH2_INTERAAPPS_CLIENT_SECRET: $CLIENT_SECRET
      PASTEFY_INFO_CUSTOM_NAME: Tamimology Home Server
      PASTEFY_INFO_CUSTOM_FOOTER: Tam HAJAR 2021
      #PASTEFY_INFO_CUSTOM_LOGO: https://urltoimage
      PASTEFY_LOGIN_REQUIRED: false
      PASTEFY_LOGIN_REQUIRED_CREATE: true
      PASTEFY_LOGIN_REQUIRED_READ: false
      PASTEFY_ENCRYPTION_DEFAULT: false
    ports:
      - 9980:80
    networks:
      tamimology_bridge:
    depends_on:
      - mariadb
    image: "interaapps/pastefy:latest"
```
</details>

[🔼 Back to top](#self-hosted)


# PiAlert
<details>
  <summary>
  </summary>

```
  pialert:  
    container_name: pialert
    restart: $ALWAYS_ON_POLICY
    hostname: pialert
    environment:
      - TZ=$TZ
      - PORT=20211
      - HOST_USER_ID=$PUID
      - HOST_USER_GID=$PGID
    volumes:
      - $PERSIST/pialert:/home/pi/pialert/config
      - $PERSIST/pialert/pialert.db:/home/pi/pialert/db/pialert.db
      - $PERSIST/pialert/logs:/home/pi/pialert/front/log #/home/pi/pialert/log
    network_mode: host
    labels: 
      autoheal: $AUTOHEAL_RESTART
      monocker.enable: $MONOCKER_ENABLE
    security_opt:
      - no-new-privileges:true
    healthcheck:
      test: curl -f http://localhost:20211/ || exit 1
    image: 'jokobsk/pi.alert:latest'
```
</details>

[🔼 Back to top](#self-hosted)


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
      tamimology_bridge:
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
    volumes:
      - $PERSIST/projectsend/config:/config
      - $PERSIST/projectsend/data:/data
    ports:
      - 8516:80
    networks:
      tamimology_bridge:
    depends_on:
      - mariadb
    healthcheck:
      test: curl -f http://localhost:80/ || exit 1
    image: 'lscr.io/linuxserver/projectsend:latest'
```
</details>

[🔼 Back to top](#self-hosted)


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
      tamimology_bridge:
    depends_on:
      - loki
    command: -config.file=/etc/promtail/config.yml
    image: 'grafana/promtail:latest'
```
</details>

[🔼 Back to top](#databases)


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
      tamimology_bridge:
    ports:
      - 8895:80
    healthcheck:
     test: curl -f http://localhost:80/ || exit 1
    labels: 
      autoheal: $AUTOHEAL_RESTART
    security_opt:
      - no-new-privileges:true
    image: 'bizzycolah/qrcode-generator:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Scrutiny
<details>
  <summary>
  </summary>

```
  scrutiny:
    container_name: scrutiny
    restart: $RESTART_POLICY
    hostname: scrutiny
    privileged: true
    volumes:
      - $PERSIST/scrutiny:/opt/scrutiny/config
      - /run/udev:/run/udev:ro
      - $PERSIST/scrutiny/influxdb:/opt/scrutiny/influxdb
    ports:
      - 6080:8080
      - 8086:8086
    networks:
      tamimology_bridge:
    cap_add:
      - SYS_RAWIO
    devices:
      - /dev:/dev
    image: 'ghcr.io/analogj/scrutiny:master-omnibus'
```
</details>

[🔼 Back to top](#system-monitoring-and-management)


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
      tamimology_bridge:
    image: 'dko0/squoosh:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# SyncThing
<details>
  <summary>
  </summary>

```
  syncthing:
    container_name: syncthing
    restart: $ALWAYS_ON_POLICY
    hostname: Synchology #my-syncthing
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
      tamimology_bridge:
    labels: 
      autoheal: $AUTOHEAL_RESTART
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
      tamimology_bridge:
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
      tamimology_bridge:
    labels: 
      autoheal: $AUTOHEAL_RESTART
    image: 'danmed/tasmobackupv1:latest'
```
</details>

[🔼 Back to top](#programming)


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
      - mariadb
    network_mode: host #to be able to integrate in HA
    labels: 
      monocker.enable: $MONOCKER_ENABLE
    image: 'traccar/traccar:alpine'
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
      autoheal: $AUTOHEAL_RESTART
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
      tamimology_bridge:
    user: $PUID:$PGID
    labels: 
      autoheal: $AUTOHEAL_RESTART
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
      tamimology_bridge:
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
      WATCHTOWER_WARN_ON_HEAD_FAILURE: "never"
      WATCHTOWER_INCLUDE_STOPPED: "true"
      WATCHTOWER_SCHEDULE: 0 0/15 1 * * ? #Daily (?) at every 15th minute (0/15) from 0 (0) through 59 past hour 1am (1)
      WATCHTOWER_TIMEOUT: 15
      WATCHTOWER_NOTIFICATIONS_LEVEL: info
      WATCHTOWER_NOTIFICATIONS: shoutrrr
      WATCHTOWER_NOTIFICATION_URL: "pushover://shoutrrr:$PUSHOVER_DOCKER_UPDATES_API@$PUSHOVER_USER_KEY"
    # volumes:
      # - $DOCKER_SOCKET:/var/run/docker.sock
    networks:
      tamimology_bridge:
    labels: 
      monocker.enable: $MONOCKER_ENABLE
    image: 'containrrr/watchtower:latest'
```
</details>

[🔼 Back to top](#docker-related)


# Container-WebTTY
<details>
  <summary>
  </summary>

```
  webtty:
    container_name: container-webtty
    restart: $RESTART_POLICY
    hostname: webtty
    environment:
      - DOCKER_HOST=$SOCKET
    # volumes:
    #   - $DOCKER_SOCKET:/var/run/docker.sock
    ports:
      - 8818:8080
    networks:
      tamimology_bridge:
    image: 'wrfly/container-web-tty:0.1.10' #latest'
```
</details>

[🔼 Back to top](#docker-related)


# YoPass
<details>
  <summary>
  </summary>
  
```
  yopass:
    container_name: yopass
    restart: $RESTART_POLICY
    hostname: yopass
    ports:
      - 8180:80
    networks:
      tamimology_bridge:
    depends_on:
      - yopass-memcached
    command: --memcached=memcached:11211 --port 80
    links:
      - yopass-memcached:memcached
    image: 'jhaals/yopass:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Yopass-MemCached
<details>
  <summary>
  </summary>

```
  yopass-memcached:
    container_name: yopass-memcached
    restart: $RESTART_POLICY
    hostname: yopass-memcached
    expose:
      - 11211
    networks:
      tamimology_bridge:
    image: 'memcached:alpine'
```
</details>

[🔼 Back to top](#docker-related)




# Maarten's Media Manager (MMM)

Setup to organize and manage media files with ease. Integrates the following tools:

- [Jellyfin](https://jellyfin.org/) for media streaming.
- [JellySeerr](https://seerr.dev/) for media request handling.
- [Sonarr](https://sonarr.tv/) for TV show management.
- [Radarr](https://radarr.video/) for movie management.
- [Prowlarr](https://prowlarr.com/) for indexer management.
- [Bazarr](https://www.bazarr.media/) for subtitle management.
- [qBittorrent](https://www.qbittorrent.org/) for torrent downloading
- [Janitorr](https://github.com/Schaka/janitorr) for media file organization and cleanup.

## Prerequisites

- Docker and Docker Compose installed on your system.
- A media server setup (e.g., NAS, dedicated server, etc.).
- Basic knowledge of Docker and command line operations.

## Installation

1. Clone this repository to your local machine:

   ```bash
   git clone https://github.com/maartenpeels/mmm.git
   ```

2. Navigate to the project directory:

   ```bash
   cd mmm
   ```

3. Create the necessary directories for persistent storage:

   ```bash
   mkdir -p config/{jellyfin,sonarr,radarr,prowlarr,bazarr,jellyseerr,qbittorrent} data/media/{tv,movies,torrents,subtitles}
   ```

4. Start the services using Docker Compose:

   ```bash
   docker-compose up -d
   ```

5. Access the web interfaces of each service via your web browser to complete their individual setups.

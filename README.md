# Maarten's Media Manager (MMM)

Setup to organize and manage media files with ease. Integrates the following tools:

- [Plex](https://www.plex.tv/) for media streaming.
- [Sonarr](https://sonarr.tv/) for TV show management.
- [Radarr](https://radarr.video/) for movie management.
- [Overseerr](https://overseerr.dev/) for media request handling.
- [Prowlarr](https://prowlarr.com/) for indexer management.
- [Bazarr](https://www.bazarr.media/) for subtitle management.
- [qBittorrent](https://www.qbittorrent.org/) for torrent downloading

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
   mkdir -p config/{plex,sonarr,radarr,prowlarr,bazarr,overseerr,qbittorrent} data/media/{tv,movies,torrents,subtitles}
   ```

4. Start the services using Docker Compose:

   ```bash
   docker-compose up -d
   ```

5. Follow the [Setup Guide](SETUP_GUIDE.md) to configure each service.

# Media Server Setup Guide

Complete guide for configuring your Plex media server stack with Sonarr, Radarr, Prowlarr, Bazarr, Overseerr, and qBittorrent.

## 1. qBittorrent Configuration

**Access:** <http://localhost:8080>

### Initial Login

- **Username:** `admin`
- **Password:** `adminadmin`

### Settings to Configure

Go to **Tools → Options**:

#### Downloads Tab

- **Default Save Path:** `/downloads`
- **Keep incomplete torrents in:** Disabled (uncheck "Temp folder enabled")
- **Pre-allocate disk space:** Disabled
- **Append .!qB extension:** Disabled

#### Connection Tab

- **Listening Port:** `6881`

#### BitTorrent Tab

- **Privacy → Enable DHT:** Disabled
- **Privacy → Enable PEX:** Disabled
- **Privacy → Enable Local Peer Discovery:** Disabled
- **Seeding Limits:**
  - When ratio reaches: `0.00`
  - When seeding time reaches: `0` minutes
  - Then: `Pause torrent`
- **Torrent Queueing:**
  - Maximum active downloads: `3`
  - Maximum active uploads: `0`
  - Maximum active torrents: `3`
- **Share Ratio Limiting:**
  - Maximum ratio: `0.00`
  - Maximum seeding time: `0` minutes

#### Web UI Tab

- **Authentication:** Keep enabled
- **Bypass authentication for clients on localhost:** Enabled
- **Bypass authentication for clients in whitelisted IP subnets:** Add `172.0.0.0/8` (Docker network)

**Click Save** to apply changes.

## 2. Prowlarr Configuration

**Access:** <http://localhost:9696>

### Initial Setup

1. **Set Authentication** (Settings → General → Security)

   - Authentication: Forms (Login Page)
   - Username: `admin` (or your choice)
   - Password: (set a strong password)

2. **Get API Key:** Settings → General → Security → Copy the API Key (you'll need this)

### Add Download Client

Go to **Settings → Download Clients → Add (+)**:

- **Type:** qBittorrent
- **Name:** qBittorrent
- **Host:** `qbittorrent` (service name in Docker network)
- **Port:** `8080`
- **Username:** `admin`
- **Password:** (your qBittorrent password)
- **Category:** `prowlarr`
- **Test** → **Save**

### Add Indexers

Go to **Indexers → Add Indexer**:

#### Public Indexers (Examples)

- **1337x** (movies and TV)
- **The Pirate Bay** (movies and TV)
- **RARBG** (movies and TV)
- **EZTV** (TV shows)
- **YTS** (movies)

For each indexer:

1. Click the indexer name
2. Configure any required fields
3. Set minimum seeders (recommended: 5-10)
4. **Test** → **Save**

#### Private Indexers

If you have private tracker accounts, add them with your credentials.

### Sync Apps to Prowlarr

We'll configure this from Sonarr/Radarr side after setting them up.

## 3. Sonarr Configuration

**Access:** <http://localhost:8989>

### Initial Setup

1. **Set Authentication** (Settings → General → Security)

   - Authentication: Forms (Login Page)
   - Username: `admin`
   - Password: (set a strong password)

2. **Get API Key:** Settings → General → Copy the API Key

### Add Root Folder

Go to **Settings → Media Management → Root Folders → Add (+)**:

- **Path:** `/data/media/tv`
- **Save**

### Configure Media Management

**Settings → Media Management:**

- **Rename Episodes:** Enabled
- **Replace Illegal Characters:** Enabled
- **Standard Episode Format:**

  ```
  {Series TitleYear} - S{season:00}E{episode:00} - {Episode CleanTitle} [{Quality Full}]
  ```

- **Daily Episode Format:**

  ```
  {Series TitleYear} - {Air-Date} - {Episode CleanTitle} [{Quality Full}]
  ```

- **Series Folder Format:**

  ```
  {Series TitleYear}
  ```

- **Season Folder Format:**

  ```
  Season {season:00}
  ```

**Save Changes**

### Add Download Client

Go to **Settings → Download Clients → Add (+)**:

- **Type:** qBittorrent
- **Name:** qBittorrent
- **Enable:** Yes
- **Host:** `qbittorrent`
- **Port:** `8080`
- **Username:** `admin`
- **Password:** (your qBittorrent password)
- **Category:** `tv`
- **Post-Import Category:** (leave empty or use `tv-complete`)
- **Recent Priority:** Last
- **Older Priority:** Last
- **Initial State:** Start
- **Test** → **Save**

### Connect to Prowlarr

Go to **Settings → Indexers → Add (+) → Prowlarr**:

- **Sync Level:** Full Sync
- **URL:** `http://prowlarr:9696`
- **API Key:** (Prowlarr API key from step 2)
- **Test** → **Save**

Prowlarr will automatically sync all configured indexers to Sonarr.

### Quality Profiles

Go to **Settings → Profiles → Quality Profiles**:

Edit or create profiles based on your preference:

- **HD-1080p:** Up to Bluray-1080p
- **HD-720p/1080p:** Up to Bluray-1080p, cutoff at WEBDL-1080p
- **Any:** Accept any quality (not recommended)

### Configure Wanted Settings

**Settings → Indexers:**

- **Minimum Age:** `0` (or set delay if needed)
- **Retention:** `0` (unlimited)
- **RSS Sync Interval:** `15` minutes

## 4. Radarr Configuration

**Access:** <http://localhost:7878>

### Initial Setup

1. **Set Authentication** (Settings → General → Security)

   - Authentication: Forms (Login Page)
   - Username: `admin`
   - Password: (set a strong password)

2. **Get API Key:** Settings → General → Copy the API Key

### Add Root Folder

Go to **Settings → Media Management → Root Folders → Add (+)**:

- **Path:** `/data/media/movies`
- **Save**

### Configure Media Management

**Settings → Media Management:**

- **Rename Movies:** Enabled
- **Replace Illegal Characters:** Enabled
- **Standard Movie Format:**

  ```
  {Movie CleanTitle} ({Release Year}) [{Quality Full}]
  ```

- **Movie Folder Format:**

  ```
  {Movie CleanTitle} ({Release Year})
  ```

**Save Changes**

### Add Download Client

Go to **Settings → Download Clients → Add (+)**:

- **Type:** qBittorrent
- **Name:** qBittorrent
- **Enable:** Yes
- **Host:** `qbittorrent`
- **Port:** `8080`
- **Username:** `admin`
- **Password:** (your qBittorrent password)
- **Category:** `movies`
- **Post-Import Category:** (leave empty or use `movies-complete`)
- **Recent Priority:** Last
- **Older Priority:** Last
- **Initial State:** Start
- **Test** → **Save**

### Connect to Prowlarr

Go to **Settings → Indexers → Add (+) → Prowlarr**:

- **Sync Level:** Full Sync
- **URL:** `http://prowlarr:9696`
- **API Key:** (Prowlarr API key from step 2)
- **Test** → **Save**

### Quality Profiles

Go to **Settings → Profiles → Quality Profiles**:

Configure based on preference:

- **HD-1080p:** Up to Bluray-1080p
- **HD-720p/1080p:** Up to Bluray-1080p, cutoff at WEBDL-1080p
- **Ultra-HD (4K):** If you want 4K content

### Configure Wanted Settings

**Settings → Indexers:**

- **Minimum Age:** `0`
- **Retention:** `0`
- **RSS Sync Interval:** `15` minutes

## 5. Bazarr Configuration

**Access:** <http://localhost:6767>

### Initial Setup Wizard

1. **Languages:**

   - **Single Language:** No (if you want multiple subtitle languages)
   - **Languages Filter:** Select your preferred languages (e.g., English, Dutch)
   - **Default Enabled:** Yes for primary language

2. **Sonarr Connection:**

   - **Use Sonarr:** Yes
   - **Hostname or IP:** `sonarr`
   - **Listening Port:** `8989`
   - **Base URL:** (leave empty)
   - **SSL:** No
   - **API Key:** (Sonarr API key from step 3)
   - **Test** → **Next**

3. **Radarr Connection:**
   - **Use Radarr:** Yes
   - **Hostname or IP:** `radarr`
   - **Listening Port:** `7878`
   - **Base URL:** (leave empty)
   - **SSL:** No
   - **API Key:** (Radarr API key from step 4)
   - **Test** → **Next**

### Configure Subtitle Providers

Go to **Settings → Providers**:

Enable and configure providers (free options):

- **OpenSubtitles.com:** Requires free account
- **OpenSubtitles.org:** Requires free account
- **Subscene:** No account needed
- **YIFY Subtitles:** No account needed
- **Podnapisi:** No account needed

For each provider:

1. Enable the provider
2. Add credentials if required
3. Adjust rate limiting if needed

### Configure Languages

**Settings → Languages:**

- **Languages Profile:** Add a new profile
  - Name: `Default`
  - Languages: Select desired languages (e.g., English, Dutch)
  - **Hearing Impaired:** Choose "Exclude" or "Prefer"
- **Default Settings:**
  - **Series:** Use the profile you created
  - **Movies:** Use the profile you created

### Automatic Sync

**Settings → Sonarr/Radarr:**

- **Series Sync:** `60` minutes
- **Movies Sync:** `60` minutes
- **Update Series Subtitles:** Enabled
- **Update Movies Subtitles:** Enabled

## 6. Overseerr Configuration

**Access:** <http://localhost:5055>

### Initial Setup Wizard

1. **Sign In:**

   - Click **Use your Plex account**
   - Log in with Plex credentials
   - Grant Overseerr access

2. **Plex Configuration:**

   - Your Plex servers will be listed
   - Select your server
   - Select libraries to scan (Movies, TV Shows)
   - **Save Changes** → **Continue**

3. **Radarr Configuration:**

   - **Add Radarr Server**
   - **Server Name:** Radarr
   - **Hostname/IP:** `radarr`
   - **Port:** `7878`
   - **API Key:** (Radarr API key)
   - **URL Base:** (leave empty)
   - **Use SSL:** No
   - **Quality Profile:** Select default profile
   - **Root Folder:** `/data/media/movies`
   - **Minimum Availability:** Announced or Released
   - **Tags:** (optional)
   - **External URL:** `http://localhost:7878` (for links)
   - **Enable Scan:** Yes
   - **Enable Automatic Search:** Yes
   - **Test** → **Save**

4. **Sonarr Configuration:**
   - **Add Sonarr Server**
   - **Server Name:** Sonarr
   - **Hostname/IP:** `sonarr`
   - **Port:** `8989`
   - **API Key:** (Sonarr API key)
   - **URL Base:** (leave empty)
   - **Use SSL:** No
   - **Quality Profile:** Select default profile
   - **Root Folder:** `/data/media/tv`
   - **Language Profile:** Deprecated (leave default)
   - **Tags:** (optional)
   - **Anime Quality Profile:** (optional, if you watch anime)
   - **Anime Root Folder:** (optional)
   - **Season Folders:** Yes
   - **External URL:** `http://localhost:8989`
   - **Enable Scan:** Yes
   - **Enable Automatic Search:** Yes
   - **Test** → **Save**

### User Management

**Settings → Users:**

- **Default Permissions:** Set what new users can do

  - Request
  - Request Movies
  - Request TV Shows
  - Auto-Approve Movies (optional)
  - Auto-Approve Series (optional)

- **Import Plex Users:** Sync users from Plex

### Notifications (Optional)

**Settings → Notifications:**

Configure notifications for:

- Discord
- Telegram
- Email
- Slack
- Webhook

## 7. Plex Media Server Configuration

**Access:** <http://localhost:32400/web>

### Initial Setup

1. **Sign In:** Use your Plex account

2. **Server Setup:**

   - **Server Name:** Give your server a friendly name
   - **Allow outside access:** Enable if you want remote access

3. **Add Libraries:**

#### Movies Library

- **Library Type:** Movies
- **Add Folders:** `/data/media/movies`
- **Advanced Settings:**
  - **Scanner:** Plex Movie Scanner
  - **Agent:** Plex Movie (or TMDB)
  - **Enable video preview thumbnails:** Optional (uses resources)

#### TV Shows Library

- **Library Type:** TV Shows
- **Add Folders:** `/data/media/tv`
- **Advanced Settings:**
  - **Scanner:** Plex Series Scanner
  - **Agent:** Plex TV Series (or TheTVDB)
  - **Enable video preview thumbnails:** Optional

### Server Settings

Go to **Settings (wrench icon) → Server**:

#### Library

- **Scan my library automatically:** Enabled
- **Run a partial scan when changes detected:** Enabled
- **Scan my library periodically:** Every 6 hours (or your preference)
- **Empty trash automatically:** After every scan

#### Network

- **Remote Access:** Configure if needed
- **Custom server access URLs:** Leave default
- **LAN Networks:** `172.0.0.0/8` (allows Docker containers)

#### Transcoder

- **Transcoder quality:** Automatic
- **Transcoder temporary directory:** Default
- **Transcoder default throttle buffer:** 60 seconds
- **Background transcoding x264 preset:** Very Fast
- **Use hardware acceleration when available:** Enable if GPU available

#### Scheduled Tasks

- **Backup database:** Daily at 2 AM
- **Optimize database:** Weekly
- **Update all libraries:** Daily at 2 AM

## Service URLs & Default Ports

| Service     | URL                      | Default Port | Purpose             |
| ----------- | ------------------------ | ------------ | ------------------- |
| Plex        | <http://localhost:32400> | 32400        | Media Server        |
| Sonarr      | <http://localhost:8989>  | 8989         | TV Show Management  |
| Radarr      | <http://localhost:7878>  | 7878         | Movie Management    |
| Bazarr      | <http://localhost:6767>  | 6767         | Subtitle Management |
| Prowlarr    | <http://localhost:9696>  | 9696         | Indexer Manager     |
| Overseerr   | <http://localhost:5055>  | 5055         | Request Management  |
| qBittorrent | <http://localhost:8080>  | 8080         | Download Client     |

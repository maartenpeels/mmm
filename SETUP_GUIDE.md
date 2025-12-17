# Media Server Setup Guide

Complete guide for configuring your Plex media server stack with Sonarr, Radarr, Prowlarr, Bazarr, Overseerr, and qBittorrent.

## 1. qBittorrent Configuration

**Access:** <http://localhost:8080>

### Initial Login

- **Username:** `admin`
- **Password:** `see logs of container for password`

### Settings to Configure

Go to **Tools → Options**:

#### BitTorrent Tab

- **Seeding Limits:**
  - When ratio reaches: `0.00`
  - When seeding time reaches: `0` minutes
  - Then: `Pause torrent`
- **Torrent Queueing:**
  - Maximum active uploads: `0`
  - Maximum active torrents: `3`

#### Web UI Tab

- **Password:** Set a new password
- **Bypass authentication for clients on localhost:** Enabled
- **Bypass authentication for clients in whitelisted IP subnets:** Add `172.0.0.0/8` (Docker network)

**Click Save** to apply changes.

## 2. Sonarr Configuration

**Access:** <http://localhost:8989>

### Initial Setup

1. **Set Authentication**

   - Authentication: Forms (Login Page)
   - Username: `admin`
   - Password: (set a strong password)

2. **Get API Key:** Settings → General → Copy the API Key

### Add Root Folder

Go to **Settings → Media Management → Root Folders → Add (+)**:

- **Path:** `/data/media/tv` (Create manually on disk first)
- **Save**

### Configure Media Management

**Settings → Media Management:**

- **Rename Episodes:** Enabled
- **Replace Illegal Characters:** Enabled

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
- **Recent Priority:** Last
- **Older Priority:** Last
- **Initial State:** Started
- **Test** → **Save**

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

## 3. Radarr Configuration

**Access:** <http://localhost:7878>

### Initial Setup

1. **Set Authentication** (Settings → General → Security)

   - Authentication: Forms (Login Page)
   - Username: `admin`
   - Password: (set a strong password)

2. **Get API Key:** Settings → General → Copy the API Key

### Add Root Folder

Go to **Settings → Media Management → Root Folders → Add (+)**:

- **Path:** `/data/media/movies` (Create manually on disk first)
- **Save**

### Configure Media Management

**Settings → Media Management:**

- **Rename Movies:** Enabled
- **Replace Illegal Characters:** Enabled

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
- **Recent Priority:** Last
- **Older Priority:** Last
- **Initial State:** Started
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

## 4. Prowlarr Configuration

**Access:** <http://localhost:9696>

### Initial Setup

1. **Set Authentication**

   - Authentication: Forms (Login Page)
   - Username: `admin` (or your choice)
   - Password: (set a strong password)

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
4. **Test** → **Save**

#### Private Indexers

If you have private tracker accounts, add them with your credentials.

### Sync to Sonarr/Radarr

Go to **Settings → Apps → Add (+)**:

1. **Select** `Sonarr`
2. **Prowlarr Server:** `http://prowlarr:9696`
3. **Sonarr Server:** `http://sonarr:8989`
4. **API Key:** (Paste the API Key from Sonarr Settings → General)
5. **Sync Level:** `Full Sync`
6. **Test** → **Save**
7. **Repeat for Radarr**

## 5. Bazarr Configuration

**Access:** <http://localhost:6767>

### Initial Setup Wizard

1. **Languages:**

   - **Single Language:** No (if you want multiple subtitle languages)
   - **Languages Filter:** Select your preferred languages (e.g., English, Dutch)

2. **Sonarr Connection:**

   - **Use Sonarr:** Yes
   - **Hostname or IP:** `sonarr`
   - **Listening Port:** `8989`
   - **Base URL:** (leave empty)
   - **SSL:** No
   - **API Key:** (Sonarr API key from step 3)
   - **Test** → **Save**

3. **Radarr Connection:**
   - **Use Radarr:** Yes
   - **Hostname or IP:** `radarr`
   - **Listening Port:** `7878`
   - **Base URL:** (leave empty)
   - **SSL:** No
   - **API Key:** (Radarr API key from step 4)
   - **Test** → **Save**

### Configure Subtitle Providers

Go to **Settings → Providers**:

Enable and configure providers (free options):

- **OpenSubtitles.com:** Requires free account
- **YIFY Subtitles:** No account needed

For each provider:

1. Enable the provider
2. Add credentials if required
3. Adjust rate limiting if needed

### Configure Languages

**Settings → Languages:**

- **Languages Profile:** Add a new profile
  - Name: `Default`
  - Languages: Select desired languages (e.g., English, Dutch)
  - **Series:** Use the profile you created
  - **Movies:** Use the profile you created

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
   - **Default Server**: `yes`
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
   - **Default Server**: `yes`
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

## 7. Jellyfin Media Server Configuration
TODO...

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

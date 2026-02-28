# TRMNL Development Workflow

---

## Installation

### RubyGems (requires Ruby 3.x)
```bash
gem install trmnl_preview
```
PNG rendering requires Firefox + ImageMagick.

### Docker
```bash
docker run --publish 4567:4567 --volume "$(pwd):/plugin" trmnl/trmnlp serve
```
Interactive mode for multiple commands:
```bash
docker run -it --publish 4567:4567 --volume "$(pwd):/plugin" trmnl/trmnlp /bin/sh
```

---

## CLI Commands (trmnlp)

| Command | Description |
|---------|------------|
| `trmnlp init [name]` | Create new plugin project |
| `trmnlp serve` | Start local dev server (port 4567) |
| `trmnlp build` | Generate static HTML files |
| `trmnlp login` | Authenticate with TRMNL server |
| `trmnlp push` | Upload plugin to TRMNL |
| `trmnlp pull` | Download latest settings |
| `trmnlp clone [name] [id]` | Copy plugin from TRMNL server |
| `trmnlp list` | List private plugins |
| `trmnlp version` | Show version |

### Options
- `-d DIR` / `--dir DIR` - Plugin project directory (default: current)
- `-q` / `--quiet` - Suppress output
- `-b ADDR` / `--bind ADDR` - Bind address for serve
- `-p PORT` / `--port PORT` - Port for serve
- `-f` / `--force` - Skip confirmation prompts
- `-i ID` / `--plugin-settings-id ID` - Plugin settings ID

---

## Plugin Project Structure
```
my_plugin/
├── .trmnlp.yml              # Local dev config
├── bin/
│   └── dev                   # Convenience script
└── src/
    ├── full.liquid           # Full-screen layout
    ├── half_horizontal.liquid # Top/bottom half
    ├── half_vertical.liquid   # Left/right half
    ├── quadrant.liquid        # Quarter screen
    ├── shared.liquid          # Shared components (prepended to all views)
    └── settings.yml           # Plugin configuration
```

---

## `.trmnlp.yml` (Local Dev Config)
```yaml
---
# auto-reload when files change (set `watch: false` to disable)
watch:
  - .trmnlp.yml
  - src

# values of custom fields (defined in src/settings.yml)
custom_fields:
  station: "{{ env.ICAO }}"    # environment variable interpolation

# timezone override for local development
time_zone: America/New_York    # IANA timezone identifier

# override TRMNL global variables for local testing
variables:
  trmnl:
    user:
      name: Peter Quill
      first_name: Peter
      locale: en
      time_zone: "Eastern Time (US & Canada)"
      time_zone_iana: "America/New_York"
      utc_offset: -14400
    plugin_settings:
      instance_name: My Plugin Instance
    device:
      friendly_id: "TRMNL-DEV"
      percent_charged: 85
      wifi_strength: -50
      height: 480
      width: 800
```

---

## `settings.yml` (Plugin Config)
```yaml
---
strategy: polling              # polling | webhook | merge_tag
no_screen_padding: 'no'       # 'yes' | 'no'
dark_mode: 'no'               # 'yes' | 'no'
static_data: ''               # JSON for static strategy
polling_verb: get              # get | post
polling_url: 'https://api.example.com/data'
polling_headers: 'Authorization=Bearer token123'  # key=value&key2=value2
polling_body: ''               # JSON body for POST
name: My Plugin
refresh_interval: 1440         # minutes between refreshes
```

### Multiple Polling URLs
```yaml
polling_url: |
  https://api1.example.com/data
  https://api2.example.com/data
```
Access in templates: `{{ IDX_0.field }}`, `{{ IDX_1.field }}`

---

## Local Development Workflow

### 1. Create New Plugin
```bash
trmnlp init my_plugin
cd my_plugin
```

### 2. Configure Data Source
Edit `src/settings.yml` with your API URL, or use `.trmnlp.yml` for local overrides.

### 3. Start Dev Server
```bash
trmnlp serve
# or with options:
trmnlp serve -p 8080 -b 0.0.0.0
```
Opens at http://localhost:4567

### 4. Edit Templates
Edit `.liquid` files in `src/`. Server auto-reloads on save.

### 5. Preview
- Browser shows live preview with device picker
- Choose model, palette, orientation, dark mode
- Toggle between HTML and PNG output
- View current data JSON

### 6. Push to TRMNL
```bash
trmnlp login     # first time only
trmnlp push      # uploads src/ as ZIP
```

### 7. Pull Updates
```bash
trmnlp pull      # downloads latest settings.yml
```

---

## Web Server Routes (Local Dev)

| Route | Purpose |
|-------|---------|
| `GET /` | Redirect to /full |
| `GET /full` | Full-screen preview |
| `GET /half_horizontal` | Half horizontal preview |
| `GET /half_vertical` | Half vertical preview |
| `GET /quadrant` | Quadrant preview |
| `GET /render/{view}.html` | Rendered HTML |
| `GET /render/{view}.png` | PNG screenshot |
| `GET /data` | Current JSON data |
| `POST /webhook` | Receive webhook data |
| `GET /poll` | Force re-poll data |
| `GET /live_reload` | WebSocket for auto-reload |

PNG params: `?width=800&height=480&color_depth=1`

---

## Authentication
- `trmnlp login` saves API key to `~/.config/trmnlp/config.yml`
- API key must start with `user_`
- Environment variable alternative: `$TRMNL_API_KEY`
- Login verifies key via `api_client.get_me()`

---

## Plugin Strategies

### Polling
- TRMNL fetches from your URL(s) on schedule
- Supports: JSON, RSS, XML, plaintext, CSV
- GET or POST with custom headers/body
- Multiple URLs with namespace access (`IDX_0`, `IDX_1`)

### Webhook
- POST data to TRMNL endpoint
- Max 2KB payload, 12x/hour (Standard)
- Max 5KB payload, 30x/hour (TRMNL+)
- Exceeding returns HTTP 429
- URL includes Plugin Settings UUID

#### Webhook Merge Strategies
- **Default**: Replace all merge variables
- **Deep Merge**: `"merge_strategy": "deep_merge"` - merge nested objects
- **Stream**: `"merge_strategy": "stream", "stream_limit": 10` - append to arrays

### Static
- Data defined in `settings.yml` as JSON
- No external API calls
- Good for demos/testing

---

## API Endpoints

### Display API
```
GET https://trmnl.com/api/display
Headers: access-token: xxxxx
Response: { image_url, image_name, update_firmware, refresh_rate }
```

### Current Screen
```
GET https://trmnl.com/api/current_screen
Headers: access-token: xxxxx
Response: { status, refresh_rate, image_url, filename, rendered_at }
```

### Webhook
```
POST https://trmnl.com/api/custom_plugins/{uuid}
Body: { "merge_variables": { "key": "value" } }
```

### BYOS (Build Your Own Server)
- `GET /api/setup` - Device setup (Header: `ID: <mac>`)
- `GET /api/display` - Get image (Header: `ID: <mac>`)
- `POST /api/log` - Device logs (Header: `ID: <mac>`)
- IP whitelist: `GET https://trmnl.com/api/ips`

### Plugin Screen Generation (for marketplace plugins)
```
POST {plugin_markup_url}
Headers: Authorization: Bearer {access_token}
Body: user_uuid=xx&trmnl=<metadata>
Response: { markup, markup_half_vertical, markup_half_horizontal, markup_quadrant, shared }
```

---

## Publishing (Recipes)
1. Create and test plugin locally
2. Push to TRMNL via `trmnlp push`
3. Submit as Recipe via TRMNL platform
4. "Chef" linter validates code
5. Team review (1-2 days)
6. Published to community at `usetrmnl.com/recipes`

---

## Key Dependencies
- **sinatra ~> 4.1** - Web server
- **trmnl-liquid ~> 0.4.0** - Liquid filters/tags
- **activesupport ~> 8.0** - HTML rendering
- **selenium-webdriver ~> 4.39** - PNG rendering (Firefox)
- **mini_magick ~> 4.12.0** - Image processing
- **faraday ~> 2.1** - HTTP client
- **thor ~> 1.3** - CLI framework
- **filewatcher ~> 2.1** - File watching for live reload

---

## Testing
```bash
bin/rake          # runs RSpec tests
```
Tests cover `Config::Project` and `Context` classes.

---

## Troubleshooting
- PNG rendering requires Firefox + ImageMagick installed
- If serve fails, check if port is in use: `-p 8080`
- Force re-poll: visit `/poll` in browser
- Check data: visit `/data` in browser
- Webhook data: POST to `/webhook` locally

# Home Assistant Configuration for Smart Box V1

Smart Box V1 displays weather forecasts and media player information on its screens. To make this work, you need to create template sensors in Home Assistant that extract data from your weather entity and media player.

## Quick Setup

1. Add the template sensors below to your Home Assistant `configuration.yaml`
2. Replace `weather.forecast` with your weather entity ID
3. Replace `media_player.media_player` with your media player entity ID
4. Replace `http://192.168.1.102:8123` with your Home Assistant URL
5. Restart Home Assistant or reload YAML configuration
6. Verify sensors are working in Developer Tools → States

## Required Template Sensors

Add the following to your Home Assistant `configuration.yaml` file:

```yaml
template:
  # ===========================================================================
  # MEDIA PLAYER SENSORS
  # ===========================================================================
  # These sensors extract now playing information from your media player
  
  - sensor:
      # Now Playing Title
      - name: "Now Playing Title"
        state: >
          {{ state_attr('media_player.media_player', 'media_title') or 'No media' }}

      # Now Playing Artist
      - name: "Now Playing Artist"
        state: >
          {{ state_attr('media_player.media_player', 'media_artist') or '' }}

      # Now Playing Album Art
      # This sensor extracts and formats the album art URL for the display
      - name: "Now Playing Album Art"
        state: >
          {% set base_url = 'http://192.168.1.102:8123' %}  {# REPLACE with your HA IP #}
          {% set placeholder = base_url + '/local/images/placeholder.png' %}
          {% set art = state_attr('media_player.media_player', 'entity_picture') %}
          
          {% if art %}
            {# 1. Handle Relative vs Absolute URLs #}
            {% if art.startswith('/') %}
              {% set url = base_url + art %}
            {% else %}
              {% set url = art %}
            {% endif %}

            {# 2. Regex to request 700x700 specifically #}
            {# Note: If the provider doesn't support 700, this might fail. #}
            {# Safer alternative: Request 1000x1000 and let ESPHome resize down. #}
            {% set url = url | regex_replace('(\d+)x(\d+)bb\.jpg', '700x700bb.jpg') %}
            {% set url = url | regex_replace('(\d+)x(\d+)\.jpg', '700x700.jpg') %}
            
            {{ url }}
          {% else %}
            {{ placeholder }}
          {% endif %}

  # ===========================================================================
  # WEATHER FORECAST SENSORS
  # ===========================================================================
  # These sensors extract forecast data using the weather.get_forecasts service
  
  - trigger:
      - platform: time_pattern
        minutes: "/30"  # Update every 30 minutes
      - platform: homeassistant
        event: start
    action:
      - service: weather.get_forecasts
        data:
          type: daily
        target:
          entity_id: weather.forecast  # 👈 Replace with your real entity
        response_variable: daily

    sensor:
      # =====================
      # TODAY'S WEATHER SENSORS
      # =====================
      
      # Today's high temperature
      - name: "Weather Forecast Today Temp High"
        unique_id: weather_forecast_today_temp_high
        state: >
          {% set entity = 'weather.forecast' %}
          {% if daily[entity].forecast is defined and daily[entity].forecast|length > 0 %}
            {{ daily[entity].forecast[0].temperature }}
          {% else %}
            unavailable
          {% endif %}
        unit_of_measurement: "°C"
        device_class: temperature
        state_class: measurement

      # Today's low temperature
      - name: "Weather Forecast Today Temp Low"
        unique_id: weather_forecast_today_temp_low
        state: >
          {% set entity = 'weather.forecast' %}
          {% if daily[entity].forecast is defined and daily[entity].forecast|length > 0 %}
            {{ daily[entity].forecast[0].templow }}
          {% else %}
            unavailable
          {% endif %}
        unit_of_measurement: "°C"
        device_class: temperature
        state_class: measurement
      
      # ==========================
      # TOMORROW'S WEATHER SENSORS
      # ==========================
      
      # Tomorrow's high temperature
      - name: "Weather Forecast Tomorrow Temp High"
        unique_id: weather_forecast_tomorrow_temp_high
        state: >
          {% set entity = 'weather.forecast' %}
          {% if daily[entity].forecast is defined and daily[entity].forecast|length > 1 %}
            {{ daily[entity].forecast[1].temperature }}
          {% else %}
            unavailable
          {% endif %}
        unit_of_measurement: "°C"
        device_class: temperature
        state_class: measurement

      # Tomorrow's low temperature
      - name: "Weather Forecast Tomorrow Temp Low"
        unique_id: weather_forecast_tomorrow_temp_low
        state: >
          {% set entity = 'weather.forecast' %}
          {% if daily[entity].forecast is defined and daily[entity].forecast|length > 1 %}
            {{ daily[entity].forecast[1].templow }}
          {% else %}
            unavailable
          {% endif %}
        unit_of_measurement: "°C"
        device_class: temperature
        state_class: measurement

      # Tomorrow's condition
      - name: "Weather Forecast Tomorrow Condition"
        unique_id: weather_forecast_tomorrow_condition
        state: >
          {% set entity = 'weather.forecast' %}
          {% if daily[entity].forecast is defined and daily[entity].forecast|length > 1 %}
            {{ daily[entity].forecast[1].condition }}
          {% else %}
            unavailable
          {% endif %}
      
      # =====================================
      # DAY AFTER TOMORROW'S WEATHER SENSORS
      # =====================================
      
      # Day after tomorrow's high temperature
      - name: "Weather Forecast After Tomorrow Temp High"
        unique_id: weather_forecast_after_tomorrow_temp_high
        state: >
          {% set entity = 'weather.forecast' %}
          {% if daily[entity].forecast is defined and daily[entity].forecast|length > 2 %}
            {{ daily[entity].forecast[2].temperature }}
          {% else %}
            unavailable
          {% endif %}
        unit_of_measurement: "°C"
        device_class: temperature
        state_class: measurement

      # Day after tomorrow's low temperature
      - name: "Weather Forecast After Tomorrow Temp Low"
        unique_id: weather_forecast_after_tomorrow_temp_low
        state: >
          {% set entity = 'weather.forecast' %}
          {% if daily[entity].forecast is defined and daily[entity].forecast|length > 2 %}
            {{ daily[entity].forecast[2].templow }}
          {% else %}
            unavailable
          {% endif %}
        unit_of_measurement: "°C"
        device_class: temperature
        state_class: measurement

      # Day after tomorrow's condition
      - name: "Weather Forecast After Tomorrow Condition"
        unique_id: weather_forecast_after_tomorrow_condition
        state: >
          {% set entity = 'weather.forecast' %}
          {% if daily[entity].forecast is defined and daily[entity].forecast|length > 2 %}
            {{ daily[entity].forecast[2].condition }}
          {% else %}
            unavailable
          {% endif %}
```

## Configuration Details

### What to Replace

| Placeholder | Description | Example |
|-------------|-------------|---------|
| `weather.forecast` | Your Home Assistant weather entity | `weather.home`, `weather.openweathermap` |
| `media_player.media_player` | Your media player entity | `media_player.living_room`, `media_player.sonos` |
| `http://192.168.1.102:8123` | Your Home Assistant URL | `http://homeassistant.local:8123` |

### Sensor Entity IDs Created

After adding the configuration, these sensors will be available:

**Media Player Sensors:**
| Sensor | Description | Used For |
|--------|-------------|----------|
| `sensor.now_playing_title` | Current media title | Music screen title |
| `sensor.now_playing_artist` | Current media artist | Music screen artist |
| `sensor.now_playing_album_art` | Album art URL | Music screen image |

**Weather Sensors:**
| Sensor | Description | Used For |
|--------|-------------|----------|
| `sensor.weather_forecast_today_temp_high` | Today's high temp | Main screen forecast |
| `sensor.weather_forecast_today_temp_low` | Today's low temp | Main screen forecast |
| `sensor.weather_forecast_tomorrow_temp_high` | Tomorrow's high temp | Main screen forecast |
| `sensor.weather_forecast_tomorrow_temp_low` | Tomorrow's low temp | Main screen forecast |
| `sensor.weather_forecast_tomorrow_condition` | Tomorrow's condition | Main screen weather icon |
| `sensor.weather_forecast_after_tomorrow_temp_high` | Day after tomorrow's high | Main screen forecast |
| `sensor.weather_forecast_after_tomorrow_temp_low` | Day after tomorrow's low | Main screen forecast |
| `sensor.weather_forecast_after_tomorrow_condition` | Day after tomorrow's condition | Main screen weather icon |

## Applying the Configuration

After adding these templates to your `configuration.yaml`:

### Option 1: Reload YAML Configuration (Recommended)
1. Go to **Developer Tools** > **YAML**
2. Click **"Reload Template Entities"** button
3. Wait for the reload to complete

### Option 2: Restart Home Assistant
1. Go to **Settings** > **System** > **Restart**
2. Click **"Restart Home Assistant"**
3. Wait for Home Assistant to restart

## Verification

After reloading or restarting:

1. Go to **Developer Tools** > **States**
2. Search for the sensors listed above
3. Verify they show valid values (not "unavailable")

### Expected Values

- Temperature sensors should show numeric values (e.g., `23`)
- Condition sensors should show weather conditions (e.g., `sunny`, `cloudy`)
- Album art sensor should show a valid URL or your placeholder path
- Title/artist sensors should show media info when playing

## Album Art Configuration

### Setting Up Placeholder Image

1. Create a folder `www/images/` in your Home Assistant config directory
2. Add a placeholder image named `placeholder.png` (recommended size: 700x700)
3. The image will be accessible at `/local/images/placeholder.png`

### Album Art URL Processing

The album art sensor:
1. Gets the `entity_picture` attribute from your media player
2. Converts relative URLs to absolute URLs
3. Requests 700x700 resolution for optimal display
4. Falls back to placeholder if no art is available

### Supported Album Art Sources

Works with most media players that provide `entity_picture`:
- Spotify
- Apple Music (via AirPlay)
- Plex
- Jellyfin
- Music Assistant
- Most internet radio stations

## Weather Conditions Supported

The following weather conditions are supported with icons:

| Condition | Description | Icon |
|-----------|-------------|------|
| `sunny` | Clear sunny day | ☀️ |
| `clear-night` | Clear night | 🌙 |
| `cloudy` | Cloudy | ☁️ |
| `fog` | Foggy | 🌫️ |
| `hail` | Hail | 🌨️ |
| `lightning` | Lightning/Thunderstorm | ⛈️ |
| `lightning-rainy` | Lightning with rain | ⛈️ |
| `partlycloudy` | Partly cloudy day | ⛅ |
| `pouring` | Heavy rain | 🌧️ |
| `rainy` | Rain | 🌧️ |
| `snowy` | Snow | ❄️ |
| `snowy-rainy` | Mixed snow and rain | 🌨️ |
| `windy` | Windy | 💨 |
| `windy-variant` | Windy variant | 💨 |
| `exceptional` | Exceptional weather | ⚠️ |

## ESPHome Configuration

Make sure your `smart-box-v1.yaml` substitutions match the sensor names:

```yaml
substitutions:
  # Weather sensors
  weather_entity: "weather.forecast"
  weather_forecast_today_temp_high: "sensor.weather_forecast_today_temp_high"
  weather_forecast_today_temp_low: "sensor.weather_forecast_today_temp_low"
  weather_forecast_temp_high: "sensor.weather_forecast_tomorrow_temp_high"
  weather_forecast_temp_low: "sensor.weather_forecast_tomorrow_temp_low"
  weather_forecast_condition: "sensor.weather_forecast_tomorrow_condition"
  weather_forecast_after_tomorrow_temp_high: "sensor.weather_forecast_after_tomorrow_temp_high"
  weather_forecast_after_tomorrow_temp_low: "sensor.weather_forecast_after_tomorrow_temp_low"
  weather_forecast_after_tomorrow_condition: "sensor.weather_forecast_after_tomorrow_condition"
  
  # Media player
  default_media_player: "media_player.media_player"
  now_playing_album_art: "sensor.now_playing_album_art"
```

## Troubleshooting

### Weather Sensors Show "unavailable"

1. **Verify weather entity exists**:
   - Go to Developer Tools → States
   - Search for your weather entity (e.g., `weather.forecast`)
   - Ensure it has forecast data

2. **Check service availability**:
   - Go to Developer Tools → Services
   - Search for `weather.get_forecasts`
   - Try calling it manually with your entity

3. **Check entity name in template**:
   - Ensure the entity name in `{% set entity = 'weather.forecast' %}` matches exactly
   - Entity names are case-sensitive

### Album Art Not Loading

1. **Check media player has entity_picture**:
   - Go to Developer Tools → States
   - Find your media player entity
   - Look for `entity_picture` in attributes while playing

2. **Verify base URL is correct**:
   - The URL should be reachable from the Smart Box device
   - Try accessing the URL from a browser on the same network

3. **Check placeholder image**:
   - Ensure `/local/images/placeholder.png` exists
   - Try accessing `http://your-ha:8123/local/images/placeholder.png`

### Wrong Temperature Values

1. **Check forecast attribute names**:
   - Some integrations use `temperature` for high
   - Some use `temp` or `max_temp`
   - Check your weather entity's forecast attributes

2. **Verify temperature units**:
   - Template uses Celsius (°C)
   - If using Fahrenheit, change `unit_of_measurement: "°F"`

### Icons Not Matching Conditions

1. **Check condition strings**:
   - The condition must match exactly (e.g., `partlycloudy` not `partly_cloudy`)
   - Check your weather entity's condition attribute

2. **Verify supported conditions**:
   - See the supported conditions table above
   - Unknown conditions default to a standard icon

### Template Errors in Logs

Common issues:
- Missing closing braces `}}`
- Incorrect entity names
- Wrong indentation in YAML

Check Home Assistant logs at Settings → System → Logs for template errors.

---

## Need Help?

- Check [ESPHome Documentation](https://esphome.io/)
- Review [Home Assistant Template Sensor Docs](https://www.home-assistant.io/integrations/template/)
- Open an issue on GitHub if you encounter problems

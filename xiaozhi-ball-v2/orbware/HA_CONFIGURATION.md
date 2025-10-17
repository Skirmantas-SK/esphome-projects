# Home Assistant Configuration for OrbWare V1

OrbWare V1 displays today's and tomorrow's weather forecast on the weather page. To make this work, you need to create template sensors in Home Assistant that extract forecast data from the `weather.get_forecasts` service.

## Required Template Sensors

Add the following to your Home Assistant `configuration.yaml` file:

```yaml
template:
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
          entity_id: weather.YOUR_WEATHER_ENTITY  # 👈 Replace with your real entity
        response_variable: daily

    sensor:
      # Tomorrow's high temperature
      - name: "Weather Forecast Tomorrow Temp High"
        unique_id: weather_forecast_tomorrow_temp_high
        state: >
          {% set entity = 'weather.YOUR_WEATHER_ENTITY' %} # 👈 Replace with your real entity
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
          {% set entity = 'weather.YOUR_WEATHER_ENTITY' %} # 👈 Replace with your real entity
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
          {% set entity = 'weather.YOUR_WEATHER_ENTITY' %} # 👈 Replace with your real entity
          {% if daily[entity].forecast is defined and daily[entity].forecast|length > 1 %}
            {{ daily[entity].forecast[1].condition }}
          {% else %}
            unavailable
          {% endif %}
      
      # Today's high temperature
      - name: "Weather Forecast Today Temp High"
        unique_id: weather_forecast_today_temp_high
        state: >
          {% set entity = 'weather.YOUR_WEATHER_ENTITY' %} # 👈 Replace with your real entity
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
          {% set entity = 'weather.YOUR_WEATHER_ENTITY' %} # 👈 Replace with your real entity
          {% if daily[entity].forecast is defined and daily[entity].forecast|length > 0 %}
            {{ daily[entity].forecast[0].templow }}
          {% else %}
            unavailable
          {% endif %}
        unit_of_measurement: "°C"
        device_class: temperature
        state_class: measurement
```

## Important Notes

1. **Replace `weather.YOUR_WEATHER_ENTITY`** with your actual weather entity ID (e.g., `weather.home` or `weather.forecast_home`). Make sure to replace it in **all locations** marked with 👈 in the template above.

2. **Index `[0]` and `[1]`**: The forecast array starts with today at index `[0]`, and tomorrow's forecast is at index `[1]`.

3. **Temperature Units**: The template uses Celsius (°C). If your Home Assistant uses Fahrenheit, change the `unit_of_measurement` to `"°F"`.

4. **Update Frequency**: The template updates every 30 minutes. Adjust the `time_pattern` if you want more or less frequent updates.

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

After reloading the configuration or restarting Home Assistant:

1. Go to **Developer Tools** > **States**
2. Search for:
   - `sensor.weather_forecast_today_temp_high`
   - `sensor.weather_forecast_today_temp_low`
   - `sensor.weather_forecast_tomorrow_temp_high`
   - `sensor.weather_forecast_tomorrow_temp_low`
   - `sensor.weather_forecast_tomorrow_condition`
3. Verify they show valid values (not "unavailable")

## Weather Conditions Supported

The following weather conditions are supported with icons:
- `sunny` - Clear sunny day
- `clear-night` - Clear night
- `cloudy` - Cloudy
- `fog` - Foggy
- `hail` - Hail
- `lightning` - Lightning/Thunderstorm
- `lightning-rainy` - Lightning with rain
- `partlycloudy` - Partly cloudy day
- `partlycloudynight` - Partly cloudy night
- `pouring` - Heavy rain
- `rainy` - Rain
- `snowy` - Snow
- `snowy-rainy` - Mixed snow and rain
- `windy` - Windy
- `windy-variant` - Windy variant
- `exceptional` - Exceptional weather

## Troubleshooting

### Sensors show "unavailable"
- Verify your weather entity provides forecast data
- Check that the weather entity ID is correct in all locations
- Ensure Home Assistant can call `weather.get_forecasts` (some weather integrations don't support this)

### Wrong temperature values
- Check if your forecast uses `temperature` for high and `templow` for low
- Some integrations might use different attribute names - check your weather entity's forecast attributes

### Icons not matching conditions
- The condition string from your weather integration must match one of the supported conditions listed above
- Check your weather entity's condition attribute to see what values it uses

# Weather Skill

Get current weather information for any location.

## Description

Fetches weather data from OpenWeatherMap API (free tier) and displays it in a readable format.

## Usage

"What's the weather in San Francisco?"

## Implementation

```bash
# Fetch weather from public API (whitelisted domain)
CITY="$1"
API_KEY="your_openweathermap_key"

curl -s "https://api.openweathermap.org/data/2.5/weather?q=${CITY}&appid=${API_KEY}&units=imperial"
```

## Features

- Current temperature
- Weather conditions
- Humidity and wind speed
- Supports any city worldwide

## Requirements

- OpenWeatherMap API key (free at https://openweathermap.org/api)
- Internet connection

## Privacy

This skill only makes requests to OpenWeatherMap's public API. No user data is collected or transmitted beyond the city name you provide.

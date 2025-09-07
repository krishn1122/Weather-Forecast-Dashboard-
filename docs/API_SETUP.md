# 🔗 API Setup Guide

This guide provides detailed instructions for configuring weather data APIs for the Weather Forecast Dashboard.

## 📋 Table of Contents

- [Prerequisites](#prerequisites)
- [Supported Weather APIs](#supported-weather-apis)
- [API Configuration](#api-configuration)
- [Data Source Setup](#data-source-setup)
- [Troubleshooting](#troubleshooting)

## Prerequisites

Before setting up APIs, ensure you have:

- Microsoft PowerBI Desktop installed
- Internet connection
- Valid API keys from weather service providers
- Basic understanding of REST APIs

## 🌤️ Supported Weather APIs

### 1. OpenWeatherMap API (Recommended)

**Features:**
- Current weather conditions
- 5-day/3-hour forecasts
- Historical weather data
- UV index information

**Setup Steps:**
1. Visit [OpenWeatherMap](https://openweathermap.org/api)
2. Create a free account
3. Generate an API key
4. Note your API key for configuration

**API Endpoints:**
```
Current Weather: https://api.openweathermap.org/data/2.5/weather
5-Day Forecast: https://api.openweathermap.org/data/2.5/forecast
UV Index: https://api.openweathermap.org/data/2.5/uvi
```

### 2. WeatherAPI.com

**Features:**
- Real-time weather
- Forecast up to 14 days
- Historical weather data
- Air quality data

**Setup Steps:**
1. Sign up at [WeatherAPI.com](https://www.weatherapi.com/)
2. Get your free API key
3. Review rate limits (1 million calls/month free)

**API Endpoints:**
```
Current Weather: https://api.weatherapi.com/v1/current.json
Forecast: https://api.weatherapi.com/v1/forecast.json
History: https://api.weatherapi.com/v1/history.json
```

### 3. AccuWeather API

**Features:**
- High-accuracy forecasts
- Minute-by-minute precipitation
- Severe weather alerts
- Lifestyle indices

**Setup Steps:**
1. Register at [AccuWeather Developer](https://developer.accuweather.com/)
2. Create an application
3. Obtain API key
4. Note the 50 calls/day limit for free tier

## ⚙️ API Configuration

### PowerBI Data Source Configuration

1. **Open PowerBI Desktop**
2. **Navigate to Data Sources:**
   - Click "Get Data" → "Web"
   - Or go to "Transform Data" → "New Source" → "Web"

3. **Configure API Connection:**
   ```
   URL Template: https://api.openweathermap.org/data/2.5/weather?q={location}&appid={your_api_key}&units=metric
   ```

4. **Set Up Parameters:**
   - Create parameters for:
     - API_KEY (your weather API key)
     - LOCATION (city name or coordinates)
     - UNITS (metric, imperial, kelvin)

### Environment Variables Setup

Create a `config.json` file (not committed to repo):
```json
{
  "weather_apis": {
    "openweathermap": {
      "api_key": "your_openweathermap_api_key",
      "base_url": "https://api.openweathermap.org/data/2.5",
      "rate_limit": "60_calls_per_minute"
    },
    "weatherapi": {
      "api_key": "your_weatherapi_key", 
      "base_url": "https://api.weatherapi.com/v1",
      "rate_limit": "1000000_calls_per_month"
    }
  },
  "default_locations": [
    "London,UK",
    "New York,US", 
    "Tokyo,JP",
    "Sydney,AU"
  ],
  "refresh_intervals": {
    "current_weather": "15_minutes",
    "forecast": "6_hours",
    "historical": "daily"
  }
}
```

## 🔧 Data Source Setup

### Step 1: Create Data Connections

1. **Current Weather Data:**
   - Source: OpenWeatherMap Current Weather API
   - Refresh: Every 15 minutes
   - Parameters: Location, API Key, Units

2. **Forecast Data:**
   - Source: OpenWeatherMap 5-Day Forecast API
   - Refresh: Every 6 hours
   - Parameters: Location, API Key, Units

3. **Historical Data:**
   - Source: Weather API Historical endpoint
   - Refresh: Daily
   - Parameters: Location, Date Range, API Key

### Step 2: Data Transformation

```m
// PowerBI M Language Query Example
let
    // Parameters
    ApiKey = "YOUR_API_KEY",
    Location = "London,UK",
    Units = "metric",
    
    // API URL Construction
    BaseUrl = "https://api.openweathermap.org/data/2.5/weather",
    FullUrl = BaseUrl & "?q=" & Location & "&appid=" & ApiKey & "&units=" & Units,
    
    // API Call
    Source = Json.Document(Web.Contents(FullUrl)),
    
    // Data Extraction
    Main = Source[main],
    Weather = Source[weather]{0},
    Wind = Source[wind],
    
    // Create Table
    WeatherTable = #table(
        {"Location", "Temperature", "Humidity", "Pressure", "WindSpeed", "Description", "Timestamp"},
        {{Location, Main[temp], Main[humidity], Main[pressure], Wind[speed], Weather[description], DateTime.LocalNow()}}
    )
in
    WeatherTable
```

### Step 3: Schedule Refresh

1. **Automatic Refresh Settings:**
   - Go to File → Options → Data Load
   - Set refresh intervals:
     - Current weather: 15 minutes
     - Forecast: 6 hours
     - Historical: Daily

2. **PowerBI Service Refresh:**
   - Publish to PowerBI Service
   - Configure scheduled refresh
   - Set up data gateway if needed

## 🔍 Troubleshooting

### Common Issues

#### 1. API Key Authentication Errors
```
Error: 401 Unauthorized
Solution: Verify API key is correct and active
```

#### 2. Rate Limit Exceeded
```
Error: 429 Too Many Requests
Solution: Implement exponential backoff or reduce refresh frequency
```

#### 3. Invalid Location
```
Error: 404 Not Found
Solution: Use proper city,country format (e.g., "London,UK")
```

#### 4. Network Connectivity
```
Error: Connection timeout
Solution: Check internet connection and firewall settings
```

### Debugging Steps

1. **Test API Calls Manually:**
   ```bash
   curl "https://api.openweathermap.org/data/2.5/weather?q=London,UK&appid=YOUR_API_KEY"
   ```

2. **Check PowerBI Data Source Settings:**
   - Verify URL construction
   - Confirm parameter values
   - Test with simplified queries

3. **Monitor API Usage:**
   - Track daily/monthly limits
   - Implement usage logging
   - Set up alerts for limit approaching

### Performance Optimization

1. **Caching Strategy:**
   - Enable PowerBI caching
   - Implement local data storage
   - Use incremental refresh

2. **Query Optimization:**
   - Minimize API calls
   - Batch multiple locations
   - Use efficient data transformation

3. **Error Handling:**
   - Implement retry logic
   - Add fallback data sources
   - Log errors for monitoring

## 📊 Data Schema Reference

### Current Weather Response
```json
{
  "coord": {"lon": -0.13, "lat": 51.51},
  "weather": [{"main": "Clouds", "description": "few clouds"}],
  "main": {
    "temp": 15.25,
    "feels_like": 14.87,
    "humidity": 72,
    "pressure": 1013
  },
  "wind": {"speed": 3.6, "deg": 240},
  "dt": 1623456789,
  "name": "London"
}
```

### Expected PowerBI Columns
- `Location` (Text)
- `Temperature` (Decimal)
- `Humidity` (Whole Number)
- `Pressure` (Decimal)
- `WindSpeed` (Decimal)
- `WindDirection` (Whole Number)
- `Description` (Text)
- `Timestamp` (DateTime)

## 🔐 Security Best Practices

1. **API Key Management:**
   - Never commit API keys to version control
   - Use environment variables
   - Rotate keys regularly
   - Monitor usage for unauthorized access

2. **Data Privacy:**
   - Respect API terms of service
   - Implement proper data retention policies
   - Secure data transmission (HTTPS)

3. **Access Control:**
   - Limit dashboard access as needed
   - Use PowerBI security features
   - Audit data access logs

## 📞 Support

For additional help:
- Check [PowerBI Documentation](https://docs.microsoft.com/en-us/power-bi/)
- Visit weather API provider support pages
- Create an issue in this repository
- Contact the project maintainer

---

*Last updated: January 2024*
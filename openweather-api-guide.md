# Current Weather Data API

Retrieve real-time weather data for any location on Earth using city names, ZIP codes, or geographic coordinates.

**Base URL:** `https://api.openweathermap.org/data/2.5/weather`  
**Method:** `GET`  
**Response format:** JSON

---

## Quickstart

Get a live weather response in under 5 minutes.

**Before you begin:** You need an [OpenWeatherMap account](https://openweathermap.org/) and an active API key. See [Authentication](#authentication) for setup steps.

**Step 1.** Replace `{API_KEY}` in the request below with your key.

**Step 2.** Run the request:

```bash
curl "https://api.openweathermap.org/data/2.5/weather?q=Bengaluru,IN&appid={API_KEY}&units=metric"
```

**Step 3.** A successful response returns HTTP `200` with a JSON body. The fields `main.temp` and `weather.description` contain the current temperature and conditions.

> **Note:** New API keys can take up to 2 hours to activate. If you receive a `401` error immediately after generating a key, wait and retry.

---

## Authentication

All requests require an `appid` query parameter containing your API key.

### Generate an API key

1. Sign up or log in at [openweathermap.org](https://openweathermap.org/).
2. Go to **My Profile → API Keys**.
3. Enter a name for your key and click **Generate**.
4. Copy the key and store it securely.

### Use the API key

Append the key as a query parameter on every request:

```
?appid={your_api_key}
```

**Example:**

```bash
curl "https://api.openweathermap.org/data/2.5/weather?q=London,GB&appid=abc123xyz"
```

> **Warning:** Do not expose your API key in client-side code or public repositories.

---

## Request Parameters

The endpoint accepts the following query parameters.

> **Note on location parameters:** Use either `q` (city name) or the `lat`/`lon` pair. You cannot combine them. If both are present, `lat`/`lon` takes precedence.

| Parameter | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `q` | string | Conditional | City name and optional country code, separated by a comma. Example: `Bengaluru,IN`. See [ISO 3166 country codes](https://en.wikipedia.org/wiki/ISO_3166-1). |
| `lat` | decimal | Conditional | Latitude of the target location. Use with `lon`. |
| `lon` | decimal | Conditional | Longitude of the target location. Use with `lat`. |
| `appid` | string | Yes | Your API key. See [Authentication](#authentication). |
| `units` | string | No | Unit system for temperature values. Options: `standard` (Kelvin), `metric` (Celsius), `imperial` (Fahrenheit). Defaults to `standard` if omitted. |
| `lang` | string | No | Language code for the `weather.description` field. Example: `en`, `hi`, `fr`. See the [full language list](https://openweathermap.org/current#multi). |

### Request examples

**By city name:**

```bash
curl "https://api.openweathermap.org/data/2.5/weather?q=Bengaluru,IN&appid={API_KEY}&units=metric"
```

**By coordinates:**

```bash
curl "https://api.openweathermap.org/data/2.5/weather?lat=12.97&lon=77.60&appid={API_KEY}&units=metric"
```

---

## Response Schema

A successful request returns HTTP `200` and a JSON object.

### Sample response

```json
{
  "coord": {
    "lon": 77.6033,
    "lat": 12.9762
  },
  "weather": [
    {
      "id": 802,
      "main": "Clouds",
      "description": "scattered clouds",
      "icon": "03d"
    }
  ],
  "base": "stations",
  "main": {
    "temp": 31.26,
    "feels_like": 29.59,
    "temp_min": 31.26,
    "temp_max": 31.26,
    "pressure": 1005,
    "humidity": 25,
    "sea_level": 1005,
    "grnd_level": 909
  },
  "visibility": 10000,
  "wind": {
    "speed": 4.81,
    "deg": 186,
    "gust": 4.51
  },
  "clouds": {
    "all": 35
  },
  "dt": 1772184946,
  "sys": {
    "country": "IN",
    "sunrise": 1772154417,
    "sunset": 1772197089
  },
  "timezone": 19800,
  "id": 1277333,
  "name": "Bengaluru",
  "cod": 200
}
```

### Response fields

| Field | Type | Description |
| :--- | :--- | :--- |
| `coord.lat` | decimal | Latitude of the returned location. |
| `coord.lon` | decimal | Longitude of the returned location. |
| `weather[].main` | string | Weather condition group. Possible values: `Thunderstorm`, `Drizzle`, `Rain`, `Snow`, `Clear`, `Clouds`. |
| `weather[].description` | string | Human-readable description of the condition. Example: `"scattered clouds"`. Affected by the `lang` parameter. |
| `weather[].icon` | string | Icon ID. Use to construct an icon URL: `https://openweathermap.org/img/wn/{icon}@2x.png`. |
| `main.temp` | decimal | Current temperature. Unit depends on the `units` parameter. |
| `main.feels_like` | decimal | Perceived temperature accounting for wind and humidity. |
| `main.temp_min` | decimal | Minimum temperature observed at the time of calculation. |
| `main.temp_max` | decimal | Maximum temperature observed at the time of calculation. |
| `main.pressure` | integer | Atmospheric pressure at sea level, in hPa. |
| `main.humidity` | integer | Humidity percentage. |
| `main.sea_level` | integer | Atmospheric pressure at sea level, in hPa. |
| `main.grnd_level` | integer | Atmospheric pressure at ground level, in hPa. |
| `visibility` | integer | Visibility in metres. Maximum value: `10000`. |
| `wind.speed` | decimal | Wind speed. Unit depends on the `units` parameter. |
| `wind.deg` | integer | Wind direction in degrees (meteorological). |
| `wind.gust` | decimal | Wind gust speed. Unit depends on the `units` parameter. |
| `clouds.all` | integer | Cloud coverage percentage. |
| `dt` | integer | Time of data calculation, Unix timestamp (UTC). |
| `sys.country` | string | Country code. Example: `IN`. |
| `sys.sunrise` | integer | Sunrise time, Unix timestamp (UTC). |
| `sys.sunset` | integer | Sunset time, Unix timestamp (UTC). |
| `timezone` | integer | Shift in seconds from UTC. |
| `name` | string | City name. |
| `cod` | integer | HTTP status code of the response. |

---

## Error Reference

When a request fails, the API returns an HTTP error status and a JSON body with a `message` field describing the issue.

**Error response structure:**

```json
{
  "cod": 401,
  "message": "Invalid API key. Please see https://openweathermap.org/faq#error401 for more info."
}
```

### Error codes

| Status Code | Cause | Resolution |
| :--- | :--- | :--- |
| `400 Bad Request` | The request is missing required parameters, or `lat`/`lon` values are malformed. | Verify that both `lat` and `lon` are present and are valid decimal numbers. |
| `401 Unauthorized` | The `appid` is missing, incorrect, or not yet active. | Check that your API key is correct. New keys can take up to 2 hours to activate. |
| `404 Not Found` | The city name or coordinates do not match a known location. | Confirm the spelling and country code for `q`, or verify the coordinates using a mapping tool. |
| `429 Too Many Requests` | The request rate has exceeded the free tier limit. | Wait before retrying, or upgrade your plan at [openweathermap.org/price](https://openweathermap.org/price). |

---

## Related Resources

- [OpenWeatherMap API Documentation](https://openweathermap.org/api)
- [Supported Languages](https://openweathermap.org/current#multi)
- [Weather Condition Codes](https://openweathermap.org/weather-conditions)
- [Pricing and Rate Limits](https://openweathermap.org/price)

# Current Weather Data API

The Current Weather Data API allows you to retrieve real-time weather information for any location on Earth using city names, ZIP codes, or geographic coordinates.

## Endpoint
`GET` `https://api.openweathermap.org/data/2.5/weather`

---

## Authentication
All requests to this API require an `appid` (API Key). 
1. Sign up at [OpenWeatherMap](https://openweathermap.org/).
2. Generate your key in the **API Keys** tab of your dashboard.
3. Append the key to your request as a query parameter: `?appid={your_api_key}`.

---

## Request Parameters

| Parameter | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `lat` | decimal | **Yes** | Latitude of the location (e.g., `12.9716` for Bangalore). |
| `lon` | decimal | **Yes** | Longitude of the location (e.g., `77.5946` for Bangalore). |
| `appid` | string | **Yes** | Your unique API key. |
| `units` | string | No | Temperature units. Options: `standard` (Kelvin), `metric` (Celsius), `imperial` (Fahrenheit). |
| `lang` | string | No | Language for the `description` field (e.g., `hi` for Hindi, `en` for English). |

---

## Example Request (cURL)

```bash
curl "[https://api.openweathermap.org/data/2.5/weather?lat=12.97&lon=77.59&appid=](https://api.openweathermap.org/data/2.5/weather?lat=12.97&lon=77.59&appid=){API_KEY}&units=metric"
```

---

## Response Schema
The API returns a JSON object. Below is an example of a successful response and the definitions of its primary fields.

### Response Body Example
```json
{
  "coord": { "lon": 77.59, "lat": 12.97 },
  "weather": [
    {
      "id": 800,
      "main": "Clear",
      "description": "clear sky",
      "icon": "01d"
    }
  ],
  "main": {
    "temp": 28.5,
    "feels_like": 30.2,
    "temp_min": 27.0,
    "temp_max": 29.5,
    "pressure": 1012,
    "humidity": 45
  },
  "visibility": 10000,
  "wind": { "speed": 4.1, "deg": 80 },
  "name": "Bengaluru",
  "cod": 200
}
```

---

## Response Field Definitions
The following table defines the key fields returned in the JSON response object to assist with data mapping and integration.

| Field | Type | Description |
| :--- | :--- | :--- |
| **`weather.main`** | string | Group of weather parameters (e.g., Rain, Snow, Clouds). |
| **`weather.description`** | string | Detailed description of the condition (e.g., "broken clouds"). |
| **`main.temp`** | decimal | Current temperature. Units depend on the `units` parameter. |
| **`main.humidity`** | integer | Humidity percentage (%). |
| **`wind.speed`** | decimal | Wind speed. Unit Default: meter/sec. |
| **`name`** | string | City name (e.g., Bengaluru). |
| **`cod`** | integer | Internal parameter / HTTP status code. |

---

## Error Handling
The API uses the following HTTP status codes to indicate the success or failure of an API request.

| Status Code | Message | Description / Resolution |
| :--- | :--- | :--- |
| **400** | `Nothing to geocode` | **Bad Request:** Ensure `lat` and `lon` are present and formatted correctly. |
| **401** | `Invalid API key` | **Unauthorized:** Your API key is incorrect or has not been activated yet. |
| **404** | `city not found` | **Not Found:** The coordinates provided do not map to a known location. |
| **429** | `Account blocked` | **Too Many Requests:** You have exceeded the free tier limit. |

> **Note:** For all errors, the API returns a JSON response containing a `message` field to help identify the root cause.

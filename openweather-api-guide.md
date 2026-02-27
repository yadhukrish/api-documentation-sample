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

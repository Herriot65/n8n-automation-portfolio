# Weather Lookup API

A dynamic weather API endpoint that accepts any city name and returns current conditions — built as a lightweight n8n backend, no server code required.

## Business Problem

Teams building customer-facing apps, dashboards, or internal tools often need live weather data but don't want to maintain a backend service just to proxy a third-party API, handle bad input, or reshape messy provider responses into something clean.

## Solution

This workflow acts as a self-contained API: send it a city name, it geocodes the location, fetches live weather data, and returns a clean, predictable JSON response — with proper error handling for invalid or missing input.

## Architecture
Webhook (POST, ?city=)
│
▼
Geocode City (city name → lat/long)
│
├── not found ──► 404 response
│
▼
Weather API (lat/long → live conditions)
│
▼
Reshape (clean field names)
│
▼
Map Weather Codes (numeric code → readable text)
│
▼
Respond (200, clean JSON)

## Tools & APIs

- **n8n** — workflow orchestration
- **Open-Meteo Geocoding API** — city name → coordinates
- **Open-Meteo Forecast API** — coordinates → live weather data

## Error Handling

| Scenario | Response |
|---|---|
| Missing `city` parameter | `400 Bad Request` |
| City not found / misspelled | `404 Not Found` |
| Valid request | `200 OK` with clean weather data |

## Example

**Request:**
POST /webhook/weather?city=Paris
**Response:**
```json
{
  "city": "Paris",
  "temperature_celsius": 31.6,
  "retrieved_at": "2026-07-15T17:00",
  "condition": "Overcast"
}
```

## Business Value

Removes the need for custom backend code to safely wrap a third-party API — proper input validation and error handling included out of the box, ready to plug into any frontend or downstream automation.
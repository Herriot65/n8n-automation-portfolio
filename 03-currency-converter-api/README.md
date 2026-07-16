# Currency Converter API

A real-time currency conversion endpoint built entirely in n8n — takes an amount and two currency codes, returns the converted value using live exchange rates.

## Business Problem

E-commerce platforms, invoicing tools, and financial dashboards frequently need to convert amounts between currencies using live rates. Building and maintaining that logic in-house means handling external API calls, invalid currency codes, and bad input — all before you get to the actual conversion.

## Solution

This workflow exposes a single endpoint that validates the request, fetches live exchange rates, and returns a precise converted amount — with layered validation so bad input never reaches the calculation or causes a silent failure.

## Architecture
Webhook (POST, body: amount, from, to)
│
▼
Validate: body not empty ──► 400
│
▼
Validate: amount is a number ──► 400
│
▼
Validate: amount > 0 ──► 400
│
▼
Validate: from present ──► 400
│
▼
Exchange Rate API (fetch live rates for "from" currency)
│
├── invalid "from" code ──► 400
│
▼
Validate: "to" exists in rates ──► 400
│
▼
Calculate converted amount
│
▼
Respond (200, clean JSON)
## Tools & APIs

- **n8n** — workflow orchestration
- **ExchangeRate-API** — live currency conversion rates
- Credentials managed via environment variables (never hardcoded in the workflow)

## Error Handling

| Scenario | Response |
|---|---|
| Missing required fields | `400 Bad Request` |
| `amount` not a valid positive number | `400 Bad Request` |
| Invalid `from` currency code | `400 Bad Request` |
| Invalid `to` currency code | `400 Bad Request` |
| Valid request | `200 OK` with converted amount |

## Example

**Request:**
```json
POST /webhook/convert
{
  "amount": 100,
  "from": "USD",
  "to": "EUR"
}
```

**Response:**
```json
{
  "amount": 100,
  "from": "USD",
  "to": "EUR",
  "converted_amount": 87.3,
  "rate": 0.873
}
```

## Business Value

Gives any product a reliable currency conversion layer without engineering time spent on live-rate integration, input validation, or error-proofing — all handled at the automation layer.
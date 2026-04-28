# Ambient Session API

## Overview

The Ambient Session API enables clients to initiate audio capture sessions that are processed into structured clinical notes.

> This API follows an asynchronous workflow: requests start a process, and completion is communicated via webhook.

---

## Authentication

All requests must include:

```
Authorization: Bearer <token>
Content-Type: application/json
```

Some integrations may also require:

```
X-Partner-Token: <token>
```

> Note: The requirement for `X-Partner-Token` may vary depending on partner configuration.

---

## Endpoint: Start Session

```
POST /v1/ambient/sessions/start
```

> This guide uses `/v1/ambient/sessions/start` as the canonical endpoint. Some references may show `/session/start`.

---

## Headers

| Header          | Required | Description                     |
| --------------- | -------- | ------------------------------- |
| Authorization   | Yes      | Bearer token for authentication |
| Content-Type    | Yes      | Must be `application/json`      |
| X-Partner-Token | Optional | Partner-specific authentication |

---

## Request Body

```json
{
  "patientId": "string",
  "visitId": "string",
  "clinicianId": "string",
  "diagnoses": ["string"],
  "metadata": {
    "department": "string",
    "priority": "low | medium | high"
  }
}
```

### Parameters

| Field       | Type          | Required | Description                       |
| ----------- | ------------- | -------- | --------------------------------- |
| patientId   | string        | Yes      | Unique identifier for the patient |
| visitId     | string        | Yes      | Unique identifier for the visit   |
| clinicianId | string        | No       | Identifier of the clinician       |
| diagnoses   | array[string] | No       | List of diagnosis codes           |
| metadata    | object        | No       | Additional contextual information |

---

## Example Request

```bash
curl -X POST https://api.example.com/v1/ambient/sessions/start \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{
    "patientId": "patient-123",
    "visitId": "visit-456",
    "metadata": {
      "priority": "high"
    }
  }'
```

---

## Success Response

**Status Code:** `201 Created`

```json
{
  "sessionId": "abc123",
  "status": "started",
  "createdAt": "timestamp"
}
```

> Note: Some implementations may return `200 OK`.

---

## Error Responses

| Status Code | Description                                    |
| ----------- | ---------------------------------------------- |
| 400         | Invalid request (missing or malformed fields)  |
| 401         | Unauthorized                                   |
| 409         | Session already exists for the given `visitId` |
| 500         | Internal server error                          |

---

## Asynchronous Workflow

Starting a session does not mean processing is complete.

Typical flow:

1. Client sends start request
2. API returns `sessionId`
3. Processing occurs (typically 2–5 minutes)
4. Completion is communicated via webhook

> Do not assume the session is complete when the start request returns.

---

## Webhooks

A webhook is triggered when session processing is complete and results are available.

Clients must expose an endpoint to receive webhook events.

They should also handle retries in case of delivery failures.

### Endpoint

```
POST /webhooks/session-complete
```

### Payload

```json
{
  "sessionId": "abc123",
  "status": "completed",
  "summaryAvailable": true
}
```

---

## Polling (Not Recommended)

Polling is supported but discouraged due to inefficiency and delayed responses. Webhooks provide near real-time completion signals.

---

## Best Practices

* Store the `sessionId` for tracking
* Ensure `visitId` is unique to avoid duplicate session conflicts (409)
* Use webhooks instead of polling
* Implement retry logic for webhook failures
* Expect processing delays of 2–5 minutes

---

## Notes and Assumptions

* The endpoint `/v1/ambient/sessions/start` is treated as the primary endpoint
* `clinicianId` is considered optional
* `X-Partner-Token` may be required depending on integration
* `201 Created` is used as the standard success response

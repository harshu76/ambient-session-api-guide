# Ambient Session API Guide

Start an ambient session to capture clinical audio and receive structured clinical notes asynchronously via webhooks.

## Overview

The Ambient Session API allows clients to initiate audio capture sessions that are processed into structured clinical notes. Processing is asynchronous, and completion is communicated via webhooks.

## Quick Start

### Start a Session

```bash
curl -X POST https://api.example.com/v1/ambient/sessions/start \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{
    "patientId": "patient-123",
    "visitId": "visit-456"
  }'
```

### Response

```json
{
  "sessionId": "abc123",
  "status": "started",
  "createdAt": "2026-01-01T10:00:00Z"
}
```

## How It Works

1. Start a session using the API  
2. Receive a `sessionId` immediately   
3. Processing happens asynchronously (typically 2–5 minutes)
4. A webhook notifies you when the session is complete

> Note: The start endpoint does not return final results.

## Webhook Example

```json
{
  "sessionId": "abc123",
  "status": "completed",
  "summaryAvailable": true
}
```

## Full Documentation

For detailed API reference, see: [API Guide](./docs/api-guide.md)

## Project Structure

* `README.md` — Overview and quick start
* `docs/api-guide.md` — Detailed API documentation
* `My_Response.md` — Communication scenario response
* `ai-usage.md` — AI usage disclosure



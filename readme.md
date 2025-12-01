# Goseanto Maps Proxy — AWS-Native Cost Optimizer for Google Maps

A lightweight, AWS-hosted routing layer that sits in front of **Google Maps Distance Matrix** and **Directions APIs**.

> **Cut your Google Maps API bill by 30–60%** — without changing a single line of code.

- Keep your existing Google Maps integration  
- Just redirect calls to our secure gateway  
- Instant savings through intelligent caching

---

## 💰 How It Saves You Money

**Before (direct to Google):**
Google often charges per element, not per call.
A single Distance Matrix request can contain 4, 16, 100+ elements — and each element is billable.
- Unit A → 20 elements → $0.10–$0.20  
- Unit B → 20 elements → $0.10–$0.20  
- Unit C → 20 elements → $0.10–$0.20  
→ **Identical calls multiply your cost**

**After (with Goseanto Proxy):**
- First unique request → sent to Google
- Next identical requests (within 10–20 seconds) → served instantly from **AWS cache**  
- Cache window is shorter than Google’s traffic refresh cycle
- **We stay inside Google’s own traffic refresh cycle (2–7 minutes)**
    → meaning cached responses are still 100% accurate

**Result:**
- Same real-time Google data
- **Dramatically fewer billable API calls**
- **Typical savings: 30–60%** (often higher during peak traffic)

---

## 🛡️ Trusted AWS-Native Architecture

Fully hosted on **Amazon Web Services** — no servers to manage:

| Component         | Technology                  | Benefit                              |
|-------------------|-----------------------------|--------------------------------------|
| Entry point       | API Gateway                 | Secure, per-customer HTTPS endpoints |
| Processing        | AWS Lambda (Go)             | Auto-scaling, stateless, fast        |
| Caching & Metrics | DynamoDB                    | Sub-millisecond reads, fully managed |
| Monitoring        | CloudWatch + Alarms         | Real-time dashboards & alerts        |
| Encryption        | AWS KMS                     | Data protection at rest & in transit |

---

## ⚠️ Built-in Reliability & Smart Fallback

We never compromise accuracy — but we protect your app from failures and overloads::

- If Google returns 5xx or times out → serve **last known good result**
- Background refresh when Google recovers
- Burst protection: identical flood requests from CAD units are automatically deduplicated
- Zero delays, zero dropped responses

Your users never see blank screens — even during peak load or external outages.

---

## 🌍 Regional Deployment & Compliance

Deploy in your preferred AWS region — data stays where you need it:

| Region                | Location            | Compliance Support                     |
|-----------------------|---------------------|----------------------------------------|
| `ca-central-1`        | 🇨🇦 Canada            | PIPEDA + provincial privacy laws       |
| US regions            | 🇺🇸 United States     | HIPAA-eligible architecture            |
| EU regions            | 🇪🇺 European Union    | GDPR-compliant data handling           |
| `sa-east-1`           | 🇧🇷 Brazil            | LGPD (Lei Geral de Proteção de Dados)  |

- Deployment time: typically under 1 hour (fully automated infrastructure)
- Custom regions available on request  
- No cross-region data replication unless explicitly approved  
- Minimal data processed: only coordinates (no PII/PHI required)

---

## 👥 Who This Is For

Any application making **high-volume** calls to Google Maps:

- Dispatch & fleet operations platforms
- Ride-hailing and taxi apps
- Last-mile delivery & logistics
- Field service & workforce management
- Emergency response systems
- SaaS with embedded routing/ETA logic

If Google Maps appears as a line item on your cloud bill — **this will save you money**.

---

## 🔑 Dedicated API Keys & Transparent Metrics

Every customer gets:
- Unique API key (e.g. `CLIENT-PROD-XYZ`)
- Fully isolated traffic and rate limits
- Real-time usage dashboard:

| Metric              | Description                                                                 |
|---------------------|-----------------------------------------------------------------------------|
| `total_requests`    | All incoming requests received by the proxy                                 |
| `cache_hits`        | Requests served directly from cache (no new Google call needed)             |
| `cache_misses`      | Requests not found in cache (typically lead to a Google call or fallback)   |
| `google_calls`      | Outgoing requests sent to Google (billable events)                          |
| `fallback_uses`     | Times fallback logic was triggered (Google error, timeout, or suppression)  |
| `stale_uses`        | Responses served using stale-but-valid cached data (stale-while-revalidate) |

See your savings in real time.

---

## ⚙️ Seamless API Integration

Supports **both JSON and query parameter** styles — works with existing code.

### 1. Distance Matrix API

**Recommended: POST + JSON body**
 ✅ Option A — JSON Body (recommended)
```http
POST https://maps.goseanto.com/eta?key=YOUR_API_KEY
Content-Type: application/json

{
  "origins": ["45.5017,-73.5673"],
  "destinations": ["45.5081,-73.5550"],
  "traffic": true
}
```
✅ Option B — Query Parameters (Google-compatible)

```http
GET https://maps.goseanto.com/directions
      ?origin=45.5017,-73.5673
      &destination=45.5081,-73.5550
      &mode=driving
      &traffic=true
      &waypoints=45.5050,-73.5600
      &key=<API_KEY>
```
Response: Unmodified Google Distance Matrix JSON.

### 2. Directions API

✅ Option A — JSON Body (recommended)
```http
POST https://maps.goseanto.com/directions?key=<API_KEY>
Content-Type: application/json

{
  "origin": "45.5017,-73.5673",
  "destination": "45.5081,-73.5550",
  "mode": "driving",
  "traffic": true,
  "waypoints": ["45.5050,-73.5600"]
}
```
✅ Option B — Query Parameters (Google-style)

```http
GET https://maps.<stage>.goseanto.com/directions
  ?origin=45.5017,-73.5673
  &destination=45.5081,-73.5550
  &mode=driving
  &traffic=true
  &waypoints=45.5050,-73.5600
  &key=<API_KEY>

```
Response: Unmodified Google Directions JSON.

---
## 🔄 Zero operational risk - Failover Pattern (Recommended)
Use our proxy as primary.
If any error → fallback to Google.

Pseudocode:
```javascript
try {
  return callGoseanto();
} catch {
  return callGoogle();
}
```
---

### 🧩Architecture (High Level)

Your app → Goseanto Maps Proxy → Google Maps

Roughly:

* Your application sends standard Google-compatible requests
* Our secure gateway processes them using our proprietary optimization engine
* Google is called only when necessary
* Cached responses return in a few milliseconds (faster than calling Google directly)
* Live Google calls add only a minimal overhead (usually <10–20 ms)
* Responses are returned in Google-compatible JSON, so no code changes on your side

---

## 📩Access
This documentation is public; API access is not.

Production access requires:
* A provisioned endpoint in your chosen AWS region
* A dedicated API key
* Optional test environment available on request

For onboarding or pilot testing:
📧 info@goseanto.com
🌐 https://goseanto.com
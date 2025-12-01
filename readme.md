
# Goseanto Maps Proxy — AWS-Native Cost Optimizer for Google Maps

A lightweight, AWS-hosted routing layer that sits in front of Google Maps Distance Matrix and Directions APIs.

> Cut your Google Maps API bill by **30–60%** without changing your code.

- You keep your existing Google Maps integration.
- You just point it to our gateway instead of calling Google directly.
---
## 💰How It Saves Money

Instead of every unit calling Google:

- Unit A → Google → $0.005–$0.01 per element  
- Unit B → Google → $0.005–$0.01 per element  
- Unit C → Google → $0.005–$0.01 per element  
- …

Your units call the **Goseanto Maps Proxy**:

- First request → goes to Google  
- Next identical requests (for 10–20 seconds) → served from **AWS cache**  
- Cache window is **shorter** than Google’s own traffic refresh cycle

Result:

- **Same data** you would get from Google
- **Far fewer billable calls**
- Typical savings: **30–60%**, depending on traffic patterns
---

## 🛡️AWS-Backed, Trusted Architecture

The proxy runs entirely on **Amazon Web Services**:

- **API Gateway** — secure HTTPS entry point per customer
- **AWS Lambda (Go)** — stateless compute, auto-scales with traffic
- **DynamoDB** — caching + metrics, low-latency and fully managed
- **CloudWatch** — monitoring, dashboards, alarms
- **KMS** — key management and encryption

Benefits:

- No servers to manage
- Built on trusted, globally adopted cloud services
- Easy to audit and integrate into existing AWS governance

---
## ⚠️Reliability & Fallback

We still call Google for fresh data — but we protect you from failures:

- If Google times out or returns 5xx, we can return the **last known good ETA**
- If Google recovers, we refresh in the background
- Your application avoids freezes or blank screens during incidents and spikes


## 🌍 Regional Deployment & Compliance
We support deployments in:

🇨🇦 Canada (ca-central-1) — aligned with PIPEDA and provincial privacy rule
🇺🇸 US regions — HIPAA-ready architecture
🇪🇺 EU regions — GDPR-aligned data handling

Key points:
- Data stays in your chosen AWS region
- Cache is short-lived and used only for optimization
- We process minimal routing input (lat/lng); no PII or PHI is required
- No cross-region replication unless explicitly agreed

## 👥Who This Is For

- Any platform that calls Google Maps heavily, for example:
- Dispatch & operations platforms
- Mobility / ride-hailing and taxi apps
- Logistics & last-mile delivery
- Field-service and workforce apps
- Emergency and public-sector systems
- SaaS products embedding routing or ETA logic

If Google Maps is a visible line on your bill, this proxy is likely relevant.

## 🔑 Per-Client API Keys & Metrics

Each customer gets its own API key (e.g. CLIENT-STG-123):
- Used for authentication, throttling, and key rotation
- Fully isolated traffic per client
- Daily counters per key:

* total_requests
* cache_hits / cache_misses
* google_calls
* fallback_uses

This makes it easy to see exactly how much traffic and savings the proxy delivers.

## ⚙️ API Endpoints Integration

We support **both JSON body AND traditional query parameters**  

### 1. Distance Matrix

### ✅ Option A — JSON Body (recommended)
POST https://maps.goseanto.com/eta?key=<API_KEY>

Request
```json
{
"origins": ["45.5017,-73.5673"],
"destinations": ["45.5081,-73.5550"],
"traffic": true
}
```
## ✅ Option B — Query Parameters (Google-compatible)
```bash
 GET https://maps.<stage>.goseanto.com/eta
      ?origins=45.5017,-73.5673
      &destinations=45.5081,-73.5550
      &traffic=true
      &key=<API_KEY>
```

Response — Google Distance Matrix JSON (unchanged).

### 2. Directions 

Supports **JSON body OR query parameters**.
POST https://maps.goseanto.com/directions?key=<API_KEY>


Request
### ✅ Option A — JSON Body (recommended)
```json
{
  "origin": "45.5017,-73.5673",
  "destination": "45.5081,-73.5550",
  "mode": "driving",
  "traffic": true,
  "waypoints": ["45.5050,-73.5600"]
}
```
## ✅Option B — Query Parameters (Google-compatible)
```bash
GET https://maps.<stage>.goseanto.com/directions
      ?origin=45.5017,-73.5673
      &destination=45.5081,-73.5550
      &mode=driving
      &traffic=true
      &waypoints=45.5050,-73.5600
      &key=<API_KEY>
```
      
Response

Google Directions JSON (proxied directly).

## 🔄 Failover Pattern (Recommended)

Use our proxy as primary.
If any error → fallback to Google.

Pseudocode:
```js
{
  try {
    return callGoseanto();
} catch {
    return callGoogle();
}
}
```
Zero operational risk.

### 🧩Architecture (High Level)

Your app → Goseanto Maps Proxy → Google Maps

Roughly:

- Your App (SaaS / dispatch / mobility)
- HTTPS → API Gateway
- Lambda (Go) with caching + burst control
- DynamoDB for cache + metrics
- Outbound call to Google Distance Matrix / Directions
- Response relayed back in Google-compatible format

## 📩Access

This documentation is public.

Production API access requires:
An issued API key and region selection

Interested in testing or a sample environment?

Contact us:
📧 info@goseanto.com
🌐 https://goseanto.com
# Goseanto Maps Proxy — AWS-Native Cost Optimizer for Google Maps

A lightweight, AWS-hosted routing layer that sits in front of **Google Maps Distance Matrix** and **Directions APIs**.

> **Cut your Google Maps API bill by 30–60%** — without changing a single line of code.

- Keep your existing Google Maps integration  
- Just redirect calls to our secure gateway  
- Instant savings through intelligent caching

---

## 💰 How It Saves You Money

**Before (direct calls):**
- Unit A → Google → $0.005–$0.01 per request  
- Unit B → Google → $0.005–$0.01 per request  
- Unit C → Google → $0.005–$0.01 per request  
→ **Every identical request costs money**

**After (with Goseanto Proxy):**
- First request → forwarded to Google  
- Next identical requests (within 10–20 seconds) → served instantly from **AWS cache**  
- Cache duration shorter than Google’s traffic data refresh cycle

**Result:**
- Identical real-time data from Google
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

We never compromise accuracy — but we protect your app:

- If Google returns 5xx or times out → serve **last known good result**
- Background refresh when Google recovers
- Your users never see blank screens or delays during incidents

Zero risk. Full resilience.

---

## 🌍 Regional Deployment & Compliance

Deploy in your preferred AWS region — data stays where you need it:

| Region                | Location           | Compliance Support                     |
|-----------------------|--------------------|----------------------------------------|
| `ca-central-1`        | Canada             | PIPEDA + provincial privacy laws       |
| US regions            | United States      | HIPAA-eligible architecture            |
| EU regions            | European Union     | GDPR-compliant data handling           |
| `sa-east-1`           | Brazil             | LGPD (Lei Geral de Proteção de Dados)  |

Custom regions available on request  
No cross-region data replication unless explicitly approved  
Minimal data processed: only coordinates (no PII/PHI required)

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

| Metric              | Description                       |
|---------------------|-----------------------------------|
| `total_requests`    | All incoming requests             |
| `cache_hits`        | Served from cache (free)          |
| `cache_misses`      | Forwarded to Google               |
| `google_calls`      | Actual billable requests          |
| `fallback_uses`     | Resilience events triggered       |

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
Response: Unmodified Google Distance Matrix JSON

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
Response: Google Directions JSON, proxied through the gateway.

---
## 🔄 Zero operational risk - Failover Pattern (Recommended)
Use our proxy as primary.
If any error → fallback to Google.

Pseudocode:
```javascript
{ try 
    { return callGoseanto(); } 
    catch 
    { return callGoogle(); } 
    }
```
---

### 🧩Architecture (High Level)
Your app → Goseanto Maps Proxy → Google Maps
Roughly:

Your App (SaaS / dispatch / mobility)
HTTPS → API Gateway
Lambda (Go) with caching + burst control
DynamoDB for cache + metrics
Outbound call to Google Distance Matrix / Directions
Response relayed back in Google-compatible format

---

## 📩Access
This documentation is public.
Production API access requires:
An issued API key and region selection
Interested in testing or a sample environment?
Contact us:
📧 info@goseanto.com
🌐 https://goseanto.com
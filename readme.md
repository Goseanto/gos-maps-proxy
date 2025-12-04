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
- Next identical requests (within your configured cache window, typically 1–20 seconds) → served instantly from **AWS cache** 
- Even with cache disabled (TTL=0), we still collapse micro-bursts so 100 identical in-flight requests become a single Google call

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
GET https://maps.goseanto.com/directions
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
### 🧠 Summary of Our Caching & Freshness Behavior

### Accurate, Real-Time Results — With Smart Cost Reduction

We use a **two-layer caching strategy** specifically tuned for Google’s live-traffic behavior:

| Layer                  | Duration          | Purpose                                                                 | Effect on Freshness                              |
|------------------------|-------------------|--------------------------------------------------------------------------|--------------------------------------------------|
| **Burst deduplication**| ~400 ms (fixed)   | Collapses 100 identical concurrent requests into 1 Google call           | Zero added latency, 100% fresh                    |
| **Main cache**         | 1–20 seconds (configurable per customer, capped at ≤20s when traffic=true) | Eliminates repeat calls during short-term spikes                        | Still effectively real-time                      |
> **Note:** Setting `CACHE_TTL_SECONDS=0` disables the main cache but keeps burst deduplication and last-known-good fallback enabled. This is “burst-only” mode for teams that want maximum freshness with protection against micro-bursts.
**Why this is safe and accurate:**

- Google’s live traffic data does not change materially in under 20–30 seconds in the real world.
- We always call Google with `departure_time=now` when `traffic=true`.
- Our maximum reuse window for traffic-aware responses is ≤20 seconds (often tighter).
- Result: cached responses are **indistinguishable from a brand-new Google call** from the user’s perspective, while dramatically reducing billable elements.

Non-traffic requests (e.g. static routes, walking, bicycling) can use longer configurable TTLs when appropriate.

### 2. Updated Freshness Behavior Table 

| Scenario                                   | Cache Hit? | Google Call?                     | Freshness Level       | Explanation                                                                 |
|--------------------------------------------|------------|----------------------------------|-----------------------|-----------------------------------------------------------------------------|
| Identical request within ~400 ms           | Yes (burst) | Only 1 call (shared)             | 100% fresh            | All concurrent callers get the same live result instantly                  |
| Identical request within main cache TTL    | Yes        | No                               | Effectively real-time | Max 20 s old when traffic=true → traffic hasn’t meaningfully changed       |
| Different origins / destinations           | No         | Yes                              | 100% fresh            | New geometry → must recompute                                               |
| Traffic flag flips (true ↔ false)          | No         | Yes                              | 100% fresh            | Different routing model                                                     |
| Waypoints added / removed / reordered      | No         | Yes                              | 100% fresh            | Route topology changed                                                     |
| Google returns 5xx, rate limit, or timeout| Yes (stale)| No                               | Last known good       | Your app stays up; we serve most recent valid response                     |

                                        |

### 📝 Plain-Language Notes

- **TTL (Time-To-Live):** Short reuse window (1–20s). If the same request comes in again during this window, we can reuse the previous result instead of calling Google again.
- **Burst Collapse**: When many identical requests arrive at the same moment, we combine them into one Google call and share the result with everyone — reducing cost without affecting accuracy.  
- **Key Changes:** Route changed (origin/destination/waypoints/traffic/time) → new Google call.  
- **LKG:** If Google fails, we return the last good result so your app keeps working.

---

## 📩 Access
This documentation is public; API access is not.

Production access requires:
* A provisioned endpoint in your chosen AWS region
* A dedicated API key

Optional test environment available on request

For onboarding or pilot testing:
📧 info@goseanto.com  
🌐 https://goseanto.com
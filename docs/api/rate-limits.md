# Rate Limits

The CertDirectory API enforces several layers of rate limiting to protect the platform and ensure fair use.

## Limit tiers

| Layer | Default | Scope |
|---|---|---|
| **Global IP limit** | 100 requests / minute | Per source IP across all routes. |
| **API key limit** | 60 requests / minute | Per API key (configurable per-key by us). |
| **Auth endpoint limit** | 3 requests / 15 minutes | Per IP for sensitive auth endpoints (e.g. `/api/auth/resend-verification-public`). |

Limits are evaluated independently — exceeding **any one** of them returns `429 RATE_LIMITED`.

## Response headers

Every API-key request includes:

| Header | Meaning |
|---|---|
| `X-RateLimit-Limit` | Your per-minute quota for this API key. |
| `X-RateLimit-Remaining` | Requests left in the current window. |
| `X-RateLimit-Reset` | Unix timestamp (seconds) when the current window resets. |

When throttled (`429`), use `X-RateLimit-Reset` (or `Retry-After` if present) to wait before retrying.

## Recommended retry strategy

Use **exponential backoff with jitter** when you hit a 429 or any 5xx error. A simple, well-behaved client looks like:

=== "Pseudocode"

    ```
    delay = 1
    for attempt in 1..6:
      response = call(...)
      if response.ok: return response
      if response.status in (429, 500, 502, 503, 504):
        wait_for = max(delay, response.retry_after_seconds or 0)
        wait_for *= (1 + random.uniform(0, 0.5))   # jitter
        sleep(wait_for)
        delay *= 2
        continue
      raise
    raise Exception("exhausted retries")
    ```

=== "Node.js"

    ```javascript
    async function callWithBackoff(fn) {
      let delay = 1000;
      for (let i = 0; i < 6; i++) {
        const res = await fn();
        if (res.ok) return res;
        if ([429, 500, 502, 503, 504].includes(res.status)) {
          const reset = Number(res.headers.get('X-RateLimit-Reset')) * 1000;
          const wait = Math.max(delay, reset - Date.now()) * (1 + Math.random() * 0.5);
          await new Promise((r) => setTimeout(r, wait));
          delay *= 2;
          continue;
        }
        throw new Error(`API error: ${res.status}`);
      }
      throw new Error('Exhausted retries');
    }
    ```

=== "Python"

    ```python
    import time, random, requests

    def call_with_backoff(fn):
        delay = 1
        for _ in range(6):
            r = fn()
            if r.ok:
                return r
            if r.status_code in (429, 500, 502, 503, 504):
                reset = int(r.headers.get("X-RateLimit-Reset", 0))
                wait = max(delay, reset - time.time())
                time.sleep(wait * (1 + random.random() * 0.5))
                delay *= 2
                continue
            r.raise_for_status()
        raise RuntimeError("Exhausted retries")
    ```

## Bulk operations

For high-volume issuance:

- **Use the [Bulk CSV upload](../organizations/bulk-issuance.md) UI** when you have a one-time batch — it doesn't consume API rate limit at all.
- For continuous high-volume issuance over the API, **stay below 60 requests/minute**, batch by spacing out issuance, and contact us if you need a higher per-key limit.

## Quota vs rate limit

Two different things:

- **Rate limit** — per-minute throughput. Exceeding it returns `429`. The limit resets every minute.
- **Monthly recipient quota** — total *unique recipients* per calendar month, set by your subscription tier. Exceeding it returns `403`. Resets on the 1st of each month.

See [Billing & Plans](../organizations/billing.md) for monthly quotas.

## Need a higher limit?

If your integration has a legitimate need for higher per-key throughput (e.g. real-time issuance for a high-traffic event):

1. Contact us with your use case.
2. We can raise the limit on a specific key.
3. We may suggest moving heavy traffic to bulk issuance instead.

## Next

- [Authentication](authentication.md)
- [Credentials API](credentials.md)

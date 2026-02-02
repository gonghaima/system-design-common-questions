# System Design: Handling Cache Request Surge

## How to Approach the Interview

1. **Clarify Requirements**
   - Ask about the context: What kind of cache (e.g., Redis, Memcached)? What is the expected QPS? What triggers the surge (e.g., cache miss, flash sale)?
   - Confirm functional and non-functional requirements (latency, consistency, availability, data freshness).

2. **High-Level Design**
   - Describe the main components: clients, cache layer, backend database, load balancers, monitoring.
   - Draw a simple block diagram if possible.

3. **Identify the Problem**
   - Explain what happens during a cache surge (e.g., thundering herd problem, cache stampede).
   - Discuss why it is a problem (backend overload, increased latency, possible downtime).

4. **Mitigation Strategies**
   - **Request Coalescing:** Ensure only one request populates the cache for a missing key, others wait for the result. For example, when multiple requests arrive for the same missing key, the first request triggers a backend fetch and populates the cache, while subsequent requests wait for the result instead of all hitting the backend. This can be implemented using a per-key mutex or a promise/future pattern in the application layer.
   - **Locking (Distributed Locks):** Use distributed locks (e.g., Redis SETNX, ZooKeeper, etcd) to ensure that only one process or server is allowed to fetch and repopulate a missing cache entry at a time. Other processes attempting to acquire the lock for the same key will wait or back off, preventing a thundering herd on the backend. Care must be taken to handle lock timeouts and failures gracefully.
   - **Stale Data Serving:** Serve slightly stale data while refreshing cache in the background.
   - **Randomized Expiry:** Add jitter (randomness) to cache expiry times to avoid many keys expiring simultaneously, which can cause a surge of backend requests (cache stampede). For example, instead of setting all keys to expire in exactly 10 minutes, set expiry to 10 minutes ± a random offset. This spreads out cache invalidations and smooths backend load.
   - **Rate Limiting:** Throttle requests to backend during surges.
   - **Pre-warming:** Proactively load popular keys into cache before expected surges.
   - **Multi-level Caching:** Use a hierarchy of caches, such as a local in-memory cache (e.g., Guava, Caffeine, or process memory) in addition to a distributed cache (e.g., Redis, Memcached). Requests first check the local cache, then the distributed cache, and finally the backend. This reduces latency and distributed cache load, especially for hot keys, and can be very effective in microservices or horizontally scaled environments.

5. **Deep Dive**
   - Pick one or two strategies to explain in detail (e.g., how to implement request coalescing or stale-while-revalidate).
   - Discuss trade-offs (freshness vs. availability, complexity vs. performance).

6. **Monitoring and Alerting**
   - Set up monitoring for cache hit/miss rates, backend load, latency.
   - Alert on abnormal patterns.

7. **Wrap Up**
   - Summarize the design, mention possible improvements, and ask if the interviewer wants to dive deeper into any area.

## Example Interaction with Interviewer

- "What is the expected scale and what triggers the request surge?"
- "Is it acceptable to serve slightly stale data during a surge?"
- "Should we optimize for consistency or availability?"
- "Would you like me to focus on the cache layer or backend protection strategies?"

## Tips

- Communicate your thought process clearly.
- Make reasonable assumptions and state them.
- Be ready to adjust your design based on feedback.
- Use diagrams if possible.

---

This structure will help you answer and interact effectively during a system design interview for handling cache request surges.

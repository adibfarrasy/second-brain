#systemdesign 

resource:
[10 Tips for Building Resilient Payment Systems](https://shopify.engineering/building-resilient-payment-systems)

# 1. Lower Your Timeouts

Set default timeout for HTTP requests.

For database: use `MAX_EXECUTION_TIME` if using MySQL, or use `pt-kill`.

<aside> 💡 General recommendation: set _1s_ timeout for writes, and _5s_ timeout for reads.

</aside>

# 2. Install Circuit Breakers

Motivation: Services that go down tend to stay down for a while, so if we see multiple timeouts in a short period of time, we can improve this by _not trying at all_. We can use Shopify’s [toxiproxy](https://github.com/Shopify/toxiproxy) do simulate chaotic TCP connections.

<aside> 💡 General recommendation: understand how the application can fail and what falling back could look like, then test and implement them.

</aside>

# 3. Understand Capacity

Little’s Law:

> “_the average number of customers in a system (over an interval) is equal to their average arrival rate, multiplied by their average time in the system._”

_Arrival rate_ is the amount of customers entering and leaving the system.

throughput = capacity / latency

<aside> 💡 General recommendation: use rate-limiter and/or load-shedder to control capacity to the level of the application’s throughput.

</aside>

# 4. Add Monitoring and Alerting

Monitor Google’s SRE’s four golden signals:

- Latency: the time it takes to process a unit of work.
- Traffic: the rate in which new work comes into the system.
- Error rate: the rate of unexpected things happening.
- Saturation: how much load the system is under, relative to its total capacity.

# 5. Implement Structured Logging

<aside> 💡 General recommendation: generate and pass around a `correlation_id` to background jobs/ related services related to the API call. Group the logs by `correlation_id` into a single stacktrace-like log.

</aside>

# 6. Use Idempotency Keys

The idempotency key looks up the steps the attempt completed e.g. creating a local database record of the transaction and makes sure we only send a single request to our financial partners. If any of these steps fail and a retired request with the same idempotency key is received, recovery steps are run to recreate the same state before continuing.

<aside> 💡 General recommendation: generate unique idempotency key for a transaction, and persist the steps that has been taken for the key.

</aside>

# 7. Be Consistent With Reconciliation

Shopify’s payment system: With reconciliation we make sure that our records are consistent with those of our financial partners. In case of a mismatch, we record the anomaly in our database.

<aside> 💡 General recommendation: persist the anomaly records. Attempt to automatically fix the anomaly where possible.

</aside>

# 8. Incorporate Load Testing

Regularly test the limits and protection mechanisms of our systems by simulating large volume operations on specifically set up application.

<aside> 💡 General recommendation: use scriptable load balancers to throttle the amount of requests happening at any given time. In the case of requests exceeding capacity, place the users on an application-level waiting queue.

Test the high volume scenarios often.

</aside>

# 9. Get on Top of Incident Management

Once the problem is confirmed, we start the incident process with a command sent to our Slack channel.

The conversation moves to the assigned incident channel where we have three roles involved:

- Incident Manager on Call (IMOC): responsible for coordinating the incident
- Support Response Manager (SRM): responsible for public communication
- the service owner(s): who are working on restoring stability.

# 10. Organize Incident Retrospectives

We aim to have an incident retrospective meeting within a week after the incident occurred. During this meeting:

- we dig deep into what exactly happened
- what incorrect assumptions we held about our systems
- what we can do to prevent the same thing from happening again.
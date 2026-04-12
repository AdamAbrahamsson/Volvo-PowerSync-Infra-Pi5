# Volvo PowerSync Infra (Pi5)

Infrastructure and deployment for the application in **[Volvo-PowerSync-Core](https://github.com/adamabrahamsson/Volvo-PowerSync-Core)** (update the link if your GitHub path differs).

## Public URL

- **Demo:** [https://powersync.adamcloud.dev/](https://powersync.adamcloud.dev/)

The dashboard is reachable when the Pi cluster, services, and edge tunnel (e.g. Cloudflare) are running.

## Availability

Workloads run on a **Raspberry Pi 5** at the maintainer’s site. The device is **not** guaranteed to be powered on 24/7. If **powersync.adamcloud.dev** does not load, contact the **owner** to turn the Pi on and bring the stack up.

## Related repositories

| Repo | Role |
|------|------|
| [Volvo-PowerSync-Core](https://github.com/adamabrahamsson/Volvo-PowerSync-Core) | Java services, frontend, `docker compose` for local dev |
| **Volvo-PowerSync-Infra-Pi5** (this repo) | Kubernetes manifests, tunnel/DNS, **Prometheus** scraping (**ServiceMonitor**), **Grafana** dashboards—**monitoring is documented here**, not in the core repo |

## Monitoring (Prometheus & Grafana)

The cluster is expected to run **kube-prometheus-stack** (or equivalent): **Prometheus Operator** + **Prometheus** + **Grafana**.

- **`booking-service`** exposes Spring Boot Actuator metrics at **`/actuator/prometheus`** on its HTTP port.
- A **`ServiceMonitor`** (`apps/powersync/base/booking-service-servicemonitor.yaml`) selects the booking Service and sets `release: kube-prometheus-stack` so Prometheus scrapes every **15s**.
- Use **Grafana** in that stack to graph booking metric (`powersync_station_bookings_total`).

If you scale booking to multiple replicas, aggregate with `sum by (...) (...)` so panels stay correct.

---

More deployment detail (namespaces, image tags, rollout steps) can be added here as this repo grows.

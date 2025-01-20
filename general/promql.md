sample query:
```promql
http_request_total
```

using labels:
```promql
http_request_total{path="/", method="GET"}
```

using label with regex (all paths starting with "/items"):
```promql
http_request_total{path=~"/items.*"}
# opposite (not include):
http_request_total{path!~"/items.*"}
```

show requests in the past 5 minutes (default is the resent one):
```promql
http_request_total{path=~"/items.*"}[5m]
```

**Range Vector** computes a range of values per time series.
**Instant Vector** computes a single value per time series

rate function to show average values over time (checks for frequency), while irate gets only last 2 values (checks for volatility):
```promql
rate(http_request_total{path!~"/favicon.ico"}[1m])
```

get the change in CPU between first and last value in the range of last 5 minutes:
```promql
delta(process_cpu_usage[5m])
```

**Aggregation operators** are: sum, avg, min, max

group by a label before agregation:
```promql
sum by (method) (rate(http_request_total[5m]))
```

get how long 95% of the requests on path "/" took:
```promql
histogram_quantile (0.95, sum by (le) (http_request_duration_seconds_bucket{path="/"}))
```

get average time that it takes for each request to process on the path "/":
```promql
rate(http_request_duration_seconds_sum{path="/"}[5m]) / rate(http_request_duration_seconds_count{path="/"}[5m])
```

get memory usage in Mb:
```promql
process_memory_usage_bytes / (1024 * 1024)
```

show growing memory in last hour in bytes:
```promql
deriv(process_resident_memory_bytes[1h])
```

service-monitor example for kubernetes:
```yaml
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: fastapi-app
  namespace: monitoring
  labels:
    release: prometheus # matches kube-prometheus-stack release label
spec:
  selector:
    matchLabels:
      app: fastapi-app # matches application label for service monitor
  endpoints:
    - port: web
    path: /metrics
    interval: 15s
```
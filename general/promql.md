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

**Range Vector** computes a range of values per time series
**Instant Vector** computes a single value per time series

rate function to show average values over time (checks for frequency), while irate gets only last 2 values (checks for volatility):
```promql
rate(http_request_total{path!~"/favicon.ico"}[1m])
```

get the change in CPU between first and last value in the range of last 5 minutes:
```promql
delta(process_cpu_usage[5m])
```

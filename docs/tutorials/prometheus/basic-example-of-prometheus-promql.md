# Basic example of Prometheus Query Language

PromQL (Prometheus Query Language) is a powerful and flexible query language used to select and aggregate time series data in Prometheus. Here are the main concepts with an example to illustrate how PromQL works.

### Basic Concepts

1. **Metrics**: These are the time series data points collected by Prometheus.
2. **Labels**: These are key-value pairs that identify dimensions of a metric.
3. **Functions**: These are built-in operations that can be applied to metrics.
4. **Operators**: These are used to combine and manipulate time series.

### Example Scenario

Assume we have a metric `http_requests_total` that tracks the total number of HTTP requests received by our server. This metric has labels such as `method` (e.g., GET, POST), `status_code` (e.g., 200, 404), and `instance` (the server instance).

### Example Queries

#### 1. Basic Metric Query

To get the current value of the `http_requests_total` metric:

```promql
http_requests_total
```

#### 2. Filtered Query

To get the total number of HTTP requests for the GET method:

```promql
http_requests_total{method="GET"}
```

#### 3. Aggregation

To get the total number of HTTP requests across all methods and instances:

```promql
sum(http_requests_total)
```

#### 4. Rate Calculation

To get the per-second rate of HTTP requests over the last 5 minutes:

```promql
rate(http_requests_total[5m])
```

#### 5. Aggregation with Rate

To get the per-second rate of HTTP requests for each method over the last 5 minutes:

```promql
sum by (method) (rate(http_requests_total[5m]))
```

### Detailed Example

Let's say you want to monitor the error rate (status codes 4xx and 5xx) of HTTP requests over the last 5 minutes for each server instance. Here's how you can do it:

1. **Filter for Error Status Codes**:

```promql
http_requests_total{status_code=~"4..|5.."}
```

The `=~` operator is used for regular expression matching.

2. **Calculate the Rate**:

```promql
rate(http_requests_total{status_code=~"4..|5.."}[5m])
```

This calculates the per-second rate of error requests over the last 5 minutes.

3. **Aggregate by Instance**:

```promql
sum by (instance) (rate(http_requests_total{status_code=~"4..|5.."}[5m]))
```

This sums the error rates for each instance.

### Putting It All Together

Here’s a complete PromQL query that calculates the per-second error rate of HTTP requests over the last 5 minutes, grouped by server instance:

```promql
sum by (instance) (rate(http_requests_total{status_code=~"4..|5.."}[5m]))
```

### Explanation

- `http_requests_total{status_code=~"4..|5.."}`: Selects the time series where the status code matches the pattern for 4xx and 5xx errors.
- `rate(http_requests_total{status_code=~"4..|5.."}[5m])`: Calculates the per-second rate over the last 5 minutes.
- `sum by (instance)`: Sums the rates, grouped by the `instance` label.

By mastering these concepts and examples, you'll be able to construct PromQL queries to monitor and analyze your Prometheus metrics effectively.
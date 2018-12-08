# Monitoring Functions

The Gateway component exposes several metrics to help you monitor the health and behavior of your functions

| Metric                              | Type      | Description                    | Labels                  |
| ----------------------------------- | --------- | ------------------------------ | ----------------------- |
| `gateway_functions_seconds`         | histogram | Function invocation time taken | `function_name`         |
| `gateway_function_invocation_total` | counter   | Function invocation count      | `function_name`, `code` |
| `gateway_service_count`             | counter   | Number of function replicas    | `function_name`         |

## Examples

These basic metrics can be used to track the health of your functions as well a general usage patterns. See the Prometheus [documentation][prom-query-basics] and [examples][prom-query-examples] for more details about the available options and query functions. Below are several queries you might want to include in a basic [Grafana](https://grafana.com) dashboard for observing your OpenFaaS functions

### Function invocation rate

Return the per-second rate of invocation as measured over the previous 20 seconds:

```
rate ( gateway_function_invocation_total [20s])
```

### Function replica count / scaling

Return the total function replicas:

```
gateway_service_count
```

### Total OK Function Invocation

Return the total number of successful function invocations:

```
sum( gateway_function_invocation_total {  code=\"200\"}
```

### Function execution time

Return the average function execution time, as measure over the previous 20 seconds:

```
(rate(gateway_functions_seconds_sum[20s]) / rate(gateway_functions_seconds_count[20s]))
```

[prom-query-basics]: https://prometheus.io/docs/prometheus/latest/querying/basics/
[prom-query-examples]: https://prometheus.io/docs/prometheus/latest/querying/examples/
# operator-monitoring-stack

The Drosera Operator Monitoring Stack is a collection of tools and services that are used to monitor the health and performance of the Drosera Operator.

It is composed of the following components:
- **Otel Collector** for **telemetry** collection
- **Prometheus** for **metrics** collection
- **Loki** for **log** collection
- **Tempo** for **trace** collection
- **Grafana** for visualization

The Drosera Operator Node exports all of it's logs, traces and metrics in the OTLP format to the Otel Collector at `http://localhost:4317`. The Otel Collector then forwards the logs, traces and metrics to their respective services.

It is also contains a preconfigured dashboard, `Drosera Operator` to visualize the metrics exported by the Drosera Operator node. 

## Installation
```bash
docker compose up -d
```

You can access the Grafana dashboard at `http://localhost:3000`

## Authentication
The Otel Collector utlizes basic auth to authenticate the Drosera Operator Node. The default username and password is `admin` and `drosera`. You can change the username and password by modifying the `BASIC_AUTH` environment variable in the `otel-collector` service in the `docker-compose.yml` file.

To configure the Drosera Operator Node to use the new username and password, you can set the `--otel-export-metadata` setting with the basic auth header and value. 

For example:
```bash
# Authentication for default username and password
--otel-export-metadata "authorization=Basic YWRtaW46ZHJvc2VyYQ=="
```

The authorization value is `Basic` followed by a space and the base64 encoded value of `username:password`. 

## Usage

### Metrics
Metrics are displayed in the `Drosera Operator` dashboard under "Dashboards" in Grafana.

### Logs
Logs are displayed in the `Explore` section under the `Loki` datasource in Grafana and are filtered by the `service_name` label with the value `drosera_operator`.

Logs can be further filtered by trap address or block number by clicking on one of the log messages and then clicking on the `trap_address` or `block_number` label.

### Traces
Traces are displayed in the `Explore` section under the `Tempo` datasource in Grafana and are filtered by the `service_name` label with the value `drosera_operator`.

The `enzyme` span contains the trace for traps querying state data from the blockchain. 

## Debugging Monitoring Stack
To debug the monitoring stack, the logs from each service can be viewed in the terminal by default. To turn off the logs,
you can remove the `debug` exporter from the metrics, traces and logs pipelines in the `otel-collector` configuration file.

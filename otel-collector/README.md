```
export OTEL_VERSION=0.125.0
curl --proto '=https' --tlsv1.2 -fOL https://github.com/open-telemetry/opentelemetry-collector-releases/releases/download/v${OTEL_VERSION}/otelcol-contrib_${OTEL_VERSION}_linux_amd64.tar.gz
tar -xvf otelcol-contrib_${OTEL_VERSION}_linux_amd64.tar.gz otelcol-contrib

cf push otel-col -b binary_buildpack --random-route -c "./otelcol-contrib --config=./config.yaml"
cf map-route otel-col apps.internal --hostname otel-col
cf add-network-policy prometheus otel-col --protocol tcp --port 18889
```

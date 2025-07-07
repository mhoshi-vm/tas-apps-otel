> ⚠️  Prometheus deployed by the following steps is ephemeral. I recommend to use Prometheus on k8s or Prometheus BOSH Release instead for production.


```
PROMETHEUS_VERSION=3.3.0
if [ ! -d prometheus-${PROMETHEUS_VERSION}.linux-amd64 ];then
    wget https://github.com/prometheus/prometheus/releases/download/v${PROMETHEUS_VERSION}/prometheus-${PROMETHEUS_VERSION}.linux-amd64.tar.gz
    tar xzf prometheus-${PROMETHEUS_VERSION}.linux-amd64.tar.gz
    rm -f prometheus-${PROMETHEUS_VERSION}.linux-amd64.tar.gz
fi

cf create-space -o system monitoring-tools
cf target -o system -s monitoring-tools
cf push prometheus -k 2G -b binary_buildpack --random-route -c "./prometheus-${PROMETHEUS_VERSION}.linux-amd64/prometheus --web.listen-address=:8080 --config.file=./prometheus.yml"

```

```
om -e env.yaml credentials -p p-healthwatch2-pas-exporter -c .properties.healthwatch_exporter_client_mtls -f cert_pem > cert.pem
om -e env.yaml credentials -p p-healthwatch2-pas-exporter -c .properties.healthwatch_exporter_client_mtls -f private_key_pem > private_key.pem
```

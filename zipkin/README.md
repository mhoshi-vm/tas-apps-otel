```
cf push zipkin -o ghcr.io/openzipkin-contrib/zipkin-otel:main --no-start
cf set-env zipkin UI_ENABLED true
APP_GUID=$(cf app zipkin --guid)
ROUTE_GUID=$(cf curl /v3/apps/$APP_GUID/routes | jq -r '.resources[0].guid')
cf curl -XPATCH /v3/routes/$ROUTE_GUID/destinations -d "{\"destinations\":[{\"app\":{\"guid\":\"$APP_GUID\"},\"port\":9411}]}"
cf start zipkin
cf map-route zipkin apps.internal --hostname zipkin
cf add-network-policy otel-col zipkin --protocol tcp --port 9411
```

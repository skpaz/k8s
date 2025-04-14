# Monitoring - WIP

## OCP Observability

OpenShift has a [built-in observability stack](https://docs.redhat.com/en/documentation/openshift_container_platform/4.10/html/monitoring/monitoring-overview).
 You can use the example ConfigMaps below to configure specific aspects of
 the operator's workloads.

```yaml
# cluster-monitoring-config.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: cluster-monitoring-config
  namespace: openshift-monitoring
data:
  config.yaml: |
    prometheusK8s:
      volumeClaimTemplate:
        spec:
          # default: fast
          storageClassName: FIXME_STORAGE_CLASS_NAME  
          volumeMode: Filesystem
          resources:
            requests:
              # default: 40Gi
              storage: FIXME_STORAGE_GI
```

```yaml
# user-workload-monitoring-config.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: user-workload-monitoring-config
  namespace: openshift-user-workload-monitoring
data:
  config.yaml: |
    prometheus:
      # default 24h
      retention: FIXME_RETENTION_HOURS  
      resources:
        requests:
          # default 200m
          cpu: FIXME_CPU_MICROSECONDS
          # default 2Gi
          memory: FIXME_MEMORY_GI
```

## Grafana Alloy

See [skpaz/grafana/alloy/openshift](https://github.com/skpaz/grafana/tree/main/alloy/openshift)
 to add Grafana Alloy to an OpenShift stack.

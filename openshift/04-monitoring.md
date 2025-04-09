# Monitoring - WIP

```bash
oc apply -f FIXME_PATH_TO_CONFIG
```

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

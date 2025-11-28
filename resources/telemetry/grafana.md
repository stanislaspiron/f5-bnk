```bash
cat <<EOF | kubectl apply -f -
apiVersion: apps/v1
kind: Deployment
metadata:
  name: grafana
  namespace: ${telemetry_namespace}
spec:
  replicas: 1
  selector:
    matchLabels:
      app: grafana
  template:
    metadata:
      name: grafana
      labels:
        app: grafana
    spec:
      securityContext:
        fsGroup: 472
      initContainers:
      - name: fix-permissions
        image: busybox
        command: ["sh", "-c", "chown -R 472:472 /var/lib/grafana"]
        securityContext:
          runAsUser: 0
        volumeMounts:
        - name: grafana-storage
          mountPath: /var/lib/grafana
      containers:
      - name: grafana
        image: grafana/grafana
        ports:
        - name: grafana
          containerPort: 3000
        securityContext:
          runAsUser: 472
        env:
          - name: GF_SECURITY_ADMIN_USER
            value: admin
          - name: GF_SECURITY_ADMIN_PASSWORD
            value: admin
          - name: GF_SECURITY_DISABLE_INITIAL_ADMIN_PASSWORD_CHANGE
            value: "true"
        resources:
          limits:
            memory: "1Gi"
            cpu: "1000m"
          requests:
            memory: 500M
            cpu: "500m"
        volumeMounts:
          - mountPath: /var/lib/grafana
            name: grafana-storage
          - mountPath: /etc/grafana/provisioning/datasources
            name: grafana-datasources
            readOnly: false
# support for dashboard
          - mountPath: /var/lib/grafana/dashboards
            name: grafana-dashboards
            readOnly: true
          - mountPath: /etc/grafana/provisioning/dashboards
            name: dashboard-provider
            readOnly: true
          - name: admin-user-config
            mountPath: /etc/grafana/provisioning/users/
            readOnly: true
      volumes:
        - name: grafana-datasources
          configMap:
            defaultMode: 420
            name: grafana-datasources
        - name: grafana-dashboards
          configMap:
            name: custom-grafana-dashboard
        - name: dashboard-provider
          configMap:
            name: grafana-dashboard-provider
        - name: admin-user-config
          configMap:
            name: grafana-admin-provisioning
        - name: grafana-storage
          persistentVolumeClaim:
            claimName: grafana-pvc
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: grafana-pvc
  namespace: ${telemetry_namespace}
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: grafana-admin-provisioning
  namespace: ${telemetry_namespace}
data:
  admin-user.yaml: |
    users:
      - name: admin
        email: admin@example.com
        login: admin
        password: admin2
        isAdmin: true
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: grafana-datasources
  namespace: ${telemetry_namespace}
data:
  prometheus.yaml: |-
    {
        "apiVersion": 1,
        "datasources": [
            {
               "access":"proxy",
                "editable": true,
                "name": "prometheus",
                "orgId": 1,
                "type": "prometheus",
                "url": "http://prometheus-server.${telemetry_namespace}.svc:80",
                "version": 1
            }
        ]
    }
---
apiVersion: v1
kind: Service
metadata:
  name: grafana
  annotations:
      prometheus.io/scrape: 'true'
      prometheus.io/port:   '3000'
  namespace: ${telemetry_namespace}
spec:
  selector:
    app: grafana
  type: NodePort
  ports:
    - port: 3000
      targetPort: 3000
      nodePort: 32000
---
# new configmap
apiVersion: v1
kind: ConfigMap
metadata:
  name: grafana-dashboard-provider
  namespace: ${telemetry_namespace}
data:
  dashboards.yaml: |
    apiVersion: 1
    providers:
      - name: 'default'
        orgId: 1
        folder: ''
        type: file
        disableDeletion: false
        editable: true
        options:
          path: /var/lib/grafana/dashboards
---
EOF
```
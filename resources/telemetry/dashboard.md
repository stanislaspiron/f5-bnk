```bash
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: ConfigMap
metadata:
  name: custom-grafana-dashboard
  namespace: ${telemetry_namespace}
  labels:
    grafana_dashboard: "1"
data:
  custom-dashboard.json: |
    {
      "annotations": {
        "list": [
          {
            "builtIn": 1,
            "datasource": {
              "type": "datasource",
              "uid": "grafana"
            },
            "enable": true,
            "hide": true,
            "iconColor": "rgba(0, 211, 255, 1)",
            "name": "Annotations & Alerts",
            "target": {
              "limit": 100,
              "matchAny": false,
              "tags": [],
              "type": "dashboard"
            },
            "type": "dashboard"
          }
        ]
      },
      "editable": true,
      "fiscalYearStartMonth": 0,
      "graphTooltip": 0,
      "id": 1,
      "links": [],
      "panels": [
        {
          "collapsed": true,
          "gridPos": {
            "h": 1,
            "w": 24,
            "x": 0,
            "y": 1
          },
          "id": 37,
          "panels": [
            {
              "datasource": {
                "type": "prometheus",
                "uid": "P1809F7CD0C75ACF3"
              },
              "fieldConfig": {
                "defaults": {
                  "color": {
                    "mode": "palette-classic"
                  },
                  "custom": {
                    "axisBorderShow": false,
                    "axisCenteredZero": false,
                    "axisColorMode": "text",
                    "axisLabel": "",
                    "axisPlacement": "auto",
                    "barAlignment": 0,
                    "barWidthFactor": 0.6,
                    "drawStyle": "line",
                    "fillOpacity": 0,
                    "gradientMode": "none",
                    "hideFrom": {
                      "legend": false,
                      "tooltip": false,
                      "viz": false
                    },
                    "insertNulls": false,
                    "lineInterpolation": "linear",
                    "lineWidth": 1,
                    "pointSize": 5,
                    "scaleDistribution": {
                      "type": "linear"
                    },
                    "showPoints": "auto",
                    "spanNulls": false,
                    "stacking": {
                      "group": "A",
                      "mode": "none"
                    },
                    "thresholdsStyle": {
                      "mode": "off"
                    }
                  },
                  "mappings": [],
                  "thresholds": {
                    "mode": "absolute",
                    "steps": [
                      {
                        "color": "green",
                        "value": null
                      },
                      {
                        "color": "red",
                        "value": 80
                      }
                    ]
                  }
                },
                "overrides": []
              },
              "gridPos": {
                "h": 10,
                "w": 8,
                "x": 0,
                "y": 2
              },
              "id": 4,
              "options": {
                "legend": {
                  "calcs": [],
                  "displayMode": "table",
                  "placement": "bottom",
                  "showLegend": true
                },
                "tooltip": {
                  "hideZeros": false,
                  "mode": "single",
                  "sort": "none"
                }
              },
              "pluginVersion": "11.5.2",
              "targets": [
                {
                  "datasource": {
                    "type": "prometheus",
                    "uid": "prometheus"
                  },
                  "editorMode": "code",
                  "exemplar": true,
                  "expr": "f5_tmm_f5_tmm_cpu_utilization_15mins_per_10_2{}",
                  "interval": "",
                  "legendFormat": "{{tmmID}}-{{name}}-{{column}}",
                  "range": true,
                  "refId": "A"
                },
                {
                  "datasource": {
                    "type": "prometheus",
                    "uid": "P1809F7CD0C75ACF3"
                  },
                  "editorMode": "code",
                  "exemplar": true,
                  "expr": "f5_tmm_tmm_stat_2_cpu_usage_15mins{}",
                  "hide": false,
                  "interval": "",
                  "legendFormat": "{{tmmID}}-{{name}}-{{column}}",
                  "range": true,
                  "refId": "B"
                }
              ],
              "title": "F5 TMM CPU Usage 15 mins",
              "type": "timeseries"
            },
            {
              "datasource": {
                "type": "prometheus",
                "uid": "P1809F7CD0C75ACF3"
              },
              "fieldConfig": {
                "defaults": {
                  "color": {
                    "mode": "palette-classic"
                  },
                  "custom": {
                    "axisBorderShow": false,
                    "axisCenteredZero": false,
                    "axisColorMode": "text",
                    "axisLabel": "",
                    "axisPlacement": "auto",
                    "barAlignment": 0,
                    "barWidthFactor": 0.6,
                    "drawStyle": "line",
                    "fillOpacity": 0,
                    "gradientMode": "none",
                    "hideFrom": {
                      "legend": false,
                      "tooltip": false,
                      "viz": false
                    },
                    "insertNulls": false,
                    "lineInterpolation": "linear",
                    "lineWidth": 1,
                    "pointSize": 5,
                    "scaleDistribution": {
                      "type": "linear"
                    },
                    "showPoints": "auto",
                    "spanNulls": false,
                    "stacking": {
                      "group": "A",
                      "mode": "none"
                    },
                    "thresholdsStyle": {
                      "mode": "off"
                    }
                  },
                  "mappings": [],
                  "thresholds": {
                    "mode": "absolute",
                    "steps": [
                      {
                        "color": "green",
                        "value": null
                      },
                      {
                        "color": "red",
                        "value": 80
                      }
                    ]
                  },
                  "unit": "decbits"
                },
                "overrides": []
              },
              "gridPos": {
                "h": 10,
                "w": 8,
                "x": 8,
                "y": 2
              },
              "id": 2,
              "options": {
                "legend": {
                  "calcs": [],
                  "displayMode": "table",
                  "placement": "bottom",
                  "showLegend": true
                },
                "tooltip": {
                  "hideZeros": false,
                  "mode": "single",
                  "sort": "none"
                }
              },
              "pluginVersion": "11.5.2",
              "targets": [
                {
                  "datasource": {
                    "type": "prometheus",
                    "uid": "prometheus"
                  },
                  "editorMode": "code",
                  "exemplar": true,
                  "expr": "f5_tmm_f5_tmm_memory_usage_bytes{}",
                  "interval": "",
                  "legendFormat": "{{tmmID}}-{{name}}-{{column}}",
                  "range": true,
                  "refId": "A"
                },
                {
                  "datasource": {
                    "type": "prometheus",
                    "uid": "P1809F7CD0C75ACF3"
                  },
                  "editorMode": "code",
                  "exemplar": true,
                  "expr": "f5_tmm_tmm_stat_2_memory_total{}",
                  "hide": false,
                  "interval": "",
                  "legendFormat": "{{tmmID}}-{{name}}-{{column}}",
                  "range": true,
                  "refId": "B"
                }
              ],
              "title": "F5 TMM System Memory Usage",
              "type": "timeseries"
            },
            {
              "datasource": {
                "type": "prometheus",
                "uid": "P1809F7CD0C75ACF3"
              },
              "fieldConfig": {
                "defaults": {
                  "color": {
                    "mode": "palette-classic"
                  },
                  "custom": {
                    "axisBorderShow": false,
                    "axisCenteredZero": false,
                    "axisColorMode": "text",
                    "axisLabel": "",
                    "axisPlacement": "auto",
                    "barAlignment": 0,
                    "barWidthFactor": 0.6,
                    "drawStyle": "line",
                    "fillOpacity": 0,
                    "gradientMode": "none",
                    "hideFrom": {
                      "legend": false,
                      "tooltip": false,
                      "viz": false
                    },
                    "insertNulls": false,
                    "lineInterpolation": "linear",
                    "lineWidth": 1,
                    "pointSize": 5,
                    "scaleDistribution": {
                      "type": "linear"
                    },
                    "showPoints": "auto",
                    "spanNulls": false,
                    "stacking": {
                      "group": "A",
                      "mode": "none"
                    },
                    "thresholdsStyle": {
                      "mode": "off"
                    }
                  },
                  "mappings": [],
                  "thresholds": {
                    "mode": "absolute",
                    "steps": [
                      {
                        "color": "green",
                        "value": null
                      },
                      {
                        "color": "red",
                        "value": 80
                      }
                    ]
                  },
                  "unit": "decbits"
                },
                "overrides": []
              },
              "gridPos": {
                "h": 10,
                "w": 8,
                "x": 16,
                "y": 2
              },
              "id": 26,
              "options": {
                "legend": {
                  "calcs": [],
                  "displayMode": "table",
                  "placement": "bottom",
                  "showLegend": true
                },
                "tooltip": {
                  "hideZeros": false,
                  "mode": "single",
                  "sort": "none"
                }
              },
              "pluginVersion": "11.5.2",
              "targets": [
                {
                  "datasource": {
                    "type": "prometheus",
                    "uid": "P1809F7CD0C75ACF3"
                  },
                  "editorMode": "code",
                  "exemplar": true,
                  "expr": "f5_tmm_f5_l3_interface_transmitted_octets_total{}",
                  "expr": "sum (rate(f5_tmm_f5_l3_interface_transmitted_octets_total[5m])) by(f5_l3_interface_name)",
 
                  "interval": "",
                  "range": true,
                  "refId": "A"
                }
              ],
              "title": "F5 TMM Interface Bytes In/Out",
              "type": "timeseries"
            }
          ],
          "title": "TMM",
          "type": "row"
        },
        {
          "collapsed": true,
          "gridPos": {
            "h": 1,
            "w": 24,
            "x": 0,
            "y": 3
          },
          "id": 40,
          "panels": [
            {
              "datasource": {
                "type": "prometheus",
                "uid": "prometheus"
              },
              "fieldConfig": {
                "defaults": {
                  "color": {
                    "mode": "palette-classic"
                  },
                  "custom": {
                    "axisBorderShow": false,
                    "axisCenteredZero": false,
                    "axisColorMode": "text",
                    "axisLabel": "",
                    "axisPlacement": "auto",
                    "barAlignment": 0,
                    "barWidthFactor": 0.6,
                    "drawStyle": "line",
                    "fillOpacity": 0,
                    "gradientMode": "none",
                    "hideFrom": {
                      "legend": false,
                      "tooltip": false,
                      "viz": false
                    },
                    "insertNulls": false,
                    "lineInterpolation": "linear",
                    "lineWidth": 1,
                    "pointSize": 5,
                    "scaleDistribution": {
                      "type": "linear"
                    },
                    "showPoints": "auto",
                    "spanNulls": false,
                    "stacking": {
                      "group": "A",
                      "mode": "none"
                    },
                    "thresholdsStyle": {
                      "mode": "off"
                    }
                  },
                  "mappings": [],
                  "thresholds": {
                    "mode": "absolute",
                    "steps": [
                      {
                        "color": "green",
                        "value": null
                      },
                      {
                        "color": "red",
                        "value": 80
                      }
                    ]
                  },
                  "unit": "short"
                },
                "overrides": []
              },
              "gridPos": {
                "h": 10,
                "w": 8,
                "x": 0,
                "y": 4
              },
              "id": 20,
              "options": {
                "legend": {
                  "calcs": [],
                  "displayMode": "table",
                  "placement": "bottom",
                  "showLegend": true
                },
                "tooltip": {
                  "hideZeros": false,
                  "mode": "single",
                  "sort": "none"
                }
              },
              "pluginVersion": "11.5.2",
              "targets": [
                {
                  "datasource": {
                    "type": "prometheus",
                    "uid": "prometheus"
                  },
                  "editorMode": "code",
                  "exemplar": true,
                  "expr": "sum(rate(f5_tmm_f5_virtual_server_clientside_received_packets_total[5m])) by (f5_virtual_server_name)",
                  "interval": "",
                  "range": true,
                  "refId": "CS"
                }
              ],
              "title": "Virtual Server Packets In/Out Clientside",
              "type": "timeseries"
            },
            {
              "datasource": {
                "type": "prometheus",
                "uid": "prometheus"
              },
              "fieldConfig": {
                "defaults": {
                  "color": {
                    "mode": "palette-classic"
                  },
                  "custom": {
                    "axisBorderShow": false,
                    "axisCenteredZero": false,
                    "axisColorMode": "text",
                    "axisLabel": "",
                    "axisPlacement": "auto",
                    "barAlignment": 0,
                    "barWidthFactor": 0.6,
                    "drawStyle": "line",
                    "fillOpacity": 0,
                    "gradientMode": "none",
                    "hideFrom": {
                      "legend": false,
                      "tooltip": false,
                      "viz": false
                    },
                    "insertNulls": false,
                    "lineInterpolation": "linear",
                    "lineWidth": 1,
                    "pointSize": 5,
                    "scaleDistribution": {
                      "type": "linear"
                    },
                    "showPoints": "auto",
                    "spanNulls": false,
                    "stacking": {
                      "group": "A",
                      "mode": "none"
                    },
                    "thresholdsStyle": {
                      "mode": "off"
                    }
                  },
                  "mappings": [],
                  "thresholds": {
                    "mode": "absolute",
                    "steps": [
                      {
                        "color": "green",
                        "value": null
                      },
                      {
                        "color": "red",
                        "value": 80
                      }
                    ]
                  },
                  "unit": "short"
                },
                "overrides": []
              },
              "gridPos": {
                "h": 10,
                "w": 8,
                "x": 8,
                "y": 4
              },
              "id": 25,
              "options": {
                "legend": {
                  "calcs": [],
                  "displayMode": "table",
                  "placement": "bottom",
                  "showLegend": true
                },
                "tooltip": {
                  "hideZeros": false,
                  "mode": "single",
                  "sort": "none"
                }
              },
              "pluginVersion": "11.5.2",
              "targets": [
                {
                  "datasource": {
                    "type": "prometheus",
                    "uid": "prometheus"
                  },
                  "editorMode": "code",
                  "exemplar": true,
                  "expr": "sum(rate(f5_tmm_f5_virtual_server_serverside_transmitted_packets_total[5m])) by (f5_virtual_server_name)",
                  "interval": "",
                  "range": true,
                  "refId": "SS"
                }
              ],
              "title": "Virtual Server Packets In/Out Serverside",
              "type": "timeseries"
            },
            {
              "datasource": {
                "type": "prometheus",
                "uid": "prometheus"
              },
              "fieldConfig": {
                "defaults": {
                  "color": {
                    "mode": "palette-classic"
                  },
                  "custom": {
                    "axisBorderShow": false,
                    "axisCenteredZero": false,
                    "axisColorMode": "text",
                    "axisLabel": "",
                    "axisPlacement": "auto",
                    "barAlignment": 0,
                    "barWidthFactor": 0.6,
                    "drawStyle": "line",
                    "fillOpacity": 0,
                    "gradientMode": "none",
                    "hideFrom": {
                      "legend": false,
                      "tooltip": false,
                      "viz": false
                    },
                    "insertNulls": false,
                    "lineInterpolation": "linear",
                    "lineWidth": 1,
                    "pointSize": 5,
                    "scaleDistribution": {
                      "type": "linear"
                    },
                    "showPoints": "auto",
                    "spanNulls": false,
                    "stacking": {
                      "group": "A",
                      "mode": "none"
                    },
                    "thresholdsStyle": {
                      "mode": "off"
                    }
                  },
                  "mappings": [],
                  "thresholds": {
                    "mode": "absolute",
                    "steps": [
                      {
                        "color": "green",
                        "value": null
                      },
                      {
                        "color": "red",
                        "value": 80
                      }
                    ]
                  },
                  "unit": "decbits"
                },
                "overrides": []
              },
              "gridPos": {
                "h": 10,
                "w": 8,
                "x": 0,
                "y": 4
              },
              "id": 40,
              "options": {
                "legend": {
                  "calcs": [],
                  "displayMode": "table",
                  "placement": "bottom",
                  "showLegend": true
                },
                "tooltip": {
                  "hideZeros": false,
                  "mode": "single",
                  "sort": "none"
                }
              },
              "pluginVersion": "11.5.2",
              "targets": [
                {
                  "datasource": {
                    "type": "prometheus",
                    "uid": "prometheus"
                  },
                  "editorMode": "code",
                  "exemplar": true,
                  "expr": "sum(rate(f5_tmm_f5_virtual_server_serverside_received_bytes_total[5m])) by (f5_virtual_server_name)",
                  "interval": "",
                  "range": true,
                  "refId": "A"
                }
              ],
              "title": "Virtual Server Bytes In/Out Clientside",
              "type": "timeseries"
            },
            {
              "datasource": {
                "type": "prometheus",
                "uid": "prometheus"
              },
              "fieldConfig": {
                "defaults": {
                  "color": {
                    "mode": "palette-classic"
                  },
                  "custom": {
                    "axisBorderShow": false,
                    "axisCenteredZero": false,
                    "axisColorMode": "text",
                    "axisLabel": "",
                    "axisPlacement": "auto",
                    "barAlignment": 0,
                    "barWidthFactor": 0.6,
                    "drawStyle": "line",
                    "fillOpacity": 0,
                    "gradientMode": "none",
                    "hideFrom": {
                      "legend": false,
                      "tooltip": false,
                      "viz": false
                    },
                    "insertNulls": false,
                    "lineInterpolation": "linear",
                    "lineWidth": 1,
                    "pointSize": 5,
                    "scaleDistribution": {
                      "type": "linear"
                    },
                    "showPoints": "auto",
                    "spanNulls": false,
                    "stacking": {
                      "group": "A",
                      "mode": "none"
                    },
                    "thresholdsStyle": {
                      "mode": "off"
                    }
                  },
                  "mappings": [],
                  "thresholds": {
                    "mode": "absolute",
                    "steps": [
                      {
                        "color": "green",
                        "value": null
                      },
                      {
                        "color": "red",
                        "value": 80
                      }
                    ]
                  },
                  "unit": "decbits"
                },
                "overrides": []
              },
              "gridPos": {
                "h": 10,
                "w": 8,
                "x": 8,
                "y": 4
              },
              "id": 45,
              "options": {
                "legend": {
                  "calcs": [],
                  "displayMode": "table",
                  "placement": "bottom",
                  "showLegend": true
                },
                "tooltip": {
                  "hideZeros": false,
                  "mode": "single",
                  "sort": "none"
                }
              },
              "pluginVersion": "11.5.2",
              "targets": [
                {
                  "datasource": {
                    "type": "prometheus",
                    "uid": "prometheus"
                  },
                  "editorMode": "code",
                  "exemplar": true,
                  "expr": "sum(rate(f5_tmm_f5_virtual_server_serverside_received_bytes_total[5m])) by (f5_virtual_server_name)",
                  "interval": "",
                  "range": true,
                  "refId": "A"
                }
              ],
              "title": "Virtual Server Bytes In/Out Serverside",
              "type": "timeseries"
            },
            {
              "datasource": {
                "type": "prometheus",
                "uid": "prometheus"
              },
              "fieldConfig": {
                "defaults": {
                  "color": {
                    "mode": "palette-classic"
                  },
                  "custom": {
                    "axisBorderShow": false,
                    "axisCenteredZero": false,
                    "axisColorMode": "text",
                    "axisLabel": "",
                    "axisPlacement": "auto",
                    "barAlignment": 0,
                    "barWidthFactor": 0.6,
                    "drawStyle": "line",
                    "fillOpacity": 0,
                    "gradientMode": "none",
                    "hideFrom": {
                      "legend": false,
                      "tooltip": false,
                      "viz": false
                    },
                    "insertNulls": false,
                    "lineInterpolation": "linear",
                    "lineWidth": 1,
                    "pointSize": 5,
                    "scaleDistribution": {
                      "type": "linear"
                    },
                    "showPoints": "auto",
                    "spanNulls": false,
                    "stacking": {
                      "group": "A",
                      "mode": "none"
                    },
                    "thresholdsStyle": {
                      "mode": "off"
                    }
                  },
                  "mappings": [],
                  "thresholds": {
                    "mode": "absolute",
                    "steps": [
                      {
                        "color": "green",
                        "value": null
                      },
                      {
                        "color": "red",
                        "value": 80
                      }
                    ]
                  },
                  "unit": "short"
                },
                "overrides": []
              },
              "gridPos": {
                "h": 10,
                "w": 8,
                "x": 0,
                "y": 4
              },
              "id": 30,
              "options": {
                "legend": {
                  "calcs": [],
                  "displayMode": "table",
                  "placement": "bottom",
                  "showLegend": true
                },
                "tooltip": {
                  "hideZeros": false,
                  "mode": "single",
                  "sort": "none"
                }
              },
              "pluginVersion": "11.5.2",
              "targets": [
                {
                  "datasource": {
                    "type": "prometheus",
                    "uid": "prometheus"
                  },
                  "editorMode": "code",
                  "exemplar": true,
                  "expr": "f5_tmm_f5_virtual_server_clientside_connections_current{}",
                  "hide": false,
                  "interval": "",
                  "legendFormat": "{{f5_virtual_server_name}}",
                  "range": true,
                  "refId": "CS"
                }
              ],
              "title": "Virtual Server Connections Clientside",
              "type": "timeseries"
            },
            {
              "datasource": {
                "type": "prometheus",
                "uid": "prometheus"
              },
              "fieldConfig": {
                "defaults": {
                  "color": {
                    "mode": "palette-classic"
                  },
                  "custom": {
                    "axisBorderShow": false,
                    "axisCenteredZero": false,
                    "axisColorMode": "text",
                    "axisLabel": "",
                    "axisPlacement": "auto",
                    "barAlignment": 0,
                    "barWidthFactor": 0.6,
                    "drawStyle": "line",
                    "fillOpacity": 0,
                    "gradientMode": "none",
                    "hideFrom": {
                      "legend": false,
                      "tooltip": false,
                      "viz": false
                    },
                    "insertNulls": false,
                    "lineInterpolation": "linear",
                    "lineWidth": 1,
                    "pointSize": 5,
                    "scaleDistribution": {
                      "type": "linear"
                    },
                    "showPoints": "auto",
                    "spanNulls": false,
                    "stacking": {
                      "group": "A",
                      "mode": "none"
                    },
                    "thresholdsStyle": {
                      "mode": "off"
                    }
                  },
                  "mappings": [],
                  "thresholds": {
                    "mode": "absolute",
                    "steps": [
                      {
                        "color": "green",
                        "value": null
                      },
                      {
                        "color": "red",
                        "value": 80
                      }
                    ]
                  },
                  "unit": "short"
                },
                "overrides": []
              },
              "gridPos": {
                "h": 10,
                "w": 8,
                "x": 8,
                "y": 4
              },
              "id": 35,
              "options": {
                "legend": {
                  "calcs": [],
                  "displayMode": "table",
                  "placement": "bottom",
                  "showLegend": true
                },
                "tooltip": {
                  "hideZeros": false,
                  "mode": "single",
                  "sort": "none"
                }
              },
              "pluginVersion": "11.5.2",
              "targets": [
                {
                  "datasource": {
                    "type": "prometheus",
                    "uid": "prometheus"
                  },
                  "editorMode": "code",
                  "exemplar": true,
                  "expr": "f5_tmm_f5_virtual_server_clientside_connections_current{}",
                  "interval": "",
                  "legendFormat": "{{f5_virtual_server_name}}",
                  "range": true,
                  "refId": "SS"
                }
              ],
              "title": "Virtual Server Connections Serverside",
              "type": "timeseries"
            }
          ],
          "title": "BNK Applications",
          "type": "row"
        },
        {
          "collapsed": true,
          "gridPos": {
            "h": 1,
            "w": 24,
            "x": 0,
            "y": 3
          },
          "id": 40,
          "panels": [
            {
              "datasource": {
                "type": "prometheus",
                "uid": "prometheus"
              },
              "fieldConfig": {
                "defaults": {
                  "color": {
                    "mode": "palette-classic"
                  },
                  "custom": {
                    "axisBorderShow": false,
                    "axisCenteredZero": false,
                    "axisColorMode": "text",
                    "axisLabel": "",
                    "axisPlacement": "auto",
                    "barAlignment": 0,
                    "barWidthFactor": 0.6,
                    "drawStyle": "line",
                    "fillOpacity": 0,
                    "gradientMode": "none",
                    "hideFrom": {
                      "legend": false,
                      "tooltip": false,
                      "viz": false
                    },
                    "insertNulls": false,
                    "lineInterpolation": "linear",
                    "lineWidth": 1,
                    "pointSize": 5,
                    "scaleDistribution": {
                      "type": "linear"
                    },
                    "showPoints": "auto",
                    "spanNulls": false,
                    "stacking": {
                      "group": "A",
                      "mode": "none"
                    },
                    "thresholdsStyle": {
                      "mode": "off"
                    }
                  },
                  "mappings": [],
                  "thresholds": {
                    "mode": "absolute",
                    "steps": [
                      {
                        "color": "green",
                        "value": null
                      },
                      {
                        "color": "red",
                        "value": 80
                      }
                    ]
                  },
                  "unit": "short"
                },
                "overrides": []
              },
              "gridPos": {
                "h": 10,
                "w": 8,
                "x": 0,
                "y": 4
              },
              "id": 20,
              "options": {
                "legend": {
                  "calcs": [],
                  "displayMode": "table",
                  "placement": "bottom",
                  "showLegend": true
                },
                "tooltip": {
                  "hideZeros": false,
                  "mode": "single",
                  "sort": "none"
                }
              },
              "pluginVersion": "11.5.2",
              "targets": [
                {
                  "datasource": {
                    "type": "prometheus",
                    "uid": "prometheus"
                  },
                  "editorMode": "code",
                  "exemplar": true,
                  "expr": "sum(rate(f5_tmm_f5_virtual_server_clientside_received_packets_total{f5_virtual_server_name=~\"myapp1-.*-vs\"}[5m])) by (f5_virtual_server_name)",
                  "interval": "",
                  "range": true,
                  "refId": "CS"
                }
              ],
              "title": "Virtual Server Packets In/Out Clientside",
              "type": "timeseries"
            },
            {
              "datasource": {
                "type": "prometheus",
                "uid": "prometheus"
              },
              "fieldConfig": {
                "defaults": {
                  "color": {
                    "mode": "palette-classic"
                  },
                  "custom": {
                    "axisBorderShow": false,
                    "axisCenteredZero": false,
                    "axisColorMode": "text",
                    "axisLabel": "",
                    "axisPlacement": "auto",
                    "barAlignment": 0,
                    "barWidthFactor": 0.6,
                    "drawStyle": "line",
                    "fillOpacity": 0,
                    "gradientMode": "none",
                    "hideFrom": {
                      "legend": false,
                      "tooltip": false,
                      "viz": false
                    },
                    "insertNulls": false,
                    "lineInterpolation": "linear",
                    "lineWidth": 1,
                    "pointSize": 5,
                    "scaleDistribution": {
                      "type": "linear"
                    },
                    "showPoints": "auto",
                    "spanNulls": false,
                    "stacking": {
                      "group": "A",
                      "mode": "none"
                    },
                    "thresholdsStyle": {
                      "mode": "off"
                    }
                  },
                  "mappings": [],
                  "thresholds": {
                    "mode": "absolute",
                    "steps": [
                      {
                        "color": "green",
                        "value": null
                      },
                      {
                        "color": "red",
                        "value": 80
                      }
                    ]
                  },
                  "unit": "short"
                },
                "overrides": []
              },
              "gridPos": {
                "h": 10,
                "w": 8,
                "x": 8,
                "y": 4
              },
              "id": 25,
              "options": {
                "legend": {
                  "calcs": [],
                  "displayMode": "table",
                  "placement": "bottom",
                  "showLegend": true
                },
                "tooltip": {
                  "hideZeros": false,
                  "mode": "single",
                  "sort": "none"
                }
              },
              "pluginVersion": "11.5.2",
              "targets": [
                {
                  "datasource": {
                    "type": "prometheus",
                    "uid": "prometheus"
                  },
                  "editorMode": "code",
                  "exemplar": true,
                  "expr": "sum(rate(f5_tmm_f5_virtual_server_serverside_transmitted_packets_total{f5_virtual_server_name=~\"myapp1-.*-vs\"}[5m])) by (f5_virtual_server_name)",
                  "interval": "",
                  "range": true,
                  "refId": "SS"
                }
              ],
              "title": "Virtual Server Packets In/Out Serverside",
              "type": "timeseries"
            },
            {
              "datasource": {
                "type": "prometheus",
                "uid": "prometheus"
              },
              "fieldConfig": {
                "defaults": {
                  "color": {
                    "mode": "palette-classic"
                  },
                  "custom": {
                    "axisBorderShow": false,
                    "axisCenteredZero": false,
                    "axisColorMode": "text",
                    "axisLabel": "",
                    "axisPlacement": "auto",
                    "barAlignment": 0,
                    "barWidthFactor": 0.6,
                    "drawStyle": "line",
                    "fillOpacity": 0,
                    "gradientMode": "none",
                    "hideFrom": {
                      "legend": false,
                      "tooltip": false,
                      "viz": false
                    },
                    "insertNulls": false,
                    "lineInterpolation": "linear",
                    "lineWidth": 1,
                    "pointSize": 5,
                    "scaleDistribution": {
                      "type": "linear"
                    },
                    "showPoints": "auto",
                    "spanNulls": false,
                    "stacking": {
                      "group": "A",
                      "mode": "none"
                    },
                    "thresholdsStyle": {
                      "mode": "off"
                    }
                  },
                  "mappings": [],
                  "thresholds": {
                    "mode": "absolute",
                    "steps": [
                      {
                        "color": "green",
                        "value": null
                      },
                      {
                        "color": "red",
                        "value": 80
                      }
                    ]
                  },
                  "unit": "decbits"
                },
                "overrides": []
              },
              "gridPos": {
                "h": 10,
                "w": 8,
                "x": 0,
                "y": 4
              },
              "id": 40,
              "options": {
                "legend": {
                  "calcs": [],
                  "displayMode": "table",
                  "placement": "bottom",
                  "showLegend": true
                },
                "tooltip": {
                  "hideZeros": false,
                  "mode": "single",
                  "sort": "none"
                }
              },
              "pluginVersion": "11.5.2",
              "targets": [
                {
                  "datasource": {
                    "type": "prometheus",
                    "uid": "prometheus"
                  },
                  "editorMode": "code",
                  "exemplar": true,
                  "expr": "sum(rate(f5_tmm_f5_virtual_server_serverside_received_bytes_total{f5_virtual_server_name=~\"myapp1-.*-vs\"}[5m])) by (f5_virtual_server_name)",
                  "interval": "",
                  "range": true,
                  "refId": "A"
                }
              ],
              "title": "Virtual Server Bytes In/Out Clientside",
              "type": "timeseries"
            },
            {
              "datasource": {
                "type": "prometheus",
                "uid": "prometheus"
              },
              "fieldConfig": {
                "defaults": {
                  "color": {
                    "mode": "palette-classic"
                  },
                  "custom": {
                    "axisBorderShow": false,
                    "axisCenteredZero": false,
                    "axisColorMode": "text",
                    "axisLabel": "",
                    "axisPlacement": "auto",
                    "barAlignment": 0,
                    "barWidthFactor": 0.6,
                    "drawStyle": "line",
                    "fillOpacity": 0,
                    "gradientMode": "none",
                    "hideFrom": {
                      "legend": false,
                      "tooltip": false,
                      "viz": false
                    },
                    "insertNulls": false,
                    "lineInterpolation": "linear",
                    "lineWidth": 1,
                    "pointSize": 5,
                    "scaleDistribution": {
                      "type": "linear"
                    },
                    "showPoints": "auto",
                    "spanNulls": false,
                    "stacking": {
                      "group": "A",
                      "mode": "none"
                    },
                    "thresholdsStyle": {
                      "mode": "off"
                    }
                  },
                  "mappings": [],
                  "thresholds": {
                    "mode": "absolute",
                    "steps": [
                      {
                        "color": "green",
                        "value": null
                      },
                      {
                        "color": "red",
                        "value": 80
                      }
                    ]
                  },
                  "unit": "decbits"
                },
                "overrides": []
              },
              "gridPos": {
                "h": 10,
                "w": 8,
                "x": 8,
                "y": 4
              },
              "id": 45,
              "options": {
                "legend": {
                  "calcs": [],
                  "displayMode": "table",
                  "placement": "bottom",
                  "showLegend": true
                },
                "tooltip": {
                  "hideZeros": false,
                  "mode": "single",
                  "sort": "none"
                }
              },
              "pluginVersion": "11.5.2",
              "targets": [
                {
                  "datasource": {
                    "type": "prometheus",
                    "uid": "prometheus"
                  },
                  "editorMode": "code",
                  "exemplar": true,
                  "expr": "sum(rate(f5_tmm_f5_virtual_server_serverside_received_bytes_total{f5_virtual_server_name=~\"myapp1-.*-vs\"}[5m])) by (f5_virtual_server_name)",
                  "interval": "",
                  "range": true,
                  "refId": "A"
                }
              ],
              "title": "Virtual Server Bytes In/Out Serverside",
              "type": "timeseries"
            },
            {
              "datasource": {
                "type": "prometheus",
                "uid": "prometheus"
              },
              "fieldConfig": {
                "defaults": {
                  "color": {
                    "mode": "palette-classic"
                  },
                  "custom": {
                    "axisBorderShow": false,
                    "axisCenteredZero": false,
                    "axisColorMode": "text",
                    "axisLabel": "",
                    "axisPlacement": "auto",
                    "barAlignment": 0,
                    "barWidthFactor": 0.6,
                    "drawStyle": "line",
                    "fillOpacity": 0,
                    "gradientMode": "none",
                    "hideFrom": {
                      "legend": false,
                      "tooltip": false,
                      "viz": false
                    },
                    "insertNulls": false,
                    "lineInterpolation": "linear",
                    "lineWidth": 1,
                    "pointSize": 5,
                    "scaleDistribution": {
                      "type": "linear"
                    },
                    "showPoints": "auto",
                    "spanNulls": false,
                    "stacking": {
                      "group": "A",
                      "mode": "none"
                    },
                    "thresholdsStyle": {
                      "mode": "off"
                    }
                  },
                  "mappings": [],
                  "thresholds": {
                    "mode": "absolute",
                    "steps": [
                      {
                        "color": "green",
                        "value": null
                      },
                      {
                        "color": "red",
                        "value": 80
                      }
                    ]
                  },
                  "unit": "short"
                },
                "overrides": []
              },
              "gridPos": {
                "h": 10,
                "w": 8,
                "x": 0,
                "y": 4
              },
              "id": 30,
              "options": {
                "legend": {
                  "calcs": [],
                  "displayMode": "table",
                  "placement": "bottom",
                  "showLegend": true
                },
                "tooltip": {
                  "hideZeros": false,
                  "mode": "single",
                  "sort": "none"
                }
              },
              "pluginVersion": "11.5.2",
              "targets": [
                {
                  "datasource": {
                    "type": "prometheus",
                    "uid": "prometheus"
                  },
                  "editorMode": "code",
                  "exemplar": true,
                  "expr": "f5_tmm_f5_virtual_server_clientside_connections_current{f5_virtual_server_name=~\"myapp1-.*-vs\"}",
                  "hide": false,
                  "interval": "",
                  "legendFormat": "{{f5_virtual_server_name}}",
                  "range": true,
                  "refId": "CS"
                }
              ],
              "title": "Virtual Server Connections Clientside",
              "type": "timeseries"
            },
            {
              "datasource": {
                "type": "prometheus",
                "uid": "prometheus"
              },
              "fieldConfig": {
                "defaults": {
                  "color": {
                    "mode": "palette-classic"
                  },
                  "custom": {
                    "axisBorderShow": false,
                    "axisCenteredZero": false,
                    "axisColorMode": "text",
                    "axisLabel": "",
                    "axisPlacement": "auto",
                    "barAlignment": 0,
                    "barWidthFactor": 0.6,
                    "drawStyle": "line",
                    "fillOpacity": 0,
                    "gradientMode": "none",
                    "hideFrom": {
                      "legend": false,
                      "tooltip": false,
                      "viz": false
                    },
                    "insertNulls": false,
                    "lineInterpolation": "linear",
                    "lineWidth": 1,
                    "pointSize": 5,
                    "scaleDistribution": {
                      "type": "linear"
                    },
                    "showPoints": "auto",
                    "spanNulls": false,
                    "stacking": {
                      "group": "A",
                      "mode": "none"
                    },
                    "thresholdsStyle": {
                      "mode": "off"
                    }
                  },
                  "mappings": [],
                  "thresholds": {
                    "mode": "absolute",
                    "steps": [
                      {
                        "color": "green",
                        "value": null
                      },
                      {
                        "color": "red",
                        "value": 80
                      }
                    ]
                  },
                  "unit": "short"
                },
                "overrides": []
              },
              "gridPos": {
                "h": 10,
                "w": 8,
                "x": 8,
                "y": 4
              },
              "id": 35,
              "options": {
                "legend": {
                  "calcs": [],
                  "displayMode": "table",
                  "placement": "bottom",
                  "showLegend": true
                },
                "tooltip": {
                  "hideZeros": false,
                  "mode": "single",
                  "sort": "none"
                }
              },
              "pluginVersion": "11.5.2",
              "targets": [
                {
                  "datasource": {
                    "type": "prometheus",
                    "uid": "prometheus"
                  },
                  "editorMode": "code",
                  "exemplar": true,
                  "expr": "f5_tmm_f5_virtual_server_clientside_connections_current{f5_virtual_server_name=~\"myapp1-.*-vs\"}",
                  "interval": "",
                  "legendFormat": "{{f5_virtual_server_name}}",
                  "range": true,
                  "refId": "SS"
                }
              ],
              "title": "Virtual Server Connections Serverside",
              "type": "timeseries"
            }
          ],
          "title": "Apps in myapp1 namespace",
          "type": "row"
        }
      ],
      "preload": false,
      "refresh": "",
      "schemaVersion": 40,
      "tags": [],
      "templating": {
        "list": []
      },
      "time": {
        "from": "now-1h",
        "to": "now"
      },
      "timepicker": {},
      "timezone": "",
      "title": "F5 BNK Dashboard",
      "uid": null,
      "id": null,
      "version": 4,
      "weekStart": ""
    }
EOF
```
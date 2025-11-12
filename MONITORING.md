# axionax Protocol - Monitoring & Observability 📊

Complete guide to monitoring axionax protocol infrastructure with Prometheus and Grafana.

**Last Updated**: November 12, 2025

---

## Overview

The axionax protocol monitoring stack provides real-time visibility into:
- Service health status (9 services)
- Resource utilization (CPU, RAM, Disk)
- Network performance
- Blockchain metrics (blocks, transactions, peers)
- Container health

### Monitoring Stack

| Component | Port | Purpose |
|-----------|------|---------|
| **Prometheus** | 9090 | Metrics collection & storage |
| **Grafana** | 3030 | Visualization & dashboards |
| **Node Exporter** | 9100 | System metrics (optional) |

---

## Quick Start

### Access Monitoring Dashboards

```bash
# Grafana (recommended)
http://YOUR_VPS_IP:3030

# Prometheus (advanced)
http://YOUR_VPS_IP:9090
```

**Default Credentials:**
- Username: `admin`
- Password: Set in `.env` as `GRAFANA_PASSWORD`

### Check Service Health

```bash
cd /opt/axionax-deploy
./scripts/check-vps-status.sh           # Quick summary
./scripts/check-vps-status.sh --detailed # Full details
```

---

## Prometheus Configuration

### Scrape Jobs (8 Services)

Prometheus collects metrics from all axionax services:

```yaml
# /opt/axionax-deploy/monitoring/prometheus.yml
scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']
  
  - job_name: 'axionax-rpc'
    static_configs:
      - targets: ['axionax-rpc:8545']
    metrics_path: '/metrics'
    scrape_interval: 15s
  
  - job_name: 'explorer-backend'
    static_configs:
      - targets: ['axionax-explorer-api:3001']
  
  - job_name: 'faucet'
    static_configs:
      - targets: ['axionax-faucet-api:3002']
  
  - job_name: 'postgres'
    static_configs:
      - targets: ['axionax-postgres:5432']
  
  - job_name: 'redis'
    static_configs:
      - targets: ['axionax-redis:6379']
  
  - job_name: 'nginx'
    static_configs:
      - targets: ['axionax-nginx:80']
  
  - job_name: 'web'
    static_configs:
      - targets: ['axionax-web:3000']
```

### Key Metrics

| Metric | Description |
|--------|-------------|
| `up` | Service availability (1=up, 0=down) |
| `process_cpu_seconds_total` | CPU usage |
| `process_resident_memory_bytes` | Memory usage |
| `http_requests_total` | HTTP request count |
| `http_request_duration_seconds` | Request latency |
| `axionax_block_height` | Current block height |
| `axionax_peer_count` | Connected peers |

---

## Grafana Setup

### Initial Configuration

1. **Login** to Grafana at `http://YOUR_VPS_IP:3030`
2. **Change Password** (first login)
3. **Verify Datasource**:
   - Navigate to Configuration → Data Sources
   - Prometheus should be pre-configured at `http://prometheus:9090`

### Pre-configured Dashboards

The deployment includes auto-provisioned dashboards:

1. **axionax Overview** - System health summary
2. **Service Health** - All 9 services status
3. **Resource Usage** - CPU, RAM, Disk metrics
4. **RPC Metrics** - Node performance
5. **Database Performance** - PostgreSQL metrics

### Creating Custom Dashboards

```bash
# Add custom dashboard JSON
cd /opt/axionax-deploy/monitoring/grafana/dashboards
nano custom-dashboard.json

# Restart Grafana to load
docker-compose -f docker-compose.vps.yml restart grafana
```

---

## Health Check Endpoints

### Service Health URLs

```bash
# RPC Node
curl http://localhost:8545/health
# Response: {"status":"healthy","network":"axionax-testnet-1","chainId":"888"...}

# Prometheus
curl http://localhost:9090/-/healthy
# Response: Prometheus is Healthy.

# Grafana
curl http://localhost:3030/api/health
# Response: {"database":"ok","version":"..."}

# Nginx
curl http://localhost/
# Response: HTML (200 OK)

# Check all services
cd /opt/axionax-deploy
./scripts/check-vps-status.sh
```

### Health Check Script Output

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  SERVICE HEALTH STATUS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Service              Port     Status
-------              ----     ------
Prometheus           9090     ✓ Healthy
Grafana              3030     ✓ Healthy
Web Server           80       ✓ Healthy
Explorer API         3001     ✓ Healthy
Redis                6379     ✓ Healthy
PostgreSQL           5432     ✓ Healthy
HTTPS                443      ✓ Healthy
Faucet API           3002     ✓ Healthy
RPC HTTP             8545     ✓ Healthy
RPC WebSocket        8546     ✓ Healthy
```

---

## Alerting (Optional)

### Configure Alerts in Grafana

1. Navigate to **Alerting** → **Alert Rules**
2. Create new alert rule:

**Example: High CPU Usage Alert**

```yaml
Alert Rule: High CPU Usage
Condition: avg(rate(process_cpu_seconds_total[5m])) > 0.8
Evaluation: Every 1m for 5m
Notification: Email/Slack
```

**Example: Service Down Alert**

```yaml
Alert Rule: Service Unavailable
Condition: up{job="axionax-rpc"} == 0
Evaluation: Every 30s for 1m
Notification: Critical - Immediate
```

### Notification Channels

Configure in `docker-compose.vps.yml`:

```yaml
grafana:
  environment:
    - GF_SMTP_ENABLED=true
    - GF_SMTP_HOST=smtp.gmail.com:587
    - GF_SMTP_USER=alerts@axionax.org
    - GF_SMTP_PASSWORD=${SMTP_PASSWORD}
```

---

## Troubleshooting

### Prometheus Not Collecting Metrics

```bash
# Check Prometheus targets
curl http://localhost:9090/api/v1/targets | jq

# View Prometheus logs
docker logs axionax-prometheus --tail 100

# Verify scrape config
docker exec axionax-prometheus cat /etc/prometheus/prometheus.yml
```

### Grafana Can't Connect to Prometheus

```bash
# Test connection from Grafana container
docker exec axionax-grafana curl -I http://prometheus:9090

# Check Docker network
docker network inspect axionax-deploy_default

# Restart Grafana
docker-compose -f docker-compose.vps.yml restart grafana
```

### Health Check Script Issues

```bash
# View script
cat /opt/axionax-deploy/scripts/check-vps-status.sh

# Check script permissions
chmod +x /opt/axionax-deploy/scripts/check-vps-status.sh

# Run with debug
bash -x /opt/axionax-deploy/scripts/check-vps-status.sh
```

---

## Performance Optimization

### Prometheus Data Retention

Edit `monitoring/prometheus.yml`:

```yaml
global:
  scrape_interval: 15s
  evaluation_interval: 15s
  
# Add storage retention
storage:
  tsdb:
    retention.time: 15d  # Keep metrics for 15 days
    retention.size: 5GB  # Maximum storage size
```

### Grafana Performance

```yaml
grafana:
  environment:
    - GF_DATABASE_MAX_OPEN_CONN=10
    - GF_DATABASE_MAX_IDLE_CONN=2
    - GF_CACHE_ENABLED=true
```

---

## Backup & Recovery

### Export Grafana Dashboards

```bash
# Export all dashboards
docker exec axionax-grafana grafana-cli admin export-dashboard \
  --output /var/lib/grafana/backups/

# Copy to host
docker cp axionax-grafana:/var/lib/grafana/backups/ ./grafana-backup/
```

### Prometheus Data Backup

```bash
# Stop Prometheus
docker-compose -f docker-compose.vps.yml stop prometheus

# Backup data
tar czf prometheus-backup-$(date +%Y%m%d).tar.gz \
  /var/lib/docker/volumes/axionax-deploy_prometheus-data/

# Restart
docker-compose -f docker-compose.vps.yml start prometheus
```

---

## Monitoring Best Practices

### 1. Regular Health Checks

```bash
# Add to crontab
*/5 * * * * /opt/axionax-deploy/scripts/check-vps-status.sh > /var/log/axionax-health.log
```

### 2. Set Up Alerts

- Service down alerts (critical)
- High resource usage (warning at 80%, critical at 90%)
- Disk space alerts (warning at 75%)
- Certificate expiry (warning at 30 days)

### 3. Dashboard Review

- Check dashboards daily during testnet
- Review anomalies weekly
- Update alert thresholds based on actual usage

### 4. Log Retention

```bash
# Configure Docker logging
# docker-compose.vps.yml
services:
  axionax-rpc:
    logging:
      driver: "json-file"
      options:
        max-size: "10m"
        max-file: "3"
```

---

## Security Considerations

### Restrict Monitoring Access

```nginx
# nginx/conf.d/monitoring.conf
server {
    listen 443 ssl;
    server_name monitoring.axionax.org;
    
    # IP whitelist
    allow 203.0.113.0/24;  # Your office IP
    deny all;
    
    location / {
        proxy_pass http://localhost:3030;
    }
}
```

### Authentication

Both Prometheus and Grafana are accessible only from:
- Localhost (Docker network)
- VPS IP directly
- Behind nginx reverse proxy with SSL

**Never expose monitoring ports (9090, 3030) directly to the internet.**

---

## Related Documentation

- [Health Checks Guide](HEALTH_CHECKS.md)
- [VPS Operations](VPS_OPERATIONS.md)
- [Deployment Guide](https://github.com/axionaxprotocol/axionax-deploy)
- [Troubleshooting](TROUBLESHOOTING.md)

---

## Support

- **GitHub Issues**: [axionax-deploy/issues](https://github.com/axionaxprotocol/axionax-deploy/issues)
- **Documentation**: [docs.axionax.org](https://docs.axionax.org)

---

**Current Monitoring Status (Nov 12, 2025):**
- ✅ Prometheus: Operational (Port 9090)
- ✅ Grafana: Operational (Port 3030)
- ✅ All 9 Services: Healthy
- ✅ Health Checks: Active
- ✅ Metrics Collection: 15s intervals

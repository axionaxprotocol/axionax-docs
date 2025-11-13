# axionax Protocol - Service Health Checks 🏥

Comprehensive guide to health check system for axionax protocol services.

**Last Updated**: November 13, 2025

---

## Overview

The axionax protocol deployment includes a robust health checking system that monitors all 9 services in real-time.

### Monitored Services (Current Status: 7/9 Operational)

| # | Service | Port | Health Check Method | Status |
|---|---------|------|---------------------|--------|
| 1 | Web Server (Nginx) | 80 | HTTP GET / | ✅ Healthy |
| 2 | HTTPS | 443 | TCP Connect | ✅ Healthy |
| 3 | Grafana | 3030 | HTTP /api/health | ✅ Healthy |
| 4 | RPC HTTP | 8545 | HTTP /health | ✅ Healthy |
| 5 | RPC WebSocket | 8546 | TCP Connect | ✅ Healthy |
| 6 | Explorer API | 3001 | Container Status | ❌ Not Responding |
| 7 | Faucet API | 3002 | Container Status | ❌ Not Responding |
| 8 | Prometheus | 9090 | HTTP /-/healthy | ✅ Healthy |
| 9 | PostgreSQL | 5432 | pg_isready | ✅ Healthy |
| 10 | Redis | 6379 | redis-cli ping | ✅ Healthy |

**Infrastructure Services**: 5/5 Healthy ✅  
**Monitoring Stack**: 2/2 Healthy ✅  
**Application Services**: 0/2 Responding ❌ (Troubleshooting in progress)

---

## Quick Health Check

### Using Built-in Script

```bash
cd /opt/axionax-deploy

# Quick summary (recommended)
./scripts/check-vps-status.sh

# Detailed output
./scripts/check-vps-status.sh --detailed
```

### Example Output

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  axionax VPS Status Check
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Timestamp: Wed Nov 12 16:10:12 CET 2025
Hostname:  vmi2895217
IP:        217.216.109.5

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  QUICK SUMMARY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Services Status: 9/9 running ✅
✓ All services are healthy

Resource Usage:
  RAM:  12%
  Disk: 17%

✓ Overall Status: HEALTHY ✅

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

## Health Check Methods

### 1. HTTP Health Endpoints

Services with dedicated health endpoints:

#### RPC Node (Port 8545)

```bash
curl http://localhost:8545/health
```

**Response:**
```json
{
  "status": "healthy",
  "network": "axionax-testnet-1",
  "chainId": "888",
  "blockNumber": 1000,
  "timestamp": 1762959327153
}
```

#### Prometheus (Port 9090)

```bash
curl http://localhost:9090/-/healthy
```

**Response:**
```
Prometheus is Healthy.
```

#### Grafana (Port 3030)

```bash
curl http://localhost:3030/api/health
```

**Response:**
```json
{
  "database": "ok",
  "version": "10.2.0"
}
```

### 2. TCP Connection Tests

For services without HTTP endpoints:

```bash
# HTTPS (443)
nc -z localhost 443 && echo "✓ HTTPS Healthy"

# RPC WebSocket (8546)
nc -z localhost 8546 && echo "✓ WebSocket Healthy"
```

### 3. Container Status Checks

For placeholder services (Explorer, Faucet):

```bash
# Explorer API
docker ps --filter name=axionax-explorer-api --filter status=running --format '{{.Names}}'

# Faucet API
docker ps --filter name=axionax-faucet-api --filter status=running --format '{{.Names}}'
```

**Note**: Explorer and Faucet are placeholder implementations. Health check verifies container is running.

### 4. Database Health Checks

#### PostgreSQL

```bash
docker exec axionax-postgres pg_isready -U explorer
```

**Response:**
```
/var/run/postgresql:5432 - accepting connections
```

#### Redis

```bash
docker exec axionax-redis redis-cli ping
```

**Response:**
```
PONG
```

---

## Docker Health Checks

Each service has built-in Docker health checks defined in `docker-compose.vps.yml`:

### RPC Node

```yaml
healthcheck:
  test: ["CMD", "curl", "-f", "http://localhost:8545/health"]
  interval: 30s
  timeout: 10s
  retries: 3
```

### PostgreSQL

```yaml
healthcheck:
  test: ["CMD-SHELL", "pg_isready -U explorer"]
  interval: 10s
  timeout: 5s
  retries: 5
```

### Redis

```yaml
healthcheck:
  test: ["CMD", "redis-cli", "ping"]
  interval: 10s
  timeout: 5s
  retries: 5
```

### Nginx

```yaml
healthcheck:
  test: ["CMD", "nginx", "-t"]
  interval: 30s
  timeout: 10s
  retries: 3
```

### View Container Health Status

```bash
# All containers
docker ps

# Specific service
docker inspect --format='{{.State.Health.Status}}' axionax-rpc

# Health check logs
docker inspect --format='{{json .State.Health}}' axionax-rpc | jq
```

---

## Automated Monitoring

### Cron Job Setup

Add to `/etc/crontab`:

```bash
# Check health every 5 minutes
*/5 * * * * root /opt/axionax-deploy/scripts/check-vps-status.sh > /var/log/axionax-health.log 2>&1

# Alert if unhealthy (requires mail setup)
*/10 * * * * root /opt/axionax-deploy/scripts/check-vps-status.sh | grep -q "Unhealthy" && echo "Service unhealthy!" | mail -s "axionax Alert" admin@axionax.org
```

### Systemd Service

Create `/etc/systemd/system/axionax-health-monitor.service`:

```ini
[Unit]
Description=axionax Health Monitor
After=docker.service

[Service]
Type=oneshot
ExecStart=/opt/axionax-deploy/scripts/check-vps-status.sh
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
```

Create `/etc/systemd/system/axionax-health-monitor.timer`:

```ini
[Unit]
Description=axionax Health Monitor Timer

[Timer]
OnBootSec=5min
OnUnitActiveSec=5min

[Install]
WantedBy=timers.target
```

Enable:

```bash
systemctl enable axionax-health-monitor.timer
systemctl start axionax-health-monitor.timer
```

---

## Troubleshooting Unhealthy Services

### Service Shows as Unhealthy

1. **Check container status:**
   ```bash
   docker ps -a | grep axionax
   ```

2. **View container logs:**
   ```bash
   docker logs axionax-[service-name] --tail 50
   ```

3. **Test health endpoint manually:**
   ```bash
   curl -v http://localhost:[port]/health
   ```

4. **Restart service:**
   ```bash
   docker-compose -f docker-compose.vps.yml restart [service-name]
   ```

### RPC Node Unhealthy

**Common Issues:**

1. **Syncing Blockchain:**
   - Initial sync can take time
   - Check logs: `docker logs axionax-rpc --tail 100`
   - Wait for "Sync complete" message

2. **Port Not Accessible:**
   ```bash
   # Check if port is listening
   netstat -tuln | grep 8545
   
   # Test connection
   curl http://localhost:8545/health
   ```

3. **Container Crash Loop:**
   ```bash
   docker inspect axionax-rpc | grep RestartCount
   docker logs axionax-rpc --tail 200
   ```

### Database Unhealthy

**PostgreSQL:**

```bash
# Check if running
docker ps | grep postgres

# Check logs
docker logs axionax-postgres --tail 50

# Connect to database
docker exec -it axionax-postgres psql -U explorer

# Check disk space
df -h
```

**Redis:**

```bash
# Test connection
docker exec axionax-redis redis-cli ping

# Check memory
docker exec axionax-redis redis-cli info memory

# View logs
docker logs axionax-redis --tail 50
```

### Monitoring Services Unhealthy

**Prometheus:**

```bash
# Check configuration
docker exec axionax-prometheus promtool check config /etc/prometheus/prometheus.yml

# View targets
curl http://localhost:9090/api/v1/targets | jq

# Restart
docker-compose -f docker-compose.vps.yml restart prometheus
```

**Grafana:**

```bash
# Check datasource connection
curl http://localhost:3030/api/datasources

# View logs
docker logs axionax-grafana --tail 50

# Reset admin password
docker exec -it axionax-grafana grafana-cli admin reset-admin-password newpassword
```

---

## Health Check Script Details

### Script Location

```
/opt/axionax-deploy/scripts/check-vps-status.sh
```

### How It Works

1. **System Resources Check:**
   - RAM usage (warns if >80%)
   - Disk usage (warns if >75%)
   - CPU load average
   - System uptime

2. **Docker Status Check:**
   - Docker service running
   - Container count (running/total)
   - Docker disk usage

3. **Service Health Checks:**
   - HTTP endpoints (curl)
   - TCP connections (nc)
   - Container status (docker ps)
   - Database readiness checks

4. **Output Formatting:**
   - Color-coded status (✓/✗)
   - Summary statistics
   - Service-by-service breakdown

### Customize Health Checks

Edit the service definitions in the script:

```bash
nano /opt/axionax-deploy/scripts/check-vps-status.sh
```

```bash
# Add new service check
declare -A services=(
    ["your-service"]="Service Name|PORT|health_check_command"
)
```

---

## Integration with Monitoring

### Prometheus Alerts

Health check data can trigger Prometheus alerts:

```yaml
# Alert when service is down
- alert: ServiceDown
  expr: up{job="axionax-rpc"} == 0
  for: 1m
  labels:
    severity: critical
  annotations:
    summary: "Service {{ $labels.job }} is down"
```

### Grafana Dashboard

Create dashboard panel showing service status:

```
Query: up{job=~"axionax.*"}
Panel Type: Stat
Thresholds: 
  - Red: 0
  - Green: 1
```

---

## Best Practices

### 1. Regular Health Checks

- Run every 5 minutes via cron
- Monitor trends, not just current status
- Set up alerts for repeated failures

### 2. Graceful Degradation

- Identify critical vs non-critical services
- Plan for partial outages
- Document recovery procedures

### 3. Health Check Timeout Values

```yaml
# Balance between:
# - Fast failure detection (short timeout)
# - Avoiding false positives (longer timeout)

healthcheck:
  interval: 30s   # How often to check
  timeout: 10s    # Max time to wait for response
  retries: 3      # Failures before marking unhealthy
```

### 4. Log Health Check Results

```bash
# Daily health check summary
0 0 * * * /opt/axionax-deploy/scripts/check-vps-status.sh > /var/log/axionax/health-$(date +\%Y\%m\%d).log
```

---

## Health Check API (Future)

**Planned features:**

- Unified `/health` endpoint for all services
- Detailed health status (database, dependencies, resources)
- Health check history API
- Automated recovery actions

---

## Related Documentation

- [Monitoring Guide](MONITORING.md)
- [VPS Operations](VPS_OPERATIONS.md)
- [Troubleshooting](TROUBLESHOOTING.md)
- [Deployment Guide](https://github.com/axionaxprotocol/axionax-deploy)

---

## Support

- **GitHub Issues**: [axionax-deploy/issues](https://github.com/axionaxprotocol/axionax-deploy/issues)
- **Documentation**: [docs.axionax.org](https://docs.axionax.org)

---

**Current Health Status (Nov 12, 2025):**
- ✅ All 9 Services: Healthy
- ✅ Health Check Script: Operational
- ✅ Automated Monitoring: Active
- ✅ System Resources: Normal (RAM 12%, Disk 17%)

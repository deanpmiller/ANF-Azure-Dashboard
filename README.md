# Azure NetApp Files Dashboard

A comprehensive Azure Portal dashboard for monitoring Azure NetApp Files (ANF) metrics and performance.

## 📊 Overview

This repository contains an Azure Portal dashboard template specifically designed to monitor Azure NetApp Files resources. The dashboard provides real-time insights into:

- **Throughput Metrics** - Total, Read, and Write throughput
- **IOPS Monitoring** - Total IOPS (note: multiply by 300 for actual ops/sec with 5-minute rollup)
- **Latency Analysis** - Average read and write latencies
- **Capacity Tracking** - Volume consumed size percentage
- **Performance Limits** - Throughput limit alerts and QoS latency delta

## ⚠️ Key Metrics Information

### IOPS Calculation
The dashboard displays **Average Total IOPS** which represents a 5-minute rollup window. To calculate the actual operations per second:

```
Actual IOPS = Dashboard IOPS Value × 300
```

### Resource Scoping
The dashboard is scoped at the **resource provider level** (`microsoft.netapp/netappaccounts/capacitypools/volumes`), which means:
- ✅ New volumes are automatically included
- ✅ Removed volumes are automatically excluded
- ✅ No manual tile edits required when infrastructure changes

## 🚀 Getting Started

### Prerequisites
- Azure subscription with ANF resources deployed
- Azure PowerShell modules installed
- Access to at least one ANF volume

### Setup Instructions

#### 1. **Edit the Dashboard JSON**

Open `ANF-Dashboard.json` in Visual Studio Code and search & replace:

- `INSERT_SUB_ID_HERE` → Your Azure Subscription ID
- `INSERT_REGION_HERE` → Your Azure region (e.g., `eastus`, `westeurope`)
- `INSERT_LOCATION` → Your location for the dashboard (e.g., `eastus`)

**Example using VS Code Find & Replace:**
```
Find:    INSERT_SUB_ID_HERE
Replace: 12345678-1234-1234-1234-123456789012

Find:    INSERT_REGION_HERE
Replace: westeurope

Find:    INSERT_LOCATION
Replace: westeurope
```

#### 2. **Import the Dashboard via Azure PowerShell**

```powershell
# Connect to Azure
Connect-AzAccount

# Set your subscription
Set-AzContext -SubscriptionId "YOUR_SUBSCRIPTION_ID"

# Import the dashboard
New-AzPortalDashboard -DashboardPath "ANF-Dashboard.json" -ResourceGroupName "YOUR_RESOURCE_GROUP"
```

**Alternative method** - Import via Azure CLI:

```bash
az portal dashboard create \
  --resource-group YOUR_RESOURCE_GROUP \
  --name "ANF Metrics & Monitoring" \
  --input-path ANF-Dashboard.json
```

#### 3. **Access Your Dashboard**

1. Navigate to [Azure Portal](https://portal.azure.com)
2. Select **Dashboard** from the left sidebar
3. Find **"ANF Metrics & Monitoring"** in your dashboards list
4. Pin frequently used dashboards for quick access

## 📈 Dashboard Tiles

### Performance Overview
| Metric | Description | Unit |
|--------|-------------|------|
| **Total Throughput** | Aggregate throughput across all volumes | MiB/s |
| **Read Throughput** | Read operations throughput | MiB/s |
| **Write Throughput** | Write operations throughput | MiB/s |
| **Total IOPS** | Operations per second (×300 for actual value) | IOPS |

### Capacity & Limits
| Metric | Description | Unit |
|--------|-------------|------|
| **Volume Consumed %** | Percentage of provisioned capacity in use | % |
| **Throughput Limit Reached** | Events when throughput limits exceeded | Count |
| **QoS Latency Delta** | Quality of Service latency variance | ms |

### Latency Metrics
| Metric | Description | Unit |
|--------|-------------|------|
| **Average Read Latency** | Mean latency for read operations | ms |
| **Average Write Latency** | Mean latency for write operations | ms |

## 🔧 Customization

### Modify Time Range
Each tile can be configured with different time windows:
- 1 hour
- 24 hours (default)
- 7 days
- 30 days
- Custom range

Click on any tile's time selector to adjust the rolling window.

### Add Additional Metrics

To add more metrics to the dashboard:

1. Edit `ANF-Dashboard.json`
2. Add new metric objects under the `metrics` array
3. Reference available [ANF metrics](https://learn.microsoft.com/en-us/azure/azure-netapp-files/metrics)
4. Re-import the dashboard

## 📚 References

- [Azure NetApp Files Metrics](https://learn.microsoft.com/en-us/azure/azure-netapp-files/metrics)
- [ANF Performance Considerations](https://learn.microsoft.com/en-us/azure/azure-netapp-files/performance-benchmarks)
- [Azure Portal Dashboards Documentation](https://learn.microsoft.com/en-us/azure/azure-portal/azure-portal-dashboards)
- [ANF Cache Volumes](https://learn.microsoft.com/en-us/azure/azure-netapp-files/cache-volumes)

## 📸 Screenshots

Screenshots of the dashboard in action:
[Azure Dashboard](./screenshots/Screenshot 2026-06-15 114150.jpg)

- Dashboard overview
- Specific metric trends
- Performance scenarios


## ⚙️ Troubleshooting

### Dashboard shows "No Data"
- Verify ANF volumes are deployed and generating traffic
- Confirm subscription ID and region are correctly set in the JSON
- Check that your user account has appropriate RBAC permissions

### Import fails with error
- Ensure the JSON is valid (use JSON validators if needed)
- Verify all placeholder values have been replaced
- Check that the resource group exists

### Metrics not updating
- Dashboard metrics update on a 5-minute cycle
- Wait 5+ minutes after volume deployment for data to appear
- Check Azure resource health status

### Need Help?

If you encounter issues or would like detailed walkthrough:
- 💬 Open an issue in this repository
- 📞 Schedule a call for live demonstration of the import process
- 📧 Contact for additional guidance

## 📄 Files in This Repository

| File | Purpose |
|------|---------|
| `README.md` | This documentation file |
| `ANF-Dashboard.json` | Azure Portal dashboard template (requires customization) |
| `screenshots/` | Dashboard screenshots and usage examples |

## ⚠️ Disclaimer

THIS DASHBOARD TEMPLATE IS PROVIDED AS-IS WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE, AND NONINFRINGEMENT.

## 📝 License

This dashboard template is provided as-is for use with Azure NetApp Files monitoring.

---

**Happy monitoring! 📊**

For questions or contributions, feel free to open an issue or contact the repository maintainer.

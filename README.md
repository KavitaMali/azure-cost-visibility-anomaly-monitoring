# Azure Cost Visibility & Anomaly Monitoring

## Project Overview

Azure Cost Visibility & Anomaly Monitoring is a cloud-based FinOps solution designed to monitor Azure subscription spending, visualize cost trends, configure budget alerts, and identify unexpected increases in service-level costs.

The solution uses Azure Cost Management APIs and Azure Monitor Workbooks to provide cost insights without requiring a Data Collection Endpoint (DCE), Data Collection Rule (DCR), or custom Log Analytics tables.

It also provides automated email notifications through Azure Monitor Action Groups and Cost Management budget alerts. An optional Azure Logic App performs daily cost anomaly checks.

## Objectives

- Monitor Azure subscription spending.
- Visualize daily Azure cost trends.
- Analyze spending by resource group and Azure service.
- Configure monthly budgets and spending thresholds.
- Receive automated email notifications when budget thresholds are crossed.
- Identify unexpected day-over-day cost increases for individual services.
- Improve cloud cost visibility and cost optimization.

## Azure Services Used

| Azure Service | Purpose |
|---|---|
| Azure Cost Management | Retrieve subscription cost and usage information |
| Azure Monitor Workbooks | Build interactive cost monitoring dashboards |
| Azure Monitor Action Groups | Configure email notifications |
| Azure Cost Management Budgets | Monitor actual and forecasted spending |
| Azure Logic Apps | Automate daily cost anomaly detection |
| Managed Identity | Authenticate Logic App requests securely |
| Azure Resource Manager API | Retrieve cost information programmatically |

## Solution Architecture

### Main Components

1. **Azure Cost Management API**
   - Retrieves actual cost information.
   - Groups spending by service and resource group.
   - Provides daily cost information.

2. **Azure Monitor Workbook**
   - Displays daily cost trends.
   - Provides service-wise cost breakdown.
   - Provides resource-group-wise cost analysis.
   - Supports subscription selection and configurable time ranges.

3. **Azure Cost Management Budget**
   - Configures monthly spending limits.
   - Monitors 50%, 80%, and 100% actual spending thresholds.
   - Supports 100% forecasted spending alerts.

4. **Azure Monitor Action Group**
   - Sends email notifications when configured alert conditions are met.
   - Reuses the same Action Group for budget alerts and optional anomaly notifications.

5. **Azure Logic App – Cost Anomaly Detection**
   - Runs on a daily recurrence schedule.
   - Retrieves the previous two days of cost information.
   - Groups cost information by Azure service.
   - Calculates percentage changes in spending.
   - Sends an email when the configured anomaly threshold is exceeded.

## Architecture Flow

### Cost Monitoring

Azure Cost Management API → Azure Monitor Workbook → Cost Visualization

### Budget Monitoring

Azure Cost Management Budget → Azure Monitor Action Group → Email Notification

### Automated Anomaly Detection (Optional)

Daily Recurrence Trigger → Azure Logic App → Cost Management Query API → Data Transformation → Service-wise Cost Comparison → Threshold Condition → Email Notification

## Key Features

- Subscription-level cost monitoring.
- Daily spending visualization.
- Service-level and resource-group-level cost analysis.
- Monthly budget configuration.
- Actual and forecasted budget alerts.
- Automated email notifications.
- Daily service-level anomaly detection.
- Managed Identity authentication.
- No custom Log Analytics table or DCE/DCR dependency.

## Implementation Details

### 1. Resource Group

Created a dedicated resource group:

`rg-costviz-monitoring`

All monitoring resources are organized under this resource group.

### 2. Action Group

Created an Azure Monitor Action Group:

`costviz-ag-email`

Configured an email receiver for budget and monitoring notifications.

### 3. Budget Configuration

Created a monthly Azure Cost Management budget with configurable spending thresholds:

- 50% actual cost.
- 80% actual cost.
- 100% actual cost.
- 100% forecasted cost.

### 4. Azure Monitor Workbook

Created a custom workbook named:

`Azure Cost Visibility`

Configured Azure Resource Manager as the data source and integrated the Cost Management Query API.

The workbook provides:
- Daily cost trend visualization.
- Service-wise spending breakdown.
- Resource-group-wise cost breakdown.
- Tabular cost information.

### 5. Automated Cost Anomaly Detection

Implemented an optional Azure Logic App with a daily recurrence trigger.

The workflow:
- Retrieves cost information for the previous two days.
- Extracts service names and daily costs.
- Compares the spending of each service.
- Calculates percentage changes.
- Sends an email notification when spending increases beyond the configured threshold.

Managed Identity is used for authentication, with the Cost Management Reader role assigned to the Logic App identity.

## Testing and Validation

The solution can be validated through the following scenarios:

1. Verify that the workbook retrieves subscription cost information.
2. Compare workbook results with Azure Cost Analysis.
3. Validate daily, service-wise, and resource-group-wise visualizations.
4. Test budget alerts by temporarily lowering the budget threshold.
5. Manually trigger the Logic App.
6. Validate service-wise cost comparison and anomaly detection.
7. Verify that email notifications are received.

## Important Considerations

- Azure Cost Management data can have an 8–24-hour delay.
- Budget alerts are not instantaneous and may run approximately daily.
- Users viewing the workbook require appropriate Cost Management permissions.
- The Logic App requires Cost Management Reader access through its Managed Identity.
- The anomaly detection workflow is optional.
- The solution currently targets a single subscription.

## Future Enhancements

- Multi-subscription cost monitoring.
- Advanced cost anomaly detection.
- Historical cost trend analysis.
- Automated cost optimization recommendations.
- Additional notification channels.
- Infrastructure deployment using ARM/Bicep templates.

## Author

**Kavita Patil**

Azure Integration Developer | Azure Cloud & Integration Services


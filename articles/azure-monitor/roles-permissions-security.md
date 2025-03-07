---
title: Roles, permissions, and security in Azure Monitor
description: Learn how to use roles and permissions in Azure Monitor to restrict access to monitoring resources.
services: azure-monitor
ms.topic: conceptual
ms.date: 11/27/2017 
ms.custom: devx-track-azurepowershell
---
# Roles, permissions, and security in Azure Monitor

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Many teams need to strictly regulate access to monitoring data and settings. For example, if you have team members who work exclusively on monitoring (support engineers, DevOps engineers) or if you use a managed service provider, you might want to grant them access to only monitoring data. You might want to restrict their ability to create, modify, or delete resources.

This article shows how to quickly apply a built-in monitoring role to a user in Azure or build your own custom role for a user who needs limited monitoring permissions. The article then discusses security considerations for your Azure Monitor-related resources and how you can limit access to the data in those resources.

## Built-in monitoring roles

Built-in roles in Azure Monitor help limit access to resources in a subscription while still enabling staff who monitor infrastructure to obtain and configure the data they need. Azure Monitor provides two out-of-the-box roles: Monitoring Reader and Monitoring Contributor.

### Monitoring Reader

People assigned the Monitoring Reader role can view all monitoring data in a subscription but can't modify any resource or edit any settings related to monitoring resources. This role is appropriate for users in an organization, such as support or operations engineers, who need to:

* View monitoring dashboards in the Azure portal.
* View alert rules defined in [Azure alerts](alerts/alerts-overview.md).
* Query for metrics by using the [Azure Monitor REST API](/rest/api/monitor/metrics), [PowerShell cmdlets](powershell-samples.md), or [cross-platform CLI](cli-samples.md).
* Query the Activity log by using the portal, Azure Monitor REST API, PowerShell cmdlets, or cross-platform CLI.
* View the [diagnostic settings](essentials/diagnostic-settings.md) for a resource.
* View the [log profile](essentials/activity-log.md#legacy-collection-methods) for a subscription.
* View autoscale settings.
* View alert activity and settings.
* Access Application Insights data and view data in Application Insights Analytics.
* Search Log Analytics workspace data, including usage data for the workspace.
* View management groups in Log Analytics.
* Retrieve the search schema in a Log Analytics workspace.
* List monitoring packs in a Log Analytics workspace.
* Retrieve and execute saved searches in a Log Analytics workspace.
* Retrieve the workspace storage configuration for Log Analytics.

> [!NOTE]
> This role doesn't give read access to log data that has been streamed to an event hub or stored in a storage account. For information on how to configure access to these resources, see the [Security considerations for monitoring data](#security-considerations-for-monitoring-data) section later in this article.

### Monitoring Contributor

People assigned the Monitoring Contributor role can view all monitoring data in a subscription. They can also create or modify monitoring settings, but they can't modify any other resources.

This role is a superset of the Monitoring Reader role. It's appropriate for members of an organization's monitoring team or managed service providers who, in addition to the permissions mentioned earlier, need to:

* View monitoring dashboards in the portal and create their own private monitoring dashboards.
* Set [diagnostic settings](essentials/diagnostic-settings.md) for a resource.\*
* Set the [log profile](essentials/activity-log.md#legacy-collection-methods) for a subscription.\*
* Set alert rule activity and settings via [Azure alerts](alerts/alerts-overview.md).
* Create web tests and components for Application Insights.
* List shared keys for a Log Analytics workspace.
* Enable or disable monitoring packs in a Log Analytics workspace.
* Create, delete, and execute saved searches in a Log Analytics workspace.
* Create and delete the workspace storage configuration for Log Analytics.

\*To set a log profile or a diagnostic setting, users must also separately be granted ListKeys permission on the target resource (storage account or event hub namespace).

> [!NOTE]
> This role doesn't give read access to log data that has been streamed to an event hub or stored in a storage account. For information on how to configure access to these resources, see the [Security considerations for monitoring data](#security-considerations-for-monitoring-data) section later in this article.

## Monitor permissions and Azure custom roles

If the preceding built-in roles don't meet the exact needs of your team, you can [create an Azure custom role](../role-based-access-control/custom-roles.md) with more granular permissions. The common Azure role-based access control (RBAC) operations for Azure Monitor are listed here.

| Operation | Description |
| --- | --- |
| Microsoft.Insights/ActionGroups/[Read, Write, Delete] |Read, write, or delete action groups. |
| Microsoft.Insights/ActivityLogAlerts/[Read, Write, Delete] |Read, write, or delete Activity log alerts. |
| Microsoft.Insights/AlertRules/[Read, Write, Delete] |Read, write, or delete alert rules (from classic alerts). |
| Microsoft.Insights/AlertRules/Incidents/Read |List incidents (history of the alert rule being triggered) for alert rules. This applies only to the portal. |
| Microsoft.Insights/AutoscaleSettings/[Read, Write, Delete] |Read, write, or delete autoscale settings. |
| Microsoft.Insights/DiagnosticSettings/[Read, Write, Delete] |Read, write, or delete diagnostic settings. |
| Microsoft.Insights/EventCategories/Read |Enumerate all categories possible in the Activity log. Used by the Azure portal. |
| Microsoft.Insights/eventtypes/digestevents/Read |This permission is necessary for users who need access to the Activity log via the portal. |
| Microsoft.Insights/eventtypes/values/Read |List Activity log events (management events) in a subscription. This permission applies to both programmatic and portal access to the Activity log. |
| Microsoft.Insights/ExtendedDiagnosticSettings/[Read, Write, Delete] | Read, write, or delete diagnostic settings for network flow logs. |
| Microsoft.Insights/LogDefinitions/Read |This permission is necessary for users who need access to the Activity log via the portal. |
| Microsoft.Insights/LogProfiles/[Read, Write, Delete] |Read, write, or delete log profiles (streaming the Activity log to an event hub or storage account). |
| Microsoft.Insights/MetricAlerts/[Read, Write, Delete] |Read, write, or delete near-real-time metric alerts. |
| Microsoft.Insights/MetricDefinitions/Read |Read metric definitions (list of available metric types for a resource). |
| Microsoft.Insights/Metrics/Read |Read metrics for a resource. |
| Microsoft.Insights/Register/Action |Register the Azure Monitor resource provider. |
| Microsoft.Insights/ScheduledQueryRules/[Read, Write, Delete] |Read, write, or delete log alerts in Azure Monitor. |

> [!NOTE]
> Access to alerts, diagnostic settings, and metrics for a resource requires that the user has read access to the resource type and scope of that resource. Creating (writing) a diagnostic setting or a log profile that archives to a storage account or streams to event hubs requires the user to also have ListKeys permission on the target resource.

For example, you can use the preceding table to create an Azure custom role for an Activity Log Reader like this:

```powershell
$role = Get-AzRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Activity Log Reader"
$role.Description = "Can view activity logs."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Insights/eventtypes/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription")
New-AzRoleDefinition -Role $role 
```

## Security considerations for monitoring data

Monitoring data—particularly log files—can contain sensitive information, such as IP addresses or user names. Monitoring data from Azure comes in three basic forms:

- The Activity log describes all control-plane actions on your Azure subscription.
- Resource logs are logs emitted by a resource.
- Metrics are emitted by resources.

All these data types can be stored in a storage account or streamed to an event hub, both of which are general-purpose Azure resources. Because these are general-purpose resources, creating, deleting, and accessing them is a privileged operation reserved for an administrator. Use the following practices for monitoring-related resources to prevent misuse:

* Use a single, dedicated storage account for monitoring data. If you need to separate monitoring data into multiple storage accounts, never share usage of a storage account between monitoring and non-monitoring data. Sharing usage in that way might inadvertently give access to non-monitoring data to organizations that need access to only monitoring data. For example, a third-party organization for security information and event management should need only access to monitoring data.
* Use a single, dedicated service bus or event hub namespace across all diagnostic settings for the same reason described in the previous point.
* Limit access to monitoring-related storage accounts or event hubs by keeping them in a separate resource group. [Use scope](../role-based-access-control/overview.md#scope) on your monitoring roles to limit access to only that resource group.
* Never grant the ListKeys permission for either storage accounts or event hubs at subscription scope when a user needs only access to monitoring data. Instead, give these permissions to the user at a resource or resource group scope (if you have a dedicated monitoring resource group).

### Limit access to monitoring-related storage accounts

When a user or application needs access to monitoring data in a storage account, [generate a shared access signature (SAS)](/rest/api/storageservices/create-account-sas) on the storage account that contains monitoring data with service-level read-only access to blob storage. In PowerShell, the account SAS might look like the following code:

```powershell
$context = New-AzStorageContext -ConnectionString "[connection string for your monitoring Storage Account]"
$token = New-AzStorageAccountSASToken -ResourceType Service -Service Blob -Permission "rl" -Context $context
```

You can then give the token to the entity that needs to read from that storage account. The entity can list and read from all blobs in that storage account.

Alternatively, if you need to control this permission with Azure RBAC, you can grant that entity the `Microsoft.Storage/storageAccounts/listkeys/action` permission on that particular storage account. This permission is necessary for users who need to set a diagnostic setting or a log profile to archive to a storage account. For example, you can create the following Azure custom role for a user or application that needs to read from only one storage account:

```powershell
$role = Get-AzRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Monitoring Storage Account Reader"
$role.Description = "Can get the storage account keys for a monitoring storage account."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/storageAccounts/listkeys/action")
$role.Actions.Add("Microsoft.Storage/storageAccounts/Read")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/myMonitoringStorageAccount")
New-AzRoleDefinition -Role $role 
```

> [!WARNING]
> The ListKeys permission enables the user to list the primary and secondary storage account keys. These keys grant the user all signed permissions (such as read, write, create blobs, and delete blobs) across all signed services (blob, queue, table, file) in that storage account. We recommend using an account SAS when possible.

### Limit access to monitoring-related event hubs

You can follow a similar pattern with event hubs, but first you need to create a dedicated authorization rule for listening. If you want to grant access to an application that only needs to listen to monitoring-related event hubs, follow these steps:

1. In the portal, create a shared access policy on the event hubs that were created for streaming monitoring data with only listening claims. For example, you might call it "monitoringReadOnly." If possible, give that key directly to the consumer and skip the next step.
1. If the consumer needs to get the key ad hoc, grant the user the ListKeys action for that event hub. This step is also necessary for users who need to set a diagnostic setting or a log profile to stream to event hubs. For example, you might create an Azure RBAC rule:
   
   ```powershell
   $role = Get-AzRoleDefinition "Reader"
   $role.Id = $null
   $role.Name = "Monitoring Event Hub Listener"
   $role.Description = "Can get the key to listen to an event hub streaming monitoring data."
   $role.Actions.Clear()
   $role.Actions.Add("Microsoft.EventHub/namespaces/authorizationrules/listkeys/action")
   $role.Actions.Add("Microsoft.EventHub/namespaces/Read")
   $role.AssignableScopes.Clear()
   $role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.ServiceBus/namespaces/mySBNameSpace")
   New-AzRoleDefinition -Role $role 
   ```

## Monitoring within a secured virtual network

Azure Monitor needs access to your Azure resources to provide the services that you enable. If you want to monitor your Azure resources while still securing them from access to the public internet, you can use secured storage accounts.

Monitoring data is often written to a storage account. You might want to make sure that unauthorized users can't access the data that's copied to a storage account. For extra security, you can lock down network access to give only your authorized resources and trusted Microsoft services access to a storage account by restricting a storage account to use selected networks.

![Screenshot that shows the settings for firewalls and virtual networks.](./media/roles-permissions-security/secured-storage-example.png)

Azure Monitor is considered a trusted Microsoft service. If you select the **Allow trusted Microsoft services to access this storage account** checkbox, Azure monitor will have access to your secured storage account. You then enable writing Azure Monitor resource logs, Activity log, and metrics to your storage account under these protected conditions. This setting will also enable Log Analytics to read logs from secured storage.

For more information, see [Configure Azure Storage firewalls and virtual networks](../storage/common/storage-network-security.md).

## Next steps

* [Read about Azure RBAC and permissions in Azure Resource Manager](../role-based-access-control/overview.md)
* [Read the overview of monitoring in Azure](overview.md)

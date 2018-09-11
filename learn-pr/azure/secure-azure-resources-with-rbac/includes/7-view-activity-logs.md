First Up Consultants reviews role-based access control (RBAC) changes quarterly for auditing and troubleshooting purposes. You know that changes get logged in [Azure Activity Log](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs). Your manager has asked if you can generate a report of the role assignment and custom role changes for the last month.

## View activity logs

The easiest way to get started is to view the activity logs with the Azure portal.

1. Click **All services** and then find **Activity log**.

    ![Activity logs using the portal](../media-draft/7-all-services-activity-log.png)

1. Click **Activity log**.

    ![Activity logs using the portal](../media-draft/7-activity-log-portal.png)

1. Set the **Timespan** filter to **Last month**.

1. Set the **Event category** filter to **Administrative**.

1. In the **Operation** filter, type **role** to filter the list.

1. Select the following RBAC operations:

    - Create role assignment (roleAssignments)
    - Delete role assignment (roleAssignments)
    - Create or update custom role definition (roleDefinitions)
    - Delete custom role definition (roleDefinitions)

    ![Operation filter](../media-draft/7-operation-filter.png)

1. Click **Apply** to apply your filters.

    You'll see all the role assignment and role definition operations for the last month. It also includes a link to download the activity log as a CSV file.

    ![RBAC activity logs](../media-draft/7-activity-log-portal-filter.png)

## End lab

1. To end the lab, click the hamburger menu in the upper-right corner of this window and then click **End**.

1. Click **Yes, end my lab**.

## Summary

In this unit, you learned how to use Azure Activity Log to list RBAC changes in the portal and generate a simple report.

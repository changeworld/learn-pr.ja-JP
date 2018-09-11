Using the **Access control (IAM)** blade in the Azure portal has been working fine, but you are getting several permission requests each day. To keep up with the access management tasks, you decide to use PowerShell to help automate some of the steps.

## Open Cloud Shell PowerShell

1. Make sure you are still signed in to the Azure portal as **LabAdmin-_XXXXXXX_@_xxxxxxxxxxxx_.onmicrosoft.com**. You can find the username and password on the **Resources** tab at the top of this window.

1. At the top of the portal, click **Cloud Shell** to open the Cloud Shell pane.

    ![Cloud Shell button](../media-draft/6-cloud-shell-button.png)

1. In the upper left of the Cloud Shell pane, make sure it is set to **PowerShell**. If it is set to **Bash**, change it to **PowerShell**.

    It might take a few moments to load. When finished, it will look similar to the following:

    ![Cloud Shell PowerShell](../media-draft/6-cloud-shell-powershell.png)

## Grant access

To grant access to a user using Azure PowerShell, you use the [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) command. You must specify the security principal, role definition, and scope.

Follow these steps to assign the Virtual Machine Contributor role to the **LabUser-_XXXXXXX_** user at the resource group scope.

1. On the **Resources** tab at the top of this window, copy the **Grant access PowerShell** command.

1. Paste the command into the PowerShell pane and press the Enter key to run it. The following shows an example command and the output:

    ```Example
    PS Azure:\> New-AzureRmRoleAssignment -SignInName LabUser-XXXXXXX@xxxxxxxxxxxx.onmicrosoft.com `
      -RoleDefinitionName "Virtual Machine Contributor" `
      -ResourceGroupName "FirstUpConsultantsRG1-XXXXXXX"

    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/FirstUpConsultantsRG1-XXXXXXX/providers/Microsoft.Authorization/roleAssignments/33333333-3333-3333-3333-333333333333
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/FirstUpConsultantsRG1-XXXXXXX
    DisplayName        : LabUser-XXXXXXX
    SignInName         : LabUser-XXXXXXX@xxxxxxxxxxxx.onmicrosoft.com
    RoleDefinitionName : Virtual Machine Contributor
    RoleDefinitionId   : 9980e02c-c2be-4d73-94e8-173b1dc7cf3c
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : User
    CanDelegate        : False
    ```

    The output shows that the Virtual Machine Contributor role has been assigned to LabUser-_XXXXXXX_ at the FirstUpConsultantsRG1-_XXXXXXX_ scope.

## List access

To verify the access for the resource group, you use the [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment) command to list the role assignments.

Follow these steps to list all the role assignments for the **LabUser-XXXXXXX** user at the resource group scope.

1. On the **Resources** tab at the top of this window, copy the **List access PowerShell** command.

1. Paste the command into the PowerShell pane and press the Enter key to run it. The following shows an example command and the output.

    ```Example
    PS Azure:\> Get-AzureRmRoleAssignment -SignInName LabUser-XXXXXXX@xxxxxxxxxxxx.onmicrosoft.com `
      -ResourceGroupName "FirstUpConsultantsRG1-XXXXXXX"

    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/FirstUpConsultantsRG1-XXXXXXX/providers/Microsoft.Authorization/roleAssignments/33333333-3333-3333-3333-333333333333
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/FirstUpConsultantsRG1-XXXXXXX
    DisplayName        : LabUser-XXXXXXX
    SignInName         : LabUser-XXXXXXX@xxxxxxxxxxxx.onmicrosoft.com 
    RoleDefinitionName : Virtual Machine Contributor
    RoleDefinitionId   : 9980e02c-c2be-4d73-94e8-173b1dc7cf3c
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : User
    CanDelegate        : False
    ```

    The output shows that the Virtual Machine Contributor role has been assigned to LabUser-_XXXXXXX_ at the FirstUpConsultantsRG1-_XXXXXXX_ scope.

    If you refresh the **Access control (IAM)** blade for the resource group in the Azure portal, this is how the role assignment looks:

    ![Role assignments for a user at resource group scope](../media-draft/6-cloud-shell-access-control.png)

## Remove access

To remove access for users, groups, and applications, you use [Remove-AzureRmRoleAssignment](/powershell/module/azurerm.resources/remove-azurermroleassignment) to remove a role assignment.

Follow these steps to remove the Virtual Machine Contributor role assignment for the **LabUser-_XXXXXX_** user at the resource group scope.

1. On the **Resources** tab at the top of this window, copy the **Remove access PowerShell** command.

1. Paste the command into the PowerShell pane and press the Enter key to run it. The following shows an example command.

    ```Example
    PS Azure:\> Remove-AzureRmRoleAssignment -SignInName LabUser-XXXXXXX@xxxxxxxxxxxx.onmicrosoft.com `
      -RoleDefinitionName "Virtual Machine Contributor" `
      -ResourceGroupName "FirstUpConsultantsRG1-XXXXXXX"
    ```

1. In the PowerShell pane, click the close (**X**) button to close the pane.

    ![Cloud Shell close button](../media-draft/6-cloud-shell-close.png)


## Summary

In this unit, you learned how to grant a user access to create and manage virtual machines in a resource group using Azure PowerShell. In the next unit, you learn how to view the RBAC changes over time.

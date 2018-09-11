At First Up Consultants, you've been granted access to the Azure subscription for the marketing team. You want to familiarize yourself with the Azure portal and see what roles are currently assigned.

## List role assignments for yourself

Follow these steps to see what roles are currently assigned to you.

1. In the navigation list, click **Azure Active Directory**.

1. Click **Users** to open **All users**.

    ![Azure Active Directory users](../media-draft/4-aad-all-users.png)

1. Find and click the **LabAdmin-_XXXXXXX_** user name.

    ![Azure Active Directory lab users](../media-draft/4-aad-all-users-lab.png)

1. In the **Manage** section, click **Azure resources**.

    ![Azure resources](../media-draft/4-aad-user-azure-resources.png)

    On the Azure resources blade, you can see the resources and roles you have access to. Your list will look different.

## List role assignments for a resource group

Follow these steps to see what roles are assigned at the resource group scope.

1. In the navigation list, click **Resource groups**.

   ![Resource groups](../media-draft/4-resource-groups.png)

1. Click the resource group named **FirstUpConsultantsRG1-_XXXXXXX_**.

1. Click **Access control (IAM)**.

   On the Access control (IAM) blade, you can see who has access to this resource group. Notice that some roles are scoped to **This resource** while others are **(Inherited)** from a parent scope.

   ![Access control (IAM) for resource group](../media-draft/4-resource-group-access-control.png)

## List roles

As you learned in the previous unit, a role is a collection of permissions. Azure has over 70 built-in roles that you can use in your role assignments. Follow these steps to list the roles.

- At the top of the Access control (IAM) blade, click **Roles** to see a list of all the built-in and custom roles.

   ![Roles option](../media-draft/4-roles-option.png)

   You can see the number of users and groups that are assigned to each role.

   ![Roles list](../media-draft/4-roles-list.png)

## Summary

In this unit, you learned how to list the role assignments for yourself in the Azure portal. You also learned how to list the role assignments at different scopes. In the next unit, you take the next step and use RBAC to grant access to a user.

With a free Azure account, you can build, test, and deploy enterprise applications, create custom web and mobile experiences, and gain insights from your data through machine learning and powerful analytics.

## What is an Azure account?

An _Azure account_ is tied to a specific identity and holds information like:

- Name, email, and contact preferences
- Billing information such as a credit card

An Azure account is associated with one more  _subscriptions_.

## What is an Azure subscription?

An _Azure subscription_ is a logical container used to provision resources in Microsoft Azure. It holds the details of all your resources like virtual machines, databases, etc. It also has a trust relationship to a single Azure AD _tenant_ which is used to authenticate users and roles for the resources held in the subscription.

Billing occurs at the subscription level. You can set spending limits on each subscription to ensure you aren't surprised at the end of the month. 

## What is an Azure AD tenant?

Azure AD (Azure Active Directory) is a modern identity provider that supports multiple authentication protocols to secure applications and services in the cloud. It's _not_ the same as Windows Active Directory, which is focused on securing Windows desktops and servers. Instead, Azure AD is all about web-based authentication standards such as OpenID and OAuth.

A single tenant represents a logical organization and allows multiple identities to access and utilize resources protected by that tenant. An Azure subscription always has a trust relationship with a _single_ Azure AD tenant, but _multiple_ subscriptions can share a single tenant. This structure allows the organization to manage multiple subscriptions and set security rules across all the resources contained within them.

Here's a simple representation of accounts, subscriptions, tenants, and resources.

![Diagram of how accounts, tenants, subscriptions, and resources work together](../media/3-azure-ad-tenant.png)

Notice that each Azure AD tenant has an _account owner_. This is the original Azure account that is responsible for billing. You can add additional users to the tenant, and even invite guests from other Azure AD tenants to access resources in subscriptions.

## Azure account types

Azure has several account types that cater to different customer types. The most commonly used accounts are:

- Free
- Pay-As-You-Go
- Enterprise Agreement

### Azure free account

An Azure free account includes a **$200 credit** to spend for the first 30 days, free access to the most popular Azure products for 12 months, and access to more than 25 products that are always free. This is an excellent way for new users to get started. To set up a free account, you need a phone number, a credit card, and a Microsoft account.

> [!NOTE]
> Credit card information is used for identity verification only. You won’t be charged for any services until you upgrade.

### Azure Pay-As-You-Go account

A Pay-As-You-Go (PAYG) account bills you monthly for the services you used. This account type is appropriate for a wide range of users, from individuals to small businesses and many large organizations as well.

### Azure Enterprise Agreement

An Enterprise Agreement provides flexibility to buy cloud services and software licenses under one agreement, with discounts for new licenses and Software Assurance. It is targeted at enterprise-scale organizations.

## Summary

Whether you are an individual, a small business, or an enterprise, you need an account to use Azure services. The typical sequence is to start with a free account so that you can evaluate Azure services. When your trial period expires, you will convert from the free account to Pay-As-You-Go.
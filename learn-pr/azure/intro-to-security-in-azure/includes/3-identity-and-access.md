Network perimeters and their firewalls and physical access controls used to be the primary protection for corporate data. But network perimeters have become increasingly porous with the explosion of bring your own device (BYOD), mobile apps, and cloud applications. 

Identity has become the new primary security boundary. Proper authentication and assignment of privileges is critical to maintaining control of your data.

Contoso Shipping is addressing these concerns right away. Their new hybrid cloud solution needs to account for mobile apps that have access to secret data when an authorized user is signed in. They also have shipping vehicles sending a constant stream of telemetry data that is critical to optimizing their business.

## Single sign-on

The more identities a user has to manage, the greater the risk of a credential-related security incident. More identities mean more passwords to remember and change. Password policies can vary between applications and, as complexity requirements increase, it makes it more difficult for users to remember them.

On the other side is the management required for all those identities. Additional strain is placed on help desks as they deal with account lockouts and password reset requests. If a user leaves an organization, tracking down all those identities and ensuring they are disabled can be challenging. If an identity is overlooked, this could allow access when it should have been eliminated.

With single sign-on (SSO), users need to remember only one ID and one password. Access across applications is granted to a single identity tied to a user, simplifying the security model. As users change roles or leave an organization, access modifications are tied to the single identity, greatly reducing the effort needed to change or disable accounts. Using single sign-on for accounts will make it easier for users to manage their identities and will increase the security capabilities in your environment.

### SSO with Azure Active Directory

Azure Active Directory (Azure AD) is a cloud-based identity service. It has built-in support for synchronizing with your existing on-premises Active Directory instance, or it can be used stand-alone.

This means that all your applications, whether on-premises, in the cloud (including Office 365), or even mobile can share the same credentials. 

Administrators and developers can control access to data and applications using centralized rules and policies configured in Azure AD.

In addition, Microsoft is uniquely positioned to combine multiple data sources into an intelligent security graph that can provide threat analysis and real-time identity protection to all accounts in Azure Active Directory (even accounts that are synchronized from your on-premises Active Directory instance).

Contoso Shipping is integrating their existing Active Directory instance with Azure AD. This will make controlling access consistent across the organization. It will also make it a breeze for users to get into their email and Office 365 documents without having to reauthenticate.

## Multi-factor authentication

Multi-factor authentication (MFA) provides additional security for your identities by requiring two or more elements for full authentication. These elements fall into three categories:

- *something you know*
- *something you possess*
- *something you are*

**Something you know** would be a password or the answer to a security question. **Something you possess** could be a mobile app that receives a notification or a token-generating device. **Something you are** is typically some sort of biometric property, such as a fingerprint or face scan used on many mobile devices.

Using MFA increases security of your identity by limiting the impact of credential exposure. An attacker who has a user's password would also need to have possession of their phone or their face in order to fully authenticate. Authentication with only a single factor verified is insufficient, and the attacker would be unable to use those credentials to authenticate. The benefits this brings to security are huge, and it can't be repeated enough to enable MFA wherever possible.

Azure AD has MFA capabilities built in and will integrate with other third-party MFA providers. It's provided free of charge to any user who has the Global Administrators role in Azure AD, because these are highly sensitive accounts. All other accounts can have MFA enabled by purchasing licenses with this capability and assigning a license to the account.

For Contoso Shipping, you decide to enable MFA any time a user is signing in from a non-domain-connected computer. That includes the mobile apps.

## Providing identities to services

It's often valuable for services to have identities. Often, and against best practices, credential information is embedded in configuration files. With no security around these configuration files, anyone with access to the systems or repositories can access these credentials and risk exposure.

Azure AD addresses this problem through two methods: service principals and managed service identities.

### Service principals

To understand service principals, it's useful to first understand the words **identity** and **principal**, because they are used in the identity management world.

An **identity** is just a thing that can be authenticated. Obviously, this includes users with a user name and password, but it can also include applications or other servers, which might authenticate with secret keys or certificates. As a bonus definition, an **account** is data associated with an identity.

A **principal** is an identity acting with certain roles or claims. Often, it is not useful to consider identity and principal separately, but think of using `sudo` on a bash prompt or on Windows using "run as Administrator." In both those cases, you are still logged in as the same identity as before, but you've changed the role under which you are executing. Groups are often also considered principals because they can have rights assigned.

So, a **service principal** is literally named. It is an identity that is used by a service or application. Like other identities, it can be assigned roles. 

### Managed service identities

The creation of service principals can be a tedious process, and there are a lot of touch points that can make maintaining them difficult. Managed service identities are much easier and will do most of the work for you. 

A managed service identity can be instantly created for any Azure service that supports it (the list is constantly growing). When you create a managed service identity for a service, you are creating an account on the Azure Active Directory tenant. The Azure infrastructure will automatically take care of authenticating the service and managing the account. You can then use that account like any other Azure AD account, including securely letting the authenticated service access other Azure resources.

## Role-based access control

Roles are sets of permissions, like "Read-only" or "Contributor", that users can be granted to access an Azure service instance. 

Identities are mapped to roles directly or through group membership. Separating security principals, access permissions, and resources provides simple access management and fine-grained control. Administrators are able to ensure the minimum necessary permissions are granted.

Roles can be granted at the individual service instance level, but they also flow down the Azure Resource Manager hierarchy. Roles assigned at a higher scope, like an entire subscription, are inherited by child scopes, like service instances. 

<!--TODO: replace with final media which was submitted for Design-for-security-in-azure -->
![Management groups](../media-draft/3-role-assignment-scope.png)

### Privileged Identity Management

In addition to managing Azure resource access with role-based access control (RBAC), a comprehensive approach to infrastructure protection should consider including the ongoing auditing of role members as their organization changes and evolves. Azure AD Privileged Identity Management (PIM) is an additional, paid-for offering that provides oversight of role assignments, self-service, and just-in-time role activation and Azure AD and Azure resource access reviews.

<!--TODO: replace with final media which was submitted for Design-for-security-in-azure -->
![Privileged identity management](../media-COPIED-FROM-DESIGNFORSECURITY/PIM_Dashboard.png)

Identity allows us to maintain a security perimeter, even outside our physical control. With single sign-on and appropriate role-based access configuration, we can always be sure who has the ability to see and manipulate our data and infrastructure.
---
title: Enforce Azure Active Directory Multi-Factor Authentication for Azure Virtual Desktop using Conditional Access - Azure
description: How to enforce Azure Active Directory Multi-Factor Authentication for Azure Virtual Desktop using Conditional Access to help make it more secure.
author: Heidilohr
ms.topic: how-to
ms.date: 08/24/2022
ms.author: helohr
manager: femila
---
# Enforce Azure Active Directory Multi-Factor Authentication for Azure Virtual Desktop using Conditional Access

> [!IMPORTANT]
> If you're visiting this page from the Azure Virtual Desktop (classic) documentation, make sure to [return to the Azure Virtual Desktop (classic) documentation](./virtual-desktop-fall-2019/tenant-setup-azure-active-directory.md) once you're finished.

Users can sign into Azure Virtual Desktop from anywhere using different devices and clients. However, there are certain measures you should take to help keep yourself and your users safe. Using Azure Active Directory (Azure AD) Multi-Factor Authentication (MFA) with Azure Virtual Desktop prompts users during the sign-in process for another form of identification in addition to their username and password. You can enforce MFA for Azure Virtual Desktop using Conditional Access, and can also configure whether it applies to the web client, mobile apps, desktop clients, or all clients.

How often a user is prompted to reauthenticate depends on [Azure AD session lifetime configuration settings](../active-directory/authentication/concepts-azure-multi-factor-authentication-prompts-session-lifetime.md#azure-ad-session-lifetime-configuration-settings). For example, if their Windows client device is registered with Azure AD, it will receive a [Primary Refresh Token](../active-directory/devices/concept-primary-refresh-token.md) (PRT) to use for single sign-on (SSO) across applications. Once issued, a PRT is valid for 14 days and is continuously renewed as long as the user actively uses the device.

While remembering credentials is convenient, it can also make deployments for Enterprise scenarios using personal devices less secure. To protect your users, you can make sure the client keeps asking for Azure AD Multi-Factor Authentication credentials more frequently. You can use Conditional Access to configure this behavior.

Learn how to enforce MFA for Azure Virtual Desktop and optionally configure sign-in frequency below.

## Prerequisites

Here's what you'll need to get started:

- Assign users a license that includes [Azure Active Directory Premium P1 or P2](../active-directory/authentication/concept-mfa-licensing.md).
- An [Azure Active Directory group](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md) with your Azure Virtual Desktop users assigned as group members.
- Enable Azure AD Multi-Factor Authentication for your users. For more information about how to do that, see [Enable Azure AD Multi-Factor Authentication](../active-directory/authentication/tutorial-enable-azure-mfa.md).

## Create a Conditional Access policy

Here's how to create a Conditional Access policy that requires multi-factor authentication when connecting to Azure Virtual Desktop:

1. Sign in to the [Azure portal](https://portal.azure.com) as a global administrator, security administrator, or Conditional Access administrator.
1. In the search bar, type *Azure Active Directory* and select the matching service entry.
1. Browse to **Security** > **Conditional Access**.
1. Select **New policy** > **Create new policy**.
1. Give your policy a name. We recommend that organizations create a meaningful standard for the names of their policies.
1. Under **Assignments**, select **Users or workload entities**.
1. Under the **Include** tab, select **Select users and groups** and tick **Users and groups**. On the right, search for and choose the group that contains your Azure Virtual Desktop users as group members.
1. Select **Select**.
1. Under **Assignments**, select **Cloud apps or actions**.
1. Under the **Include** tab, select **Select apps**.
1. On the right, select one of the following apps based on which version of Azure Virtual Desktop you're using.
   
   - If you're using Azure Virtual Desktop (based on Azure Resource Manager), you can configure MFA on two different apps:
       
        -  **Azure Virtual Desktop** (app ID 9cdead84-a844-4324-93f2-b2e6bb768d07), which applies when the user subscribes to a feed and authenticates to the Azure Virtual Desktop Gateway during a connection.

        > [!TIP]
        > The app name was previously *Windows Virtual Desktop*. If you registered the *Microsoft.DesktopVirtualization* resource provider before the display name changed, the application will be named **Windows Virtual Desktop** with the same app ID as above.

        - **Microsoft Remote Desktop** (app ID a4a365df-50f1-4397-bc59-1a1564b8bb9c), which applies when the user authenticates to the session host when [single sign-on](configure-single-sign-on.md) is enabled.

   - If you're using Azure Virtual Desktop (classic), choose these apps:
       
       - **Windows Virtual Desktop** (app ID 5a0aa725-4958-4b0c-80a9-34562e23f3b7)
       - **Windows Virtual Desktop Client** (app ID fa4345a4-a730-4230-84a8-7d9651b86739), which will let you set policies on the web client
       
        > [!TIP]
        > If you're using Azure Virtual Desktop (classic) and if the Conditional Access policy blocks all access excluding Azure Virtual Desktop app IDs, you can fix this by also adding the **Azure Virtual Desktop** (app ID 9cdead84-a844-4324-93f2-b2e6bb768d07) to the policy. Not adding this app ID will block feed discovery of Azure Virtual Desktop (classic) resources.

   > [!IMPORTANT]
   > Don't select the app called Azure Virtual Desktop Azure Resource Manager Provider (app ID 50e95039-b200-4007-bc97-8d5790743a63). This app is only used for retrieving the user feed and shouldn't have multi-factor authentication.   

1. Once you've selected your app, select **Select**.

    > [!div class="mx-imgBorder"]
    > ![A screenshot of the Conditional Access Cloud apps or actions page. The Azure Virtual Desktop app is shown.](media/cloud-apps-enterprise.png)
    
1. Under **Assignments**, select **Conditions** > **Client apps**. On the right, for **Configure**, select **Yes**, and then select the client apps this policy will apply to:

    - Select both check boxes if you want to apply the policy to all clients.    
    - Select **Browser** if you want the policy to apply to the web client.
    - Select **Mobile apps and desktop clients** if you want to apply the policy to other clients.
    - Deselect values for legacy authentication clients.
   
    > [!div class="mx-imgBorder"]
    > ![A screenshot of the Conditional Access Client apps page. The user has selected the mobile apps and desktop clients, and browser check boxes.](media/conditional-access-client-apps.png)

1. Once you've selected the client apps this policy will apply to, select **Done**.
1. Under **Assignments**, select **Access controls** > **Grant**, select **Grant access**, **Require multi-factor authentication**, and then select **Select**.
1. At the bottom of the page, set **Enable policy** to **On** and select **Create**.

> [!NOTE]
> When you use the web client to sign in to Azure Virtual Desktop through your browser, the log will list the client app ID as a85cf173-4192-42f8-81fa-777a763e6e2c (Azure Virtual Desktop client). This is because the client app is internally linked to the server app ID where the conditional access policy was set.

> [!TIP]
> Some users may see a prompt titled *Stay signed in to all your apps* if the Windows device they're using is not already registered with Azure AD. If they deselect **Allow my organization to manage my device** and select **No, sign in to this app only**, this may reappear frequently.

## Configure sign-in frequency

To optionally configure the time period before a user is asked to sign-in again:

1. Open the policy you created previously.
1. Under **Assignments**, select **Access controls** > **Session**. On the right, select **Sign-in frequency**. Set the value for the time period before a user is asked to sign-in again, and then select **Select**. For example, setting the value to **1** and the unit to **Hours**, will require multi-factor authentication if a connection is launched over an hour after the last one.
1. At the bottom of the page, under **Enable policy** select **Save**.

## Azure AD joined session host VMs

For connections to succeed, you must [disable the legacy per-user multi-factor authentication sign-in method](../active-directory/devices/howto-vm-sign-in-azure-ad-windows.md#mfa-sign-in-method-required). If you don't want to restrict signing in to strong authentication methods like Windows Hello for Business, you'll also need to [exclude the Azure Windows VM Sign-In app](../active-directory/devices/howto-vm-sign-in-azure-ad-windows.md#enforce-conditional-access-policies) from your Conditional Access policy.

## Next steps

- [Learn more about Conditional Access policies](../active-directory/conditional-access/concept-conditional-access-policies.md)
- [Learn more about user sign in frequency](../active-directory/conditional-access/howto-conditional-access-session-lifetime.md#user-sign-in-frequency)

---
title: 'Tutorial: Azure AD SSO integration with Sketch'
description: Learn how to configure single sign-on between Azure Active Directory and Sketch.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 11/21/2022
ms.author: jeedes

---

# Tutorial: Azure AD SSO integration with Sketch

In this tutorial, you'll learn how to integrate Sketch with Azure Active Directory (Azure AD). When you integrate Sketch with Azure AD, you can:

* Control in Azure AD who has access to Sketch.
* Enable your users to be automatically signed-in to Sketch with their Azure AD accounts.
* Manage your accounts in one central location - the Azure portal.

## Prerequisites

To get started, you need the following items:

* An Azure AD subscription. If you don't have a subscription, you can get a [free account](https://azure.microsoft.com/free/).
* Sketch single sign-on (SSO) enabled subscription.
* Along with Cloud Application Administrator, Application Administrator can also add or manage applications in Azure AD.
For more information, see [Azure built-in roles](../roles/permissions-reference.md).

## Scenario description

In this tutorial, you configure and test Azure AD SSO in a test environment.

* Sketch supports **SP** initiated SSO.
* Sketch supports **Just In Time** user provisioning.

## Add Sketch from the gallery

To configure the integration of Sketch into Azure AD, you need to add Sketch from the gallery to your list of managed SaaS apps.

1. Sign in to the Azure portal using either a work or school account, or a personal Microsoft account.
1. On the left navigation pane, select the **Azure Active Directory** service.
1. Navigate to **Enterprise Applications** and then select **All Applications**.
1. To add new application, select **New application**.
1. In the **Add from the gallery** section, type **Sketch** in the search box.
1. Select **Sketch** from results panel and then add the app. Wait a few seconds while the app is added to your tenant.

 Alternatively, you can also use the [Enterprise App Configuration Wizard](https://portal.office.com/AdminPortal/home?Q=Docs#/azureadappintegration). In this wizard, you can add an application to your tenant, add users/groups to the app, assign roles, as well as walk through the SSO configuration as well. [Learn more about Microsoft 365 wizards.](/microsoft-365/admin/misc/azure-ad-setup-guides)

## Configure and test Azure AD SSO for Sketch

Configure and test Azure AD SSO with Sketch using a test user called **B.Simon**. For SSO to work, you need to establish a link relationship between an Azure AD user and the related user in Sketch.

To configure and test Azure AD SSO with Sketch, perform the following steps:

1. **[Configure Azure AD SSO](#configure-azure-ad-sso)** - to enable your users to use this feature.
    1. **[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with B.Simon.
    1. **[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable B.Simon to use Azure AD single sign-on.
1. **[Configure Sketch SSO](#configure-sketch-sso)** - to configure the single sign-on settings on application side.
1. **[Test SSO](#test-sso)** - to verify whether the configuration works.

## Choose a shortname for your Workspace in Sketch

Follow these steps to choose a shortname and gather information to continue the setup process in Azure AD.

>[!Note]
> Before starting this process, make sure SSO is available in your Workspace, check there is an SSO tab in your Workspace Admin panel.
> If you don't see the SSO tab, please reach out to customer support.
1. [Sign in to your Workspace](https://www.sketch.com/signin/) as an Admin.
1. Head to the **People & Settings** section in the sidebar.
1. Click on the **Single Sign-On** tab.
1. Click **Choose** a short name.
1. Enter a unique name, it should have less than 16 characters and can only include letters, numbers or hyphens. You can edit this name later on.
1. Click **Submit**.
1. Click on the first tab **Set Up Identity Provider**. In this tab, you’ll find the unique Workspace values you’ll need to set up the integration with Azure AD.
    1. **EntityID:** In Azure AD, this is the `Identifier` field.
    1. **ACS URL:** In Azure AD, this is the `Reply URL` field.

Make sure to keep these values at hand! You’ll need them in the next step. Click Copy next to each value to copy it to your clipboard.

## Configure Azure AD SSO

Follow these steps to enable Azure AD SSO in the Azure portal.

1. In the Azure portal, on the **Sketch** application integration page, find the **Manage** section and select **single sign-on**.
1. On the **Select a single sign-on method** page, select **SAML**.
1. On the **Set up single sign-on with SAML** page, click the pencil icon for **Basic SAML Configuration** to edit the settings.

    ![Screenshot shows how to edit Basic SAML Configuration.](common/edit-urls.png "Basic Configuration")

1. On the **Basic SAML Configuration** section, perform the following steps:

    a. In the **Identifier** textbox, use the `EntityID` field from the previous step. It looks like:
    `sketch-<uuid_v4>`

    b. In the **Reply URL** textbox, use the `ACS URL` field from the previous step. It looks like:
    `https://sso.sketch.com/saml/acs?id=<uuid_v4>`

1. Click **Set additional URLs** and perform the following step:  

    In the **Sign-on URL** text box, type the URL:
    `https://www.sketch.com`

    > [!Note]
    > Please use **Identifier** and **Reply URL** values from [Choose a shortname for your Workspace in Sketch](#choose-a-shortname-for-your-workspace-in-sketch) section.

1. Sketch application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration. The following screenshot shows the list of default attributes.

    ![Screenshot shows the image of attribute mappings.](common/default-attributes.png "Attributes")

1. In addition to above, Sketch application expects few more attributes to be passed back in SAML response, which are shown below. These attributes are also pre populated but you can review them as per your requirements.

    | Name | Source Attribute|
    | ------------ | --------- |
    | email | user.mail |
    | first_name | user.givenname |
    | surname | user.surname |  

1. On the **Set-up single sign-on with SAML** page, in the **SAML Signing Certificate** section,  find **Federation Metadata XML** and select **Download** to download the certificate and save it on your computer.

    ![Screenshot shows the Certificate download link.](common/metadataxml.png "Certificate")  

### Create an Azure AD test user

In this section, you'll create a test user in the Azure portal called B.Simon.

1. From the left pane in the Azure portal, select **Azure Active Directory**, select **Users**, and then select **All users**.
1. Select **New user** at the top of the screen.
1. In the **User** properties, follow these steps:
   1. In the **Name** field, enter `B.Simon`.  
   1. In the **User name** field, enter the username@companydomain.extension. For example, `B.Simon@contoso.com`.
   1. Select the **Show password** check box, and then write down the value that's displayed in the **Password** box.
   1. Click **Create**.

### Assign the Azure AD test user

In this section, you'll enable B.Simon to use Azure single sign-on by granting access to Sketch.

1. In the Azure portal, select **Enterprise Applications**, and then select **All applications**.
1. In the applications list, select **Sketch**.
1. In the app's overview page, find the **Manage** section and select **Users and groups**.
1. Select **Add user**, then select **Users and groups** in the **Add Assignment** dialog.
1. In the **Users and groups** dialog, select **B.Simon** from the Users list, then click the **Select** button at the bottom of the screen.
1. If you are expecting a role to be assigned to the users, you can select it from the **Select a role** dropdown. If no role has been set up for this app, you see "Default Access" role selected.
1. In the **Add Assignment** dialog, click the **Assign** button.

## Configure Sketch SSO

Follow these steps to finish the configuration in Sketch.

1. In your Workspace, head to the **Set up Sketch** tab in the **Single Sign-On** window.
1. Upload the XML file you downloaded previously in the **Import XML Metadata file** section.
1. Log out.
1. Click **Sign in with SSO**. 
1. Use the shortname you configured previously to proceed.

## Test SSO 

In this section, you test your Azure AD single sign-on configuration with following options. 

* Click on **Test this application** in Azure portal. This will redirect to Sketch Sign-on URL where you can initiate the login flow. 

* Go to Sketch Sign-on URL directly and initiate the login flow from there.

* You can use Microsoft My Apps. When you click the Sketch tile in the My Apps, this will redirect to Sketch Sign-on URL. For more information about the My Apps, see [Introduction to the My Apps](../user-help/my-apps-portal-end-user-access.md).

## Next steps

Once you configure Sketch you can enforce session control, which protects exfiltration and infiltration of your organization’s sensitive data in real time. Session control extends from Conditional Access. [Learn how to enforce session control with Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).

---
title: 'Tutorial: Azure AD SSO integration with Infogix Data3Sixty Govern'
description: Learn how to configure single sign-on between Azure Active Directory and Infogix Data3Sixty Govern.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 11/21/2022
ms.author: jeedes
---
# Tutorial: Azure AD SSO integration with Infogix Data3Sixty Govern

In this tutorial, you'll learn how to integrate Infogix Data3Sixty Govern with Azure Active Directory (Azure AD). When you integrate Infogix Data3Sixty Govern with Azure AD, you can:

* Control in Azure AD who has access to Infogix Data3Sixty Govern.
* Enable your users to be automatically signed-in to Infogix Data3Sixty Govern with their Azure AD accounts.
* Manage your accounts in one central location - the Azure portal.

## Prerequisites

To get started, you need the following items:

* An Azure AD subscription. If you don't have a subscription, you can get a [free account](https://azure.microsoft.com/free/).
* Infogix Data3Sixty Govern single sign-on (SSO) enabled subscription.
* Along with Cloud Application Administrator, Application Administrator can also add or manage applications in Azure AD.
For more information, see [Azure built-in roles](../roles/permissions-reference.md).

## Scenario description

In this tutorial, you configure and test Azure AD single sign-on in a test environment.

* Infogix Data3Sixty Govern supports **SP and IDP** initiated SSO.
* Infogix Data3Sixty Govern supports **Just In Time** user provisioning.

> [!NOTE]
> Identifier of this application is a fixed string value so only one instance can be configured in one tenant.

## Add Infogix Data3Sixty Govern from the gallery

To configure the integration of Infogix Data3Sixty Govern into Azure AD, you need to add Infogix Data3Sixty Govern from the gallery to your list of managed SaaS apps.

1. Sign in to the Azure portal using either a work or school account, or a personal Microsoft account.
1. On the left navigation pane, select the **Azure Active Directory** service.
1. Navigate to **Enterprise Applications** and then select **All Applications**.
1. To add new application, select **New application**.
1. In the **Add from the gallery** section, type **Infogix Data3Sixty Govern** in the search box.
1. Select **Infogix Data3Sixty Govern** from results panel and then add the app. Wait a few seconds while the app is added to your tenant.

 Alternatively, you can also use the [Enterprise App Configuration Wizard](https://portal.office.com/AdminPortal/home?Q=Docs#/azureadappintegration). In this wizard, you can add an application to your tenant, add users/groups to the app, assign roles, as well as walk through the SSO configuration as well. [Learn more about Microsoft 365 wizards.](/microsoft-365/admin/misc/azure-ad-setup-guides)

## Configure and test Azure AD SSO for Infogix Data3Sixty Govern

Configure and test Azure AD SSO with Infogix Data3Sixty Govern using a test user called **B.Simon**. For SSO to work, you need to establish a link relationship between an Azure AD user and the related user in Infogix Data3Sixty Govern.

To configure and test Azure AD SSO with Infogix Data3Sixty Govern, perform the following steps:

1. **[Configure Azure AD SSO](#configure-azure-ad-sso)** - to enable your users to use this feature.
   1. **[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with B.Simon.
   1. **[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable B.Simon to use Azure AD single sign-on.
1. **[Configure Infogix Data3Sixty Govern SSO](#configure-infogix-data3sixty-govern-sso)** - to configure the single sign-on settings on application side.
   1. **[Create Infogix Data3Sixty Govern test user](#create-infogix-data3sixty-govern-test-user)** - to have a counterpart of B.Simon in Infogix Data3Sixty Govern that is linked to the Azure AD representation of user.
1. **[Test SSO](#test-sso)** - to verify whether the configuration works.

## Configure Azure AD SSO

Follow these steps to enable Azure AD SSO in the Azure portal.

1. In the Azure portal, on the **Infogix Data3Sixty Govern** application integration page, find the **Manage** section and select **single sign-on**.
1. On the **Select a single sign-on method** page, select **SAML**.
1. On the **Set up single sign-on with SAML** page, click the pencil icon for **Basic SAML Configuration** to edit the settings.

   ![Screenshot shows to edit Basic S A M L Configuration.](common/edit-urls.png "Basic Configuration")

1. On the **Basic SAML Configuration** section, perform the following steps:

    a. In the **Identifier** text box, type the URL:
    `https://data3sixty.com/ui`

    b. In the **Reply URL** text box, type a URL using the following pattern:
    `https://<subdomain>.data3sixty.com/sso/acs`

1. Click **Set additional URLs** and perform the following step if you wish to configure the application in **SP** initiated mode:

    In the **Sign-on URL** text box, type a URL using the following pattern:
    `https://<subdomain>.data3sixty.com`

    > [!NOTE]
    > These values are not real. Update these values with the actual Reply URL and Sign-On URL. Contact [Infogix Data3Sixty Govern Client support team](mailto:data3sixtysupport@infogix.com) to get these values. You can also refer to the patterns shown in the **Basic SAML Configuration** section in the Azure portal.

1. Infogix Data3Sixty Govern application expects the SAML assertions in a specific format. Configure the following claims for this application. You can manage the values of these attributes from the **User Attributes** section on application integration page. On the **Set up Single Sign-On with SAML** page, click **Edit** button to open **User Attributes** dialog.

    ![Screenshot shows User Attributes with the Edit icon selected.](common/edit-attribute.png "Attributes")

1. In the **User Claims** section on the **User Attributes** dialog, edit the claims by using **Edit icon** or add the claims by using **Add new claim** to configure SAML token attribute as shown in the image above and perform the following steps:

    | Name | Source Attribute|
    | -----------| -------------- |
    | firstname  | user.givenname |
    | lastname | user.surname |
    | username | user.mail |

    a. Click **Add new claim** to open the **Manage user claims** dialog.

    ![Screenshot shows User claims with the option to Add new claim.](common/new-save-attribute.png "Claims")

    ![Screenshot shows the Manage user claims dialog box where you can enter the values described.](common/new-attribute-details.png "Values")

    b. In the **Name** textbox, type the attribute name shown for that row.

    c. Leave the **Namespace** blank.

    d. Select Source as **Attribute**.

    e. From the **Source attribute** list, type the attribute value shown for that row.

    f. Click **Ok**

    g. Click **Save**.

1. On the **Set up Single Sign-On with SAML** page, in the **SAML Signing Certificate** section, click **Download** to download the **Certificate (Raw)** from the given options as per your requirement and save it on your computer.

    ![Screenshot shows the Certificate download link.](common/certificateraw.png "Certificate")

1. On the **Set up Infogix Data3Sixty Govern** section, copy the appropriate URL(s) as per your requirement.

    ![Screenshot shows to copy configuration appropriate U R L.](common/copy-configuration-urls.png "Metadata")

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

In this section, you'll enable B.Simon to use Azure single sign-on by granting access to Infogix Data3Sixty Govern.

1. In the Azure portal, select **Enterprise Applications**, and then select **All applications**.
1. In the applications list, select **Infogix Data3Sixty Govern**.
1. In the app's overview page, find the **Manage** section and select **Users and groups**.
1. Select **Add user**, then select **Users and groups** in the **Add Assignment** dialog.
1. In the **Users and groups** dialog, select **B.Simon** from the Users list, then click the **Select** button at the bottom of the screen.
1. If you're expecting any role value in the SAML assertion, in the **Select Role** dialog, select the appropriate role for the user from the list and then click the **Select** button at the bottom of the screen.
1. In the **Add Assignment** dialog, click the **Assign** button.

## Configure Infogix Data3Sixty Govern SSO

To configure single sign-on on **Infogix Data3Sixty Govern** side, you need to send the downloaded **Certificate (Raw)** and appropriate copied URLs from Azure portal to [Infogix Data3Sixty Govern support team](mailto:data3sixtysupport@infogix.com). They set this setting to have the SAML SSO connection set properly on both sides.

### Create Infogix Data3Sixty Govern test user

In this section, a user called Britta Simon is created in Infogix Data3Sixty Govern. Infogix Data3Sixty Govern supports just-in-time user provisioning, which is enabled by default. There is no action item for you in this section. If a user doesn't already exist in Infogix Data3Sixty Govern, a new one is created after authentication.

> [!Note]
> If you need to create a user manually, contact [Infogix Data3Sixty Govern support team](mailto:data3sixtysupport@infogix.com).

## Test SSO

In this section, you test your Azure AD single sign-on configuration with following options. 

#### SP initiated:

* Click on **Test this application** in Azure portal. This will redirect to Infogix Data3Sixty Govern Sign-on URL where you can initiate the login flow.  

* Go to Infogix Data3Sixty Govern Sign-on URL directly and initiate the login flow from there.

#### IDP initiated:

* Click on **Test this application** in Azure portal and you should be automatically signed in to the Infogix Data3Sixty Govern for which you set up the SSO. 

You can also use Microsoft My Apps to test the application in any mode. When you click the Infogix Data3Sixty Govern tile in the My Apps, if configured in SP mode you would be redirected to the application sign-on page for initiating the login flow and if configured in IDP mode, you should be automatically signed in to the Infogix Data3Sixty Govern for which you set up the SSO. For more information about the My Apps, see [Introduction to the My Apps](../user-help/my-apps-portal-end-user-access.md).

## Next steps

Once you configure Infogix Data3Sixty Govern you can enforce session control, which protects exfiltration and infiltration of your organization’s sensitive data in real time. Session control extends from Conditional Access. [Learn how to enforce session control with Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).
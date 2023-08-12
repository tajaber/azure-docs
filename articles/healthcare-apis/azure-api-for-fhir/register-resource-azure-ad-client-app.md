---
title: Register a resource app in Azure AD - Azure API for FHIR
description: Register a resource (or API) app in Azure Active Directory, so that client applications can request access to the resource when authenticating.
services: healthcare-apis
author: expekesheth
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.date: 06/03/2022
ms.author: kesheth
ms.custom: devx-tr2ck-azurepowershell
---

# Register a resource application in Azure Active Directory for Azure API for FHIR

In this article, you'll learn how to register a resource (or API) application in Azure Active Directory (Azure AD). A resource application is an Azure AD representation of the FHIR server API itself and client applications can request access to the resource when authenticating. The resource application is also known as the *audience* in OAuth parlance.

## Azure API for FHIR

If you're using the Azure API for FHIR, a resource application is automatically created when you deploy the service. As long as you're using the Azure API for FHIR in the same Azure AD tenant as you're deploying your application, you can skip this how-to-guide and instead deploy your Azure API for FHIR to get started.

If you're using a different Azure AD tenant (not associated with your subscription), you can import the Azure API for FHIR resource application into your tenant with
PowerShell:

```azurepowershell-interactive
New-AzADServicePrincipal -ApplicationId 4f6778d8-5aef-43dc-a1ff-b073724b9495 -Role Contributor
```

or you can use Azure CLI:

```azurecli-interactive
az ad sp create --id 4f6778d8-5aef-43dc-a1ff-b073724b9495
```

## FHIR Server for Azure

If you're using the open source FHIR Server for Azure, follow the steps on the [GitHub repo](https://github.com/microsoft/fhir-server/blob/master/docs/Register-Resource-Application.md) to register a resource application.

## Next steps

In this article, you've learned how to register a resource application in Azure AD. Next, register your confidential client application.

>[!div class="nextstepaction"]
>[Register Confidential Client Application](register-confidential-azure-ad-client-app.md)

FHIR&#174; is a registered trademark of [HL7](https://hl7.org/fhir/) and is used with the permission of HL7.
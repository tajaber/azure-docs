---
title: Deploy DICOM service using the Azure portal - Azure Health Data Services
description: This article describes how to deploy DICOM service in the Azure portal.
author: mmitrik
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: quickstart
ms.date: 05/03/2022
ms.author: mmitrik
ms.custom: mode-api
---

# Deploy DICOM service using the Azure portal

In this quickstart, you'll learn how to deploy DICOM Service using the Azure portal.

Once deployment is complete, you can use the Azure portal to navigate to the newly created DICOM service to see the details including your Service URL. The Service URL to access your DICOM service  will be: ```https://<workspacename-dicomservicename>.dicom.azurehealthcareapis.com```. Make sure to specify the version as part of the url when making requests. More information can be found in the [API Versioning for DICOM service documentation](api-versioning-dicom-service.md).

## Prerequisite

To deploy DICOM service, you must have a workspace created in the Azure portal. For more information about creating a workspace, see **Deploy workspace in the Azure portal**.

## Deploying DICOM service

1. On the **Resource group** page of the Azure portal, select the name of your **Azure Health Data Services workspace**.

   [ ![Screenshot of select workspace resource group.](media/select-workspace-resource-group.png) ](media/select-workspace-resource-group.png#lightbox)

2. Select **Deploy DICOM service**.

   [ ![Screenshot of deploy DICOM service.](media/workspace-deploy-dicom-services.png) ](media/workspace-deploy-dicom-services.png#lightbox)


3. Select **Add DICOM service**.

   [ ![Screenshot of add DICOM service.](media/add-dicom-service.png) ](media/add-dicom-service.png#lightbox)


4. Enter a name for DICOM service, and then select **Review + create**. 

    [ ![Screenshot of DICOM service name.](media/enter-dicom-service-name.png) ](media/enter-dicom-service-name.png#lightbox)


   (**Optional**) Select **Next: Tags >**.

    Tags are name/value pairs used for categorizing resources. For information about tags, see [Use tags to organize your Azure resources and management hierarchy](../../azure-resource-manager/management/tag-resources.md).

5. When you notice the green validation check mark, select **Create** to deploy DICOM service.

6. When the deployment process completes, select **Go to resource**.  

   [ ![Screenshot of DICOM go to resource.](media/go-to-resource.png) ](media/go-to-resource.png#lightbox)

   The result of the newly deployed DICOM service is shown below.

   [ ![Screenshot of DICOM finished deployment.](media/results-deployed-dicom-service.png) ](media/results-deployed-dicom-service.png#lightbox)


## Next steps

In this quickstart, you learned how to deploy DICOM service using the Azure portal. For information about assigning roles for the DICOM service, see 

>[!div class="nextstepaction"]
>[Assign roles for the DICOM service](../configure-azure-rbac.md#assign-roles-for-the-dicom-service)

For more information about  how to use the DICOMweb&trade; Standard APIs with the DICOM service, see

>[!div class="nextstepaction"]
>[Using DICOMweb&trade;Standard APIs with DICOM services](dicomweb-standard-apis-with-dicom-services.md)
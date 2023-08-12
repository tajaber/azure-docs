---
title:  Pull DICOM changes using the Change Feed 
description: This how-to guide explains how to pull DICOM changes using DICOM Change Feed for Azure Health Data Services.
author: mmitrik
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: how-to
ms.date: 02/15/2022
ms.author: mmitrik
---

# Pull DICOM changes using the Change Feed

DICOM Change Feed offers customers the ability to go through the history of the DICOM service and act on the create and delete events in the service. This how-to guide describes how to consume Change Feed.

The Change Feed is accessed using REST APIs. These APIs along with sample usage of Change Feed are documented in the [Overview of DICOM Change Feed](dicom-change-feed-overview.md). The version of the REST API should be explicitly specified in the request URL as called out in the [API Versioning for DICOM service Documentation](api-versioning-dicom-service.md).

## Consume Change Feed

The following C# code example shows how to consume Change Feed using the DICOM client package.

```csharp
const int limit = 10;
 
using HttpClient httpClient = new HttpClient { BaseAddress = new Uri("<URL>") };
using CancellationTokenSource tokenSource = new CancellationTokenSource();
 
int read;
List<ChangeFeedEntry> entries = new List<ChangeFeedEntry>();
DicomWebClient client = new DicomWebClient(httpClient);
do
{
    read = 0;
    DicomWebAsyncEnumerableResponse<ChangeFeedEntry> result = await client.GetChangeFeed(
        $"?offset={entries.Count}&limit={limit}&includeMetadata={true}",
        tokenSource.Token);
 
    await foreach (ChangeFeedEntry entry in result)
    {
        read++;
        entries.Add(entry);
    }
} while (read > 0);
```

To view and access the **ChangeFeedRetrieveService.cs** code example, see [Consume Change Feed](https://github.com/microsoft/dicom-server/blob/main/converter/dicom-cast/src/Microsoft.Health.DicomCast.Core/Features/DicomWeb/Service/ChangeFeedRetrieveService.cs).

## Next Steps

This how-to guide describes how to consume Change Feed. Change Feed allows you to monitor the history of the DICOM service. For information about the DICOM service, see

>[!div class="nextstepaction"]
>[Overview of the DICOM service](dicom-services-overview.md)

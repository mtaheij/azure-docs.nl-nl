---
title: De Postman-omgeving importeren voor Azure Media Services REST-aanroepen
description: In dit onderwerp vindt u een definitie van de Postman-omgeving voor Azure Media Services REST-aanroepen.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: 2ccc765a3fb0063d92382edcec260808183f27a2
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/01/2020
ms.locfileid: "89259248"
---
# <a name="import-the-postman-environment"></a>De Postman-omgeving importeren

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)] 

Dit artikel bevat een definitie van de **postman** -omgevings variabelen die worden gebruikt de [postman-verzameling](postman-collection.md) die gegroepeerde HTTP-aanvragen bevat die Media Services rest-api's aanroepen. De omgevings-en verzamelings bestanden worden gebruikt door de zelf studie [postman configureren voor Media Services rest API](media-rest-apis-with-postman.md) .

> [!NOTE]
> De waarde van `AzureADSTSEndpoint `  =  `https://login.microsoftonline.com/{{TenantId}}/oauth2/token` . Als u uw Tenant-ID wilt ophalen, kunt u de muis aanwijzer over uw gebruikers naam in de portal (in de rechter bovenhoek) bewegen en deze bevindt zich in de map: micro soft ({{TENANTID}}).

```
{
  "id": "2dbce3ce-74c2-2ceb-0461-c4c2323f5b09",
  "name": "AzureMedia",
  "values": [
    {
      "enabled": true,
      "key": "TenantID",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "AzureADSTSEndpoint",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "RESTAPIEndpoint",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "ClientID",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "ClientSecret",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "AccessToken",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "LastAssetId",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "LastAccessPolicyId",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "UploadURL",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "MediaFileName",
      "value": "BigBuckBunny.mp4",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "LastChannelId",
      "value": "",
      "type": "text"
    }
  ],
  "_postman_variable_scope": "environment",
  "_postman_exported_using": "Postman/5.5.0"
}
```

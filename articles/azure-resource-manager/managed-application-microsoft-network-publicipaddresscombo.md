---
title: "aaaAzure hanterade program PublicIpAddressCombo gränssnittselement | Microsoft Docs"
description: "Beskriver hello Microsoft.Network.PublicIpAddressCombo UI-element för hanterade program i Azure"
services: azure-resource-manager
documentationcenter: na
author: tabrezm
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/12/2017
ms.author: tabrezm;tomfitz
ms.openlocfilehash: 8ba689005c0eccda0a57bf628de4b5197886a950
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftnetworkpublicipaddresscombo-ui-element"></a>Microsoft.Network.PublicIpAddressCombo UI-element
En grupp av kontroller för att välja en ny eller befintlig offentlig IP-adress. Du använder det här elementet när [att skapa ett Azure hanterade program](managed-application-publishing.md).

## <a name="ui-sample"></a>UI-exempel
![Microsoft.Network.PublicIpAddressCombo](./media/managed-application-elements/microsoft.network.publicipaddresscombo.png)

- Om hello användaren väljer ”Ingen” för den offentliga IP-adress, döljs hello domän etikett textrutan.
- Om hello användaren väljer en befintlig offentlig IP-adress, inaktiveras hello domän etikett textrutan. Dess värde är hello domännamnet hello valt IP-adress.
- hello domain name suffix (till exempel westus.cloudapp.azure.com) uppdateringar automatiskt baserat på valda hello plats.

## <a name="schema"></a>Schemat
```json
{
  "name": "element1",
  "type": "Microsoft.Network.PublicIpAddressCombo",
  "label": {
    "publicIpAddress": "Public IP address",
    "domainNameLabel": "Domain name label"
  },
  "toolTip": {
    "publicIpAddress": "",
    "domainNameLabel": ""
  },
  "defaultValue": {
    "publicIpAddressName": "ip01",
    "domainNameLabel": "foobar"
  },
  "constraints": {
    "required": {
      "domainNameLabel": true
    }
  },
  "options": {
    "hideNone": false,
    "hideDomainNameLabel": false,
    "hideExisting": false
  },
  "visible": true
}
```

## <a name="remarks"></a>Kommentarer
- Om `constraints.required.domainNameLabel` har angetts för**SANT**, hello användaren måste ange en domännamnet när du skapar en ny offentlig IP-adress. Befintliga offentliga IP-adresser utan en etikett inte är tillgängliga för val.
- Om `options.hideNone` har angetts för**SANT**, hello alternativet tooselect **ingen** för hello offentliga IP-adressen är dolt. hello standardvärdet är **FALSKT**.
- Om `options.hideDomainNameLabel` har angetts för**SANT**, och sedan hello textruta för domännamnet är dolt. hello standardvärdet är **FALSKT**.
- Om `options.hideExisting` är sant, hello användaren är inte kan toochoose en befintlig offentlig IP-adress. hello standardvärdet är **FALSKT**.

## <a name="sample-output"></a>Exempel på utdata
Om hello användaren väljer Ingen offentlig IP-adress, förväntas hello följande utdata:
```json
{
  "newOrExistingOrNone": "none"
}
```

Om hello användaren markerar en ny eller befintlig IP-adress, förväntas hello följande utdata:
```json
{
  "name": "ip01",
  "resourceGroup": "rg01",
  "domainNameLabel": "foobar",
  "newOrExistingOrNone": "new"
}
```
- När `options.hideNone` anges `newOrExistingOrNone` returnerar alltid **ingen**.
- När `options.hideDomainNameLabel` anges `domainNameLabel` har inte deklarerats.

## <a name="next-steps"></a>Nästa steg
* En introduktion toomanaged program, se [översikt över Azure Managed Application](managed-application-overview.md).
* En introduktion toocreating UI definitioner finns [komma igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).
* En beskrivning av gemensamma egenskaper i UI-element, se [CreateUiDefinition element](managed-application-createuidefinition-elements.md).

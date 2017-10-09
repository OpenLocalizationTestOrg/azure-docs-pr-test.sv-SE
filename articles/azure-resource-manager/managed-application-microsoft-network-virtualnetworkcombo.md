---
title: "aaaAzure hanterade program VirtualNetworkCombo gränssnittselement | Microsoft Docs"
description: "Beskriver hello Microsoft.Network.VirtualNetworkCombo UI-element för hanterade program i Azure"
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
ms.openlocfilehash: 1b0fa5360d93306f7a814723f77e42540bdaaa9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftnetworkvirtualnetworkcombo-ui-element"></a>Microsoft.Network.VirtualNetworkCombo UI-element
En grupp av kontroller för att välja ett nytt eller befintligt virtuellt nätverk. Du använder det här elementet när [att skapa ett Azure hanterade program](managed-application-publishing.md).

## <a name="ui-sample"></a>UI-exempel
![Microsoft.Network.VirtualNetworkCombo](./media/managed-application-elements/microsoft.network.virtualnetworkcombo.png)

- I hello översta Trådblock, har hello användaren valts ett nytt virtuellt nätverk så hello användare kan anpassa namn och adress prefix för varje undernät. I detta fall är konfigurera undernät valfritt.
- I hello nedre Trådblock hello användare har valts ut för ett befintligt virtuellt nätverk, så hello användaren måste mappa varje undernät hello Distributionsmall kräver tooan befintligt undernät. Konfiguration av undernät krävs i det här fallet.

## <a name="schema"></a>Schemat
```json
{
  "name": "element1",
  "type": "Microsoft.Network.VirtualNetworkCombo",
  "label": {
    "virtualNetwork": "Virtual network",
    "subnets": "Subnets"
  },
  "toolTip": {
    "virtualNetwork": "",
    "subnets": ""
  },
  "defaultValue": {
    "name": "vnet01",
    "addressPrefixSize": "/16"
  },
  "constraints": {
    "minAddressPrefixSize": "/16"
  },
  "options": {
    "hideExisting": false
  },
  "subnets": {
    "subnet1": {
      "label": "First subnet",
      "defaultValue": {
        "name": "subnet-1",
        "addressPrefixSize": "/24"
      },
      "constraints": {
        "minAddressPrefixSize": "/24",
        "minAddressCount": 12,
        "requireContiguousAddresses": true
      }
    },
    "subnet2": {
      "label": "Second subnet",
      "defaultValue": {
        "name": "subnet-2",
        "addressPrefixSize": "/26"
      },
      "constraints": {
        "minAddressPrefixSize": "/26",
        "minAddressCount": 8,
        "requireContiguousAddresses": true
      }
    }
  },
  "visible": true
}
```

## <a name="remarks"></a>Kommentarer
- Om anges hello första icke-överlappande adressprefixet storlek `defaultValue.addressPrefixSize` bestäms automatiskt baserat på de befintliga virtuella nätverk i hello användarens prenumeration.
- Hej standardvärdet för `defaultValue.name` och `defaultValue.addressPrefixSize` är **null**.
- `constraints.minAddressPrefixSize`måste anges. Alla befintliga virtuella nätverk med ett adressutrymme som är mindre än hello angivna värdet är inte tillgänglig för val.
- `subnets`måste anges, och `constraints.minAddressPrefixSize` måste anges för varje undernät.
- När du skapar ett nytt virtuellt nätverk, varje undernät adressprefixet beräknas automatiskt baserat på hello virtuellt adressprefixet och motsvarande `addressPrefixSize`.
- När du använder ett befintligt virtuellt nätverk, alla undernät som är mindre än respektive `constraints.minAddressPrefixSize` är inte tillgängliga för val. Dessutom, om angivna undernät som inte innehåller minst `minAddressCount` tillgängliga adresser är inte tillgängliga för val.
hello standardvärdet är **0**. tooensure som hello tillgängliga adresser är sammanhängande, ange **SANT** för `requireContiguousAddresses`. hello standardvärdet är **SANT**.
- Det går inte att skapa undernät i ett befintligt virtuellt nätverk.
- Om `options.hideExisting` är **SANT**, hello användaren kan inte välja ett befintligt virtuellt nätverk. hello standardvärdet är **FALSKT**.

## <a name="sample-output"></a>Exempel på utdata
```json
{
  "name": "vnet01",
  "resourceGroup": "rg01",
  "addressPrefixes": ["10.0.0.0/16"],
  "newOrExisting": "new",
  "subnets": {
    "subnet1": {
      "name": "subnet-1",
      "addressPrefix": "10.0.0.0/24",
      "startAddress": "10.0.0.1"
    },
    "subnet2": {
      "name": "subnet-2",
      "addressPrefix": "10.0.1.0/26",
      "startAddress": "10.0.1.1"
    }
  }
}
```

## <a name="next-steps"></a>Nästa steg
* En introduktion toomanaged program, se [översikt över Azure Managed Application](managed-application-overview.md).
* En introduktion toocreating UI definitioner finns [komma igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).
* En beskrivning av gemensamma egenskaper i UI-element, se [CreateUiDefinition element](managed-application-createuidefinition-elements.md).

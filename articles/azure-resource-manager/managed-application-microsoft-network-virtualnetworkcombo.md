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
# <a name="microsoftnetworkvirtualnetworkcombo-ui-element"></a><span data-ttu-id="d4c3e-103">Microsoft.Network.VirtualNetworkCombo UI-element</span><span class="sxs-lookup"><span data-stu-id="d4c3e-103">Microsoft.Network.VirtualNetworkCombo UI element</span></span>
<span data-ttu-id="d4c3e-104">En grupp av kontroller för att välja ett nytt eller befintligt virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="d4c3e-104">A group of controls for selecting a new or existing virtual network.</span></span> <span data-ttu-id="d4c3e-105">Du använder det här elementet när [att skapa ett Azure hanterade program](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="d4c3e-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="d4c3e-106">UI-exempel</span><span class="sxs-lookup"><span data-stu-id="d4c3e-106">UI sample</span></span>
![Microsoft.Network.VirtualNetworkCombo](./media/managed-application-elements/microsoft.network.virtualnetworkcombo.png)

- <span data-ttu-id="d4c3e-108">I hello översta Trådblock, har hello användaren valts ett nytt virtuellt nätverk så hello användare kan anpassa namn och adress prefix för varje undernät.</span><span class="sxs-lookup"><span data-stu-id="d4c3e-108">In hello top wireframe, hello user has picked a new virtual network, so hello user can customize each subnet's name and address prefix.</span></span> <span data-ttu-id="d4c3e-109">I detta fall är konfigurera undernät valfritt.</span><span class="sxs-lookup"><span data-stu-id="d4c3e-109">Configuring subnets in this case is optional.</span></span>
- <span data-ttu-id="d4c3e-110">I hello nedre Trådblock hello användare har valts ut för ett befintligt virtuellt nätverk, så hello användaren måste mappa varje undernät hello Distributionsmall kräver tooan befintligt undernät.</span><span class="sxs-lookup"><span data-stu-id="d4c3e-110">In hello bottom wireframe, hello user has picked an existing virtual network, so hello user must map each subnet hello deployment template requires tooan existing subnet.</span></span> <span data-ttu-id="d4c3e-111">Konfiguration av undernät krävs i det här fallet.</span><span class="sxs-lookup"><span data-stu-id="d4c3e-111">Configuring subnets in this case is required.</span></span>

## <a name="schema"></a><span data-ttu-id="d4c3e-112">Schemat</span><span class="sxs-lookup"><span data-stu-id="d4c3e-112">Schema</span></span>
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

## <a name="remarks"></a><span data-ttu-id="d4c3e-113">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="d4c3e-113">Remarks</span></span>
- <span data-ttu-id="d4c3e-114">Om anges hello första icke-överlappande adressprefixet storlek `defaultValue.addressPrefixSize` bestäms automatiskt baserat på de befintliga virtuella nätverk i hello användarens prenumeration.</span><span class="sxs-lookup"><span data-stu-id="d4c3e-114">If specified, hello first non-overlapping address prefix of size `defaultValue.addressPrefixSize` is determined automatically based on the existing virtual networks in hello user's subscription.</span></span>
- <span data-ttu-id="d4c3e-115">Hej standardvärdet för `defaultValue.name` och `defaultValue.addressPrefixSize` är **null**.</span><span class="sxs-lookup"><span data-stu-id="d4c3e-115">hello default value for `defaultValue.name` and `defaultValue.addressPrefixSize` is **null**.</span></span>
- <span data-ttu-id="d4c3e-116">`constraints.minAddressPrefixSize`måste anges.</span><span class="sxs-lookup"><span data-stu-id="d4c3e-116">`constraints.minAddressPrefixSize` must be specified.</span></span> <span data-ttu-id="d4c3e-117">Alla befintliga virtuella nätverk med ett adressutrymme som är mindre än hello angivna värdet är inte tillgänglig för val.</span><span class="sxs-lookup"><span data-stu-id="d4c3e-117">Any existing virtual networks with an address space smaller than hello specified value are unavailable for selection.</span></span>
- <span data-ttu-id="d4c3e-118">`subnets`måste anges, och `constraints.minAddressPrefixSize` måste anges för varje undernät.</span><span class="sxs-lookup"><span data-stu-id="d4c3e-118">`subnets` must be specified, and `constraints.minAddressPrefixSize` must be specified for each subnet.</span></span>
- <span data-ttu-id="d4c3e-119">När du skapar ett nytt virtuellt nätverk, varje undernät adressprefixet beräknas automatiskt baserat på hello virtuellt adressprefixet och motsvarande `addressPrefixSize`.</span><span class="sxs-lookup"><span data-stu-id="d4c3e-119">When creating a new virtual network, each subnet's address prefix is calculated automatically based on hello virtual network's address prefix and the respective `addressPrefixSize`.</span></span>
- <span data-ttu-id="d4c3e-120">När du använder ett befintligt virtuellt nätverk, alla undernät som är mindre än respektive `constraints.minAddressPrefixSize` är inte tillgängliga för val.</span><span class="sxs-lookup"><span data-stu-id="d4c3e-120">When using an existing virtual network, any subnets smaller than the respective `constraints.minAddressPrefixSize` are unavailable for selection.</span></span> <span data-ttu-id="d4c3e-121">Dessutom, om angivna undernät som inte innehåller minst `minAddressCount` tillgängliga adresser är inte tillgängliga för val.</span><span class="sxs-lookup"><span data-stu-id="d4c3e-121">Additionally, if specified, subnets that do not contain at least `minAddressCount` available addresses are unavailable for selection.</span></span>
<span data-ttu-id="d4c3e-122">hello standardvärdet är **0**.</span><span class="sxs-lookup"><span data-stu-id="d4c3e-122">hello default value is **0**.</span></span> <span data-ttu-id="d4c3e-123">tooensure som hello tillgängliga adresser är sammanhängande, ange **SANT** för `requireContiguousAddresses`.</span><span class="sxs-lookup"><span data-stu-id="d4c3e-123">tooensure that hello available addresses are contiguous, specify **true** for `requireContiguousAddresses`.</span></span> <span data-ttu-id="d4c3e-124">hello standardvärdet är **SANT**.</span><span class="sxs-lookup"><span data-stu-id="d4c3e-124">hello default value is **true**.</span></span>
- <span data-ttu-id="d4c3e-125">Det går inte att skapa undernät i ett befintligt virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="d4c3e-125">Creating subnets in an existing virtual network is not supported.</span></span>
- <span data-ttu-id="d4c3e-126">Om `options.hideExisting` är **SANT**, hello användaren kan inte välja ett befintligt virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="d4c3e-126">If `options.hideExisting` is **true**, hello user can't choose an existing virtual network.</span></span> <span data-ttu-id="d4c3e-127">hello standardvärdet är **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="d4c3e-127">hello default value is **false**.</span></span>

## <a name="sample-output"></a><span data-ttu-id="d4c3e-128">Exempel på utdata</span><span class="sxs-lookup"><span data-stu-id="d4c3e-128">Sample output</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="d4c3e-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d4c3e-129">Next steps</span></span>
* <span data-ttu-id="d4c3e-130">En introduktion toomanaged program, se [översikt över Azure Managed Application](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d4c3e-130">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="d4c3e-131">En introduktion toocreating UI definitioner finns [komma igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d4c3e-131">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="d4c3e-132">En beskrivning av gemensamma egenskaper i UI-element, se [CreateUiDefinition element](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="d4c3e-132">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>

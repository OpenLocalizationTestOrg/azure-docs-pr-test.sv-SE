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
# <a name="microsoftnetworkpublicipaddresscombo-ui-element"></a><span data-ttu-id="eb6e7-103">Microsoft.Network.PublicIpAddressCombo UI-element</span><span class="sxs-lookup"><span data-stu-id="eb6e7-103">Microsoft.Network.PublicIpAddressCombo UI element</span></span>
<span data-ttu-id="eb6e7-104">En grupp av kontroller för att välja en ny eller befintlig offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="eb6e7-104">A group of controls for selecting a new or existing public IP address.</span></span> <span data-ttu-id="eb6e7-105">Du använder det här elementet när [att skapa ett Azure hanterade program](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="eb6e7-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="eb6e7-106">UI-exempel</span><span class="sxs-lookup"><span data-stu-id="eb6e7-106">UI sample</span></span>
![Microsoft.Network.PublicIpAddressCombo](./media/managed-application-elements/microsoft.network.publicipaddresscombo.png)

- <span data-ttu-id="eb6e7-108">Om hello användaren väljer ”Ingen” för den offentliga IP-adress, döljs hello domän etikett textrutan.</span><span class="sxs-lookup"><span data-stu-id="eb6e7-108">If hello user selects 'None' for public IP address, hello domain name label text box is hidden.</span></span>
- <span data-ttu-id="eb6e7-109">Om hello användaren väljer en befintlig offentlig IP-adress, inaktiveras hello domän etikett textrutan.</span><span class="sxs-lookup"><span data-stu-id="eb6e7-109">If hello user selects an existing public IP address, hello domain name label text box is disabled.</span></span> <span data-ttu-id="eb6e7-110">Dess värde är hello domännamnet hello valt IP-adress.</span><span class="sxs-lookup"><span data-stu-id="eb6e7-110">Its value is hello domain name label of hello selected IP address.</span></span>
- <span data-ttu-id="eb6e7-111">hello domain name suffix (till exempel westus.cloudapp.azure.com) uppdateringar automatiskt baserat på valda hello plats.</span><span class="sxs-lookup"><span data-stu-id="eb6e7-111">hello domain name suffix (for example, westus.cloudapp.azure.com) updates automatically based on hello selected location.</span></span>

## <a name="schema"></a><span data-ttu-id="eb6e7-112">Schemat</span><span class="sxs-lookup"><span data-stu-id="eb6e7-112">Schema</span></span>
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

## <a name="remarks"></a><span data-ttu-id="eb6e7-113">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="eb6e7-113">Remarks</span></span>
- <span data-ttu-id="eb6e7-114">Om `constraints.required.domainNameLabel` har angetts för**SANT**, hello användaren måste ange en domännamnet när du skapar en ny offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="eb6e7-114">If `constraints.required.domainNameLabel` is set too**true**, hello user must provide a domain name label when creating a new public IP address.</span></span> <span data-ttu-id="eb6e7-115">Befintliga offentliga IP-adresser utan en etikett inte är tillgängliga för val.</span><span class="sxs-lookup"><span data-stu-id="eb6e7-115">Existing public IP addresses without a label are not available for selection.</span></span>
- <span data-ttu-id="eb6e7-116">Om `options.hideNone` har angetts för**SANT**, hello alternativet tooselect **ingen** för hello offentliga IP-adressen är dolt.</span><span class="sxs-lookup"><span data-stu-id="eb6e7-116">If `options.hideNone` is set too**true**, then hello option tooselect **None** for hello public IP address is hidden.</span></span> <span data-ttu-id="eb6e7-117">hello standardvärdet är **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="eb6e7-117">hello default value is **false**.</span></span>
- <span data-ttu-id="eb6e7-118">Om `options.hideDomainNameLabel` har angetts för**SANT**, och sedan hello textruta för domännamnet är dolt.</span><span class="sxs-lookup"><span data-stu-id="eb6e7-118">If `options.hideDomainNameLabel` is set too**true**, then hello text box for domain name label is hidden.</span></span> <span data-ttu-id="eb6e7-119">hello standardvärdet är **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="eb6e7-119">hello default value is **false**.</span></span>
- <span data-ttu-id="eb6e7-120">Om `options.hideExisting` är sant, hello användaren är inte kan toochoose en befintlig offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="eb6e7-120">If `options.hideExisting` is true, then hello user is not able toochoose an existing public IP address.</span></span> <span data-ttu-id="eb6e7-121">hello standardvärdet är **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="eb6e7-121">hello default value is **false**.</span></span>

## <a name="sample-output"></a><span data-ttu-id="eb6e7-122">Exempel på utdata</span><span class="sxs-lookup"><span data-stu-id="eb6e7-122">Sample output</span></span>
<span data-ttu-id="eb6e7-123">Om hello användaren väljer Ingen offentlig IP-adress, förväntas hello följande utdata:</span><span class="sxs-lookup"><span data-stu-id="eb6e7-123">If hello user selects no public IP address, hello following output is expected:</span></span>
```json
{
  "newOrExistingOrNone": "none"
}
```

<span data-ttu-id="eb6e7-124">Om hello användaren markerar en ny eller befintlig IP-adress, förväntas hello följande utdata:</span><span class="sxs-lookup"><span data-stu-id="eb6e7-124">If hello user selects a new or existing IP address, hello following output is expected:</span></span>
```json
{
  "name": "ip01",
  "resourceGroup": "rg01",
  "domainNameLabel": "foobar",
  "newOrExistingOrNone": "new"
}
```
- <span data-ttu-id="eb6e7-125">När `options.hideNone` anges `newOrExistingOrNone` returnerar alltid **ingen**.</span><span class="sxs-lookup"><span data-stu-id="eb6e7-125">When `options.hideNone` is specified, `newOrExistingOrNone` always returns **none**.</span></span>
- <span data-ttu-id="eb6e7-126">När `options.hideDomainNameLabel` anges `domainNameLabel` har inte deklarerats.</span><span class="sxs-lookup"><span data-stu-id="eb6e7-126">When `options.hideDomainNameLabel` is specified, `domainNameLabel` is undeclared.</span></span>

## <a name="next-steps"></a><span data-ttu-id="eb6e7-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="eb6e7-127">Next steps</span></span>
* <span data-ttu-id="eb6e7-128">En introduktion toomanaged program, se [översikt över Azure Managed Application](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="eb6e7-128">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="eb6e7-129">En introduktion toocreating UI definitioner finns [komma igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="eb6e7-129">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="eb6e7-130">En beskrivning av gemensamma egenskaper i UI-element, se [CreateUiDefinition element](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="eb6e7-130">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>

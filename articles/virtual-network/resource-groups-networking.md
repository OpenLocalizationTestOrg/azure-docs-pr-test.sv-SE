---
title: "aaaNetwork Resource Provider översikt | Microsoft Docs"
description: "Lär dig mer om hello nya Nätverksresursprovidern i Azure Resource Manager"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: 79bf09da-4809-45cb-8d21-705616ef24dc
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.openlocfilehash: 81b8f51fe8ee180d8f7885c6e04eb953904d7be5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="network-resource-provider"></a><span data-ttu-id="c91a4-103">Provider för nätverksresurser</span><span class="sxs-lookup"><span data-stu-id="c91a4-103">Network Resource Provider</span></span>
<span data-ttu-id="c91a4-104">Behovet av en gäller i dagens företag lyckade är hello möjlighet toobuild och hantera anpassade program i stor skala nätverk på ett flexibelt, flexibel, säker och repeterbara sätt.</span><span class="sxs-lookup"><span data-stu-id="c91a4-104">An underpinning need in today’s business success, is hello ability toobuild and manage large scale network aware applications in an agile, flexible, secure and repeatable way.</span></span> <span data-ttu-id="c91a4-105">Azure Resource Manager kan du toocreate sådana program som en enda samling resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="c91a4-105">Azure Resource Manager enables you toocreate such applications, as a single collection of resources in resource groups.</span></span> <span data-ttu-id="c91a4-106">Dessa resurser hanteras via olika resursproviders under Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="c91a4-106">Such resources are managed through various resource providers under Resource Manager.</span></span>

<span data-ttu-id="c91a4-107">Azure Resource Manager förlitar sig på annan resurs providers tooprovide åtkomst tooyour resurser.</span><span class="sxs-lookup"><span data-stu-id="c91a4-107">Azure Resource Manager relies on different resource providers tooprovide access tooyour resources.</span></span> <span data-ttu-id="c91a4-108">Det finns tre huvudsakliga resursproviders: nätverk, lagring och beräkning.</span><span class="sxs-lookup"><span data-stu-id="c91a4-108">There are three main resource providers: Network, Storage and Compute.</span></span> <span data-ttu-id="c91a4-109">Det här dokumentet beskrivs hello egenskaper och fördelarna med hello Nätverksresursprovidern, inklusive:</span><span class="sxs-lookup"><span data-stu-id="c91a4-109">This document discusses hello characteristics and benefits of hello Network Resource Provider, including:</span></span>

* <span data-ttu-id="c91a4-110">**Metadata** – du kan lägga till information tooresources med hjälp av taggar.</span><span class="sxs-lookup"><span data-stu-id="c91a4-110">**Metadata** – you can add information tooresources using tags.</span></span> <span data-ttu-id="c91a4-111">Dessa taggar kan vara används tootrack resursutnyttjande över resursgrupper och prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="c91a4-111">These tags can be used tootrack resource utilization across resource groups and subscriptions.</span></span>
* <span data-ttu-id="c91a4-112">**Större kontroll över nätverket** - nätverksresurser är löst sammanfogade och du kan kontrollera dem på ett bättre sätt.</span><span class="sxs-lookup"><span data-stu-id="c91a4-112">**Greater control of your network** - network resources are loosely coupled and you can control them in a more granular fashion.</span></span> <span data-ttu-id="c91a4-113">Det innebär att du har mer flexibilitet för att hantera hello nätverksresurser.</span><span class="sxs-lookup"><span data-stu-id="c91a4-113">This means you have more flexibility in managing hello networking resources.</span></span>
* <span data-ttu-id="c91a4-114">**Snabbare configuration** -eftersom nätverksresurser är löst sammansatta, kan du skapa och samordna nätverksresurser parallellt.</span><span class="sxs-lookup"><span data-stu-id="c91a4-114">**Faster configuration** - because network resources are loosely coupled, you can create and orchestrate network resources in parallel.</span></span> <span data-ttu-id="c91a4-115">Detta har kraftigt minska konfiguration.</span><span class="sxs-lookup"><span data-stu-id="c91a4-115">This has drastically reduced configuration time.</span></span>
* <span data-ttu-id="c91a4-116">**Rollbaserad åtkomstkontroll** -RBAC ger standardroller med specifika säkerhetsomfattning tillägg tooallowing hello att skapa anpassade roller för säker hantering.</span><span class="sxs-lookup"><span data-stu-id="c91a4-116">**Role Based Access Control** - RBAC provides default roles, with specific security scope, in addition tooallowing hello creation of custom roles for secure management.</span></span>
* <span data-ttu-id="c91a4-117">**Enklare hantering och distribution** – det är enklare toodeploy och hantera program eftersom du kan skapa en hel programstack som en enda samling resurser i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="c91a4-117">**Easier management and deployment** - it’s easier toodeploy and manage applications since you can can create an entire application stack as a single collection of resources in a resource group.</span></span> <span data-ttu-id="c91a4-118">Och snabbare toodeploy, eftersom du kan distribuera genom att bara ange en mall för JSON-nyttolast.</span><span class="sxs-lookup"><span data-stu-id="c91a4-118">And faster toodeploy, since you can deploy by simply providing a template JSON payload.</span></span>
* <span data-ttu-id="c91a4-119">**Snabb anpassning** -du kan använda mallar deklarativ typ tooenable repeterbara och snabb anpassning av distributioner.</span><span class="sxs-lookup"><span data-stu-id="c91a4-119">**Rapid customization** - you can use declarative-style templates tooenable repeatable and rapid customization of deployments.</span></span>
* <span data-ttu-id="c91a4-120">**Anpassning av repeterbara** -du kan använda mallar deklarativ typ tooenable repeterbara och snabb anpassning av distributioner.</span><span class="sxs-lookup"><span data-stu-id="c91a4-120">**Repeatable customization** - you can use declarative-style templates tooenable repeatable and rapid customization of deployments.</span></span>
* <span data-ttu-id="c91a4-121">**Hanteringsgränssnitt** -du kan använda någon av följande gränssnitt toomanage hello dina resurser:</span><span class="sxs-lookup"><span data-stu-id="c91a4-121">**Management interfaces** - you can use any of hello following interfaces toomanage your resources:</span></span>
  * <span data-ttu-id="c91a4-122">REST-baserad API</span><span class="sxs-lookup"><span data-stu-id="c91a4-122">REST based API</span></span>
  * <span data-ttu-id="c91a4-123">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c91a4-123">PowerShell</span></span>
  * <span data-ttu-id="c91a4-124">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="c91a4-124">.NET SDK</span></span>
  * <span data-ttu-id="c91a4-125">SDK för Node.JS</span><span class="sxs-lookup"><span data-stu-id="c91a4-125">Node.JS SDK</span></span>
  * <span data-ttu-id="c91a4-126">Java SDK</span><span class="sxs-lookup"><span data-stu-id="c91a4-126">Java SDK</span></span>
  * <span data-ttu-id="c91a4-127">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c91a4-127">Azure CLI</span></span>
  * <span data-ttu-id="c91a4-128">Preview-portalen</span><span class="sxs-lookup"><span data-stu-id="c91a4-128">Preview Portal</span></span>
  * <span data-ttu-id="c91a4-129">Språk för Resource Manager-mallen</span><span class="sxs-lookup"><span data-stu-id="c91a4-129">Resource Manager template language</span></span>

## <a name="network-resources"></a><span data-ttu-id="c91a4-130">Nätverksresurser</span><span class="sxs-lookup"><span data-stu-id="c91a4-130">Network resources</span></span>
<span data-ttu-id="c91a4-131">Du kan nu hantera nätverksresurser oberoende av varandra, i stället för att de hanteras via en enda beräknings-resurs (en virtuell dator).</span><span class="sxs-lookup"><span data-stu-id="c91a4-131">You can now manage network resources independently, instead of having them all managed through a single compute resource (a virtual machine).</span></span> <span data-ttu-id="c91a4-132">Detta innebär en högre grad av flexibilitet och smidighet i skriver en komplex och stor skala infrastruktur i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="c91a4-132">This ensures a higher degree of flexibility and agility in composing a complex and large scale infrastructure in a resource group.</span></span>

<span data-ttu-id="c91a4-133">Nedan visas en översikt över en exempeldistribution med ett program med flera nivåer.</span><span class="sxs-lookup"><span data-stu-id="c91a4-133">A conceptual view of a sample deployment involving a multi-tiered application is presented below.</span></span> <span data-ttu-id="c91a4-134">Varje resurs som du ser, till exempel nätverkskort, offentliga IP-adresser och virtuella datorer kan hanteras oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="c91a4-134">Each resource you see, such as NICs, public IP addresses, and VMs, can be managed independently.</span></span>

![Resursmodell för nätverk](./media/resource-groups-networking/Figure2.png)

<span data-ttu-id="c91a4-136">Alla resurser innehåller en gemensam uppsättning egenskaper och deras enskilda egenskapsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="c91a4-136">Every resource contains a common set of properties, and their individual property set.</span></span> <span data-ttu-id="c91a4-137">hello gemensamma egenskaper är:</span><span class="sxs-lookup"><span data-stu-id="c91a4-137">hello common properties are:</span></span>

| <span data-ttu-id="c91a4-138">Egenskap</span><span class="sxs-lookup"><span data-stu-id="c91a4-138">Property</span></span> | <span data-ttu-id="c91a4-139">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c91a4-139">Description</span></span> | <span data-ttu-id="c91a4-140">Exempelvärden</span><span class="sxs-lookup"><span data-stu-id="c91a4-140">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c91a4-141">**Namn**</span><span class="sxs-lookup"><span data-stu-id="c91a4-141">**name**</span></span> |<span data-ttu-id="c91a4-142">Unikt resursnamn.</span><span class="sxs-lookup"><span data-stu-id="c91a4-142">Unique resource name.</span></span> <span data-ttu-id="c91a4-143">Varje resurs har sin egen namngivningsbegränsningar.</span><span class="sxs-lookup"><span data-stu-id="c91a4-143">Each resource type has its own naming restrictions.</span></span> |<span data-ttu-id="c91a4-144">PIP01 VM01, NIC01</span><span class="sxs-lookup"><span data-stu-id="c91a4-144">PIP01, VM01, NIC01</span></span> |
| <span data-ttu-id="c91a4-145">**Plats**</span><span class="sxs-lookup"><span data-stu-id="c91a4-145">**location**</span></span> |<span data-ttu-id="c91a4-146">Azure-region i vilken hello resursen finns</span><span class="sxs-lookup"><span data-stu-id="c91a4-146">Azure region in which hello resource resides</span></span> |<span data-ttu-id="c91a4-147">westus eastus</span><span class="sxs-lookup"><span data-stu-id="c91a4-147">westus, eastus</span></span> |
| <span data-ttu-id="c91a4-148">**ID**</span><span class="sxs-lookup"><span data-stu-id="c91a4-148">**id**</span></span> |<span data-ttu-id="c91a4-149">Unik URI baserad identifiering</span><span class="sxs-lookup"><span data-stu-id="c91a4-149">Unique URI based identification</span></span> |<span data-ttu-id="c91a4-150">/subscriptions/<subGUID>/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP</span><span class="sxs-lookup"><span data-stu-id="c91a4-150">/subscriptions/<subGUID>/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP</span></span> |

<span data-ttu-id="c91a4-151">Du kan kontrollera hello enskilda egenskaper för resurser i hello avsnitten nedan.</span><span class="sxs-lookup"><span data-stu-id="c91a4-151">You can check hello individual properties of resources in hello sections below.</span></span>

[!INCLUDE [virtual-networks-nrp-pip-include](../../includes/virtual-networks-nrp-pip-include.md)]

[!INCLUDE [virtual-networks-nrp-nic-include](../../includes/virtual-networks-nrp-nic-include.md)]

[!INCLUDE [virtual-networks-nrp-nsg-include](../../includes/virtual-networks-nrp-nsg-include.md)]

[!INCLUDE [virtual-networks-nrp-udr-include](../../includes/virtual-networks-nrp-udr-include.md)]

[!INCLUDE [virtual-networks-nrp-vnet-include](../../includes/virtual-networks-nrp-vnet-include.md)]

[!INCLUDE [virtual-networks-nrp-dns-include](../../includes/virtual-networks-nrp-dns-include.md)]

[!INCLUDE [virtual-networks-nrp-lb-include](../../includes/virtual-networks-nrp-lb-include.md)]

[!INCLUDE [virtual-networks-nrp-appgw-include](../../includes/virtual-networks-nrp-appgw-include.md)]

[!INCLUDE [virtual-networks-nrp-vpn-include](../../includes/virtual-networks-nrp-vpn-include.md)]

[!INCLUDE [virtual-networks-nrp-tm-include](../../includes/virtual-networks-nrp-tm-include.md)]

## <a name="management-interfaces"></a><span data-ttu-id="c91a4-152">Hanteringsgränssnitt</span><span class="sxs-lookup"><span data-stu-id="c91a4-152">Management interfaces</span></span>
<span data-ttu-id="c91a4-153">Du kan hantera dina Azure-nätverk resurser med olika gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="c91a4-153">You can manage your Azure networking resources using different interfaces.</span></span> <span data-ttu-id="c91a4-154">I det här dokumentet fokuserar vi på dra av dessa gränssnitt: REST-API och mallar.</span><span class="sxs-lookup"><span data-stu-id="c91a4-154">In this document we will focus on tow of those interfaces: REST API, and templates.</span></span>

### <a name="rest-api"></a><span data-ttu-id="c91a4-155">REST API</span><span class="sxs-lookup"><span data-stu-id="c91a4-155">REST API</span></span>
<span data-ttu-id="c91a4-156">Som tidigare nämnts kan nätverksresurser hanteras via en mängd olika gränssnitt, inklusive REST API, .NET SDK, SDK för Node.JS, Java SDK, PowerShell, CLI, Azure-portalen och mallar.</span><span class="sxs-lookup"><span data-stu-id="c91a4-156">As mentioned earlier, network resources can be managed via a variety of interfaces, including REST API,.NET SDK, Node.JS SDK, Java SDK, PowerShell, CLI, Azure Portal and templates.</span></span>

<span data-ttu-id="c91a4-157">hello Rest API kraven toohello specifikationen för HTTP 1.1-protokollet.</span><span class="sxs-lookup"><span data-stu-id="c91a4-157">hello Rest API’s conform toohello HTTP 1.1 protocol specification.</span></span> <span data-ttu-id="c91a4-158">hello URI uppbyggnad i hello API visas nedan:</span><span class="sxs-lookup"><span data-stu-id="c91a4-158">hello general URI structure of hello API is presented below:</span></span>

    https://management.azure.com/subscriptions/{subscription-id}/providers/{resource-provider-namespace}/locations/{region-location}/register?api-version={api-version}

<span data-ttu-id="c91a4-159">Och hello parametrar i klammerparenteser representerar hello följande element:</span><span class="sxs-lookup"><span data-stu-id="c91a4-159">And hello parameters in braces represent hello following elements:</span></span>

* <span data-ttu-id="c91a4-160">**prenumerations-id** -id för din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c91a4-160">**subscription-id** - your Azure subscription id.</span></span>
* <span data-ttu-id="c91a4-161">**resurs-providernamnrymden** -namnområde för hello-providern som används.</span><span class="sxs-lookup"><span data-stu-id="c91a4-161">**resource-provider-namespace** - namespace for hello provider being used.</span></span> <span data-ttu-id="c91a4-162">hello-värdet för hello nätverksresursprovidern *Microsoft.Network*.</span><span class="sxs-lookup"><span data-stu-id="c91a4-162">hello value for hello network resource provider is *Microsoft.Network*.</span></span>
* <span data-ttu-id="c91a4-163">**regionsnamnet** -hello Azure-regionnamn</span><span class="sxs-lookup"><span data-stu-id="c91a4-163">**region-name** - hello Azure region name</span></span>

<span data-ttu-id="c91a4-164">hello stöds följande HTTP-metoder när du gör anrop toohello REST-API:</span><span class="sxs-lookup"><span data-stu-id="c91a4-164">hello following HTTP methods are supported when making calls toohello REST API:</span></span>

* <span data-ttu-id="c91a4-165">**PLACERA** – används toocreate en resurs som tillhör en viss typ, ändra en resursegenskap eller ändra en association mellan resurser.</span><span class="sxs-lookup"><span data-stu-id="c91a4-165">**PUT** - used toocreate a resource of a given type, modify a resource property or change an association between resources.</span></span>
* <span data-ttu-id="c91a4-166">**Hämta** -tooretrieve information som används för en etablerad resurs.</span><span class="sxs-lookup"><span data-stu-id="c91a4-166">**GET** - used tooretrieve information for a provisioned resource.</span></span>
* <span data-ttu-id="c91a4-167">**Ta bort** -används toodelete en befintlig resurs.</span><span class="sxs-lookup"><span data-stu-id="c91a4-167">**DELETE** - used toodelete an existing resource.</span></span>

<span data-ttu-id="c91a4-168">Både hello förfrågan och svar kraven tooa JSON-nyttolast-format.</span><span class="sxs-lookup"><span data-stu-id="c91a4-168">Both hello request and response conform tooa JSON payload format.</span></span> <span data-ttu-id="c91a4-169">Mer information finns i [Azure Resource Manager API: er](https://msdn.microsoft.com/library/azure/dn948464.aspx).</span><span class="sxs-lookup"><span data-stu-id="c91a4-169">For more details, see [Azure Resource Management APIs](https://msdn.microsoft.com/library/azure/dn948464.aspx).</span></span>

### <a name="resource-manager-template-language"></a><span data-ttu-id="c91a4-170">Språk för Resource Manager-mallen</span><span class="sxs-lookup"><span data-stu-id="c91a4-170">Resource Manager template language</span></span>
<span data-ttu-id="c91a4-171">I tillägg toomanaging resurser imperatively (via API: er eller SDK), du också använda en deklarativ programmering style toobuild och hantera nätverksresurser med hjälp av hello språk för Resource Manager-mallen.</span><span class="sxs-lookup"><span data-stu-id="c91a4-171">In addition toomanaging resources imperatively (via APIs or SDK), you can also use a declarative programming style toobuild and manage network resources by using hello Resource Manager Template Language.</span></span>

<span data-ttu-id="c91a4-172">En exempel-representation av en mall som har angetts under –</span><span class="sxs-lookup"><span data-stu-id="c91a4-172">A sample representation of a template is provided below –</span></span>

    {
      "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
      "contentVersion": "<version-number-of-template>",
      "parameters": { <parameter-definitions-of-template> },
      "variables": { <variable-definitions-of-template> },
      "resources": [ { <definition-of-resource-to-deploy> } ],
      "outputs": { <output-of-template> }    
    }

<span data-ttu-id="c91a4-173">hello mallen är i första hand en JSON-beskrivning av hello resurser och hello instansvärden matas in via parametrar.</span><span class="sxs-lookup"><span data-stu-id="c91a4-173">hello template is primarily a JSON description of hello resources and hello instance values injected via parameters.</span></span> <span data-ttu-id="c91a4-174">hello exemplet nedan kan vara används toocreate ett virtuellt nätverk med 2 undernät.</span><span class="sxs-lookup"><span data-stu-id="c91a4-174">hello example below can be used toocreate a virtual network with 2 subnets.</span></span>

    {
        "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/VNET.json",
        "contentVersion": "1.0.0.0",
        "parameters" : {
          "location": {
            "type": "String",
            "allowedValues": ["East US", "West US", "West Europe", "East Asia", "South East Asia"],
            "metadata" : {
              "Description" : "Deployment location"
            }
          },
          "virtualNetworkName":{
            "type" : "string",
            "defaultValue":"myVNET",
            "metadata" : {
              "Description" : "VNET name"
            }
          },
          "addressPrefix":{
            "type" : "string",
            "defaultValue" : "10.0.0.0/16",
            "metadata" : {
              "Description" : "Address prefix"
            }

          },
          "subnet1Name": {
            "type" : "string",
            "defaultValue" : "Subnet-1",
            "metadata" : {
              "Description" : "Subnet 1 Name"
            }
          },
          "subnet2Name": {
            "type" : "string",
            "defaultValue" : "Subnet-2",
            "metadata" : {
              "Description" : "Subnet 2 name"
            }
          },
          "subnet1Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.0.0/24",
            "metadata" : {
              "Description" : "Subnet 1 Prefix"
            }
          },
          "subnet2Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.1.0/24",
            "metadata" : {
              "Description" : "Subnet 2 Prefix"
            }
          }
        },
        "resources": [
        {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "[parameters('virtualNetworkName')]",
          "location": "[parameters('location')]",
          "properties": {
            "addressSpace": {
              "addressPrefixes": [
                "[parameters('addressPrefix')]"
              ]
            },
            "subnets": [
              {
                "name": "[parameters('subnet1Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet1Prefix')]"
                }
              },
              {
                "name": "[parameters('subnet2Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet2Prefix')]"
                }
              }
            ]
          }
        }
        ]
    }

<span data-ttu-id="c91a4-175">Du har hello möjlighet att ange parametervärden för hello manuellt när du använder en mall eller du kan använda en parameterfil.</span><span class="sxs-lookup"><span data-stu-id="c91a4-175">You have hello option of providing hello parameter values manually when using a template, or you can use a parameter file.</span></span> <span data-ttu-id="c91a4-176">hello exemplet nedan visar möjliga uppsättning parametern värden toobe används med hello mallen ovan:</span><span class="sxs-lookup"><span data-stu-id="c91a4-176">hello example below shows a possible set of parameter values toobe used with hello template above:</span></span>

    {
      "location": {
          "value": "East US"
      },
      "virtualNetworkName": {
          "value": "VNET1"
      },
      "subnet1Name": {
          "value": "Subnet1"
      },
      "subnet2Name": {
          "value": "Subnet2"
      },
      "addressPrefix": {
          "value": "192.168.0.0/16"
      },
      "subnet1Prefix": {
          "value": "192.168.1.0/24"
      },
      "subnet2Prefix": {
          "value": "192.168.2.0/24"
      }
    }


<span data-ttu-id="c91a4-177">hello huvudsakliga fördelarna med hjälp av mallar är:</span><span class="sxs-lookup"><span data-stu-id="c91a4-177">hello main advantages of using templates are:</span></span>

* <span data-ttu-id="c91a4-178">Du kan skapa en komplicerad infrastruktur i en resursgrupp i en deklarativ stil.</span><span class="sxs-lookup"><span data-stu-id="c91a4-178">You can build a complex infrastructure in a resource group in a declarative style.</span></span> <span data-ttu-id="c91a4-179">hello hanteras orchestration skapar hello resurser, inklusive beroende management av Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="c91a4-179">hello orchestration of creating hello resources, including dependency management, is handled by Resource Manager.</span></span>
* <span data-ttu-id="c91a4-180">hello infrastruktur kan skapas på repeterbara sätt över olika regioner, och inom en region genom att ändra parametrar.</span><span class="sxs-lookup"><span data-stu-id="c91a4-180">hello infrastructure can be created in a repeatable way across various regions and within a region by simply changing parameters.</span></span>
* <span data-ttu-id="c91a4-181">hello deklarativ format leder tooshorter ledtid i bygga hello mallar och lansera hello infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="c91a4-181">hello declarative style leads tooshorter lead time in building hello templates and rolling out hello infrastructure.</span></span>

<span data-ttu-id="c91a4-182">Exempelmallar, se [Azure snabbstartsmallar](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="c91a4-182">For sample templates, see [Azure quickstart templates](https://github.com/Azure/azure-quickstart-templates).</span></span>

<span data-ttu-id="c91a4-183">Mer information om hello språk för Resource Manager-mallen finns [språk för Azure Resource Manager-mallen](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="c91a4-183">For more information on hello Resource Manager Template Language, see [Azure Resource Manager Template Language](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="c91a4-184">hello exempelmall ovan använder hello virtuella nätverk och nätverksresurser på undernätet.</span><span class="sxs-lookup"><span data-stu-id="c91a4-184">hello sample template above uses hello virtual network and subnet resources.</span></span> <span data-ttu-id="c91a4-185">Det finns andra nätverksresurser som du kan använda enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="c91a4-185">There are other network resources you can use as listed below:</span></span>

### <a name="using-a-template"></a><span data-ttu-id="c91a4-186">Använda en mall</span><span class="sxs-lookup"><span data-stu-id="c91a4-186">Using a template</span></span>
<span data-ttu-id="c91a4-187">Du kan distribuera tjänster tooAzure från en mall med hjälp av PowerShell AzureCLI, eller genom att utföra en Klicka toodeploy från GitHub.</span><span class="sxs-lookup"><span data-stu-id="c91a4-187">You can deploy services tooAzure from a template by using PowerShell, AzureCLI, or by performing a click toodeploy from GitHub.</span></span> <span data-ttu-id="c91a4-188">toodeploy tjänster från en mall i GitHub, köra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="c91a4-188">toodeploy services from a template in GitHub, execute hello following steps:</span></span>

1. <span data-ttu-id="c91a4-189">Öppna hello template3 filen från GitHub.</span><span class="sxs-lookup"><span data-stu-id="c91a4-189">Open hello template3 file from GitHub.</span></span> <span data-ttu-id="c91a4-190">Exempelvis öppna [virtuella nätverk med två undernät](https://github.com/Azure/azure-quickstart-templates/tree/master/101-virtual-network).</span><span class="sxs-lookup"><span data-stu-id="c91a4-190">As an example, open [Virtual network with two subnets](https://github.com/Azure/azure-quickstart-templates/tree/master/101-virtual-network).</span></span>
2. <span data-ttu-id="c91a4-191">Klicka på **distribuera tooAzure**, och sedan logga in på toohello Azure-portalen med dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="c91a4-191">Click on **Deploy tooAzure**, and then sign in on toohello Azure portal with your credentials.</span></span>
3. <span data-ttu-id="c91a4-192">Verifiera hello mallen och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="c91a4-192">Verify hello template, and then click **Save**.</span></span>
4. <span data-ttu-id="c91a4-193">Klicka på **redigera parametrar** och välja en plats, till exempel *västra USA*, för hello vnet och undernät.</span><span class="sxs-lookup"><span data-stu-id="c91a4-193">Click **Edit parameters** and select a location, such as *West US*, for hello vnet and subnets.</span></span>
5. <span data-ttu-id="c91a4-194">Om det behövs ändrar hello **ADDRESSPREFIX** och **SUBNETPREFIX** parametrar och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c91a4-194">If necessary, change hello **ADDRESSPREFIX** and **SUBNETPREFIX** parameters, and then click **OK**.</span></span>
6. <span data-ttu-id="c91a4-195">Klicka på **välja en resursgrupp** och klicka sedan på hello resursgrupp tooadd hello vnet och undernät med.</span><span class="sxs-lookup"><span data-stu-id="c91a4-195">Click **Select a resource group** and then click on hello resource group you want tooadd hello vnet and subnets to.</span></span> <span data-ttu-id="c91a4-196">Du kan också skapa en ny resursgrupp genom att klicka på **eller skapa nya**.</span><span class="sxs-lookup"><span data-stu-id="c91a4-196">Alternatively, you can create a new resource group by clicking **Or create new**.</span></span>
7. <span data-ttu-id="c91a4-197">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="c91a4-197">Click **Create**.</span></span> <span data-ttu-id="c91a4-198">Lägg märke till hello panelen Visa **etablering malldistribution**.</span><span class="sxs-lookup"><span data-stu-id="c91a4-198">Notice hello tile displaying **Provisioning Template deployment**.</span></span> <span data-ttu-id="c91a4-199">När hello distributionen är klar, visas en skärm liknande tooone nedan.</span><span class="sxs-lookup"><span data-stu-id="c91a4-199">Once hello deployment is done, you will see a screen similar tooone below.</span></span>

![Exempel malldistribution](./media/resource-groups-networking/Figure6.png)

## <a name="next-steps"></a><span data-ttu-id="c91a4-201">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c91a4-201">Next steps</span></span>
[<span data-ttu-id="c91a4-202">Språk för Azure Resource Manager-mallen</span><span class="sxs-lookup"><span data-stu-id="c91a4-202">Azure Resource Manager Template Language</span></span>](../azure-resource-manager/resource-group-authoring-templates.md)

[<span data-ttu-id="c91a4-203">Azure-nätverk – vanliga mallar</span><span class="sxs-lookup"><span data-stu-id="c91a4-203">Azure Networking – commonly used templates</span></span>](https://github.com/Azure/azure-quickstart-templates)

[<span data-ttu-id="c91a4-204">Azure Resource Manager och klassisk distribution</span><span class="sxs-lookup"><span data-stu-id="c91a4-204">Azure Resource Manager vs. classic deployment</span></span>](../azure-resource-manager/resource-manager-deployment-model.md)

[<span data-ttu-id="c91a4-205">Översikt över Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="c91a4-205">Azure Resource Manager Overview</span></span>](../azure-resource-manager/resource-group-overview.md)


---
title: "Resurs-providern: översikt | Microsoft Docs"
description: "Lär dig mer om den nya Nätverksresursprovidern i Azure Resource Manager"
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
ms.openlocfilehash: 2428c707ddeed281fddd1e57bc5574603f0b9b1c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="network-resource-provider"></a><span data-ttu-id="3d26b-103">Provider för nätverksresurser</span><span class="sxs-lookup"><span data-stu-id="3d26b-103">Network Resource Provider</span></span>
<span data-ttu-id="3d26b-104">Behovet av en gäller i dagens företag lyckade är möjligheten att bygga och hantera anpassade program i stor skala nätverk på ett flexibelt, flexibel, säker och repeterbara sätt.</span><span class="sxs-lookup"><span data-stu-id="3d26b-104">An underpinning need in today’s business success, is the ability to build and manage large scale network aware applications in an agile, flexible, secure and repeatable way.</span></span> <span data-ttu-id="3d26b-105">Azure Resource Manager kan du skapa sådana program som en enda samling resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="3d26b-105">Azure Resource Manager enables you to create such applications, as a single collection of resources in resource groups.</span></span> <span data-ttu-id="3d26b-106">Dessa resurser hanteras via olika resursproviders under Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="3d26b-106">Such resources are managed through various resource providers under Resource Manager.</span></span>

<span data-ttu-id="3d26b-107">Azure Resource Manager förlitar sig på olika resursproviders att ge åtkomst till resurser.</span><span class="sxs-lookup"><span data-stu-id="3d26b-107">Azure Resource Manager relies on different resource providers to provide access to your resources.</span></span> <span data-ttu-id="3d26b-108">Det finns tre huvudsakliga resursproviders: nätverk, lagring och beräkning.</span><span class="sxs-lookup"><span data-stu-id="3d26b-108">There are three main resource providers: Network, Storage and Compute.</span></span> <span data-ttu-id="3d26b-109">Det här dokumentet beskriver egenskaper och fördelarna med Nätverksresursprovidern, inklusive:</span><span class="sxs-lookup"><span data-stu-id="3d26b-109">This document discusses the characteristics and benefits of the Network Resource Provider, including:</span></span>

* <span data-ttu-id="3d26b-110">**Metadata** – du kan lägga till information till resurser med hjälp av taggar.</span><span class="sxs-lookup"><span data-stu-id="3d26b-110">**Metadata** – you can add information to resources using tags.</span></span> <span data-ttu-id="3d26b-111">Dessa taggar kan användas för att spåra resursutnyttjande alla resursgrupper och prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="3d26b-111">These tags can be used to track resource utilization across resource groups and subscriptions.</span></span>
* <span data-ttu-id="3d26b-112">**Större kontroll över nätverket** - nätverksresurser är löst sammanfogade och du kan kontrollera dem på ett bättre sätt.</span><span class="sxs-lookup"><span data-stu-id="3d26b-112">**Greater control of your network** - network resources are loosely coupled and you can control them in a more granular fashion.</span></span> <span data-ttu-id="3d26b-113">Det innebär att du har mer flexibilitet för att hantera nätverksresurser.</span><span class="sxs-lookup"><span data-stu-id="3d26b-113">This means you have more flexibility in managing the networking resources.</span></span>
* <span data-ttu-id="3d26b-114">**Snabbare configuration** -eftersom nätverksresurser är löst sammansatta, kan du skapa och samordna nätverksresurser parallellt.</span><span class="sxs-lookup"><span data-stu-id="3d26b-114">**Faster configuration** - because network resources are loosely coupled, you can create and orchestrate network resources in parallel.</span></span> <span data-ttu-id="3d26b-115">Detta har kraftigt minska konfiguration.</span><span class="sxs-lookup"><span data-stu-id="3d26b-115">This has drastically reduced configuration time.</span></span>
* <span data-ttu-id="3d26b-116">**Rollbaserad åtkomstkontroll** -RBAC ger standardroller med specifika säkerhetsomfattning, förutom att tillåta skapandet av anpassade roller för säker hantering.</span><span class="sxs-lookup"><span data-stu-id="3d26b-116">**Role Based Access Control** - RBAC provides default roles, with specific security scope, in addition to allowing the creation of custom roles for secure management.</span></span>
* <span data-ttu-id="3d26b-117">**Enklare hantering och distribution** – det är enklare att distribuera och hantera program eftersom du kan skapa en hel programstack som en enda samling resurser i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="3d26b-117">**Easier management and deployment** - it’s easier to deploy and manage applications since you can can create an entire application stack as a single collection of resources in a resource group.</span></span> <span data-ttu-id="3d26b-118">Och snabbare att distribuera, eftersom du kan distribuera genom att bara ange en mall för JSON-nyttolast.</span><span class="sxs-lookup"><span data-stu-id="3d26b-118">And faster to deploy, since you can deploy by simply providing a template JSON payload.</span></span>
* <span data-ttu-id="3d26b-119">**Snabb anpassning** -du kan använda mallar deklarativ typ för att aktivera repeterbara och snabb anpassning av distributioner.</span><span class="sxs-lookup"><span data-stu-id="3d26b-119">**Rapid customization** - you can use declarative-style templates to enable repeatable and rapid customization of deployments.</span></span>
* <span data-ttu-id="3d26b-120">**Anpassning av repeterbara** -du kan använda mallar deklarativ typ för att aktivera repeterbara och snabb anpassning av distributioner.</span><span class="sxs-lookup"><span data-stu-id="3d26b-120">**Repeatable customization** - you can use declarative-style templates to enable repeatable and rapid customization of deployments.</span></span>
* <span data-ttu-id="3d26b-121">**Hanteringsgränssnitt** -du kan använda någon av följande gränssnitt för att hantera dina resurser:</span><span class="sxs-lookup"><span data-stu-id="3d26b-121">**Management interfaces** - you can use any of the following interfaces to manage your resources:</span></span>
  * <span data-ttu-id="3d26b-122">REST-baserad API</span><span class="sxs-lookup"><span data-stu-id="3d26b-122">REST based API</span></span>
  * <span data-ttu-id="3d26b-123">PowerShell</span><span class="sxs-lookup"><span data-stu-id="3d26b-123">PowerShell</span></span>
  * <span data-ttu-id="3d26b-124">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="3d26b-124">.NET SDK</span></span>
  * <span data-ttu-id="3d26b-125">SDK för Node.JS</span><span class="sxs-lookup"><span data-stu-id="3d26b-125">Node.JS SDK</span></span>
  * <span data-ttu-id="3d26b-126">Java SDK</span><span class="sxs-lookup"><span data-stu-id="3d26b-126">Java SDK</span></span>
  * <span data-ttu-id="3d26b-127">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="3d26b-127">Azure CLI</span></span>
  * <span data-ttu-id="3d26b-128">Preview-portalen</span><span class="sxs-lookup"><span data-stu-id="3d26b-128">Preview Portal</span></span>
  * <span data-ttu-id="3d26b-129">Språk för Resource Manager-mallen</span><span class="sxs-lookup"><span data-stu-id="3d26b-129">Resource Manager template language</span></span>

## <a name="network-resources"></a><span data-ttu-id="3d26b-130">Nätverksresurser</span><span class="sxs-lookup"><span data-stu-id="3d26b-130">Network resources</span></span>
<span data-ttu-id="3d26b-131">Du kan nu hantera nätverksresurser oberoende av varandra, i stället för att de hanteras via en enda beräknings-resurs (en virtuell dator).</span><span class="sxs-lookup"><span data-stu-id="3d26b-131">You can now manage network resources independently, instead of having them all managed through a single compute resource (a virtual machine).</span></span> <span data-ttu-id="3d26b-132">Detta innebär en högre grad av flexibilitet och smidighet i skriver en komplex och stor skala infrastruktur i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="3d26b-132">This ensures a higher degree of flexibility and agility in composing a complex and large scale infrastructure in a resource group.</span></span>

<span data-ttu-id="3d26b-133">Nedan visas en översikt över en exempeldistribution med ett program med flera nivåer.</span><span class="sxs-lookup"><span data-stu-id="3d26b-133">A conceptual view of a sample deployment involving a multi-tiered application is presented below.</span></span> <span data-ttu-id="3d26b-134">Varje resurs som du ser, till exempel nätverkskort, offentliga IP-adresser och virtuella datorer kan hanteras oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="3d26b-134">Each resource you see, such as NICs, public IP addresses, and VMs, can be managed independently.</span></span>

![Resursmodell för nätverk](./media/resource-groups-networking/Figure2.png)

<span data-ttu-id="3d26b-136">Alla resurser innehåller en gemensam uppsättning egenskaper och deras enskilda egenskapsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="3d26b-136">Every resource contains a common set of properties, and their individual property set.</span></span> <span data-ttu-id="3d26b-137">Gemensamma egenskaper är:</span><span class="sxs-lookup"><span data-stu-id="3d26b-137">The common properties are:</span></span>

| <span data-ttu-id="3d26b-138">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3d26b-138">Property</span></span> | <span data-ttu-id="3d26b-139">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3d26b-139">Description</span></span> | <span data-ttu-id="3d26b-140">Exempelvärden</span><span class="sxs-lookup"><span data-stu-id="3d26b-140">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3d26b-141">**Namn**</span><span class="sxs-lookup"><span data-stu-id="3d26b-141">**name**</span></span> |<span data-ttu-id="3d26b-142">Unikt resursnamn.</span><span class="sxs-lookup"><span data-stu-id="3d26b-142">Unique resource name.</span></span> <span data-ttu-id="3d26b-143">Varje resurs har sin egen namngivningsbegränsningar.</span><span class="sxs-lookup"><span data-stu-id="3d26b-143">Each resource type has its own naming restrictions.</span></span> |<span data-ttu-id="3d26b-144">PIP01 VM01, NIC01</span><span class="sxs-lookup"><span data-stu-id="3d26b-144">PIP01, VM01, NIC01</span></span> |
| <span data-ttu-id="3d26b-145">**Plats**</span><span class="sxs-lookup"><span data-stu-id="3d26b-145">**location**</span></span> |<span data-ttu-id="3d26b-146">Azure-region där resursen finns</span><span class="sxs-lookup"><span data-stu-id="3d26b-146">Azure region in which the resource resides</span></span> |<span data-ttu-id="3d26b-147">westus eastus</span><span class="sxs-lookup"><span data-stu-id="3d26b-147">westus, eastus</span></span> |
| <span data-ttu-id="3d26b-148">**ID**</span><span class="sxs-lookup"><span data-stu-id="3d26b-148">**id**</span></span> |<span data-ttu-id="3d26b-149">Unik URI baserad identifiering</span><span class="sxs-lookup"><span data-stu-id="3d26b-149">Unique URI based identification</span></span> |<span data-ttu-id="3d26b-150">/subscriptions/<subGUID>/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP</span><span class="sxs-lookup"><span data-stu-id="3d26b-150">/subscriptions/<subGUID>/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP</span></span> |

<span data-ttu-id="3d26b-151">Du kan kontrollera enskilda egenskaper för resurserna i avsnitten nedan.</span><span class="sxs-lookup"><span data-stu-id="3d26b-151">You can check the individual properties of resources in the sections below.</span></span>

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

## <a name="management-interfaces"></a><span data-ttu-id="3d26b-152">Hanteringsgränssnitt</span><span class="sxs-lookup"><span data-stu-id="3d26b-152">Management interfaces</span></span>
<span data-ttu-id="3d26b-153">Du kan hantera dina Azure-nätverk resurser med olika gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="3d26b-153">You can manage your Azure networking resources using different interfaces.</span></span> <span data-ttu-id="3d26b-154">I det här dokumentet fokuserar vi på dra av dessa gränssnitt: REST-API och mallar.</span><span class="sxs-lookup"><span data-stu-id="3d26b-154">In this document we will focus on tow of those interfaces: REST API, and templates.</span></span>

### <a name="rest-api"></a><span data-ttu-id="3d26b-155">REST API</span><span class="sxs-lookup"><span data-stu-id="3d26b-155">REST API</span></span>
<span data-ttu-id="3d26b-156">Som tidigare nämnts kan nätverksresurser hanteras via en mängd olika gränssnitt, inklusive REST API, .NET SDK, SDK för Node.JS, Java SDK, PowerShell, CLI, Azure-portalen och mallar.</span><span class="sxs-lookup"><span data-stu-id="3d26b-156">As mentioned earlier, network resources can be managed via a variety of interfaces, including REST API,.NET SDK, Node.JS SDK, Java SDK, PowerShell, CLI, Azure Portal and templates.</span></span>

<span data-ttu-id="3d26b-157">Rest-API som stämmer överens med specifikationen för HTTP 1.1-protokollet.</span><span class="sxs-lookup"><span data-stu-id="3d26b-157">The Rest API’s conform to the HTTP 1.1 protocol specification.</span></span> <span data-ttu-id="3d26b-158">Den allmänna strukturen URI för API: et visas nedan:</span><span class="sxs-lookup"><span data-stu-id="3d26b-158">The general URI structure of the API is presented below:</span></span>

    https://management.azure.com/subscriptions/{subscription-id}/providers/{resource-provider-namespace}/locations/{region-location}/register?api-version={api-version}

<span data-ttu-id="3d26b-159">Och parametrar i klammerparenteser representerar följande element:</span><span class="sxs-lookup"><span data-stu-id="3d26b-159">And the parameters in braces represent the following elements:</span></span>

* <span data-ttu-id="3d26b-160">**prenumerations-id** -id för din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3d26b-160">**subscription-id** - your Azure subscription id.</span></span>
* <span data-ttu-id="3d26b-161">**resurs-providernamnrymden** -namnrymd för den leverantör som används.</span><span class="sxs-lookup"><span data-stu-id="3d26b-161">**resource-provider-namespace** - namespace for the provider being used.</span></span> <span data-ttu-id="3d26b-162">Värdet för nätverksresursprovidern *Microsoft.Network*.</span><span class="sxs-lookup"><span data-stu-id="3d26b-162">THe value for the network resource provider is *Microsoft.Network*.</span></span>
* <span data-ttu-id="3d26b-163">**regionsnamnet** -namnet för Azure-region</span><span class="sxs-lookup"><span data-stu-id="3d26b-163">**region-name** - the Azure region name</span></span>

<span data-ttu-id="3d26b-164">Följande HTTP-metoder stöds när du anropar REST-API:</span><span class="sxs-lookup"><span data-stu-id="3d26b-164">The following HTTP methods are supported when making calls to the REST API:</span></span>

* <span data-ttu-id="3d26b-165">**PLACERA** – används för att skapa en resurs som tillhör en viss typ, ändra en resursegenskap eller ändra en association mellan resurser.</span><span class="sxs-lookup"><span data-stu-id="3d26b-165">**PUT** - used to create a resource of a given type, modify a resource property or change an association between resources.</span></span>
* <span data-ttu-id="3d26b-166">**Hämta** – används för att hämta information för en etablerad resurs.</span><span class="sxs-lookup"><span data-stu-id="3d26b-166">**GET** - used to retrieve information for a provisioned resource.</span></span>
* <span data-ttu-id="3d26b-167">**Ta bort** – används för att ta bort en befintlig resurs.</span><span class="sxs-lookup"><span data-stu-id="3d26b-167">**DELETE** - used to delete an existing resource.</span></span>

<span data-ttu-id="3d26b-168">Både begäran och svar uppfylla en JSON-nyttolast-format.</span><span class="sxs-lookup"><span data-stu-id="3d26b-168">Both the request and response conform to a JSON payload format.</span></span> <span data-ttu-id="3d26b-169">Mer information finns i [Azure Resource Manager API: er](https://msdn.microsoft.com/library/azure/dn948464.aspx).</span><span class="sxs-lookup"><span data-stu-id="3d26b-169">For more details, see [Azure Resource Management APIs](https://msdn.microsoft.com/library/azure/dn948464.aspx).</span></span>

### <a name="resource-manager-template-language"></a><span data-ttu-id="3d26b-170">Språk för Resource Manager-mallen</span><span class="sxs-lookup"><span data-stu-id="3d26b-170">Resource Manager template language</span></span>
<span data-ttu-id="3d26b-171">Du kan också använda en deklarativ programmering stil att bygga och hantera nätverksresurser med hjälp av hanteraren för filserverresurser mallen språk utöver att hantera resurser imperatively (via API: er eller SDK).</span><span class="sxs-lookup"><span data-stu-id="3d26b-171">In addition to managing resources imperatively (via APIs or SDK), you can also use a declarative programming style to build and manage network resources by using the Resource Manager Template Language.</span></span>

<span data-ttu-id="3d26b-172">En exempel-representation av en mall som har angetts under –</span><span class="sxs-lookup"><span data-stu-id="3d26b-172">A sample representation of a template is provided below –</span></span>

    {
      "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
      "contentVersion": "<version-number-of-template>",
      "parameters": { <parameter-definitions-of-template> },
      "variables": { <variable-definitions-of-template> },
      "resources": [ { <definition-of-resource-to-deploy> } ],
      "outputs": { <output-of-template> }    
    }

<span data-ttu-id="3d26b-173">Mallen är i första hand en JSON-beskrivning av resurserna och instans värdena matas in via parametrar.</span><span class="sxs-lookup"><span data-stu-id="3d26b-173">The template is primarily a JSON description of the resources and the instance values injected via parameters.</span></span> <span data-ttu-id="3d26b-174">Exemplet nedan kan användas för att skapa ett virtuellt nätverk med 2 undernät.</span><span class="sxs-lookup"><span data-stu-id="3d26b-174">The example below can be used to create a virtual network with 2 subnets.</span></span>

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

<span data-ttu-id="3d26b-175">Du har möjlighet att ange parametervärden manuellt när du använder en mall eller du kan använda en parameterfil.</span><span class="sxs-lookup"><span data-stu-id="3d26b-175">You have the option of providing the parameter values manually when using a template, or you can use a parameter file.</span></span> <span data-ttu-id="3d26b-176">Exemplet nedan visar möjliga uppsättning parametervärden som ska användas med hjälp av mallen ovan:</span><span class="sxs-lookup"><span data-stu-id="3d26b-176">The example below shows a possible set of parameter values to be used with the template above:</span></span>

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


<span data-ttu-id="3d26b-177">De huvudsakliga fördelarna med hjälp av mallar är:</span><span class="sxs-lookup"><span data-stu-id="3d26b-177">The main advantages of using templates are:</span></span>

* <span data-ttu-id="3d26b-178">Du kan skapa en komplicerad infrastruktur i en resursgrupp i en deklarativ stil.</span><span class="sxs-lookup"><span data-stu-id="3d26b-178">You can build a complex infrastructure in a resource group in a declarative style.</span></span> <span data-ttu-id="3d26b-179">Dirigering av skapar resurser, inklusive beroende management hanteras av Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="3d26b-179">The orchestration of creating the resources, including dependency management, is handled by Resource Manager.</span></span>
* <span data-ttu-id="3d26b-180">Infrastrukturen kan skapas på repeterbara sätt över olika regioner, och inom en region genom att ändra parametrar.</span><span class="sxs-lookup"><span data-stu-id="3d26b-180">The infrastructure can be created in a repeatable way across various regions and within a region by simply changing parameters.</span></span>
* <span data-ttu-id="3d26b-181">Stilen deklarativ leder till kortare tid i Skapa mallar och lansera infrastrukturen.</span><span class="sxs-lookup"><span data-stu-id="3d26b-181">The declarative style leads to shorter lead time in building the templates and rolling out the infrastructure.</span></span>

<span data-ttu-id="3d26b-182">Exempelmallar, se [Azure snabbstartsmallar](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="3d26b-182">For sample templates, see [Azure quickstart templates](https://github.com/Azure/azure-quickstart-templates).</span></span>

<span data-ttu-id="3d26b-183">Mer information om språk för Resource Manager-mallen finns [språk för Azure Resource Manager-mallen](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="3d26b-183">For more information on the Resource Manager Template Language, see [Azure Resource Manager Template Language](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="3d26b-184">Mallen exempel använder virtuella nätverk och nätverksresurser på undernätet.</span><span class="sxs-lookup"><span data-stu-id="3d26b-184">The sample template above uses the virtual network and subnet resources.</span></span> <span data-ttu-id="3d26b-185">Det finns andra nätverksresurser som du kan använda enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="3d26b-185">There are other network resources you can use as listed below:</span></span>

### <a name="using-a-template"></a><span data-ttu-id="3d26b-186">Använda en mall</span><span class="sxs-lookup"><span data-stu-id="3d26b-186">Using a template</span></span>
<span data-ttu-id="3d26b-187">Du kan distribuera tjänster till Azure från en mall med hjälp av PowerShell AzureCLI, eller genom att utföra en Klicka om du vill distribuera från GitHub.</span><span class="sxs-lookup"><span data-stu-id="3d26b-187">You can deploy services to Azure from a template by using PowerShell, AzureCLI, or by performing a click to deploy from GitHub.</span></span> <span data-ttu-id="3d26b-188">Utför följande steg för att distribuera tjänster från en mall i GitHub:</span><span class="sxs-lookup"><span data-stu-id="3d26b-188">To deploy services from a template in GitHub, execute the following steps:</span></span>

1. <span data-ttu-id="3d26b-189">Öppna filen template3 från GitHub.</span><span class="sxs-lookup"><span data-stu-id="3d26b-189">Open the template3 file from GitHub.</span></span> <span data-ttu-id="3d26b-190">Exempelvis öppna [virtuella nätverk med två undernät](https://github.com/Azure/azure-quickstart-templates/tree/master/101-virtual-network).</span><span class="sxs-lookup"><span data-stu-id="3d26b-190">As an example, open [Virtual network with two subnets](https://github.com/Azure/azure-quickstart-templates/tree/master/101-virtual-network).</span></span>
2. <span data-ttu-id="3d26b-191">Klicka på **till Azure**, och sedan logga in Azure-portalen med dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="3d26b-191">Click on **Deploy to Azure**, and then sign in on to the Azure portal with your credentials.</span></span>
3. <span data-ttu-id="3d26b-192">Kontrollera mallen och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="3d26b-192">Verify the template, and then click **Save**.</span></span>
4. <span data-ttu-id="3d26b-193">Klicka på **redigera parametrar** och välja en plats, till exempel *västra USA*, för vnet och undernät.</span><span class="sxs-lookup"><span data-stu-id="3d26b-193">Click **Edit parameters** and select a location, such as *West US*, for the vnet and subnets.</span></span>
5. <span data-ttu-id="3d26b-194">Om det behövs ändrar den **ADDRESSPREFIX** och **SUBNETPREFIX** parametrar och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="3d26b-194">If necessary, change the **ADDRESSPREFIX** and **SUBNETPREFIX** parameters, and then click **OK**.</span></span>
6. <span data-ttu-id="3d26b-195">Klicka på **välja en resursgrupp** och klicka sedan på den resursgrupp som du vill lägga till vnet och undernät med.</span><span class="sxs-lookup"><span data-stu-id="3d26b-195">Click **Select a resource group** and then click on the resource group you want to add the vnet and subnets to.</span></span> <span data-ttu-id="3d26b-196">Du kan också skapa en ny resursgrupp genom att klicka på **eller skapa nya**.</span><span class="sxs-lookup"><span data-stu-id="3d26b-196">Alternatively, you can create a new resource group by clicking **Or create new**.</span></span>
7. <span data-ttu-id="3d26b-197">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="3d26b-197">Click **Create**.</span></span> <span data-ttu-id="3d26b-198">Lägg märke till panelen Visa **etablering malldistribution**.</span><span class="sxs-lookup"><span data-stu-id="3d26b-198">Notice the tile displaying **Provisioning Template deployment**.</span></span> <span data-ttu-id="3d26b-199">När distributionen är klar visas en skärm som liknar en nedan.</span><span class="sxs-lookup"><span data-stu-id="3d26b-199">Once the deployment is done, you will see a screen similar to one below.</span></span>

![Exempel malldistribution](./media/resource-groups-networking/Figure6.png)

## <a name="next-steps"></a><span data-ttu-id="3d26b-201">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3d26b-201">Next steps</span></span>
[<span data-ttu-id="3d26b-202">Språk för Azure Resource Manager-mallen</span><span class="sxs-lookup"><span data-stu-id="3d26b-202">Azure Resource Manager Template Language</span></span>](../azure-resource-manager/resource-group-authoring-templates.md)

[<span data-ttu-id="3d26b-203">Azure-nätverk – vanliga mallar</span><span class="sxs-lookup"><span data-stu-id="3d26b-203">Azure Networking – commonly used templates</span></span>](https://github.com/Azure/azure-quickstart-templates)

[<span data-ttu-id="3d26b-204">Azure Resource Manager och klassisk distribution</span><span class="sxs-lookup"><span data-stu-id="3d26b-204">Azure Resource Manager vs. classic deployment</span></span>](../azure-resource-manager/resource-manager-deployment-model.md)

[<span data-ttu-id="3d26b-205">Översikt över Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="3d26b-205">Azure Resource Manager Overview</span></span>](../azure-resource-manager/resource-group-overview.md)


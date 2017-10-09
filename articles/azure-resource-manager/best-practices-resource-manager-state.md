---
title: "aaaPass komplexa värden mellan Azure-mallar | Microsoft Docs"
description: "Visar rekommenderade metoder för att använda komplexa objekt tooshare tillståndsdata med Azure Resource Manager-mallar och länkade mallar."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: fd2f5e2d-d56d-4e01-a57d-34f3eaead4a9
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2016
ms.author: tomfitz
ms.openlocfilehash: 72df1dee351446cea6ce15269e6db288b1f1db79
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="share-state-tooand-from-azure-resource-manager-templates"></a><span data-ttu-id="4cf76-103">Dela tillstånd tooand från Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="4cf76-103">Share state tooand from Azure Resource Manager templates</span></span>
<span data-ttu-id="4cf76-104">Det här avsnittet innehåller metodtips för att hantera och dela tillstånd i mallar.</span><span class="sxs-lookup"><span data-stu-id="4cf76-104">This topic shows best practices for managing and sharing state within templates.</span></span> <span data-ttu-id="4cf76-105">hello parametrar och variabler som visas i det här avsnittet är exempel på hello typen av objekt som du kan definiera tooconveniently ordna dina distributionskrav.</span><span class="sxs-lookup"><span data-stu-id="4cf76-105">hello parameters and variables shown in this topic are examples of hello type of objects you can define tooconveniently organize your deployment requirements.</span></span> <span data-ttu-id="4cf76-106">Du kan implementera objekten med egenskapsvärden som passar din miljö i dessa exempel.</span><span class="sxs-lookup"><span data-stu-id="4cf76-106">From these examples, you can implement your own objects with property values that make sense for your environment.</span></span>

<span data-ttu-id="4cf76-107">Det här avsnittet är en del av en större vitbok.</span><span class="sxs-lookup"><span data-stu-id="4cf76-107">This topic is part of a larger whitepaper.</span></span> <span data-ttu-id="4cf76-108">Hämta tooread hello fullständig papper [World klassen Resource Manager-mallar överväganden och beprövade metoder](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).</span><span class="sxs-lookup"><span data-stu-id="4cf76-108">tooread hello full paper, download [World Class Resource Manager Templates Considerations and Proven Practices](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).</span></span>

## <a name="provide-standard-configuration-settings"></a><span data-ttu-id="4cf76-109">Ange standard konfigurationsinställningar</span><span class="sxs-lookup"><span data-stu-id="4cf76-109">Provide standard configuration settings</span></span>
<span data-ttu-id="4cf76-110">I stället för att erbjuda en mall som ger totalt flexibilitet och oräkneliga variationer är ett vanligt mönster tooprovide ett urval av kända konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="4cf76-110">Rather than offer a template that provides total flexibility and countless variations, a common pattern is tooprovide a selection of known configurations.</span></span> <span data-ttu-id="4cf76-111">Användare kan i praktiken välja standard t-shirt storlekar, till exempel sandbox små, medelstora och stora.</span><span class="sxs-lookup"><span data-stu-id="4cf76-111">In effect, users can select standard t-shirt sizes such as sandbox, small, medium, and large.</span></span> <span data-ttu-id="4cf76-112">Andra exempel på t-shirt storlekar är produkterbjudanden, till exempel community edition eller enterprise edition.</span><span class="sxs-lookup"><span data-stu-id="4cf76-112">Other examples of t-shirt sizes are product offerings, such as community edition or enterprise edition.</span></span> <span data-ttu-id="4cf76-113">I andra fall kan det vara belastningsspecifikt konfigurationer av en teknik – som kartan minska eller utan sql.</span><span class="sxs-lookup"><span data-stu-id="4cf76-113">In other cases, it may be workload-specific configurations of a technology – such as map reduce or no sql.</span></span>

<span data-ttu-id="4cf76-114">Du kan skapa variabler som innehåller datasamlingar, kallas ibland för ”egenskapsuppsättningar” och använda data toodrive hello resursdeklarationen i mallen med komplexa objekt.</span><span class="sxs-lookup"><span data-stu-id="4cf76-114">With complex objects, you can create variables that contain collections of data, sometimes known as "property bags" and use that data toodrive hello resource declaration in your template.</span></span> <span data-ttu-id="4cf76-115">Den här metoden ger bra, kända konfigurationer i olika storlekar förkonfigurerade för kunder.</span><span class="sxs-lookup"><span data-stu-id="4cf76-115">This approach provides good, known configurations of varying sizes that are preconfigured for customers.</span></span> <span data-ttu-id="4cf76-116">Utan kända konfigurationer användare av hello mall måste fastställa klustret storlek på sina egna, faktor plattform resurs begränsningar och gör matematiska tooidentify hello resulterande partitionering av lagringskonton och andra resurser (på grund av toocluster storlek och resurs-begränsningar).</span><span class="sxs-lookup"><span data-stu-id="4cf76-116">Without known configurations, users of hello template must determine cluster sizing on their own, factor in platform resource constraints, and do math tooidentify hello resulting partitioning of storage accounts and other resources (due toocluster size and resource constraints).</span></span> <span data-ttu-id="4cf76-117">Dessutom toomaking en bättre upplevelse för hello kund några kända konfigurationer är enklare toosupport och kan hjälpa dig att leverera en högre säkerhetsnivå för densitet.</span><span class="sxs-lookup"><span data-stu-id="4cf76-117">In addition toomaking a better experience for hello customer, a few known configurations are easier toosupport and can help you deliver a higher level of density.</span></span>

<span data-ttu-id="4cf76-118">följande exempel visar hur hello toodefine variabler som innehåller komplexa objekt som representerar samlingar av data.</span><span class="sxs-lookup"><span data-stu-id="4cf76-118">hello following example shows how toodefine variables that contain complex objects for representing collections of data.</span></span> <span data-ttu-id="4cf76-119">hello samlingar Definiera värden som används för storlek på virtuell dator, nätverksinställningar, operativsysteminställningar och tillgänglighetsinställningarna.</span><span class="sxs-lookup"><span data-stu-id="4cf76-119">hello collections define values that are used for virtual machine size, network settings, operating system settings and availability settings.</span></span>

    "variables": {
      "tshirtSize": "[variables(concat('tshirtSize', parameters('tshirtSize')))]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      },
      "tshirtSizeMedium": {
        "vmSize": "Standard_A3",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-8disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1],
          "jumpbox": 0
        }
      },
      "tshirtSizeLarge": {
        "vmSize": "Standard_A4",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-16disk-resources.json')]",
        "vmCount": 3,
        "slaveCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1,1],
          "jumpbox": 0
        }
      },
      "osSettings": {
        "scripts": [
          "[concat(variables('templateBaseUrl'), 'install_postgresql.sh')]",
          "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/shared_scripts/ubuntu/vm-disk-utils-0.1.sh"
        ],
        "imageReference": {
          "publisher": "Canonical",
          "offer": "UbuntuServer",
          "sku": "14.04.2-LTS",
          "version": "latest"
        }
      },
      "networkSettings": {
        "vnetName": "[parameters('virtualNetworkName')]",
        "addressPrefix": "10.0.0.0/16",
        "subnets": {
          "dmz": {
            "name": "dmz",
            "prefix": "10.0.0.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          },
          "data": {
            "name": "data",
            "prefix": "10.0.1.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          }
        }
      },
      "availabilitySetSettings": {
        "name": "pgsqlAvailabilitySet",
        "fdCount": 3,
        "udCount": 5
      }
    }

<span data-ttu-id="4cf76-120">Observera att hello **tshirtSize** variabeln sammanfogar hello t-shirt storlek som du angav via en parameter (**små**, **medel**, **stor**) toohello text **tshirtSize**.</span><span class="sxs-lookup"><span data-stu-id="4cf76-120">Notice that hello **tshirtSize** variable concatenates hello t-shirt size you provided through a parameter (**Small**, **Medium**, **Large**) toohello text **tshirtSize**.</span></span> <span data-ttu-id="4cf76-121">Du använder den här variabeln tooretrieve hello associerade komplexa objektvariabeln för att t-shirt storlek.</span><span class="sxs-lookup"><span data-stu-id="4cf76-121">You use this variable tooretrieve hello associated complex object variable for that t-shirt size.</span></span>

<span data-ttu-id="4cf76-122">Sedan kan du referera dessa variabler senare i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="4cf76-122">You can then reference these variables later in hello template.</span></span> <span data-ttu-id="4cf76-123">hello möjlighet tooreference med namnet-variabler och deras egenskaper förenklar hello mallens syntax och gör det enkelt toounderstand kontext.</span><span class="sxs-lookup"><span data-stu-id="4cf76-123">hello ability tooreference named-variables and their properties simplifies hello template syntax, and makes it easy toounderstand context.</span></span> <span data-ttu-id="4cf76-124">hello följande exempel definierar en resurs toodeploy genom att använda hello-objekt som visas tidigare tooset värden.</span><span class="sxs-lookup"><span data-stu-id="4cf76-124">hello following example defines a resource toodeploy by using hello objects shown previously tooset values.</span></span> <span data-ttu-id="4cf76-125">Till exempel hello VM-storlek anges genom att hämta hello värde för `variables('tshirtSize').vmSize` medan hello värdet för hello diskstorleken hämtas från `variables('tshirtSize').diskSize`.</span><span class="sxs-lookup"><span data-stu-id="4cf76-125">For example, hello VM size is set by retrieving hello value for `variables('tshirtSize').vmSize` while hello value for hello disk size is retrieved from `variables('tshirtSize').diskSize`.</span></span> <span data-ttu-id="4cf76-126">Dessutom hello URI för en länkad mall har angetts med hello värde för `variables('tshirtSize').vmTemplate`.</span><span class="sxs-lookup"><span data-stu-id="4cf76-126">In addition, hello URI for a linked template is set with hello value for `variables('tshirtSize').vmTemplate`.</span></span>

    "name": "master-node",
    "type": "Microsoft.Resources/deployments",
    "apiVersion": "2015-01-01",
    "dependsOn": [
        "[concat('Microsoft.Resources/deployments/', 'shared')]"
    ],
    "properties": {
        "mode": "Incremental",
        "templateLink": {
          "uri": "[variables('tshirtSize').vmTemplate]",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "value": "[parameters('adminPassword')]"
          },
          "replicatorPassword": {
            "value": "[parameters('replicatorPassword')]"
          },
          "osSettings": {
            "value": "[variables('osSettings')]"
          },
          "subnet": {
            "value": "[variables('networkSettings').subnets.data]"
          },
          "commonSettings": {
            "value": {
              "region": "[parameters('region')]",
              "adminUsername": "[parameters('adminUsername')]",
              "namespace": "ms"
            }
          },
          "storageSettings": {
            "value":"[variables('tshirtSize').storage]"
          },
          "machineSettings": {
            "value": {
              "vmSize": "[variables('tshirtSize').vmSize]",
              "diskSize": "[variables('tshirtSize').diskSize]",
              "vmCount": 1,
              "availabilitySet": "[variables('availabilitySetSettings').name]"
            }
          },
          "masterIpAddress": {
            "value": "0"
          },
          "dbType": {
            "value": "MASTER"
          }
        }
      }
    }

## <a name="pass-state-tooa-template"></a><span data-ttu-id="4cf76-127">Skicka tillstånd tooa mall</span><span class="sxs-lookup"><span data-stu-id="4cf76-127">Pass state tooa template</span></span>
<span data-ttu-id="4cf76-128">Du kan dela tillstånd till en mall med parametrar som du anger direkt under distributionen.</span><span class="sxs-lookup"><span data-stu-id="4cf76-128">You share state into a template through parameters that you provide directly during deployment.</span></span>

<span data-ttu-id="4cf76-129">hello följande tabell listas vanliga parametrar i mallar.</span><span class="sxs-lookup"><span data-stu-id="4cf76-129">hello following table lists commonly used parameters in templates.</span></span>

| <span data-ttu-id="4cf76-130">Namn</span><span class="sxs-lookup"><span data-stu-id="4cf76-130">Name</span></span> | <span data-ttu-id="4cf76-131">Värde</span><span class="sxs-lookup"><span data-stu-id="4cf76-131">Value</span></span> | <span data-ttu-id="4cf76-132">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4cf76-132">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="4cf76-133">location</span><span class="sxs-lookup"><span data-stu-id="4cf76-133">location</span></span> |<span data-ttu-id="4cf76-134">Sträng från en begränsad lista med Azure-regioner</span><span class="sxs-lookup"><span data-stu-id="4cf76-134">String from a constrained list of Azure regions</span></span> |<span data-ttu-id="4cf76-135">hello plats där hello resurser har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="4cf76-135">hello location where hello resources are deployed.</span></span> |
| <span data-ttu-id="4cf76-136">storageAccountNamePrefix</span><span class="sxs-lookup"><span data-stu-id="4cf76-136">storageAccountNamePrefix</span></span> |<span data-ttu-id="4cf76-137">Sträng</span><span class="sxs-lookup"><span data-stu-id="4cf76-137">String</span></span> |<span data-ttu-id="4cf76-138">Unikt DNS-namn för hello Storage-konto där hello Virtuella diskar placeras</span><span class="sxs-lookup"><span data-stu-id="4cf76-138">Unique DNS name for hello Storage Account where hello VM's disks are placed</span></span> |
| <span data-ttu-id="4cf76-139">Domännamn</span><span class="sxs-lookup"><span data-stu-id="4cf76-139">domainName</span></span> |<span data-ttu-id="4cf76-140">Sträng</span><span class="sxs-lookup"><span data-stu-id="4cf76-140">String</span></span> |<span data-ttu-id="4cf76-141">Domännamnet för hello offentligt tillgänglig jumpbox VM hello format: **{domainName}. { Location}.cloudapp.com** till exempel: **mydomainname.westus.cloudapp.azure.com**</span><span class="sxs-lookup"><span data-stu-id="4cf76-141">Domain name of hello publicly accessible jumpbox VM in hello format: **{domainName}.{location}.cloudapp.com** For example: **mydomainname.westus.cloudapp.azure.com**</span></span> |
| <span data-ttu-id="4cf76-142">adminUsername</span><span class="sxs-lookup"><span data-stu-id="4cf76-142">adminUsername</span></span> |<span data-ttu-id="4cf76-143">Sträng</span><span class="sxs-lookup"><span data-stu-id="4cf76-143">String</span></span> |<span data-ttu-id="4cf76-144">Användarnamn för hello virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="4cf76-144">Username for hello VMs</span></span> |
| <span data-ttu-id="4cf76-145">adminPassword</span><span class="sxs-lookup"><span data-stu-id="4cf76-145">adminPassword</span></span> |<span data-ttu-id="4cf76-146">Sträng</span><span class="sxs-lookup"><span data-stu-id="4cf76-146">String</span></span> |<span data-ttu-id="4cf76-147">Lösenordet för hello virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="4cf76-147">Password for hello VMs</span></span> |
| <span data-ttu-id="4cf76-148">tshirtSize</span><span class="sxs-lookup"><span data-stu-id="4cf76-148">tshirtSize</span></span> |<span data-ttu-id="4cf76-149">Sträng från en begränsad lista över erbjuds t-shirt storlekar</span><span class="sxs-lookup"><span data-stu-id="4cf76-149">String from a constrained list of offered t-shirt sizes</span></span> |<span data-ttu-id="4cf76-150">hello namngivna skala enhet storlek tooprovision.</span><span class="sxs-lookup"><span data-stu-id="4cf76-150">hello named scale unit size tooprovision.</span></span> <span data-ttu-id="4cf76-151">Till exempel ”liten”, ”medel”, ”stor”</span><span class="sxs-lookup"><span data-stu-id="4cf76-151">For example, "Small", "Medium", "Large"</span></span> |
| <span data-ttu-id="4cf76-152">virtualNetworkName</span><span class="sxs-lookup"><span data-stu-id="4cf76-152">virtualNetworkName</span></span> |<span data-ttu-id="4cf76-153">Sträng</span><span class="sxs-lookup"><span data-stu-id="4cf76-153">String</span></span> |<span data-ttu-id="4cf76-154">Namnet på hello virtuellt nätverk som hello konsumenten vill toouse.</span><span class="sxs-lookup"><span data-stu-id="4cf76-154">Name of hello virtual network that hello consumer wants toouse.</span></span> |
| <span data-ttu-id="4cf76-155">enableJumpbox</span><span class="sxs-lookup"><span data-stu-id="4cf76-155">enableJumpbox</span></span> |<span data-ttu-id="4cf76-156">Sträng från en begränsad lista (aktiverat/inaktiverat)</span><span class="sxs-lookup"><span data-stu-id="4cf76-156">String from a constrained list (enabled/disabled)</span></span> |<span data-ttu-id="4cf76-157">Parametern som identifierar om tooenable en jumpbox för hello-miljö.</span><span class="sxs-lookup"><span data-stu-id="4cf76-157">Parameter that identifies whether tooenable a jumpbox for hello environment.</span></span> <span data-ttu-id="4cf76-158">Värden: ”aktiverad”, ”inaktiverad”</span><span class="sxs-lookup"><span data-stu-id="4cf76-158">Values: "enabled", "disabled"</span></span> |

<span data-ttu-id="4cf76-159">Hej **tshirtSize** parameter används i föregående avsnitt i hello definieras som:</span><span class="sxs-lookup"><span data-stu-id="4cf76-159">hello **tshirtSize** parameter used in hello previous section is defined as:</span></span>

    "parameters": {
      "tshirtSize": {
        "type": "string",
        "defaultValue": "Small",
        "allowedValues": [
          "Small",
          "Medium",
          "Large"
        ],
        "metadata": {
          "Description": "T-shirt size of hello MongoDB deployment"
        }
      }
    }


## <a name="pass-state-toolinked-templates"></a><span data-ttu-id="4cf76-160">Skicka tillstånd toolinked mallar</span><span class="sxs-lookup"><span data-stu-id="4cf76-160">Pass state toolinked templates</span></span>
<span data-ttu-id="4cf76-161">När du ansluter toolinked mallar kan du ofta använder en blandning av statiska och genereras variabler.</span><span class="sxs-lookup"><span data-stu-id="4cf76-161">When connecting toolinked templates, you often use a mix of static and generated variables.</span></span>

### <a name="static-variables"></a><span data-ttu-id="4cf76-162">Statiska variabler</span><span class="sxs-lookup"><span data-stu-id="4cf76-162">Static variables</span></span>
<span data-ttu-id="4cf76-163">Statiska variabler är ofta använda tooprovide grundläggande värden, till exempel URL: er som används i en mall.</span><span class="sxs-lookup"><span data-stu-id="4cf76-163">Static variables are often used tooprovide base values, such as URLs, that are used throughout a template.</span></span>

<span data-ttu-id="4cf76-164">I följande mall utdrag hello `templateBaseUrl` anger hello rotplats hello mallen i GitHub.</span><span class="sxs-lookup"><span data-stu-id="4cf76-164">In hello following template excerpt, `templateBaseUrl` specifies hello root location for hello template in GitHub.</span></span> <span data-ttu-id="4cf76-165">hello nästa rad skapar en ny variabel `sharedTemplateUrl` som sammanfogar hello bas-URL med hello kända namnet på mallen för hello delade resurser.</span><span class="sxs-lookup"><span data-stu-id="4cf76-165">hello next line builds a new variable `sharedTemplateUrl` that concatenates hello base URL with hello known name of hello shared resources template.</span></span> <span data-ttu-id="4cf76-166">Under den raden är en variabel med komplexa objekt används toostore en t-shirt storlek, där hello bas-URL är sammanfogad toohello fungerande konfiguration mallplats och lagras i hello `vmTemplate` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="4cf76-166">Below that line, a complex object variable is used toostore a t-shirt size, where hello base URL is concatenated toohello known configuration template location and stored in hello `vmTemplate` property.</span></span>

<span data-ttu-id="4cf76-167">hello fördelen med den här metoden är att om platsen för hello mall ändras, behöver du bara toochange hello statisk variabel på en plats som passerar i hela hello länkade mallar.</span><span class="sxs-lookup"><span data-stu-id="4cf76-167">hello benefit of this approach is that if hello template location changes, you only need toochange hello static variable in one place, which passes it throughout hello linked templates.</span></span>

    "variables": {
      "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
      "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "slaveCount": 1,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      }
    }

### <a name="generated-variables"></a><span data-ttu-id="4cf76-168">Genererade variabler</span><span class="sxs-lookup"><span data-stu-id="4cf76-168">Generated variables</span></span>
<span data-ttu-id="4cf76-169">I tillägg toostatic variabler genereras flera variabler dynamiskt.</span><span class="sxs-lookup"><span data-stu-id="4cf76-169">In addition toostatic variables, several variables are generated dynamically.</span></span> <span data-ttu-id="4cf76-170">Det här avsnittet beskrivs några av hello vanliga typer av genererade variabler.</span><span class="sxs-lookup"><span data-stu-id="4cf76-170">This section identifies some of hello common types of generated variables.</span></span>

#### <a name="tshirtsize"></a><span data-ttu-id="4cf76-171">tshirtSize</span><span class="sxs-lookup"><span data-stu-id="4cf76-171">tshirtSize</span></span>
<span data-ttu-id="4cf76-172">Du är bekant med den här variabeln som genereras från hello exemplen ovan.</span><span class="sxs-lookup"><span data-stu-id="4cf76-172">You are familiar with this generated variable from hello examples above.</span></span>

#### <a name="networksettings"></a><span data-ttu-id="4cf76-173">networkSettings</span><span class="sxs-lookup"><span data-stu-id="4cf76-173">networkSettings</span></span>
<span data-ttu-id="4cf76-174">I en kapacitet, kapaciteten eller lösningsmall för slutpunkt till slutpunkt begränsade hello Skapa länkade mallar normalt resurser som finns i ett nätverk.</span><span class="sxs-lookup"><span data-stu-id="4cf76-174">In a capacity, capability, or end-to-end scoped solution template, hello linked templates typically create resources that exist on a network.</span></span> <span data-ttu-id="4cf76-175">Ett enkelt sätt är toouse komplexa objektet toostore nätverksinställningar och skicka dem toolinked mallar.</span><span class="sxs-lookup"><span data-stu-id="4cf76-175">One straightforward approach is toouse a complex object toostore network settings and pass them toolinked templates.</span></span>

<span data-ttu-id="4cf76-176">Du kan se ett exempel på kommunikation nätverksinställningar nedan.</span><span class="sxs-lookup"><span data-stu-id="4cf76-176">An example of communicating network settings can be seen below.</span></span>

    "networkSettings": {
      "vnetName": "[parameters('virtualNetworkName')]",
      "addressPrefix": "10.0.0.0/16",
      "subnets": {
        "dmz": {
          "name": "dmz",
          "prefix": "10.0.0.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        },
        "data": {
          "name": "data",
          "prefix": "10.0.1.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        }
      }
    }

#### <a name="availabilitysettings"></a><span data-ttu-id="4cf76-177">availabilitySettings</span><span class="sxs-lookup"><span data-stu-id="4cf76-177">availabilitySettings</span></span>
<span data-ttu-id="4cf76-178">Resurser som skapades i länkade mallar placeras ofta i en tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="4cf76-178">Resources created in linked templates are often placed in an availability set.</span></span> <span data-ttu-id="4cf76-179">I följande exempel hello, hello tillgänglighetsuppsättningen anges också hello feldomän och uppdatera domän antal toouse.</span><span class="sxs-lookup"><span data-stu-id="4cf76-179">In hello following example, hello availability set name is specified and also hello fault domain and update domain count toouse.</span></span>

    "availabilitySetSettings": {
      "name": "pgsqlAvailabilitySet",
      "fdCount": 3,
      "udCount": 5
    }

<span data-ttu-id="4cf76-180">Om du behöver flera tillgänglighetsuppsättningar (till exempel en för överordnade noder) och en annan för datanoder, kan du använda ett namn som ett prefix kan ange flera tillgänglighetsuppsättningar eller följa hello-modell som visades tidigare för att skapa en variabel för en viss t-shirt storlek.</span><span class="sxs-lookup"><span data-stu-id="4cf76-180">If you need multiple availability sets (for example, one for master nodes and another for data nodes), you can use a name as a prefix, specify multiple availability sets, or follow hello model shown earlier for creating a variable for a specific t-shirt size.</span></span>

#### <a name="storagesettings"></a><span data-ttu-id="4cf76-181">storageSettings</span><span class="sxs-lookup"><span data-stu-id="4cf76-181">storageSettings</span></span>
<span data-ttu-id="4cf76-182">Lagringsinformation om som ofta delas med länkade mallar.</span><span class="sxs-lookup"><span data-stu-id="4cf76-182">Storage details are often shared with linked templates.</span></span> <span data-ttu-id="4cf76-183">I hello exemplet nedan är ett *storageSettings* objekt innehåller information om hello storage-konto och en behållare namn.</span><span class="sxs-lookup"><span data-stu-id="4cf76-183">In hello example below, a *storageSettings* object provides details about hello storage account and container names.</span></span>

    "storageSettings": {
        "vhdStorageAccountName": "[parameters('storageAccountName')]",
        "vhdContainerName": "[variables('vmStorageAccountContainerName')]",
        "destinationVhdsContainer": "[concat('https://', parameters('storageAccountName'), variables('vmStorageAccountDomain'), '/', variables('vmStorageAccountContainerName'), '/')]"
    }

#### <a name="ossettings"></a><span data-ttu-id="4cf76-184">osSettings</span><span class="sxs-lookup"><span data-stu-id="4cf76-184">osSettings</span></span>
<span data-ttu-id="4cf76-185">Länkade mallar kan behöva du toopass inställningar toovarious noder av typen operativsystem över olika för kända konfigurationstyper.</span><span class="sxs-lookup"><span data-stu-id="4cf76-185">With linked templates, you may need toopass operating system settings toovarious nodes types across different known configuration types.</span></span> <span data-ttu-id="4cf76-186">Ett komplext objekt är ett enkelt sätt toostore och dela information om operativsystem och gör det också lättare toosupport flera operativsystem alternativ för distributionen.</span><span class="sxs-lookup"><span data-stu-id="4cf76-186">A complex object is an easy way toostore and share operating system information and also makes it easier toosupport multiple operating system choices for deployment.</span></span>

<span data-ttu-id="4cf76-187">hello följande exempel visas ett objekt för *osSettings*:</span><span class="sxs-lookup"><span data-stu-id="4cf76-187">hello following example shows an object for *osSettings*:</span></span>

    "osSettings": {
      "imageReference": {
        "publisher": "Canonical",
        "offer": "UbuntuServer",
        "sku": "14.04.2-LTS",
        "version": "latest"
      }
    }

#### <a name="machinesettings"></a><span data-ttu-id="4cf76-188">machineSettings</span><span class="sxs-lookup"><span data-stu-id="4cf76-188">machineSettings</span></span>
<span data-ttu-id="4cf76-189">En genererad variabel *machineSettings* är ett komplext objekt som innehåller en blandning av kärnor variabler för att skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="4cf76-189">A generated variable, *machineSettings* is a complex object containing a mix of core variables for creating a VM.</span></span> <span data-ttu-id="4cf76-190">hello variabler är administratörsanvändarnamn och lösenord, ett prefix för hello VM-namn och en referens för avbildningen av operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="4cf76-190">hello variables include administrator user name and password, a prefix for hello VM names, and an operating system image reference.</span></span>

    "machineSettings": {
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]",
        "machineNamePrefix": "mongodb-",
        "osImageReference": {
            "publisher": "[variables('osFamilySpec').imagePublisher]",
            "offer": "[variables('osFamilySpec').imageOffer]",
            "sku": "[variables('osFamilySpec').imageSKU]",
            "version": "latest"
        }
    },

<span data-ttu-id="4cf76-191">Observera att *osImageReference* hämtar hello värden från hello *osSettings* variabel som definierats i hello Huvudmall.</span><span class="sxs-lookup"><span data-stu-id="4cf76-191">Note that *osImageReference* retrieves hello values from hello *osSettings* variable defined in hello main template.</span></span> <span data-ttu-id="4cf76-192">Det innebär att du kan enkelt ändra hello operativsystem för en virtuell dator, helt eller baserat på hello inställningar mallen konsumenter.</span><span class="sxs-lookup"><span data-stu-id="4cf76-192">That means you can easily change hello operating system for a VM—entirely or based on hello preference of a template consumer.</span></span>

#### <a name="vmscripts"></a><span data-ttu-id="4cf76-193">vmScripts</span><span class="sxs-lookup"><span data-stu-id="4cf76-193">vmScripts</span></span>
<span data-ttu-id="4cf76-194">Hej *vmScripts* objekt innehåller information om hello skript toodownload och köra på en VM-instans, inklusive utanför och inom referenser.</span><span class="sxs-lookup"><span data-stu-id="4cf76-194">hello *vmScripts* object contains details about hello scripts toodownload and execute on a VM instance, including outside and inside references.</span></span> <span data-ttu-id="4cf76-195">Utanför innehåller referenser hello infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="4cf76-195">Outside references include hello infrastructure.</span></span>
<span data-ttu-id="4cf76-196">I referenser omfattar hello installerad programvara som är installerad och konfiguration.</span><span class="sxs-lookup"><span data-stu-id="4cf76-196">Inside references include hello installed software installed and configuration.</span></span>

<span data-ttu-id="4cf76-197">Du använder hello *scriptsToDownload* egenskapen toolist hello skript toodownload toohello VM.</span><span class="sxs-lookup"><span data-stu-id="4cf76-197">You use hello *scriptsToDownload* property toolist hello scripts toodownload toohello VM.</span></span> <span data-ttu-id="4cf76-198">Det här objektet innehåller också referenser toocommand-argument för olika typer av åtgärder.</span><span class="sxs-lookup"><span data-stu-id="4cf76-198">This object also contains references toocommand-line arguments for different types of actions.</span></span> <span data-ttu-id="4cf76-199">Dessa åtgärder omfattar köra hello standardinstallationen för varje enskild nod, en installation som körs när alla noder har distribuerats och ytterligare skript som kan vara specifika tooa som anges i mallen.</span><span class="sxs-lookup"><span data-stu-id="4cf76-199">These actions include executing hello default installation for each individual node, an installation that runs after all nodes are deployed, and any additional scripts that may be specific tooa given template.</span></span>

<span data-ttu-id="4cf76-200">Det här exemplet är från en mall som används toodeploy MongoDB, som kräver en avgörandet toodeliver hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="4cf76-200">This example is from a template used toodeploy MongoDB, which requires an arbiter toodeliver high availability.</span></span> <span data-ttu-id="4cf76-201">Hej *arbiterNodeInstallCommand* har lagts till för*vmScripts* tooinstall hello avgörandet.</span><span class="sxs-lookup"><span data-stu-id="4cf76-201">hello *arbiterNodeInstallCommand* has been added too*vmScripts* tooinstall hello arbiter.</span></span>

<span data-ttu-id="4cf76-202">hello variabler avsnittet är hittar du hello variabler som definierar hello viss text tooexecute hello skriptet med hello rätt värden.</span><span class="sxs-lookup"><span data-stu-id="4cf76-202">hello variables section is where you find hello variables that define hello specific text tooexecute hello script with hello proper values.</span></span>

    "vmScripts": {
        "scriptsToDownload": [
            "[concat(variables('scriptUrl'), 'mongodb-', variables('osFamilySpec').osName, '-install.sh')]",
            "[concat(variables('sharedScriptUrl'), 'vm-disk-utils-0.1.sh')]"
        ],
        "regularNodeInstallCommand": "[variables('installCommand')]",
        "lastNodeInstallCommand": "[concat(variables('installCommand'), ' -l')]",
        "arbiterNodeInstallCommand": "[concat(variables('installCommand'), ' -a')]"
    },


## <a name="return-state-from-a-template"></a><span data-ttu-id="4cf76-203">Returnera tillstånd från en mall</span><span class="sxs-lookup"><span data-stu-id="4cf76-203">Return state from a template</span></span>
<span data-ttu-id="4cf76-204">Inte bara kan du skicka data till en mall, du kan också dela data tillbaka toohello anropa mall.</span><span class="sxs-lookup"><span data-stu-id="4cf76-204">Not only can you pass data into a template, you can also share data back toohello calling template.</span></span> <span data-ttu-id="4cf76-205">I hello **matar ut** avsnitt av en länkad mall kan du ange nyckel/värde-par som kan användas av hello källmallen.</span><span class="sxs-lookup"><span data-stu-id="4cf76-205">In hello **outputs** section of a linked template, you can provide key/value pairs that can be consumed by hello source template.</span></span>

<span data-ttu-id="4cf76-206">hello följande exempel visas hur toopass hello privat IP-adress som genereras i en länkad mall.</span><span class="sxs-lookup"><span data-stu-id="4cf76-206">hello following example shows how toopass hello private IP address generated in a linked template.</span></span>

    "outputs": {
        "masterip": {
            "value": "[reference(concat(variables('nicName'),0)).ipConfigurations[0].properties.privateIPAddress]",
            "type": "string"
         }
    }

<span data-ttu-id="4cf76-207">Inom hello Huvudmall, kan du använda dessa data med hello följande syntax:</span><span class="sxs-lookup"><span data-stu-id="4cf76-207">Within hello main template, you can use that data with hello following syntax:</span></span>

    "[reference('master-node').outputs.masterip.value]"

<span data-ttu-id="4cf76-208">Du kan använda det här uttrycket i hello utdata avsnittet eller hello resurser avsnitt i hello huvudsakliga mall.</span><span class="sxs-lookup"><span data-stu-id="4cf76-208">You can use this expression in either hello outputs section or hello resources section of hello main template.</span></span> <span data-ttu-id="4cf76-209">Du kan inte använda hello uttryck i hello variabler avsnittet eftersom det är beroende av hello runtime-tillståndet.</span><span class="sxs-lookup"><span data-stu-id="4cf76-209">You cannot use hello expression in hello variables section because it relies on hello runtime state.</span></span> <span data-ttu-id="4cf76-210">tooreturn detta värde från hello Huvudmall, Använd:</span><span class="sxs-lookup"><span data-stu-id="4cf76-210">tooreturn this value from hello main template, use:</span></span>

    "outputs": {
      "masterIpAddress": {
        "value": "[reference('master-node').outputs.masterip.value]",
        "type": "string"
      }

<span data-ttu-id="4cf76-211">Ett exempel på hur hello matar ut avsnitt i en länkad mall tooreturn datadiskar för en virtuell dator, finns [skapar flera datadiskar för en virtuell dator](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="4cf76-211">For an example of using hello outputs section of a linked template tooreturn data disks for a virtual machine, see [Creating multiple data disks for a Virtual Machine](resource-group-create-multiple.md).</span></span>

## <a name="define-authentication-settings-for-virtual-machine"></a><span data-ttu-id="4cf76-212">Definiera autentiseringsinställningar för den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="4cf76-212">Define authentication settings for virtual machine</span></span>
<span data-ttu-id="4cf76-213">Du kan använda hello samma mönster tidigare inställningar toospecify hello autentisering konfigurationsinställningar för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="4cf76-213">You can use hello same pattern shown previously for configuration settings toospecify hello authentication settings for a virtual machine.</span></span> <span data-ttu-id="4cf76-214">Du skapar en parameter för överföring i hello typ av autentisering.</span><span class="sxs-lookup"><span data-stu-id="4cf76-214">You create a parameter for passing in hello type of authentication.</span></span>

    "parameters": {
      "authenticationType": {
        "allowedValues": [
          "password",
          "sshPublicKey"
        ],
        "defaultValue": "password",
        "metadata": {
          "description": "Authentication type"
        },
        "type": "string"
      }
    }

<span data-ttu-id="4cf76-215">Du lägger till variabler för hello olika typer av autentisering och en variabel toostore vilken typ som används för den här distributionen baserat på hello hello parameterns värde.</span><span class="sxs-lookup"><span data-stu-id="4cf76-215">You add variables for hello different authentication types, and a variable toostore which type is used for this deployment based on hello value of hello parameter.</span></span>

    "variables": {
      "osProfile": "[variables(concat('osProfile', parameters('authenticationType')))]",
      "osProfilepassword": {
        "adminPassword": "[parameters('adminPassword')]",
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]"
      },
      "osProfilesshPublicKey": {
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]",
        "linuxConfiguration": {
          "disablePasswordAuthentication": "true",
          "ssh": {
            "publicKeys": [
              {
                "keyData": "[parameters('sshPublicKey')]",
                "path": "/home/notused/.ssh/authorized_keys"
              }
            ]
          }
        }
      }
    }

<span data-ttu-id="4cf76-216">När definiera hello virtuell dator kan du ställa in hello **osProfile** toohello variabeln som du skapat.</span><span class="sxs-lookup"><span data-stu-id="4cf76-216">When defining hello virtual machine, you set hello **osProfile** toohello variable you created.</span></span>

    {
      "type": "Microsoft.Compute/virtualMachines",
      ...
      "osProfile": "[variables('osProfile')]"
    }


## <a name="next-steps"></a><span data-ttu-id="4cf76-217">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4cf76-217">Next steps</span></span>
* <span data-ttu-id="4cf76-218">toolearn om hello avsnitt i hello-mallen finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md)</span><span class="sxs-lookup"><span data-stu-id="4cf76-218">toolearn about hello sections of hello template, see [Authoring Azure Resource Manager Templates](resource-group-authoring-templates.md)</span></span>
* <span data-ttu-id="4cf76-219">toosee hello funktioner som är tillgängliga i en mall finns [Azure Resource Manager Mallfunktioner](resource-group-template-functions.md)</span><span class="sxs-lookup"><span data-stu-id="4cf76-219">toosee hello functions that are available within a template, see [Azure Resource Manager Template Functions](resource-group-template-functions.md)</span></span>

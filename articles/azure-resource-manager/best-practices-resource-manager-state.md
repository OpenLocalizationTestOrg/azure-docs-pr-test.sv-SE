---
title: "Skicka komplexa värden mellan Azure-mallar | Microsoft Docs"
description: "Visar rekommenderade metoder för att dela tillstånd med Azure Resource Manager-mallar och länkade mallar med komplexa objekt."
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
ms.openlocfilehash: 23cc4321159a87b61c177b11381646af8bd9eb35
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="share-state-to-and-from-azure-resource-manager-templates"></a><span data-ttu-id="210ee-103">Tillstånd för filresurs till och från Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="210ee-103">Share state to and from Azure Resource Manager templates</span></span>
<span data-ttu-id="210ee-104">Det här avsnittet innehåller metodtips för att hantera och dela tillstånd i mallar.</span><span class="sxs-lookup"><span data-stu-id="210ee-104">This topic shows best practices for managing and sharing state within templates.</span></span> <span data-ttu-id="210ee-105">Parametrar och variabler som visas i det här avsnittet är exempel på vilken typ av objekt som du kan definiera lätt ordna dina distributionskrav.</span><span class="sxs-lookup"><span data-stu-id="210ee-105">The parameters and variables shown in this topic are examples of the type of objects you can define to conveniently organize your deployment requirements.</span></span> <span data-ttu-id="210ee-106">Du kan implementera objekten med egenskapsvärden som passar din miljö i dessa exempel.</span><span class="sxs-lookup"><span data-stu-id="210ee-106">From these examples, you can implement your own objects with property values that make sense for your environment.</span></span>

<span data-ttu-id="210ee-107">Det här avsnittet är en del av en större vitbok.</span><span class="sxs-lookup"><span data-stu-id="210ee-107">This topic is part of a larger whitepaper.</span></span> <span data-ttu-id="210ee-108">Om du vill läsa fullständig dokumentet, hämta [World klassen Resource Manager-mallar överväganden och beprövade metoder](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).</span><span class="sxs-lookup"><span data-stu-id="210ee-108">To read the full paper, download [World Class Resource Manager Templates Considerations and Proven Practices](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).</span></span>

## <a name="provide-standard-configuration-settings"></a><span data-ttu-id="210ee-109">Ange standard konfigurationsinställningar</span><span class="sxs-lookup"><span data-stu-id="210ee-109">Provide standard configuration settings</span></span>
<span data-ttu-id="210ee-110">I stället för att erbjuda en mall som ger totalt flexibilitet och oräkneliga variationer är vanliga mönstret att tillhandahålla ett urval av kända konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="210ee-110">Rather than offer a template that provides total flexibility and countless variations, a common pattern is to provide a selection of known configurations.</span></span> <span data-ttu-id="210ee-111">Användare kan i praktiken välja standard t-shirt storlekar, till exempel sandbox små, medelstora och stora.</span><span class="sxs-lookup"><span data-stu-id="210ee-111">In effect, users can select standard t-shirt sizes such as sandbox, small, medium, and large.</span></span> <span data-ttu-id="210ee-112">Andra exempel på t-shirt storlekar är produkterbjudanden, till exempel community edition eller enterprise edition.</span><span class="sxs-lookup"><span data-stu-id="210ee-112">Other examples of t-shirt sizes are product offerings, such as community edition or enterprise edition.</span></span> <span data-ttu-id="210ee-113">I andra fall kan det vara belastningsspecifikt konfigurationer av en teknik – som kartan minska eller utan sql.</span><span class="sxs-lookup"><span data-stu-id="210ee-113">In other cases, it may be workload-specific configurations of a technology – such as map reduce or no sql.</span></span>

<span data-ttu-id="210ee-114">Du kan skapa variabler som innehåller datasamlingar, kallas ibland för ”egenskapsuppsättningar” och använda dessa data att resursdeklarationen i mallen med komplexa objekt.</span><span class="sxs-lookup"><span data-stu-id="210ee-114">With complex objects, you can create variables that contain collections of data, sometimes known as "property bags" and use that data to drive the resource declaration in your template.</span></span> <span data-ttu-id="210ee-115">Den här metoden ger bra, kända konfigurationer i olika storlekar förkonfigurerade för kunder.</span><span class="sxs-lookup"><span data-stu-id="210ee-115">This approach provides good, known configurations of varying sizes that are preconfigured for customers.</span></span> <span data-ttu-id="210ee-116">Utan kända konfigurationer måste användare av mallen fastställa klustret storleken på egen hand factor plattform resurs begränsningar och beräkna för att identifiera den resulterande partitionering av lagringskonton och andra resurser (på grund av storlek och resursen begränsningar för kluster).</span><span class="sxs-lookup"><span data-stu-id="210ee-116">Without known configurations, users of the template must determine cluster sizing on their own, factor in platform resource constraints, and do math to identify the resulting partitioning of storage accounts and other resources (due to cluster size and resource constraints).</span></span> <span data-ttu-id="210ee-117">Förutom att göra en bättre upplevelse för kunden, några kända konfigurationer är enklare att stödja och kan hjälpa dig att leverera en högre säkerhetsnivå för densitet.</span><span class="sxs-lookup"><span data-stu-id="210ee-117">In addition to making a better experience for the customer, a few known configurations are easier to support and can help you deliver a higher level of density.</span></span>

<span data-ttu-id="210ee-118">I följande exempel visas hur du definierar variabler som innehåller komplexa objekt som representerar samlingar av data.</span><span class="sxs-lookup"><span data-stu-id="210ee-118">The following example shows how to define variables that contain complex objects for representing collections of data.</span></span> <span data-ttu-id="210ee-119">Samlingarna Definiera värden som används för storlek på virtuell dator, nätverksinställningar, operativsysteminställningar och tillgänglighetsinställningarna.</span><span class="sxs-lookup"><span data-stu-id="210ee-119">The collections define values that are used for virtual machine size, network settings, operating system settings and availability settings.</span></span>

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

<span data-ttu-id="210ee-120">Observera att den **tshirtSize** variabeln sammanfogar t-shirt storlek som du angav via en parameter (**små**, **medel**, **stor**) i texten **tshirtSize**.</span><span class="sxs-lookup"><span data-stu-id="210ee-120">Notice that the **tshirtSize** variable concatenates the t-shirt size you provided through a parameter (**Small**, **Medium**, **Large**) to the text **tshirtSize**.</span></span> <span data-ttu-id="210ee-121">Du kan använda den här variabeln för att hämta associerade komplexa objektvariabeln för att t-shirt storlek.</span><span class="sxs-lookup"><span data-stu-id="210ee-121">You use this variable to retrieve the associated complex object variable for that t-shirt size.</span></span>

<span data-ttu-id="210ee-122">Sedan kan du referera dessa variabler senare i mallen.</span><span class="sxs-lookup"><span data-stu-id="210ee-122">You can then reference these variables later in the template.</span></span> <span data-ttu-id="210ee-123">Möjligheten att referera till namngivna variabler och deras egenskaper förenklar syntaxen och gör det enkelt att förstå kontext.</span><span class="sxs-lookup"><span data-stu-id="210ee-123">The ability to reference named-variables and their properties simplifies the template syntax, and makes it easy to understand context.</span></span> <span data-ttu-id="210ee-124">I följande exempel definieras en resurs för att distribuera med hjälp av objekten som visas tidigare för att ställa in värden.</span><span class="sxs-lookup"><span data-stu-id="210ee-124">The following example defines a resource to deploy by using the objects shown previously to set values.</span></span> <span data-ttu-id="210ee-125">Till exempel VM-storlek anges genom att hämta värdet för `variables('tshirtSize').vmSize` när värdet för diskens storlek hämtas från `variables('tshirtSize').diskSize`.</span><span class="sxs-lookup"><span data-stu-id="210ee-125">For example, the VM size is set by retrieving the value for `variables('tshirtSize').vmSize` while the value for the disk size is retrieved from `variables('tshirtSize').diskSize`.</span></span> <span data-ttu-id="210ee-126">Dessutom URI för en länkad mall har angetts med värdet för `variables('tshirtSize').vmTemplate`.</span><span class="sxs-lookup"><span data-stu-id="210ee-126">In addition, the URI for a linked template is set with the value for `variables('tshirtSize').vmTemplate`.</span></span>

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

## <a name="pass-state-to-a-template"></a><span data-ttu-id="210ee-127">Skicka status till en mall</span><span class="sxs-lookup"><span data-stu-id="210ee-127">Pass state to a template</span></span>
<span data-ttu-id="210ee-128">Du kan dela tillstånd till en mall med parametrar som du anger direkt under distributionen.</span><span class="sxs-lookup"><span data-stu-id="210ee-128">You share state into a template through parameters that you provide directly during deployment.</span></span>

<span data-ttu-id="210ee-129">I följande tabell visas vanliga parametrar i mallar.</span><span class="sxs-lookup"><span data-stu-id="210ee-129">The following table lists commonly used parameters in templates.</span></span>

| <span data-ttu-id="210ee-130">Namn</span><span class="sxs-lookup"><span data-stu-id="210ee-130">Name</span></span> | <span data-ttu-id="210ee-131">Värde</span><span class="sxs-lookup"><span data-stu-id="210ee-131">Value</span></span> | <span data-ttu-id="210ee-132">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="210ee-132">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="210ee-133">location</span><span class="sxs-lookup"><span data-stu-id="210ee-133">location</span></span> |<span data-ttu-id="210ee-134">Sträng från en begränsad lista med Azure-regioner</span><span class="sxs-lookup"><span data-stu-id="210ee-134">String from a constrained list of Azure regions</span></span> |<span data-ttu-id="210ee-135">Den plats där resurserna som har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="210ee-135">The location where the resources are deployed.</span></span> |
| <span data-ttu-id="210ee-136">storageAccountNamePrefix</span><span class="sxs-lookup"><span data-stu-id="210ee-136">storageAccountNamePrefix</span></span> |<span data-ttu-id="210ee-137">Sträng</span><span class="sxs-lookup"><span data-stu-id="210ee-137">String</span></span> |<span data-ttu-id="210ee-138">Unikt DNS-namn för det Lagringskonto där den Virtuella datorns diskar placeras</span><span class="sxs-lookup"><span data-stu-id="210ee-138">Unique DNS name for the Storage Account where the VM's disks are placed</span></span> |
| <span data-ttu-id="210ee-139">Domännamn</span><span class="sxs-lookup"><span data-stu-id="210ee-139">domainName</span></span> |<span data-ttu-id="210ee-140">Sträng</span><span class="sxs-lookup"><span data-stu-id="210ee-140">String</span></span> |<span data-ttu-id="210ee-141">Domännamnet för offentligt tillgänglig jumpbox VM i formatet: **{domainName}. { Location}.cloudapp.com** till exempel: **mydomainname.westus.cloudapp.azure.com**</span><span class="sxs-lookup"><span data-stu-id="210ee-141">Domain name of the publicly accessible jumpbox VM in the format: **{domainName}.{location}.cloudapp.com** For example: **mydomainname.westus.cloudapp.azure.com**</span></span> |
| <span data-ttu-id="210ee-142">adminUsername</span><span class="sxs-lookup"><span data-stu-id="210ee-142">adminUsername</span></span> |<span data-ttu-id="210ee-143">Sträng</span><span class="sxs-lookup"><span data-stu-id="210ee-143">String</span></span> |<span data-ttu-id="210ee-144">Användarnamn för de virtuella datorerna</span><span class="sxs-lookup"><span data-stu-id="210ee-144">Username for the VMs</span></span> |
| <span data-ttu-id="210ee-145">adminPassword</span><span class="sxs-lookup"><span data-stu-id="210ee-145">adminPassword</span></span> |<span data-ttu-id="210ee-146">Sträng</span><span class="sxs-lookup"><span data-stu-id="210ee-146">String</span></span> |<span data-ttu-id="210ee-147">Lösenord för de virtuella datorerna</span><span class="sxs-lookup"><span data-stu-id="210ee-147">Password for the VMs</span></span> |
| <span data-ttu-id="210ee-148">tshirtSize</span><span class="sxs-lookup"><span data-stu-id="210ee-148">tshirtSize</span></span> |<span data-ttu-id="210ee-149">Sträng från en begränsad lista över erbjuds t-shirt storlekar</span><span class="sxs-lookup"><span data-stu-id="210ee-149">String from a constrained list of offered t-shirt sizes</span></span> |<span data-ttu-id="210ee-150">Storleken på namngivna skala att etablera.</span><span class="sxs-lookup"><span data-stu-id="210ee-150">The named scale unit size to provision.</span></span> <span data-ttu-id="210ee-151">Till exempel ”liten”, ”medel”, ”stor”</span><span class="sxs-lookup"><span data-stu-id="210ee-151">For example, "Small", "Medium", "Large"</span></span> |
| <span data-ttu-id="210ee-152">virtualNetworkName</span><span class="sxs-lookup"><span data-stu-id="210ee-152">virtualNetworkName</span></span> |<span data-ttu-id="210ee-153">Sträng</span><span class="sxs-lookup"><span data-stu-id="210ee-153">String</span></span> |<span data-ttu-id="210ee-154">Namnet på det virtuella nätverk som du vill att klienten ska använda.</span><span class="sxs-lookup"><span data-stu-id="210ee-154">Name of the virtual network that the consumer wants to use.</span></span> |
| <span data-ttu-id="210ee-155">enableJumpbox</span><span class="sxs-lookup"><span data-stu-id="210ee-155">enableJumpbox</span></span> |<span data-ttu-id="210ee-156">Sträng från en begränsad lista (aktiverat/inaktiverat)</span><span class="sxs-lookup"><span data-stu-id="210ee-156">String from a constrained list (enabled/disabled)</span></span> |<span data-ttu-id="210ee-157">Parameter som anger om du vill aktivera en jumpbox för miljön.</span><span class="sxs-lookup"><span data-stu-id="210ee-157">Parameter that identifies whether to enable a jumpbox for the environment.</span></span> <span data-ttu-id="210ee-158">Värden: ”aktiverad”, ”inaktiverad”</span><span class="sxs-lookup"><span data-stu-id="210ee-158">Values: "enabled", "disabled"</span></span> |

<span data-ttu-id="210ee-159">Den **tshirtSize** parameter används i föregående avsnitt definieras som:</span><span class="sxs-lookup"><span data-stu-id="210ee-159">The **tshirtSize** parameter used in the previous section is defined as:</span></span>

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
          "Description": "T-shirt size of the MongoDB deployment"
        }
      }
    }


## <a name="pass-state-to-linked-templates"></a><span data-ttu-id="210ee-160">Skicka status till länkade mallar</span><span class="sxs-lookup"><span data-stu-id="210ee-160">Pass state to linked templates</span></span>
<span data-ttu-id="210ee-161">När du ansluter till länkade mallar kan du ofta använder en blandning av statiska och genereras variabler.</span><span class="sxs-lookup"><span data-stu-id="210ee-161">When connecting to linked templates, you often use a mix of static and generated variables.</span></span>

### <a name="static-variables"></a><span data-ttu-id="210ee-162">Statiska variabler</span><span class="sxs-lookup"><span data-stu-id="210ee-162">Static variables</span></span>
<span data-ttu-id="210ee-163">Statiska variabler används ofta för att ge grundläggande värden, till exempel URL: er som används i en mall.</span><span class="sxs-lookup"><span data-stu-id="210ee-163">Static variables are often used to provide base values, such as URLs, that are used throughout a template.</span></span>

<span data-ttu-id="210ee-164">I följande utdrag i mallen `templateBaseUrl` anger var roten för mallen i GitHub.</span><span class="sxs-lookup"><span data-stu-id="210ee-164">In the following template excerpt, `templateBaseUrl` specifies the root location for the template in GitHub.</span></span> <span data-ttu-id="210ee-165">Nästa rad skapar en ny variabel `sharedTemplateUrl` som sammanfogar bas-URL med kända namnet på mallen delade resurser.</span><span class="sxs-lookup"><span data-stu-id="210ee-165">The next line builds a new variable `sharedTemplateUrl` that concatenates the base URL with the known name of the shared resources template.</span></span> <span data-ttu-id="210ee-166">Under den raden är en variabel med komplexa objekt används för att lagra en t-shirt storlek, där den grundläggande Webbadressen sammanfogas till platsen för mall kända konfiguration och lagras i den `vmTemplate` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="210ee-166">Below that line, a complex object variable is used to store a t-shirt size, where the base URL is concatenated to the known configuration template location and stored in the `vmTemplate` property.</span></span>

<span data-ttu-id="210ee-167">Fördelen med den här metoden är att om platsen mall ändras, behöver du bara ändra den statiska variabeln på ett ställe som passerar i hela länkade mallar.</span><span class="sxs-lookup"><span data-stu-id="210ee-167">The benefit of this approach is that if the template location changes, you only need to change the static variable in one place, which passes it throughout the linked templates.</span></span>

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

### <a name="generated-variables"></a><span data-ttu-id="210ee-168">Genererade variabler</span><span class="sxs-lookup"><span data-stu-id="210ee-168">Generated variables</span></span>
<span data-ttu-id="210ee-169">Förutom statiska variabler genereras flera variabler dynamiskt.</span><span class="sxs-lookup"><span data-stu-id="210ee-169">In addition to static variables, several variables are generated dynamically.</span></span> <span data-ttu-id="210ee-170">Det här avsnittet beskrivs några av vanliga genererade variabler.</span><span class="sxs-lookup"><span data-stu-id="210ee-170">This section identifies some of the common types of generated variables.</span></span>

#### <a name="tshirtsize"></a><span data-ttu-id="210ee-171">tshirtSize</span><span class="sxs-lookup"><span data-stu-id="210ee-171">tshirtSize</span></span>
<span data-ttu-id="210ee-172">Du är bekant med den här variabeln som genereras från exemplen ovan.</span><span class="sxs-lookup"><span data-stu-id="210ee-172">You are familiar with this generated variable from the examples above.</span></span>

#### <a name="networksettings"></a><span data-ttu-id="210ee-173">networkSettings</span><span class="sxs-lookup"><span data-stu-id="210ee-173">networkSettings</span></span>
<span data-ttu-id="210ee-174">I en kapacitet, kapaciteten eller begränsade lösningsmall för slutpunkt till slutpunkt Skapa länkade mallar normalt resurser som finns på ett nätverk.</span><span class="sxs-lookup"><span data-stu-id="210ee-174">In a capacity, capability, or end-to-end scoped solution template, the linked templates typically create resources that exist on a network.</span></span> <span data-ttu-id="210ee-175">Ett enkelt sätt är att använda ett komplext objekt för att lagra nätverksinställningar och skicka dem till länkade mallar.</span><span class="sxs-lookup"><span data-stu-id="210ee-175">One straightforward approach is to use a complex object to store network settings and pass them to linked templates.</span></span>

<span data-ttu-id="210ee-176">Du kan se ett exempel på kommunikation nätverksinställningar nedan.</span><span class="sxs-lookup"><span data-stu-id="210ee-176">An example of communicating network settings can be seen below.</span></span>

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

#### <a name="availabilitysettings"></a><span data-ttu-id="210ee-177">availabilitySettings</span><span class="sxs-lookup"><span data-stu-id="210ee-177">availabilitySettings</span></span>
<span data-ttu-id="210ee-178">Resurser som skapades i länkade mallar placeras ofta i en tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="210ee-178">Resources created in linked templates are often placed in an availability set.</span></span> <span data-ttu-id="210ee-179">I följande exempel anges tillgänglighetsuppsättningen och även feldomän och uppdateringsdomän räkna om du vill använda.</span><span class="sxs-lookup"><span data-stu-id="210ee-179">In the following example, the availability set name is specified and also the fault domain and update domain count to use.</span></span>

    "availabilitySetSettings": {
      "name": "pgsqlAvailabilitySet",
      "fdCount": 3,
      "udCount": 5
    }

<span data-ttu-id="210ee-180">Om du behöver flera tillgänglighetsuppsättningar (till exempel en för överordnade noder) och en annan för datanoder, kan du använda ett namn som ett prefix kan ange flera tillgänglighetsuppsättningar eller följa modellen som visades tidigare för att skapa en variabel för en viss t-shirt storlek.</span><span class="sxs-lookup"><span data-stu-id="210ee-180">If you need multiple availability sets (for example, one for master nodes and another for data nodes), you can use a name as a prefix, specify multiple availability sets, or follow the model shown earlier for creating a variable for a specific t-shirt size.</span></span>

#### <a name="storagesettings"></a><span data-ttu-id="210ee-181">storageSettings</span><span class="sxs-lookup"><span data-stu-id="210ee-181">storageSettings</span></span>
<span data-ttu-id="210ee-182">Lagringsinformation om som ofta delas med länkade mallar.</span><span class="sxs-lookup"><span data-stu-id="210ee-182">Storage details are often shared with linked templates.</span></span> <span data-ttu-id="210ee-183">I exemplet nedan, en *storageSettings* objekt innehåller information om de storage-konto och behållare.</span><span class="sxs-lookup"><span data-stu-id="210ee-183">In the example below, a *storageSettings* object provides details about the storage account and container names.</span></span>

    "storageSettings": {
        "vhdStorageAccountName": "[parameters('storageAccountName')]",
        "vhdContainerName": "[variables('vmStorageAccountContainerName')]",
        "destinationVhdsContainer": "[concat('https://', parameters('storageAccountName'), variables('vmStorageAccountDomain'), '/', variables('vmStorageAccountContainerName'), '/')]"
    }

#### <a name="ossettings"></a><span data-ttu-id="210ee-184">osSettings</span><span class="sxs-lookup"><span data-stu-id="210ee-184">osSettings</span></span>
<span data-ttu-id="210ee-185">Länkade mallar kan behöva du skicka operativsysteminställningar till olika typer av noder i olika för kända konfigurationstyper.</span><span class="sxs-lookup"><span data-stu-id="210ee-185">With linked templates, you may need to pass operating system settings to various nodes types across different known configuration types.</span></span> <span data-ttu-id="210ee-186">Ett komplext objekt är ett enkelt sätt att lagra och dela information om operativsystem så att det blir enklare att stödja flera operativsystem alternativ för distributionen.</span><span class="sxs-lookup"><span data-stu-id="210ee-186">A complex object is an easy way to store and share operating system information and also makes it easier to support multiple operating system choices for deployment.</span></span>

<span data-ttu-id="210ee-187">I följande exempel visas ett objekt för *osSettings*:</span><span class="sxs-lookup"><span data-stu-id="210ee-187">The following example shows an object for *osSettings*:</span></span>

    "osSettings": {
      "imageReference": {
        "publisher": "Canonical",
        "offer": "UbuntuServer",
        "sku": "14.04.2-LTS",
        "version": "latest"
      }
    }

#### <a name="machinesettings"></a><span data-ttu-id="210ee-188">machineSettings</span><span class="sxs-lookup"><span data-stu-id="210ee-188">machineSettings</span></span>
<span data-ttu-id="210ee-189">En genererad variabel *machineSettings* är ett komplext objekt som innehåller en blandning av kärnor variabler för att skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="210ee-189">A generated variable, *machineSettings* is a complex object containing a mix of core variables for creating a VM.</span></span> <span data-ttu-id="210ee-190">Variablerna omfattar administratörsanvändarnamn och lösenord, ett prefix för VM-namn och en referens för avbildningen av operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="210ee-190">The variables include administrator user name and password, a prefix for the VM names, and an operating system image reference.</span></span>

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

<span data-ttu-id="210ee-191">Observera att *osImageReference* hämtar värden från den *osSettings* variabel som definierats i mallen för huvudsakliga.</span><span class="sxs-lookup"><span data-stu-id="210ee-191">Note that *osImageReference* retrieves the values from the *osSettings* variable defined in the main template.</span></span> <span data-ttu-id="210ee-192">Det innebär att du kan enkelt ändra operativsystemet för en virtuell dator, helt eller baserat på inställningen för mallen konsumenter.</span><span class="sxs-lookup"><span data-stu-id="210ee-192">That means you can easily change the operating system for a VM—entirely or based on the preference of a template consumer.</span></span>

#### <a name="vmscripts"></a><span data-ttu-id="210ee-193">vmScripts</span><span class="sxs-lookup"><span data-stu-id="210ee-193">vmScripts</span></span>
<span data-ttu-id="210ee-194">Den *vmScripts* objekt innehåller information om skript för att hämta och köra på en VM-instans, inklusive utanför och inom referenser.</span><span class="sxs-lookup"><span data-stu-id="210ee-194">The *vmScripts* object contains details about the scripts to download and execute on a VM instance, including outside and inside references.</span></span> <span data-ttu-id="210ee-195">Utanför innehåller referenser infrastrukturen.</span><span class="sxs-lookup"><span data-stu-id="210ee-195">Outside references include the infrastructure.</span></span>
<span data-ttu-id="210ee-196">I referenser är installerad programvara installerad och konfiguration.</span><span class="sxs-lookup"><span data-stu-id="210ee-196">Inside references include the installed software installed and configuration.</span></span>

<span data-ttu-id="210ee-197">Du använder den *scriptsToDownload* egenskapen för att visa en lista över de skript som ska hämtas till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="210ee-197">You use the *scriptsToDownload* property to list the scripts to download to the VM.</span></span> <span data-ttu-id="210ee-198">Det här objektet innehåller också referenser till kommandoradsargument för olika typer av åtgärder.</span><span class="sxs-lookup"><span data-stu-id="210ee-198">This object also contains references to command-line arguments for different types of actions.</span></span> <span data-ttu-id="210ee-199">Dessa åtgärder omfattar köra standardinstallationen för varje enskild nod, en installation som körs när alla noder har distribuerats och ytterligare skript som är specifika för en viss mall.</span><span class="sxs-lookup"><span data-stu-id="210ee-199">These actions include executing the default installation for each individual node, an installation that runs after all nodes are deployed, and any additional scripts that may be specific to a given template.</span></span>

<span data-ttu-id="210ee-200">Det här exemplet är från en mall som används för att distribuera MongoDB, som kräver en avgörandet att ge hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="210ee-200">This example is from a template used to deploy MongoDB, which requires an arbiter to deliver high availability.</span></span> <span data-ttu-id="210ee-201">Den *arbiterNodeInstallCommand* har lagts till *vmScripts* installera avgörandet.</span><span class="sxs-lookup"><span data-stu-id="210ee-201">The *arbiterNodeInstallCommand* has been added to *vmScripts* to install the arbiter.</span></span>

<span data-ttu-id="210ee-202">Avsnittet variabler är där du hittar de variabler som definierar den specifika texten för att köra skriptet med rätt värden.</span><span class="sxs-lookup"><span data-stu-id="210ee-202">The variables section is where you find the variables that define the specific text to execute the script with the proper values.</span></span>

    "vmScripts": {
        "scriptsToDownload": [
            "[concat(variables('scriptUrl'), 'mongodb-', variables('osFamilySpec').osName, '-install.sh')]",
            "[concat(variables('sharedScriptUrl'), 'vm-disk-utils-0.1.sh')]"
        ],
        "regularNodeInstallCommand": "[variables('installCommand')]",
        "lastNodeInstallCommand": "[concat(variables('installCommand'), ' -l')]",
        "arbiterNodeInstallCommand": "[concat(variables('installCommand'), ' -a')]"
    },


## <a name="return-state-from-a-template"></a><span data-ttu-id="210ee-203">Returnera tillstånd från en mall</span><span class="sxs-lookup"><span data-stu-id="210ee-203">Return state from a template</span></span>
<span data-ttu-id="210ee-204">Inte bara kan du överföra data till en mall kan du också dela data tillbaka till anropande mallen.</span><span class="sxs-lookup"><span data-stu-id="210ee-204">Not only can you pass data into a template, you can also share data back to the calling template.</span></span> <span data-ttu-id="210ee-205">I den **matar ut** avsnitt i en länkad mall kan du ange nyckel/värde-par som kan användas av källmallen.</span><span class="sxs-lookup"><span data-stu-id="210ee-205">In the **outputs** section of a linked template, you can provide key/value pairs that can be consumed by the source template.</span></span>

<span data-ttu-id="210ee-206">I följande exempel visas hur du skickar den privata IP-adressen som genereras i en länkad mall.</span><span class="sxs-lookup"><span data-stu-id="210ee-206">The following example shows how to pass the private IP address generated in a linked template.</span></span>

    "outputs": {
        "masterip": {
            "value": "[reference(concat(variables('nicName'),0)).ipConfigurations[0].properties.privateIPAddress]",
            "type": "string"
         }
    }

<span data-ttu-id="210ee-207">Du kan använda dessa data med följande syntax i huvudsakliga mallen:</span><span class="sxs-lookup"><span data-stu-id="210ee-207">Within the main template, you can use that data with the following syntax:</span></span>

    "[reference('master-node').outputs.masterip.value]"

<span data-ttu-id="210ee-208">Du kan använda det här uttrycket i avsnittet utdata eller avsnittet resurser huvudsakliga mallen.</span><span class="sxs-lookup"><span data-stu-id="210ee-208">You can use this expression in either the outputs section or the resources section of the main template.</span></span> <span data-ttu-id="210ee-209">Du kan inte använda uttrycket i avsnittet variables eftersom den är beroende av runtime-tillståndet.</span><span class="sxs-lookup"><span data-stu-id="210ee-209">You cannot use the expression in the variables section because it relies on the runtime state.</span></span> <span data-ttu-id="210ee-210">Använd följande för att returnera värdet från den huvudsakliga mallen:</span><span class="sxs-lookup"><span data-stu-id="210ee-210">To return this value from the main template, use:</span></span>

    "outputs": {
      "masterIpAddress": {
        "value": "[reference('master-node').outputs.masterip.value]",
        "type": "string"
      }

<span data-ttu-id="210ee-211">Ett exempel på hur du använder avsnittet utdata för en länkad mall för att returnera datadiskar för en virtuell dator finns [skapar flera datadiskar för en virtuell dator](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="210ee-211">For an example of using the outputs section of a linked template to return data disks for a virtual machine, see [Creating multiple data disks for a Virtual Machine](resource-group-create-multiple.md).</span></span>

## <a name="define-authentication-settings-for-virtual-machine"></a><span data-ttu-id="210ee-212">Definiera autentiseringsinställningar för den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="210ee-212">Define authentication settings for virtual machine</span></span>
<span data-ttu-id="210ee-213">Du kan använda samma mönster tidigare för konfigurationsinställningar för att ange inställningarna för autentisering för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="210ee-213">You can use the same pattern shown previously for configuration settings to specify the authentication settings for a virtual machine.</span></span> <span data-ttu-id="210ee-214">Du skapar en parameter för typ av autentisering.</span><span class="sxs-lookup"><span data-stu-id="210ee-214">You create a parameter for passing in the type of authentication.</span></span>

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

<span data-ttu-id="210ee-215">Du lägger till variabler för de olika typerna av autentisering och en variabel för att lagra vilken typ som används för den här distributionen som baseras på värdet för parametern.</span><span class="sxs-lookup"><span data-stu-id="210ee-215">You add variables for the different authentication types, and a variable to store which type is used for this deployment based on the value of the parameter.</span></span>

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

<span data-ttu-id="210ee-216">När du definierar den virtuella datorn kan du ange den **osProfile** till variabeln som du skapade.</span><span class="sxs-lookup"><span data-stu-id="210ee-216">When defining the virtual machine, you set the **osProfile** to the variable you created.</span></span>

    {
      "type": "Microsoft.Compute/virtualMachines",
      ...
      "osProfile": "[variables('osProfile')]"
    }


## <a name="next-steps"></a><span data-ttu-id="210ee-217">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="210ee-217">Next steps</span></span>
* <span data-ttu-id="210ee-218">Läs om avsnitt i mallen i [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md)</span><span class="sxs-lookup"><span data-stu-id="210ee-218">To learn about the sections of the template, see [Authoring Azure Resource Manager Templates](resource-group-authoring-templates.md)</span></span>
* <span data-ttu-id="210ee-219">De funktioner som är tillgängliga i en mall finns [Azure Resource Manager Mallfunktioner](resource-group-template-functions.md)</span><span class="sxs-lookup"><span data-stu-id="210ee-219">To see the functions that are available within a template, see [Azure Resource Manager Template Functions](resource-group-template-functions.md)</span></span>

---
title: "Framtvinga säkerhet med principer på virtuella Windows-datorer i Azure | Microsoft Docs"
description: "Hur du använder en princip till en Azure Resource Manager Windows virtuell dator"
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 0b71ba54-01db-43ad-9bca-8ab358ae141b
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: kasing
ms.openlocfilehash: 246f5958478fd6d9afc9ba990413ab08429bd25d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="apply-policies-to-windows-vms-with-azure-resource-manager"></a><span data-ttu-id="f6114-103">Tillämpa principer för virtuella Windows-datorer med Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="f6114-103">Apply policies to Windows VMs with Azure Resource Manager</span></span>
<span data-ttu-id="f6114-104">En organisation kan tillämpa olika konventioner och regler i hela företaget med hjälp av principer.</span><span class="sxs-lookup"><span data-stu-id="f6114-104">By using policies, an organization can enforce various conventions and rules throughout the enterprise.</span></span> <span data-ttu-id="f6114-105">Tillämpning av önskat beteende kan du minimera risken när bidrar till att organisationen.</span><span class="sxs-lookup"><span data-stu-id="f6114-105">Enforcement of the desired behavior can help mitigate risk while contributing to the success of the organization.</span></span> <span data-ttu-id="f6114-106">I den här artikeln beskriver vi hur du kan använda principer för Azure Resource Manager för att definiera önskat beteende för virtuella datorer i din organisation.</span><span class="sxs-lookup"><span data-stu-id="f6114-106">In this article, we describe how you can use Azure Resource Manager policies to define the desired behavior for your organization’s Virtual Machines.</span></span>

<span data-ttu-id="f6114-107">En introduktion till principer, se [använda princip för att hantera resurser och åtkomstkontroll](../../azure-resource-manager/resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="f6114-107">For an introduction to policies, see [Use Policy to manage resources and control access](../../azure-resource-manager/resource-manager-policy.md).</span></span>

## <a name="permitted-virtual-machines"></a><span data-ttu-id="f6114-108">Tillåtna virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="f6114-108">Permitted Virtual Machines</span></span>
<span data-ttu-id="f6114-109">För att säkerställa att virtuella datorer för din organisation är kompatibla med ett program, kan du begränsa de tillåtna operativsystem.</span><span class="sxs-lookup"><span data-stu-id="f6114-109">To ensure that virtual machines for your organization are compatible with an application, you can restrict the permitted operating systems.</span></span> <span data-ttu-id="f6114-110">I exemplet nedan principen Tillåt endast Windows Server 2012 R2 Datacenter virtuella datorer som ska skapas:</span><span class="sxs-lookup"><span data-stu-id="f6114-110">In the following policy example, you allow only Windows Server 2012 R2 Datacenter Virtual Machines to be created:</span></span>

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "in": [
          "Microsoft.Compute/disks",
          "Microsoft.Compute/virtualMachines",
          "Microsoft.Compute/VirtualMachineScaleSets"
        ]
      },
      {
        "not": {
          "allOf": [
            {
              "field": "Microsoft.Compute/imagePublisher",
              "in": [
                "MicrosoftWindowsServer"
              ]
            },
            {
              "field": "Microsoft.Compute/imageOffer",
              "in": [
                "WindowsServer"
              ]
            },
            {
              "field": "Microsoft.Compute/imageSku",
              "in": [
                "2012-R2-Datacenter"
              ]
            },
            {
              "field": "Microsoft.Compute/imageVersion",
              "in": [
                "latest"
              ]
            }
          ]
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

<span data-ttu-id="f6114-111">Du kan använda jokertecken för att ändra föregående princip för att tillåta alla Windows Server Datacenter-avbildning:</span><span class="sxs-lookup"><span data-stu-id="f6114-111">Use a wild card to modify the preceding policy to allow any Windows Server Datacenter image:</span></span>

```json
{
  "field": "Microsoft.Compute/imageSku",
  "like": "*Datacenter"
}
```

<span data-ttu-id="f6114-112">Använd anyOf för att ändra föregående princip för att tillåta alla Windows Server 2012 R2 Datacenter eller högre avbildningen:</span><span class="sxs-lookup"><span data-stu-id="f6114-112">Use anyOf to modify the preceding policy to allow any Windows Server 2012 R2 Datacenter or higher image:</span></span>

```json
{
  "anyOf": [
    {
      "field": "Microsoft.Compute/imageSku",
      "like": "2012-R2-Datacenter*"
    },
    {
      "field": "Microsoft.Compute/imageSku",
      "like": "2016-Datacenter*"
    }
  ]
}
```

<span data-ttu-id="f6114-113">Information om principfält finns [princip alias](../../azure-resource-manager/resource-manager-policy.md#aliases).</span><span class="sxs-lookup"><span data-stu-id="f6114-113">For information about policy fields, see [Policy aliases](../../azure-resource-manager/resource-manager-policy.md#aliases).</span></span>

## <a name="managed-disks"></a><span data-ttu-id="f6114-114">Hanterade diskar</span><span class="sxs-lookup"><span data-stu-id="f6114-114">Managed disks</span></span>

<span data-ttu-id="f6114-115">Om du vill kräva användning av hanterade diskar, använder du följande princip:</span><span class="sxs-lookup"><span data-stu-id="f6114-115">To require the use of managed disks, use the following policy:</span></span>

```json
{
  "if": {
    "anyOf": [
      {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Compute/virtualMachines"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/osDisk.uri",
            "exists": true
          }
        ]
      },
      {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Compute/VirtualMachineScaleSets"
          },
          {
            "anyOf": [
              {
                "field": "Microsoft.Compute/VirtualMachineScaleSets/osDisk.vhdContainers",
                "exists": true
              },
              {
                "field": "Microsoft.Compute/VirtualMachineScaleSets/osdisk.imageUrl",
                "exists": true
              }
            ]
          }
        ]
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="images-for-virtual-machines"></a><span data-ttu-id="f6114-116">Avbildningar för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="f6114-116">Images for Virtual Machines</span></span>

<span data-ttu-id="f6114-117">Av säkerhetsskäl bör kräva du att godkända anpassade avbildningar distribueras i din miljö.</span><span class="sxs-lookup"><span data-stu-id="f6114-117">For security reasons, you can require that only approved custom images are deployed in your environment.</span></span> <span data-ttu-id="f6114-118">Du kan ange antingen resursgruppen som innehåller godkända bilder eller specifika godkända bilder.</span><span class="sxs-lookup"><span data-stu-id="f6114-118">You can specify either the resource group that contains the approved images, or the specific approved images.</span></span>

<span data-ttu-id="f6114-119">I följande exempel kräver avbildningar från en godkänd resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="f6114-119">The following example requires images from an approved resource group:</span></span>

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "in": [
                    "Microsoft.Compute/virtualMachines",
                    "Microsoft.Compute/VirtualMachineScaleSets"
                ]
            },
            {
                "not": {
                    "field": "Microsoft.Compute/imageId",
                    "contains": "resourceGroups/CustomImage"
                }
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
} 
```

<span data-ttu-id="f6114-120">I följande exempel anger godkända image-ID: N:</span><span class="sxs-lookup"><span data-stu-id="f6114-120">The following example specifies the approved image IDs:</span></span>

```json
{
    "field": "Microsoft.Compute/imageId",
    "in": ["{imageId1}","{imageId2}"]
}
```

## <a name="virtual-machine-extensions"></a><span data-ttu-id="f6114-121">Tillägg för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="f6114-121">Virtual Machine extensions</span></span>

<span data-ttu-id="f6114-122">Du kanske vill förbjuda användningen av vissa typer av tillägg.</span><span class="sxs-lookup"><span data-stu-id="f6114-122">You may want to forbid usage of certain types of extensions.</span></span> <span data-ttu-id="f6114-123">Till exempel kanske ett tillägg inte kompatibelt med vissa virtuella datoravbildningar.</span><span class="sxs-lookup"><span data-stu-id="f6114-123">For example, an extension may not be compatible with certain custom virtual machine images.</span></span> <span data-ttu-id="f6114-124">I följande exempel visas hur du blockerar ett specifikt filnamnstillägg.</span><span class="sxs-lookup"><span data-stu-id="f6114-124">The following example shows how to block a specific extension.</span></span> <span data-ttu-id="f6114-125">Används för utgivare och typ för att bestämma vilka tillägg som ska blockeras.</span><span class="sxs-lookup"><span data-stu-id="f6114-125">It uses publisher and type to determine which extension to block.</span></span>

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "equals": "Microsoft.Compute/virtualMachines/extensions"
            },
            {
                "field": "Microsoft.Compute/virtualMachines/extensions/publisher",
                "equals": "Microsoft.Compute"
            },
            {
                "field": "Microsoft.Compute/virtualMachines/extensions/type",
                "equals": "{extension-type}"

      }
        ]
    },
    "then": {
        "effect": "deny"
    }
}
```


## <a name="azure-hybrid-use-benefit"></a><span data-ttu-id="f6114-126">Hybridrapportering i Azure används förmån</span><span class="sxs-lookup"><span data-stu-id="f6114-126">Azure Hybrid Use Benefit</span></span>

<span data-ttu-id="f6114-127">När du har en licens för lokala sparar du licens avgift på virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="f6114-127">When you have an on-premise license, you can save the license fee on your virtual machines.</span></span> <span data-ttu-id="f6114-128">När du inte har licensen som bör du förbjuda alternativet.</span><span class="sxs-lookup"><span data-stu-id="f6114-128">When you don't have the license, you should forbid the option.</span></span> <span data-ttu-id="f6114-129">Följande princip tillåter inte användning av Azure Hybrid Använd förmånen (AHUB):</span><span class="sxs-lookup"><span data-stu-id="f6114-129">The following policy forbids usage of Azure Hybrid Use Benefit (AHUB):</span></span>

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "in":[ "Microsoft.Compute/virtualMachines","Microsoft.Compute/VirtualMachineScaleSets"]
            },
            {
                "field": "Microsoft.Compute/licenseType",
                "exists": true
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="f6114-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f6114-130">Next steps</span></span>
* <span data-ttu-id="f6114-131">När du definierar en regel (som visas i föregående exempel) behöver du skapar principdefinitionen och kopplar den till ett omfång.</span><span class="sxs-lookup"><span data-stu-id="f6114-131">After defining a policy rule (as shown in the preceding examples), you need to create the policy definition and assign it to a scope.</span></span> <span data-ttu-id="f6114-132">Omfattningen kan vara en prenumeration, resursgrupp eller resurs.</span><span class="sxs-lookup"><span data-stu-id="f6114-132">The scope can be a subscription, resource group, or resource.</span></span> <span data-ttu-id="f6114-133">Om du vill tilldela principer via portalen finns [Använd Azure-portalen för att tilldela och hantera resursprinciper](../../azure-resource-manager/resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f6114-133">To assign policies through the portal, see [Use Azure portal to assign and manage resource policies](../../azure-resource-manager/resource-manager-policy-portal.md).</span></span> <span data-ttu-id="f6114-134">Om du vill tilldela principer via REST-API, PowerShell eller Azure CLI, se [tilldela och hantera principer via skript](../../azure-resource-manager/resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="f6114-134">To assign policies through REST API, PowerShell or Azure CLI, see [Assign and manage policies through script](../../azure-resource-manager/resource-manager-policy-create-assign.md).</span></span>
* <span data-ttu-id="f6114-135">En introduktion till resursprinciper finns [resurs uppgifter](../../azure-resource-manager/resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="f6114-135">For an introduction to resource policies, see [Resource policy overview](../../azure-resource-manager/resource-manager-policy.md).</span></span>
* <span data-ttu-id="f6114-136">Vägledning för hur företag kan använda resurshanteraren för att effektivt hantera prenumerationer finns i [Azure enterprise scaffold - förebyggande prenumerationsåtgärder](../../azure-resource-manager/resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="f6114-136">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](../../azure-resource-manager/resource-manager-subscription-governance.md).</span></span>

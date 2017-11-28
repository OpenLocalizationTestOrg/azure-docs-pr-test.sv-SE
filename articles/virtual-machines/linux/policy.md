---
title: "aaaEnforce säkerheten med hjälp av principer på de virtuella Linux-datorer i Azure | Microsoft Docs"
description: Hur tooapply princip-tooan Azure Resource Manager Linux-dator
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 06778ab4-f8ff-4eed-ae10-26a276fc3faa
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: singhkay
ms.openlocfilehash: 5abd0c937578aba7e72b62c65b4eef9a9737aa2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="apply-policies-toolinux-vms-with-azure-resource-manager"></a><span data-ttu-id="a3a4c-103">Tillämpa principer tooLinux virtuella datorer med Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="a3a4c-103">Apply policies tooLinux VMs with Azure Resource Manager</span></span>
<span data-ttu-id="a3a4c-104">En organisation kan tillämpa olika konventioner och regler i hello företag med hjälp av principer.</span><span class="sxs-lookup"><span data-stu-id="a3a4c-104">By using policies, an organization can enforce various conventions and rules throughout hello enterprise.</span></span> <span data-ttu-id="a3a4c-105">Tvingande av hello önskad beteendet kan att minska risken vid bidrar toohello framgång hello organisation.</span><span class="sxs-lookup"><span data-stu-id="a3a4c-105">Enforcement of hello desired behavior can help mitigate risk while contributing toohello success of hello organization.</span></span> <span data-ttu-id="a3a4c-106">I den här artikeln beskriver vi hur du kan använda Azure Resource Manager principer toodefine hello önskad beteendet för virtuella datorer i din organisation.</span><span class="sxs-lookup"><span data-stu-id="a3a4c-106">In this article, we describe how you can use Azure Resource Manager policies toodefine hello desired behavior for your organization's Virtual Machines.</span></span>

<span data-ttu-id="a3a4c-107">En introduktion toopolicies finns [använda princip toomanage resurser och kontrollera åtkomst](../../azure-resource-manager/resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="a3a4c-107">For an introduction toopolicies, see [Use Policy toomanage resources and control access](../../azure-resource-manager/resource-manager-policy.md).</span></span>

## <a name="permitted-virtual-machines"></a><span data-ttu-id="a3a4c-108">Tillåtna virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="a3a4c-108">Permitted Virtual Machines</span></span>
<span data-ttu-id="a3a4c-109">tooensure att virtuella datorer för din organisation är kompatibla med ett program, kan du begränsa hello tillåtna operativsystem.</span><span class="sxs-lookup"><span data-stu-id="a3a4c-109">tooensure that virtual machines for your organization are compatible with an application, you can restrict hello permitted operating systems.</span></span> <span data-ttu-id="a3a4c-110">Hello som följande exempel princip du tillåter endast Ubuntu 14.04.2-LTS virtuella datorer toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="a3a4c-110">In hello following policy example, you allow only Ubuntu 14.04.2-LTS Virtual Machines toobe created.</span></span>

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
                "Canonical"
              ]
            },
            {
              "field": "Microsoft.Compute/imageOffer",
              "in": [
                "UbuntuServer"
              ]
            },
            {
              "field": "Microsoft.Compute/imageSku",
              "in": [
                "14.04.2-LTS"
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

<span data-ttu-id="a3a4c-111">Använd ett jokertecken toomodify hello föregående princip tooallow någon Ubuntu LTS bild:</span><span class="sxs-lookup"><span data-stu-id="a3a4c-111">Use a wild card toomodify hello preceding policy tooallow any Ubuntu LTS image:</span></span> 

```json
{
  "field": "Microsoft.Compute/virtualMachines/imageSku",
  "like": "*LTS"
}
```

<span data-ttu-id="a3a4c-112">Information om principfält finns [princip alias](../../azure-resource-manager/resource-manager-policy.md#aliases).</span><span class="sxs-lookup"><span data-stu-id="a3a4c-112">For information about policy fields, see [Policy aliases](../../azure-resource-manager/resource-manager-policy.md#aliases).</span></span>

## <a name="managed-disks"></a><span data-ttu-id="a3a4c-113">Hanterade diskar</span><span class="sxs-lookup"><span data-stu-id="a3a4c-113">Managed disks</span></span>

<span data-ttu-id="a3a4c-114">toorequire hello användning av hanterade diskar, Använd hello följande princip:</span><span class="sxs-lookup"><span data-stu-id="a3a4c-114">toorequire hello use of managed disks, use hello following policy:</span></span>

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

## <a name="images-for-virtual-machines"></a><span data-ttu-id="a3a4c-115">Avbildningar för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="a3a4c-115">Images for Virtual Machines</span></span>

<span data-ttu-id="a3a4c-116">Av säkerhetsskäl bör kräva du att godkända anpassade avbildningar distribueras i din miljö.</span><span class="sxs-lookup"><span data-stu-id="a3a4c-116">For security reasons, you can require that only approved custom images are deployed in your environment.</span></span> <span data-ttu-id="a3a4c-117">Du kan ange hello resursgruppen som innehåller hello godkända bilder eller hello specifika godkända bilder.</span><span class="sxs-lookup"><span data-stu-id="a3a4c-117">You can specify either hello resource group that contains hello approved images, or hello specific approved images.</span></span>

<span data-ttu-id="a3a4c-118">följande exempel hello kräver avbildningar från en godkänd resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="a3a4c-118">hello following example requires images from an approved resource group:</span></span>

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

<span data-ttu-id="a3a4c-119">hello anger följande exempel hello godkända bild ID: N:</span><span class="sxs-lookup"><span data-stu-id="a3a4c-119">hello following example specifies hello approved image IDs:</span></span>

```json
{
    "field": "Microsoft.Compute/imageId",
    "in": ["{imageId1}","{imageId2}"]
}
```

## <a name="virtual-machine-extensions"></a><span data-ttu-id="a3a4c-120">Tillägg för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="a3a4c-120">Virtual Machine extensions</span></span>

<span data-ttu-id="a3a4c-121">Du kanske vill tooforbid användningen av vissa typer av tillägg.</span><span class="sxs-lookup"><span data-stu-id="a3a4c-121">You may want tooforbid usage of certain types of extensions.</span></span> <span data-ttu-id="a3a4c-122">Till exempel kanske ett tillägg inte kompatibelt med vissa virtuella datoravbildningar.</span><span class="sxs-lookup"><span data-stu-id="a3a4c-122">For example, an extension may not be compatible with certain custom virtual machine images.</span></span> <span data-ttu-id="a3a4c-123">följande exempel visar hur hello tooblock ett specifikt filnamnstillägg.</span><span class="sxs-lookup"><span data-stu-id="a3a4c-123">hello following example shows how tooblock a specific extension.</span></span> <span data-ttu-id="a3a4c-124">Den använder utgivare och typen toodetermine vilka tillägg tooblock.</span><span class="sxs-lookup"><span data-stu-id="a3a4c-124">It uses publisher and type toodetermine which extension tooblock.</span></span>

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


## <a name="next-steps"></a><span data-ttu-id="a3a4c-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a3a4c-125">Next steps</span></span>
* <span data-ttu-id="a3a4c-126">När du har definierat en regel (som visas i föregående exempel hello) måste toocreate hello principdefinitionen och tilldela den tooa omfång.</span><span class="sxs-lookup"><span data-stu-id="a3a4c-126">After defining a policy rule (as shown in hello preceding examples), you need toocreate hello policy definition and assign it tooa scope.</span></span> <span data-ttu-id="a3a4c-127">hello scope kan vara en prenumeration, resursgrupp eller resurs.</span><span class="sxs-lookup"><span data-stu-id="a3a4c-127">hello scope can be a subscription, resource group, or resource.</span></span> <span data-ttu-id="a3a4c-128">tooassign principer hello-portalen finns i [Använd Azure portal tooassign och hantera resursprinciper](../../azure-resource-manager/resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a3a4c-128">tooassign policies through hello portal, see [Use Azure portal tooassign and manage resource policies](../../azure-resource-manager/resource-manager-policy-portal.md).</span></span> <span data-ttu-id="a3a4c-129">tooassign principer via REST-API, PowerShell eller Azure CLI, se [tilldela och hantera principer via skript](../../azure-resource-manager/resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="a3a4c-129">tooassign policies through REST API, PowerShell or Azure CLI, see [Assign and manage policies through script](../../azure-resource-manager/resource-manager-policy-create-assign.md).</span></span>
* <span data-ttu-id="a3a4c-130">En introduktion tooresource principer finns i [resurs uppgifter](../../azure-resource-manager/resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="a3a4c-130">For an introduction tooresource policies, see [Resource policy overview](../../azure-resource-manager/resource-manager-policy.md).</span></span>
* <span data-ttu-id="a3a4c-131">Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](../../azure-resource-manager/resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="a3a4c-131">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](../../azure-resource-manager/resource-manager-subscription-governance.md).</span></span>

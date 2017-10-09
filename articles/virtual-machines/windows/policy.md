---
title: "aaaEnforce säkerheten med hjälp av principer på virtuella Windows-datorer i Azure | Microsoft Docs"
description: Hur tooapply princip-tooan Azure Resource Manager Windows-dator
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
ms.openlocfilehash: b31c8a03ecf8eed6a929f97fe4146ea14364404f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="apply-policies-toowindows-vms-with-azure-resource-manager"></a>Tillämpa principer tooWindows virtuella datorer med Azure Resource Manager
En organisation kan tillämpa olika konventioner och regler i hello företag med hjälp av principer. Tvingande av hello önskad beteendet kan att minska risken vid bidrar toohello framgång hello organisation. I den här artikeln beskriver vi hur du kan använda Azure Resource Manager principer toodefine hello önskad beteendet för virtuella datorer i din organisation.

En introduktion toopolicies finns [använda princip toomanage resurser och kontrollera åtkomst](../../azure-resource-manager/resource-manager-policy.md).

## <a name="permitted-virtual-machines"></a>Tillåtna virtuella datorer
tooensure att virtuella datorer för din organisation är kompatibla med ett program, kan du begränsa hello tillåtna operativsystem. I följande exempel princip hello, Tillåt endast Windows Server 2012 R2 Datacenter-datorer toobe skapas:

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

Använd ett jokertecken toomodify hello föregående princip tooallow någon Windows Server Datacenter bild:

```json
{
  "field": "Microsoft.Compute/imageSku",
  "like": "*Datacenter"
}
```

Använd anyOf toomodify hello föregående princip tooallow alla Windows Server 2012 R2 Datacenter eller högre bild:

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

Information om principfält finns [princip alias](../../azure-resource-manager/resource-manager-policy.md#aliases).

## <a name="managed-disks"></a>Hanterade diskar

toorequire hello användning av hanterade diskar, Använd hello följande princip:

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

## <a name="images-for-virtual-machines"></a>Avbildningar för virtuella datorer

Av säkerhetsskäl bör kräva du att godkända anpassade avbildningar distribueras i din miljö. Du kan ange hello resursgruppen som innehåller hello godkända bilder eller hello specifika godkända bilder.

följande exempel hello kräver avbildningar från en godkänd resursgrupp:

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

hello anger följande exempel hello godkända bild ID: N:

```json
{
    "field": "Microsoft.Compute/imageId",
    "in": ["{imageId1}","{imageId2}"]
}
```

## <a name="virtual-machine-extensions"></a>Tillägg för virtuell dator

Du kanske vill tooforbid användningen av vissa typer av tillägg. Till exempel kanske ett tillägg inte kompatibelt med vissa virtuella datoravbildningar. följande exempel visar hur hello tooblock ett specifikt filnamnstillägg. Den använder utgivare och typen toodetermine vilka tillägg tooblock.

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


## <a name="azure-hybrid-use-benefit"></a>Hybridrapportering i Azure används förmån

När du har en licens för lokala sparar du hello licens avgift på virtuella datorer. När du inte har hello licens, bör du förbjuda hello-alternativet. hello följande princip tillåter inte användning av Azure Hybrid Använd förmånen (AHUB):

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

## <a name="next-steps"></a>Nästa steg
* När du har definierat en regel (som visas i föregående exempel hello) måste toocreate hello principdefinitionen och tilldela den tooa omfång. hello scope kan vara en prenumeration, resursgrupp eller resurs. tooassign principer hello-portalen finns i [Använd Azure portal tooassign och hantera resursprinciper](../../azure-resource-manager/resource-manager-policy-portal.md). tooassign principer via REST-API, PowerShell eller Azure CLI, se [tilldela och hantera principer via skript](../../azure-resource-manager/resource-manager-policy-create-assign.md).
* En introduktion tooresource principer finns i [resurs uppgifter](../../azure-resource-manager/resource-manager-policy.md).
* Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](../../azure-resource-manager/resource-manager-subscription-governance.md).

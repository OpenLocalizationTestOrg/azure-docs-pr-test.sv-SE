---
title: "aaaAnalyze nätverkssäkerhet med Azure Network Watcher säkerhet gruppvyn - REST API | Microsoft Docs"
description: "Den här artikeln beskriver hur toouse PowerShell tooanalyze en virtuella datorer säkerhet med Gruppvy för säkerhet."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: a2f418fe-f5d2-43ed-8dc3-df0ed2a4d4ac
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 0858a64a9454816e05f06dadb9536ad0c755e90e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-rest-api"></a>Analysera dina virtuella säkerhet med säkerhet gruppvyn med hjälp av REST API

> [!div class="op_single_selector"]
> - [PowerShell](network-watcher-security-group-view-powershell.md)
> - [CLI 1.0](network-watcher-security-group-view-cli-nodejs.md)
> - [CLI 2.0](network-watcher-security-group-view-cli.md)
> - [REST-API](network-watcher-security-group-view-rest.md)

Säkerhet gruppvyn returnerar konfigurerade och effektivt Nätverkssäkerhetsregler som är kopplade tooa virtuella datorn. Den här funktionen är användbart tooaudit och diagnostisera Nätverkssäkerhetsgrupper och regler som är konfigurerade på en VM tooensure trafik som tillåts eller nekas på rätt sätt. I den här artikeln visa vi hur tooretrieve hello effektiva och tillämpade säkerhetsregler tooa virtuell dator med hjälp av REST API

## <a name="before-you-begin"></a>Innan du börjar

I detta scenario anropa du hello nätverket Watcher Rest API tooget hello säkerhet gruppvyn för en virtuell dator. ARMclient är används toocall hello REST-API med hjälp av PowerShell. ARMClient hittas på chocolatey på [ARMClient på Chocolatey](https://chocolatey.org/packages/ARMClient)

Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren. hello scenariot förutsätter att en resursgrupp med en giltig virtuell dator finns toobe används.

## <a name="scenario"></a>Scenario

hello-scenario som beskrivs i den här artikeln hämtar hello effektiva och tillämpade säkerhetsregler för en viss virtuell dator.

## <a name="log-in-with-armclient"></a>Logga in med ARMClient

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a>Hämta en virtuell dator

Kör följande skript tooreturn virtuella machineThe hello måste följande kod variabler:

- **subscriptionId** -hello prenumerations-id kan också hämtas med hello **Get-AzureRMSubscription** cmdlet.
- **resourceGroupName** - hello namn på en resursgrupp som innehåller virtuella datorer.

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = '<resource group name>'

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

hello information som behövs är hello **id** under hello typ `Microsoft.Compute/virtualMachines` svar som visas i följande exempel hello:

```json
...,
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft
.Network/networkInterfaces/{nicName}"
            }
          ]
        },
        "provisioningState": "Succeeded"
      },
      "resources": [
        {
          "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Com
pute/virtualMachines/{vmName}/extensions/CustomScriptExtension"
        }
      ],
      "type": "Microsoft.Compute/virtualMachines",
      "location": "westcentralus",
      "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute
/virtualMachines/{vmName}",
      "name": "{vmName}"
    }
  ]
}
```

## <a name="get-security-group-view-for-virtual-machine"></a>Hämta gruppvy för säkerhet för den virtuella datorn

följande exempel hello begär hello säkerhet gruppvyn för en viss virtuell dator. hello resultat från det här exemplet kan vara används toocompare toohello regler och säkerhet som definierats av hello ursprung toolook konfigurationsavvikelser.

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>"
$networkWatcherName = "<network watcher name>"
$targetUri = "<uri of target resource>" # Example: /subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.compute/virtualMachine/$vmName

$requestBody = @"
{
    'targetResourceId': '${targetUri}'

}
"@
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/securityGroupView?api-version=2016-12-01" $requestBody -verbose
```

## <a name="view-hello-response"></a>Visa hello svar

följande exempel hello är hello svaret som returnerades från hello föregående kommando. hello resultatet visar alla hello effektiva och tillämpade säkerhetsregler på hello virtuella datorn är uppdelade i grupper med **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, och  **EffectiveSecurityRules**.

```json

{
  "networkInterfaces": [
    {
      "securityRuleAssociations": {
        "networkInterfaceAssociation": {
          "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkInterfaces/{nicName}",
          "securityRules": [
            {
              "name": "default-allow-rdp",
              "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}/securityRules/default-allow-rdp",
              "etag": "W/\"d4c411d4-0d62-49dc-8092-3d4b57825740\"",
              "properties": {
                "provisioningState": "Succeeded",
                "protocol": "TCP",
                "sourcePortRange": "*",
                "destinationPortRange": "3389",
                "sourceAddressPrefix": "*",
                "destinationAddressPrefix": "*",
                "access": "Allow",
                "priority": 1000,
                "direction": "Inbound"
              }
            }
          ]
        },
        "defaultSecurityRules": [
          {
            "name": "AllowVnetInBound",
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}/defaultSecurityRules/",
            "properties": {
              "provisioningState": "Succeeded",
              "description": "Allow inbound traffic from all VMs in VNET",
              "protocol": "*",
              "sourcePortRange": "*",
              "destinationPortRange": "*",
              "sourceAddressPrefix": "VirtualNetwork",
              "destinationAddressPrefix": "VirtualNetwork",
              "access": "Allow",
              "priority": 65000,
              "direction": "Inbound"
            }
          },
          ...
        ],
        "effectiveSecurityRules": [
          {
            "name": "DefaultOutboundDenyAll",
            "protocol": "All",
            "sourcePortRange": "0-65535",
            "destinationPortRange": "0-65535",
            "sourceAddressPrefix": "*",
            "destinationAddressPrefix": "*",
            "access": "Deny",
            "priority": 65500,
            "direction": "Outbound"
          },
          ...
        ]
      }
    }
  ]
}
```

## <a name="next-steps"></a>Nästa steg

Besök [granskning Nätverkssäkerhetsgrupp grupper (NSG) med Nätverksbevakaren](network-watcher-security-group-view-powershell.md) toolearn hur tooautomate validering av Nätverkssäkerhetsgrupper.



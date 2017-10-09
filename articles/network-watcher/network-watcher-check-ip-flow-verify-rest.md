---
title: Kontrollera aaaVerify trafik med Azure Network Watcher IP - REST | Microsoft Docs
description: "Den här artikeln beskriver hur toocheck om trafik tooor från en virtuell dator tillåts eller nekas"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 3307a79f-03be-46a0-aaaf-b2079cb5f3b2
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 956db0d326db597c6c402a9e8d4a5522c47c02d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-with-ip-flow-verify-a-component-of-azure-network-watcher"></a>Kontrollera om trafik tillåts eller nekas med IP-flöde verifiera en komponent i Azure Nätverksbevakaren

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-ip-flow-verify-portal.md)
> - [PowerShell](network-watcher-check-ip-flow-verify-powershell.md)
> - [CLI 1.0](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-ip-flow-verify-cli.md)
> - [Azure REST-API](network-watcher-check-ip-flow-verify-rest.md)


IP-flöde verifiera är en funktion i Nätverksbevakaren som gör att du tooverify om trafik tillåts tooor från en virtuell dator. hello verifiering kan köras för inkommande eller utgående trafik. Det här scenariot är användbart tooget aktuella tillstånd om en virtuell dator kan prata tooan extern resurs eller backend. IP-flöde verifiera går att använda tooverify om Nätverkssäkerhetsgrupp (NSG)-regler är korrekt konfigurerade och felsöka flöden som blockeras av NSG-regler. En annan orsak till att använda IP flödet Kontrollera tooensure trafik som du vill blockerade blockeras korrekt av hello NSG.

## <a name="before-you-begin"></a>Innan du börjar

ARMclient är används toocall hello REST-API med hjälp av PowerShell. ARMClient hittas på chocolatey på [ARMClient på Chocolatey](https://chocolatey.org/packages/ARMClient)

Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren.

## <a name="scenario"></a>Scenario

Det här scenariot använder IP-flöde Kontrollera tooverify om en virtuell dator kan prata tooanother datorn via port 443. Om hello trafik nekas returnerar hello säkerhetsregel som nekar trafiken. toolearn mer om IP-flöde kontrollera, besök [IP-flöde Kontrollera översikt](network-watcher-ip-flow-verify-overview.md)

I det här scenariot kan du:

* Hämta en virtuell dator
* Verifiera anropet IP-flöde
* Verifiera resultaten

## <a name="log-in-with-armclient"></a>Logga in med ARMClient

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a>Hämta en virtuell dator

Kör följande skript tooreturn hello en virtuell dator. hello följande kod måste ha värden för hello variabler:

* **subscriptionId** -hello prenumerations-Id toouse.
* **resourceGroupName** - hello namn på en resursgrupp som innehåller virtuella datorer.

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>"

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

hello information som behövs är hello-id under hello typ `Microsoft.Compute/virtualMachines`. hello resultaten ska vara liknande toohello följande kodexempel:

```json
...,
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "/subscriptions/{00000000-0000-0000-0000-000000000000}/resourceGroups/ContosoExampleRG/providers/Microsoft
.Network/networkInterfaces/contosovm842"
            }
          ]
        },
        "provisioningState": "Succeeded"
      },
      "resources": [
        {
          "id": "/subscriptions/{00000000-0000-0000-0000-000000000000}/resourceGroups/ContosoExampleRG/providers/Microsoft.Com
pute/virtualMachines/ContosoVM/extensions/CustomScriptExtension"
        }
      ],
      "type": "Microsoft.Compute/virtualMachines",
      "location": "westcentralus",
      "id": "/subscriptions/{00000000-0000-0000-0000-000000000000}/resourceGroups/ContosoExampleRG/providers/Microsoft.Compute
/virtualMachines/ContosoVM",
      "name": "ContosoVM"
    }
  ]
}
```

## <a name="call-ip-flow-verify"></a>Anropa IP-flöde Kontrollera

hello skapas följande exempel en begäran tooverify hello-trafik för en angiven virtuell dator. hello svar returnerar om hello trafik tillåts eller nekas hello trafik. Om trafik nekas den returnerar även vilka regeln block hello trafik.

> [!NOTE]
> IP-flöde Kontrollera kräver att hello Virtuella datorresursen allokeras.

hello skriptet kräver hello resursens Id för en virtuell dator och ett nätverkskort på hello virtuella datorn. Dessa värden tillhandahålls av hello föregående utdata.

> [!Important]
> För alla nätverk Watcher REST-anrop hello resursgruppens namn i hello Begärd URI är hello en som innehåller hello Nätverksbevakaren instansen inte hello resurser som du utför hello diagnostiska åtgärder på.

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>"
$networkWatcherName = "<network watcher name>"
$vmName = "<vm name>"
$vmNICName = "<vm NIC name>"
$direction = "<direction of traffic>" # Examples are: Inbound or Outbound
$localIP = "<source IP>"
$localPort = "<source Port>"
$remoteIP = "<destination IP>"
$remotePort = "<destination Port>" # Examples are: 80, or 80-120
$protocol = "<UDP, TCP or *>"
$targetUri = "<uri of target resource>" # Example: /subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.compute/virtualMachine/${vmName}
$targetNic = "<uri of target nic resource>" # Example: /subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkInterfaces/${vmNICName}

$requestBody = @"
{
    'targetResourceId':  '$targetUri',
    'direction':  '$direction',
    'protocol':  '$protocol',
    'localPort':  '$localPort',
    'remotePort':  '$remotePort',
    'localIPAddress':  '$localIP',
    'remoteIPAddress':  '$remoteIP',
    'targetNICResourceId':  '$targetNic'
}
"@
        
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/ipFlowVerify?api-version=2016-12-01" $requestBody -verbose
```

## <a name="understanding-hello-results"></a>Förstå hello resultat

hello svar åter berättar om hello trafik tillåts eller nekas. Det ser ut som något av följande exempel hello hello svar:

**Tillåtna**

```json
{
  "access": "Allow",
  "ruleName": "defaultSecurityRules/AllowInternetOutBound"
}
```

**Nekad**

```json
{
  "access": "Deny",
  "ruleName": "defaultSecurityRules/DefaultInboundDenyAll"
}
```

## <a name="next-steps"></a>Nästa steg

Om trafik blockeras och får inte vara, se [hantera Nätverkssäkerhetsgrupper](../virtual-network/virtual-network-manage-nsg-arm-portal.md) toolearn mer om Nätverkssäkerhetsgrupper.













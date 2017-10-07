---
title: "aaaAnalyze nätverkssäkerhet med Azure Network Watcher säkerhet gruppvyn - Azure CLI 1.0 | Microsoft Docs"
description: "Den här artikeln beskriver hur toouse Azure CLI 1.0 tooanalyze en virtuella datorer säkerhet med Gruppvy för säkerhet."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: a986ff4f-7e0c-4994-95e1-4ac824986500
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 96383a734b94d215d5b0f3d47339e46940d700b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-azure-cli-10"></a>Analysera dina virtuella säkerhet med säkerhet gruppvyn använder Azure CLI 1.0

> [!div class="op_single_selector"]
> - [PowerShell](network-watcher-security-group-view-powershell.md)
> - [CLI 1.0](network-watcher-security-group-view-cli-nodejs.md)
> - [CLI 2.0](network-watcher-security-group-view-cli.md)
> - [REST-API](network-watcher-security-group-view-rest.md)

Säkerhet gruppvyn returnerar konfigurerade och effektivt Nätverkssäkerhetsregler som är kopplade tooa virtuella datorn. Den här funktionen är användbart tooaudit och diagnostisera Nätverkssäkerhetsgrupper och regler som är konfigurerade på en VM tooensure trafik som tillåts eller nekas på rätt sätt. I den här artikeln visar vi hur tooretrieve hello konfigurerade och effektiv säkerhet regler tooa virtuell dator med Azure CLI

Den här artikeln använder plattformsoberoende Azure CLI version 1.0, som är tillgänglig för Windows, Mac och Linux. Nätverksbevakaren använder för närvarande Azure CLI 1.0 för CLI-stöd.

## <a name="before-you-begin"></a>Innan du börjar

Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren.

## <a name="scenario"></a>Scenario

hello-scenario som beskrivs i den här artikeln hämtar hello konfigurerad och effektiva säkerhetsregler för en viss virtuell dator.

## <a name="get-a-vm"></a>Hämta en virtuell dator

En virtuell dator är obligatoriska toorun hello `vm list` cmdlet. hello visar följande kommando hello virtuella machinese i en resursgrupp:

```azurecli
azure vm list -g resourceGroupName
```

När du vet hello virtuell dator kan du använda hello `vm show` cmdlet tooget dess resursens Id:

```azurecli
azure vm show -g resourceGroupName -n virtualMachineName
```

## <a name="retrieve-security-group-view"></a>Hämta gruppvy för säkerhet

hello nästa steg är tooretrieve hello säkerhet grupp visa resultatet. Att lägga till hello ”--json” flaggan formaterar hello resulterar i json.

```azurecli
azure network watcher security-group-view -g resourceGroupName -n networkWatcherName -t targetResourceId --json
```

## <a name="viewing-hello-results"></a>Visa hello resultat

hello är följande exempel ett kortare svar hello resultat returneras. hello resultatet visar alla hello effektiva och tillämpade säkerhetsregler på hello virtuella datorn är uppdelade i grupper med **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, och ** EffectiveSecurityRules**.

```json
{
  "networkInterfaces": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkInterfaces/testnic",
      "securityRuleAssociations": {
        "networkInterfaceAssociation": {
          "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkInterfaces/testvm",
          "securityRules": [
            {
              "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/test-nsg/securityRules/default-allow-rdp",
              "protocol": "TCP",
              "sourcePortRange": "*",
              "destinationPortRange": "3389",
              "sourceAddressPrefix": "*",
              "destinationAddressPrefix": "*",
              "access": "Allow",
              "priority": 1000,
              "direction": "Inbound",
              "provisioningState": "Succeeded",
              "name": "default-allow-rdp",
              "etag": "W/\"00000000-0000-0000-0000-000000000000\""
            }
          ]
        },
        "defaultSecurityRules": [
          {
            "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/networkSecurityGroups//defaultSecurityRules/",
            "description": "Allow inbound traffic from all VMs in VNET",
            "protocol": "*",
            "sourcePortRange": "*",
            "destinationPortRange": "*",
            "sourceAddressPrefix": "VirtualNetwork",
            "destinationAddressPrefix": "VirtualNetwork",
            "access": "Allow",
            "priority": 65000,
            "direction": "Inbound",
            "provisioningState": "Succeeded",
            "name": "AllowVnetInBound"
          }
        ]
      }
    }
  ]
}
```

## <a name="next-steps"></a>Nästa steg

Besök [granskning Nätverkssäkerhetsgrupp grupper (NSG) med Nätverksbevakaren](network-watcher-nsg-auditing-powershell.md) toolearn hur tooautomate validering av Nätverkssäkerhetsgrupper.

Mer information om hello säkerhetsregler som tillämpade tooyour nätverksresurser genom att besöka [Säkerhetsöversikt grupp vy](network-watcher-security-group-view-overview.md)

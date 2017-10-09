---
title: "aaaAutomate NSG granskning med Watcher nätverkssäkerhet för Azure gruppvyn | Microsoft Docs"
description: "Den här sidan innehåller instruktioner om hur tooconfigure granskning av en Nätverkssäkerhetsgrupp"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 78a01bcf-74fe-402a-9812-285f3501f877
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 24fc418c433fceaf55a74b7c3b0e354dc46c8729
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automate-nsg-auditing-with-azure-network-watcher-security-group-view"></a>Automatisera NSG granskning med vyn för Watcher nätverkssäkerhet för Azure-grupp

Kunder inför ofta hello utmaningen i att verifiera hello säkerhetsläget i sin infrastruktur. Denna utmaning skiljer sig inte för sina virtuella datorer i Azure. Det är viktigt toohave en liknande säkerhetsprofil baserat på hello Nätverkssäkerhetsgrupp (NSG) regler tillämpas. Med hello säkerhet gruppvyn kan nu du få hello lista över regler tillämpas tooa VM inom en NSG. Du kan definiera en gyllene NSG säkerhetsprofil och initiera Gruppvy för säkerhet i varje vecka takt och jämföra hello utdata toohello gyllene profil och skapa en rapport. Det här sättet kan du identifiera enkelt alla hello VMs som inte följer toohello föreskrivs säkerhetsprofil.

Om du är bekant med Nätverkssäkerhetsgrupper [översikt över säkerheten i nätverket](../virtual-network/virtual-networks-nsg.md)

## <a name="before-you-begin"></a>Innan du börjar

I det här scenariot kan du jämföra en säkerhetsgrupp för kända fungerande baslinjen toohello visa resultaten som returnerades för en virtuell dator.

Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren. hello scenariot förutsätter att en resursgrupp med en giltig virtuell dator finns toobe används.

## <a name="scenario"></a>Scenario

hello-scenario som beskrivs i den här artikeln hämtar hello säkerhet gruppvyn för en virtuell dator.

I det här scenariot kommer du att:

- Hämta en känd bra regeluppsättning
- Hämta en virtuell dator med Rest API
- Hämta gruppvy för säkerhet för den virtuella datorn
- Utvärdera svar

## <a name="retrieve-rule-set"></a>Hämta regeluppsättning

hello första steget i det här exemplet är toowork med en befintlig baslinje. hello följande exempel är vissa json som extraherats från en befintlig säkerhetsgrupp i nätverket med hjälp av hello `Get-AzureRmNetworkSecurityGroup` cmdlet som används som hello baslinje för det här exemplet.

```json
[
    {
        "Description":  null,
        "Protocol":  "TCP",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "3389",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Allow",
        "Priority":  1000,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "default-allow-rdp",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/default-allow-rdp"
    },
    {
        "Description":  null,
        "Protocol":  "*",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "111",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Allow",
        "Priority":  1010,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "MyRuleDoNotDelete",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/MyRuleDoNotDelete"
    },
    {
        "Description":  null,
        "Protocol":  "*",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "112",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Allow",
        "Priority":  1020,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "My2ndRuleDoNotDelete",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/My2ndRuleDoNotDelete"
    },
    {
        "Description":  null,
        "Protocol":  "TCP",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "5672",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Deny",
        "Priority":  1030,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "ThisRuleNeedsToStay",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/ThisRuleNeedsToStay"
    }
]
```

## <a name="convert-rule-set-toopowershell-objects"></a>Omvandla regeln set tooPowerShell objekt

I det här steget ska läser vi en json-fil som har skapats tidigare med hello regler som är förväntade toobe på hello Nätverkssäkerhetsgruppen för det här exemplet.

```powershell
$nsgbaserules = Get-Content -Path C:\temp\testvm1-nsg.json | ConvertFrom-Json
```

## <a name="retrieve-network-watcher"></a>Hämta Nätverksbevakaren

hello nästa steg är tooretrieve hello Nätverksbevakaren instans. Hej `$networkWatcher` variabel har skickats toohello `AzureRmNetworkWatcherSecurityGroupView` cmdlet.

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-vm"></a>Hämta en virtuell dator

En virtuell dator är obligatoriska toorun hello `Get-AzureRmNetworkWatcherSecurityGroupView` cmdlet mot. hello följande exempel hämtar ett VM-objekt.

```powershell
$VM = Get-AzurermVM -ResourceGroupName "testrg" -Name "testvm1"
```

## <a name="retrieve-security-group-view"></a>Hämta gruppvy för säkerhet

hello nästa steg är tooretrieve hello säkerhet grupp visa resultatet. Resultatet är jämfört med toohello ”baseline” json som visades tidigare.

```powershell
$secgroup = Get-AzureRmNetworkWatcherSecurityGroupView -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id
```

## <a name="analyzing-hello-results"></a>Analyserar hello resultat

hello svar är grupperad efter nätverksgränssnitt. hello olika typer av regler som returnerades är giltiga och standard säkerhetsregler. hello resultatet är ytterligare fördelade på hur den används, antingen på ett undernät eller ett virtuellt nätverkskort.

hello följande PowerShell-skript för att jämföra hello resultaten av hello säkerhet gruppvyn tooan befintliga utdata från en NSG. hello följande exempel är ett enkelt exempel på hur hello resultaten kan jämföras med `Compare-Object` cmdlet.

```powershell
Compare-Object -ReferenceObject $nsgbaserules `
-DifferenceObject $secgroup.NetworkInterfaces[0].NetworkInterfaceSecurityRules `
-Property Name,Description,Protocol,SourcePortRange,DestinationPortRange,SourceAddressPrefix,DestinationAddressPrefix,Access,Priority,Direction
```

följande exempel hello är hello resultat. Du kan se två hello regler som fanns i hello första regeluppsättning inte finns några hello jämförelse.

```
Name                     : My2ndRuleDoNotDelete
Description              : 
Protocol                 : *
SourcePortRange          : *
DestinationPortRange     : 112
SourceAddressPrefix      : *
DestinationAddressPrefix : *
Access                   : Allow
Priority                 : 1020
Direction                : Inbound
SideIndicator            : <=

Name                     : ThisRuleNeedsToStay
Description              : 
Protocol                 : TCP
SourcePortRange          : *
DestinationPortRange     : 5672
SourceAddressPrefix      : *
DestinationAddressPrefix : *
Access                   : Deny
Priority                 : 1030
Direction                : Inbound
SideIndicator            : <=
```

## <a name="next-steps"></a>Nästa steg

Om inställningarna har ändrats, se [hantera Nätverkssäkerhetsgrupper](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack hello network security grupp och säkerhet regler som är i fråga.














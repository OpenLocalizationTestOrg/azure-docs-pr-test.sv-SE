---
title: "aaaMonitor åtgärder, händelser och prestandaräknare för NSG: er | Microsoft Docs"
description: "Lär dig hur tooenable räknare, händelser och operativa loggning för NSG: er"
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 2e699078-043f-48bd-8aa8-b011a32d98ca
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/31/2017
ms.author: jdial
ms.openlocfilehash: f16f1a0ad693028ee7aba21574b5c8ddfcd27096
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-for-network-security-groups-nsgs"></a>Log Analytics för nätverkssäkerhetsgrupper (NSG:er)

Du kan aktivera hello följande diagnostiska loggen kategorier för NSG: er:

* **Händelse:** innehåller poster för vilka NSG regler är tillämpade tooVMs och instans roller baserat på MAC-adress. hello status för de här reglerna samlas var 60: e sekund.
* **Regelräknare:** innehåller poster för hur många gånger varje NSG regel är tillämpade toodeny eller Tillåt trafiken.

> [!NOTE]
> Diagnostikloggar är bara tillgängliga för NSG: er som distribueras via hello Azure Resource Manager-distributionsmodellen. Du kan inte aktivera diagnostikloggning för NSG: er som distribueras via hello klassiska distributionsmodellen. För bättre förståelse av hello två modeller, referera till hello [förstå Azure distributionsmodeller](../resource-manager-deployment-model.md) artikel.

Aktivitetsloggning (tidigare kallat granskningsläge eller operativa loggar) är aktiverad som standard för NSG: er som skapats via antingen Azure distributionsmodell. vilka åtgärder har slutförts på NSG: er i hello aktivitetsloggen, leta efter poster som innehåller följande typer av resurser hello toodetermine: 

- Microsoft.ClassicNetwork/networkSecurityGroups 
- Microsoft.ClassicNetwork/networkSecurityGroups/securityRules
- Microsoft.Network/networkSecurityGroups
- Microsoft.Network/networkSecurityGroups/securityRules 

Läs hello [översikt över hello Azure-aktivitetsloggen](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) artikel toolearn mer om aktivitetsloggar. 

## <a name="enable-diagnostic-logging"></a>Aktivera diagnostikloggning

Diagnostikloggning måste vara aktiverat för *varje* NSG önskade toocollect data för. Hej [översikt av Azure diagnostikloggar](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) artikeln förklarar var diagnostikloggar skickas. Om du inte har en befintlig NSG fullständig hello stegen i hello [skapar en nätverkssäkerhetsgrupp](virtual-networks-create-nsg-arm-pportal.md) artikel toocreate en. Du kan aktivera loggning med hjälp av följande metoder hello NSG:

### <a name="azure-portal"></a>Azure Portal

toouse hello portal tooenable loggning, inloggning toohello [portal](https://portal.azure.com). Klicka på **fler tjänster**, Skriv *nätverkssäkerhetsgrupper*. Välj hello NSG som du vill tooenable loggning för. Följ instruktionerna för hello för icke-beräkningsresurser i hello [aktivera diagnostikloggar i hello portal](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) artikel. Välj **NetworkSecurityGroupEvent**, **NetworkSecurityGroupRuleCounter**, eller båda kategorier av loggar.

### <a name="powershell"></a>PowerShell

toouse PowerShell tooenable loggning, följer du anvisningarna hello i hello [aktivera diagnostikloggar via PowerShell](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) artikel. Utvärdera hello följande information innan du anger ett kommando från hello-artikel:

- Du kan fastställa hello värdet toouse för hello `-ResourceId` parameter genom att ersätta hello följande [text] efter behov, hello kommandot `Get-AzureRmNetworkSecurityGroup -Name [nsg-name] -ResourceGroupName [resource-group-name]`. hello ID utdata från kommandot hello ser ut ungefär för*/subscriptions/ [prenumeration Id]/resourceGroups/[resource-group]/providers/Microsoft.Network/networkSecurityGroups/[NSG name]*.
- Om du bara vill toocollect data från loggen kategori lägger du till `-Categories [category]` toohello slutet hello kommandot i hello artikeln, där kategorin är antingen *NetworkSecurityGroupEvent* eller *NetworkSecurityGroupRuleCounter*. Om du inte använder hello `-Categories` parameter, datainsamling har aktiverats för både loggfilen kategorier.

### <a name="azure-command-line-interface-cli"></a>Azure-kommandoradsgränssnittet (CLI)

toouse Hej CLI tooenable loggning, följer du anvisningarna hello i hello [aktivera diagnostikloggar via CLI](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) artikel. Utvärdera hello följande information innan du anger ett kommando från hello-artikel:

- Du kan fastställa hello värdet toouse för hello `-ResourceId` parameter genom att ersätta hello följande [text] efter behov, hello kommandot `azure network nsg show [resource-group-name] [nsg-name]`. hello ID utdata från kommandot hello ser ut ungefär för*/subscriptions/ [prenumeration Id]/resourceGroups/[resource-group]/providers/Microsoft.Network/networkSecurityGroups/[NSG name]*.
- Om du bara vill toocollect data från loggen kategori lägger du till `-Categories [category]` toohello slutet hello kommandot i hello artikeln, där kategorin är antingen *NetworkSecurityGroupEvent* eller *NetworkSecurityGroupRuleCounter*. Om du inte använder hello `-Categories` parameter, datainsamling har aktiverats för både loggfilen kategorier.

## <a name="logged-data"></a>Loggdata

JSON-formaterade data skrivs för båda loggar. hello specifika data som skrivs för varje loggtyp av visas i hello följande avsnitt:

### <a name="event-log"></a>Händelseloggen
Den här loggfilen innehåller information om vilka NSG-regler är tillämpade tooVMs och cloud service rollinstanser baserat på MAC-adress. hello följande exempeldata loggas för varje händelse:

```json
{
    "time": "[DATE-TIME]",
    "systemId": "007d0441-5d6b-41f6-8bfd-930db640ec03",
    "category": "NetworkSecurityGroupEvent",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION-ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupEvents",
    "properties": {
        "vnetResourceGuid":"{5E8AEC16-C728-441F-B0CA-B791E1DBC2F4}",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"UserRule_default-allow-rdp",
        "direction":"In",
        "priority":1000,
        "type":"allow",
        "conditions":{
            "protocols":"6",
            "destinationPortRange":"3389-3389",
            "sourcePortRange":"0-65535",
            "sourceIP":"0.0.0.0/0",
            "destinationIP":"0.0.0.0/0"
            }
        }
}
```

### <a name="rule-counter-log"></a>Logg för räknaren

Den här loggfilen innehåller information om varje regel som tillämpas tooresources. hello följande exempeldata loggas varje gång som ska tillämpas:

```json
{
    "time": "[DATE-TIME]",
    "systemId": "007d0441-5d6b-41f6-8bfd-930db640ec03",
    "category": "NetworkSecurityGroupRuleCounter",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]TESTRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupCounters",
    "properties": {
        "vnetResourceGuid":"{5E8AEC16-C728-441F-B0CA-791E1DBC2F4}",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"UserRule_default-allow-rdp",
        "direction":"In",
        "type":"allow",
        "matchedConnections":125
        }
}
```

## <a name="view-and-analyze-logs"></a>Visa och analysera loggar

toolearn hur tooview aktivitet logga data, läsa hello [översikt över hello Azure-aktivitetsloggen](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) artikel. toolearn hur tooview diagnostik logga data, läsa hello [översikt av Azure diagnostikloggar](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) artikel. Om du skickar diagnostik data tooLog Analytics, kan du använda hello [Azure Network Security Group analytics](../log-analytics/log-analytics-azure-networking-analytics.md) lösning för förbättrad insights (förhandsversion). 

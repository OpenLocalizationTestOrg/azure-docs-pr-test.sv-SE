---
title: "aaaCreate en instans av Azure Nätverksbevakaren | Microsoft Docs"
description: "Den här sidan innehåller hello steg toocreate en instans av Nätverksbevakaren med hello-portalen och Azure REST API"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: b1314119-0b87-4f4d-b44c-2c4d0547fb76
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 90d4f90c9709a80e4b27863e79e5b6e16de145c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-network-watcher-instance"></a>Skapa en instans av Azure Nätverksbevakaren

Nätverksbevakaren är en regionala tjänst som gör att du toomonitor och diagnostisera villkor på nätverket scenariot nivå, till och från Azure. Scenariot nivån övervakning kan du toodiagnose problem vid slutet tooend nätverk på vyn. Diagnostiska nätverks- och visualiseringsverktyg som finns tillgängliga med Nätverksbevakaren hjälpa dig att förstå, diagnostisera och få insikter tooyour nätverk i Azure.

> [!NOTE]
> Nätverksbevakaren stöder för närvarande endast CLI 1.0, hello instruktioner toocreate en ny instans av Nätverksbevakaren har angetts för CLI 1.0.

## <a name="create-a-network-watcher-in-hello-portal"></a>Skapa en Nätverksbevakaren hello-portalen

Navigera för**fler tjänster** > **nätverk** > **Nätverksbevakaren**. Du kan välja alla hello prenumerationer som du vill tooenable Nätverksbevakaren för. Den här åtgärden skapar en Nätverksbevakaren i varje region som är tillgänglig.

![Skapa en nätverksbevakaren][1]

När du aktiverar Nätverksbevakaren med hello Portal anges hello namnet på hello Nätverksbevakaren instans automatiskt tooNetworkWatcher_region_name där region_name motsvarar toohello Azure-Region där hello-instansen har aktiverats.  Till exempel får en Nätverksbevakaren aktiverat i West centrala oss region namnet NetworkWatcher_westcentralus

Dessutom hello Nätverksbevakaren instans automatiskt till i en resursgrupp med namnet NetworkWatcherRG.  Den här resursgruppen ska skapas om den inte redan finns.

Om du inte vill toocustomize hello namnet på en instans av Nätverksbevakaren och hello resursgruppen har placerats i, kan du använda Powershell, hello REST API eller ARMClient metoder som beskrivs nedan.  I varje alternativ, hello resursgruppens namn måste finnas innan du placerar hello Nätverksbevakaren till den.  

## <a name="create-a-network-watcher-with-powershell"></a>Skapa en Nätverksbevakaren med PowerShell

toocreate en instans av Nätverksbevakaren kör hello följande exempel:

```powershell
New-AzureRmNetworkWatcher -Name "NetworkWatcher_westcentralus" -ResourceGroupName "NetworkWatcherRG" -Location "West Central US"
```

## <a name="create-a-network-watcher-with-hello-rest-api"></a>Skapa en Nätverksbevakaren med hello REST API

ARMclient är används toocall hello REST-API med hjälp av PowerShell. ARMClient hittas på chocolatey på [ARMClient på Chocolatey](https://chocolatey.org/packages/ARMClient)

### <a name="log-in-with-armclient"></a>Logga in med ARMClient

```powerShell
armclient login
```

### <a name="create-hello-network-watcher"></a>Skapa hello nätverksbevakaren

```powershell
$subscriptionId = '<subscription id>'
$networkWatcherName = '<name of network watcher>'
$resourceGroupName = '<resource group name>'
$apiversion = "2016-09-01"
$requestBody = @"
{
'location': 'West Central US'
}
"@

armclient put "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}?api-version=${api-version}" $requestBody
```

## <a name="next-steps"></a>Nästa steg

Nu när du har en instans av Nätverksbevakaren Lär dig mer om hello-funktioner som är tillgängliga:

* [Topologi](network-watcher-topology-overview.md)
* [Paketinsamling](network-watcher-packet-capture-overview.md)
* [Verifiera IP-flöde](network-watcher-ip-flow-verify-overview.md)
* [Nästa hopp](network-watcher-next-hop-overview.md)
* [Säkerhetsgruppvy](network-watcher-security-group-view-overview.md)
* [NSG flödet loggning](network-watcher-nsg-flow-logging-overview.md)
* [Felsökning av virtuella nätverksgateway](network-watcher-troubleshoot-overview.md)

När en instans av Nätverksbevakaren har skapats, paket avbilda kan konfigureras med följande hello-artikel: [skapar en avisering utlösta paketinsamling](network-watcher-alert-triggered-packet-capture.md)

[1]: ./media/network-watcher-create/figure1.png












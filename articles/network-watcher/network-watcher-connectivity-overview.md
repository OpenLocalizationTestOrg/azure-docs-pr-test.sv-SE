---
title: "aaaIntroduction tooconnectivity incheckning Azure Nätverksbevakaren | Microsoft Docs"
description: "Den här sidan innehåller en översikt över hello Nätverksbevakaren anslutning kapaciteten"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/11/2017
ms.author: gwallace
ms.openlocfilehash: 52fc4547f167cea2992a046859dc0550d136e80d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooconnectivity-check-in-azure-network-watcher"></a>Introduktion tooconnectivity incheckning Azure Nätverksbevakaren

hello anslutningen funktion i Nätverksbevakaren ger hello kapaciteten toocheck direktanslutning från en virtuell dator tooa virtuell dator (VM), fullständigt kvalificerade domännamnet (FQDN), URI, TCP eller IPv4-adress. Scenarier för nätverk är komplexa, implementeras med hjälp av Nätverkssäkerhetsgrupper, brandväggar, användardefinierade vägar och resurser som tillhandahålls av Azure. Komplexa konfigurationer gör det svårt felsöker problem med nätverksanslutningen. Nätverksbevakaren kan minska hello tid toofind och identifiera problem med nätverksanslutningen. hello resultaten kan ge insikter om ett anslutningsproblem förfaller tooa plattform eller ett problem med användare. Anslutningen kan kontrolleras med [PowerShell](network-watcher-connectivity-powershell.md), [Azure CLI](network-watcher-connectivity-cli.md), och [REST API](network-watcher-connectivity-rest.md).

> [!IMPORTANT]
> Kontrollera anslutningen kräver ett tillägg för virtuell dator `AzureNetworkWatcherExtension`. Installera hello tillägg på en Windows VM finns [tillägg för virtuell dator i Azure Network Watcher Agent för Windows](../virtual-machines/windows/extensions-nwa.md) och för Linux VM besöka [tillägg för virtuell dator i Azure Network Watcher Agent för Linux](../virtual-machines/linux/extensions-nwa.md).

## <a name="response"></a>Svar

hello följande tabell visar hello egenskaperna som returneras när en anslutning kontroll har slutförts.

|Egenskap  |Beskrivning  |
|---------|---------|
|ConnectionStatus     | hello status för hello anslutning kontroll. Resultat är **nåbar** och **inte åtkomlig**.        |
|AvgLatencyInMs     | Genomsnittlig svarstid under hello anslutningen kontrollen i millisekunder. (Endast visas om statusen kan nås)        |
|MinLatencyInMs     | Lägsta svarstid under hello anslutningen Kontrollera i millisekunder. (Endast visas om statusen kan nås)        |
|MaxLatencyInMs     | Högsta fördröjning under hello anslutningen Kontrollera i millisekunder. (Endast visas om statusen kan nås)        |
|ProbesSent     | Antal avsökningar skickas under hello-kontrollen. Värdet för maximalt antal är 100.        |
|ProbesFailed     | Antal avsökningar som misslyckades under hello-kontrollen. Värdet för maximalt antal är 100.        |
|Hopp     | Hopp hopp sökväg från källan toodestination.        |
|Hopp []. Typ     | Typ av resurs. Möjliga värden är **källa**, **VirtualAppliance**, **VnetLocal**, och **Internet**.        |
|Hopp []. ID | Unik identifierare för hello hopp.|
|Hopp []. Adress | IP-adress hello hopp.|
|Hopp []. Resurs-ID | ResourceID hello hopp om hello hopp är en Azure-resurs. Om det är en internet-resurs ResourceID är **Internet**. |
|Hopp []. NextHopIds | hello Unik identifierare för hello nästa hopp som vidtagits.|
|Hopp []. Problem | En samling problem som uppstod under hello kontrollen på hoppet. Om det inte fanns några problem, är hello-värdet tomt.|
|Hopp []. Utfärdar []. Ursprung | Vid hello aktuella hopp, och där problemet uppstod. Möjliga värden:<br/> **Inkommande** -problemet finns på hello länk från hello tidigare hopp toohello aktuella hopp<br/>**Utgående** -problemet finns på hello länk från hello aktuella hopp toohello nästa hopp<br/>**Lokala** -problemet finns på hello aktuella hopp.|
|Hopp []. Utfärdar []. Allvarlighetsgrad | hello allvarlighetsgraden hello problemet upptäcktes. Möjliga värden är **fel** och **varning**. |
|Hopp []. Utfärdar []. Typ |hello typ av problem som hittas. Möjliga värden: <br/>**CPU**<br/>**Minne**<br/>**GuestFirewall**<br/>**DnsResolution**<br/>**NetworkSecurityRule**<br/>**UserDefinedRoute** |
|Hopp []. Utfärdar []. Kontexten |Information om hello problemet hittades.|
|Hopp []. Utfärdar []. Kontexten [] .key |Nyckeln för hello nyckel/värde-par returneras.|
|Hopp []. Utfärdar []. Kontexten [] .value |Värdet för hello nyckel/värde-par returneras.|

hello följande är ett exempel på ett problem som finns på ett hopp.

```json
"Issues": [
    {
        "Origin": "Outbound",
        "Severity": "Error",
        "Type": "NetworkSecurityRule",
        "Context": [
            {
                "key": "RuleName",
                "value": "UserRule_Port80"
            }
        ]
    }
]
```
## <a name="fault-types"></a>Fel-typer

hello anslutningen Kontrollera returnerar fel typer om hello-anslutningen. hello innehåller följande tabell en lista över hello aktuella fault-typer som returneras.

|Typ  |Beskrivning  |
|---------|---------|
|Processor     | Hög processoranvändning.       |
|Minne     | Hög minnesanvändning.       |
|GuestFirewall     | Trafik blockeras på grund av tooa brandväggskonfigurationen för virtuell dator.        |
|DNSResolution     | DNS-matchningen misslyckades för hello måladressen.        |
|NetworkSecurityRule    | Trafik blockeras av en regel för NSG (regeln returneras)        |
|UserDefinedRoute|Trafik ignoreras på grund av tooa användardefinierade eller systemväg. |

### <a name="next-steps"></a>Nästa steg

Lär dig hur tooverify anslutning tooa resursen genom att besöka: [Kontrollera anslutningen till Azure Nätverksbevakaren](network-watcher-connectivity-powershell.md).

<!--Image references-->
[1]: ./media/network-watcher-next-hop-overview/figure1.png


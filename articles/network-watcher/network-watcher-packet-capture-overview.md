---
title: "aaaIntroduction tooPacket avbildning i Azure-Nätverksbevakaren | Microsoft Docs"
description: "Den här sidan innehåller en översikt över hello Nätverksbevakaren paket avbilda kapaciteten"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 3a81afaa-ecd9-4004-b68e-69ab56913356
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 2ce01b391b9c1a1c19aa29c8620628c55586df03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toovariable-packet-capture-in-azure-network-watcher"></a>Introduktion toovariable paketinsamling i Azure Nätverksbevakaren

Nätverket Watcher variabeln paketinsamling kan toocreate paket avbilda sessioner tootrack trafik tooand från en virtuell dator. Paketinsamling hjälper toodiagnose nätverk avvikelser båda reaktivt och proactivity. Andra användningsområden omfattar att samla in nätverksstatistik får information om nätverket intrång, toodebug klient / server-kommunikation och mycket mer.

Paketinsamling är ett tillägg för virtuell dator som startas från en fjärrdator via Nätverksbevakaren. Den här funktionen underlättar hello belastningen för körs en paketinsamling manuellt på hello önskade virtuella datorn, vilket sparar värdefull tid. Paketinsamling kan aktiveras via hello portal, PowerShell, CLI eller REST API. Ett exempel på hur paketinsamling kan utlösas är med virtuella aviseringar. Filter som är angivna för hello avbilda session tooensure du fånga in trafik som du vill toomonitor. Filter baseras på 5-tuppel (protokoll, lokal IP-adress, fjärranslutna IP-adress, lokal port och Fjärrport) information. hello infångade data lagras i hello lokal disk eller en lagringsblob. Det finns en begränsning på 10 paket avbilda sessioner per region per prenumeration. Den här gränsen gäller endast toohello sessioner och gäller inte toohello spara paketet avbilda filer lokalt på hello VM eller i ett lagringskonto.

> [!IMPORTANT]
> Paketinsamling kräver ett tillägg för virtuell dator `AzureNetworkWatcherExtension`. Installera hello tillägg på en Windows VM finns [tillägg för virtuell dator i Azure Network Watcher Agent för Windows](../virtual-machines/windows/extensions-nwa.md) och för Linux VM besöka [tillägg för virtuell dator i Azure Network Watcher Agent för Linux](../virtual-machines/linux/extensions-nwa.md).

tooreduce hello information du avbildar tooonly hello information som du vill, hello följande alternativ är tillgängliga för en paket-avbildningssessionen:

**Samla in konfiguration**

|Egenskap|Beskrivning|
|---|---|
|**Maximalt antal byte per paket (byte)** | Hej antalet byte från varje paket som har hämtats avbildas alla byte om värdet är tomt. Hej antalet byte från varje paket som har hämtats avbildas alla byte om värdet är tomt. Om du behöver endast hello IPv4-huvud – ange 34 här |
|**Maximalt antal byte per session (byte)** | Sammanlagt antal byte som fångas när hello-värdet har nåtts hello konsolsessionen avslutas.|
|**Tidsgräns (sekunder)** | Anger tidsbegränsning för hello paket avbilda session. hello standardvärdet är 18000 sekunder eller 5 timmar.|

**Filtrering (valfritt)**

|Egenskap|Beskrivning|
|---|---|
|**Protokoll** | Avbilda hello protokollet toofilter för hello-paket. hello tillgängliga värden är TCP, UDP och alla.|
|**Lokal IP-adress** | Det här värdet filtrerar hello paket avbilda toopackets där hello lokala IP-adressen matchar den här filtervärdet.|
|**Lokal port** | Det här värdet filter hello paketet avbilda toopackets där hello lokal port matchar filtret värdet.|
|**Fjärranslutna IP-adress** | Det här värdet filter hello paketet avbilda toopackets där hello fjärr-IP-matchar filtret värdet.|
|**Fjärrport** | Det här värdet filter hello paketet avbilda toopackets där hello Fjärrport matchar filtret värdet.|

### <a name="next-steps"></a>Nästa steg

Lär dig hur du kan hantera paket insamlingar hello-portalen genom att besöka [hantera paketinsamling i hello Azure-portalen](network-watcher-packet-capture-manage-portal.md) eller med PowerShell genom att besöka [hantera paket avbilda med PowerShell](network-watcher-packet-capture-manage-powershell.md).

Läs hur toocreate proaktiv paket samlar in baserat på virtuella aviseringar genom att besöka [skapar en avisering utlösta paketinsamling](network-watcher-alert-triggered-packet-capture.md)

<!--Image references-->
[1]: ./media/network-watcher-packet-capture-overview/figure1.png














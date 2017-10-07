---
title: "aaaPacket kontroll och Azure Nätverksbevakaren | Microsoft Docs"
description: "Den här artikeln beskriver hur toouse Nätverksbevakaren tooperform djup paketinspektion samlas in från en virtuell dator"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 7b907d00-9c35-40f5-a61e-beb7b782276f
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 4aeddcd482edc4df3d63e87b5c4b0788c540850b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="packet-inspection-with-azure-network-watcher"></a>Paketinspektion med Azure Nätverksbevakaren

Funktionen hello paket avbildning av Nätverksbevakaren kan du starta och hantera insamlingar sessionerna på din virtuella Azure-datorer från hello-portalen, PowerShell, CLI, och genom programmering via hello SDK och REST-API. Paketinsamling kan tooaddress scenarier som kräver nivån paketdata genom att ange hello information i ett format som enkelt kan användas. Utnyttja gratisverktyg tooinspect hello data du undersöka tooand för kommunikation från dina virtuella datorer och få insyn i nätverkstrafiken. Vissa exempel användningsområden för avbildning paketdata är: undersöker problem med nätverket eller ett program, identifiera nätverket missbruk och intrångsidentifiering försök eller upprätthålla regelefterlevnad. I den här artikeln visar vi hur tooopen filen ett paket som tillhandahålls av Nätverksbevakaren verktyget populära öppen källkod. Vi ger även exempel som visar hur toocalculate en fördröjning för anslutning, identifiera onormalt trafik och undersöka nätverk statistik.

## <a name="before-you-begin"></a>Innan du börjar

Den här artikeln går igenom några förkonfigurerade scenarier på paketinsamling som kördes tidigare. Dessa scenarier visar funktioner som kan nås genom att granska en paketinsamling. Det här scenariot använder [WireShark](https://www.wireshark.org/) tooinspect hello paketinsamling.

Det här scenariot förutsätter att du körde en paketinsamling på en virtuell dator. toolearn hur toocreate en paketinsamling finns [hantera paket som samlar in med hello portal](network-watcher-packet-capture-manage-portal.md) eller med övriga genom att besöka [hantera paket som avbildar med REST API](network-watcher-packet-capture-manage-rest.md).

## <a name="scenario"></a>Scenario

I det här scenariot kan du:

* Granska en paketinsamling

## <a name="calculate-network-latency"></a>Beräkna nätverks-svarstid

I det här scenariot visar vi hur tooview hello inledande förfluten tid (RTT) i en konversation för Transmission Control Protocol (TCP) som sker mellan två slutpunkter.

När en TCP-anslutning har upprättats Följ hello först tre paket som skickas i hello-anslutning en mönstret kallas tooas hello trevägs handskakning. Genom att undersöka först två hälsningspaket skickas i den här handskakningen, första begäran hello-klient och ett svar från hello server beräkna vi hello fördröjning när den här anslutningen har upprättats. Den här fördröjningen är refererad tooas hello förfluten tid (RTT). Mer information om hello TCP-protokollet och hello trevägs handskakning finns toohello efter resurs. https://support.microsoft.com/en-US/Help/172983/EXPLANATION-of-the-Three-Way-Handshake-via-TCP-IP

### <a name="step-1"></a>Steg 1

Starta WireShark

### <a name="step-2"></a>Steg 2

Läs in hello **CAP** fil från din paketinsamling. Den här filen finns i hello blob som den har sparats i vårt lokalt på hello virtuell dator, beroende på hur du har konfigurerat.

### <a name="step-3"></a>Steg 3

tooview Hej inledande förfluten tid (RTT) i TCP konversationer, vi kommer bara att titta på två första hälsningspaket ingår i hello TCP-handskakningen. Vi kommer att använda två första hälsningspaket i hello trevägs handskakning som är hello [SYN], [SYN, ACK] paket. De har namngetts för flaggor i hello TCP-huvudet. senaste hello-paket i hello-handskakningen hello [ACK] paketet används inte i det här scenariot. Hej [SYN] paket skickas av hello-klienten. När den tas emot skickar hello server hello [ACK] paket som en bekräftelse för att ta emot hello SYN hello-klient. Utnyttja hello faktum att hello serversvar kräver mycket lite belastning beräkna vi hello RTT genom att subtrahera hello tid hello [SYN, ACK] paketet togs emot av klienten hello av hello tid [SYN] paket har skickats av hello-klienten.

Med WireShark beräknas detta värde för oss.

toomore Se enkelt två första hälsningspaket i hello TCP trevägs handskakning, vi ska använda hello filtrering kapaciteten som tillhandahålls av WireShark.

tooapply hello filter i WireShark, expandera hello ”Transmission Control Protocol” segmentet i ett paket [SYN] i din avbildning och undersöka hello flaggor i hello TCP-huvudet.

Eftersom vi söker toofilter på alla [SYN] och [SYN ACK] paket under flaggor cofirm att hello Syn-biten är aktiverad too1 sedan högerklickar du på hello Syn bitar -> Använd som Filter -> valda.

![Bild 7][7]

### <a name="step-4"></a>Steg 4

Nu när du har filtrerat fönstret tooonly Se hälsningspaket med hello [SYN] set-bitars kan du enkelt välja konversationer som du är intresserad av tooview hello inledande RTT. Ett enkelt sätt tooview hello RTT i WireShark Klicka bara på hello dropdown markerad ”SEQ-ACK” analys. Sedan visas hello RTT visas. I det här fallet kunde hello RTT 0.0022114 sekunder eller 2.211 ms.

![bild 8][8]

## <a name="unwanted-protocols"></a>Oönskade protokoll

Du kan ha många program som körs på en virtuell dator-instans som du har distribuerat i Azure. Många av dessa program kommunicerar via hello-nätverket kanske utan uttryckligt tillstånd. Undersök med hjälp av paket avbilda toostore nätverkskommunikation, vi kan hur programmet pratar hello nätverket och leta efter eventuella problem.

I det här exemplet vi går igenom ett tidigare körde paketinsamling för oönskade protokoll som kan indikera obehörig kommunikation från ett program som körs på datorn.

### <a name="step-1"></a>Steg 1

Med hjälp av hello samma avbildning i hello föregående scenario klickar du på **statistik** > **protokollet hierarki**

![protokollet hierarki-menyn][2]

hello visas protokollet hierarki. Den här vyn visar en lista över alla hello-protokoll som har använts under hello avbildningssessionen och hello antalet paket som skickas och tas emot med hello-protokoll. Den här vyn kan vara användbart för att hitta oönskad trafik på virtuella datorer eller i nätverket.

![protokollet hierachy öppnas][3]

Som du ser i följande skärmbild hello uppstod trafik med hjälp av hello BitTorrent protokoll som används för peer-toopeer fildelning. Som administratör räknar du inte toosee BitTorrent trafik på denna specifika virtuella datorer. Nu du känner till den här trafiken, du kan ta bort hello peer toopeer programvara som installeras på den här virtuella datorn eller blockera hello trafik med hjälp av Nätverkssäkerhetsgrupper eller en brandvägg. Du kan dessutom välja toorun paket insamlingar enligt ett schema så att du kan granska hello-protokollet används på de virtuella datorerna regelbundet. Ett exempel på hur tooautomate nätverk uppgifter i azure besök [övervaka nätverksresurser med azure automation](network-watcher-monitor-with-azure-automation.md)

## <a name="finding-top-destinations-and-ports"></a>Söka efter vanliga mål och portar

Förstå hello typer av trafik, hello slutpunkter och hello-portar som kommunicerades via är en viktig vid övervakning eller felsöka program och resurser i nätverket. Använda filen ett paket från ovan kan vi snabbt lära hello vanliga mål våra VM kommunicerar med och hello portar som används.

### <a name="step-1"></a>Steg 1

Med hjälp av hello samma avbildning i hello föregående scenario klickar du på **statistik** > **IPv4-statistik** > **mål och portar**

![paketet insamlings-fönstret][4]

### <a name="step-2"></a>Steg 2

Vi ser hello resultat via en rad framhävs, det fanns flera anslutningar på port 111. var hello används oftast port 3389, vilket är Fjärrskrivbord och hello återstående är dynamiska RPC-portar.

När den här trafiken kan innebära att ingenting, är en port som användes för många anslutningar och är okänd toohello administratör.

![Bild 5][5]

### <a name="step-3"></a>Steg 3

Nu när vi har fastställt att en out-of plats port filtrera vi våra avbildning baserat på hello port.

hello filter i det här scenariot är:

```
tcp.port == 111
```

Vi ange hello filtertext ovan hello filter-textrutan med och träffar.

![Bild 6][6]

Hello resultaten kan vi se alla hello trafiken kommer från en lokal virtuell dator på hello samma undernät. Om vi inte fortfarande förstår varför den här trafiken uppstår, kan vi ytterligare inspektera hello paket toodetermine varför den anropar dessa på port 111. Med den här informationen kan vi ta hello lämplig åtgärd.

## <a name="next-steps"></a>Nästa steg

Lär dig mer om hello andra diagnostiska funktioner i Nätverksbevakaren genom att besöka [Azure översikt för nätverksövervakning](network-watcher-monitoring-overview.md)

[1]: ./media/network-watcher-deep-packet-inspection/figure1.png
[2]: ./media/network-watcher-deep-packet-inspection/figure2.png
[3]: ./media/network-watcher-deep-packet-inspection/figure3.png
[4]: ./media/network-watcher-deep-packet-inspection/figure4.png
[5]: ./media/network-watcher-deep-packet-inspection/figure5.png
[6]: ./media/network-watcher-deep-packet-inspection/figure6.png
[7]: ./media/network-watcher-deep-packet-inspection/figure7.png
[8]: ./media/network-watcher-deep-packet-inspection/figure8.png














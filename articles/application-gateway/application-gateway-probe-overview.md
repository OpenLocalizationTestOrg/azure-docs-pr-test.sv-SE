---
title: "Översikt över övervakning för Azure Programgateway aaaHealth | Microsoft Docs"
description: "Lär dig mer om hello övervakningsfunktionerna i Azure Programgateway"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7eeba328-bb2d-4d3e-bdac-7552e7900b7f
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/14/2016
ms.author: gwallace
ms.openlocfilehash: 5091d80394a354ff849ce7ccee8cc9d2fd0456db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-gateway-health-monitoring-overview"></a>Gateway hälsa övervakning Programöversikt

Azure Application Gateway som standard övervakar hello hälsotillståndet för alla resurser i dess backend-pool och tar automatiskt bort alla resurser som bedöms som dåligt hello poolen. Programgateway fortsätter toomonitor hello ohälsosamt instanser och lägger till dem tillbaka toohello felfri backend-poolen när de blir tillgängliga och svara toohealth-avsökningar. Programgateway skickar hello hälsoavsökningar med hello samma port som har definierats i hello backend-HTTP-inställningar. Den här konfigurationen garanterar att hello avsökningen provar hello samma port som kunder kan använda tooconnect toohello backend.

![exempel på gateway avsökning][1]

I tillägg toousing standard avsökningen hälsoövervakning, kan du också anpassa hello hälsa avsökningen toosuit programmets krav. I den här artikeln omfattar både standard- och anpassade hälsoavsökningar.

> [!NOTE]
> Om det finns en NSG på Application Gateway-undernät, bör portintervall 65503 65534 öppnas i hello Programgateway undernät för inkommande trafik. Dessa portar är obligatoriska för hello backend hälsa API toowork.

## <a name="default-health-probe"></a>Standard hälsoavsökningen

En Programgateway konfigureras automatiskt en standard hälsoavsökningen när du inte konfigurera eventuella anpassade avsökningen konfiguration. hello övervakning beteende fungerar genom att göra en HTTP-begäran toohello IP-adresser konfigureras för hello backend-adresspool. Om hello serverdelens http-inställningar som är konfigurerade för HTTPS, för avsökningar som standard använder HTTPS som korrekt tootest hälsotillstånd hello serverdelar hello avsökning.

Exempel: du konfigurerar programmet gateway toouse backend-servrar A, B och C tooreceive HTTP nätverkstrafiken på port 80. hello standard hälsoövervakning testar hello tre servrar med 30 sekunders mellanrum felfri HTTP-svar. En felfri HTTP-svar har en [statuskod](https://msdn.microsoft.com/library/aa287675.aspx) mellan 200 och 399.

Om hello standard avsökningen kontrollen misslyckas Server A hello Programgateway tas den bort från dess backend-adresspool och nätverkstrafik slutar flöda toothis server. hello standard avsökningen kvarstår toocheck för servern en var 30: e sekund. När servern A svarar har tooone begäran från en standard hälsoavsökningen, läggs den tillbaka som felfri toohello backend-poolen och trafiken börjar flöda toohello server igen.

### <a name="default-health-probe-settings"></a>Standardinställningarna hälsa avsökning

| Avsökningen egenskapen | Värde | Beskrivning |
| --- | --- | --- |
| URL för webbavsökning |http://127.0.0.1:\<port\>/ |URL-sökväg |
| intervall |30 |Avsökningsintervall i sekunder |
| Timeout |30 |Avsökningen tidsgräns i sekunder |
| Tröskelvärde för ohälsosamt värde |3 |Avsökning för antal nya försök. hello backend-server markeras när hello efterföljande avsökningen Felberäkning når hello tröskelvärde för ohälsosamt värde. |

> [!NOTE]
> hello port är hello samma port som hello backend-HTTP-inställningar.

hello standard avsökning kontrollerar endast http://127.0.0.1:\<port\> toodetermine hälsostatus. Om du behöver tooconfigure hello hälsa toogo tooa anpassad URL för webbavsökning eller ändra andra inställningar måste du använda anpassade avsökningar enligt beskrivningen i hello följande steg:

## <a name="custom-health-probe"></a>Anpassade hälsoavsökningen

Anpassade avsökningar Tillåt toohave en mer detaljerad kontroll över hello hälsoövervakning. När du använder anpassade avsökningar, kan du konfigurera hello avsökningsintervall hello URL och sökvägen tootest och hur många misslyckade svar tooaccept innan du markerar hello backend-pool-instans som ohälsosamt.

### <a name="custom-health-probe-settings"></a>Inställningar för anpassade hälsotillstånd avsökning

hello innehåller följande tabell definitioner för hello egenskaperna för en anpassad hälsoavsökningen.

| Avsökningen egenskapen | Beskrivning |
| --- | --- |
| Namn |Namnet på hello avsökning. Det här namnet är används toorefer toohello avsökning i backend-HTTP-inställningar. |
| Protokoll |Protokoll som används toosend hello avsökning. hello avsökningen använder hello-protokollet som definieras i hello backend-HTTP-inställningar |
| Värd |Värden namnet toosend hello avsökning. Gäller endast när flera platser har konfigurerats på Application Gateway, Använd annars ”127.0.0.1”. Det här värdet skiljer sig från den virtuella datorns värdnamn. |
| Sökväg |Relativ sökväg hello avsökning. hello giltig sökväg börjar från '/'. |
| intervall |Avsökning intervall i sekunder. Det här värdet är hello tidsintervallet mellan två på varandra följande avsökningar. |
| Timeout |Avsökning tidsgräns i sekunder. Om ett giltigt svar inte emot inom denna tidsgräns, markeras hello avsökningen som misslyckad.  |
| Tröskelvärde för ohälsosamt värde |Avsökning för antal nya försök. hello backend-server markeras när hello efterföljande avsökningen Felberäkning når hello tröskelvärde för ohälsosamt värde. |

> [!IMPORTANT]
> Om Application Gateway har konfigurerats för en viss plats, som standard hello värden ska namn anges som 127.0.0.1, såvida inte annat konfigurerade i anpassade avsökning.
> För referens skickas en anpassad avsökningsåtgärd för\<protokollet\>://\<värden\>:\<port\>\<sökvägen\>. hello port används blir hello samma port som definierats i hello backend-HTTP-inställningar.

## <a name="next-steps"></a>Nästa steg
När du lära dig mer om övervakning av hälsotillstånd för Programgateway, kan du konfigurera en [anpassade hälsoavsökningen](application-gateway-create-probe-portal.md) i hello Azure-portalen eller en [anpassade hälsoavsökningen](application-gateway-create-probe-ps.md) med PowerShell och hello Azure Resource Manager distributionsmodell.

[1]: ./media/application-gateway-probe-overview/appgatewayprobe.png

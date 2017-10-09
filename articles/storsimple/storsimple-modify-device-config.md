---
title: aaaModify hello StorSimple enhetskonfiguration | Microsoft Docs
description: Beskriver hur hello toouse StorSimple Manager service tooreconfigure en StorSimple-enhet som redan har distribuerats.
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: bb679264-8d46-429c-9ef7-630dc3b4a423
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/19/2017
ms.author: v-sharos
ms.openlocfilehash: 10a54c191260bf1baba58d28cdbfa0ed72217f48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomodify-your-storsimple-device-configuration"></a>Använd hello StorSimple Manager-tjänsten toomodify konfigurationen StorSimple-enhet
## <a name="overview"></a>Översikt
Hej klassiska Azure-portalen **konfigurera** innehåller alla hello enheten parametrar som du kan konfigurera om på en StorSimple-enhet som hanteras av en StorSimple Manager-tjänsten. Den här självstudiekursen beskrivs hur du kan använda hello **konfigurera** sidan tooperform hello följande enhetsnivå uppgifter:

* Ändra inställningar för enheter 
* Ändra inställningarna för tid 
* Ändra DNS-inställningar 
* Ändra nätverksgränssnitt
* Tilldela IP-adresser eller växla

## <a name="modify-device-settings"></a>Ändra inställningar för enheter
hello Enhetsinställningar innehåller hello eget namn för hello enheten och hello enhetsbeskrivning.

> [!NOTE] 
> Du kan inte ändra hello enhetsnamnet i hello klassiska Azure-portalen. Byta namn på hello enheten stöds inte.

En StorSimple-enhet som är anslutna toohello StorSimple Manager-tjänsten har tilldelats ett standardnamn. hello standardnamnet visar normalt hello serienumret för hello enhet. En enhet för standardnamnet är 15 tecken långt, till exempel 8600-SHX0991003G44HT anger till exempel hello följande:

* **8600** – anger hello enhetsmodell.
* **SHX** – anger hello anläggning.
* **0991003** -indikerar en viss produkt.
* **G44HT**- hello 5 sista siffrorna är ökas toocreate unika serienummer. Det kan inte vara en sekventiell uppsättning.

Du kan ange en enhetsbeskrivning. En enhetsbeskrivning identifierar vanligtvis hello ägare och hello fysiska platsen för hello enhet. beskrivningsfält hello måste innehålla färre än 256 tecken.

## <a name="modify-time-settings"></a>Ändra inställningarna för tid
Din enhet måste synkronisera tiden i ordning tooauthenticate med din molntjänstleverantör för lagring. Välj din tidszon hello listrutan och ange in tootwo Network Time Protocol (NTP) servrar. hello primär NTP-server krävs och anges när du använder Windows PowerShell för StorSimple tooconfigure din enhet. Du kan ange hello standard Windows Server **time.windows.com** som NTP-server. Du kan visa hello primär NTP konfiguration via hello klassiska Azure-portalen, men du måste använda hello Windows PowerShell-gränssnittet toochange den.

hello sekundära NTP-serverkonfigurationen är valfritt. Du kan använda hello klassiska portal tooconfigure en sekundär NTP-server. 

När du konfigurerar hello NTP-server måste du kontrollera att ditt nätverk tillåter hello NTP-trafik toopass från ditt datacenter toohello Internet. När du anger en offentlig NTP-server, måste du se till att nätverkets brandväggar och andra säkerhetsenheter är konfigurerade tooallow NTP-trafik tootravel tooand från hello utanför nätverket. Om dubbelriktad NTP-trafik inte tillåts, måste du använda en intern NTP-server (en Windows-domänkontrollant ger den här funktionen). Om enheten inte kan synkronisera tiden, kanske inte kan toocommunicate med leverantören av lagring molnet.

toosee en lista över allmänna NTP-servrar, gå toohello [NTP-servrar Web](http://support.ntp.org/bin/view/Servers/WebHome). 

### <a name="what-happens-if-hello-device-is-deployed-in-a-different-time-zone"></a>Vad händer om hello enheten distribueras i en annan tidszon?
Om hello enheten har distribuerats i en annan tidszon, ändras tidszon för hello enhet. Med hänsyn till att alla principer för säkerhetskopiering av hello använder hello enheten tidszon, justeras automatiskt hello principer för säkerhetskopiering i enlighet med hello nya tidszonen. Inga användaråtgärder krävs.

## <a name="modify-dns-settings"></a>Ändra DNS-inställningar
En DNS-server används när enheten försöker toocommunicate med din molntjänstleverantör för lagring. För hög tillgänglighet och är obligatoriska tooconfigure båda primära hello och hello sekundära DNS-servrar under hello inledande distributionen. tooreconfigure hello primär DNS-server, måste toouse hello Windows PowerShell-gränssnittet på din StorSimple-enhet.

Du kan använda hello klassiska Azure-portalen toomodify hello sekundär DNS-server.

## <a name="modify-network-interfaces"></a>Ändra nätverksgränssnitt
Enheten har sex enheten nätverksgränssnitt, varav fyra är 1 GbE och varav två 10 GbE. Gränssnitten är märkta som DATA 0 – DATA 5. DATA 0 DATA 1, DATA 4 och 5 DATA är 1 GbE DATA 2 och DATA 3 är 10 GbE-nätverkskort.

Konfigurera **gränssnitt för nätverksinställningar** för varje hello gränssnitt toobe används. tooensure hög tillgänglighet, rekommenderar vi att du har minst två iSCSI och två gränssnitt för moln-aktiverat på enheten. Vi rekommenderar men som kräver inte att oanvända gränssnitt inaktiveras.

När du konfigurerar någon av hello nätverksgränssnitt, måste du konfigurera en virtuell IP-adress (VIP).

DATA 0 är moln-aktiverat som standard. När du konfigurerar DATA 0, som också krävs tooconfigure två fasta IP-adresser, en för varje domänkontrollant. De fasta IP-adresser kan använda tooaccess hello styrenheter direkt och är användbara när du installerar uppdateringar på hello enhet eller när du öppnar hello styrenheter för hello syftet med felsökningen.

I StorSimple 8000 Series uppdatering 1 anges hello routning mått för DATA 0 toohello lägsta; Om enheten kör StorSimple 8000 Series uppdatering 1, därför dirigeras alla hello molnet trafik via DATA 0. Anteckna detta om du har mer än en moln-aktiverat nätverksgränssnittet på din StorSimple-enhet.

> [!NOTE]
> hello fasta IP-adresser för hello controller används för att underhålla hello uppdateringar toohello enhet. Hello statiska IP-adresser måste därför vara dirigerbara och kunna tooconnect toohello Internet.
> 
> 

För varje nätverksgränssnitt visas hello följande parametrar:

* **Hastighet** – inte en parameter som kan konfigureras av användaren. DATA 0 DATA 1, DATA 4 och 5 DATA är alltid 1 GbE DATA 2 och DATA 3 är 10 GbE-gränssnitt.
  
  > [!NOTE]
  > Hastighet och duplex är alltid automatiskt förhandlade. Jumboramar stöds inte.
  > 
  > 
* **Gränssnitt tillstånd** – ett gränssnitt kan aktiveras eller inaktiveras. Om aktiverad, försöker hello enheten toouse hello-gränssnittet. Vi rekommenderar att gränssnitt som är anslutna toohello nätverk och som används är aktiverat. Inaktivera alla gränssnitt som du inte använder.
* **Gränssnittstyp** – den här parametern kan tooisolate iSCSI-trafik från molnet lagringsrelaterad trafik. Den här parametern kan vara något av följande hello:
  
  * **Molnet aktiverat** – när aktiverad hello enheten kommer att använda det här gränssnittet toocommunicate med hello molnet.
  * **iSCSI-aktiverade** – när aktiverad hello enheten kommer att använda det här gränssnittet toocommunicate med hello iSCSI-värden.
    
    Vi rekommenderar att du isolerar iSCSI-trafik från molnet lagringsrelaterad trafik. Observera också att om värden ligger inom hello samma undernät som enheten behöver inte tooassign en gateway. Om värden finns i ett annat undernät än enheten, behöver tooassign en gateway.
* **IP-adress** – det kan vara IPv4 eller IPv6- eller båda. Både hello IPv4 och IPv6-adress familjer stöds för hello enheten nätverksgränssnitt. När du använder IPv4 kan ange en 32-bitars IP-adress (*xxx.xxx.xxx.xxx*) i punkt decimalform. När du använder IPv6 kan bara ange ett 4-siffrig prefix och en 128-bitars adress genereras automatiskt för din enhet nätverksgränssnittet baserat på det prefixet.
* **Undernät** – detta refererar toohello nätmask och konfigureras via hello Windows PowerShell-gränssnittet.
* **Gateway** – detta är hello standard-gateway som ska användas av det här gränssnittet när den försöker toocommunicate med noder som inte är inom hello samma IP-adressutrymme (undernät). hello standard-gateway måste vara i hello samma adressutrymmet (undernät) som hello interface IP-adress, enligt systemets hello nätmask.
* **Fasta IP-adressen** – det här fältet är bara tillgänglig när du konfigurerar hello DATA 0 gränssnitt. Du kanske måste tooconnect för åtgärder som till exempel uppdateringar eller felsökning hello-enhet direkt toohello enhetskontroll. hello fasta IP-adressen kan vara används tooaccess både hello active och hello passiva controller på enheten.

Du kan konfigurera styrenhet 0 och 1 via hello klassiska Azure-portalen.

> [!NOTE]
> * tooensure ska fungera korrekt, kontrollera hello gränssnittets hastighet och duplex på hello växeln som varje enhetsgränssnittet är ansluten till. Växeln gränssnitt länkegenskaper antingen med eller konfigureras för Gigabit Ethernet (1000 Mbps) och vara full duplex. Gränssnitt med långsammare hastigheter eller läget halv duplex leder prestandaproblem.
> * toominimize avbrott och driftstopp, rekommenderar vi att du aktiverar stöd för portfast på varje hello switch-portar som hello iSCSI-gränssnitt för enheten ska kunna ansluta till. Se till att nätverksanslutningen kan upprättas snabbt i ett redundanskluster hello-händelse.
> 
> 

## <a name="swap-or-reassign-ips"></a>Tilldela IP-adresser eller växla
För närvarande någon nätverksgränssnittet på hello domänkontrollant är tilldelad en VIP som används (av hello samma enhet eller en annan enhet i nätverket hello), hello domänkontrollant kommer att misslyckas över. Du måste därför toofollow hello rätt proceduren om byte av VIP för hello nätverksgränssnittet för enheten, eftersom du kommer att skapa en duplicerad IP-situation.

Utför följande steg tooswap hello eller tilldela hello VIP för någon av hello nätverksgränssnitt:

#### <a name="tooreassign-ips"></a>tooreassign IP-adresser
1. Rensa hello IP-adressen för båda gränssnitten.
2. Tilldela hello nya IP-adresser toohello respektive gränssnitt hello IP-adresser är avmarkerat.

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[konfigurera MPIO för din StorSimple-enhet](storsimple-configure-mpio-windows-server.md).
* Lär dig hur för[Använd hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).


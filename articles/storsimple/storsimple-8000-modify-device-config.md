---
title: aaaModify hello StorSimple 8000-serien enhetskonfiguration | Microsoft Docs
description: Beskriver hur hello toouse StorSimple Enhetshanteraren service tooreconfigure en StorSimple-enhet som redan har distribuerats.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/19/2017
ms.author: alkohli
ms.openlocfilehash: 79711b45eafe732c1eed1e562b05e3837fbc9d03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomodify-your-storsimple-device-configuration"></a>Använd hello StorSimple Enhetshanteraren service toomodify konfigurationen StorSimple-enhet

## <a name="overview"></a>Översikt

hello Azure-portalen **Enhetsinställningar** avsnitt i hello **inställningar** bladet innehåller alla hello enheten parametrar som du kan konfigurera om på en StorSimple-enhet som hanteras av en StorSimple Device Manager tjänsten. Den här självstudiekursen beskrivs hur du kan använda hello **inställningar** bladet tooperform hello följande enhetsnivå uppgifter:

* Ändra eget namn för enhet
* Ändra tid Enhetsinställningar
* Tilldela en sekundär DNS
* Ändra nätverksgränssnitt
* Tilldela IP-adresser eller växla

## <a name="modify-device-friendly-name"></a>Ändra eget namn för enhet

Du kan använda hello Azure portal toochange hello enhetsnamnet och tilldela den ett unikt namn för ditt val. Använd hello **allmänna inställningar** bladet på din enhet toomodify hello eget namn för enhet. hello eget namn får innehålla tecken och kan innehålla högst 64 tecken.

> [!NOTE] 
> Du kan bara ändra hello enhetsnamnet i hello Azure-portalen innan hello enheten installationen är klar. När hello minimala Enhetsinstallationen är klar kan ändra du inte hello enhetsnamn.

![Enhetens namn i allmänhet inställningar](./media/storsimple-8000-modify-device-config/modify-general-settings3.png)

En StorSimple-enhet som är anslutna toohello StorSimple enheten Manager-tjänsten har tilldelats ett standardnamn. hello standardnamnet visar normalt hello serienumret för hello enhet. En enhet för standardnamnet är 15 tecken långt, till exempel 8600-SHX0991003G44HT anger till exempel hello följande:

* **8600** – anger hello enhetsmodell.
* **SHX** – anger hello anläggning.
* **0991003** -indikerar en viss produkt.
* **G44HT**- hello 5 sista siffrorna är ökas toocreate unika serienummer. Det kan inte vara en sekventiell uppsättning.

## <a name="modify-device-description"></a>Ändra enhetsbeskrivning

Använd hello **allmänna inställningar** bladet på din enhet toomodify hello enhetsbeskrivning.

![Enhetsbeskrivning i allmänna inställningar](./media/storsimple-8000-modify-device-config/modify-general-settings4.png)

En enhetsbeskrivning identifierar vanligtvis hello ägare och hello fysiska platsen för hello enhet. beskrivningsfält hello måste innehålla färre än 256 tecken.

## <a name="modify-time-settings"></a>Ändra inställningarna för tid

Din enhet måste synkronisera tiden i ordning tooauthenticate med din molntjänstleverantör för lagring. Använd hello **allmänna inställningar** bladet på enheten toomodify hello tidsinställningarna på enheten.

![Enhetsbeskrivning i allmänna inställningar](./media/storsimple-8000-modify-device-config/modify-general-settings2.png)

 Välj din tidszon hello nedrullningsbara listan. Du kan ange upp tootwo Network Time Protocol (NTP) servrar:

 - **Primär NTP-server** -hello konfiguration krävs och anges när du använder Windows PowerShell för StorSimple tooconfigure din enhet. Du kan ange hello standard Windows Server **time.windows.com** som NTP-server. Du kan visa hello primär NTP konfiguration via hello Azure-portalen, men du måste använda hello Windows PowerShell-gränssnittet toochange den. Använd hello `Set-HcsNTPClientServerAddress` cmdlet toomodify hello primär NTP-server i din enhet. Mer information finns i toosynxtax för [Set-HcsNTPClientServerAddress] (https://technet.microsoft.com/library/dn688138.aspx) cmdlet.

- **Sekundär NTP-server** -hello konfigurationen är valfria. Du kan använda hello portal tooconfigure en sekundär NTP-server.

När du konfigurerar hello NTP-server måste du kontrollera att ditt nätverk tillåter hello NTP-trafik toopass från ditt datacenter toohello Internet. När du anger en offentlig NTP-server, måste du se till att nätverkets brandväggar och andra säkerhetsenheter är konfigurerade tooallow NTP-trafik tootravel tooand från hello utanför nätverket. Om dubbelriktad NTP-trafik inte tillåts, måste du använda en intern NTP-server (en Windows-domänkontrollant ger den här funktionen). Om enheten inte kan synkronisera tiden, kanske inte kan toocommunicate med leverantören av lagring molnet.

toosee en lista över allmänna NTP-servrar, gå toohello [NTP-servrar Web](http://support.ntp.org/bin/view/Servers/WebHome).

### <a name="what-happens-if-hello-device-is-deployed-in-a-different-time-zone"></a>Vad händer om hello enheten distribueras i en annan tidszon?

Om hello enheten har distribuerats i en annan tidszon, ändras tidszon för hello enhet. Med hänsyn till att alla principer för säkerhetskopiering av hello använder hello enheten tidszon, justeras automatiskt hello principer för säkerhetskopiering i enlighet med hello nya tidszonen. Inga användaråtgärder krävs.

## <a name="modify-dns-settings"></a>Ändra DNS-inställningar

En DNS-server används när enheten försöker toocommunicate med din molntjänstleverantör för lagring. Använd hello **nätverksinställningar** bladet på din enhet tooview och ändra hello konfigureras DNS-inställningarna. 

![DNS-inställningar i nätverksinställningar](./media/storsimple-8000-modify-device-config/modify-network-settings1.png)

För hög tillgänglighet och är obligatoriska tooconfigure båda primära hello och hello sekundära DNS-servrar under hello inledande distributionen.

**Primär DNS-server** -du använder hello Windows PowerShell för StorSimple toofirst ange hello primär DNS-server under hello-installationen. Du kan konfigurera hello primära DNS-servern endast via hello Windows PowerShell-gränssnittet. Använd hello `Set-HcsDNSClientServerAddress` cmdlet toomodify hello primär DNS-server i din enhet. Mer information finns i toosynxtax [Set HcsDNSClientServerAddress](https://technet.microsoft.com/library/dn688138.aspx) cmdlet.

**Sekundär DNS-server** -toomodify hello sekundära DNS-servern, Använd hello `Set-HcsDNSClientServerAddress` cmdlet i hello Windows PowerShell-gränssnittet för hello enhet eller **nätverksinställningar** bladet för din StorSimple-enhet i hello Azure portalen.

toomodify hello sekundära DNS-servern i Azure-portalen, utföra hello följande steg.

1. Gå tooyour StorSimple enheten Manager-tjänsten. Välj hello lista över enheter, och klicka på enheten.

2. I hello **inställningar** bladet går för**Enhetsinställningar > nätverk**. Detta öppnar hello **nätverksinställningar** bladet. Klicka på **DNS-inställningarna** panelen. Ändra hello sekundär DNS-serverns IP-adress.

    ![Ändra sekundär DNS server IP adderss](./media/storsimple-8000-modify-device-config/modify-secondary-dns1.png)

4. Hello kommandofältet klickar du på **spara** och när du uppmanas att bekräfta på **OK**.

    ![Spara och bekräfta ändringar](./media/storsimple-8000-modify-device-config/modify-secondary-dns-2.png)



## <a name="modify-network-interfaces"></a>Ändra nätverksgränssnitt

Enheten har sex enheten nätverksgränssnitt, varav fyra är 1 GbE och varav två 10 GbE. Gränssnitten är märkta som DATA 0 – DATA 5. DATA 0 DATA 1, DATA 4 och 5 DATA är 1 GbE DATA 2 och DATA 3 är 10 GbE-nätverkskort.

Använd hello **nätverksinställningar** bladet tooconfigure varje av hello gränssnitt toobe används.

![Konfigurera nätverksgränssnitt via nätverksinställningar](./media/storsimple-8000-modify-device-config/modify-network-settings3.png)

tooensure hög tillgänglighet, rekommenderar vi att du har minst två iSCSI och två gränssnitt för moln-aktiverat på enheten. Vi rekommenderar men som kräver inte att oanvända gränssnitt inaktiveras.

För varje nätverksgränssnitt visas hello följande parametrar:

* **Hastighet** – inte en parameter som kan konfigureras av användaren. DATA 0 DATA 1, DATA 4 och 5 DATA är alltid 1 GbE DATA 2 och DATA 3 är 10 GbE-gränssnitt.
  
  > [!NOTE]
  > Hastighet och duplex är alltid automatiskt förhandlade. Jumboramar stöds inte.
  
* **Gränssnitt tillstånd** – ett gränssnitt kan aktiveras eller inaktiveras. Om aktiverad, försöker hello enheten toouse hello-gränssnittet. Vi rekommenderar att gränssnitt som är anslutna toohello nätverk och som används är aktiverat. Inaktivera alla gränssnitt som du inte använder.
* **Gränssnittstyp** – den här parametern kan tooisolate iSCSI-trafik från molnet lagringsrelaterad trafik. Den här parametern kan vara något av följande hello:
  
  * **Molnet aktiverat** – när aktiverad hello enheten kommer att använda det här gränssnittet toocommunicate med hello molnet.
  * **iSCSI-aktiverade** – när aktiverad hello enheten kommer att använda det här gränssnittet toocommunicate med hello iSCSI-värden.
    
    Vi rekommenderar att du isolerar iSCSI-trafik från molnet lagringsrelaterad trafik. Observera också att om värden ligger inom hello samma undernät som enheten behöver inte tooassign en gateway. Om värden finns i ett annat undernät än enheten, behöver tooassign en gateway.
* **IP-adress** – när du konfigurerar någon hello nätverksgränssnitt, måste du konfigurera en virtuell IP-adress (VIP). Detta kan vara IPv4 eller IPv6- eller båda. Både hello IPv4 och IPv6-adress familjer stöds för hello enheten nätverksgränssnitt. När du använder IPv4 kan ange en 32-bitars IP-adress (*xxx.xxx.xxx.xxx*) i punkt decimalform. När du använder IPv6 kan bara ange ett 4-siffrig prefix och en 128-bitars adress genereras automatiskt för din enhet nätverksgränssnittet baserat på det prefixet.
* **Undernät** – detta refererar toohello nätmask och konfigureras via hello Windows PowerShell-gränssnittet.
* **Gateway** – detta är hello standard-gateway som ska användas av det här gränssnittet när den försöker toocommunicate med noder som inte är inom hello samma IP-adressutrymme (undernät). hello standard-gateway måste vara i hello samma adressutrymmet (undernät) som hello interface IP-adress, enligt systemets hello nätmask.
* **Fasta IP-adressen** – det här fältet är bara tillgänglig när du konfigurerar hello DATA 0 gränssnitt. Du kanske måste tooconnect för åtgärder som till exempel uppdateringar eller felsökning hello-enhet direkt toohello enhetskontroll. hello fasta IP-adressen kan vara används tooaccess både hello active och hello passiva controller på enheten.

> [!NOTE]
> * tooensure ska fungera korrekt, kontrollera hello gränssnittets hastighet och duplex på hello växeln som varje enhetsgränssnittet är ansluten till. Växeln gränssnitt länkegenskaper antingen med eller konfigureras för Gigabit Ethernet (1000 Mbps) och vara full duplex. Gränssnitt med långsammare hastigheter eller läget halv duplex leder prestandaproblem.
> * toominimize avbrott och driftstopp, rekommenderar vi att du aktiverar stöd för portfast på varje hello switch-portar som hello iSCSI-gränssnitt för enheten ska kunna ansluta till. Se till att nätverksanslutningen kan upprättas snabbt i ett redundanskluster hello-händelse.

### <a name="configure-data-0"></a>Konfigurera DATA 0

DATA 0 är moln-aktiverat som standard. När du konfigurerar DATA 0, som också krävs tooconfigure två fasta IP-adresser, en för varje domänkontrollant. De fasta IP-adresser kan använda tooaccess hello styrenheter direkt och är användbara när du installerar uppdateringar på hello enhet eller när du öppnar hello styrenheter för hello syftet med felsökningen.

Du kan konfigurera hello fasta IP-domänkontrollanter via hello DATA 0 inställningsbladet.

![Konfigurera nätverksgränssnittet - DATA 0](./media/storsimple-8000-modify-device-config/modify-network-settings2.png)

> [!NOTE]
> hello fasta IP-adresser för hello controller används för att underhålla hello uppdateringar toohello enhet. Hello statiska IP-adresser måste därför vara dirigerbara och kunna tooconnect toohello Internet.

### <a name="configure-data-1---data-5"></a>Konfigurera DATA 1 - DATA 5

Du kan konfigurera alla hello nätverksinställningar som visas i följande skärmbild hello för DATA 1 - DATA 5 nätverksgränssnitt:

![Konfigurera nätverksgränssnitt DATA 1 - 5 DATA](./media/storsimple-8000-modify-device-config/modify-network-settings4.png)


## <a name="swap-or-reassign-ips"></a>Tilldela IP-adresser eller växla

För närvarande någon nätverksgränssnittet på hello domänkontrollant är tilldelad en VIP som används (av hello samma enhet eller en annan enhet i nätverket hello), hello domänkontrollant kommer att misslyckas över. Om du byter eller omtilldela VIP för ett nätverksgränssnitt på enheten måste du följa en rätt procedur som du kan skapa en duplicerad IP-situation.

Utför följande steg tooswap hello eller tilldela hello VIP för någon av hello nätverksgränssnitt:

#### <a name="tooreassign-ips"></a>tooreassign IP-adresser

1. Rensa hello IP-adressen för båda gränssnitten.
2. Tilldela hello nya IP-adresser toohello respektive gränssnitt hello IP-adresser är avmarkerat.

## <a name="next-steps"></a>Nästa steg

* Lär dig hur för[konfigurera MPIO för din StorSimple-enhet](storsimple-8000-configure-mpio-windows-server.md).
* Lär dig hur för[Använd hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).


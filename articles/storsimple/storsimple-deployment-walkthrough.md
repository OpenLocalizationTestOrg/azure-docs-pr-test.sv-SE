---
title: aaaDeploy en lokal StorSimple-enhet | Microsoft Docs
description: "Beskriver hello stegen och bästa praxis för att distribuera hello StorSimple-enheten och tjänsten. (Gäller tooMicrosoft Azure StorSimple version 3 och tidigare.)"
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: b27f87a2-1363-4e0d-90f7-37b5dd1f21c9
ms.service: storsimple
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: d45abba1786ceae586a99ca77b90de3f290c2f0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-on-premises-storsimple-device"></a>Distribuera din lokala StorSimple-enhet
> [!div class="op_single_selector"]
> * [Uppdatering 2](storsimple-deployment-walkthrough-u2.md)
> * [Uppdatering 1](storsimple-deployment-walkthrough-u1.md)
> * [GA-version](storsimple-deployment-walkthrough.md)
> 
> 

## <a name="overview"></a>Översikt
Välkommen till distribution av tooMicrosoft Azure StorSimple-enheten. De här kurserna om distribution gäller tooStorSimple 8000-serien versionen, StorSimple 8000 uppdatering 0.1, StorSimple 8000 uppdatering 0,2 och StorSimple 8000 uppdatering 0.3. Denna serie självstudier beskriver hur tooconfigure din StorSimple-enhet och innehåller en checklista för konfiguration, konfigurationskrav och detaljerade konfigurationssteg steg.

hello informationen i de här kurserna förutsätter att du har granskat hello säkerhetsåtgärder, packat upp, rackmonterat och kabelanslutit din StorSimple-enhet. Om du fortfarande behöver tooperform de uppgifter du börja med att granska hello [säkerhetsåtgärderna](storsimple-safety.md). Beroende på din enhetsmodell kan du sedan packa upp, rackmontera och kabelansluta genom att följa instruktionerna hello i:

* [Packa upp, rackmontera och kabelanslut din 8100](storsimple-8100-hardware-installation.md)
* [Packa upp, rackmontera och kabelanslut din 8600](storsimple-8600-hardware-installation.md)

Du måste administratören privilegier toocomplete hello installationen och konfigurationen processen. Vi rekommenderar att du granskar hello checklista för konfiguration innan du börjar. hello kan distribution och konfiguration ta viss tid toocomplete.

> [!NOTE]
> distributionsinformation för hello StorSimple publiceras på Microsoft Azure-webbplats hello gäller tooStorSimple 8000-serien endast enheter. Fullständig information om hello 5000 och 7000-serien enheter går du till: [http://onlinehelp.storsimple.com/](http://onlinehelp.storsimple.com). 5000 och 7000 serien information om distribution finns hello [StorSimple Snabbstartsguide System](http://onlinehelp.storsimple.com/111_Appliance/).
> 
> 

## <a name="deployment-steps"></a>Distributionssteg
Utföra dessa steg tooconfigure StorSimple-enheten och anslut den tooyour StorSimple Manager-tjänsten. Dessutom toohello krävs steg finns det valfria steg och procedurer som du kan behöva under hello-distribution. instruktionerna för hello stegvis distribution anger när du ska utföra de olika valfria stegen.

| Steg | Beskrivning |
| --- | --- |
| **Förutsättningar** |Dessa måste toobe slutförts inför kommande hello-distributionen. |
| Checklista för distributionskonfiguration. |Använd den här checklistan toogather och registrera information tidigare tooand under hello distributionen. |
| Distributionskrav. |Dessa verifierar hello miljön är klar för distribution. |
|  | |
| **Distribution steg för steg** |Dessa steg är nödvändiga toodeploy StorSimple-enheten i produktionen. |
| Steg 1: Skapa en ny tjänst. |Ställ in hantering och lagring i molnet för din StorSimple-enhet. Hoppa över det här steget om du har en befintlig tjänst för andra StorSimple-enheter. |
| Steg 2: Hämta nyckel för tjänstregistrering hello. |Använd den här nyckeln tooregister & ansluta din StorSimple-enhet med hello management-tjänsten. |
| Steg 3: Konfigurera och registrera hello enheten via Windows PowerShell för StorSimple. |Ansluta hello enheten tooyour nätverket och registrera den med Azure toocomplete hello installationen med hjälp av hello management-tjänsten. |
| Steg 4: Slutför de inställningar som krävs som minimum</br>Valfritt: Uppdatera din StorSimple-enhet. |Använd hello management service toocomplete hello Enhetsinställningar och aktivera det tooprovide lagring. |
| Steg 5: Skapa en volymbehållare. |Skapa en behållare tooprovision volymer. En volymbehållare har lagringskonto, bandbredd och krypteringsinställningar för hello volymer som finns i den. |
| Steg 6: Skapa en volym. |Etablera lagringsvolymer på hello StorSimple-enheten för dina servrar. |
| Steg 7: Montera, initiera och formatera en volym.</br>Valfritt: Konfigurera MPIO. |Anslut dina servrar toohello iSCSI-lagring som tillhandahålls av hello enhet. Du kan också konfigurera MPIO tooensure att dina servrar kan tolerera länkar, nätverk och fel. |
| Steg 8: Gör en säkerhetskopia. |Konfigurera din säkerhetskopieringsprincip tooprotect dina data |
|  | |
| **Andra procedurer** |Du kan behöva toorefer toothese procedurer när du distribuerar din lösning. |
| Konfigurera ett nytt lagringskonto för hello-tjänsten. | |
| Använd PuTTY tooconnect toohello enhetens seriekonsol. | |
| Hämta hello IQN för en Windows Server-värd. | |
| Skapa en manuell säkerhetskopia. | |

## <a name="deployment-configuration-checklist"></a>Checklista för distributionskonfiguration
Följande checklista för distributionskonfiguration hello beskriver hello-information som du behöver toocollect innan och när du konfigurerar hello programvara på din StorSimple-enhet. Förbereda en del av informationen i förväg effektiviserar hello processen för att distribuera hello StorSimple-enheten i din miljö. Använd den här checklistan tooalso Skriv ner hello konfigurationsinformation som du distribuerar din enhet.

| Fas | Parameter | Detaljer | Värden |
| --- | --- | --- | --- |
| **Kabelanslut din enhet** |Seriell åtkomst |Första enhetskonfigurationen |Ja/nej |
|  | | | |
| **Konfigurera och registrera enheten** |Data 0 nätverksinställningar |Data 0 IP-adress:</br>Nätmask:</br>Gateway:</br>Primär DNS-server:</br>Primär NTP-server:</br>Webbproxyserver IP/FQDN (valfritt):</br>Webbproxyport: | |
| &nbsp; |Enhetens administratörslösenord |Lösenordet ska vara mellan 8 och 15 tecken och innehålla gemener, versaler, siffror och specialtecken. | |
| &nbsp; |Lösenordet för StorSimple Snapshot Manager |Lösenordet ska vara 14 eller 15 tecken och innehålla gemener, versaler, siffror och specialtecken. | |
| &nbsp; |Nyckel för tjänstregistrering |Den här nyckeln skapas från hello klassiska Azure-portalen. | |
| &nbsp; |Krypteringsnyckel för tjänstdata |Den här nyckeln skapas när hello enheten är registrerad med hello management-tjänsten via hello Windows PowerShell för StorSimple. Kopiera den här nyckeln och spara den på säker plats. | |
|  | | | |
| **Slutför de inställningar som krävs som minimum** |Eget namn på din enhet |Det här är ett beskrivande namn för hello enheten. | |
| &nbsp; |Tidszon |Enheten använder den här tidszonen för alla schemalagda åtgärder. | |
| &nbsp; |Sekundär DNS-server |Det här måste konfigureras. | |
| &nbsp; |Nätverksgränssnitt: Data 0-kontrollant med statiska IP-adresser |Dessa IP-Adressen ska vara dirigerbara toohello Internet.</br>Kontrollant 0, statisk IP-adress:</br>Kontrollant 1, statisk IP-adress: | |
|  | | | |
| **Ytterligare inställningar för nätverksgränssnittet** |Nätverksgränssnitt: Data 1</br>Konfigurera inte hello gatewayen om iSCSI är aktiverad. |Syfte: Molnet/iSCSI/används inte</br>IP-adress:</br>Nätmask:</br>Gateway: | |
| &nbsp; |Nätverksgränssnitt: Data 2</br>Konfigurera inte hello gatewayen om iSCSI är aktiverad. |Syfte: Molnet/iSCSI/används inte</br>IP-adress:</br>Nätmask:</br>Gateway: | |
| &nbsp; |Nätverksgränssnitt: Data 3</br>Konfigurera inte hello gatewayen om iSCSI är aktiverad. |Syfte: Molnet/iSCSI/används inte</br>IP-adress:</br>Nätmask:</br>Gateway: | |
| &nbsp; |Nätverksgränssnitt: Data 4</br>Konfigurera inte hello gatewayen om iSCSI är aktiverad. |Syfte: Molnet/iSCSI/används inte</br>IP-adress:</br>Nätmask:</br>Gateway: | |
| &nbsp; |Nätverksgränssnitt: Data 5</br>Konfigurera inte hello gatewayen om iSCSI är aktiverad. |Syfte: Molnet/iSCSI/används inte</br>IP-adress:</br>Nätmask:</br>Gateway: | |
|  | | | |
| **Skapa en volymcontainer** |Volymbehållarens namn: |Namn för hello behållare | |
| &nbsp; |Azure lagringskonto: |Storage-konto och nyckel tooassociate med volymbehållaren | |
| &nbsp; |Krypteringsnyckel för molnlagring: |Krypteringsnyckeln för lagring i varje behållare | |
|  | | | |
| **Skapa en volym** |Information för varje volym |Volymnamn: | |
| &nbsp; |&nbsp; |Storlek: | |
| &nbsp; |&nbsp; |Användningstyp: | |
| &nbsp; |&nbsp; |ACR-namn: | |
| &nbsp; |&nbsp; |Standardprincip för säkerhetskopiering: | |
|  | | | |
| **Montera, initiera och formatera en volym** |Information om varje värdserver som ansluter toohello lagring |Windows Server namn: | |
| &nbsp; |&nbsp; |Windows Server IQN: | |
| &nbsp; |&nbsp; |Windows Server volymnamn: | |
| &nbsp; |&nbsp; |NTFS monteringspunkt/enhetsbeteckning: | |

## <a name="deployment-prerequisites"></a>Distributionskrav
hello följande avsnitt beskrivs kraven för din StorSimple Manager-tjänsten, din StorSimple-enhet och hello nätverket i datacentret hello-konfiguration.

### <a name="for-hello-storsimple-manager-service"></a>För hello StorSimple Manager-tjänsten
Innan du börjar bör du kontrollera att:

* Du har ditt Microsoft-konto med autentiseringsuppgifter.
* Du har ditt Microsoft Azure lagringskonto med autentiseringsuppgifter.
* Microsoft Azure-prenumerationen har aktiverats för hello StorSimple Manager-tjänsten. Din prenumeration ska köpas via hello [Enterprise-avtal](https://azure.microsoft.com/pricing/enterprise-agreement/).
* Du har åtkomst tooterminal emulering programvara, till exempel PuTTY.

### <a name="for-hello-device-in-hello-datacenter"></a>För hello-enhet i hello datacenter
Innan du konfigurerar hello enheten kontrollerar du att:

* Din enhet är helt uppackad, rackmonterad och helt kabelansluten till ström, nätverk och serieåtkomst enligt beskrivningen i:
  
  * [Packa upp, rackmontera och kabelanslut din 8100-enhet](storsimple-8100-hardware-installation.md)
  * [Packa upp, rackmontera och kabelanslut din 8600-enhet](storsimple-8600-hardware-installation.md)

### <a name="for-hello-network-in-hello-datacenter"></a>För hello nätverket i hello datacenter
Innan du börjar bör du kontrollera att:

* hello portarna i ditt datacenterbrandvägg är öppna tooallow för iSCSI och molntrafik enligt beskrivningen i [nätverkskrav för din StorSimple-enhet](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).
* hello enheten i ditt datacenter kan ansluta toooutside nätverk. Kör följande hello [Windows PowerShell 4.0](http://www.microsoft.com/download/details.aspx?id=40855) cmdlets (tabell nedan) toovalidate hello anslutningen toohello utanför nätverket. Utför verifieringen på en dator (i datacenternätverk) som har anslutning tooAzure och där du ska distribuera StorSimple-enheten.  

| För denna parameter... | toocheck hello giltigheten... | Kör följande kommandon/cmdlets. |
| --- | --- | --- |
| **IP**</br>**Undernät**</br>**Gateway** |Är detta en giltig IPv4- eller IPv6-adress?</br>Är detta ett giltigt undernät?</br>Är detta en giltig gateway?</br>Är detta en duplicerad IP-adress på nätverket? |`ping ip`</br>`arp -a`</br>Hej `ping` och `arp` kommandon bör misslyckas som anger att det finns ingen enhet i hello datacenternätverket som använder den här IP-adress. |
|  | | |
| **DNS** |Är detta en giltig DNS och kan lösa Azure URL: er? |`Resolve-DnsName -Name www.bing.com -Server <DNS server IP address>` </br>Ett alternativkommando som kan användas är:</br>`nslookup --dns-ip=<DNS server IP address> www.bing.com` |
| &nbsp; |Kontrollera om port 53 är öppen. Detta gäller endast om du använder en extern DNS för din enhet. Internt DNS ska automatiskt lösa hello externa URL: er. |`Test-Port -comp dc1 -port 53 -udp -UDPtimeout 10000`  </br>[Mer information om denna cmdlet](http://learn-powershell.net/2011/02/21/querying-udp-ports-with-powershell/) |
|  | | |
| **NTP** |Vi utlöser en tidssynkronisering så snart det finns indata på NTP-servern. Kontrollera att UDP-porten 123 är öppen vid inmatning av `time.windows.com` eller offentliga tidsservrar). |[Hämta och använda det här skriptet](https://gallery.technet.microsoft.com/scriptcenter/Get-Network-NTP-Time-with-07b216ca). |
|  | | |
| **Proxy (valfritt)** |Är detta en giltig proxy-URI och port? </br> Är hello autentiseringsläget korrekt? |<code>wget http://bing.com &#124; % {$_.StatusCode}</code></br>Det här kommandot ska köras omedelbart efter webbproxykonfigurationen. Om statuskoden 200 returneras, indikerar det att hello anslutningen är klar. |
| &nbsp; |Kan trafik dirigeras via proxy? |Kör hello DNS-verifiering, NTP-kontroll eller HTTP-kontroll en gång när du har konfigurerat proxy på enheten. Detta ger en klar bild över om trafik blockeras vid proxyn eller någon annanstans. |
|  | | |
| **Registrering** |Kontrollera om de utgående TCP-portarna 443, 80, 9354 är öppna. |`Test-NetConnection -Port   443 -InformationLevel Detailed`</br>[Mer information om Test-NetConnection cmdlet](https://technet.microsoft.com/library/dn372891.aspx) |

## <a name="step-by-step-deployment"></a>Steg för steg-distribution
Använd följande steg för steg-instruktioner toodeploy hello din StorSimple-enhet i hello datacenter.

## <a name="step-1-create-a-new-service"></a>Steg 1: Skapa en ny tjänst
En StorSimple Manager-tjänst kan hantera flera StorSimple-enheter. Hello distribution av din första StorSimple-enhet måste toocreate en ny StorSimple Manager-tjänst.

> [!IMPORTANT]
> Hoppa över det här steget om du har en befintlig StorSimple Manager-tjänst och du planerar toodeploy StorSimple-enheten med den tjänsten.
> 
> 

Utföra hello följande steg toocreate en ny instans av hello StorSimple Manager-tjänsten.

[!INCLUDE [storsimple-create-new-service](../../includes/storsimple-create-new-service.md)]

> [!IMPORTANT]
> Om du inte har aktiverat hello automatiskt skapande av lagringskonton med din tjänst måste toocreate minst ett lagringskonto efter att du har skapat en tjänst. Det här lagringskontot används när du skapar en volymbehållare.
> 
> Om du inte har skapat ett lagringskonto automatiskt, gå för[konfigurera ett nytt lagringskonto för tjänsten hello](#configure-a-new-storage-account-for-the-service) detaljerade anvisningar.
> Om du har aktiverat hello automatiskt skapande av lagringskonton går för[steg 2: hämta hello nyckel för tjänstregistrering](#step-2:-get-the-service-registration-key).
> 
> 

## <a name="step-2-get-hello-service-registration-key"></a>Steg 2: Hämta nyckel för tjänstregistrering hello
När hello StorSimple Manager-tjänsten är igång kan behöver du tooget hello nyckel för tjänstregistrering. Den här nyckeln används tooregister och ansluta din StorSimple-enhet med hello-tjänsten.

Utföra hello följa stegen i hello klassiska Azure-portalen.

[!INCLUDE [storsimple-get-service-registration-key](../../includes/storsimple-get-service-registration-key.md)]

## <a name="step-3-configure-and-register-hello-device-through-windows-powershell-for-storsimple"></a>Steg 3: Konfigurera och registrera hello enheten via Windows PowerShell för StorSimple
> [!IMPORTANT]
> Tidigare tooperforming den här konfigurationen, koppla från alla hello nätverksgränssnitt än DATA 0 på båda (aktiva och passiva) hello-domänkontrollanter.
> 
> 

Använda Windows PowerShell för StorSimple toocomplete hello första installationen av StorSimple-enheten enligt beskrivningen i hello nedan. Du behöver toouse terminalemulering programvara toocomplete det här steget. Mer information finns i [använda PuTTY tooconnect toohello enhetens seriekonsol](#use-putty-to-connect-to-the-device-serial-console).

[!INCLUDE [storsimple-configure-and-register-device](../../includes/storsimple-configure-and-register-device.md)]

## <a name="step-4-complete-minimum-device-setup"></a>Steg 4: Slutför de inställningar som krävs som minimum
Hello minimikrav konfiguration av din StorSimple-enhet måste du:

* Ställ in hello sekundära DNS-servern.
* Aktivera iSCSI i minst ett nätverksgränssnitt.
* Tilldela den fasta IP-adresser tooboth hello domänkontrollanter.

Utföra hello följa stegen i hello Azure klassiska portal toocomplete hello minimala Enhetsinstallationen.

[!INCLUDE [storsimple-complete-minimum-device-setup](../../includes/storsimple-complete-minimum-device-setup.md)]

När hello enhetskonfigurationen är klar måste du söka efter uppdateringar och installera uppdateringar. hello uppdateringar kan ta flera timmar toocomplete. Följ anvisningarna för hello i [söka efter och installera uppdateringar](#scan-for-and-apply-updates).

## <a name="step-5-create-a-volume-container"></a>Steg 5: Skapa en volymbehållare
En volymbehållare har lagringskonto, bandbredd och krypteringsinställningar för hello volymer som finns i den. Du behöver toocreate en volymbehållare innan du kan börja etablera volymer på StorSimple-enheten.

Utför följande steg i hello Azure klassiska portal toocreate en volymbehållare hello.

[!INCLUDE [storsimple-create-volume-container](../../includes/storsimple-create-volume-container.md)]

## <a name="step-6-create-a-volume"></a>Steg 6: Skapa en volym
När du har skapat en volymbehållare kan du etablera en lagringsvolym på StorSimple-enhet för hello för dina servrar. Utföra hello följa stegen i hello Azure klassiska portal toocreate en volym.

> [!IMPORTANT]
> StorSimple Manager kan endast skapa tunt allokerade volymer.  Du kan inte skapa helt eller delvis etablerade volymer.
> 
> 

[!INCLUDE [storsimple-create-volume](../../includes/storsimple-create-volume.md)]

## <a name="step-7-mount-initialize-and-format-a-volume"></a>Steg 7: Montera, initiera och formatera en volym
> [!IMPORTANT]
> * För hello hög tillgänglighet i StorSimple-lösningen rekommenderar vi att du konfigurerar MPIO på din Windows Server-värd (valfritt) tidigare tooconfiguring iSCSI på Windows Server-värd. MPIO-konfiguration på värdservrar säkerställer att hello servrar kan tolerera länken, nätverk och fel.
> * MPIO och iSCSI-installation och konfiguration anvisningar finns för[konfigurera MPIO för din StorSimple-enhet](storsimple-configure-mpio-windows-server.md). Dessa kommer också inkludera hello steg toomount, initiera och formatera StorSimple-volymer.
> 
> 

Om du bestämma inte tooconfigure MPIO, utför följande steg toomount hello, initiera och formatera dina StorSimple-volymer.

[!INCLUDE [storsimple-mount-initialize-format-volume](../../includes/storsimple-mount-initialize-format-volume.md)]

## <a name="step-8-take-a-backup"></a>Steg 8: Gör en säkerhetskopia
Säkerhetskopieringar ger tidpunktsskydd för volymer och förbättrar återställningsmöjligheterna samtidigt som återställningstiderna minimeras. Du kan utföra två typer av säkerhetskopiering på StorSimple-enheten: lokala ögonblicksbilder och molnögonblicksbilder. Båda av de här säkerhetskopieringstyperna kan vara antingen **schemalagda** eller **manuella**.

Utför följande steg i hello Azure klassiska portal toocreate en schemalagd säkerhetskopiering hello.

[!INCLUDE [storsimple-take-backup](../../includes/storsimple-take-backup.md)]

Du kan utföra en manuell säkerhetskopiering när som helst. Procedurer finns för[skapa en manuell säkerhetskopiering](#Create-a-manual-backup).

## <a name="configure-a-new-storage-account-for-hello-service"></a>Konfigurera ett nytt lagringskonto för hello-tjänsten
Det här är ett valfritt steg som du behöver tooperform endast om du inte har aktiverat hello automatiskt skapande av lagringskonton med din tjänst. Ett Microsoft Azure storage-konto är obligatoriskt toocreate en behållare för StorSimple-volym.

Om du behöver toocreate ett Azure storage-konto i en annan region finns [om Azure Lagringskonton](../storage/common/storage-create-storage-account.md) stegvisa instruktioner.

Utför följande steg i hello klassiska Azure-portalen på hello hello **StorSimple Manager-tjänsten** sidan.

[!INCLUDE [storsimple-configure-new-storage-account](../../includes/storsimple-configure-new-storage-account.md)]

## <a name="use-putty-tooconnect-toohello-device-serial-console"></a>Använd PuTTY tooconnect toohello enhetens seriekonsol
tooconnect tooWindows PowerShell för StorSimple, behöver du toouse programvara för terminalemulering, till exempel PuTTY. Du kan använda PuTTY när du använder hello enhet direkt via seriekonsolen hello eller genom att öppna en telnet-session från en fjärrdator.

[!INCLUDE [Use PuTTY tooconnect toohello device serial console](../../includes/storsimple-use-putty.md)]

## <a name="scan-for-and-apply-updates"></a>Söka efter och installera uppdateringar
Det tar mellan 1 - 4 timmar att uppdatera din enhet. Utför följande steg tooscan för hello och tillämpa uppdateringar på din enhet.

> [!NOTE]
> Om du har en gateway som konfigurerats för ett nätverksgränssnitt än Data 0 behöver nätverksgränssnitten för toodisable Data 2 och Data 3 innan du installerar hello uppdatering. Gå för**enheter > Konfigurera** och inaktivera Data 2 och Data 3 gränssnitt. Du bör återaktivera gränssnitten när hello enheten har uppdaterats.
> 
> 

#### <a name="tooupdate-your-device"></a>tooupdate din enhet
1. På hello enhet **Snabbstart** klickar du på **enheter**. Välj hello fysiska enheten, klicka på **Underhåll** och klicka sedan på **Sök efter uppdateringar**.  
2. Ett jobb tooscan efter uppdateringar har skapats. Om det finns uppdateringar, hello **Sök efter uppdateringar** ändras för**installera uppdateringar**. Klicka på **Installera uppdateringar**. Du kanske begärda toodisable Data 2 och Data 3 tidigare tooinstalling hello uppdateringar. Du måste inaktivera dessa nätverksgränssnitt eller hello uppdateringar kan misslyckas.
3. Ett uppdateringsjobb skapas. Övervaka hello statusen för uppdateringen genom att navigera för**jobb**.
   
   > [!NOTE]
   > När hello Uppdateringsjobbet startar visas omedelbart hello statusen som 50 procent. hello status ändras sedan too100 procent först när hello Uppdateringsjobbet har slutförts. Det finns ingen realtidsstatus för uppdateringsprocessen hello.
   > 
   > 
4. När hello enheten har uppdaterats, aktiverar du nätverksgränssnitten Data 2 och Data 3 om dessa inaktiverades.

## <a name="get-hello-iqn-of-a-windows-server-host"></a>Hämta hello IQN för en Windows Server-värd
Utför följande steg tooget hello iSCSI hello kvalificerade namn (IQN) för en Windows-värd som kör Windows Server 2012.

[!INCLUDE [Create a manual backup](../../includes/storsimple-get-iqn.md)]

## <a name="create-a-manual-backup"></a>Skapa en manuell säkerhetskopia
Utföra hello följa stegen i hello Azure klassiska portal toocreate en på-begäran manuell säkerhetskopiering för en enskild volym på din StorSimple-enhet.

[!INCLUDE [Create a manual backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="next-steps"></a>Nästa steg
* Konfigurera en [virtuell enhet](storsimple-virtual-device-u2.md).
* Använd hello [StorSimple Manager-tjänsten](https://msdn.microsoft.com/library/azure/dn772396.aspx) toomanage din StorSimple-enhet.


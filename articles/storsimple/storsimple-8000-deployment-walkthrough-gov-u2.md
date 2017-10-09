---
title: aaaDeploy StorSimple 8000-serieenhet i Government portal | Microsoft Docs
description: "Beskriver hello stegen och bästa praxis för distribution av hello StorSimple 8000-serieenhet som kör uppdatering 3 och senare och hello-tjänsten i hello Azure Government-portalen."
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
ms.workload: NA
ms.date: 06/22/2017
ms.author: alkohli
ms.openlocfilehash: ea643cd59dcdf17482268d14c1348a3b5fb098b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-on-premises-storsimple-device-in-hello-government-portal"></a>Distribuera din lokala StorSimple-enhet i hello Government portal

## <a name="overview"></a>Översikt
Välkommen till distribution av tooMicrosoft Azure StorSimple-enheten. De här kurserna om distribution gäller toohello StorSimple 8000-serien Kör uppdatering 3 programvara eller senare i hello Azure Government-portalen. Denna serie självstudier innehåller en checklista för konfiguration, en lista över konfigurationskrav och detaljerade konfigurationssteg för din StorSimple-enhet.

hello informationen i de här kurserna förutsätter att du har granskat hello säkerhetsåtgärder, packat upp, rackmonterat och kabelanslutit din StorSimple-enhet. Om du fortfarande behöver tooperform de uppgifter du börja med att granska hello [säkerhetsåtgärderna](storsimple-safety.md). Följ instruktionerna för hello enhetsspecifika toounpack, rackmontera och kabelanslut din enhet.

* [Packa upp, rackmontera och kabelanslut din 8100](storsimple-8100-hardware-installation.md)
* [Packa upp, rackmontera och kabelanslut din 8600](storsimple-8600-hardware-installation.md)

Du måste administratören privilegier toocomplete hello installationen och konfigurationen processen. Vi rekommenderar att du granskar hello checklista för konfiguration innan du börjar. hello kan distribution och konfiguration ta viss tid toocomplete.

> [!NOTE]
> distributionsinformation för hello StorSimple publiceras på Microsoft Azure-webbplats hello gäller tooStorSimple 8000-serien endast enheter. Fullständig information om hello 7000-serien enheter går du till: [http://onlinehelp.storsimple.com/](http://onlinehelp.storsimple.com). Distributionsinformation för 7000-serien finns hello [StorSimple Snabbstartsguide System](http://onlinehelp.storsimple.com/111_Appliance/).


## <a name="deployment-steps"></a>Distributionssteg
Utföra dessa steg tooconfigure StorSimple-enheten och anslut den tooyour StorSimple enheten Manager-tjänsten. Dessutom toohello krävs steg finns det valfria steg och procedurer som du kan behöva toocomplete under hello distributionen. instruktionerna för hello stegvis distribution anger när du ska utföra de olika valfria stegen.

| Steg | Beskrivning |
| --- | --- |
| **Förutsättningar** |Dessa måste toobe slutförts inför kommande hello-distributionen. |
| [Checklista för distributionskonfiguration](#deployment-configuration-checklist) |Använd den här checklistan toogather och registrera information tidigare tooand under hello distributionen. |
| [Förhandskrav för distribution](#deployment-prerequisites) |Dessa verifierar att hello miljön är klar för distribution. |
|  | |
| **Distribution steg för steg** |Dessa steg är nödvändiga toodeploy StorSimple-enheten i produktionen. |
| [Steg 1: Skapa en ny tjänst](#step-1-create-a-new-service) |Konfigurera hantering och lagring i molnet för StorSimple-enheten. *Hoppa över det här steget om du har en befintlig tjänst för andra StorSimple-enheter*. |
| [Steg 2: Hämta nyckel för tjänstregistrering hello](#step-2-get-the-service-registration-key) |Använd den här nyckeln tooregister och ansluta din StorSimple-enhet med hello management-tjänsten. |
| [Steg 3: Konfigurera och registrera hello enheten via Windows PowerShell för StorSimple](#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple) |Ansluta hello enheten tooyour nätverket och registrera den med Azure toocomplete hello installationen med hjälp av hello management-tjänsten. |
| [Steg 4: Slutför hello minimikrav för konfiguration](#step-4-complete-minimum-device-setup) </br>Valfritt: Uppdatera din StorSimple-enhet. |Använd hello management service toocomplete hello Enhetsinställningar och aktivera det tooprovide lagring. |
| [Steg 5: Skapa en volymbehållare](#step-5-create-a-volume-container) |Skapa en behållare tooprovision volymer. En volymbehållare har lagringskonto, bandbredd och krypteringsinställningar för hello volymer som finns i den. |
| [Steg 6: Skapa en volym](#step-6-create-a-volume) |Etablera lagringsvolymer på hello StorSimple-enheten för dina servrar. |
| [Steg 7: Montera, initiera och formatera en volym](#step-7-mount-initialize-and-format-a-volume) </br>Valfritt: Konfigurera MPIO. |Anslut dina servrar toohello iSCSI-lagring som tillhandahålls av hello enhet. Du kan också konfigurera MPIO tooensure att dina servrar kan tolerera länkar, nätverk och fel. |
| [Steg 8: Ta en säkerhetskopia](#step-8-take-a-backup) |Konfigurera din säkerhetskopieringsprincip tooprotect dina data |
|  | |
| **Andra procedurer** |Du kan behöva toorefer toothese procedurer när du distribuerar din lösning. |
| [Konfigurera ett nytt lagringskonto för hello-tjänsten](#configure-a-new-storage-account-for-the-service) | |
| [Använd PuTTY tooconnect toohello enhetens seriekonsol](#use-putty-to-connect-to-the-device-serial-console) | |
| [Söka efter och installera uppdateringar](#scan-for-and-apply-updates) | |
| [Hämta hello IQN för en Windows Server-värd](#get-the-iqn-of-a-windows-server-host) | |
| [Skapa en manuell säkerhetskopia](#create-a-manual-backup) | |


## <a name="deployment-configuration-checklist"></a>Checklista för distributionskonfiguration
Innan du distribuerar din StorSimple-enhet behöver toocollect information tooconfigure hello programvara på din enhet. Förbereda en del av informationen i förväg effektiviserar hello processen för att distribuera hello StorSimple-enheten i din miljö. Hämta och använda den här checklistan toonote hello konfigurationsinformation som du distribuerar din enhet.

[Hämta konfigurationschecklistan för StorSimple-distributionen](http://www.microsoft.com/download/details.aspx?id=49159)

## <a name="deployment-prerequisites"></a>Distributionskrav
hello följande avsnitt beskrivs kraven för din StorSimple Device Manager-tjänst och StorSimple-enheten hello-konfiguration.

### <a name="for-hello-storsimple-device-manager-service"></a>För hello StorSimple enheten Manager-tjänsten
Innan du börjar bör du kontrollera att:

* Du har ditt Microsoft-konto med autentiseringsuppgifter.
* Du har ditt Microsoft Azure lagringskonto med autentiseringsuppgifter.
* Microsoft Azure-prenumerationen har aktiverats för hello StorSimple enheten Manager-tjänsten. Din prenumeration ska köpas via hello [Enterprise-avtal](https://azure.microsoft.com/pricing/enterprise-agreement/).
* Du har åtkomst tooterminal emulering programvara, till exempel PuTTY.

### <a name="for-hello-device-in-hello-datacenter"></a>För hello-enhet i hello datacenter
Innan du konfigurerar hello enheten kontrollerar du att:

* Din enhet är helt uppackad, rackmonterad och helt kabelansluten till ström, nätverk och serieåtkomst enligt beskrivningen i:
  
  * [Packa upp, rackmontera och kabelanslut din 8100-enhet](storsimple-8100-hardware-installation.md)
  * [Packa upp, rackmontera och kabelanslut din 8600-enhet](storsimple-8600-hardware-installation.md)

### <a name="for-hello-network-in-hello-datacenter"></a>För hello nätverket i hello datacenter
Innan du börjar bör du kontrollera att:

* hello portarna i ditt datacenterbrandvägg är öppna tooallow för iSCSI och molntrafik enligt beskrivningen i [nätverkskrav för din StorSimple-enhet](storsimple-8000-system-requirements.md#networking-requirements-for-your-storsimple-device).

## <a name="step-by-step-deployment"></a>Steg för steg-distribution
Använd följande steg för steg-instruktioner toodeploy hello din StorSimple-enhet i hello datacenter.

## <a name="step-1-create-a-new-service"></a>Steg 1: Skapa en ny tjänst
En StorSimple Device Manager-tjänst kan hantera flera StorSimple-enheter. Utföra hello följande steg toocreate en ny instans av hello StorSimple enheten Manager-tjänsten.

[!INCLUDE [storsimple-8000-create-new-service-gov](../../includes/storsimple-8000-create-new-service-gov.md)]

> [!IMPORTANT]
> Om du inte har aktiverat hello automatiskt skapande av lagringskonton med din tjänst måste toocreate minst ett lagringskonto efter att du har skapat en tjänst. Det här lagringskontot används när du skapar en volymbehållare.
> 
> * Om du inte har skapat ett lagringskonto automatiskt, gå för[konfigurera ett nytt lagringskonto för tjänsten hello](#configure-a-new-storage-account-for-the-service) detaljerade anvisningar.
> * Om du har aktiverat hello automatiskt skapande av lagringskonton går för[steg 2: hämta hello nyckel för tjänstregistrering](#step-2-get-the-service-registration-key).


## <a name="step-2-get-hello-service-registration-key"></a>Steg 2: Hämta nyckel för tjänstregistrering hello
När hello StorSimple enheten Manager-tjänsten är igång kan behöver du tooget hello nyckel för tjänstregistrering. Den här nyckeln används tooregister och ansluta din StorSimple-enhet toohello tjänst.

Utföra hello följa stegen i hello Government-portalen.

[!INCLUDE [storsimple-8000-get-service-registration-key](../../includes/storsimple-8000-get-service-registration-key.md)]

## <a name="step-3-configure-and-register-hello-device-through-windows-powershell-for-storsimple"></a>Steg 3: Konfigurera och registrera hello enheten via Windows PowerShell för StorSimple
Använda Windows PowerShell för StorSimple toocomplete hello första installationen av StorSimple-enheten enligt beskrivningen i hello nedan. Du behöver toouse terminalemulering programvara toocomplete det här steget. Mer information finns i [använda PuTTY tooconnect toohello enhetens seriekonsol](#use-putty-to-connect-to-the-device-serial-console).

[!INCLUDE [storsimple-8000-configure-and-register-device-gov](../../includes/storsimple-8000-configure-and-register-device-gov-u2.md)]

## <a name="step-4-complete-minimum-device-setup"></a>Steg 4: Slutför de inställningar som krävs som minimum
Hello minimikrav konfiguration av din StorSimple-enhet måste du:

* Ange ett eget namn för din enhet.
* Ange hello enheten tidszon.
* Tilldela den fasta IP-adresser tooboth hello domänkontrollanter.

Utföra hello följa stegen i hello Azure Government portal toocomplete hello minimikrav för konfiguration.

[!INCLUDE [storsimple-8000-complete-minimum-device-setup-u2](../../includes/storsimple-8000-complete-minimum-device-setup-u2.md)]

## <a name="step-5-create-a-volume-container"></a>Steg 5: Skapa en volymbehållare
En volymbehållare har lagringskonto, bandbredd och krypteringsinställningar för hello volymer som finns i den. Du behöver toocreate en volymbehållare innan du kan börja etablera volymer på StorSimple-enheten.

Utför följande steg i hello Government portal toocreate en volymbehållare hello.

[!INCLUDE [storsimple-8000-create-volume-container](../../includes/storsimple-8000-create-volume-container.md)]

## <a name="step-6-create-a-volume"></a>Steg 6: Skapa en volym
När du har skapat en volymbehållare kan du etablera en lagringsvolym på StorSimple-enhet för hello för dina servrar. Utför följande steg i hello Government portal toocreate en volym hello.

> [!IMPORTANT]
> StorSimple Enhetshanteraren kan skapa endast tunt allokerade volymer.  Du kan dock inte skapa delvis allokerade volymer.

[!INCLUDE [storsimple-8000-create-volume](../../includes/storsimple-8000-create-volume-u2.md)]

## <a name="step-7-mount-initialize-and-format-a-volume"></a>Steg 7: Montera, initiera och formatera en volym
Utför de här stegen på Windows Server-värd.

> [!IMPORTANT]
> * För hello hög tillgänglighet i StorSimple-lösningen rekommenderar vi att du konfigurerar MPIO på din värd tidigare tooconfiguring iSCSI-servrar (valfritt). MPIO-konfiguration på värdservrar säkerställer att hello servrar kan tolerera länken, nätverk och fel.
> * Anvisningar MPIO och iSCSI-installation och konfiguration för Windows Server-värden, gå för[konfigurera MPIO för din StorSimple-enhet](storsimple-configure-mpio-windows-server.md). Dessa kommer också inkludera hello steg toomount, initiera och formatera StorSimple-volymer.
> * Anvisningar MPIO och iSCSI-installation och konfiguration för en Linux-värd, gå för[konfigurera MPIO för din StorSimple Linux-värd](storsimple-configure-mpio-on-linux.md)

Om du bestämma inte tooconfigure MPIO, utför följande steg toomount hello, initiera och formatera dina StorSimple-volymer på en Windows Server-värd.

[!INCLUDE [storsimple-mount-initialize-format-volume](../../includes/storsimple-mount-initialize-format-volume.md)]

## <a name="step-8-take-a-backup"></a>Steg 8: Gör en säkerhetskopia
Säkerhetskopieringar ger tidpunktsskydd för volymer och förbättrar återställningsmöjligheterna samtidigt som återställningstiderna minimeras. Du kan utföra två typer av säkerhetskopiering på StorSimple-enheten: lokala ögonblicksbilder och molnögonblicksbilder. Båda av de här säkerhetskopieringstyperna kan vara antingen **schemalagda** eller **manuella**.

Utför följande steg i hello Government portal toocreate en schemalagd säkerhetskopiering hello.

[!INCLUDE [storsimple-8000-take-backup](../../includes/storsimple-8000-take-backup.md)]

Du kan utföra en manuell säkerhetskopiering när som helst. Procedurer finns för[skapa en manuell säkerhetskopiering](#create-a-manual-backup).

## <a name="configure-a-new-storage-account-for-hello-service"></a>Konfigurera ett nytt lagringskonto för hello-tjänsten
Det här är ett valfritt steg som du behöver tooperform endast om du inte har aktiverat hello automatiskt skapande av lagringskonton med din tjänst. Ett Microsoft Azure storage-konto är obligatoriskt toocreate en behållare för StorSimple-volym.

Om du behöver toocreate ett Azure storage-konto i en annan region finns [om Azure Lagringskonton](../storage/common/storage-create-storage-account.md) stegvisa instruktioner.

Utför följande steg i hello Government-portalen på hello hello **StorSimple Enhetshanteraren service** sidan.

[!INCLUDE [storsimple-configure-new-storage-account-u1](../../includes/storsimple-8000-configure-new-storage-account-u2.md)]

## <a name="use-putty-tooconnect-toohello-device-serial-console"></a>Använd PuTTY tooconnect toohello enhetens seriekonsol
tooconnect tooWindows PowerShell för StorSimple, behöver du toouse programvara för terminalemulering, till exempel PuTTY. Du kan använda PuTTY när du använder hello enhet direkt via seriekonsolen hello eller genom att öppna en telnet-session från en fjärrdator.

[!INCLUDE [Use PuTTY tooconnect toohello device serial console](../../includes/storsimple-use-putty.md)]

## <a name="scan-for-and-apply-updates"></a>Söka efter och installera uppdateringar
Det kan ta flera timmar att uppdatera din enhet. Detaljerade anvisningar om hur tooinstall hello senaste uppdateringen finns för[installera uppdatering 4](storsimple-8000-install-update-4.md).

## <a name="get-hello-iqn-of-a-windows-server-host"></a>Hämta hello IQN för en Windows Server-värd
Utför följande steg tooget hello iSCSI hello kvalificerade namn (IQN) för en Windows-värd som kör Windows Server® 2012.

[!INCLUDE [Get IQN of your Windows Server host](../../includes/storsimple-get-iqn.md)]

## <a name="create-a-manual-backup"></a>Skapa en manuell säkerhetskopia
Utföra hello följa stegen i hello Government portal toocreate en manuell säkerhetskopiering på begäran för en enskild volym på din StorSimple-enhet.

[!INCLUDE [Create a manual backup](../../includes/storsimple-8000-create-manual-backup.md)]

## <a name="next-steps"></a>Nästa steg
* Konfigurera en [virtuell enhet](storsimple-8000-cloud-appliance-u2.md).
* Använd hello [StorSimple Enhetshanteraren service](storsimple-8000-manager-service-administration.md) toomanage din StorSimple-enhet.


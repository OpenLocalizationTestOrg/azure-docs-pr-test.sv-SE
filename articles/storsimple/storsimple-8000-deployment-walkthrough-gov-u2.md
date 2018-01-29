---
title: Distribuera StorSimple 8000-serien enheten i Government portal | Microsoft Docs
description: "Beskriver stegen och bästa praxis för att distribuera StorSimple 8000-serien enheten med uppdatering 3 och senare och tjänsten Azure Government-portalen."
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
ms.openlocfilehash: 5a622eb5ae14a6c6b0c2dd4eceb6ffdb9733dcff
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-your-on-premises-storsimple-device-in-the-government-portal"></a>Distribuera din lokala StorSimple-enhet i Government-portal

## <a name="overview"></a>Översikt
Välkommen till distribution av Microsoft Azure StorSimple-enheten  Dessa självstudiekurser för distribution gäller StorSimple 8000-serien Kör uppdatering 3 eller senare i Azure Government-portalen. Denna serie självstudier innehåller en checklista för konfiguration, en lista över konfigurationskrav och detaljerade konfigurationssteg för din StorSimple-enhet.

Informationen i dessa självstudiekurser förutsätter att du har granskat säkerhetsåtgärder, packat upp, rackmonterat och kabelanslutit din StorSimple-enhet. Om du fortfarande inte har utförd ovanstående bör du börja med att vidta [säkerhetsåtgärderna](storsimple-safety.md). Packa upp, rackmontera och kabelanslut enheten genom att följa de specifika instruktionerna för enheten.

* [Packa upp, rackmontera och kabelanslut din 8100](storsimple-8100-hardware-installation.md)
* [Packa upp, rackmontera och kabelanslut din 8600](storsimple-8600-hardware-installation.md)

Du måste ha administratörsbehörighet för att slutföra installationen och konfigurationen. Vi rekommenderar att du läser checklistan för konfiguration innan du börjar. Processen för distribution och konfiguration kan ta lite tid att slutföra.

> [!NOTE]
> Distributionsinformationen för StorSimple som publiceras på webbplatsen Microsoft Azure gäller endast StorSimple-enheter i 8000-serien. Fullständig information om enheterna i 7000-serien finns i: [http://onlinehelp.storsimple.com/](http://onlinehelp.storsimple.com). Distributionsinformation för 7000-serien finns i [Snabbstartsguide för StorSimple-system](http://onlinehelp.storsimple.com/111_Appliance/).


## <a name="deployment-steps"></a>Distributionssteg
Utför dessa obligatoriska steg för att konfigurera StorSimple-enheten och ansluta den till StorSimple Device Manager-tjänsten. Förutom de obligatoriska stegen finns valfria steg och procedurer som du kan behöva utföra under distributionen. Steg för steg-instruktionerna för distribution anger när du ska utföra de olika valfria stegen.

| Steg | Beskrivning |
| --- | --- |
| **Förutsättningar** |De här instruktionerna måste slutföras som del av förberedelserna inför den distributionen. |
| [Checklista för distributionskonfiguration](#deployment-configuration-checklist) |Använd den här checklistan för att samla in och registrera information före och under distributionen. |
| [Förhandskrav för distribution](#deployment-prerequisites) |Dessa verifierar att miljön är klar för distribution. |
|  | |
| **Distribution steg för steg** |De här stegen måste utföras för att distribuera StorSimple-enheten i produktionen. |
| [Steg 1: Skapa en ny tjänst](#step-1-create-a-new-service) |Konfigurera hantering och lagring i molnet för StorSimple-enheten. *Hoppa över det här steget om du har en befintlig tjänst för andra StorSimple-enheter*. |
| [Steg 2: Hämta tjänstregistreringsnyckeln](#step-2-get-the-service-registration-key) |Du kan använda denna nyckel för att registrera och ansluta din StorSimple-enhet med hanteringstjänsten. |
| [Steg 3: Konfigurera och registrera enheten via Windows PowerShell för StorSimple](#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple) |Anslut enheten till nätverket och registrera den med Azure för att slutföra installationen med hjälp av hanteringstjänsten. |
| [Steg 4: Slutför den minimala Enhetsinstallationen](#step-4-complete-minimum-device-setup) </br>Valfritt: Uppdatera din StorSimple-enhet. |Använd hanteringstjänsten för att slutföra installationen av enheten och aktivera den för att tillhandahålla lagring. |
| [Steg 5: Skapa en volymbehållare](#step-5-create-a-volume-container) |Skapa en behållare för att etablera volymer. En volymbehållare har lagringskonto, bandbredd och krypteringsinställningar för de volymer som finns i den. |
| [Steg 6: Skapa en volym](#step-6-create-a-volume) |Etablera lagringsvolymer på StorSimple-enheten för dina servrar. |
| [Steg 7: Montera, initiera och formatera en volym](#step-7-mount-initialize-and-format-a-volume) </br>Valfritt: Konfigurera MPIO. |Anslut dina servrar till den iSCSI-lagring som ingår i enheten. Du kan också konfigurera MPIO för att säkerställa att dina servrar kan tolerera länkar, nätverk och fel. |
| [Steg 8: Ta en säkerhetskopia](#step-8-take-a-backup) |Konfigurera din säkerhetskopieringsprincip för att skydda dina data |
|  | |
| **Andra procedurer** |Du kan behöva använda de här procedurerna när du distribuerar din lösning. |
| [Konfigurera ett nytt lagringskonto för tjänsten](#configure-a-new-storage-account-for-the-service) | |
| [Använd PuTTY för att ansluta till enhetens seriekonsol](#use-putty-to-connect-to-the-device-serial-console) | |
| [Söka efter och installera uppdateringar](#scan-for-and-apply-updates) | |
| [Hämt IQN:en för en Windows Server-värd](#get-the-iqn-of-a-windows-server-host) | |
| [Skapa en manuell säkerhetskopia](#create-a-manual-backup) | |


## <a name="deployment-configuration-checklist"></a>Checklista för distributionskonfiguration
Innan du distribuerar din StorSimple-enhet behöver du samla in information om du vill konfigurera programvaran på din enhet. Processen för att distribuera StorSimple-enheten i din miljö blir effektivare om en del av den här informationen förbereds i förväg. Hämta och Använd den här checklistan för att notera konfigurationsinformationen när du distribuerar din enhet.

[Hämta konfigurationschecklistan för StorSimple-distributionen](http://www.microsoft.com/download/details.aspx?id=49159)

## <a name="deployment-prerequisites"></a>Distributionskrav
I följande avsnitt beskrivs konfigurationskraven för StorSimple Device Manager-tjänsten och StorSimple-enheten.

### <a name="for-the-storsimple-device-manager-service"></a>För StorSimple Device Manager-tjänsten
Innan du börjar bör du kontrollera att:

* Du har ditt Microsoft-konto med autentiseringsuppgifter.
* Du har ditt Microsoft Azure lagringskonto med autentiseringsuppgifter.
* Microsoft Azure-prenumerationen har aktiverats för StorSimple Device Manager-tjänsten. Din prenumeration ska köpas via [Enterprise-avtalet](https://azure.microsoft.com/pricing/enterprise-agreement/).
* Du har åtkomst till programvara för terminalemulering, till exempel PuTTY.

### <a name="for-the-device-in-the-datacenter"></a>För enheten i datacentret
Innan du konfigurerar enheten bör du kontrollera att:

* Din enhet är helt uppackad, rackmonterad och helt kabelansluten till ström, nätverk och serieåtkomst enligt beskrivningen i:
  
  * [Packa upp, rackmontera och kabelanslut din 8100-enhet](storsimple-8100-hardware-installation.md)
  * [Packa upp, rackmontera och kabelanslut din 8600-enhet](storsimple-8600-hardware-installation.md)

### <a name="for-the-network-in-the-datacenter"></a>För nätverket i datacentret
Innan du börjar ska du kontrollera att:

* Portarna i ditt datacenters brandvägg är öppna för att möjliggöra iSCSI- och molntrafik enligt beskrivningen i [Nätverkskrav för din StorSimple-enhet](storsimple-8000-system-requirements.md#networking-requirements-for-your-storsimple-device).

## <a name="step-by-step-deployment"></a>Steg för steg-distribution
Utför nedanstående steg för steg-instruktioner för att distribuera StorSimple-enheten i datacentret.

## <a name="step-1-create-a-new-service"></a>Steg 1: Skapa en ny tjänst
En StorSimple Device Manager-tjänst kan hantera flera StorSimple-enheter. Utför följande steg för att skapa en ny instans av StorSimple enheten Manager-tjänsten.

[!INCLUDE [storsimple-8000-create-new-service-gov](../../includes/storsimple-8000-create-new-service-gov.md)]

> [!IMPORTANT]
> Om du inte har aktiverat automatiskt skapande av lagringskonton med din tjänst måste du skapa minst ett lagringskonto efter att du har skapat en tjänst. Det här lagringskontot används när du skapar en volymbehållare.
> 
> * Om du inte har skapat ett lagringskonto automatiskt går du till [Konfigurera ett nytt lagringskonto för tjänsten](#configure-a-new-storage-account-for-the-service) för detaljerade anvisningar.
> * Om du har aktiverat automatiskt skapande av ett lagringskonto går du till [steg 2: hämta nyckel för tjänstregistrering](#step-2-get-the-service-registration-key).


## <a name="step-2-get-the-service-registration-key"></a>Steg 2: Hämta nyckel för tjänstregistrering
När StorSimple Device Manager-tjänsten är igång måste du hämta tjänstregistreringsnyckeln. Den här nyckeln används för att registrera och ansluta din StorSimple-enhet till tjänsten.

Utför följande steg i den offentliga-portalen.

[!INCLUDE [storsimple-8000-get-service-registration-key](../../includes/storsimple-8000-get-service-registration-key.md)]

## <a name="step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple"></a>Steg 3: Konfigurera och registrera enheten via Windows PowerShell för StorSimple
Använd Windows PowerShell för StorSimple för att slutföra den förberedande inställningen av StorSimple-enheten enligt nedanstående procedur. Du måste använda programvara för terminalemulering för att slutföra det här steget. Mer information finns i [Använda PuTTY för att ansluta till enhetens seriekonsol](#use-putty-to-connect-to-the-device-serial-console).

[!INCLUDE [storsimple-8000-configure-and-register-device-gov](../../includes/storsimple-8000-configure-and-register-device-gov-u2.md)]

## <a name="step-4-complete-minimum-device-setup"></a>Steg 4: Slutför de inställningar som krävs som minimum
Som minimikrav för konfigurationen av din StorSimple-enhet måste du:

* Ange ett eget namn för din enhet.
* Ange tidszonen för enheten.
* Tilldela båda domänkontrollanterna statiska IP-adresser.

Utför följande steg i Azure Government-portalen för att slutföra den minimala Enhetsinstallationen.

[!INCLUDE [storsimple-8000-complete-minimum-device-setup-u2](../../includes/storsimple-8000-complete-minimum-device-setup-u2.md)]

## <a name="step-5-create-a-volume-container"></a>Steg 5: Skapa en volymbehållare
En volymbehållare har lagringskonto, bandbredd och krypteringsinställningar för de volymer som finns i den. Du måste skapa en volymbehållare innan du kan börja etablera volymer på StorSimple-enheten.

Utför följande steg i den offentliga portalen för att skapa en volymbehållare.

[!INCLUDE [storsimple-8000-create-volume-container](../../includes/storsimple-8000-create-volume-container.md)]

## <a name="step-6-create-a-volume"></a>Steg 6: Skapa en volym
När du har skapat en volymbehållare kan du etablera en lagringsvolym på StorSimple-enheten för dina servrar. Utför följande steg i Government-portalen för att skapa en volym.

> [!IMPORTANT]
> StorSimple Enhetshanteraren kan skapa endast tunt allokerade volymer.  Du kan dock inte skapa delvis allokerade volymer.

[!INCLUDE [storsimple-8000-create-volume](../../includes/storsimple-8000-create-volume-u2.md)]

## <a name="step-7-mount-initialize-and-format-a-volume"></a>Steg 7: Montera, initiera och formatera en volym
Utför de här stegen på Windows Server-värd.

> [!IMPORTANT]
> * För att få hög tillgänglighet i StorSimple-lösningen rekommenderar vi att du konfigurerar MPIO på dina värdservrar (valfritt) innan du konfigurerar iSCSI. MPIO-konfiguration på värdservrar säkerställer att servrarna kan tolerera fel på en länk, ett nätverk eller gränssnitt.
> * Anvisningar för installation och konfiguration av MPIO och iSCSI på Windows Server-värden finns i [Konfigurera MPIO för din StorSimple-enhet](storsimple-configure-mpio-windows-server.md). Här finns även steg för att montera, initiera och formatera StorSimple-volymer.
> * Anvisningar för installation och konfiguration av MPIO och iSCSI på Linux-värdar finns i [Konfigurera MPIO för din StorSimple Linux-värd](storsimple-configure-mpio-on-linux.md)

Utför nedanstående steg för att montera, initiera och formatera dina StorSimple-volymer på en Windows Server-värd om du väljer att inte konfigurera MPIO.

[!INCLUDE [storsimple-mount-initialize-format-volume](../../includes/storsimple-mount-initialize-format-volume.md)]

## <a name="step-8-take-a-backup"></a>Steg 8: Gör en säkerhetskopia
Säkerhetskopieringar ger tidpunktsskydd för volymer och förbättrar återställningsmöjligheterna samtidigt som återställningstiderna minimeras. Du kan utföra två typer av säkerhetskopiering på StorSimple-enheten: lokala ögonblicksbilder och molnögonblicksbilder. Båda av de här säkerhetskopieringstyperna kan vara antingen **schemalagda** eller **manuella**.

Utför följande steg i den offentliga portalen för att skapa en schemalagd säkerhetskopiering.

[!INCLUDE [storsimple-8000-take-backup](../../includes/storsimple-8000-take-backup.md)]

Du kan utföra en manuell säkerhetskopiering när som helst. Gå till [Skapa en manuell säkerhetskopiering](#create-a-manual-backup) för anvisningar.

## <a name="configure-a-new-storage-account-for-the-service"></a>Konfigurera ett nytt lagringskonto för tjänsten
Det här är ett valfritt steg som du endast måste utföra om du inte har aktiverat automatiskt skapande av lagringskonton med din tjänst. Ett Microsoft Azure-lagringskonto krävs för att skapa en behållare för StorSimple-volymer.

Om du behöver skapa ett Azure-lagringskonto i en annan region finns stegvisa instruktioner i [Om Azure lagringskonton](../storage/common/storage-create-storage-account.md).

Utför följande steg i Government-portal på den **StorSimple Enhetshanteraren service** sidan.

[!INCLUDE [storsimple-configure-new-storage-account-u1](../../includes/storsimple-8000-configure-new-storage-account-u2.md)]

## <a name="use-putty-to-connect-to-the-device-serial-console"></a>Använd PuTTY för att ansluta till enhetens seriekonsol
För att ansluta till Windows PowerShell för StorSimple behövs en programvara för terminalemulering, till exempel PuTTY. Du kan använda PuTTY när åtkomst till enheten sker direkt via seriekonsolen eller genom att öppna en Telnet-session från en fjärrdator.

[!INCLUDE [Use PuTTY to connect to the device serial console](../../includes/storsimple-use-putty.md)]

## <a name="scan-for-and-apply-updates"></a>Söka efter och installera uppdateringar
Det kan ta flera timmar att uppdatera din enhet. Detaljerade anvisningar om hur du installerar den senaste uppdateringen finns i [Installera Uppdatering 4](storsimple-8000-install-update-4.md).

## <a name="get-the-iqn-of-a-windows-server-host"></a>Hämta IQN för en Windows Server-värd
Utför stegen nedan för att få det kvalificerade iSCSI-namnet (IQN) för en Windows-värd som kör Windows Server® 2012.

[!INCLUDE [Get IQN of your Windows Server host](../../includes/storsimple-get-iqn.md)]

## <a name="create-a-manual-backup"></a>Skapa en manuell säkerhetskopia
Utför följande steg i Government-portalen för att skapa en manuell säkerhetskopiering på begäran för en enskild volym på din StorSimple-enhet.

[!INCLUDE [Create a manual backup](../../includes/storsimple-8000-create-manual-backup.md)]

## <a name="next-steps"></a>Nästa steg
* Konfigurera en [virtuell enhet](storsimple-8000-cloud-appliance-u2.md).
* Hantera StorSimple-enheten med hjälp av [StorSimple Device Manager-tjänsten](storsimple-8000-manager-service-administration.md).


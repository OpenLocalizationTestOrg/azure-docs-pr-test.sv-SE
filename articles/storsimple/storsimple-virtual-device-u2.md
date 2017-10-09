---
title: aaaStorSimple virtuell enhet uppdatering 2 | Microsoft Docs
description: "Lär dig hur toocreate, distribuera och hantera en virtuell StorSimple-enhet i ett virtuellt nätverk i Microsoft Azure. (Gäller tooStorSimple uppdatering 2)."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: f37752a5-cd0c-479b-bef2-ac2c724bcc37
ms.service: storsimple
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/07/2017
ms.author: alkohli
ms.openlocfilehash: 8d2a0520f1ed30ebec929c2bdabb4838691b8ad6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-a-storsimple-virtual-device-in-azure"></a>Distribuera och hantera en virtuell StorSimple-enhet i Azure
## <a name="overview"></a>Översikt
Hej StorSimple 8000-serien virtuell enhet är en ytterligare funktion som ingår i din Microsoft Azure StorSimple-lösning. hello virtuella StorSimple-enheten körs på en virtuell dator i ett virtuellt nätverk i Microsoft Azure och du kan använda den tooback upp och klona data från värdar. Den här självstudiekursen beskriver hur toodeploy och hantera en virtuell enhet i Azure och tillämpliga tooall hello virtuella enheter som kör programvaruversion uppdatering 2 och lägre.

#### <a name="virtual-device-model-comparison"></a>Jämförelse av virtuella enhetsmodeller
Hej StorSimple virtuell enhet är tillgänglig i två modeller, en standardmodell, 8010 (kallades tidigare hello 1100), och premiummodellen 8020 (introducerad i uppdatering 2). En jämförelse av hello två modellerna visas i tabellen nedan.

| Enhetsmodell | 8010<sup>1</sup> | 8020 |
| --- | --- | --- |
| **Maximal kapacitet** |30 TB |64 TB |
| **Virtuell Azure-dator** |Standard_A3 (4 kärnor, 7 GB minne) |Standard_DS3 (4 kärnor, 14 GB minne) |
| **Versionskompatibilitet** |Versioner som körs före Uppdatering 2 eller senare |Versioner som körs med Uppdatering 2 eller senare |
| **Regional tillgänglighet** |Alla Azure-regioner |Alla Azure-regioner som har stöd för Premium Storage och DS3 Azure VM<br></br> Använd [listan](https://azure.microsoft.com/en-us/regions/services) toosee om båda *virtuella datorer > DS-serien* och *lagring > disklagring* är tillgängliga i din region. |
| **Lagringstyp** |Använder Azure Standardlagring för lokala diskar<br></br> Lär dig hur för[skapar ett standardlagringskonto](../storage/common/storage-create-storage-account.md) |Använder Azure Premium Storage för lokala diskar<sup>2</sup> <br></br>Lär dig hur för[skapar en Premium Storage-konto](../storage/common/storage-premium-storage.md) |
| **Riktlinjer för arbetsbelastning** |Hämtning av filer från säkerhetskopior på objektnivå |Scenarier för utveckling och test av molnet, låg latens, arbetsbelastningar med hög prestanda <br></br>Sekundär enhet för katastrofåterställning |

<sup>1</sup> *kallades hello 1100*.

<sup>2</sup> *både hello 8010 och 8020 använder Azure standardlagring för hello molnet nivån. hello skillnaden finns endast på hello lokala nivån i hello enheten*.

Den här artikeln beskriver hello steg för steg för att distribuera en virtuell StorSimple-enhet i Azure. När du har läst den här artikeln, kommer du att:

* Förstå hur hello virtuella enheten skiljer sig från hello fysisk enhet.
* Vara kan toocreate och konfigurera hello virtuella enheten.
* Ansluta toohello virtuell enhet.
* Lär dig hur toowork med hello virtuella enheten.

Den här kursen gäller tooall hello virtuella StorSimple-enheter med uppdatering 2 och högre.

## <a name="how-hello-virtual-device-differs-from-hello-physical-device"></a>Hur hello virtuella enheten skiljer sig från hello fysisk enhet
hello virtuella StorSimple-enheten är en programvarubaserad version av StorSimple som körs på en nod i en Microsoft Azure-dator. hello virtuella enheten stöds disaster recovery scenarier där den fysiska enheten finns inte och är lämplig för användning i objektsnivå från säkerhetskopior, katastrofåterställning, lokal och molnet dev och testa scenarier.

#### <a name="differences-from-hello-physical-device"></a>Skillnader från hello fysisk enhet
hello visas följande tabell några huvudsakliga skillnader mellan hello virtuella StorSimple-enheten och fysiska hello StorSimple-enheten.

|  | Fysisk enhet | Virtuell enhet |
| --- | --- | --- |
| **Plats** |Finns i hello datacenter. |Körs i Azure. |
| **Nätverksgränssnitt** |Har sex nätverksgränssnitt: DATA 0 till DATA 5. |Har bara ett nätverksgränssnitt: DATA 0. |
| **Registrering** |Registrering under konfigurationssteget hello. |Registreringen är en separat åtgärd. |
| **Krypteringsnyckel för tjänstdata** |Återskapa på hello fysisk enhet och uppdatera sedan hello virtuella enheten med hello ny nyckel. |Det går inte att återskapa från hello virtuell enhet. |

## <a name="prerequisites-for-hello-virtual-device"></a>Krav för hello virtuell enhet
hello beskrivs följande avsnitt kraven för din virtuella StorSimple-enhet hello-konfiguration. Tidigare toodeploying en virtuell enhet granska hello [säkerhetsöverväganden vid användning av en virtuell enhet](storsimple-security.md#storsimple-virtual-device-security).

#### <a name="azure-requirements"></a>Krav för Azure
Innan du etablerar hello virtuell enhet behöver du toomake hello följande förberedelser i Azure-miljön:

* För hello virtuell enhet, [konfigurera ett virtuellt nätverk i Azure](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Om du använder Premiumlagring, måste du skapa ett virtuellt nätverk i en Azure-region som har stöd för Premiumlagring. hello Premium storage-regioner är regioner som motsvarar toohello rad för *disklagring* i hello lista över [Azure-tjänster efter Region](https://azure.microsoft.com/en-us/regions/services).
* Det är lämpligt toouse hello standard DNS-servern från Azure istället för att ange dina egna DNS-servernamnet. Om DNS-servernamnet inte är giltig eller om hello DNS-servern inte är korrekt kan tooresolve IP-adresser, misslyckas hello skapandet av hello virtuell enhet.
* Punkt-till-plats och plats-till-plats går att välja, men är inget krav. Om du vill kan du konfigurera dessa alternativ för mer avancerade scenarier.
* Du kan skapa [Azure Virtual Machines](../virtual-machines/virtual-machines-linux-about.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (värdservrar) i hello virtuella nätverk som kan använda hello volymer som exponeras av hello virtuell enhet. Servrarna måste uppfylla följande krav hello:                             

  * Vara virtuella Windows eller Linux-datorer som har installerad programvara med iSCSI-initierare.
  * Körs i hello samma virtuella nätverk som hello virtuell enhet.
  * Vara kan tooconnect toohello iSCSI-målet på hello virtuella enheten via hello interna IP-adress hello virtuell enhet.
* Kontrollera att du har konfigurerat stöd för iSCSI- och trafik på hello samma virtuella nätverk.

#### <a name="storsimple-requirements"></a>Krav för StorSimple
Att Hej följande uppdateringar tooyour Azure StorSimple-tjänsten innan du skapar en virtuell enhet:

* Lägg till [åtkomstkontrollposter](storsimple-manage-acrs.md) för hello virtuella datorer som ska toobe värdservrar för din virtuella enhet.
* Använd en [lagringskonto](storsimple-manage-storage-accounts.md#add-a-storage-account) i hello samma region som hello virtuell enhet. Lagringskonton i olika regioner kan resultera i sämre prestanda. Du kan använda ett konto som Standard eller Premium-lagring med hello virtuella enheten. Mer information om hur toocreate en [standardlagringskonto](../storage/common/storage-create-storage-account.md) eller en [Premium Storage-konto](../storage/common/storage-premium-storage.md)
* Använd ett annat lagringskonto för att skapa en virtuell enhet från hello som används för dina data. Med hjälp av hello samma lagringskonto kan resultera i sämre prestanda.

Kontrollera att du har hello följande information innan du börjar:

* Ditt klassiska Azure-portalkonto med autentiseringsuppgifter.
* En kopia av hello krypteringsnyckel för tjänstdata från den fysiska enheten.

## <a name="create-and-configure-hello-virtual-device"></a>Skapa och konfigurera hello virtuell enhet
Innan du utför de här procedurerna bör du kontrollera att du har uppfyllt hello [krav för hello virtuella enheten](#prerequisites-for-the-virtual-device).

När du har skapat ett virtuellt nätverk, konfigurerat en StorSimple Manager-tjänst och registrerat din fysiska StorSimple-enhet med hello-tjänsten, kan du använda följande steg toocreate hello och konfigurera en virtuell StorSimple-enhet.

### <a name="step-1-create-a-virtual-device"></a>Steg 1: Skapa en virtuell enhet
Utföra hello följande steg toocreate hello virtuella StorSimple-enheten.

[!INCLUDE [Create a virtual device](../../includes/storsimple-create-virtual-device-u2.md)]

Om hello av hello virtuella enheten misslyckas i det här steget, kan du inte har anslutning toohello Internet. Mer information finns för[Felsök anslutningsfel till Internet](#troubleshoot-internet-connectivity-errors) när du skapar en virtuell enhet.

### <a name="step-2-configure-and-register-hello-virtual-device"></a>Steg 2: Konfigurera och registrera hello virtuell enhet
Innan du börjar den här proceduren måste du kontrollera att du har en kopia av krypteringsnyckeln för tjänstdata hello. hello krypteringsnyckel för tjänstdata skapades när du konfigurerade din första StorSimple-enhet och du har fått instruktioner toosave den på en säker plats. Om du inte har en kopia av krypteringsnyckeln för tjänstdata hello måste du kontakta Microsoft Support för hjälp.

Utför följande steg tooconfigure hello och registrera din virtuella StorSimple-enhet.

[!INCLUDE [Configure and register a virtual device](../../includes/storsimple-configure-register-virtual-device.md)]

### <a name="step-3-optional-modify-hello-device-configuration-settings"></a>Steg 3: (Valfritt) Ändra hello konfigurationsinställningar
hello beskrivs följande avsnitt hello konfigurationsinställningar som behövs för hello virtuell StorSimple-enhet om du vill toouse CHAP, StorSimple Snapshot Manager eller ändra hello administratörslösenordet för enheten.

#### <a name="configure-hello-chap-initiator"></a>Konfigurera hello CHAP-initieraren
Den här parametern innehåller hello-autentiseringsuppgifter som din virtuella enhet (målet) förväntar sig från hello initierarna (servrarna) som försöker tooaccess hello volymer. hello initierarna skapar ett CHAP-användarnamn och ett CHAP-lösenord tooidentify själva tooyour enheten under den här autentiseringen. Detaljerade anvisningar finns för[konfigurera CHAP för din enhet](storsimple-configure-chap.md#unidirectional-or-one-way-authentication).

#### <a name="configure-hello-chap-target"></a>Konfigurera hello CHAP-målet
Den här parametern innehåller hello-autentiseringsuppgifter som din virtuella enhet använder när en CHAP-aktiverad initierare begär ömsesidig eller dubbelriktad autentisering. Din virtuella enhet använder användarnamn omvänd CHAP och omvänd CHAP lösenord tooidentify själva toohello initieraren under den här autentiseringsprocessen. Observera att inställningarna för CHAP-målet är globala inställningar. När dessa tillämpas använder alla hello volymer anslutna toohello virtuella lagringsenheten CHAP-autentisering. Detaljerade anvisningar finns för[konfigurera CHAP för din enhet](storsimple-configure-chap.md#bidirectional-or-mutual-authentication).

#### <a name="configure-hello-storsimple-snapshot-manager-password"></a>Konfigurera hello lösenordet för StorSimple Snapshot Manager
Programvaran StorSimple Manager finns på din Windows-värd och kan administratörer toomanage säkerhetskopior av StorSimple-enheten i hello form av lokala och molnögonblicksbilder.

> [!NOTE]
> Hello virtuell enhet är Windows-värd en virtuell Azure-dator.
>
>

När du konfigurerar en enhet i hello StorSimple Snapshot Manager uppmanas du att tooprovide hello StorSimple enheten IP-adress och lösenord tooauthenticate lagringsenheten. Detaljerade anvisningar finns för[lösenordet konfigurera StorSimple Snapshot Manager](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password).

#### <a name="change-hello-device-administrator-password"></a>Ändra hello enhetens administratörslösenord
När du använder hello Windows PowerShell-gränssnittet tooaccess Hej virtuell enhet, kommer du att nödvändiga tooenter administratörslösenord för enheten. Hello säkerheten för dina data, du är obligatoriska toochange lösenordet innan hello virtuella enheten kan användas. Detaljerade anvisningar finns för[konfigurera enhetens administratörslösenord](storsimple-change-passwords.md#change-the-device-administrator-password).

## <a name="connect-remotely-toohello-virtual-device"></a>Fjärransluta toohello virtuell enhet
Fjärråtkomst tooyour virtuella enheten via hello Windows PowerShell-gränssnittet är inte aktiverad som standard. Du behöver tooenable fjärrhantering på hello virtuella enheten först och sedan aktivera det på hello-klient som kommer att använda tooaccess din virtuella enhet.

Hej tvåstegsverifiering processen tooconnect beskrivs fjärransluta nedan.

### <a name="step-1-configure-remote-management"></a>Steg 1: Konfigurera fjärrhantering
Utföra hello följande steg tooconfigure fjärrhantering för din virtuella StorSimple-enhet.

[!INCLUDE [Configure remote management via HTTP for virtual device](../../includes/storsimple-configure-remote-management-http-device.md)]

### <a name="step-2-remotely-access-hello-virtual-device"></a>Steg 2: Fjärråtkomst till hello virtuell enhet
När du har aktiverat fjärrhantering på konfigurationssidan för hello StorSimple-enhet, kan du använda Windows PowerShell-fjärrkommunikation tooconnect toohello virtuella enheten från en annan virtuell dator i hello samma virtuella nätverk; Du kan till exempel ansluta från hello värden VM att du konfigurerat och använt tooconnect iSCSI. I de flesta distributioner kommer du redan har öppnat en offentlig slutpunkt tooaccess din Virtuella värddator, som du kan använda för att komma åt hello virtuell enhet.

> [!WARNING]
> **Förbättrad säkerhet rekommenderar vi att du använder HTTPS när du ansluter toohello slutpunkter och ta sedan bort hello slutpunkter när du har slutfört din PowerShell-fjärrsession.**
>
>

Du bör följa hello procedurerna i [tooyour StorSimple-enhet för fjärranslutning](storsimple-remote-connect.md) tooset in fjärrstyrning för din virtuella enhet.

## <a name="connect-directly-toohello-virtual-device"></a>Anslut direkt toohello virtuell enhet
Du kan också ansluta direkt toohello virtuell enhet. Om du vill tooconnect direkt toohello virtuella enheten från en annan dator utanför hello virtuellt nätverk eller utanför hello Microsoft Azure-miljön, måste toocreate ytterligare slutpunkter som beskrivs i hello nedan.

Utföra hello följande steg toocreate en offentlig slutpunkt på hello virtuell enhet.

[!INCLUDE [Create public endpoints on a virtual device](../../includes/storsimple-create-public-endpoints-virtual-device.md)]

Vi rekommenderar att du ansluter från en annan virtuell dator i hello samma virtuella nätverket eftersom den här metoden minimerar hello antalet offentliga slutpunkter i ditt virtuella nätverk. När du använder den här metoden kan du enkelt ansluta toohello virtuella datorn via en fjärrskrivbordssession och sedan konfigurera den virtuella datorn för användning som alla andra Windows-klienter i ett lokalt nätverk. Du behöver inte tooappend hello offentliga portnumret eftersom hello port redan är känt.

## <a name="work-with-hello-storsimple-virtual-device"></a>Arbeta med hello virtuell StorSimple-enhet
Nu när du har skapat och konfigurerat hello virtuella StorSimple-enheten, är klar toostart arbeta med den. Du kan arbeta med volymbehållare, volymer och principer för säkerhetskopiering på en virtuell enhet precis som på en fysisk enhet; hello enda skillnaden är att du måste du markera hello virtuella enheten från din enhetslista toomake. Se för[använda hello StorSimple Manager-tjänsten toomanage en virtuell enhet](storsimple-manager-service-administration.md) stegvisa anvisningar för hello olika administrationsuppgifter för hello virtuella enheten.

hello följande avsnitt beskrivs några av hello skillnaderna du kommer märka när du arbetar med hello virtuell enhet.

### <a name="maintain-a-storsimple-virtual-device"></a>Underhålla en virtuell StorSimple-enhet
Eftersom det är en programvarubaserad enhet, underhåll för hello virtuell enhet är minimal när jämfört med toomaintenance för hello fysisk enhet. Du har hello följande alternativ:

* **Programuppdateringar** – du kan visa hello datum då hello programmet senast uppdaterades, tillsammans med eventuella statusmeddelanden för uppdateringen. Du kan använda hello **skanna uppdateringar** knappen längst ned hello hello sidan tooperform en manuell genomsökning om du vill toocheck efter nya uppdateringar. Om det finns uppdateringar, klickar du på **installera uppdateringar** tooinstall. Eftersom det finns bara ett enda gränssnitt på hello virtuell enhet, innebär det att en viss driftstörning uppstår när uppdateringar tillämpas. hello virtuella enheten avslutas och startas om (vid behov) tooapply alla uppdateringar som har släppts. Stegvisa anvisningar finns för[uppdatera enheten](storsimple-update-device.md#install-regular-updates-via-the-azure-classic-portal).
* **Stödpaket** – du kan skapa och ladda upp en stöd paketet toohelp Microsoft-supporten felsöka problem med din virtuella enhet. Stegvisa anvisningar finns för[skapa och hantera ett stödpaket](storsimple-create-manage-support-package.md).

### <a name="storage-accounts-for-a-virtual-device"></a>Lagringskonton för en virtuell enhet
Lagringskonton skapas för användning av hello StorSimple Manager-tjänsten, hello virtuella enheten och hello fysisk enhet. När du skapar dina lagringskonton rekommenderar vi att du använder en region identifierare i hello eget namn toohelp Kontrollera hello regionen är konsekvent på alla hello systemkomponenter. Det är viktigt att hello alla hello komponenter vara i samma region tooprevent prestandaproblem för en virtuell enhet.

Stegvisa anvisningar finns för[Lägg till ett lagringskonto](storsimple-manage-storage-accounts.md#add-a-storage-account).

### <a name="deactivate-a-storsimple-virtual-device"></a>Inaktivera en virtuell StorSimple-enhet
Inaktivera en virtuell enhet tar bort hello VM och hello resurser som skapades när den etablerades. När hello virtuella enheten inaktiveras kan den inte återställas tooits tidigare tillstånd. Innan du inaktiverar hello virtuell enhet gör att toostop eller ta bort klienter och värdar som är beroende av den.

Inaktivering av virtuella enheter resulterar i hello följande åtgärder:

* hello virtuella enheten tas bort.
* hello OS-disk- och datadiskar som skapats för hello virtuella enheten tas bort.
* hello värdbaserade tjänsten och virtuella nätverk som skapades under etableringen bevaras. Om du inte använder dem, bör du ta bort dem manuellt.
* Molnögonblicksbilder som skapats för hello virtuella enheten bevaras.

Stegvisa anvisningar finns för[inaktivera och ta bort din StorSimple-enhet](storsimple-deactivate-and-delete-device.md).

Så snart hello virtuella enheten visas som inaktiverad på tjänstsidan för hello StorSimple Manager, du kan ta bort hello virtuella enheten från listan över enheter på hello **enheter** sidan.

### <a name="start-stop-and-restart-a-virtual-device"></a>Starta, stoppa och starta om en virtuell enhet
Till skillnad från hello fysiska StorSimple-enheten finns det ingen ström på eller power av knappen toopush på en virtuell StorSimple-enhet. Det kan dock finnas tillfällen där du behöver toostop och starta om hello virtuell enhet. Vissa uppdateringar kan till exempel kräva att hello virtuella datorn startas om toofinish hello uppdateringsprocessen. Hej enklast för du toostart, stoppa och starta om en virtuell enhet toouse hello konsolen för hantering av virtuella datorer.

När du tittar på hello hanteringskonsolen hello virtuella enhetens status är **kör** eftersom den startas som standard när den har skapats. Du kan starta, stoppa och starta om en virtuell dator när som helst.

[!INCLUDE [Stop and restart virtual device](../../includes/storsimple-stop-restart-virtual-device.md)]

### <a name="reset-toofactory-defaults"></a>Återställ toofactory inställningar
Om du väljer att du bara vill toostart över med din virtuella enhet, inaktivera och ta bort den och skapa en ny. Precis som när den fysiska enheten har återställts, kommer den nya virtuella enheten inte har installerat; uppdateringar därför se till att toocheck för uppdateringar innan du använder den.

## <a name="fail-over-toohello-virtual-device"></a>Växla över toohello virtuell enhet
Katastrofåterställning (DR) är en av hello viktiga scenarier som hello StorSimple-enheten har designats för. I det här scenariot hello fysiska StorSimple-enheten eller hela datacentret kanske inte är tillgänglig. Lyckligtvis kan du använda en virtuell enhet toorestore åtgärder i en annan plats. Under Katastrofåterställning hello volymbehållarna från hello källenheten ändra ägarskap och är överförda toohello virtuell enhet. hello kraven för Katastrofåterställning är att hello virtuella enheten har skapats och konfigurerats, alla hello volymer inom hello volymbehållare har varit offline och hello volymbehållare har en associerad ögonblicksbild i molnet.

> [!NOTE]
> * När du använder en virtuell enhet som hello sekundär enhet för Katastrofåterställning, Kom ihåg att hello 8010 har 30 TB standardlagring och 8020 har 64 TB premiumlagring. hello högre kapacitet 8020 virtuella enheten kan vara mer lämpad för katastrofåterställning.
> * Du kan inte växla eller klona från en enhet som kör uppdatering 2 tooa enheter före uppdateringen 1 programvaran som körs. Du kan dock växla en enhet som kör uppdatering 2 tooa enhet som kör uppdatering 1 (1.1 eller 1.2)
>
>

Stegvisa anvisningar finns för[redundans tooa virtuella enheten](storsimple-device-failover-disaster-recovery.md#fail-over-to-a-storsimple-virtual-device).

## <a name="shut-down-or-delete-hello-virtual-device"></a>Stänga av eller ta bort hello virtuella enheten
Om du tidigare har konfigurerat och använt en virtuell StorSimple enhet men nu vill toostop debiteringarna för dess användning, du kan stänga av hello virtuell enhet. Stänger av hello virtuell enhet tas inte bort operativsystem eller lagringar i datadiskar. Det stoppa debiteringen för din prenumeration, men lagringskostnaderna för hello OS- och datadiskar som kommer att fortsätta.

Om du tar bort eller stänga av hello virtuella enheten visas den som **Offline** på hello enheter-sidan i hello StorSimple Manager-tjänsten. Du kan välja toodeactivate eller ta bort hello enheten om du även vill toodelete hello säkerhetskopior som har skapats av hello virtuella enheten. Mer information finns i [Inaktivera och ta bort en StorSimple-enhet](storsimple-deactivate-and-delete-device.md).

[!INCLUDE [Shut down a virtual device](../../includes/storsimple-shutdown-virtual-device.md)]

[!INCLUDE [Delete a virtual device](../../includes/storsimple-delete-virtual-device.md)]

## <a name="troubleshoot-internet-connectivity-errors"></a>Felsöka problem med Internetanslutning
Under hello skapandet av en virtuell enhet, om det finns ingen anslutning toohello Internet, att hello skapa steg misslyckas. tootroubleshoot om hello misslyckades på grund av anslutning till Internet, utför hello följa stegen i hello klassiska Azure-portalen:

1. Skapa en virtuell dator med Windows Server 2012 i Azure. Den här virtuella datorn ska använda hello samma lagringskonto, VNet och undernät som används av din virtuella enhet. Om du redan har en befintlig Windows Server-värd i Azure med hjälp av Hej samma lagringskonto, Vnet och undernät, du kan också använda den tootroubleshoot hello Internet-anslutning.
2. Fjärråtkomst logga in på hello virtuell dator som skapats i hello föregående steg.
3. Öppna ett kommandofönster inuti hello virtuella datorn (Win + R och skriv sedan `cmd`).
4. Kör följande kommando i Kommandotolken hello hello.

    `nslookup windows.net`
5. Om `nslookup` misslyckas, och sedan Internet-anslutningsfel förhindrar hello virtuella enheten registreringen toohello StorSimple Manager-tjänsten.
6. Ändra hello krävs tooyour virtuellt nätverk tooensure som hello virtuell enhet är kan tooaccess Azure platser, till exempel ”windows.net”.

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[använda hello StorSimple Manager-tjänsten toomanage en virtuell enhet](storsimple-manager-service-administration.md).
* Förstå hur för[återställer en StorSimple-volym från en säkerhetskopia](storsimple-restore-from-backup-set.md).

---
title: aaaStorSimple moln enhet uppdatering 3 | Microsoft Docs
description: "Lär dig hur toocreate, distribuera och hantera en enhet med StorSimple molntjänster i ett virtuellt nätverk i Microsoft Azure. (Gäller tooStorSimple uppdatering 3 och senare)."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/10/2017
ms.author: alkohli
ms.openlocfilehash: ba60a629f1f4b8f0d4566eeb45bae8696f50d0af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-a-storsimple-cloud-appliance-in-azure-update-3-and-later"></a>Distribuera och hantera en StorSimple Cloud Appliance-installation i Azure (Uppdatering 3 eller senare)

## <a name="overview"></a>Översikt

Hej StorSimple 8000-serien molnet installation är en ytterligare funktion som ingår i din Microsoft Azure StorSimple-lösning. hello StorSimple moln installation körs på en virtuell dator i ett virtuellt nätverk i Microsoft Azure och du kan använda den tooback upp och klona data från värdar.

Den här artikeln beskriver hello stegvis processen toodeploy och hantera en enhet med StorSimple moln i Azure. När du har läst den här artikeln, kommer du att:

* Förstå hur hello molnet installation skiljer sig från hello fysisk enhet.
* Att kunna toocreate och konfigurera hello molnet installation.
* Ansluta toohello moln installation.
* Lär dig hur toowork med hello cloud installation.

Den här självstudien gäller tooall hello StorSimple moln installationer med uppdatering 3 och senare.

#### <a name="cloud-appliance-model-comparison"></a>Jämförelse av molninstallationsmodeller

Hej StorSimple moln installation finns i två modeller, en standardmodell, 8010 (kallades tidigare hello 1100), och premiummodellen 8020 (introducerad i uppdatering 2). hello följande tabell visar en jämförelse av hello två modeller.

| Enhetsmodell | 8010<sup>1</sup> | 8020 |
| --- | --- | --- |
| **Maximal kapacitet** |30 TB |64 TB |
| **Virtuell Azure-dator** |Standard_A3 (4 kärnor, 7 GB minne)| Standard_DS3 (4 kärnor, 14 GB minne)|
| **Regional tillgänglighet** |Alla Azure-regioner |Azure-regioner som har stöd för Premium Storage och virtuella Azure-datorer, DS3<br></br>Använd [listan](https://azure.microsoft.com/regions/services/) toosee om båda **virtuella datorer > DS-serien** och **lagring > disklagring** är tillgängliga i din region. |
| **Lagringstyp** |Använder Azure Standardlagring för lokala diskar<br></br> Lär dig hur för[skapar ett standardlagringskonto](../storage/common/storage-create-storage-account.md) |Använder Azure Premium Storage för lokala diskar<sup>2</sup> <br></br>Lär dig hur för[skapar en Premium Storage-konto](../storage/common/storage-premium-storage.md) |
| **Riktlinjer för arbetsbelastning** |Hämtning av filer från säkerhetskopior på objektnivå |Utvecklings- och testscenarier i molnet <br></br>Arbetsbelastningar med kortare svarstider och högre prestanda<br></br>Sekundär enhet för katastrofåterställning |

<sup>1</sup> *kallades hello 1100*.

<sup>2</sup> *både hello 8010 och 8020 använder Azure standardlagring för hello molnet nivån. hello skillnaden finns endast på hello lokala nivån i hello enheten*.

## <a name="how-hello-cloud-appliance-differs-from-hello-physical-device"></a>Hur hello molnet installation skiljer sig från hello fysisk enhet

Hej StorSimple moln installation är en programvarubaserad version av StorSimple som körs på en nod i en Microsoft Azure-dator. hello molnet enhet stöder disaster recovery scenarier där den fysiska enheten inte är tillgänglig. hello molnet installation är lämplig för användning i objektsnivå från säkerhetskopior, lokal katastrofåterställning och scenarier för utveckling och testning.

#### <a name="differences-from-hello-physical-device"></a>Skillnader från hello fysisk enhet

hello visas följande tabell några huvudsakliga skillnader mellan hello StorSimple-enhet för molnet och fysiska hello StorSimple-enheten.

|  | Fysisk enhet | Molninstallation |
| --- | --- | --- |
| **Plats** |Finns i hello datacenter. |Körs i Azure. |
| **Nätverksgränssnitt** |Har sex nätverksgränssnitt: DATA 0 till DATA 5. |Har bara ett nätverksgränssnitt: DATA 0. |
| **Registrering** |Registrerad under hello inledande konfigurationssteg. |Registreringen är en separat åtgärd. |
| **Krypteringsnyckel för tjänstdata** |Återskapa på hello fysisk enhet och uppdatera sedan hello molnet installation med hello ny nyckel. |Det går inte att återskapa från hello molnet installation. |
| **Volymtyper som stöds** |Har stöd både för lokalt fixerade och nivåindelade volymer. |Har stöd endast för nivåindelade volymer. |

## <a name="prerequisites-for-hello-cloud-appliance"></a>Krav för hello för molnet

hello följande avsnitt beskrivs kraven för din StorSimple-enhet för molnet hello-konfiguration. Innan du distribuerar en moln-installation, granska hello säkerhetsöverväganden vid användning av en moln-installation.

[!INCLUDE [StorSimple Cloud Appliance security](../../includes/storsimple-8000-cloud-appliance-security.md)]

#### <a name="azure-requirements"></a>Krav för Azure

Innan du etablerar hello moln-enhet behöver du toomake hello följande förberedelser i Azure-miljön:

* Se till att du har en fysisk StorSimple 8000 series-enhet (modell 8100 eller 8600) distribuerad och körandes i ditt datacenter. Registrera enheten med hello samma Enhetshanteraren för StorSimple-tjänst som du avser toocreate en StorSimple molnet program för.
* För för hello molnet, [konfigurera ett virtuellt nätverk i Azure](../virtual-network/virtual-networks-create-vnet-arm-pportal.md). Om du använder Premiumlagring, måste du skapa ett virtuellt nätverk i en Azure-region som har stöd för Premiumlagring. hello Premium Storage-regioner är regioner som motsvarar toohello rad för disklagring i hello [lista över tjänster per Region](https://azure.microsoft.com/regions/services/).
* Vi rekommenderar att du använder hello standard DNS-servern från Azure istället för att ange dina egna DNS-servernamnet. Om DNS-servernamnet inte är giltig eller om hello DNS-server inte kan tooresolve IP-adresser korrekt misslyckas hello hello molnet enhetens.
* Punkt-till-plats och plats-till-plats går att välja, men är inget krav. Om du vill kan du konfigurera dessa alternativ för mer avancerade scenarier.
* Du kan skapa [Azure Virtual Machines](../virtual-machines/virtual-machines-windows-quick-create-portal.md) (värdservrar) i hello virtuella nätverk som kan använda hello volymer som exponeras av hello molnet installation. Servrarna måste uppfylla följande krav hello:

  * Vara virtuella Windows eller Linux-datorer som har installerad programvara med iSCSI-initierare.
  * Körs i hello samma virtuella nätverk som hello molnet utrustning.
  * Vara kan tooconnect toohello iSCSI-målet hello molnet enhetens via hello interna IP-adress hello molnet installation.
  * Kontrollera att du har konfigurerat stöd för iSCSI- och trafik på hello samma virtuella nätverk.

#### <a name="storsimple-requirements"></a>Krav för StorSimple

Att Hej följande uppdateringar tooyour StorSimple enheten Manager-tjänsten innan du skapar en moln-installation:

* Lägg till [åtkomstkontrollposter](storsimple-8000-manage-acrs.md) för hello virtuella datorer som är pågående toobe hello värdservrar för din enhet i molnet.
* Använd en [lagringskonto](storsimple-8000-manage-storage-accounts.md#add-a-storage-account) i hello samma region som hello molnet utrustning. Lagringskonton i olika regioner kan resultera i sämre prestanda. Du kan använda ett konto som Standard eller Premium-lagring med hello molnet installation. Mer information om hur toocreate en [standardlagringskonto](../storage/common/storage-create-storage-account.md) eller en [Premium Storage-konto](../storage/common/storage-premium-storage.md)
* Använd ett annat lagringskonto för att skapa en moln-enhet från hello som används för dina data. Med hjälp av hello samma lagringskonto kan resultera i sämre prestanda.

Kontrollera att du har hello följande information innan du börjar:

* Ditt Azure Portal-konto med autentiseringsuppgifter.
* En kopia av hello krypteringsnyckel för tjänstdata från den fysiska enheten registrerad toohello StorSimple enheten Manager-tjänsten.

## <a name="create-and-configure-hello-cloud-appliance"></a>Skapa och konfigurera hello molnet installation

Innan du utför de här procedurerna bör du kontrollera att du har uppfyllt hello [krav för hello för molnet](#prerequisites-for-the-cloud-appliance).

Utföra hello följande steg toocreate en StorSimple-enhet för molnet.

### <a name="step-1-create-a-cloud-appliance"></a>Steg 1: Skapa en molninstallation

Utföra hello följande steg toocreate hello StorSimple-enhet för molnet.

[!INCLUDE [Create a cloud appliance](../../includes/storsimple-8000-create-cloud-appliance-u2.md)]

Om hello hello molnet enhetens misslyckas i det här steget, kan du inte har anslutning toohello Internet. Mer information finns för[Felsök anslutningsfel till Internet](#troubleshoot-internet-connectivity-errors) när du skapar en moln-installation.

### <a name="step-2-configure-and-register-hello-cloud-appliance"></a>Steg 2: Konfigurera och registrera hello molnet installation

Innan du börjar den här proceduren måste du kontrollera att du har en kopia av krypteringsnyckeln för tjänstdata hello. krypteringsnyckel för tjänstdata hello skapas när du har registrerat din första fysiska StorSimple-enhet med hello StorSimple enheten Manager-tjänsten. Du har fått instruktioner toosave den på en säker plats. Om du inte har en kopia av krypteringsnyckeln för tjänstdata hello måste du kontakta Microsoft Support för hjälp.

Utför följande steg tooconfigure hello och registrera ditt moln StorSimple-enhet.

[!INCLUDE [Configure and register a cloud appliance](../../includes/storsimple-8000-configure-register-cloud-appliance.md)]

### <a name="step-3-optional-modify-hello-device-configuration-settings"></a>Steg 3: (Valfritt) Ändra hello konfigurationsinställningar

hello beskrivs följande avsnitt hello konfigurationsinställningar som behövs för hello StorSimple moln installation om du vill toouse CHAP, StorSimple Snapshot Manager eller ändra hello enhetens administratörslösenord.

#### <a name="configure-hello-chap-initiator"></a>Konfigurera hello CHAP-initieraren

Den här parametern innehåller hello-autentiseringsuppgifter som din moln-enhet (målet) förväntar sig från hello initierarna (servrarna) som försöker tooaccess hello volymer. hello initierare anger ett CHAP-användarnamn och ett CHAP-lösenord tooidentify själva tooyour enheten under den här autentiseringen. Detaljerade anvisningar finns för[konfigurera CHAP för din enhet](storsimple-8000-configure-chap.md#unidirectional-or-one-way-authentication).

#### <a name="configure-hello-chap-target"></a>Konfigurera hello CHAP-målet

Den här parametern innehåller hello-autentiseringsuppgifter som din moln-enhet använder när en CHAP-aktiverad initierare begär ömsesidig eller dubbelriktad autentisering. Moln-enhet använder användarnamn omvänd CHAP och omvänd CHAP lösenord tooidentify själva toohello initieraren under den här autentiseringsprocessen.

> [!NOTE]
> Inställningarna för CHAP-målet är globala inställningar. När de här inställningarna tillämpas anslutet alla hello volymer toohello moln enheten använda CHAP-autentisering.

Detaljerade anvisningar finns för[konfigurera CHAP för din enhet](storsimple-8000-configure-chap.md#bidirectional-or-mutual-authentication).

#### <a name="configure-hello-storsimple-snapshot-manager-password"></a>Konfigurera hello lösenordet för StorSimple Snapshot Manager

Programvaran StorSimple Manager finns på din Windows-värd och kan administratörer toomanage säkerhetskopior av StorSimple-enheten i hello form av lokala och molnögonblicksbilder.

> [!NOTE]
> För hello molnet installation är din Windows-värd en virtuell Azure-dator.

När du konfigurerar en enhet i hello StorSimple Snapshot Manager uppmanas du tooprovide hello StorSimple enheten IP-adress och lösenord tooauthenticate lagringsenheten. Detaljerade anvisningar finns för[lösenordet konfigurera StorSimple Snapshot Manager](storsimple-8000-change-passwords.md#set-the-storsimple-snapshot-manager-password).

#### <a name="change-hello-device-administrator-password"></a>Ändra hello enhetens administratörslösenord

När du använder hello Windows PowerShell-gränssnittet tooaccess hello molnet installation är du obligatoriska tooenter administratörslösenord för enheten. För hello säkerheten för dina data, måste du ändra lösenordet innan hello moln-enhet kan användas. Detaljerade anvisningar finns för[konfigurera enhetens administratörslösenord](../storsimple/storsimple-8000-change-passwords.md#change-the-device-administrator-password).

## <a name="connect-remotely-toohello-cloud-appliance"></a>Fjärransluta toohello moln-enhet

Fjärråtkomst tooyour moln installation via hello Windows PowerShell-gränssnittet är inte aktiverad som standard. Måste du aktivera fjärrhantering på hello molnet enheten först och sedan på hello klienten användas tooaccess hello molnet installation.

Hej tvåstegsverifiering nedan beskriver hur tooconnect via fjärranslutning tooyour cloud installation.

### <a name="step-1-configure-remote-management"></a>Steg 1: Konfigurera fjärrhantering

Utföra hello följande steg tooconfigure fjärrhantering för din StorSimple-enhet för molnet.

[!INCLUDE [Configure remote management via HTTP for cloud appliance](../../includes/storsimple-8000-configure-remote-management-http-device.md)]

### <a name="step-2-remotely-access-hello-cloud-appliance"></a>Steg 2: Fjärråtkomst till hello molnet installation

När du aktiverar fjärrhantering på hello moln-enhet, kan du använda Windows PowerShell-fjärrkommunikation tooconnect toohello installation från en annan virtuell dator i hello samma virtuella nätverk. Du kan till exempel ansluta från hello värden VM att du konfigurerat och använt tooconnect iSCSI. I de flesta distributioner öppnas du en offentlig slutpunkt tooaccess värden VM som du kan använda för att komma åt hello molnet installation.

> [!WARNING]
> **Förbättrad säkerhet rekommenderar vi att du använder HTTPS när du ansluter toohello slutpunkter och ta sedan bort hello slutpunkter när du har slutfört din PowerShell-fjärrsession.**

Du måste följa hello procedurer i [fjärranslutning tooyour StorSimple-enhet](storsimple-8000-remote-connect.md) tooset in fjärrstyrning för din enhet i molnet.

## <a name="connect-directly-toohello-cloud-appliance"></a>Anslut direkt toohello moln-enhet

Du kan också ansluta direkt toohello moln installation. tooconnect direkt toohello molnet installation från en annan dator utanför Hej virtuellt nätverk eller utanför hello Microsoft Azure-miljön, måste du skapa ytterligare slutpunkter.

Utföra hello följande steg toocreate en offentlig slutpunkt på hello moln-enhet.

[!INCLUDE [Create public endpoints on a cloud appliance](../../includes/storsimple-8000-create-public-endpoints-cloud-appliance.md)]

Vi rekommenderar att du ansluter från en annan virtuell dator i hello samma virtuella nätverket eftersom den här metoden minimerar hello antalet offentliga slutpunkter i ditt virtuella nätverk. I det här fallet ansluter toohello virtuella datorn via en fjärrskrivbordssession och sedan konfigurera den virtuella datorn för användning som alla andra Windows-klienter i ett lokalt nätverk. Du behöver inte tooappend hello offentliga portnumret eftersom hello port redan är känd.

## <a name="work-with-hello-storsimple-cloud-appliance"></a>Arbeta med hello StorSimple-enhet för molnet

Nu när du har skapat och konfigurerat hello StorSimple-enhet för moln, är klar toostart arbeta med den. Du kan arbeta med volymbehållare, volymer och säkerhetskopieringspolicyer i en molninstallation på samma sätt som på en fysisk StorSimple-enhet. hello enda skillnaden är att du måste du markera hello molnet installation från din enhetslista toomake. Se för[använda hello StorSimple Enhetshanteraren service toomanage en moln-installation](storsimple-8000-manager-service-administration.md) stegvisa anvisningar för hello olika administrationsuppgifter för hello för molnet.

hello följande avsnitt beskrivs några av hello skillnader som du får när du arbetar med hello molnet installation.

### <a name="maintain-a-storsimple-cloud-appliance"></a>Underhålla en StorSimple-molninstallation

Eftersom det är en programvarubaserad enhet, underhåll för hello för molnet är minimal när jämfört med toomaintenance för hello fysisk enhet.

Du kan inte uppdatera en molninstallation. Använd hello senaste versionen av programvaran toocreate en ny installation av molnet.


### <a name="storage-accounts-for-a-cloud-appliance"></a>Lagringskonton för en molninstallation

Lagringskonton skapas för användning av hello StorSimple Device Manager service, hello molnet installation och hello fysisk enhet. När du skapar dina lagringskonton rekommenderar vi att du använder en regionsidentifierare i hello eget namn. Detta säkerställer att hello regionen är konsekvent på alla systemkomponenter hello. För en för moln, är det viktigt att alla hello-komponenter finns i hello samma region tooprevent prestandaproblem.

Stegvisa anvisningar finns för[Lägg till ett lagringskonto](storsimple-8000-manage-storage-accounts.md#add-a-storage-account).

### <a name="deactivate-a-storsimple-cloud-appliance"></a>Inaktivera en StorSimple-molninstallation

När du inaktiverar en enhet i molnet hello åtgärden tar bort hello VM och hello resurser som skapades när den etablerades. När hello molnet installation inaktiveras, kan den inte återställas tooits tidigare tillstånd. Innan du inaktiverar hello molnet installation Se till att toostop eller ta bort klienter och värdar som är beroende av den.

Inaktivera en enhet i molnet resulterar i hello följande åtgärder:

* hello molnet enheten tas bort.
* hello OS-disk- och datadiskar som skapats för hello för molnet tas bort.
* hello värdbaserade tjänsten och virtuella nätverk som skapades under etableringen bevaras. Om du inte använder dem, bör du ta bort dem manuellt.
* Molnögonblicksbilder som skapats för hello för molnet bevaras.

Stegvisa anvisningar finns för[inaktivera och ta bort din StorSimple-enhet](storsimple-8000-deactivate-and-delete-device.md).

Så snart hello moln-enhet visas som inaktiverad på hello StorSimple Enhetshanteraren service blad, du kan ta bort hello moln-enhet från listan över enheter på hello **enheter** bladet.

### <a name="start-stop-and-restart-a-cloud-appliance"></a>Starta, stoppa och starta om en molninstallation
Till skillnad från hello fysiska StorSimple-enheten finns det ingen ström på eller power av knappen toopush på en StorSimple-enhet för molnet. Det kan dock finnas tillfällen där du behöver toostop och starta om hello molnet installation.

Hej enklast för du toostart, stoppa och starta om en installation med molnet via hello service bladet för virtuella datorer. Gå hello virtuella service. Hello listan över virtuella datorer och identifiera hello VM motsvarande tooyour moln-enhet (samma namn) och på hello VM-namn. När du tittar på din virtuella bladet hello molnet installation status är **kör** eftersom den startas som standard när den har skapats. Du kan starta, stoppa och starta om en virtuell dator när som helst.

[!INCLUDE [Stop and restart cloud appliance](../../includes/storsimple-8000-stop-restart-cloud-appliance.md)]

### <a name="reset-toofactory-defaults"></a>Återställ toofactory inställningar
Om du bestämmer dig för toostart över med din enhet i molnet, inaktivera och ta bort den och skapa en ny.

## <a name="fail-over-toohello-cloud-appliance"></a>Växla över toohello moln-enhet
Katastrofåterställning (DR) är en av hello viktiga scenarier som hello StorSimple-enhet för molnet har utformats för. I det här scenariot hello fysiska StorSimple-enheten eller hela datacentret inte tillgängliga. Lyckligtvis kan du använda ett moln installation toorestore åtgärder på en annan plats. Under Katastrofåterställning hello volymbehållarna från hello källenheten ändra ägarskap och är överförda toohello moln installation.

hello kraven för Katastrofåterställning är:

* hello molnet installation har skapats och konfigurerats.
* Alla hello volymer inom hello volymbehållare är offline.
* Hej volymbehållare som växlar du över, har en associerad ögonblicksbild i molnet.

> [!NOTE]
> * När du använder en moln-installation som hello sekundär enhet för Katastrofåterställning, Kom ihåg att hello 8010 har 30 TB standardlagring och 8020 har 64 TB premiumlagring. hello högre kapacitet 8020 moln installation kan vara mer lämpad för katastrofåterställning.

Stegvisa anvisningar finns för[växla över tooa moln installation](storsimple-8000-device-failover-cloud-appliance.md).

## <a name="delete-hello-cloud-appliance"></a>Ta bort hello molnet installation
Om du tidigare har konfigurerat och använt en molnet StorSimple-enhet men nu vill toostop debiteringarna för dess användning måste du stoppa hello molnet installation. Stoppa hello molnet installation tar bort hello VM. Om du stoppar molninstallationen ackumuleras inte längre kostnader för dess användning mot din prenumeration. hello lagringskostnaderna för hello OS- och datadiskar kommer dock fortsätta.

toostop alla hello debiterar, måste du ta bort hello molnet installation. toodelete hello säkerhetskopior som skapats av hello moln-enhet, kan du inaktivera eller ta bort hello enhet. Mer information finns i [Inaktivera och ta bort en StorSimple-enhet](storsimple-8000-deactivate-and-delete-device.md).

[!INCLUDE [Delete a cloud appliance](../../includes/storsimple-8000-delete-cloud-appliance.md)]

## <a name="troubleshoot-internet-connectivity-errors"></a>Felsöka problem med Internetanslutning
Under hello skapandet av en moln-installation, om det finns ingen anslutning toohello Internet, hello skapa steget misslyckas. tootroubleshoot anslutningsfel till Internet, utför hello följa stegen i hello Azure-portalen:

1. [Skapa en virtuell dator med Windows Server 2012 i Azure](/articles/virtual-machines/windows/quick-create-portal.md). Den här virtuella datorn ska använda hello samma lagringskonto, VNet och undernät som används av din enhet i molnet. Om det finns en befintlig Windows Server-värd i Azure med hjälp av Hej samma lagringskonto, VNet och undernät, du kan också använda den tootroubleshoot hello Internet-anslutning.
2. Fjärråtkomst logga in på hello virtuell dator som skapats i hello föregående steg.
3. Öppna ett kommandofönster inuti hello virtuella datorn (Win + R och skriv sedan `cmd`).
4. Kör följande kommando i Kommandotolken hello hello.

    `nslookup windows.net`
5. Om `nslookup` misslyckas, och sedan Internet-anslutningsfel förhindrar hello molnet installation registrerar toohello StorSimple enheten Manager-tjänsten.
6. Ändra hello krävs tooyour virtuellt nätverk tooensure som hello molnet installation är kan tooaccess Azure platser som _windows.net_.

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[använda hello StorSimple Enhetshanteraren service toomanage en moln-installation](storsimple-8000-manager-service-administration.md).
* Förstå hur för[återställer en StorSimple-volym från en säkerhetskopia](storsimple-8000-restore-from-backup-set-u2.md).

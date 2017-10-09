---
title: "aaaConfigure MPIO för din StorSimple-enhet | Microsoft Docs"
description: "Beskriver hur tooconfigure MPIO (Multipath I/O) för din StorSimple-enhet ansluten tooa värd som kör Windows Server 2012 R2."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/05/2017
ms.author: alkohli
ms.openlocfilehash: 7796524edc739826ba1e977161fc9988c6d316e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-multipath-io-for-your-storsimple-device"></a>Konfigurera MPIO för din StorSimple-enhet

Den här självstudiekursen beskriver hello steg du bör följa tooinstall och använda hello Multipath I/O (MPIO)-funktionen på en värd som kör Windows Server 2012 R2 och anslutna tooa fysiska StorSimple-enheten. hello anvisningarna i den här artikeln gäller tooStorSimple 8000-serien fysiska enheter. MPIO stöds för närvarande inte på en StorSimple-enhet för molnet.

Microsoft har inbyggt stöd för hello Multipath I/O (MPIO)-funktion i Windows Server toohelp skapa hög tillgänglighet, feltolerant SAN-konfigurationer. MPIO använder redundanta fysiska sökvägskomponenter – nätverkskort, kablar och växlar – toocreate logiska sökvägar mellan hello-servern och hello lagringsenhet. Om det finns ett komponentfel, använder orsakar en logiska sökvägen toofail, multipathing-logik en alternativ sökväg för i/o så att program fortfarande kommer åt sina data. Dessutom beroende på din konfiguration kan MPIO också förbättra prestanda genom ombalansering hello belastningen över dessa sökvägar. Mer information finns i [översikt över MPIO](https://technet.microsoft.com/library/cc725907.aspx "MPIO översikt och funktioner").

Hello hög tillgänglighet i StorSimple-lösningen ska konfigurerats MPIO på din StorSimple-enhet. Om MPIO är installerad på dina värdservrar som kör Windows Server 2012 R2, hello servrar kan sedan tolerera länken, nätverk och fel.

## <a name="mpio-configuration-summary"></a>Översikt över MPIO-konfiguration

MPIO är en valfri funktion i Windows Server och installeras inte som standard. Den installeras som en funktion via Server Manager.

Följ dessa steg tooconfigure MPIO på din StorSimple-enhet:

* Steg 1: Installera MPIO på hello Windows Server-värd
* Steg 2: Konfigurera MPIO för StorSimple-volymer
* Steg 3: Montera StorSimple-volymer på hello värden
* Steg 4: Konfigurera MPIO för hög tillgänglighet och belastningsutjämning

Var och en av hello föregående steg beskrivs i följande avsnitt hello.

## <a name="step-1-install-mpio-on-hello-windows-server-host"></a>Steg 1: Installera MPIO på hello Windows Server-värd

slutföra tooinstall-funktionen i Windows Server-värd hello följande procedur.

#### <a name="tooinstall-mpio-on-hello-host"></a>tooinstall MPIO på hello värden

1. Öppna Serverhanteraren på Windows Server-värd. Serverhanteraren startar som standard när en medlem i administratörsgruppen för hello loggar in tooa dator som kör Windows Server 2012 R2 eller Windows Server 2012. Om hello Serverhanteraren inte redan är öppen klickar du på **Start > Serverhanteraren**.
   
   ![Serverhanteraren](./media/storsimple-configure-mpio-windows-server/IC740997.png)

2. Klicka på **Serverhanteraren > instrumentpanelen > Lägg till roller och funktioner**. Detta startar hello **Lägg till roller och funktioner** guiden.
   
   ![Guiden Lägg till roller och funktioner 1](./media/storsimple-configure-mpio-windows-server/IC740998.png)
3. I hello **Lägg till roller och funktioner** guiden utföra hello följande steg:
   
   1. På hello **innan du börjar** klickar du på **nästa**.
   2. På hello **Välj installationstyp** godkänner hello standardinställningen **rollbaserad eller funktionsbaserad** installation. Klicka på **Nästa**.
   
       ![Guiden Lägg till roller och funktioner 2](./media/storsimple-configure-mpio-windows-server/IC740999.png)
   3. På hello **väljer målservern** väljer **välja en server från serverpoolen hello**. Värdservern bör identifieras automatiskt. Klicka på **Nästa**.
   4. På hello **Välj serverroller** klickar du på **nästa**.
   5. På hello **Välj funktioner** väljer **Multipath i/o**, och klicka på **nästa**.
   
       ![Guiden Lägg till roller och funktioner 5](./media/storsimple-configure-mpio-windows-server/IC741000.png)
   6. På hello **Bekräfta installationsinställningarna** sidan Bekräfta hello val och välj sedan **omstart hello-målservern automatiskt om det behövs**, enligt nedan. Klicka på **Installera**.
   
       ![Guiden Lägg till roller och funktioner 8](./media/storsimple-configure-mpio-windows-server/IC741001.png)
   7. Du meddelas när hello installationen är klar. Klicka på **Stäng** tooclose hello guiden.
   
       ![Guiden Lägg till roller och funktioner 9](./media/storsimple-configure-mpio-windows-server/IC741002.png)

## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>Steg 2: Konfigurera MPIO för StorSimple-volymer

MPIO måste vara konfigurerade tooidentify StorSimple-volymer. tooconfigure MPIO toorecognize StorSimple-volymer, utför följande steg hello.

#### <a name="tooconfigure-mpio-for-storsimple-volumes"></a>tooconfigure MPIO för StorSimple-volymer

1. Öppna hello **MPIO-konfiguration**. Klicka på **Serverhanteraren > instrumentpanelen > Verktyg > MPIO**.
2. I hello **MPIO-egenskaper** dialogrutan, Välj hello **Upptäck flera sökvägar** fliken.
3. Välj **lägga till stöd för iSCSI-enheter**, och klicka sedan på **Lägg till**.  
   ![Identifiera flera sökvägar för MPIO-egenskaper](./media/storsimple-configure-mpio-windows-server/IC741003.png)
4. Starta om när du uppmanas hello-servern.
5. I hello **MPIO-egenskaper** dialogrutan klickar du på hello **MPIO enheter** fliken. Klicka på **Lägg till**.
    </br>![MPIO egenskaper MPIO enheter](./media/storsimple-configure-mpio-windows-server/IC741004.png)
6. I hello **lägga till stöd för MPIO** dialogrutan under **enhetens maskinvaru-ID**, ange serienumret för enheten. tooget hello Enhetsåtkomst för seriell tal, StorSimple Device Manager-tjänsten. Navigera för**enheter > instrumentpanelen**. hello enhetens serienummer visas i högra hello **snabb i korthet** rutan hello enheten instrumentpanelen.
    </br>
    ![Lägga till stöd för MPIO](./media/storsimple-configure-mpio-windows-server/IC741005.png)
7. Starta om när du uppmanas hello-servern.

## <a name="step-3-mount-storsimple-volumes-on-hello-host"></a>Steg 3: Montera StorSimple-volymer på hello värden

När MPIO har konfigurerats på Windows Server, volymerna som skapats på hello StorSimple-enhet kan monteras och sedan dra nytta av MPIO för redundans. toomount en volym, utföra hello följande steg.

#### <a name="toomount-volumes-on-hello-host"></a>toomount volymer på hello värden

1. Öppna hello **iSCSI-Initieraregenskaper** fönster på hello Windows Server-värd. Klicka på **Serverhanteraren > instrumentpanelen > Verktyg > iSCSI-initieraren**.
2. I hello **iSCSI-Initieraregenskaper** dialogrutan hello identifiering fliken och klicka sedan på **identifiera målportal**.
3. I hello **identifiera målportal** dialogrutan utför hello följande steg:
   
   1. Ange hello IP-adressen för hello-dataporten av StorSimple-enheten (till exempel ange DATA 0).
   2. Klicka på **OK** tooreturn toohello **iSCSI-Initieraregenskaper** dialogrutan.
     
     > [!IMPORTANT]
     > **Om du använder ett privat nätverk för iSCSI-anslutningar, ange hello IP-adressen för hello-dataporten som är anslutna toohello privat nätverk.**
    
4. Upprepa steg 2 – 3 för ett andra nätverksgränssnitt (till exempel DATA 1) på enheten. Tänk på att dessa gränssnitt ska aktiveras för iSCSI. Mer information finns i [ändra nätverksgränssnitt](storsimple-8000-modify-device-config.md#modify-network-interfaces).
5. Välj hello **mål** fliken i hello **iSCSI-Initieraregenskaper** dialogrutan. Du bör se hello StorSimple-enhet mål IQN under **upptäckta mål**.

   ![iSCSI-initieraren egenskaper för mål](./media/storsimple-configure-mpio-windows-server/IC741007.png)
   
6. Klicka på **Anslut** tooestablish en iSCSI-session med din StorSimple-enhet. En **ansluta tooTarget** dialogrutan visas.
7. I hello **ansluta tooTarget** dialogrutan, Välj hello **aktivera Multipath** kryssrutan. Klicka på **avancerade**.
8. I hello **avancerade inställningar** dialogrutan utför hello följande steg:
   
   1. På hello **lokala kortet** listrutan, Välj **Microsoft iSCSI Initiator**.
   2. På hello **initieraren IP** listrutan, Välj hello hello värdens IP-adress.
   3. På hello **målportal** IP-listan, Välj hello IP enhetsgränssnittet.
   4. Klicka på **OK** tooreturn toohello **iSCSI-Initieraregenskaper** dialogrutan.
9. Klicka på **Egenskaper**. I hello **egenskaper** dialogrutan klickar du på **lägga till sessionen**.
10. I hello **ansluta tooTarget** dialogrutan, Välj hello **aktivera Multipath** kryssrutan. Klicka på **avancerade**.
11. I hello **avancerade inställningar** dialogrutan:

    1. På hello **lokala kortet** listrutan, Välj Microsoft iSCSI-initieraren.
    2. På hello **initieraren IP** listrutan, Välj hello IP-adress motsvarande toohello värd. I så fall måste ansluter du två nätverksgränssnitt på enheten hello tooa nätverksgränssnitt på hello värden. Det här gränssnittet är därför hello samma som angetts för hello första sessionen.
    3. På hello **Target Portal IP** listrutan, Välj hello IP-adress för hello andra data gränssnittet aktiveras hello enhet.
    4. Klicka på **OK** tooreturn toohello iSCSI-initieraren egenskapsdialogrutan. Du har lagt till ett andra session toohello mål.
12. Öppna **Datorhantering** genom att navigera för**Serverhanteraren > instrumentpanelen > Datorhantering**. Hello vänster klickar du på **lagring > Diskhantering**. hello volym som har skapats på hello StorSimple-enhet som är synliga toothis värden visas under **Diskhantering** som nya diskarna.
13. Initierar hello disk och skapar en ny volym. Välj en blockstorlek på 64 KB under hello-format.
    
    ![Diskhantering](./media/storsimple-configure-mpio-windows-server/IC741008.png)
14. Under **Diskhantering**, högerklicka på hello **Disk** och välj **egenskaper**.
15. I hello StorSimple modellen ### **Multipath Disk enhetsegenskaper** dialogrutan klickar du på hello **MPIO** fliken.
    
    ![StorSimple 8100 Disk med flera sökvägar DeviceProp.](./media/storsimple-configure-mpio-windows-server/IC741009.png)
16. I hello **DSM namnet** klickar du på **information** och kontrollera att toohello standardparametrarna är hello parametrar. hello-standardparametrar är:
    
    * Kontrollera Period = 30
    * Antal försök = 3
    * PDO ta bort Period = 20
    * Återförsöksintervall = 1
    * Kontrollera aktiverat = avmarkerad.

> [!NOTE]
> **Ändra inte hello-standardparametrar.**


## <a name="step-4-configure-mpio-for-high-availability-and-load-balancing"></a>Steg 4: Konfigurera MPIO för hög tillgänglighet och belastningsutjämning

Flera sökvägar baseras i hög tillgänglighet och belastningsutjämning, flera sessioner måste läggas till manuellt toodeclare hello olika sökvägar tillgängliga. Till exempel om hello värden har två gränssnitt som är anslutna har tooSAN och hello enhet två gränssnitt anslutna tooSAN, måste du fyra sessioner som är konfigurerad med rätt sökväg permutationer (endast två sessioner måste utföras om varje gränssnitt för DATA och värd-gränssnittet är på en annan IP-undernät och inte är dirigerbart).

**Vi rekommenderar att du har minst 8 parallella aktiva sessioner mellan hello enhet och programvärden.** Detta kan uppnås genom att aktivera 4 nätverkskort på Windows Server-systemet. Använda fysiska nätverkskort eller virtuella gränssnitt via nätverket virtualiseringsteknik på hello maskinvara eller operativsystem nivå på Windows Server-värd. Med hello två nätverksgränssnitt på hello enhet leder den här konfigurationen 8 aktiva sessioner. Den här konfigurationen hjälper till att optimera hello enhet och molnet genomflöde.

> [!IMPORTANT]
> **Vi rekommenderar att du inte blandar 1 GbE och 10 GbE-nätverkskort. Om du använder två nätverksgränssnitt ska båda gränssnitten vara hello identiska.**

hello nedan beskrivs hur tooadd sessioner när en StorSimple-enhet med två nätverksgränssnitt är anslutna tooa värd med två nätverksgränssnitt. Detta ger dig endast 4 sessioner. Använd samma procedur med en StorSimple-enhet med två gränssnitt anslutna tooa värden med fyra nätverksgränssnitt. Du måste i stället för hello tooconfigure 8 4 sessioner som beskrivs här.

### <a name="tooconfigure-mpio-for-high-availability-and-load-balancing"></a>tooconfigure MPIO för hög tillgänglighet och belastningsutjämning

1. Utföra en identifiering av hello mål: i hello **iSCSI-Initieraregenskaper** på hello dialogrutan **identifiering** klickar du på **identifiera Portal**.
2. I hello **ansluta tooTarget** dialogrutan Ange hello IP-adressen till en av hello enheten nätverksgränssnitt.
3. Klicka på **OK** tooreturn toohello **iSCSI-Initieraregenskaper** dialogrutan.
4. I hello **iSCSI-Initieraregenskaper** dialogrutan, Välj hello **mål** fliken Markera hello identifierade mål och klicka sedan på **Anslut**. Hej **ansluta tooTarget** dialogrutan visas.
5. I hello **ansluta tooTarget** dialogrutan:
   
   1. Lämna hello standard markerad Målinställningar för **lägga till den här anslutningen** toohello lista med favorit mål. Detta gör hello enheten automatiskt att försöka toorestart hello anslutningen varje gång datorn startas om.
   2. Välj hello **aktivera Multipath** kryssrutan.
   3. Klicka på **avancerade**.
6. I hello **avancerade inställningar** dialogrutan:
   
   1. På hello **lokala kortet** listrutan, Välj **Microsoft iSCSI Initiator**.
   2. På hello **initieraren IP** listrutan, Välj hello hello värdens IP-adress.
   3. På hello **Target Portal IP** listrutan, Välj hello IP-adressen för hello data gränssnitt är aktiverat på hello enhet.
   4. Klicka på **OK** tooreturn toohello iSCSI-initieraren egenskapsdialogrutan.
7. Klicka på **egenskaper**, och i hello **egenskaper** dialogrutan klickar du på **lägga till sessionen**.
8. I hello **ansluta tooTarget** dialogrutan, Välj hello **aktivera Multipath** kryssrutan och klicka sedan på **Avancerat**.
9. I hello **avancerade inställningar** dialogrutan:
   
   1. På hello **lokala kortet** listrutan, Välj **Microsoft iSCSI Initiator**.
   2. På hello **initieraren IP** listrutan, Välj hello IP-adress motsvarande toohello andra gränssnitt på hello värden.
   3. På hello **Target Portal IP** listrutan, Välj hello IP-adress för hello andra data gränssnittet aktiveras hello enhet.
   4. Klicka på **OK** tooreturn toohello **iSCSI-Initieraregenskaper** dialogrutan. Du har nu lagt till ett andra session toohello mål.
10. Upprepa steg 8-10 tooadd ytterligare sessioner (sökvägar) toohello mål. Du kan lägga till summan av fyra sessioner med två gränssnitt på hello värden och två på hello enhet.
11. När du lägger till hello önskad sessioner (sökvägar) i hello **iSCSI-Initieraregenskaper** dialogrutan, Välj hello mål och klicka på **egenskaper**. På fliken för hello-sessioner för hello **egenskaper** dialogrutan, Observera hello fyra session identifierare som motsvarar toohello möjliga sökvägar permutationer. toocancel en session, Välj hello kryssrutan nästa tooa sessions-ID och klicka sedan på **frånkoppling**.
12. tooview enheter visas inom sessioner, Välj hello **enheter** klickar du på fliken tooconfigure hello MPIO-princip för en vald enhet **MPIO**. Hej **enhetsinformation** dialogrutan visas. På hello **MPIO** fliken kan du välja lämpliga hello **princip för belastningsutjämning** inställningar. Du kan också visa hello **Active** eller **vänteläge** Sökvägstyp.

## <a name="next-steps"></a>Nästa steg

Lär dig mer om [med hello StorSimple Enhetshanteraren service toomodify enhetskonfigurationen StorSimple](storsimple-8000-modify-device-config.md).


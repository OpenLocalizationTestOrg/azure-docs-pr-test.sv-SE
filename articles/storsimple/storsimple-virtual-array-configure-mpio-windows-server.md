---
title: "aaaConfigure MPIO på värden anslutna tooStorSimple virtuella matris | Microsoft Docs"
description: "Beskriver hur tooconfigure MPIO (Multipath I/O) för din virtuella StorSimple-matrisen anslutna tooa värd som kör Windows Server 2012 R2."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 5b7a7f99-ee5b-4b7d-ab32-483a5a1fa504
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/01/2017
ms.author: alkohli
ms.openlocfilehash: 0e6df23bba29395329685cbf2c968675abb04cfc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-multipath-io-on-windows-server-host-for-hello-storsimple-virtual-array"></a>Konfigurera Multipath i/o på Windows Server-värden för hello virtuella StorSimple-matris
## <a name="overview"></a>Översikt
Den här artikeln beskrivs hur tooinstall-funktionen Multipath I/O (MPIO) på Windows Server-värd, tillämpa specifika konfigurationsinställningar för endast StorSimple-volymer och sedan kontrollera MPIO för StorSimple-volymer. hello proceduren förutsätter att din virtuella StorSimple 1200-matris med två nätverksgränssnitt anslutna tooa Windows Server-värd med två nätverksgränssnitt. hello informationen i den här artikeln gäller bara toohello virtuella matris. Information om enheter för StorSimple 8000-serien finns för[konfigurera MPIO för StorSimple värden](storsimple-configure-mpio-windows-server.md). 

hello MPIO-funktion i Windows Server hjälper dig att skapa hög tillgänglighet, feltolerant lagringskonfigurationer. MPIO använder redundanta fysiska sökvägskomponenter – nätverkskort, kablar och växlar – toocreate logiska sökvägar mellan hello-servern och hello lagringsenhet. Om det finns ett komponentfel, använder orsakar en logiska sökvägen toofail, multipathing-logik en alternativ sökväg för i/o så att program fortfarande kommer åt sina data. Dessutom beroende på din konfiguration kan MPIO också förbättra prestanda genom att väga nytt hello belastningen över dessa sökvägar. Mer information finns i [översikt över MPIO](https://technet.microsoft.com/library/cc725907.aspx "MPIO översikt och funktioner").

Konfigurera MPIO på hello Windows Server-värdar anslutna tooyour virtuella StorSimple-matris (model 1200) för hello hög tillgänglighet i StorSimple-lösningen. hello värdservrar för fjärrskrivbordssessioner kan sedan tolerera länken, nätverk och fel. 

Du behöver toofollow dessa steg tooconfigure MPIO: 

* Förutsättningar för konfiguration
* Steg 1: Installera MPIO på hello Windows Server-värd
* Steg 2: Konfigurera MPIO för StorSimple-volymer
* Steg 3: Montera StorSimple-volymer på hello värden

Varje hello senare steg beskrivs i följande avsnitt hello.

## <a name="prerequisites"></a>Krav
Det här avsnittet beskrivs hello konfigurationskrav för hello Windows Server-värden och virtuella matrisen.

### <a name="on-windows-server-host"></a>På Windows Server-värd
* Se till att Windows Server-värd har 2 nätverksgränssnitt som är aktiverad.

### <a name="on-storsimple-virtual-array"></a>På virtuella StorSimple-matris
* hello virtuella matrisen ska konfigureras som en iSCSI-server. Det finns fler toolearn [Konfigurera virtuella matris som en iSCSI-server](storsimple-virtual-array-deploy3-iscsi-setup.md). En eller flera nätverksgränssnitt måste vara aktiverad på hello matris.   
* hello nätverksgränssnitt på din virtuella matrisen ska kunna nås från hello Windows Server-värd.
* En eller flera volymer ska skapas på din virtuella StorSimple-matrisen. Det finns fler toolearn [lägga till en volym](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume) på din virtuella StorSimple-matrisen. I den här proceduren skapat vi 3 volymer (1 lokalt Fäst och 2 nivåindelade volymer som visas nedan) på virtuella hello-matrisen.
  
    ![mpio0](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio0.png)

### <a name="hardware-configuration-for-storsimple-virtual-array"></a>Maskinvarukonfiguration för virtuell StorSimple-matris
hello bilden nedan visar hello maskinvarukonfiguration för hög tillgänglighet och belastningsutjämning MPIO för din Windows Server-värden och virtuella StorSimple matris som används i den här proceduren.

![maskinvarukonfiguration för MPIO](./media/storsimple-virtual-array-configure-mpio-windows-server/1200hardwareconfig.png)

Som visas i föregående bild hello:

* Din virtuella StorSimple-matris som har etablerats på Hyper-V är en enskild nod active enhet som konfigurerats som en iSCSI-server.
* Två virtuella nätverksgränssnitt är aktiverade på matrisen. Kontrollera att två nätverksgränssnitt har aktiverats genom att navigera för i hello lokala webbgränssnittet för ditt virtuella matrisen,**nätverksinställningar** enligt nedan:
  
    ![Nätverksgränssnitt som är aktiverad på 1200](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio9.png)
  
    Obs hello IPv4-adresser för hello aktiverade nätverksgränssnitt (Ethernet, Ethernet 2 som standard) och spara för senare användning på hello värden.
* Två nätverksgränssnitt är aktiverade på Windows Server-värd. Om hello anslutna gränssnitt för värden och matrisen är på hello samma undernät, och sedan det ska finnas 4 sökvägar. Detta var hello skiftläget för den här proceduren. Men om varje nätverksgränssnitt på hello matris och värd-gränssnittet är i ett annat IP-undernät (och inte dirigerbara) kan blir endast 2 sökvägar tillgängliga.

## <a name="step-1-install-mpio-on-hello-windows-server-host"></a>Steg 1: Installera MPIO på hello Windows Server-värd
MPIO är en valfri funktion i Windows Server och installeras inte som standard. Den installeras som en funktion via Server Manager. slutföra tooinstall-funktionen i Windows Server-värd hello följande procedur.

[!INCLUDE [storsimple-install-mpio-windows-server-host](../../includes/storsimple-install-mpio-windows-server-host.md)]

## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>Steg 2: Konfigurera MPIO för StorSimple-volymer
MPIO måste toobe konfigurerats tooidentify StorSimple-volymer. tooconfigure MPIO toorecognize StorSimple-volymer, utför följande steg hello.

[!INCLUDE [storsimple-configure-mpio-volumes](../../includes/storsimple-configure-mpio-volumes.md)]

## <a name="step-3-mount-storsimple-volumes-on-hello-host"></a>Steg 3: Montera StorSimple-volymer på hello värden
När MPIO har konfigurerats på Windows Server, volymerna som skapats på hello StorSimple matrisen kan monteras och sedan dra nytta av MPIO för redundans. toomount en volym, utföra hello följande steg.

#### <a name="toomount-volumes-on-hello-host"></a>toomount volymer på hello värden
1. Öppna hello **iSCSI-Initieraregenskaper** fönster på hello Windows Server-värd. Gå för**Serverhanteraren > instrumentpanelen > Verktyg > iSCSI-initieraren**.
2. I hello **iSCSI-Initieraregenskaper** dialogrutan klickar du på **identifiering**, och klicka sedan på **identifiera målportal**.
3. I hello **identifiera målportal** dialogrutan rutan, hello följande:
   
    1. Ange hello IP-adressen för nätverksgränssnittet för hello första aktiverade på din virtuella StorSimple-matrisen. Detta är som standard **Ethernet**. 
    2. Klicka på **OK** tooreturn toohello **iSCSI-Initieraregenskaper** dialogrutan.
     
    > [!IMPORTANT]
    > Om du använder ett privat nätverk för iSCSI-anslutningar, ange hello IP-adressen för hello-dataporten som är anslutna toohello privat nätverk.
     
4. Upprepa steg 2 – 3 för ett andra nätverksgränssnitt (exempelvis Ethernet 2) på matrisen. 
5. Välj hello **mål** fliken i hello **iSCSI-Initieraregenskaper** dialogrutan. För din virtuella matrisen du bör se varje volym yta som mål under **upptäckta mål**. I det här fallet skulle tre mål (motsvarande toothree volymer) identifieras.
   
    ![mpio1](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio1.png)
6. Klicka på **Anslut** tooestablish en iSCSI-session med din virtuella StorSimple-matris. En **ansluta tooTarget** visas dialogrutan. Välj hello **aktivera Multipath** kryssrutan. Klicka på **avancerade**.
   
    ![mpio2](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio2.png)
7. I hello **avancerade inställningar** dialogrutan rutan, hello följande:                                        
   
    1. På hello **lokala kortet** listrutan, Välj **Microsoft iSCSI Initiator**.
    2. På hello **initieraren IP** listrutan, Välj hello hello värdens IP-adress.
    3. På hello **målportal** IP-listan, Välj hello IP för matris-gränssnittet.
    4. Klicka på **OK** tooreturn toohello **iSCSI-Initieraregenskaper** dialogrutan.
     
        ![mpio3](./media/storsimple-ova-configure-mpio-windows-server/mpio3.png)

8. Klicka på **Egenskaper**. 
   
    ![mpio4](./media/storsimple-ova-configure-mpio-windows-server/mpio4.png)

9. I hello **egenskaper** dialogrutan klickar du på **lägga till sessionen**.
   
   ![mpio5](./media/storsimple-ova-configure-mpio-windows-server/mpio5.png)
10. I hello **ansluta tooTarget** dialogrutan, Välj hello **aktivera Multipath** kryssrutan. Klicka på **avancerade**.
11. I hello **avancerade inställningar** dialogrutan:                                        
    
    1. På hello **lokala kortet** listrutan, Välj Microsoft iSCSI-initieraren.

    2. På hello **initieraren IP** listrutan, Välj hello IP-adress motsvarande toohello värd. I så fall måste ansluter du två nätverksgränssnitt på matrisen hello tooa nätverksgränssnitt på hello värden. Det här gränssnittet är därför hello samma som angetts för hello första sessionen.

    3. På hello **Target Portal IP** listrutan, Välj hello IP-adress för hello andra data gränssnittet aktiveras hello matris.

    4. Klicka på **OK** tooreturn toohello iSCSI-initieraren egenskapsdialogrutan. Du har lagt till ett andra session toohello mål.
      
       ![mpio11](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio11.png)
    
    5. När du lägger till hello önskad sessioner (sökvägar) i hello **iSCSI-Initieraregenskaper** dialogrutan, Välj hello mål och klicka på **egenskaper**. På fliken för hello-sessioner för hello **egenskaper** dialogrutan, Observera hello fyra session identifierare som motsvarar toohello möjliga sökvägar permutationer. toocancel en session, Välj hello kryssrutan nästa tooa sessions-ID och klicka sedan på **frånkoppling**.

    6. tooview enheter visas inom sessioner, Välj hello **enheter** klickar du på fliken tooconfigure hello MPIO-princip för en vald enhet **MPIO**.

    7. Hej **information** visas dialogrutan. På hello **MPIO** fliken kan du välja lämpliga hello **princip för belastningsutjämning** inställningar. Du kan också visa hello **Active** eller **vänteläge** Sökvägstyp.
12. Upprepa steg 8-11 tooadd ytterligare sessioner (sökvägar) toohello mål. Du kan lägga till summan av fyra sessioner för varje mål med två gränssnitt på hello värden och två på hello virtuella matrisen. 
    
    ![mpio14](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio14.png)
13. Du behöver toorepeat dessa steg för varje volym (hämtar som mål).
    
    ![mpio15](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio15.png)
14. Öppna **Datorhantering** genom att navigera för**Serverhanteraren > instrumentpanelen > Datorhantering**. Hello vänster klickar du på **lagring > Diskhantering**. hello volymerna som skapats på hello virtuella StorSimple-matris som är synliga toothis värden visas under **Diskhantering** som nya diskarna.
15. Initierar hello disk och skapar en ny volym. Välj en storlek på allokeringsenhet (Australien) på 64 KB under hello-format. Upprepa hello processen för alla tillgängliga hello-volymer.
    
    ![Diskhantering](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio20.png)
16. Under **Diskhantering**, högerklicka på hello **Disk** och välj **egenskaper**.
17. I hello **Multipath Disk enhetsegenskaper** dialogrutan klickar du på hello **MPIO** fliken.
    
    ![Diskegenskaper](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio21.png)
18. I hello **DSM namnet** klickar du på **information** och kontrollera att toohello standardparametrarna är hello parametrar. hello-standardparametrar är:
    
    * Kontrollera Period = 30
    * Antal försök = 3
    * PDO ta bort Period = 20
    * Återförsöksintervall = 1
    * Kontrollera aktiverat = avmarkerad.
    
    > [!NOTE]
    > **Ändra inte hello-standardparametrar.**
   
## <a name="next-steps"></a>Nästa steg
Lär dig mer om [med hello StorSimple Enhetshanteraren service tooadminister din virtuella StorSimple-matris](storsimple-virtual-array-manager-service-administration.md).


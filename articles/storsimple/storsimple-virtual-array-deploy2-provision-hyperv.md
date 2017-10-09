---
title: "aaaProvision virtuella StorSimple-matris på Hyper-V | Microsoft Docs"
description: "Självstudierna andra i virtuella StorSimple-matris distribution innebär att etablera en virtuell Hyper-V-matris."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 4354963c-e09d-41ac-9c8b-f21abeae9913
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/15/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f47d642f740827ae1440b819e07067c6a183527f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-storsimple-virtual-array---provision-in-hyper-v"></a>Distribuera StorSimple virtuell matris - etablera i Hyper-V
![](./media/storsimple-virtual-array-deploy2-provision-hyperv/hyperv4.png)

## <a name="overview"></a>Översikt
Den här självstudiekursen beskrivs hur tooprovision en StorSimple virtuell matrisen på ett värdsystem som kör Hyper-V på Windows Server 2012 R2, Windows Server 2012 eller Windows Server 2008 R2. Den här artikeln gäller toohello distribution av virtuella StorSimple-matriser i Azure-portalen och Microsoft Azure Government-molnet.

Du måste administratören privilegier tooprovision och konfigurerar en virtuell matris. hello etablering och inledande installationen kan ta ungefär 10 minuter toocomplete.

## <a name="provisioning-prerequisites"></a>Etablering av förutsättningar
Här hittar du hello krav tooprovision virtuella matrisen på ett värdsystem som kör Hyper-V på Windows Server 2012 R2, Windows Server 2012 eller Windows Server 2008 R2.

### <a name="for-hello-storsimple-device-manager-service"></a>För hello StorSimple enheten Manager-tjänsten
Innan du börjar bör du kontrollera att:

* Du har slutfört alla steg i hello i [Förbered hello portal för virtuell StorSimple-matrisen](storsimple-virtual-array-deploy1-portal-prep.md).
* Du har hämtat hello virtuella matris avbildning för Hyper-V från hello Azure-portalen. Mer information finns **steg3: hämta hello virtuella matris avbildningen** av [Förbered hello portal för virtuell StorSimple-matris guiden](storsimple-virtual-array-deploy1-portal-prep.md).

  > [!IMPORTANT]
  > hello-program som körs på hello virtuella StorSimple-matrisen får endast användas med hello StorSimple enheten Manager-tjänsten.
  >
  >

### <a name="for-hello-storsimple-virtual-array"></a>För hello virtuella StorSimple-matris
Innan du distribuerar en virtuell matris, kontrollerar du att:

* Du har åtkomst tooa värdsystem som kör Hyper-V på Windows Server 2008 R2 eller senare att kan vara används tooa etablera en enhet.
* hello värdsystem är kan toodedicate hello följande resurser tooprovision virtuella matrisen:

  * Minst 4 kärnor.
  * Minst 8 GB RAM-minne. Om du planerar tooconfigure hello virtuella matris som filserver stöder 8 GB mindre än 2 miljoner filer. Du måste 16 GB RAM-minne toosupport 2-4 miljoner filer.
  * Ett nätverksgränssnitt.
  * En virtuell disk 500 GB för data.

### <a name="for-hello-network-in-hello-datacenter"></a>För hello nätverket i hello datacenter
Innan du kan granska hello nätverk krav toodeploy en virtuell StorSimple-matris och konfigurera hello datacenternätverket på lämpligt sätt. Mer information finns i [StorSimple virtuell matris nätverkskrav](storsimple-ova-system-requirements.md#networking-requirements).

## <a name="step-by-step-provisioning"></a>Steg för steg-etablering
tooprovision och ansluta tooa virtuella matrisen måste du tooperform hello följande steg:

1. Kontrollera att hello värdsystem har tillräckliga resurser toomeet hello minsta virtuella matris krav.
2. Etablera en virtuell matris i din hypervisor-program.
3. Starta virtuella hello-matris och hämta hello IP-adress.

Var och en av de här stegen beskrivs hello följande avsnitt.

## <a name="step-1-ensure-that-hello-host-system-meets-minimum-virtual-array-requirements"></a>Steg 1: Kontrollera att hello värdsystem uppfyller minsta virtuella matris
toocreate virtuella matris, behöver du:

* hello Hyper-V-rollen är installerad på Windows Server 2012 R2, Windows Server 2012 eller Windows Server 2008 R2 SP1.
* Microsoft Hyper-V Manager på en Microsoft Windows-klient ansluten toohello värden.

Se till att hello underliggande maskinvara (värdsystem) som du skapar virtuella hello-matris kan toodedicate hello följande resurser tooyour virtuella matris:

* Minst 4 kärnor.
* Minst 8 GB RAM-minne. Om du planerar tooconfigure hello virtuella matris som filserver stöder 8 GB mindre än 2 miljoner filer. Du måste 16 GB RAM-minne toosupport 2-4 miljoner filer.
* Ett nätverksgränssnitt.
* En virtuell disk 500 GB efter systemdata.

## <a name="step-2-provision-a-virtual-array-in-hypervisor"></a>Steg 2: Etablera en virtuell matris i hypervisor-program
Utför följande steg tooprovision en enhet i din hypervisor-programmet hello.

#### <a name="tooprovision-a-virtual-array"></a>tooprovision virtuella matris
1. Kopiera hello virtuella matris avbildningen tooa lokal enhet på Windows Server-värd. Du har hämtat den här avbildningen (VHD eller VHDX) via hello Azure-portalen. Anteckna hello plats dit du kopierade hello bilden som du använder den här avbildningen senare i hello proceduren.
2. Öppna **Serverhanteraren**. I hello längst upp till höger klickar du på **verktyg** och välj **Hyper-V Manager**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image1.png)  

   Om du kör Windows Server 2008 R2, öppna hello Hyper-V Manager. I Serverhanteraren klickar du på **roller > Hyper-V > Hyper-V Manager**.
3. I **Hyper-V Manager**hello scope i fönstret, högerklicka på ditt system nod tooopen hello snabbmenyn, och klicka sedan på **ny** > **virtuella**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image2.png)
4. På hello **innan du börjar** av hello guiden Ny virtuell dator, klickar du på **nästa**.
5. På hello **ange namn och plats** anger en **namn** för din virtuella matrisen. Klicka på **Nästa**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image4.png)
6. På hello **ange generation** , Välj hello enhetstyp avbildningen, och klickar sedan på **nästa**. Den här sidan visas inte om du använder Windows Server 2008 R2.

   * Välj **Generation 2** om du har hämtat en vhdx-avbildning för Windows Server 2012 eller senare.
   * Välj **Generation 1** om du har hämtat en VHD-avbildning för Windows Server 2008 R2 eller senare.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image5.png)
7. På hello **tilldela minne** anger en **startminne** av minst **8192 MB**inte aktivera dynamiskt minne, och klicka sedan på **nästa**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image6.png)  
8. På hello **Konfigurera nätverk** anger hello virtuell växel som är anslutna toohello Internet och klicka sedan på **nästa**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image7.png)
9. På hello **Anslut virtuell hårddisk** väljer **använder en befintlig virtuell hårddisk**anger hello plats hello virtuella matris bild (vhdx eller VHD) och klicka sedan på **nästa**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image8m.png)
10. Granska hello **sammanfattning** och klicka sedan på **Slutför** toocreate hello virtuell dator.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image9.png)
11. toomeet hello minimikraven, behöver du 4 kärnor. tooadd 4 virtuella processorer, Välj din värdsystem i hello **Hyper-V Manager** fönster. I hello högra rutan under hello lista över **virtuella datorer**, leta upp hello virtuell dator som du nyss skapade. Markera och högerklicka på datornamnet hello och välj **inställningar**.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image10.png)
12. På hello **inställningar** i hello vänstra fönsterrutan klickar du på **Processor**. Hello högra rutan Ange **antal virtuella processorer** too4 (eller mer). Klicka på **Använd**.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image11.png)
13. toomeet hello minimikraven, behöver du också tooadd en 500 GB data för virtuell disk. I hello **inställningar** sidan:

    1. I hello vänster och välj **SCSI-styrenhet**.
    2. I hello högra rutan, Välj **hårddisk,** och på **Lägg till**.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image12.png)
14. På hello **hårddisken** sidan, Välj hello **virtuella hårddisk** och klickar på **ny**. Hej **guiden för ny virtuell hårddisk** startar.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image13.png)
15. På hello **innan du börjar** av hello guiden Ny virtuell hårddisk, klickar du på **nästa**.
16. På hello **sidan Välj diskformat**, acceptera hello standardalternativet **VHDX** format. Klicka på **Nästa**. Den här skärmen visas inte om du kör Windows Server 2008 R2.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image15.png)
17. På hello **sidan Välj disktyp**, ange typen av virtuell hårddisk som **dynamiskt expanderande** (rekommenderas). **En fast storlek** disk skulle fungera men du kan behöva toowait lång tid. Vi rekommenderar att du inte använder hello **Differencing** alternativet. Klicka på **Nästa**. I Windows Server 2012 R2 och Windows Server 2012 **dynamiskt expanderande** är hello standardalternativet i Windows Server 2008 R2 standard hello är **fast storlek**.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image16.png)
18. På hello **ange namn och plats** anger en **namn** samt **plats** (du kan bläddra tooone) för hello datadisk. Klicka på **Nästa**.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image17.png)
19. På hello **konfigurera Disk** sidan Välj hello alternativet **skapa en ny tom virtuell hårddisk** och ange hello storlek som **500 GB** (eller mer). 500 GB är minimikravet för hello, kan du alltid etablera en större disk. Observera att du kan öka eller minska hello-disk som etablerats en gång. Mer information om hello storleken på disken tooprovision granska hello sizing avsnitt i hello [bästa praxis dokumentet](storsimple-ova-best-practices.md). Klicka på **Nästa**.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image18.png)
20. På hello **sammanfattning** granskar hello information om din virtuella datadisk och om uppfyllt, klickar du på **Slutför** toocreate hello disk. hello guiden stängs och en virtuell hårddisk läggs tooyour datorn.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image19.png)
21. Returnera toohello **inställningar** sidan. Klicka på **OK** tooclose hello **inställningar** sidan och returnera tooHyper-V Manager-fönstret.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image20.png)

## <a name="step-3-start-hello-virtual-array-and-get-hello-ip"></a>Steg 3: Starta hello virtuella matris och hämta hello IP
Utför följande steg toostart hello virtuella matrisen och ansluta tooit.

#### <a name="toostart-hello-virtual-array"></a>toostart hello virtuella matris
1. Starta virtuella hello-matris.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image21.png)
2. När hello enheten körs Välj hello enhet, högerklicka på och välj **Anslut**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image22.png)
3. Du kan ha toowait 5-10 minuter för hello enheten toobe redo. Ett meddelande visas på hello konsolen tooindicate hello pågår. När hello enheten är klar, går för**åtgärd**. Tryck på `Ctrl + Alt + Delete` toolog i toohello virtuella matris. hello standardanvändaren är *StorSimpleAdmin* och hello standardlösenordet är *Password1*.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image23.png)
4. Av säkerhetsskäl upphör hello enhetens administratörslösenord vid hello första inloggningen. Du kan ange toochange hello lösenord.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image24.png)

   Ange ett lösenord som innehåller minst 8 tecken. hello lösenordet måste uppfylla minst 3 av följande 4 krav hello: versaler, gemener, siffror och särskilda tecken. Ange hello lösenord tooconfirm den. Du meddelas hello lösenordet har ändrats.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image25.png)
5. Hello virtuella matris får att starta om när hello lösenordet har ändrats. Vänta tills hello enheten toostart.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image26.png)

    hello Windows PowerShell-konsol för hello enhet visas tillsammans med en förloppsindikator.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image27.png)
6. Steg 6 – 8 gäller endast när du startar upp i en icke-DHCP-miljö. Om du har en DHCP-miljö, hoppa över de här stegen och går toostep 9. Om du har startat upp din enhet i DHCP-miljön visas hello följande skärm.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image28m.png)

    Konfigurera hello nätverk.
7. Använd hello `Get-HcsIpAddress` kommandot toolist hello nätverksgränssnitt aktiverade på din virtuella matrisen. Om enheten har ett enda nätverksgränssnitt som är aktiverad, hello standardgränssnittet tilldelade toothis är `Ethernet`.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image29m.png)
8. Använd hello `Set-HcsIpAddress` cmdlet tooconfigure hello nätverk. Se följande exempel hello:

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image30.png)
9. När hello installationen är klar och hello enheten har startats in, visas hello banderolltext för enheten. Anteckna hello IP-adress och hello-URL som visas i hello banderoll text toomanage hello enhet. Använd den här IP-adressen tooconnect toohello webbgränssnittet för ditt virtuella matris och fullständig hello lokal installation och registrering.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image31m.png)
10. (Valfritt) Utför det här steget endast om du distribuerar din enhet i hello offentliga moln. Du kan nu hello USA FIPS Federal Information Processing Standard ()-läge på enheten. hello FIPS 140-standarden definierar kryptografiska algoritmer som godkänts för användning av oss federala myndigheter datorsystem för hello skyddet av känsliga data.

    1. tooenable hello FIPS-läge, kör följande cmdlet hello:

        `Enable-HcsFIPSMode`
    2. Starta om enheten när du har aktiverat hello FIPS-läge så att hello kryptografiska verifieringar börjar gälla.

       > [!NOTE]
       > Du kan aktivera eller inaktivera FIPS-läge på enheten. Alternerande hello enhet mellan FIPS och icke-FIPS-läge stöds inte.
       >
       >

Om enheten inte uppfyller lägsta konfigurationskrav som hello finns hello följande fel i hello banderolltext (se nedan). Ändra hello enhetskonfigurationen så att hello datorn har tillräckliga resurser toomeet hello minimikraven. Du kan sedan starta om och Anslut toohello enhet. Se toohello lägsta konfigurationskrav i [steg 1: Kontrollera att hello värdsystem uppfyller minsta virtuella matris](#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements).

![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image32.png)

Om du står inför ett fel under hello startkonfiguration med hello lokala webbgränssnittet Se toohello följande arbetsflöden:

* Kör diagnostiktest för[felsöka installationen av UI](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).
* [Generera loggen paketet och visa loggfiler](storsimple-ova-web-ui-admin.md#generate-a-log-package).

## <a name="next-steps"></a>Nästa steg
* [Konfigurera din virtuella StorSimple-matris som en filserver](storsimple-virtual-array-deploy3-fs-setup.md)
* [Konfigurera din virtuella StorSimple-matris som en iSCSI-server](storsimple-virtual-array-deploy3-iscsi-setup.md)

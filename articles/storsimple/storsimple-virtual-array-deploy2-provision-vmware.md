---
title: "aaaProvision virtuella StorSimple-matris på VMware | Microsoft Docs"
description: "Den här andra kursen i virtuella StorSimple-matris deployment serien innebär att etablera en virtuell enhet i VMware."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 0425b2a9-d36f-433d-8131-ee0cacef95f8
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/15/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0912c1c315a04ea46b6373a8fcd5554ecae14e61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-storsimple-virtual-array---provision-in-vmware"></a>Distribuera StorSimple virtuell matris - etablera i VMware
![](./media/storsimple-virtual-array-deploy2-provision-vmware/vmware4.png)

## <a name="overview"></a>Översikt
Den här självstudiekursen beskriver hur tooprovision och ansluta tooa virtuella StorSimple-matris på ett värdsystem som kör VMware ESXi 5.5 och senare. Den här artikeln gäller toohello distribution av virtuella StorSimple-matriser i Azure-portalen och hello Microsoft Azure Government-molnet.

Du behöver administratören privilegier tooprovision och ansluta tooa virtuell enhet. hello etablering och inledande installationen kan ta ungefär 10 minuter toocomplete.

## <a name="provisioning-prerequisites"></a>Etablering av förutsättningar
Hej krav tooprovision en virtuell enhet på ett värdsystem som kör VMware ESXi 5.5 och ovan, är följande.

### <a name="for-hello-storsimple-device-manager-service"></a>För hello StorSimple enheten Manager-tjänsten
Innan du börjar bör du kontrollera att:

* Du har slutfört alla steg i hello i [Förbered hello portal för virtuell StorSimple-matrisen](storsimple-virtual-array-deploy1-portal-prep.md).
* Du har hämtat hello virtuella enheten avbildning för VMware från hello Azure-portalen. Mer information finns **steg3: hämta hello virtuella enheten avbildningen** av [Förbered hello portal för virtuell StorSimple-matris guiden](storsimple-virtual-array-deploy1-portal-prep.md).

### <a name="for-hello-storsimple-virtual-device"></a>För hello virtuell StorSimple-enhet
Kontrollera följande innan du distribuerar en virtuell enhet:

* Du har åtkomst tooa värdsystem som kör Hyper-V (2008 R2 eller senare) som kan vara används tooa etablera en enhet.
* hello värdsystem är kan toodedicate hello följande resurser tooprovision din virtuella enhet:

  * Minst 4 kärnor.
  * Minst 8 GB RAM-minne. Om du planerar tooconfigure hello virtuella matris som filserver stöder 8 GB mindre än 2 miljoner filer. Du måste 16 GB RAM-minne toosupport 2-4 miljoner filer.
  * Ett nätverksgränssnitt.
  * En virtuell disk 500 GB efter systemdata.

### <a name="for-hello-network-in-datacenter"></a>För hello nätverket i datacentret
Innan du börjar bör du kontrollera att:

* Du har granskat hello nätverk krav toodeploy en virtuell StorSimple-enhet och konfigurerade hello datacenternätverket enligt hello krav. 

## <a name="step-by-step-provisioning"></a>Steg för steg-etablering
tooprovision och ansluta tooa virtuell enhet, måste du tooperform hello följande steg:

1. Kontrollera att hello värdsystem har tillräckliga resurser toomeet hello minsta virtuella enhetskraven.
2. Etablera en virtuell enhet i din hypervisor-program.
3. Starta hello virtuell enhet och hämta hello IP-adress.

## <a name="step-1-ensure-host-system-meets-minimum-virtual-device-requirements"></a>Steg 1: Kontrollera värdsystem uppfyller minsta virtuell enhet
toocreate en virtuell enhet behöver du:

* Åtkomst tooa värdsystem som kör VMware ESXi Server 5.5 och senare.
* VMware vSphere-klienten på dina system toomanage hello ESXi-värd.

  * Minst 4 kärnor.
  * Minst 8 GB RAM-minne. Om du planerar tooconfigure hello virtuella matris som filserver stöder 8 GB mindre än 2 miljoner filer. Du måste 16 GB RAM-minne toosupport 2-4 miljoner filer.
  * Ett nätverksgränssnitt är anslutna till toohello kan routning trafik tooInternet. hello Internet minimibandbredd måste vara 5 Mbit/s tooallow för optimala hello enheten att fungera.
  * En virtuell disk 500 GB för data.

## <a name="step-2-provision-a-virtual-device-in-hypervisor"></a>Steg 2: Etablera en virtuell enhet i hypervisor-program
Utför följande steg tooprovision virtuella enheter i din hypervisor-programmet hello.

1. Kopiera hello virtuella enheten på datorn. Du har hämtat den här virtuella avbildningen via hello Azure-portalen.

   1. Se till att du har hämtat hello senaste avbildningsfilen. Om du har hämtat hello avbildningen tidigare hämta igen tooensure hello senaste bild. hello senaste avbildningen har två filer (i stället för en).
   2. Anteckna hello plats dit du kopierade hello bilden som du använder den här avbildningen senare i hello proceduren.

2. Logga in toohello ESXi-servern använder hello vSphere-klienten. Du måste toohave administratör privilegier toocreate en virtuell dator.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image1.png)
3. Välj hello ESXi-servern i hello vSphere-klient under hello inventering i hello till vänster.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image2.png)
4. Överför hello VMDK toohello ESXi-servern. Navigera toohello **Configuration** fliken i hello till höger. Under **maskinvara**väljer **lagring**.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image3.png)
5. I hello Högerklicka rutan under **Datastores**, Välj hello datalagret där du vill att tooupload hello VMDK. hello datalagret måste ha tillräckligt med ledigt utrymme för hello OS och datadiskar.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image4.png)
6. Högerklicka och välj **Bläddra Datastore**.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image5.png)
7. En **Datastore webbläsare** visas.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image6.png)
8. Klicka på i verktygsfältet för hello ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image7.png) ikonen toocreate en ny mapp. Ange hello mappnamn och anteckna den. Du använder den här mappnamn senare när du skapar en virtuell dator (rekommenderas för bästa praxis). Klicka på **OK**.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image8.png)
9. hello nya mappen visas i hello till vänster i hello **Datastore webbläsare**.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image9.png)
10. Klicka på ikonen för hello överför ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image10.png) och välj **överför filen**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image11.png)
11. Bläddra och toohello VMDK-filer som du hämtat. Det finns två filer. Välj en fil tooupload.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image12m.png)
12. Klicka på **öppna**. hello överföringen av hello VMDK-filen toohello angetts datastore startar. Det kan ta flera minuter för hello filen tooupload.
13. När hello överföringen är klar, visas hello-filen i hello datalagret i hello-mappen som du skapade.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image14.png)

    Ladda upp hello andra VMDK-filen toohello samma datalager.
14. Returnera toohello vSphere klienten fönster. Med ESXi-servern är markerad, högerklicka och välj **ny virtuell dator**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image15.png)
15. En **Skapa ny virtuell dator** fönster visas. På hello **Configuration** sidan, Välj hello **anpassad** alternativet. Klicka på **Nästa**.
    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image16.png)
16. På hello **namn och plats** anger hello namnet på den virtuella datorn. Det här namnet ska matcha hello (rekommenderas) det angivna mappnamnet tidigare i steg 8.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image17.png)
17. På hello **lagring** väljer du ett datalager som du vill toouse tooprovision den virtuella datorn.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image18.png)
18. På hello **virtuella Version** väljer **virtuella Version: 8**. Versioner 8 too11 stöds.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image19.png)
19. På hello **gästoperativsystemet** sidan, Välj hello **gästoperativsystemet** som **Windows**. För **Version**, hello listrutan, Välj **Microsoft Windows Server 2012 (64-bitars)**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image20.png)
20. På hello **processorer** sidan, justera hello **antalet virtuella uttag** och **antalet kärnor per virtuell socket** så som hello **Totalt antal kärnor**4 (eller mer). Klicka på **Nästa**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image21.png)
21. På hello **minne** anger 8 GB (eller mer) RAM-minne. Klicka på **Nästa**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image22.png)
22. På hello **nätverk** anger hello antalet hello nätverksgränssnitt. hello Minimikravet är ett nätverksgränssnitt.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image23.png)
23. På hello **SCSI-styrenhet** godkänner hello standard **LSI Logic SAS-styrenhet**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image24.png)
24. På hello **Välj en Disk** väljer **använder en befintlig virtuell disk**. Klicka på **Nästa**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image25.png)
25. På hello **Välj befintlig Disk** sidan under **Disk filsökväg**, klickar du på **Bläddra**. Då öppnas en **Bläddra Datastores** dialogrutan. Navigera toohello plats där du har överfört hello VMDK. Du kan nu se en enstaka fil i hello datalagret som hello två filer som du ursprungligen har överfört har slagits samman. Välj hello-filen och klicka på **OK**. Klicka på **Nästa**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image26.png)
26. På hello **avancerade alternativ** accepterar hello standard och klicka på **nästa**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image27.png)
27. På hello **klar tooComplete** granskar alla hello inställningar hello ny virtuell dator. Kontrollera **redigera hello inställningarna för virtuella datorer före slutförande**. Klicka på **fortsätta**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image28.png)
28. På hello **egenskaper för virtuella datorer** i hello sidan **maskinvara** , letar du reda hello maskinvara. Välj **ny hårddisk**. Klicka på **Lägg till**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image29.png)
29. Du ser en **Lägg till maskinvara** fönster. På hello **enhetstyp** sidan under **Välj hello typ av enhet du vill tooadd**väljer **hårddisk**, och klicka på **nästa**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image30.png)
30. På hello **Välj en Disk** väljer **skapar en ny virtuell disk**. Klicka på **Nästa**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image31.png)
31. På hello **skapar du en Disk** ändrar hello **diskstorleken** too500 GB (eller mer). 500 GB är minimikravet för hello, kan du alltid etablera en större disk. Observera att du kan öka eller minska hello-disk som etablerats en gång. Mer information om hello storleken på disken tooprovision granska hello sizing avsnitt i hello [bästa praxis dokumentet](storsimple-ova-best-practices.md). Under **Disk etablering**väljer **tunn**. Klicka på **Nästa**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image32.png)
32. På hello **avancerade alternativ** godkänner hello standard.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image33.png)
33. På hello **klar tooComplete** läser du igenom alternativen för hello-diskar. Klicka på **Slutför**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image34.png)
34. Gå tillbaka toohello egenskaper för virtuell dator. En ny hårddisk läggs tooyour virtuella datorn. Klicka på **Slutför**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image35.png)
35. Med den virtuella datorn har valts i hello högra, navigera toohello **sammanfattning** fliken. Kontrollera hello inställningarna för den virtuella datorn.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image36.png)

Den virtuella datorn har nu etablerats. hello nästa steg är toopower på den här datorn och hämta hello IP-adress.

## <a name="step-3-start-hello-virtual-device-and-get-hello-ip"></a>Steg 3: Starta hello virtuell enhet och få hello IP
Utför följande steg toostart hello din virtuella enhet och ansluta tooit.

#### <a name="toostart-hello-virtual-device"></a>toostart hello virtuell enhet
1. Starta hello virtuell enhet. Välj din enhet i hello vSphere Configuration Manager hello vänster och högerklicka på toobring in hello snabbmenyn. Välj **Power** och välj sedan **ström på**. Detta bör sätta på den virtuella datorn. Du kan visa statusen för hello i hello nedre **senaste aktiviteter** rutan av hello vSphere-klienten.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image37.png)
2. hello installationsaktiviteter tar några minuter toocomplete. När enheten hello körs kan navigera toohello **konsolen** fliken. Skicka Ctrl + Alt + Delete toolog i toohello enhet. Du kan också peka hello markören på hello konsolfönster och tryck på Ctrl + Alt + Insert. hello standardanvändaren är *StorSimpleAdmin* och hello standardlösenordet är *Password1*.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image38.png)
3. Av säkerhetsskäl upphör hello enhetens administratörslösenord vid hello första inloggningen. Du kan ange toochange hello lösenord.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image39.png)
4. Ange ett lösenord som innehåller minst 8 tecken. hello lösenordet måste innehålla 3 av 4 av dessa krav: versaler, gemener, siffror och särskilda tecken. Ange hello lösenord tooconfirm den. Du meddelas hello lösenordet har ändrats.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image40.png)
5. När hello lösenordet har ändrats hello virtuella enheten kan starta om. Vänta tills hello omstart toocomplete. hello Windows PowerShell-konsol för hello enhet visas tillsammans med en förloppsindikator.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image41.png)
6. Steg 6 – 8 gäller endast när du startar upp i en icke-DHCP-miljö. Om du har en DHCP-miljö, hoppa över de här stegen och går toostep 9. Om du har startat upp din enhet i DHCP-miljön visas hello följande skärm.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image42m.png)

   Konfigurera hello nätverk.
7. Använd hello `Get-HcsIpAddress` kommandot toolist hello nätverksgränssnitt aktiverad på din virtuella enhet. Om enheten har ett enda nätverksgränssnitt som är aktiverad, hello standardgränssnittet tilldelade toothis är `Ethernet`.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image43m.png)
8. Använd hello `Set-HcsIpAddress` cmdlet tooconfigure hello nätverk. Ett exempel visas nedan:

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image44.png)
9. När hello installationen är klar och hello enheten har startats in, visas hello banderolltext för enheten. Anteckna hello IP-adress och hello-URL som visas i hello banderoll text toomanage hello enhet. Du använder den här IP-adressen tooconnect toohello webbgränssnittet för din virtuella enhet och fullständig hello lokal installation och registrering.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image45.png)
10. (Valfritt) Utför det här steget endast om du distribuerar din enhet i hello offentliga moln. Du kan nu hello USA FIPS Federal Information Processing Standard ()-läge på enheten. hello FIPS 140-standarden definierar kryptografiska algoritmer som godkänts för användning av oss federala myndigheter datorsystem för hello skyddet av känsliga data.

    1. tooenable hello FIPS-läge, kör följande cmdlet hello:

        `Enable-HcsFIPSMode`
    2. Starta om enheten när du har aktiverat hello FIPS-läge så att hello kryptografiska verifieringar börjar gälla.

       > [!NOTE]
       > Du kan aktivera eller inaktivera FIPS-läge på enheten. Alternerande hello enhet mellan FIPS och icke-FIPS-läge stöds inte.
       >
       >

Om enheten inte uppfyller lägsta konfigurationskrav som hello, visas ett fel i hello banderolltext (se nedan). Du behöver toomodify hello enhetskonfigurationen så att det finns tillräckliga resurser toomeet hello minimikraven. Du kan sedan starta om och Anslut toohello enhet. Se toohello lägsta konfigurationskrav i [steg 1: Kontrollera att hello värdsystem uppfyller minsta virtuella enheten](#step-1-ensure-host-system-meets-minimum-virtual-device-requirements).

![](./media/storsimple-virtual-array-deploy2-provision-vmware/image46.png)

Om du står inför ett fel under hello startkonfiguration med hello lokala webbgränssnittet Se toohello följande arbetsflöden:

* Kör diagnostiktest för[felsöka installationen av UI](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).
* [Generera loggen paketet och visa loggfiler](storsimple-ova-web-ui-admin.md#generate-a-log-package).

## <a name="next-steps"></a>Nästa steg
* [Konfigurera din virtuella StorSimple-matris som en filserver](storsimple-virtual-array-deploy3-fs-setup.md)
* [Konfigurera din virtuella StorSimple-matris som en iSCSI-server](storsimple-virtual-array-deploy3-iscsi-setup.md)

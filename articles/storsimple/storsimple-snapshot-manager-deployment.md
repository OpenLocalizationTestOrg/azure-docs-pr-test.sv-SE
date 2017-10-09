---
title: aaaDeploy StorSimple Snapshot Manager | Microsoft Docs
description: "Lär dig hur toodownload och installera hello StorSimple Snapshot Manager, en snapin-modul för MMC för att hantera StorSimple skydd och säkerhetskopiering datafunktioner."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: f0128f57-519e-49ec-9187-23575809cdbe
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: dd90cca2bbb3410bb8cd459fb1a3ff3fb5f2997b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-hello-storsimple-snapshot-manager-mmc-snap-in"></a>Distribuera hello StorSimple Snapshot Manager MMC-snapin-modulen

## <a name="overview"></a>Översikt
Hej StorSimple Snapshot Manager är en snapin-modul i Microsoft Management Console (MMC) som förenklar dataskydd och hantering av säkerhetskopiering i en miljö med Microsoft Azure StorSimple. Med StorSimple Snapshot Manager som du kan hantera Microsoft Azure StorSimple lokala och molnlagring som om det vore en helt integrerad lagringssystemet därmed förenkla säkerhetskopiering och återställning och minska kostnaderna. 

Den här självstudiekursen beskriver konfigurationskrav, samt procedurer för att installera, ta bort och uppgradera StorSimple Snapshot Manager.

> [!NOTE]
> * Du kan inte använda StorSimple Snapshot Manager toomanage Microsoft Azure StorSimple virtuell matriser (även kallat StorSimple lokala virtuella enheter).
> * Om du planerar tooinstall StorSimple uppdatering 2 på StorSimple-enheten vara säker på att toodownload hello senaste versionen av StorSimple Snapshot Manager och installera den **innan du installerar StorSimple uppdatering 2**. hello senaste versionen av StorSimple Snapshot Manager är bakåtkompatibla och fungerar med alla versioner av Microsoft Azure StorSimple. Om du använder hello tidigare version av StorSimple Snapshot Manager behöver du tooupdate it (du inte behöver toouninstall hello tidigare versionen innan du installerar en ny version av hello).


## <a name="storsimple-snapshot-manager-installation"></a>Installation av StorSimple Snapshot Manager
StorSimple Snapshot Manager kan installeras på datorer som kör hello operativsystemet för Windows Server 2008 R2 SP1, Windows Server 2012 eller Windows Server 2012 R2. På servrar som kör Windows 2008 R2, måste du också installera Windows Server 2008 SP1 och Windows Management Framework 3.0.

Innan du installerar eller uppgraderar hello StorSimple Snapshot Manager snapin-modulen för hello Microsoft Management Console (MMC), kontrollera att den hello Microsoft Azure StorSimple-enheten och värdservern är korrekt konfigurerade.

## <a name="configure-prerequisites"></a>Konfigurera krav
hello ger följande steg en översikt av konfigurationsåtgärder som du måste slutföra innan du installerar hello StorSimple Snapshot Manager. Slutföra konfigurationen av Microsoft Azure StorSimple och installationsprogrammet information, inklusive systemkrav och stegvisa instruktioner finns [distribuera din lokala StorSimple-enhet](storsimple-8000-deployment-walkthrough-u2.md).

> [!IMPORTANT]
> Innan du kan granska hello [checklista för distributionskonfiguration](storsimple-8000-deployment-walkthrough-u2.md#deployment-configuration-checklist) och och [kraven för distribution av](storsimple-8000-deployment-walkthrough-u2.md#deployment-prerequisites) i [distribuera din lokala StorSimple-enhet](storsimple-8000-deployment-walkthrough-u2.md).
> <br>
> 
> 

### <a name="before-you-install-storsimple-snapshot-manager"></a>Innan du installerar StorSimple Snapshot Manager
1. Packa upp, montera och ansluta hello Microsoft Azure StorSimple-enhet som beskrivs i [installerar StorSimple 8100-enhet](storsimple-8100-hardware-installation.md) eller [installera din StorSimple-8600-enhet](storsimple-8600-hardware-installation.md).
2. Kontrollera att värddatorn kör något av följande operativsystem hello:
   
   * Windows Server 2008 R2 (på servrar som kör Windows 2008 R2, måste du också installera Windows Server 2008 SP1 och Windows Management Framework 3.0)
   * Windows Server 2012
   * Windows Server 2012 R2
     
     Hello-värden måste vara en Microsoft Azure-dator för en virtuell StorSimple-enhet.
3. Kontrollera att du uppfyller alla krav för konfiguration av hello Microsoft Azure StorSimple. Mer information går för[kraven för distribution av](storsimple-8000-deployment-walkthrough-u2.md#deployment-prerequisites).
4. Anslut hello enheten toohello värden och utför hello inledande konfiguration. Mer information går för[distributionssteg](storsimple-8000-deployment-walkthrough-u2.md#deployment-steps).
5. Skapa volymer på hello-enhet, tilldela dem toohello värden och kontrollera att hello-värd kan montera och använda dem. StorSimple Snapshot Manager stöder följande typer av volymer hello:
   
   * Enkla volymer
   * Enkla volymer
   * Dynamiska volymer
   * Dynamiska volymer (RAID 1)
   * Klusterdelade volymer
     
     Information om hur du skapar volymer på hello StorSimple-enhet eller virtuella StorSimple-enheten finns för[steg 6: skapa en volym](storsimple-8000-deployment-walkthrough-u2.md#step-6-create-a-volume)i [distribuera din lokala StorSimple-enhet](storsimple-8000-deployment-walkthrough-u2.md).

## <a name="install-a-new-storsimple-snapshot-manager"></a>Installera en ny StorSimple Snapshot Manager
Innan du installerar StorSimple Snapshot Manager, se till att hello volymer som du skapade i hello StorSimple-enhet eller virtuella StorSimple-enheten är monterad, initiera och formaterad enligt beskrivningen i [Konfigurera förutsättningar](#configure-prerequisites).

> [!IMPORTANT]
> * Hello-värden måste vara en Microsoft Azure-dator för en virtuell StorSimple-enhet. 
> * hello-värden måste köra Windows 2008 R2, Windows Server 2012 eller Windows Server 2012 R2. Om servern kör Windows Server 2008 R2, måste du också installera Windows Server 2008 SP1 och Windows Management Framework 3.0.
> * Innan du kan ansluta hello enheten tooStorSimple Snapshot Manager måste du konfigurera en iSCSI-anslutning från hello värden toohello StorSimple-enhet.

Följ dessa steg toocomplete en ny installation av StorSimple Snapshot Manager. Om du installerar en uppgradering måste gå för[uppgradera eller installera om StorSimple Snapshot Manager](#upgrade-or-reinstall-storsimple-snapshot-manager).

* Steg 1: Installera StorSimple Snapshot Manager 
* Steg 2: Anslut StorSimple Snapshot Manager tooa enhet 
* Steg 3: Kontrollera hello toohello anslutning 

### <a name="step-1-install-storsimple-snapshot-manager"></a>Steg 1: Installera StorSimple Snapshot Manager
Använd följande steg tooinstall StorSimple Snapshot Manager hello.

#### <a name="tooinstall-storsimple-snapshot-manager"></a>tooinstall StorSimple Snapshot Manager
1. Hämta hello StorSimple Snapshot Manager programvara (gå för[StorSimple Snapshot Manager](https://www.microsoft.com/download/details.aspx?id=44220) i hello Microsoft Download Center) och spara den lokalt på hello värden.
2. Högerklicka på hello komprimerad mapp i Utforskaren och klicka sedan på **extrahera alla**.
3. I hello **extrahera komprimerade (Zipped) mappar** i hello fönstret **Välj ett mål och extrahera filer** Skriv eller bläddra toohello sökväg där du vill ha toofile toobe extraheras.
   
    > [!IMPORTANT]
    > Du måste installera StorSimple Snapshot Manager på hello C:-enheten.
    
4. Välj hello **Visa extraherade filer när du är färdig** kryssrutan och klicka sedan på **extrahera**.
   
    ![Extrahera filerna i dialogrutan](./media/storsimple-snapshot-manager-deployment/HCS_SSM_extract_files.png) 
5. När hello extrahering är klar, öppnar hello målmappen. Dubbelklicka på hello programmet installationsprogrammet ikonen som visas i hello målmappen.
6. När hello **installationen lyckas** visas klickar du på **Stäng**. Du bör se hello StorSimple Snapshot Manager ikon på skrivbordet.
   
    ![skrivbordsikon](./media/storsimple-snapshot-manager-deployment/HCS_SSM_desktop_icon.png) 

### <a name="step-2-connect-storsimple-snapshot-manager-tooa-device"></a>Steg 2: Anslut StorSimple Snapshot Manager tooa enhet
Använd hello följande steg tooconnect StorSimple Snapshot Manager tooa StorSimple-enhet.

#### <a name="tooconnect-storsimple-snapshot-manager-tooa-device"></a>tooconnect StorSimple Snapshot Manager tooa enhet
1. Klicka på hello StorSimple Snapshot Manager ikon på skrivbordet. Hej öppnas StorSimple Snapshot Manager. hello fönstret innehåller en **omfång** fönstret en **resultat** rutan och en **åtgärder** fönstret. 
   
    ![Användargränssnittet för StorSimple Snapshot Manager](./media/storsimple-snapshot-manager-deployment/HCS_SSM_gui_panes.png)
   
   * Hej **omfång** fönstret (hello till vänster) innehåller en lista över ordnade i en trädstruktur noder. Du kan expandera vissa noder tooselect en vy eller specifika data relaterade toothat nod. Klicka på hello pilen ikonen tooexpand eller komprimera en nod. Högerklicka på ett objekt i hello **omfång** fönstret toosee en lista över tillgängliga åtgärder för objektet.
   * Hej **resultat** fönstret (hello mellersta rutan) innehåller detaljerad statusinformation om hello nod, visa eller data som du valde i hello **omfång** fönstret.
   * Hej **åtgärder** hello-åtgärder som du kan utföra på hello nod, visa eller data som du valde i hello-fönsterrutan **omfång** fönstret.
     
     En fullständig beskrivning av hello StorSimple Snapshot Manager-användargränssnittet, se [StorSimple Snapshot Manager användargränssnittet](storsimple-use-snapshot-manager.md).
2. I hello **omfång** fönstret, högerklicka på hello **enheter** noden och klicka sedan på **konfigurera en enhet**. Hej **konfigurera en enhet** dialogrutan visas.
   
    ![Konfigurera en enhet](./media/storsimple-snapshot-manager-deployment/HCS_SSM_config_device.png) 
3. I hello **enhet** listan rutan, Välj hello IP-adressen för hello Microsoft Azure StorSimple-enhet eller virtuell enhet. I hello **lösenord** textruta, ange hello StorSimple Snapshot Manager ett lösenord som du skapade för hello-enhet i hello Azure-portalen. Klicka på **OK**.
4. StorSimple Snapshot Manager söker efter hello-enhet som du har identifierats. Om hello-enhet är tillgänglig, StorSimple Snapshot Manager lägger till en anslutning. Du kan [Kontrollera hello anslutningsenhet toohello](#to-verify-the-connection) tooconfirm som hello anslutning har lagts till.
   
    Om hello enheten inte är tillgänglig av någon anledning, returnerar StorSimple Snapshot Manager ett felmeddelande. Klicka på **OK** tooclose hello felmeddelande och klicka sedan på **Avbryt** tooclose hello **konfigurera en enhet** dialogrutan.
5. När den ansluter tooa enheten importerar StorSimple Snapshot Manager varje volym-grupp som konfigurerats för denna enhet, förutsatt att hello volym grupp har associerade säkerhetskopior. Volymen grupper som inte har tillhörande säkerhetskopior har inte importerats. Dessutom har principer för säkerhetskopiering som har skapats för en grupp för volymen inte importerats. toosee Hej importerade grupper, högerklicka på hello översta **volym grupper** nod i hello **omfång** rutan och klicka på **växla importerade grupperna**.

### <a name="step-3-verify-hello-connection-toohello-device"></a>Steg 3: Kontrollera hello toohello anslutning
Använd hello följande steg tooverify att StorSimple Snapshot Manager är anslutna toohello StorSimple-enhet.

#### <a name="tooverify-hello-connection"></a>tooverify hello-anslutning
1. I hello **omfång** rutan klickar du på hello **enheter** nod.
   
    ![Status för StorSimple Snapshot Manager](./media/storsimple-snapshot-manager-deployment/HCS_SSM_Device_status.png) 
2. Kontrollera hello **resultat** fönstret: 
   
   * Om en grön indikator visas på hello enhetens ikon och **tillgänglig** visas i hello **Status** kolumn, och sedan hello-enhet är ansluten. 
   * Om en röd indikator visas på hello enhetens ikon och inte tillgänglig i hello **Status** kolumn, och sedan hello-enheten är inte ansluten. 
   * Om **uppdaterar** visas i hello **Status** kolumn, och sedan StorSimple Snapshot Manager hämtar volym grupper och tillhörande säkerhetskopior för en ansluten enhet.

## <a name="upgrade-or-reinstall-storsimple-snapshot-manager"></a>Uppgradera eller installera om StorSimple Snapshot Manager
Du bör avinstallera StorSimple Snapshot Manager helt innan du uppgraderar eller installerar om hello programvara. 

Säkerhetskopiera hello befintlig StorSimple Snapshot Manager-databas på värddatorn för hello innan du installerar om StorSimple Snapshot Manager. Detta sparar hello säkerhetskopiering principerna och konfigurationen information så att du lätt kan återställa data från en säkerhetskopia.

Följ dessa steg om du uppgraderar eller installerar om StorSimple Snapshot Manager:

* Steg 1: Avinstallera StorSimple Snapshot Manager 
* Steg 2: Säkerhetskopiera hello StorSimple Snapshot Manager-databasen 
* Steg 3: Installera om StorSimple Snapshot Manager och återställa hello-databas 

### <a name="step-1-uninstall-storsimple-snapshot-manager"></a>Steg 1: Avinstallera StorSimple Snapshot Manager
Använd följande steg toouninstall StorSimple Snapshot Manager hello.

#### <a name="toouninstall-storsimple-snapshot-manager"></a>toouninstall StorSimple Snapshot Manager
1. Öppna hello på hello värddatorn **Kontrollpanelen**, klickar du på **program**, och klicka sedan på **program och funktioner**.
2. Hello vänster klickar du på **avinstallera eller ändra ett program**.
3. Högerklicka på **StorSimple Snapshot Manager**, och klicka sedan på **avinstallera**.
4. Detta startar hello StorSimple Snapshot Manager-installationsprogrammet. Klicka på **ändra installationen**, och klicka sedan på **avinstallera**.
   
   > [!NOTE]
   > Om det finns några MMC-processer som körs i bakgrunden hello, till exempel StorSimple Snapshot Manager eller Diskhantering, hello avinstallationen misslyckas och du får ett meddelande tooclose försöker alla instanser av MMC innan du toouninstall hello program. Välj **automatiskt Stäng programmen och försök toorestart dem när installationen är slutföra**, och klicka sedan på **OK**.
   > 
   > 
5. När hello avinstallerar processen är klar visas en **installationen lyckas** meddelandet visas. Klicka på **Stäng**.

### <a name="step-2-back-up-hello-storsimple-snapshot-manager-database"></a>Steg 2: Säkerhetskopiera hello StorSimple Snapshot Manager-databasen
Använd följande steg toocreate hello och spara en kopia av hello StorSimple Snapshot Manager-databasen.

#### <a name="tooback-up-hello-database"></a>tooback hello-databas
1. Stoppa hello Microsoft StorSimple Management-tjänsten:
   
   1. Starta Serverhanteraren.
   2. På hello instrumentpanelen Serverhanteraren på hello **verktyg** väljer du **Services**.
   3. På hello **Services** väljer **Management-tjänsten för Microsoft StorSimple**.
   4. I hello Högerklicka rutan under **Management-tjänsten för Microsoft StorSimple**, klickar du på **stoppa hello tjänsten**.
      
        ![Stoppa hello StorSimple Device Manager-tjänsten](./media/storsimple-snapshot-manager-deployment/HCS_SSM_stop_service.png)
2. Bläddra tooC:\ProgramData\Microsoft\StorSimple\BACatalog. 
   
   > [!NOTE]
   > ProgramData är en dold mapp.
  
3. Hitta hello katalog XML-filen och kopiera hello fil lagra hello kopia på en säker plats eller i hello molnet.
   
    ![StorSimple säkerhetskopiering katalogfil](./media/storsimple-snapshot-manager-deployment/HCS_SSM_bacatalog.png)
4. Starta om hello Microsoft StorSimple Management-tjänsten: 
   
   1. På hello instrumentpanelen Serverhanteraren på hello **verktyg** väljer du **Services**.
   2. På hello **Services** sidan, Välj hello **Microsoft StorSimple Management Servic**e.
   3. I hello Högerklicka rutan under **Management-tjänsten för Microsoft StorSimple**, klickar du på **starta om tjänsten hello**. 

### <a name="step-3-reinstall-storsimple-snapshot-manager-and-restore-hello-database"></a>Steg 3: Installera om StorSimple Snapshot Manager och återställa hello-databas
tooreinstall StorSimple Snapshot Manager gör hello i [installera en ny StorSimple Snapshot Manager](#install-a-new-storsimple-snapshot-manager). Använd sedan hello följa proceduren toorestore hello StorSimple Snapshot Manager-databasen.

#### <a name="toorestore-hello-database"></a>toorestore hello-databas
1. Stoppa hello Microsoft StorSimple Management-tjänsten:
   
   1. Starta Serverhanteraren.
   2. På hello instrumentpanelen Serverhanteraren på hello **verktyg** väljer du **Services**.
   3. På hello **Services** väljer **Management-tjänsten för Microsoft StorSimple**.
   4. I hello Högerklicka rutan under **Management-tjänsten för Microsoft StorSimple**, klickar du på **stoppa hello tjänsten**.
2. Bläddra tooC:\ProgramData\Microsoft\StorSimple\BACatalog.
   
   > [!NOTE]
   > ProgramData är en dold mapp.
   > 
   > 
3. Ta bort hello katalog XML-filen och Ersätt den med hello-version som du sparade tidigare.
4. Starta om hello Microsoft StorSimple Management-tjänsten: 
   
   1. På hello instrumentpanelen Serverhanteraren på hello **verktyg** väljer du **Services**.
   2. På hello **Services** väljer **Management-tjänsten för Microsoft StorSimple**.
   3. I hello Högerklicka rutan under **Management-tjänsten för Microsoft StorSimple**, klickar du på **starta om tjänsten hello**.

## <a name="next-steps"></a>Nästa steg
* toolearn mer om StorSimple Snapshot Manager, gå för[vad är StorSimple Snapshot Manager?](storsimple-what-is-snapshot-manager.md).
* toolearn mer information om användargränssnittet för hello StorSimple Snapshot Manager gå för[StorSimple Snapshot Manager användargränssnittet](storsimple-use-snapshot-manager.md).
* toolearn mer information om hur du använder StorSimple Snapshot Manager gå för[Använd StorSimple Snapshot Manager tooadminister StorSimple-lösningen](storsimple-snapshot-manager-admin.md).


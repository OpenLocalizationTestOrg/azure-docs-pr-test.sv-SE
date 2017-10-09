---
title: "aaaManage Azure Backup-valv och servrar Azure använder hello klassiska distributionsmodellen | Microsoft Docs"
description: "Använd den här självstudiekursen toolearn hur toomanage Azure Backup-valv och servrar."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: tysonn
ms.assetid: f175eb12-0905-437f-91fd-eaee03ab6e81
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: markgal;
ms.openlocfilehash: 6c38b04f4a76604bfd639a9b2d58237ce44e2386
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-backup-vaults-and-servers-using-hello-classic-deployment-model"></a>Hantera Azure-säkerhetskopieringsvalv och servrar med hjälp av hello klassiska distributionsmodellen
> [!div class="op_single_selector"]
> * [Resource Manager](backup-azure-manage-windows-server.md)
> * [Klassisk](backup-azure-manage-windows-server-classic.md)
>
>

I den här artikeln hittar du en översikt över hello säkerhetskopiering hanteringsuppgifter tillgängliga via hello klassiska Azure-portalen och hello Microsoft Azure Backup agent.

> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.

> [!IMPORTANT]
> Nu kan du uppgradera ditt valv tooRecovery Services säkerhetskopieringsvalv. Mer information finns i artikeln hello [uppgradera en Backup-valvet tooa Recovery Services-valvet](backup-azure-upgrade-backup-to-recovery-services.md). Microsoft rekommenderar att du tooupgrade din säkerhetskopieringsvalv tooRecovery Services-valv.<br/> Du kan inte använda PowerShell toocreate säkerhetskopieringsvalv efter 15 oktober 2017. **Från den 1 november 2017**:
>- Alla återstående säkerhetskopieringsvalv blir automatiskt uppgraderade tooRecovery Services-valv.
>- Du kommer inte att kunna tooaccess dina säkerhetskopierade data i hello klassiska portalen. Använd i stället hello Azure portal tooaccess dina säkerhetskopierade data i Recovery Services-valv.
>

## <a name="management-portal-tasks"></a>Portalen hanteringsuppgifter
1. Logga in toohello [hanteringsportalen](https://manage.windowsazure.com).
2. Klicka på **återställningstjänster**, klickar sedan på säkerhetskopieringsvalvet tooview hello snabbstartsidan hello namn.

    ![Återställningstjänster](./media/backup-azure-manage-windows-server-classic/rs-left-nav.png)

Du kan se hello tillgängliga hanteringsuppgifter genom att välja hello alternativ hello överst i hello snabbstartsidan.

![Hantera flikar](./media/backup-azure-manage-windows-server-classic/qs-page.png)

### <a name="dashboard"></a>Instrumentpanel
Välj **instrumentpanelen** toosee hello användningsöversikten för hello-servern. Hej **användningsöversikten** omfattar:

* hello antal Windows-servrar registrerade toocloud
* hello antalet virtuella datorer i Azure skyddad i molnet
* hello totalt lagringsutrymme som förbrukas i Azure
* hello status för senaste jobb

Längst ned hello hello instrumentpanelen kan du utföra följande uppgifter hello:

* **Hantera certifikat** – om ett certifikat har använt tooregister hello server och sedan använda det här tooupdate hello certifikatet. Om du använder autentiseringsuppgifter för valv kan inte använda **hantera certifikat**.
* **Ta bort** -borttagningar hello aktuella säkerhetskopieringsvalvet. Om ett säkerhetskopieringsvalv används inte längre kan radera du den toofree upp lagringsutrymme. **Ta bort** aktiveras först när alla registrerade servrar har tagits bort från hello-valvet.

![Säkerhetskopiering instrumentpanelsuppgifter](./media/backup-azure-manage-windows-server-classic/dashboard-tasks.png)

## <a name="registered-items"></a>Registrerade artiklar
Välj **registrerade objekt** tooview hello namnen på hello-servrar som är registrerade toothis valvet.

![Registrerade artiklar](./media/backup-azure-manage-windows-server-classic/registered-items.png)

Hej **typen** filter som standard tooAzure virtuella datorn. tooview hello namnen på hello-servrar som är registrerade toothis valvet, Välj **Windows server** från hello nedrullningsbara meny.

Härifrån kan du utföra följande uppgifter hello:

* **Tillåt omregistrering** – när det här alternativet väljs för en server som du kan använda hello **Registreringsguiden** i hello lokala Microsoft Azure Backup agent tooregister hello server med hello säkerhetskopieringsvalvet en gång . Du kan behöva toore-registret på grund av tooan fel i hello certifikat eller om en server som hade toobe återskapas.
* **Ta bort** -tar bort en server från hello säkerhetskopieringsvalvet. Alla hello lagras data som associeras med hello servern tas bort omedelbart.

    ![Registrerade artiklar uppgifter](./media/backup-azure-manage-windows-server-classic/registered-items-tasks.png)

## <a name="protected-items"></a>Skyddade objekt
Välj **skyddade objekt** tooview hello objekt som har säkerhetskopierats från hello-servrar.

![Skyddade objekt](./media/backup-azure-manage-windows-server-classic/protected-items.png)

## <a name="configure"></a>Konfigurera
Från hello **konfigurera** fliken som du kan välja hello lämpliga redundans lagringsalternativ. hello visas bästa tooselect hello lagring redundans direkt när du har skapat ett valv och alla datorer som är registrerade tooit.

> [!WARNING]
> När ett objekt har varit registrerade toohello valvet, hello lagringsalternativ för redundans är låst och kan inte ändras.
>
>

![Konfigurera](./media/backup-azure-manage-windows-server-classic/configure.png)

Se den här artikeln för mer information om [lagring redundans](../storage/common/storage-redundancy.md).

## <a name="microsoft-azure-backup-agent-tasks"></a>Microsoft Azure Backup agentuppgifter
### <a name="console"></a>Konsolen
Öppna hello **Microsoft Azure Backup-agenten** (du kan hitta den genom att söka igenom din dator för *Microsoft Azure Backup*).

![Backup-agenten](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)

Från hello **åtgärder** på hello höger i hello säkerhetskopieringsagent konsolen kan du utföra följande hanteringsuppgifter hello:

* Registrera Server
* Schema för säkerhetskopiering
* Säkerhetskopiera nu
* Ändra egenskaper för

![Konsolen agentåtgärder](./media/backup-azure-manage-windows-server-classic/console-actions.png)

> [!NOTE]
> för**återställa Data**, se [återställa filer tooa Windows server eller Windows-klientdatorn](backup-azure-restore-windows-server.md).
>
>

### <a name="modify-an-existing-backup"></a>Ändra en befintlig säkerhetskopia
1. Klicka på hello Microsoft Azure Backup agent **schemalägga säkerhetskopiering**.

    ![Schemalägga en Windows Serverbackup](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)
2. I hello **schema Säkerhetskopieringsguiden** lämna hello **göra ändringar toobackup objekt eller gånger** alternativ som valts och klickar på **nästa**.

    ![Ändra en schemalagd säkerhetskopiering](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)
3. Om du vill tooadd eller ändra objekt på hello **Välj objekt tooBackup** skärmen klickar du på **Lägg till objekt**.

    Du kan också ange **Undantagsinställningar** från den här sidan i guiden hello. Om du vill tooexclude filer eller filtyper läsa hello proceduren för att lägga till [undantagsinställningar](#exclusion-settings).
4. Välj hello filer och mappar som du vill att tooback och klicka på **okej**.

    ![Lägg till artiklar](./media/backup-azure-manage-windows-server-classic/add-items-modify.png)
5. Ange hello **Säkerhetskopieringsschemat** och på **nästa**.

    Du kan schemalägga varje dag (högst 3 gånger per dag) eller veckovisa säkerhetskopior.

    ![Ange schema för säkerhetskopiering](./media/backup-azure-manage-windows-server-classic/specify-backup-schedule-modify-close.png)

   > [!NOTE]
   > Ange schemat för säkerhetskopiering av hello beskrivs i detalj i detta [artikel](backup-azure-backup-cloud-as-tape.md).
   >
   >
6. Välj hello **bevarandeprincip** för hello säkerhetskopia och klickar på **nästa**.

    ![Välj din bevarandeprincip](./media/backup-azure-manage-windows-server-classic/select-retention-policy-modify.png)
7. På hello **bekräftelse** skärmen granska hello information och klicka på **Slutför**.
8. När hello guiden är färdig med hello **Säkerhetskopieringsschemat**, klickar du på **Stäng**.

    Ändrar skydd måste du bekräfta att säkerhetskopieringar utlöser korrekt genom att gå toohello **jobb** fliken och bekräftar att ändringarna visas i hello säkerhetskopiera jobb.

### <a name="enable-network-throttling"></a>Aktivera begränsning
hello Azure Backup-agenten ger en begränsning flik där du toocontrol hur nätverksbandbredden används när data överfördes. Den här kontrollen kan vara användbart om du behöver tooback upp data under arbetstid men inte vill att hello säkerhetskopieringsprocessen toointerfere med andra Internettrafik. Begränsning av data transfer gäller tooback upp och återställa aktiviteter.  

tooenable begränsning:

1. I hello **säkerhetskopieringsagenten**, klickar du på **ändra egenskaper för**.
2. Välj hello **aktivera Användningsbegränsning för internetbandbredd för säkerhetskopieringsåtgärder** kryssrutan.

    ![Nätverksbegränsning](./media/backup-azure-manage-windows-server-classic/throttling-dialog.png)
3. När du har aktiverat begränsning, ange hello tillåtet bandbredd för överföring av säkerhetskopierade data under **arbetstid** och **icke-arbetstid**.

    hello bandbredd värden börjar på 512 kilobit per sekund (Kbps) och gå upp too1023 megabyte per sekund (Mbps). Du kan också ange hello start och avslutas för **arbetstid**, och vilka dagar i veckan för hello betraktas arbete dagar. hello är utanför hello avses arbetstid övervägt toobe icke-arbetstid.
4. Klicka på **OK**.

## <a name="exclusion-settings"></a>Undantagsinställningar
1. Öppna hello **Microsoft Azure Backup-agenten** (du kan hitta den genom att söka igenom din dator för *Microsoft Azure Backup*).

    ![Öppna säkerhetskopieringsagent](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)
2. Klicka på hello Microsoft Azure Backup agent **schemalägga säkerhetskopiering**.

    ![Schemalägga en Windows Serverbackup](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)
3. Lämna hello i hello schema Säkerhetskopieringsguiden **göra ändringar toobackup objekt eller gånger** alternativ som valts och klickar på **nästa**.

    ![Ändra ett schema](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)
4. Klicka på **undantag inställningar**.

    ![Välj objekt tooexclude](./media/backup-azure-manage-windows-server-classic/exclusion-settings.png)
5. Klicka på **Lägg till undantag**.

    ![Lägg till undantag](./media/backup-azure-manage-windows-server-classic/add-exclusion.png)
6. Välj hello plats och klicka sedan på **OK**.

    ![Välj en plats för undantag](./media/backup-azure-manage-windows-server-classic/exclusion-location.png)
7. Lägga till filnamnstillägget hello i hello **filtyp** fältet.

    ![Exkludera efter filtyp](./media/backup-azure-manage-windows-server-classic/exclude-file-type.png)

    Lägger till en .mp3-tillägg

    ![Exempel på en typ.](./media/backup-azure-manage-windows-server-classic/exclude-mp3.png)

    tooadd ett annat filtillägg klickar du på **Lägg till undantag** och ange en annan filtyp (lägga till filnamnstillägget .jpeg).

    ![Exempel på en annan typ](./media/backup-azure-manage-windows-server-classic/exclude-jpg.png)
8. När du har lagt till alla hello-tillägg, klickar du på **OK**.
9. Fortsätter med hello schema för Säkerhetskopieringsguiden genom att klicka på **nästa** tills hello **bekräftelsesidan**, klicka på **Slutför**.

    ![Bekräftelse av undantag](./media/backup-azure-manage-windows-server-classic/finish-exclusions.png)

## <a name="next-steps"></a>Nästa steg
* [Återställa Windows Server eller Windows-klienten från Azure](backup-azure-restore-windows-server.md)
* toolearn mer om Azure Backup finns [översikt över Azure-säkerhetskopiering](backup-introduction-to-azure-backup.md)
* Besök hello [Azure Backup-forumet](http://go.microsoft.com/fwlink/p/?LinkId=290933)

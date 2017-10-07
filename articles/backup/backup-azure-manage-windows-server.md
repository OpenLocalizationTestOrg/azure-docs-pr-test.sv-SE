---
title: aaaManage Azure recovery services-valv och servrar | Microsoft Docs
description: "Använd den här självstudiekursen toolearn hur toomanage Azure recovery services-valv och servrar."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: tysonn
ms.assetid: 4eea984b-7ed6-4600-ac60-99d2e9cb6d8a
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: markgal
ms.openlocfilehash: b4c35c86faa0828b3c63a13b85c095c0cbaba50e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-manage-azure-recovery-services-vaults-and-servers-for-windows-machines"></a>Övervaka och hantera Azure Recovery Services-valv och servrar för Windows-datorer
> [!div class="op_single_selector"]
> * [Resource Manager](backup-azure-manage-windows-server.md)
> * [Klassisk](backup-azure-manage-windows-server-classic.md)
>
>

I den här artikeln hittar du en översikt över hello säkerhetskopiering Övervakare och hanteringsuppgifter som är tillgängliga via hello Azure-portalen och hello Microsoft Azure Backup agent. Den här artikeln förutsätter att du redan har en Azure-prenumeration och har skapat minst ett Recovery Services-valvet.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]


## <a name="open-a-recovery-services-vault"></a>Öppna ett Recovery Services-valv

hello Recovery Services-valvet instrumentpanelen visar hello information eller attribut för Recovery Services-valvet.

1. Logga in toohello [Azure Portal](https://portal.azure.com/) med din Azure-prenumeration.
2. Hej hubbmenyn, klicka på **fler tjänster**.

    ![Öppna en lista över Recovery Services-valv steg 1](./media/backup-azure-manage-windows-server/open-rs-vault-list.png) <br/>

3. Vill du tooopen Recovery Services-valvet. I dialogrutan för hello börja skriva **återställningstjänster**. När du börjar skriva in hello listan filtreras baserat på dina indata. Klicka på **Recovery Services-valv** toodisplay hello lista över Recovery Services-valv i din prenumeration.

    ![Skapa Recovery Services-valv (steg 1)](./media/backup-azure-manage-windows-server/browse-to-rs-vaults-2.png) <br/>

    hello lista över Recovery Services-valv öppnas.

    ![Skapa Recovery Services-valv (steg 1)](./media/backup-azure-manage-windows-server/list-of-rs-vaults.png) <br/>

4. Hello lista över valv, Välj hello namnet på hello Recovery Services-valv som du vill tooopen. hello Recovery Services-valvet instrumentpanelen blad öppnas.

    ![Recovery services-valvet instrumentpanelen](./media/backup-azure-manage-windows-server/rs-vault-blade.png) <br/>

    Nu när du har öppnat hello Recovery Services-valvet, försöker du med hello övervakning eller hantering av uppgifter.

## <a name="monitor-backup-jobs-and-alerts"></a>Övervakaren säkerhetskopieringsjobb och -varningar

Du övervaka jobb och aviseringar från hello återställningstjänster valvet instrumentpanel, där du ser:

* Information om aviseringar om säkerhetskopiering
* Filer och mappar samt Azure virtuella datorer som skyddas i hello moln
* Totalt lagringsutrymme som förbrukas i Azure
* Status för säkerhetskopieringsjobb

![Säkerhetskopiering instrumentpanelsuppgifter](./media/backup-azure-manage-windows-server/dashboard-tiles.png)

Om du klickar på hello informationen i dessa brickor öppnas hello associerade bladet där du hanterar relaterade aktiviteter.

Från hello överkant hello instrumentpanelen:

* Inställningar som ger åtkomst tillgängliga aktiviteter för säkerhetskopiering.
* Backup - hjälper till att du säkerhetskopierar nya filer och mappar (eller virtuella Azure-datorer) toohello Recovery Services-valvet.
* Ta bort – om en recovery services-valvet används inte längre kan du kan ta bort den toofree upp lagringsutrymme. Ta bort aktiveras först när alla skyddade servrar har tagits bort från hello-valvet.

![Säkerhetskopiering instrumentpanelsuppgifter](./media/backup-azure-manage-windows-server/dashboard-tasks.png)

## <a name="alerts-for-backups-using-azure-backup-agent"></a>Aviseringar för säkerhetskopiering med Azure backup-agenten:
| Aviseringsnivå | Aviseringar skickas |
| --- | --- |
| kritiska |Säkerhetskopieringen har misslyckats, Återställningsfel |
| Varning |Slutfördes med varningar (när färre än 100 filerna säkerhetskopieras inte på grund av problem med toocorruption och mer än en miljon filer har säkerhetskopierats) |
| Information |Ingen |

## <a name="manage-backup-alerts"></a>Hantera aviseringar för säkerhetskopiering
Klicka på hello **säkerhetskopiering aviseringar** panelen tooopen hello **säkerhetskopiering aviseringar** bladet och hantera aviseringar.

![Aviseringar om säkerhetskopiering](./media/backup-azure-manage-windows-server/manage-backup-alerts.png)

hello säkerhetskopiering aviseringar innehåller hello antalet:

* kritiska aviseringar som inte matchas med de senaste 24 timmarna
* varningsaviseringar som inte matchas med de senaste 24 timmarna

Klicka på var och en av dessa länkar tar toohello **säkerhetskopiering aviseringar** bladet med en filtrerad vy av aviseringarna (kritiskt eller varning).

Hello säkerhetskopiering aviseringar bladet du:

* Välj hello lämplig information tooinclude med aviseringarna.

    ![Välj kolumner](./media/backup-azure-manage-windows-server/choose-alerts-colunms.png)
* Filtrera aviseringar efter allvarlighetsgrad, status och börja/sluta tider.

    ![Filtrera aviseringar](./media/backup-azure-manage-windows-server/filter-alerts.png)
* Konfigurera aviseringar för allvarlighetsgrad, frekvens och mottagare, samt aktivera och inaktivera aviseringar.

    ![Filtrera aviseringar](./media/backup-azure-manage-windows-server/configure-notifications.png)

Om **Per avisering** är markerad som hello **Avisera** frekvens ingen gruppering eller minskning av e-post inträffar. Alla aviseringar som resulterar i 1-meddelande. Detta är standardinställningen för hello och hello upplösning e-postmeddelande skickas också omedelbart.

Om **timvis sammanfattad** är markerad som hello **Avisera** frekvens som en e-postmeddelande skickas toohello användare som talar om att det inte finns olöst nya aviseringar som genererats i hello senaste timmen. En lösning i e-postmeddelande skickas slutet hello hello timme.

Aviseringar kan skickas för hello följande allvarlighetsgrader:

* kritiska
* Varning
* Information

Du inaktiverar hello avisering med hello **inaktivera** knapp i hello informationsbladet för jobbet. När du klickar på Inaktivera, kan du ange upplösning anteckningar.

Du väljer hello kolumner du vill tooappear som en del av hello avisering med hello **Välj kolumner** knappen.

> [!NOTE]
> Från hello **inställningar** bladet du hanterar aviseringar om säkerhetskopiering genom att välja **övervakning och rapporter > aviseringar och händelser > Säkerhetskopieringsaviseringar** och sedan klicka på **Filter** eller  **Konfigurera meddelanden**.
>
>

## <a name="manage-backup-items"></a>Hantera säkerhetskopiering objekt
Hantera lokala säkerhetskopiorna är nu tillgänglig i hello-hanteringsportalen. Under hello säkerhetskopiering av hello instrumentpanelen hello **Säkerhetskopieringsobjekt** panelen visar hello antalet Säkerhetskopiera objekt skyddade toohello valvet.

Klicka på **-mappar** i hello panelen Backup-objekt.

![Säkerhetskopiera objekt sida vid sida](./media/backup-azure-manage-windows-server/backup-items-tile.png)

hello Säkerhetskopieringsobjekt öppnas i blad med hello filtrera uppsättningen tooFile-mappen där du vill se varje säkerhetskopieringen post i listan.

![Säkerhetskopiera objekt](./media/backup-azure-manage-windows-server/backup-item-list.png)

Om du väljer ett specifikt objekt för säkerhetskopiering hello listan finns hello viktig information för objektet.

> [!NOTE]
> Från hello **inställningar** bladet du hanterar filer och mappar genom att välja **skyddade objekt > säkerhetskopiering objekt** och sedan välja **-mappar** från hello nedrullningsbara meny.
>
>

![Säkerhetskopiera objekt från inställningar](./media/backup-azure-manage-windows-server/backup-files-and-folders.png)

## <a name="manage-backup-jobs"></a>Hantera säkerhetskopieringsjobb
Säkerhetskopieringsjobb för både lokala (när hello lokal server säkerhetskopierar tooAzure) och Azure-säkerhetskopieringar visas i hello instrumentpanelen.

I hello säkerhetskopiering avsnittet hello instrumentpanelen visar hello säkerhetskopiering jobbet panelen hello antalet jobb:

* pågår
* kunde inte hello senaste 24 timmarna.

toomanage säkerhetskopieringsjobb, klicka på hello **säkerhetskopieringsjobb** panelen, vilket öppnar bladet för hello säkerhetskopieringsjobb.

![Säkerhetskopiera objekt från inställningar](./media/backup-azure-manage-windows-server/backup-jobs.png)

Du ändrar hello informationen i hello säkerhetskopieringsjobb bladet med hello **Välj kolumner** knappen hello överst på hello sidan.

Använd hello **Filter** knappen tooselect mellan filer och mappar och säkerhetskopiering av Azure virtuella datorer.

Om du inte ser den säkerhetskopierade filer och mappar, klickar du på **Filter** knappen hello överst i hello sida och välj **filer och mappar** hello objekttypen-menyn.

> [!NOTE]
> Från hello **inställningar** bladet du hantera säkerhetskopieringsjobb genom att välja **övervakning och rapporter > jobb > säkerhetskopieringsjobb** och sedan välja **-mappar** hello listrutan meny.
>
>

## <a name="monitor-backup-usage"></a>Övervaka användningen av säkerhetskopiering
I hello säkerhetskopiering avsnittet hello instrumentpanelen visar ikonen för användning i säkerhetskopieringen av hello hello lagringsutrymme som förbrukas i Azure. Lagringskvoten tillhandahålls för:

* Lagringskvoten på molnet LRS som är associerade med hello-valvet
* Lagringskvoten på molnet GRS som är associerade med hello-valvet

## <a name="manage-your-production-servers"></a>Hantera produktionsservrarna
toomanage produktionsservrarna, klicka på **inställningar**.

Klicka på under hantera **säkerhetskopiering infrastruktur > produktionsservrar**.

hello produktionsservrar bladet en lista över alla tillgängliga produktionsservrar. Klicka på en server i hello listan tooopen hello serverinformation.

![Skyddade objekt](./media/backup-azure-manage-windows-server/production-server-list.png)


## <a name="open-hello-azure-backup-agent"></a>Öppna hello Azure Backup-agenten
Öppna hello **Microsoft Azure Backup-agenten** (du hittar det genom att söka igenom din dator för *Microsoft Azure Backup*).

![Schemalägga en Windows Serverbackup](./media/backup-azure-manage-windows-server/snap-in-search.png)

Från hello **åtgärder** på hello höger i hello säkerhetskopieringsagent konsolen du utföra följande hanteringsuppgifter hello:

* Registrera Server
* Schema för säkerhetskopiering
* Säkerhetskopiera nu
* Ändra egenskaper för

![Microsoft Azure Backup agent konsolen åtgärder](./media/backup-azure-manage-windows-server/console-actions.png)

> [!NOTE]
> för**återställa Data**, se [återställa filer tooa Windows server eller Windows-klientdatorn](backup-azure-restore-windows-server.md).
>
>

## <a name="modify-hello-backup-schedule"></a>Ändra schemat för säkerhetskopiering av hello
1. Klicka på hello Microsoft Azure Backup agent **schemalägga säkerhetskopiering**.

    ![Schemalägga en Windows Serverbackup](./media/backup-azure-manage-windows-server/schedule-backup.png)
2. I hello **schema Säkerhetskopieringsguiden** lämna hello **göra ändringar toobackup objekt eller gånger** alternativ som valts och klickar på **nästa**.

    ![Schemalägga en Windows Serverbackup](./media/backup-azure-manage-windows-server/modify-or-stop-a-scheduled-backup.png)
3. Om du vill tooadd eller ändra objekt på hello **Välj objekt tooBackup** skärmen klickar du på **Lägg till objekt**.

    Du kan också ange **Undantagsinställningar** från den här sidan i guiden hello. Om du vill tooexclude filer eller filtyper läsa hello proceduren för att lägga till [undantagsinställningar](#manage-exclusion-settings).
4. Välj hello filer och mappar som du vill att tooback och klicka på **okej**.

    ![Schemalägga en Windows Serverbackup](./media/backup-azure-manage-windows-server/add-items-modify.png)
5. Ange hello **Säkerhetskopieringsschemat** och på **nästa**.

    Du kan schemalägga varje dag (högst 3 gånger per dag) eller veckovisa säkerhetskopior.

    ![Windows Server Backup-objekt](./media/backup-azure-manage-windows-server/specify-backup-schedule-modify-close.png)

   > [!NOTE]
   > Ange schemat för säkerhetskopiering av hello beskrivs i detalj i detta [artikel](backup-azure-backup-cloud-as-tape.md).
   >

6. Välj hello **bevarandeprincip** för hello säkerhetskopia och klickar på **nästa**.

    ![Windows Server Backup-objekt](./media/backup-azure-manage-windows-server/select-retention-policy-modify.png)
7. På hello **bekräftelse** skärmen granska hello information och klicka på **Slutför**.
8. När hello guiden är färdig med hello **Säkerhetskopieringsschemat**, klickar du på **Stäng**.

    Ändrar skydd måste du bekräfta att säkerhetskopieringar utlöser korrekt genom att gå toohello **jobb** fliken och bekräftar att ändringarna visas i hello säkerhetskopiera jobb.

## <a name="enable-network-throttling"></a>Aktivera begränsning

hello Azure Backup-agenten ger en begränsning flik där du toocontrol hur nätverksbandbredden används när data överfördes. Den här kontrollen kan vara användbart om du behöver tooback upp data under arbetstid men inte vill att hello säkerhetskopieringsprocessen toointerfere med andra Internettrafik. Begränsning av data transfer gäller tooback upp och återställa aktiviteter.  

tooenable begränsning:

1. I hello **säkerhetskopieringsagenten**, klickar du på **ändra egenskaper för**.
2. På hello ** begränsning fliken Välj **aktivera Användningsbegränsning för internetbandbredd för säkerhetskopieringsåtgärder**.

    ![Nätverksbegränsning](./media/backup-azure-manage-windows-server/throttling-dialog.png)

    När du har aktiverat begränsning, ange hello tillåtet bandbredd för överföring av säkerhetskopierade data under **arbetstid** och **icke-arbetstid**.

    hello bandbredd värden börjar på 512 kilobit per sekund (Kbps) och gå upp too1023 megabyte per sekund (Mbps). Du kan också ange hello start och avslutas för **arbetstid**, och vilka dagar i veckan för hello betraktas arbete dagar. hello är utanför hello avses arbetstid övervägt toobe icke-arbetstid.
3. Klicka på **OK**.

## <a name="manage-exclusion-settings"></a>Hantera undantagsinställningar
1. Öppna hello **Microsoft Azure Backup-agenten** (du kan hitta den genom att söka igenom din dator för *Microsoft Azure Backup*).

    ![Schemalägga en Windows Serverbackup](./media/backup-azure-manage-windows-server/snap-in-search.png)
2. Klicka på hello Microsoft Azure Backup agent **schemalägga säkerhetskopiering**.

    ![Schemalägga en Windows Serverbackup](./media/backup-azure-manage-windows-server/schedule-backup.png)
3. Lämna hello i hello schema Säkerhetskopieringsguiden **göra ändringar toobackup objekt eller gånger** alternativ som valts och klickar på **nästa**.

    ![Schemalägga en Windows Serverbackup](./media/backup-azure-manage-windows-server/modify-or-stop-a-scheduled-backup.png)
4. Klicka på **undantag inställningar**.

    ![Schemalägga en Windows Serverbackup](./media/backup-azure-manage-windows-server/exclusion-settings.png)
5. Klicka på **Lägg till undantag**.

    ![Schemalägga en Windows Serverbackup](./media/backup-azure-manage-windows-server/add-exclusion.png)
6. Välj hello plats och klicka sedan på **OK**.

    ![Schemalägga en Windows Serverbackup](./media/backup-azure-manage-windows-server/exclusion-location.png)
7. Lägga till filnamnstillägget hello i hello **filtyp** fältet.

    ![Schemalägga en Windows Serverbackup](./media/backup-azure-manage-windows-server/exclude-file-type.png)

    Lägger till en .mp3-tillägg

    ![Schemalägga en Windows Serverbackup](./media/backup-azure-manage-windows-server/exclude-mp3.png)

    tooadd ett annat filtillägg klickar du på **Lägg till undantag** och ange en annan filtyp (lägga till filnamnstillägget .jpeg).

    ![Schemalägga en Windows Serverbackup](./media/backup-azure-manage-windows-server/exclude-jpg.png)
8. När du har lagt till alla hello-tillägg, klickar du på **OK**.
9. Fortsätter med hello schema för Säkerhetskopieringsguiden genom att klicka på **nästa** tills hello **bekräftelsesidan**, klicka på **Slutför**.

    ![Schemalägga en Windows Serverbackup](./media/backup-azure-manage-windows-server/finish-exclusions.png)

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar
**F1. hello säkerhetskopieringsjobbet status visas som slutförda i hello Azure backup-agenten, varför inte den hämta visas omedelbart i portal?**

S1. Det visas på Maximal fördröjning på 15 minuter mellan hello säkerhetskopieringsjobbet status i hello Azure backup-agenten och hello Azure-portalen.

**Q.2 när ett säkerhetskopieringsjobb misslyckas, hur lång tid tar det tooraise en avisering?**

A.2 en avisering utlöses inom 20 minuter för hello Azure säkerhetskopieringen har misslyckats.

**KV. 3. Finns det fall där inte ett e-postmeddelande skickas om meddelanden har konfigurerats?**

S3. Nedan visas hello fall när hello-meddelande inte skickas i ordning tooreduce hello avisering brus:

* Om meddelanden konfigureras varje timme och en avisering aktiveras som lösts inom hello timme
* Jobbet har avbrutits.
* Andra säkerhetskopieringsjobbet misslyckades eftersom ursprungliga säkerhetskopiering pågår.

## <a name="troubleshooting-monitoring-issues"></a>Felsökning av problem med övervakning
**Problem:** jobb och/eller varningar från hello Azure Backup-agenten visas inte i hello-portalen.

**Felsökningssteg:** hello processen ```OBRecoveryServicesManagementAgent```, skickar hello jobbet och avisering data toohello Azure Backup-tjänsten. Ibland den här processen kan fastna eller avstängning.

1. tooverify hello processen körs inte, öppna **Aktivitetshanteraren** och kontrollera om hello ```OBRecoveryServicesManagementAgent``` processen körs.
2. Förutsatt att hello processen inte körs, öppna **Kontrollpanelen** och bläddra hello lista över tjänster. Starta eller starta om **Microsoft Azure Recovery Services-hanteringsagenten**.

    Bläddra hello loggar vid ytterligare information:<br/>
   `<AzureBackup_agent_install_folder>\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider*`Exempel:<br/>
   `C:\Program Files\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider0.errlog`

## <a name="next-steps"></a>Nästa steg
* [Återställa Windows Server eller Windows-klienten från Azure](backup-azure-restore-windows-server.md)
* toolearn mer om Azure Backup finns [översikt över Azure-säkerhetskopiering](backup-introduction-to-azure-backup.md)
* Besök hello [Azure Backup-forumet](http://go.microsoft.com/fwlink/p/?LinkId=290933)

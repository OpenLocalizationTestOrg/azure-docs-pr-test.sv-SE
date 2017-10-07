---
title: aaaBack in Windows filer och mappar tooAzure (Resource Manager) | Microsoft Docs
description: "Lär dig tooback in Windows-filer och mappar tooAzure i en Resource Manager distribution."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "hur toobackup; hur tooback. Säkerhetskopiera filer och mappar"
ms.assetid: 5b15ebf1-2214-4722-b937-96e2be8872bb
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 8/15/2017
ms.author: markgal;
ms.openlocfilehash: 07d6580a84d5092ed2c61bf86ff5fcb148423ef2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="first-look-back-up-files-and-folders-in-resource-manager-deployment"></a>En första titt: Säkerhetskopiera filer och mappar med Resource Manager-distributions
Den här artikeln förklarar hur tooback in Windows Server (eller Windows-dator) filer och mappar tooAzure med hjälp av en Resource Manager distribution. Det är en självstudiekurs avsedda toowalk du via hello grunderna. Om du vill tooget igång med Azure Backup är hello rätt plats.

Om du vill tooknow mer om Azure Backup kan läsa det här [översikt](backup-introduction-to-azure-backup.md).

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/) som ger dig åtkomst till Azure-tjänsten.

## <a name="create-a-recovery-services-vault"></a>Skapa ett Recovery Services-valv
tooback upp filer och mappar måste toocreate Recovery Services-valvet i hello region där du vill toostore hello data. Du måste också toodetermine du hur replikerade lagringen.

### <a name="toocreate-a-recovery-services-vault"></a>toocreate Recovery Services-valvet
1. Om du inte redan har gjort det loggar du in toohello [Azure Portal](https://portal.azure.com/) med din Azure-prenumeration.
2. Hej hubbmenyn, klicka på **fler tjänster** och Skriv i hello lista över resurser, **återställningstjänster** och på **Recovery Services-valv**.

    ![Skapa Recovery Services-valv (steg 1)](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    Om det finns recovery services-valv i hello prenumeration, visas hello valv.
3. På hello **Recovery Services-valv** -menyn klickar du på **Lägg till**.

    ![Skapa Recovery Services-valv (steg 2)](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

    hello återställningstjänster valvet blad öppnas, där du uppmanas tooprovide en **namn**, **prenumeration**, **resursgruppen**, och **plats**.

    ![Skapa Recovery Services-valv (steg 3)](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

4. För **namn**, ange ett eget namn tooidentify hello valv. hello namn måste toobe unika för hello Azure-prenumeration. Skriv ett namn som innehåller mellan 2 och 50 tecken. Det måste börja med en bokstav och får endast innehålla bokstäver, siffror och bindestreck.

5. I hello **prenumeration** Använd hello nedrullningsbara menyn toochoose hello Azure-prenumeration. Om du använder bara en prenumeration som prenumeration visas och du kan hoppa över toohello nästa steg. Om du inte är säker på vilken prenumeration toouse använder standard-hello (eller förslag) prenumerationen. Du kan bara välja mellan flera alternativ om ditt organisationskonto är associerat med flera Azure-prenumerationer.

6. I hello **resursgruppen** avsnitt:

    * Välj **Skapa nytt** om du vill toocreate en ny resursgrupp.
    Eller
    * Välj **använda befintliga** och klicka på hello nedrullningsbara menyn toosee hello tillgängliga listan över resursgrupper.

  Mer information om resursgrupper finns i hello [översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

7. Klicka på **plats** tooselect hello geografiskt område för hello-valvet. Det här alternativet anger hello geografiska region där dina säkerhetskopierade data skickas.

8. Hello längst ned på bladet för hello Recovery Services-valvet, klickar du på **skapa**.

    Det kan ta flera minuter för hello Recovery Services-valvet toobe skapas. Övervaka hello statusmeddelanden i hello övre högra delen av hello-portalen. När din valvet har skapats visas den i hello lista över Recovery Services-valv. Om du inte ser ditt valv efter ett par minuter klickar du på **Uppdatera**.

    ![Klicka på Uppdatera](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

    När du ser ditt valv i hello lista över Recovery Services-valv, är du redo tooset hello lagring redundans.

### <a name="set-storage-redundancy-for-hello-vault"></a>Ange lagringsredundans för hello valvet
När du skapar ett Recovery Services-valv, kontrollera att lagring redundans är konfigurerade hello som du vill.

1. Från hello **Recovery Services-valv** bladet Klicka hello nya valvet.

    ![Välj hello nytt valv hello listan över Recovery Services-valvet](./media/backup-try-azure-backup-in-10-mins/rs-vault-list.png)

    När du väljer hello valvet hello **Recovery Services-valvet** bladet begränsar och hello inställningsbladet (*som har hello namnet på valvet hello överst hello*) och hello valvet informationsbladet öppna.

    ![Visa hello lagringskonfigurationen för nytt valv](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration-2.png)
2. Använda hello lodräta bilden tooscroll ned toohello hantera avsnitt i hello nytt valv inställningar-bladet och klickar på **säkerhetskopiering infrastruktur**.
    hello säkerhetskopiering infrastruktur blad öppnas.
3. I hello säkerhetskopiering infrastruktur-bladet, klickar du på **konfigurering av säkerhetskopiering** tooopen hello **konfigurering av säkerhetskopiering** bladet.

    ![Ange hello lagringskonfigurationen för nytt valv](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration.png)
4. Välj hello lämpliga replikering lagringsalternativ för ditt valv.

    ![alternativ för lagringskonfiguration](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

    Valvet använder geo-redundant lagring som standard. Om du använder Azure som en primär säkerhetskopieringslagring slutpunkt fortsätta toouse **Geo-redundant**. Om du inte använder Azure som en primär säkerhetskopieringslagring slutpunkt, Välj **lokalt redundant**, vilket minskar hello Azure lagringskostnader. Läs mer om alternativen för [geo-redundant](../storage/common/storage-redundancy.md#geo-redundant-storage) och [lokalt redundant](../storage/common/storage-redundancy.md#locally-redundant-storage) i denna [översikt av lagringsredundans](../storage/common/storage-redundancy.md).

Nu när du har skapat ett valv konfigurerar du det för säkerhetskopiering av filer och mappar.

## <a name="configure-hello-vault"></a>Konfigurera hello valvet
1. Hej Recovery Services-valvet bladet (för hello valvet du just skapade), i hello komma igång-avsnittet, klickar du på **säkerhetskopiering**, sedan på hello **Kom igång med säkerhetskopiering** bladet väljer  **Säkerhetskopiering målet**.

    ![Öppna bladet för säkerhetskopieringsmål](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

    Hej **säkerhetskopiering målet** blad öppnas.

    ![Öppna bladet för säkerhetskopieringsmål](./media/backup-try-azure-backup-in-10-mins/backup-goal-blade.png)

2. Från hello **var körs din arbetsbelastning?** nedrullningsbara menyn och väljer **lokalt**.

    Du väljer **Lokalt** eftersom din Windows Server- eller Windows-dator är en fysisk dator som inte finns i Azure.

3. Från hello **vad vill du vill toobackup?** väljer du **filer och mappar**, och klicka på **OK**.

    ![Konfigurera filer och mappar](./media/backup-try-azure-backup-in-10-mins/set-file-folder.png)

    När du klickar på OK, visas en markering nästa för**säkerhetskopiering målet**, och hello **Förbered infrastruktur** blad öppnas.

    ![När säkerhetskopieringsmålet har konfigurerats går du vidare och förbereder infrastrukturen](./media/backup-try-azure-backup-in-10-mins/backup-goal-configed.png)

4. På hello **Förbered infrastruktur** bladet, klickar du på **ladda ned Agent för Windows Server och Windows-klient**.

    ![förbereda infrastrukturen](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

    Om du använder Windows Server viktigt, och välj sedan toodownload hello agent för Windows Server viktigt. Ett popup-menyn efterfrågar toorun eller spara MARSAgentInstaller.exe.

    ![Dialogrutan MARSAgentInstaller](./media/backup-try-azure-backup-in-10-mins/mars-installer-run-save.png)

5. I hello hämta popup-menyn, klickar du på **spara**.

    Som standard hello **MARSagentinstaller.exe** tooyour hämtningsmapp sparas i filen. När hello installer är klar visas ett popup-fönster som frågar om du vill toorun hello installer eller öppna hello mapp.

    ![förbereda infrastrukturen](./media/backup-try-azure-backup-in-10-mins/mars-installer-complete.png)

    Du behöver inte tooinstall hello agenten ännu. Du kan installera hello agenten när du har hämtat hello valvautentiseringsuppgifter.

6. På hello **Förbered infrastruktur** bladet, klickar du på **hämta**.

    ![hämta autentiseringsuppgifter för valvet](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

    Hej valvautentiseringsuppgifter hämta tooyour hämtningsmapp. När hello valvautentiseringsuppgifter nedladdning, visas ett popup-fönster som frågar om du vill tooopen eller spara hello autentiseringsuppgifter. Klicka på **Spara**. Om du råkar klicka på **öppna**, låta hello dialogruta som försöker tooopen hello valvautentiseringsuppgifter, misslyckas. Du kan inte öppna hello valvautentiseringsuppgifter. Fortsätt toohello nästa steg. Hej valvautentiseringsuppgifter finns i hello hämtningsmapp.   

    ![valvautentiseringsuppgifterna har hämtats](./media/backup-try-azure-backup-in-10-mins/vault-credentials-downloaded.png)

## <a name="install-and-register-hello-agent"></a>Installera och registrera hello-agent

> [!NOTE]
> Aktivera säkerhetskopiering via hello Azure-portalen är inte tillgängligt ännu. Använd hello Microsoft Azure Recovery Services-agenten tooback upp filer och mappar.
>

1. Leta upp och dubbelklicka på hello **MARSagentinstaller.exe** från hello hämtningar mappen (eller andra lagringsplatsen).

    hello installer innehåller ett antal meddelanden som extraheras, installerar och registrerar hello Recovery Services-agenten.

    ![Autentiseringsuppgifter för att köra Recovery Services-agentinstallationsprogrammet](./media/backup-try-azure-backup-in-10-mins/mars-installer-registration.png)

2. Slutför hello installationsguiden för Microsoft Azure Recovery Services-agenten. toocomplete hello guiden måste du:

   * Välj en plats för hello installation och cache-mappen.
   * Ange proxyserveradressen serverinformation om du använder en proxy server tooconnect toohello internet.
   * Ange ditt användarnamn och lösenord om du använder en autentiserad proxyserver.
   * Ange hello ned valvautentiseringsuppgifter
   * Spara hello krypteringslösenfrasen på en säker plats.

     > [!NOTE]
     > Om du förlorar eller glömmer hello lösenfras går inte Microsoft att återställa hello säkerhetskopierade data. Spara hello-filen på en säker plats. Det är obligatoriskt toorestore en säkerhetskopia.
     >
     >

hello-agent har installerats och datorn är registrerade toohello valvet. Du är klar tooconfigure och schemalägger säkerhetskopieringen.

## <a name="network-and-connectivity-requirements"></a>Nätverks- och anslutningskrav

Om din dator/proxy har begränsad tillgång till internet, kan du kontrollera att brandväggen på hello mcahine/proxy är konfigurerade tooallow hello följande URL: er: <br>
    1. www.msftncsi.com
    2. *.Microsoft.com
    3. *.WindowsAzure.com
    4. *.microsoftonline.com
    5. *.windows.ne

## <a name="back-up-your-files-and-folders"></a>Säkerhetskopiera dina filer och mappar
hello första säkerhetskopian innehåller två viktiga uppgifter:

* Schemalägga hello säkerhetskopiering
* Säkerhetskopiera filer och mappar för hello första gången

toocomplete hello första säkerhetskopiering, Använd hello Microsoft Azure Recovery Services-agenten.

### <a name="tooschedule-hello-backup-job"></a>tooschedule hello säkerhetskopieringsjobb
1. Öppna hello Microsoft Azure Recovery Services-agenten. Du hittar den genom att söka efter **Microsoft Azure Backup** på datorn.

    ![Starta hello Azure Recovery Services-agenten](./media/backup-try-azure-backup-in-10-mins/snap-in-search.png)
2. Klicka på hello Recovery Services-agenten **schemalägga säkerhetskopiering**.

    ![Schemalägga en Windows Server-säkerhetskopiering](./media/backup-try-azure-backup-in-10-mins/schedule-first-backup.png)
3. På hello komma igång-sidan hello guiden för Schemalägg säkerhetskopiering klickar du på **nästa**.
4. På hello Välj objekt tooBackup klickar du på **Lägg till objekt**.
5. Välj hello filer och mappar som du vill att tooback och klicka sedan på **okej**.
6. Klicka på **Nästa**.
7. På hello **ange schemat för säkerhetskopiering** anger hello **Säkerhetskopieringsschemat** och på **nästa**.

    Du kan schemalägga säkerhetskopieringar varje dag (högst tre gånger om dagen) eller varje vecka.

    ![Objekt för Windows Server-säkerhetskopiering](./media/backup-try-azure-backup-in-10-mins/specify-backup-schedule-close.png)

   > [!NOTE]
   > Mer information om hur toospecify hello schemat för säkerhetskopiering finns hello artikel [Använd Azure Backup tooreplace infrastrukturen band](backup-azure-backup-cloud-as-tape.md).
   >

8. På hello **Välj bevarandeprincip** sidan, Välj hello **bevarandeprincip** hello säkerhetskopian.

    hello bevarandeprincip anger hur länge hello säkerhetskopierade data lagras. Du kan ange olika bevarandeprinciper baserat på när hello säkerhetskopia i stället för att ange en ”platt policy” för alla säkerhetskopiering punkterna. Du kan ändra hello dagliga, veckovisa, månatliga och årliga Kvarhållningsintervall principer toomeet dina behov.
9. Välj hello inledande säkerhetskopieringstyp hello på sidan Välj typ av inledande säkerhetskopiering. Lämna alternativet hello **automatiskt över hello nätverk** markerad och klicka sedan på **nästa**.

    Du kan säkerhetskopiera automatiskt över hello nätverk eller du kan säkerhetskopiera offline. hello resten av den här artikeln beskrivs hello säkerhetskopiera automatiskt. Om du föredrar toodo en offlinesäkerhetskopiering granska hello artikel [Offline säkerhetskopiering arbetsflödet i Azure Backup](backup-azure-backup-import-export.md) för ytterligare information.
10. På sidan Bekräfta hello granska hello information och klicka sedan på **Slutför**.
11. När hello guiden är färdig med hello schemat för säkerhetskopiering, klickar du på **Stäng**.

### <a name="tooback-up-files-and-folders-for-hello-first-time"></a>tooback av filer och mappar för hello första gången
1. Klicka på hello Recovery Services-agenten **Säkerhetskopiera nu** toocomplete hello inledande seeding hello nätverket.

    ![Säkerhetskopiera Windows Server nu](./media/backup-try-azure-backup-in-10-mins/backup-now.png)
2. På sidan Bekräfta hello använder hello granskningsinställningar som hello tillbaka nu guiden Installera tooback upp hello-datorn. Klicka på **Säkerhetskopiera**.
3. Klicka på **Stäng** tooclose hello guiden. Om du stänger guiden hello innan hello säkerhetskopieringen är klar fortsätter hello guiden toorun i hello bakgrund.

När hello första säkerhetskopieringen har slutförts hello **jobbet slutfört** status visas i konsolen för hello-säkerhetskopiering.

![IR slutfört](./media/backup-try-azure-backup-in-10-mins/ircomplete.png)

## <a name="questions"></a>Frågor?
Om du har frågor eller om det inte finns någon funktion som du vill att toosee ingår, [skicka feedback](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Nästa steg
* Få mer information om hur du [säkerhetskopierar Windows-datorer](backup-configure-vault.md).
* Nu när du har säkerhetskopierat dina filer och mappar kan du [hantera dina valv och servrar](backup-azure-manage-windows-server.md).
* Om du behöver toorestore en säkerhetskopia kan använda den här artikeln för[återställa filer tooa Windows datorn](backup-azure-restore-windows-server.md).

---
title: "aaaBack in Windows system tillstånd tooAzure | Microsoft Docs"
description: "Lär dig tooback in hello systemtillståndet för Windows Server och/eller tooAzure för Windows-datorer."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: carmonm
editor: 
keywords: "hur toobackup; hur tooback. Säkerhetskopiera filer och mappar"
ms.assetid: 5b15ebf1-2214-4722-b937-96e2be8872bb
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: saurse;markgal
ms.openlocfilehash: be5d4be81af981c10de82add9fe962a730753cf5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-windows-system-state-in-resource-manager-deployment"></a>Säkerhetskopiera systemtillståndet för Windows i Resource Manager-distribution
Den här artikeln förklarar hur tooback in Windows Server-systemet tillstånd tooAzure. Det är en självstudiekurs avsedda toowalk du via hello grunderna.

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

    * Välj **Skapa nytt** om du vill toocreate en resursgrupp.
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

Nu när du har skapat ett valv, kan du konfigurera den för att säkerhetskopiera systemtillståndet i Windows.

## <a name="configure-hello-vault"></a>Konfigurera hello valvet
1. Hej Recovery Services-valvet bladet (för hello valvet du just skapade), i hello komma igång-avsnittet, klickar du på **säkerhetskopiering**, sedan på hello **Kom igång med säkerhetskopiering** bladet väljer  **Säkerhetskopiering målet**.

    ![Öppna bladet för säkerhetskopieringsmål](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

    Hej **säkerhetskopiering målet** blad öppnas.

    ![Öppna bladet för säkerhetskopieringsmål](./media/backup-try-azure-backup-in-10-mins/backup-goal-blade.png)

2. Från hello **var körs din arbetsbelastning?** nedrullningsbara menyn och väljer **lokalt**.

    Du väljer **Lokalt** eftersom din Windows Server- eller Windows-dator är en fysisk dator som inte finns i Azure.

3. Från hello **vad vill du vill toobackup?** väljer du **systemtillstånd**, och klicka på **OK**.

    ![Konfigurera filer och mappar](./media/backup-azure-system-state/backup-goal-system-state.png)

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
> Aktivera säkerhetskopiering via hello Azure-portalen är inte tillgängligt ännu. Använd hello Microsoft Azure Recovery Services-agenten tooback status för Windows Server-System.
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

## <a name="back-up-windows-server-system-state-preview"></a>Säkerhetskopiera systemtillståndet för Windows Server (förhandsgranskning)
hello första säkerhetskopian innehåller tre uppgifter:

* Aktivera säkerhetskopiering av systemtillstånd med hello Azure Backup-agenten
* Schemalägga hello säkerhetskopiering
* Säkerhetskopiera filer och mappar för hello första gången

toocomplete hello första säkerhetskopiering, Använd hello Microsoft Azure Recovery Services-agenten.

### <a name="tooenable-system-state-backup-using-hello-azure-backup-agent"></a>säkerhetskopian av systemtillstånd tooenable med hello Azure Backup-agenten

1. Kör hello efter kommandot toostop hello Azure Backup-motorn i en PowerShell-session.

  ```
  PS C:\> Net stop obengine
  ```

2. Öppna hello Windows-registret.

  ```
  PS C:\> regedit.exe
  ```

3. Lägg till följande registernyckel med hello hello angetts DWord-värde.

  | Sökväg i registret | Registernyckel | DWord-värde |
  |---------------|--------------|-------------|
  | HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider | TurnOffSSBFeature | 2 |

4. Starta om hello säkerhetskopiering motorn genom att köra följande kommando i en upphöjd kommandotolk hello.

  ```
  PS C:\> Net start obengine
  ```

### <a name="tooschedule-hello-backup-job"></a>tooschedule hello säkerhetskopieringsjobb

1. Öppna hello Microsoft Azure Recovery Services-agenten. Du hittar den genom att söka efter **Microsoft Azure Backup** på datorn.

    ![Starta hello Azure Recovery Services-agenten](./media/backup-try-azure-backup-in-10-mins/snap-in-search.png)

2. Klicka på hello Recovery Services-agenten **schemalägga säkerhetskopiering**.

    ![Schemalägga en Windows Server-säkerhetskopiering](./media/backup-try-azure-backup-in-10-mins/schedule-first-backup.png)

3. På hello komma igång-sidan hello guiden för Schemalägg säkerhetskopiering klickar du på **nästa**.

4. På hello Välj objekt tooBackup klickar du på **Lägg till objekt**.

5. Välj **systemtillstånd** och klicka sedan på **OK**.

6. Klicka på **Nästa**.

7. hello schema för säkerhetskopiering av systemtillstånd och förvaring automatiskt tooback in varje söndag vid 21:00:00 lokal tid, och hello kvarhållningsperiod anges too60 dagar.

   > [!NOTE]
   > Princip för säkerhetskopiering och kvarhållning av systemtillståndet konfigureras automatiskt. Om du säkerhetskopierar filer och mappar dessutom toohello systemtillstånd för Windows Server, ange bara hello-säkerhetskopiering och kvarhållning princip för säkerhetskopior av filer från hello guiden. 
   >

8. På sidan Bekräfta hello granska hello information och klicka sedan på **Slutför**.

9. När hello guiden är färdig med hello schemat för säkerhetskopiering, klickar du på **Stäng**.

### <a name="tooback-up-windows-server-system-state-for-hello-first-time"></a>tooback Windows Server System tillstånd för hello första gången

1. Kontrollera att det finns inga väntande uppdateringar för Windows Server som kräver en omstart.

2. Klicka på hello Recovery Services-agenten **Säkerhetskopiera nu** toocomplete hello inledande seeding hello nätverket.

    ![Säkerhetskopiera Windows Server nu](./media/backup-try-azure-backup-in-10-mins/backup-now.png)

3. På sidan Bekräfta hello använder hello granskningsinställningar som hello tillbaka nu guiden Installera tooback upp hello-datorn. Klicka på **Säkerhetskopiera**.

4. Klicka på **Stäng** tooclose hello guiden. Om du stänger guiden hello innan hello säkerhetskopieringen är klar fortsätter hello guiden toorun i hello bakgrund.

5. Om du säkerhetskopierar filer och mappar på din server dessutom status för Windows-System för toohello, kommer hello säkerhetskopiering nu guiden bara säkerhetskopiera filer. tooperform en ad hoc-systemtillstånd säkerhetskopiera använder hello följande PowerShell-kommando:

    ```
    PS C:\> Start-OBSystemStateBackup
    ```

  När hello första säkerhetskopieringen har slutförts hello **jobbet slutfört** status visas i konsolen för hello-säkerhetskopiering.

  ![IR slutfört](./media/backup-try-azure-backup-in-10-mins/ircomplete.png)

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar

hello följande frågor och svar ger ytterligare information.

### <a name="what-is-hello-staging-volume"></a>Vad är hello mellanlagring volymen?

hello mellanlagring volym representerar hello mellanliggande plats där hello inbyggt, Windows Server Backup skapar etapper hello säkerhetskopia av systemtillståndet. Azure Backup-agenten och komprimerar och krypterar mellanliggande säkerhetskopian och skickar den via säker HTTPS-protokollet toohello konfigurerad Recovery Services-valvet. **Vi rekommenderar starkt att du etablerar hello mellanlagring volymen i en Windows OS-volym. Om du upptäcker problem med säkerhetskopiering av systemtillståndet, kontrollerar hello platsen för mellanlagring volymen är hello första felsökning.** 

### <a name="how-can-i-change-hello-staging-volume-path-specified-in-hello-azure-backup-agent"></a>Hur kan jag ändra hello mellanlagring volymsökväg som anges i hello Azure Backup-agenten?

hello mellanlagring volymen finns som standard i hello cachemappen. 

1. toochange den här platsen, Använd hello följande kommando (i en upphöjd kommandotolk):
  ```
  PS C:\> Net stop obengine
  ```

2. Uppdatera hello följande registervärden med hello toohello nya mellanlagring volym sökväg.

  |Sökväg i registret|Registernyckel|Värde|
  |-------------|------------|-----|
  |HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider | SSBStagingPath | nya mellanlagringsplatsen volym |

hello mellanlagring sökväg är skiftlägeskänsligt och måste vara exakt samma skiftläge hello som det som finns på hello-servern. 

3. Starta om hello säkerhetskopiering motorn när du har ändrat hello mellanlagring Volymsökväg:
  ```
  PS C:\> Net start obengine
  ```
4. toopick hello ändras sökvägen, öppna hello Microsoft Azure Recovery Services-agenten och utlösa en ad hoc-säkerhetskopiering av systemtillstånd.

### <a name="why-is-hello-system-state-default-retention-set-too60-days"></a>Varför är hello systemtillstånd Standardkvarhållning ställa in too60 dagar?

en säkerhetskopia av systemtillståndet hello livslängd är hello samma som hello ”tombstone-livslängden” för hello Windows Server Active Directory-rollen. hello standardvärdet för posten för hello tombstone-livslängden är 60 dagar. Det här värdet kan ställas in på hello konfigurationsobjekt för Directory Service (NTDS).

### <a name="how-do-i-change-hello-default-backup-and-retention-policy-for-system-state"></a>Hur ändrar jag hello standard säkerhetskopiering och bevarandeprincip för systemtillstånd?

toochange hello standard säkerhetskopiering och bevarandeprincip för systemtillstånd:
1. Stoppa hello säkerhetskopiering motorn. Kör följande kommando från en upphöjd kommandotolk hello.

  ```
  PS C:\> Net stop obengine
  ```

2. Lägg till eller uppdatera hello följande poster för registernyckel i HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider.

  |Namn i registret|Beskrivning|Värde|
  |-------------|-----------|-----|
  |SSBScheduleTime|Använda tooconfigure hello tid för hello säkerhetskopiering. Standardvärdet är 21: 00 lokaltid.|DWord: Formatet HHMM (decimalt) till exempel 2130 för 9:30:00 lokal tid|
  |SSBScheduleDays|Använda tooconfigure hello dagar när systemtillstånd måste utföras på hello angetts tid. Enskilda siffror ange hello veckodagar. 0 representerar söndag, 1 är måndag och så vidare. Standard dag för säkerhetskopiering är söndag.|DWord: dagarnas hello vecka toorun säkerhetskopiering (decimalt), till exempel 1230 schemalägger säkerhetskopieringar på måndag, tisdag, onsdag och söndag.|
  |SSBRetentionDays|Använda tooconfigure hello dagar tooretain säkerhetskopiering. Standardvärdet är 60. Högsta tillåtna värdet är 180.|DWord: Dagar tooretain säkerhetskopiering (decimalt).|

3. Använd hello följande kommando toorestart hello säkerhetskopieringen.
    ```
    PS C:\> Net start obengine
    ```

4. Öppna hello Microsoft Recovery Services-agenten.

5. Klicka på **schemalägga säkerhetskopiering** och klicka sedan på **nästa** tills du ser hello ändringarna återges.

6. Klicka på **Slutför** tooapply hello ändringar.


## <a name="questions"></a>Frågor?
Om du har frågor eller om det inte finns någon funktion som du vill att toosee ingår, [skicka feedback](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Nästa steg
* Få mer information om hur du [säkerhetskopierar Windows-datorer](backup-configure-vault.md).
* Nu när du har säkerhetskopierat dina filer och mappar kan du [hantera dina valv och servrar](backup-azure-manage-windows-server.md).
* Om du behöver toorestore en säkerhetskopia kan använda den här artikeln för[återställa filer tooa Windows datorn](backup-azure-restore-windows-server.md).

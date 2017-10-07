---
title: aaaBack in Windows server eller workstation tooAzure (klassiska modellen) | Microsoft Docs
description: "Säkerhetskopiera Windows-servrar eller klienter tooa säkerhetskopieringsvalvet i Azure. Gå igenom grunderna för att skydda filer och mappar tooa Backup-valvet via hello Azure Backup-agenten."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "säkerhetskopieringsvalvet; Säkerhetskopiera en Windows-server. Säkerhetskopiera windows;"
ms.assetid: 3b543bfd-8978-4f11-816a-0498fe14a8ba
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: c8f2a9bed1e5885f5c272c65b9282ededc05850a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-a-windows-server-or-workstation-tooazure-using-hello-classic-portal"></a>Säkerhetskopiera en Windows-server eller workstation-tooAzure med hello klassiska portalen
> [!div class="op_single_selector"]
> * [Klassisk portal](backup-configure-vault-classic.md)
> * [Azure Portal](backup-configure-vault.md)
>
>

Den här artikeln innehåller hello procedurer du behöver toofollow tooprepare din miljö och säkerhetskopiering av en Windows server (eller workstation) tooAzure. Den omfattar också överväganden för distribution av din lösning för säkerhetskopiering. Om du är intresserad av försök Azure Backup för hello första gången den här artikeln guidar snabbt dig igenom processen hello.

Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: Resource Manager och klassisk. Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.

## <a name="before-you-start"></a>Innan du börjar
tooback konfigurerar en server eller klient tooAzure du behöver ett Azure-konto. Om du inte har någon, kan du skapa en [kostnadsfritt konto](https://azure.microsoft.com/free/) på bara några minuter.

## <a name="create-a-backup-vault"></a>Skapa ett säkerhetskopieringsvalv
tooback av filer och mappar från en server eller klient behöver du toocreate ett säkerhetskopieringsvalv i hello geografiska region där du vill toostore hello data.

> [!IMPORTANT]
> Från mars 2017 kan använda du inte längre hello klassiska portal toocreate säkerhetskopieringsvalv.
>
> Nu kan du uppgradera ditt valv tooRecovery Services säkerhetskopieringsvalv. Mer information finns i artikeln hello [uppgradera en Backup-valvet tooa Recovery Services-valvet](backup-azure-upgrade-backup-to-recovery-services.md). Microsoft rekommenderar att du tooupgrade din säkerhetskopieringsvalv tooRecovery Services-valv.<br/> **15 oktober 2017**, kommer du inte längre att kunna toouse PowerShell toocreate säkerhetskopieringsvalv. <br/> **Från den 1 november 2017**:
>- Alla återstående säkerhetskopieringsvalv blir automatiskt uppgraderade tooRecovery Services-valv.
>- Du kommer inte att kunna tooaccess dina säkerhetskopierade data i hello klassiska portalen. Använd i stället hello Azure portal tooaccess dina säkerhetskopierade data i Recovery Services-valv.
>


## <a name="download-hello-vault-credential-file"></a>Hämta valvautentiseringsfilen hello
hello lokal dator måste autentiseras med ett säkerhetskopieringsvalv innan den kan säkerhetskopiera data tooAzure toobe. hello autentisering uppnås genom *valvet autentiseringsuppgifter*. Hej valvautentiseringsfilen hämtas via en säker kanal från hello klassiska portalen. hello certifikatets privata nyckel inte har behållits i hello portal eller hello-tjänsten.

### <a name="toodownload-hello-vault-credential-file-tooa-local-machine"></a>toodownload hello valvet autentiseringsuppgifter filen tooa lokal dator
1. Hello vänstra navigeringsfönstret, klicka på **återställningstjänster**, och välj sedan hello säkerhetskopieringsvalv som du skapade.

    ![IR slutfört](./media/backup-configure-vault-classic/rs-left-nav.png)
2. På hello Snabbstart klickar du på **ladda ned valvautentiseringsuppgifter**.

   hello klassiska portalen genererar en valvautentiseringen med hjälp av en kombination av hello valvnamnet och hello aktuellt datum. Hej valvautentiseringsfilen används endast under hello arbetsflöde för registrering och upphör att gälla efter 48 timmar.

   Hej valvautentiseringsfilen kan hämtas från hello-portalen.
3. Klicka på **spara** toodownload hello valvet autentiseringsuppgifter filen toohello mapp för nedladdningar av hello lokalt konto. Du kan också välja **Spara som** från hello **spara** menyn toospecify en plats för hello valvautentiseringsfilen.

   > [!NOTE]
   > Kontrollera att hello valvautentiseringsfilen sparas på en plats som kan nås från datorn. Om den är lagrad i en fil resurs eller server message block, kontrollerar du att hello behörigheter tooaccess den.
   >
   >

## <a name="download-install-and-register-hello-backup-agent"></a>Hämta, installera och registrera hello Backup-agenten
När du har skapat hello säkerhetskopieringsvalv och hämta hello valvautentiseringsfilen installeras en agent på varje Windows-datorer.

### <a name="toodownload-install-and-register-hello-agent"></a>toodownload, installera och registrera hello-agent
1. Klicka på **återställningstjänster**, och välj sedan hello säkerhetskopieringsvalv som du vill tooregister med en server.
2. På hello Snabbstart klickar du på hello agent **Agent för Windows Server och System Center Data Protection Manager eller Windows-klient**. Klicka sedan på **Spara**.

    ![Spara agent](./media/backup-configure-vault-classic/agent.png)
3. När hello MARSagentinstaller.exe filen har hämtats, klickar du på **kör** (eller dubbelklicka på **MARSAgentInstaller.exe** från hello sparade plats).
4. Välj hello installationsmappen och cachemappen som krävs för hello agent och klicka sedan på **nästa**. hello cacheplats som du anger måste ha ledigt utrymme lika tooat minst 5 procent av hello säkerhetskopierade data.
5. Du kan fortsätta tooconnect toohello Internet via hello standard proxyinställningar.             Om du använder en proxy server tooconnect toohello Internet, på hello proxykonfiguration sidan Välj hello **anpassade proxyinställningar** kryssrutan och ange sedan hello proxy-serverinformation. Om du använder en autentiserad proxyserver ange hello namn och lösenord användarinformation och klicka sedan på **nästa**.
6. Klicka på **installera** toobegin hello agentinstallation. hello säkerhetskopieringsagenten installerar .NET Framework 4.5 och Windows PowerShell (om det inte redan är installerat) toocomplete hello installation.
7. När hello-agenten är installerad klickar du på **fortsätta tooRegistration** toocontinue med hello arbetsflöde.
8. Bläddra tooand väljer hello valvautentiseringsfilen som du tidigare hämtade hello valvet identifiering på sidan.

    Hej valvautentiseringsfilen är giltig för endast 48 timmar när den har hämtats från hello-portalen. Om det uppstår ett fel på den här sidan (till exempel ”valvautentiseringsuppgifter tillhandahållna filen har upphört att gälla”), logga in toohello portal och hämta hello valvautentiseringsfilen igen.

    Se till att hello valvautentiseringsfilen finns på en plats som kan nås av hello installationsprogram. Om du får åtkomst-relaterade fel, kopiera hello valvet autentiseringsuppgifter filen tooa tillfällig plats på hello samma datorn och försök utföra hello igen.

    Om det uppstår ett valv autentiseringsuppgifter fel som ”ogiltiga autentiseringsuppgifter om” hello-filen är skadad eller inte har hello senaste autentiseringsuppgifterna som är associerade med återställningstjänsten hello. Försök igen när du har hämtat en ny valvautentiseringsfil från portalen hello om hello. Det här felet kan också inträffa om en användare klickar på hello **Download valvautentiseringen** alternativet flera gånger i snabb följd. I det här fallet gäller endast hello senaste valvautentiseringsfilen.
9. På sidan krypteringsinställning hello du generera en lösenfras eller ange en lösenfras (med minst 16 tecken). Kom ihåg toosave hello lösenfras på en säker plats.
10. Klicka på **Slutför**. hello guiden registrera Server registrerar hello-server med Backup.

    > [!WARNING]
    > Om du förlorar eller glömmer hello lösenfras kan inte Microsoft hjälper dig att återställa hello säkerhetskopierade data. Du äger hello krypteringslösenfrasen och Microsoft inte har insyn i hello lösenfras som du använder. Spara hello-filen på en säker plats eftersom den måste utföras under en återställning.
    >
    >

11. När du har angett hello krypteringsnyckeln lämna hello **starta Microsoft Azure Recovery Services-agenten** vara markerad och klicka sedan på **Stäng**.

## <a name="complete-hello-initial-backup"></a>Fullständig hello första säkerhetskopian
hello första säkerhetskopian innehåller två viktiga uppgifter:

* Skapa hello schemat för säkerhetskopiering
* Säkerhetskopiera filer och mappar för hello första gången

När hello säkerhetskopieringsprincip är klar hello första säkerhetskopiering skapar säkerhetskopiering återställningspunkter som du kan använda om du behöver toorecover hello data. hello säkerhetskopieringsprincip sker baserat på hello-schema som du definierar.

### <a name="tooschedule-hello-backup"></a>tooschedule hello säkerhetskopiering
1. Öppna hello Microsoft Azure Backup-agenten. (Öppnas automatiskt om du lämnar hello **starta Microsoft Azure Recovery Services-agenten** markerad när du har stängt hello guiden för serverregistrering.) Du hittar den genom att söka efter **Microsoft Azure Backup** på datorn.

    ![Starta hello Azure Backup-agenten](./media/backup-configure-vault-classic/snap-in-search.png)
2. Klicka i hello säkerhetskopieringsagenten **schemalägga säkerhetskopiering**.

    ![Schemalägga en Windows Server-säkerhetskopiering](./media/backup-configure-vault-classic/schedule-backup-close.png)
3. På hello komma igång-sidan hello guiden för Schemalägg säkerhetskopiering klickar du på **nästa**.
4. På hello Välj objekt tooBackup klickar du på **Lägg till objekt**.
5. Välj hello filer och mappar som du vill att tooback och klicka sedan på **okej**.
6. Klicka på **Nästa**.
7. På hello **ange schemat för säkerhetskopiering** anger hello **Säkerhetskopieringsschemat** och på **nästa**.

    Du kan schemalägga säkerhetskopieringar varje dag (högst tre gånger om dagen) eller varje vecka.

    ![Windows Server Backup-objekt](./media/backup-configure-vault-classic/specify-backup-schedule-close.png)

   > [!NOTE]
   > Mer information om hur toospecify hello schemat för säkerhetskopiering finns hello artikel [Använd Azure Backup tooreplace infrastrukturen band](backup-azure-backup-cloud-as-tape.md).
   >
   >

8. På hello **Välj bevarandeprincip** sidan, Välj hello **bevarandeprincip** hello säkerhetskopian.

    hello bevarandeprincip anger hello varaktigheten hello säkerhetskopian ska sparas. I stället för att bara ange en ”platt policy” för alla återställningspunkter för säkerhetskopiering, kan du ange olika bevarandeprinciper baserat på när hello säkerhetskopia. Du kan ändra hello dagliga, veckovisa, månatliga och årliga Kvarhållningsintervall principer toomeet dina behov.
9. Välj hello inledande säkerhetskopieringstyp hello på sidan Välj typ av inledande säkerhetskopiering. Lämna alternativet hello **automatiskt över hello nätverk** markerad och klicka sedan på **nästa**.

    Du kan säkerhetskopiera automatiskt över hello nätverk eller du kan säkerhetskopiera offline. hello resten av den här artikeln beskrivs hello säkerhetskopiera automatiskt. Om du föredrar toodo en offlinesäkerhetskopiering granska hello artikel [Offline säkerhetskopiering arbetsflödet i Azure Backup](backup-azure-backup-import-export.md) för ytterligare information.
10. På sidan Bekräfta hello granska hello information och klicka sedan på **Slutför**.
11. När hello guiden är färdig med hello schemat för säkerhetskopiering, klickar du på **Stäng**.

### <a name="enable-network-throttling-optional"></a>Aktivera begränsning av (valfritt)
hello säkerhetskopieringsagenten ger nätverksbegränsning. Begränsningskontroller hur nätverksbandbredden används när data överfördes. Den här kontrollen kan vara användbart om du behöver tooback upp data under arbetstid men inte vill att hello säkerhetskopieringsprocessen toointerfere med andra Internettrafik. Begränsning gäller tooback upp och återställa aktiviteter.

**tooenable nätverksbegränsning**

1. Klicka i hello säkerhetskopieringsagenten **ändra egenskaper för**.

    ![Ändra egenskaper för](./media/backup-configure-vault-classic/change-properties.png)
2. På hello **begränsning** fliken, väljer hello **aktivera Användningsbegränsning för internetbandbredd för säkerhetskopieringsåtgärder** kryssrutan.

    ![Nätverksbegränsning](./media/backup-configure-vault-classic/throttling-dialog.png)
3. När du har aktiverat begränsning, ange hello tillåtet bandbredd för överföring av säkerhetskopierade data under **arbetstid** och **icke-arbetstid**.

    hello bandbredd värden börjar på 512 kilobit per sekund (Kbps) och gå upp too1 023 megabyte per sekund (MBps). Du kan också ange hello start och avslutas för **arbetstid**, och vilka hello veckodagar som anses vara arbetsdagar. Timmar utanför avsedda arbete timmar anses fungerar timmar.
4. Klicka på **OK**.

### <a name="tooback-up-now"></a>tooback nu
1. Klicka i hello säkerhetskopieringsagenten **Säkerhetskopiera nu** toocomplete hello inledande seeding hello nätverket.

    ![Säkerhetskopiera Windows Server nu](./media/backup-configure-vault-classic/backup-now.png)
2. På sidan Bekräfta hello använder hello granskningsinställningar som hello tillbaka nu guiden Installera tooback upp hello-datorn. Klicka på **Säkerhetskopiera**.
3. Klicka på **Stäng** tooclose hello guiden. Om du gör detta innan hello säkerhetskopieringen är klar fortsätter hello guiden toorun i hello bakgrund.

När hello första säkerhetskopieringen har slutförts hello **jobbet slutfört** status visas i konsolen för hello-säkerhetskopiering.

![IR slutfört](./media/backup-configure-vault-classic/ircomplete.png)

## <a name="next-steps"></a>Nästa steg
* Registrera dig för en [kostnadsfritt Azure-konto](https://azure.microsoft.com/free/).

För ytterligare information om hur du säkerhetskopierar virtuella datorer eller andra arbetsbelastningar, se:

* [Säkerhetskopiera virtuella IaaS-datorer](backup-azure-vms-prepare.md)
* [Säkerhetskopiera arbetsbelastningar tooAzure med Microsoft Azure Backup-Server](backup-azure-microsoft-azure-backup.md)
* [Säkerhetskopiera arbetsbelastningar tooAzure med DPM](backup-azure-dpm-introduction.md)

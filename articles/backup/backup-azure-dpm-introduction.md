---
title: aaaUse DPM tooback in arbetsbelastningar tooAzure portal | Microsoft Docs
description: "En introduktion toobacking in DPM-servrar med hello Azure Backup-tjänsten"
services: backup
documentationcenter: 
author: adigan
manager: nkolli
editor: 
keywords: "System Center Data Protection Manager data protection manager, dpm-säkerhetskopiering"
ms.assetid: c8c322cf-f5eb-422c-a34c-04a4801bfec7
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: adigan;giridham;jimpark;markgal;trinadhk
ms.openlocfilehash: 1dd988ae55012ac7dc485d2416458542c60b6ae3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="preparing-tooback-up-workloads-tooazure-with-dpm"></a>Förbereda tooback in tooAzure arbetsbelastningar med DPM
> [!div class="op_single_selector"]
> * [Azure Backup Server](backup-azure-microsoft-azure-backup.md)
> * [SCDPM](backup-azure-dpm-introduction.md)
> * [Azure Backup-Server (klassisk)](backup-azure-microsoft-azure-backup-classic.md)
> * [SCDPM (klassisk)](backup-azure-dpm-introduction-classic.md)
>
>

Den här artikeln innehåller en introduktion toousing Microsoft Azure Backup tooprotect System Center Data Protection Manager (DPM) servrar och arbetsbelastningar. Genom att läsa det, måste du förstå:

* Hur fungerar Azure DPM server-säkerhetskopiering
* hello krav tooachieve en smidig säkerhetskopiering upplevelse
* Hej vanliga fel som inträffar och hur toodeal med dem.
* Scenarier som stöds

> [!NOTE]
> Azure har två distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md). Den här artikeln innehåller hello information och procedurer för att återställa virtuella datorer som distribueras med hjälp av hello Resource Manager-modellen.
>
>

System Center DPM säkerhetskopierar filer och programdata. Data som säkerhetskopieras tooDPM kan lagras på band på disk, eller säkerhetskopierade tooAzure med Microsoft Azure Backup. DPM samverkar med Azure Backup på följande sätt:

* **DPM distribueras som en fysisk server eller lokal virtuell dator** – om DPM distribueras som en fysisk server eller en lokal Hyper-V virtuell dator som du kan säkerhetskopiera data tooa återställningstjänster valvet dessutom toodisk och säkerhetskopiering av band.
* **DPM distribueras som en virtuell Azure-dator** – från System Center 2012 R2 med uppdatering 3 kan DPM distribueras som en virtuell Azure-dator. Om DPM distribueras som en virtuell Azure-dator kan du säkerhetskopiera data tooAzure diskar kopplade toohello DPM Azure-datorn eller du omfördela datalagring hello genom att säkerhetskopiera tooa Recovery Services-valvet.

## <a name="why-backup-from-dpm-tooazure"></a>Varför säkerhetskopiera från DPM-tooAzure?
hello fördelarna med att använda Azure Backup för att säkerhetskopiera DPM-servrar är:

* Du kan använda Azure som en alternativ toolong sikt distribution tootape för lokal DPM-distribution.
* För DPM-distributioner i Azure kan Azure Backup du toooffload storage från hello Azure-disken så att du tooscale upp genom att lagra äldre data i Recovery Services-valvet och nya data på disken.

## <a name="prerequisites"></a>Krav
Förbered Azure Backup tooback in DPM-data på följande sätt:

1. **Skapa ett Recovery Services-valv** – skapa ett valv i Azure-portalen.
2. **Ladda ned valvautentiseringsuppgifter** – hämta hello-autentiseringsuppgifter som du använder tooregister hello DPM server tooRecovery Services-valvet.
3. **Installera hello Azure Backup-agenten** – från Azure Backup installera hello-agenten på varje DPM-server.
4. **Registrera hello servern** – registrera hello DPM server tooRecovery Services-valvet.

### <a name="1-create-a-recovery-services-vault"></a>1. Skapa ett Recovery Services-valv
toocreate en recovery services-valv:

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Hej hubbmenyn, klicka på **Bläddra** och Skriv i hello lista över resurser, **återställningstjänster**. När du börjar skriva in hello listan filtreras baserat på dina indata. Klicka på **Recovery Services-valv**.

    ![Skapa Recovery Services-valv (steg 1)](./media/backup-azure-dpm-introduction/open-recovery-services-vault.png)

    hello lista över Recovery Services-valv visas.
3. På hello **Recovery Services-valv** -menyn klickar du på **Lägg till**.

    ![Skapa Recovery Services-valv (steg 2)](./media/backup-azure-dpm-introduction/rs-vault-menu.png)

    hello återställningstjänster valvet blad öppnas, där du uppmanas tooprovide en **namn**, **prenumeration**, **resursgruppen**, och **plats**.

    ![Skapa ett Recovery Services-valv (steg 5)](./media/backup-azure-dpm-introduction/rs-vault-attributes.png)
4. För **namn**, ange ett eget namn tooidentify hello valv. hello namn måste toobe unika för hello Azure-prenumeration. Skriv ett namn som innehåller mellan 2 och 50 tecken. Det måste börja med en bokstav och får endast innehålla bokstäver, siffror och bindestreck.
5. Klicka på **prenumeration** toosee hello tillgängliga listan över prenumerationer. Om du inte är säker på vilken prenumeration toouse använder standard-hello (eller förslag) prenumerationen. Du kan bara välja mellan flera alternativ om ditt organisationskonto är associerat med flera Azure-prenumerationer.
6. Klicka på **resursgruppen** toosee hello tillgängliga listan över resursgrupper, eller klicka på **ny** toocreate en ny resursgrupp. För komplett information om resursgrupper, se [Översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).
7. Klicka på **plats** tooselect hello geografiskt område för hello-valvet.
8. Klicka på **Skapa**. Det kan ta en stund innan hello Recovery Services-valvet toobe skapas. Övervaka hello statusmeddelanden i övre högra delen hello hello-portalen.
   När du har skapat ditt valv öppnas i hello-portalen.

### <a name="set-storage-replication"></a>Konfigurera lagringsreplikering
hello lagringsalternativ för replikering kan du toochoose mellan geo-redundant lagring och lokalt redundant lagring. Valvet använder geo-redundant lagring som standard. Lämna hello alternativet set toogeo-redundant lagring om det här är den primära säkerhetskopieringen. Välj lokalt redundant lagring om du vill använda ett billigare alternativ som inte är lika beständigt. Läs mer om [geo-redundant](../storage/common/storage-redundancy.md#geo-redundant-storage) och [lokalt redundant](../storage/common/storage-redundancy.md#locally-redundant-storage) lagringsalternativ i hello [Azure Storage-replikering: översikt](../storage/common/storage-redundancy.md).

replikeringsinställningen för tooedit hello lagring:

1. Välj valvet tooopen hello valvet instrumentpanelen och hello inställningsbladet. Om hello **inställningar** bladet inte öppnas, klickar du på **alla inställningar** hello valvet instrumentpanelen.
2. På hello **inställningar** bladet, klickar du på **säkerhetskopiering infrastruktur** > **konfigurering av säkerhetskopiering** tooopen hello **konfigurering av säkerhetskopiering** bladet. På hello **konfigurering av säkerhetskopiering** bladet välj hello replikering lagringsalternativ för ditt valv.

    ![Lista över säkerhetskopieringsvalv](./media/backup-azure-vms-first-look-arm/choose-storage-configuration-rs-vault.png)

    När du väljer hello lagringsalternativ för ditt valv, är du redo tooassociate hello VM hello-valvet. toobegin hello association, ska du identifiera och registrera hello Azure virtuella datorer.

### <a name="2-download-vault-credentials"></a>2. Ladda ned autentiseringsuppgifter för valvet
Hej valvautentiseringsfilen är ett certifikat som genereras av hello portal för varje säkerhetskopieringsvalvet. hello portal sedan överför hello offentliga nyckel toohello Access Control Service (ACS). hello privata nyckeln för certifikatet för hello görs tillgängliga toohello användaren som en del av hello arbetsflöde som angetts som indata i hello arbetsflöde för registrering av datorn. Detta autentiserar hello datorn toosend säkerhetskopieringsdata tooan identifieras valvet i hello Azure Backup-tjänsten.

Hej valvautentiseringen används endast under hello arbetsflöde för registrering. Det är hello användarens ansvar tooensure som hello valvautentiseringsuppgifter filen inte har komprometterats. Om den hamnar i hello händer en obehörig användare hello valvautentiseringsfilen kan vara används tooregister andra datorer mot hello samma valvet. Dock som hello säkerhetskopierade data krypteras med en lösenfras som tillhör toohello kunden, kan inte befintlig säkerhetskopierad data vara hotad. toomitigate problem, valvautentiseringsuppgifter ställs tooexpire i 48hrs. Du kan hämta hello valvet autentiseringsuppgifterna för en återställningstjänster valfritt antal gånger – men endast hello senaste valvautentiseringsfilen gäller under hello arbetsflöde för registrering.

Hej valvautentiseringsfilen hämtas via en säker kanal från hello Azure-portalen. hello Azure Backup-tjänsten inte känner till hello privat nyckel för hello certifikat och hello privata nyckeln beständig inte i hello-portalen eller hello-tjänsten. Använd hello följande steg toodownload hello valvet autentiseringsuppgifter filen tooa lokal dator.

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Öppna Recovery Services-valvet toowhich toowhich du vill tooregister DPM-datorn.
3. Inställningsbladet öppnas som standard. Om den är stängd klickar du på **inställningar** på inställningsbladet för valvet instrumentpanelen tooopen hello. I inställningar-bladet klickar du på **egenskaper**.

    ![Öppna bladet för valvet](./media/backup-azure-dpm-introduction/vault-settings-dpm.png)
4. På sidan Egenskaper för hello klickar du på **hämta** under **säkerhetskopiering autentiseringsuppgifter**. hello portal genererar hello valvautentiseringsfilen, vilket görs tillgänglig för hämtning.

    ![Ladda ned](./media/backup-azure-dpm-introduction/vault-credentials.png)

hello portal kommer att generera en valvautentiseringen med hjälp av en kombination av hello valvnamnet och hello aktuellt datum. Klicka på **spara** toodownload hello valvet autentiseringsuppgifter toohello lokala kontots hämtar mappen eller välj Spara som från hello spara menyn toospecify en plats för hello valvautentiseringsuppgifter. Det tar upp tooa minut för hello filen toobe genereras.

### <a name="note"></a>Obs!
* Se till att hello valvautentiseringsfilen sparas på en plats som kan nås från datorn. Om den är lagrad i en fil resursen/SMB Kontrollera hello åtkomstbehörighet.
* Hej valvautentiseringsfilen används endast under hello arbetsflöde för registrering.
* Hej valvautentiseringsfilen upphör att gälla efter 48hrs och kan hämtas från hello-portalen.

### <a name="3-install-backup-agent"></a>3. Installera säkerhetskopieringsagenten
När du har skapat hello Azure Backup-valv, ska en agent installeras på var och en av dina Windows-datorer (Windows Server, Windows-klient, System Center Data Protection Manager-server eller Azure Backup Server-dator) som möjliggör säkerhetskopiering av data och program tooAzure.

1. Öppna Recovery Services-valvet toowhich toowhich du vill tooregister DPM-datorn.
2. Inställningsbladet öppnas som standard. Om den är stängd klickar du på **inställningar** tooopen hello inställningsbladet. I inställningar-bladet klickar du på **egenskaper**.

    ![Öppna bladet för valvet](./media/backup-azure-dpm-introduction/vault-settings-dpm.png)
3. På hello inställningar klickar du på **hämta** under **Azure Backup-agenten**.

    ![Ladda ned](./media/backup-azure-dpm-introduction/azure-backup-agent.png)

   När hello agent har hämtats, dubbelklicka på MARSAgentInstaller.exe toolaunch hello installation av hello Azure Backup-agenten. Välj hello installationsmappen och tillfälliga mapp som krävs för hello agent. hello cacheplats anges måste ha ledigt utrymme som är minst 5% av hello säkerhetskopierade data.
4. Om du använder en proxy server tooconnect toohello internet i hello **proxykonfiguration** anger hello proxy-serverinformation. Om du använder en autentiserad proxyserver ange hello namn och lösenord användarinformation i den här skärmen.
5. hello Azure Backup-agenten installeras .NET Framework 4.5 och Windows PowerShell (om det inte är tillgängligt) toocomplete hello installation.
6. När hello-agenten är installerad **Stäng** hello-fönstret.

   ![Stäng](../../includes/media/backup-install-agent/dpm_FinishInstallation.png)
7. för**registrera hello DPM-servern** toohello valvet i hello **Management** fliken, klicka på **Online**. Markera **registrera**. Hello registrera installationsguiden öppnas.
8. Om du använder en proxy server tooconnect toohello internet i hello **proxykonfiguration** anger hello proxy-serverinformation. Om du använder en autentiserad proxyserver ange hello namn och lösenord användarinformation i den här skärmen.

    ![Proxykonfiguration](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Proxy.png)
9. Bläddra i hello valvet autentiseringsuppgifter skärmen tooand väljer hello valvautentiseringsfilen som tidigare har hämtats.

    ![Valvautentiseringsuppgifter](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Credentials.jpg)

    Hej valvautentiseringsfilen är endast giltig för 48 timmar (när den har hämtats från hello portal). Om det uppstår något fel i den här skärmen (till exempel ”valvet autentiseringsuppgifter tillhandahållna filen har upphört att gälla”), inloggning toohello Azure portal och hämta hello valvautentiseringsfilen igen.

    Se till att hello valvautentiseringsfilen finns på en plats som kan nås av hello installationsprogram. Om du får åtkomst till relaterade fel, kopiera hello valvet autentiseringsuppgifter tooa tillfällig filplats i den här datorn och försök hello igen.

    Om det uppstår ett fel för ogiltiga autentiseringsuppgifter (till exempel ”ogiltiga autentiseringsuppgifter angivna”) hello-filen är antingen skadad eller inte har hello senaste autentiseringsuppgifterna som är associerade med hello recovery-tjänsten. Försök igen när du har hämtat en ny valvautentiseringsfil från portalen hello om hello. Det här felet visas vanligen om hello användare klickar på hello **Download valvautentiseringen** alternativ i hello Azure-portalen i snabb följd. I det här fallet gäller endast hello andra valvautentiseringsfilen.
10. toocontrol hello användningen av nätverksbandbredden under arbete och icke-arbetstid i hello **begränsning inställningen** skärmen, du kan ange hello bandbreddsbegränsningen och definiera hello arbete och fritid timmar.

    ![Begränsningsinställning](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Throttling.png)
11. I hello **Recovery mappen inställningen** skärmen, bläddra för hello mapp dit hello filer hämtas från Azure mellanlagrad tillfälligt.

    ![Inställningen för återställning-mappen](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_RecoveryFolder.png)
12. I hello **krypteringsinställning** skärmen, kan du antingen skapa en lösenfras eller ange en lösenfras (minst 16 tecken). Kom ihåg toosave hello lösenfras på en säker plats.

    ![Kryptering](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Encryption.png)

    > [!WARNING]
    > Om hello tappar bort lösenfrasen eller glömt; Microsoft kan inte att återställa hello säkerhetskopierade data. hello användaren äger hello krypteringslösenfrasen och Microsoft har inte insyn i hello lösenfras som används av hello slutanvändaren. Spara hello-filen på en säker plats när den behövs under en återställning.
    >
    >
13. När du klickar på hello **registrera** knappen, hello datorn har registrerats toohello valv och du är nu redo toostart säkerhetskopiera tooMicrosoft Azure.
14. När du använder Data Protection Manager, du kan ändra hello-inställningar som anges när ett arbetsflöde för registrering av hello genom att klicka på hello **konfigurera** alternativet genom att välja **Online** under hello  **Hantering av** fliken.

## <a name="requirements-and-limitations"></a>Krav (och begränsningar)
* DPM kan köras som en fysisk server eller en virtuell dator för Hyper-V som är installerade på System Center 2012 SP1 eller System Center 2012 R2. Det kan också köras som en Azure-dator som körs på System Center 2012 R2 med minst DPM 2012 R2 Samlad uppdatering 3 eller en Windows-dator i VMWare som körs på System Center 2012 R2 med minst Samlad uppdatering 5.
* Om du kör DPM med System Center 2012 SP1 bör du installera Samlad uppdatering 2 för System Center Data Protection Manager SP1. Detta krävs innan du kan installera hello Azure Backup-agenten.
* hello DPM-servern måste ha Windows PowerShell och .net Framework 4.5 installerat.
* DPM kan säkerhetskopiera de flesta arbetsbelastningar tooAzure säkerhetskopiering. Hello Azure Backup för en fullständig lista över vad har stöd för finns stöd för objekten nedan.
* Data som lagras i Azure Backup kan inte återställas med alternativet för hello ”kopiera tootape”.
* Du behöver ett Azure-konto med hello Azure Backup-funktionen aktiverad. Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter. Läs mer om [priser för Azure Backup](https://azure.microsoft.com/pricing/details/backup/).
* Med Azure Backup kräver hello Azure Backup-agenten toobe installerade på hello-servrar som du vill att tooback. Alla servrar måste ha minst 5% av hello storlek hello data som säkerhetskopieras, tillgänglig som lokala ledigt utrymme. Till exempel kräver säkerhetskopiera 100 GB data minst 5 GB ledigt utrymme på hello virtuellt plats.
* Data lagras i hello Azure valvet lagring. Det finns ingen gräns toohello mängden data som du kan säkerhetskopiera tooan Azure Backup-valv men hello storleken för en datakälla (till exempel en virtuell dator eller en databas) får inte överskrida 54400 GB.

Dessa filtyper stöds för säkerhetskopiering av tooAzure:

* Krypterade (endast fullständiga säkerhetskopior)
* Komprimerade (inkrementella säkerhetskopior stöds)
* Sparse-filer (inkrementella säkerhetskopior stöds)
* Komprimerade och sparse-filer (hanteras som sparse-filer)

Och de stöds inte:

* Servrar i skiftlägeskänsliga filsystem stöds inte.
* Hårda länkar (ignoreras)
* Referenspunkt (ignoreras)
* Krypterade och komprimerade (ignoreras)
* Krypterade och utspridda (ignoreras)
* Komprimerad ström
* Utspridd ström

> [!NOTE]
> Från i System Center 2012 DPM med SP1 och senare kan du säkerhetskopiera upp arbetsbelastningar skyddade av DPM tooAzure med hjälp av Microsoft Azure Backup.
>
>

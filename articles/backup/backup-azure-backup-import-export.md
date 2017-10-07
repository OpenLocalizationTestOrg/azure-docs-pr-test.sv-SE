---
title: "aaaAzure säkerhetskopia - offlinesäkerhetskopiering eller inledande seeding med hello Azure Import/Export service | Microsoft Docs"
description: "Lär dig hur Azure Backup kan du toosend data från hello nätverk med hello Azure Import/Export service. Den här artikeln förklarar hello offline seeding av hello första säkerhetskopierade informationen med hjälp av hello Azure Import exportera service."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: ada19c12-3e60-457b-8a6e-cf21b9553b97
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 4/20/2017
ms.author: saurse;nkolli;trinadhk
ms.openlocfilehash: f1696957c3e9684b800c8d030131255905459f7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="offline-backup-workflow-in-azure-backup"></a>Arbetsflöde för säkerhetskopiering offline i Azure Backup
Azure-säkerhetskopiering har flera inbyggda effektivitet som sparar kostnader för nätverk och lagring under hello första fullständiga säkerhetskopieringar av data tooAzure. Första fullständiga säkerhetskopieringar vanligtvis överföra stora mängder data och kräver större nätverksbandbredd jämfört toosubsequent säkerhetskopieringar som överför bara hello går/varje. Azure-säkerhetskopiering komprimerar hello inledande säkerhetskopieringar. Genom hello av offline seeding, kan Azure Backup använda diskar tooupload hello komprimerade första säkerhetskopierade informationen offline tooAzure.  

hello offline seeding process för Azure Backup är nära integrerad med hello [Azure Import/Export service](../storage/common/storage-import-export-service.md) som du kan använda tootransfer data tooAzure med diskar. Om du har terabyte (TBs) första säkerhetskopierade informationen som överförs i nätverket tidsfördröjning och låg bandbredd toobe måste använda du hello offline seeding arbetsflöde tooship hello första säkerhetskopiering på en eller flera hårddiskar tooan Azure-datacenter. Den här artikeln innehåller en översikt över hello steg som slutför det här arbetsflödet.

## <a name="overview"></a>Översikt
Med hello offline seeding möjligheterna för Azure Backup och Azure Import/Export är det enkelt tooupload hello data offline tooAzure med diskar. I stället för att överföra hello inledande full kopia över nätverket hello hello säkerhetskopierade data skrivs tooa *mellanlagringsplatsen*. När hello kopiera toohello mellanlagringsplatsen har slutförts med hello Azure Import/Export verktyget skrivs dessa data tooone eller mer SATA-enheter, beroende på hello mängden data. Dessa enheter är slutligen levererade toohello närmaste Azure-datacenter.

Hej [augusti 2016 uppdatera Azure Backup (och senare)](http://go.microsoft.com/fwlink/?LinkID=229525) innehåller hello *förberedelseverktyget för Azure Disk*, med namnet AzureOfflineBackupDiskPrep, som:

* Hjälper dig att förbereda dina enheter för Azure Import med hjälp av hello Azure Import/Export-verktyget.
* Skapar automatiskt en Azure-importjobbet för hello Azure Import/Export service på hello [klassiska Azure-portalen](https://manage.windowsazure.com) som skillnad från toocreating hello samma manuellt med äldre versioner av Azure Backup.

När hello säkerhetskopieringsdata tooAzure hello överföringen är klar, Azure Backup kopierar hello säkerhetskopieringsdata toohello säkerhetskopieringsvalv och hello inkrementella säkerhetskopieringar är schemalagda.

> [!NOTE]
> toouse hello förberedelseverktyget för Azure Disk, se till att du har installerat hello augusti 2016 Azure Backup (eller senare) och utför alla hello steg för hello arbetsflöde med den. Om du använder en äldre version av Azure Backup kan du förbereda hello SATA-enhet med verktyget hello Azure Import/Export enligt anvisningarna i senare avsnitt i den här artikeln.
>
>

## <a name="prerequisites"></a>Krav
* [Bekanta dig med hello Azure Import/Export arbetsflöde](../storage/common/storage-import-export-service.md).
* Kontrollera hello följande innan du påbörjar hello arbetsflödet:
  * Ett Azure Backup-valvet har skapats.
  * Valvautentiseringsuppgifter som har hämtats.
  * hello Azure Backup-agenten har installerats på antingen Windows Server/Windows klient- eller System Center Data Protection Manager och hello dator har registrerats med hello Azure Backup-valvet.
* [Hämta hello Azure filen Publiceringsinställningar](https://manage.windowsazure.com/publishsettings) på hello dator från vilken du planerar tooback in dina data.
* Förbered en fristående plats, som kan vara en nätverksresurs eller en ytterligare enhet på hello-dator. hello mellanlagringsplatsen är tillfälliga lagringen och används tillfälligt under det här arbetsflödet. Kontrollera att hello mellanlagringsplatsen har tillräckligt med disk space toohold din första kopian. Till exempel om du försöker tooback in en 500 GB filserver, se till att det hello mellanlagringsområdet är minst 500 GB. (Färre används på grund av toocompression.)
* Kontrollera att du använder en enhet som stöds. Endast 2,5 tum SSD eller 2,5 eller 3,5-tums SATA II/III interna hårddiskar stöds för användning med hello Import/Export service. Du kan använda hårddiskar upp too10 TB. Kontrollera hello [Azure Import/Export service dokumentationen](../storage/common/storage-import-export-service.md#hard-disk-drives) för hello senaste uppsättning enheter som hello tjänsten stöder.
* Aktivera BitLocker på hello datorn toowhich hello SATA enhet skrivaren är ansluten.
* [Hämta hello Azure Import/Export](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409) toohello datorn toowhich hello SATA-enheten skrivaren är ansluten. Det här steget är inte nödvändigt om du har hämtat och installerat hello augusti 2016 Azure Backup (eller senare).

## <a name="workflow"></a>Arbetsflöde
hello informationen i det här avsnittet hjälper dig att slutföra hello offlinesäkerhetskopiering arbetsflödet så att dina data levereras tooan Azure-datacenter och överföra tooAzure lagring. Om du har frågor om hello importera tjänsten eller någon aspekt av hello process finns hello [översikt över Import](../storage/common/storage-import-export-service.md) dokumentationen refererar till tidigare.

### <a name="initiate-offline-backup"></a>Initiera offlinesäkerhetskopiering
1. När du schemalägger en säkerhetskopia kan se du hello följande skärm (i Windows Server, Windows-klient eller System Center Data Protection Manager).

    ![Importskärmen](./media/backup-azure-backup-import-export/offlineBackupscreenInputs.png)

    Det finns motsvarande hello-skärmen i System Center Data Protection Manager: <br/>
    ![DPM importskärmen](./media/backup-azure-backup-import-export/dpmoffline.png)

    hello beskrivning av hello indata är följande:

    * **Plats för mellanlagring**: hello tillfällig lagring plats toowhich hello första säkerhetskopiering skrivs. Detta kan bero på en nätverksresurs eller en lokal dator. Om hello kopiera datorn och källdatorn är olika, rekommenderar vi att du anger hello fullständiga nätverkssökvägen för hello mellanlagringsplatsen.
    * **Azure Importjobbets namn**: hello unikt namn för vilka Azure Import-tjänsten och Azure Backup spåra hello överföring av data som skickas på diskar tooAzure.
    * **Azure Publiceringsinställningar**: en XML-fil som innehåller information om din prenumeration profil. Den innehåller också säkra referenser som är associerade med prenumerationen. Du kan [hämtningen hello](https://manage.windowsazure.com/publishsettings). Ange hello lokal sökväg toohello inställningsfilen för publicering.
    * **Prenumerations-ID för Azure**: hello Azure prenumerations-ID för hello prenumeration där du planerar att tooinitiate hello Azure-importjobbet. Om du har flera Azure-prenumerationer, använda hello-ID för hello prenumeration som du vill tooassociate med hello-importjobbet.
    * **Azure Storage-konto**: hello klassisk typ storage-konto i hello angivna Azure-prenumeration som ska associeras med hello Azure-importjobbet.
    * **Azure Storage-behållare**: hello namnet på hello mål lagringsblob i hello Azure storage-konto där det här jobbet data importeras.

    > [!NOTE]
    > Om du har registrerat din server tooan Azure Recovery Services valvet från hello [Azure-portalen](https://portal.azure.com) för dina säkerhetskopieringar och finns inte på en prenumeration som Cloud Solution Providers (CSP) du kan ändå skapa ett lagringskonto för klassisk typ från hello Azure-portalen och använda den för hello offlinesäkerhetskopiering arbetsflöde.
    >
    >

     Spara den här informationen eftersom du behöver tooenter den igen i följande steg. Endast hello *mellanlagringsplatsen* krävs om du använde hello Azure Disk förberedelse verktyget tooprepare hello diskar.    
2. Slutför hello arbetsflödet och välj sedan **Säkerhetskopiera nu** i hello Azure Backup management console tooinitiate hello-offlinesäkerhetskopiering kopian. hello första säkerhetskopieringen skrivs toohello mellanlagringsområdet som en del av det här steget.

    ![Säkerhetskopiera nu](./media/backup-azure-backup-import-export/backupnow.png)

    toocomplete hello motsvarande arbetsflödet i System Center Data Protection Manager, högerklicka på hello **Skyddsgrupp**, och välj sedan hello **Skapa återställningspunkt** alternativet. Sedan kan du välja hello **Onlineskydd** alternativet.

    ![DPM-säkerhetskopiering nu](./media/backup-azure-backup-import-export/dpmbackupnow.png)

    När hello är klar, är hello mellanlagringsplatsen klar toobe som används för förberedelse av disk.

    ![Säkerhetskopiering](./media/backup-azure-backup-import-export/opbackupnow.png)

### <a name="prepare-a-sata-drive-and-create-an-azure-import-job-by-using-hello-azure-disk-preparation-tool"></a>Förbereda en SATA-enhet och skapa en Azure-importjobbet med hjälp av hello Azure Disk systemförberedelseverktyget
hello Azure Disk förberedelseverktyget finns i installationskatalogen för hello Recovery Services-agenten (augusti 2016 uppdatera och senare) i hello följande sökväg.

   *\Microsoft* *azure* *Recovery* *Services* * Agent\Utils\*

1. Gå toohello directory och kopiera hello **AzureOfflineBackupDiskPrep** directory tooa kopiera dator på vilken hello enheter toobe förberedd är monterade. Kontrollera hello följande med beaktande toohello kopiera datorn:

    * hello kopiera datorn kan komma åt hello mellanlagringsplatsen för hello offline seeding arbetsflöde med hjälp av samma nätverkssökväg som angavs i hello hello **initiera offlinesäkerhetskopiering** arbetsflöde.
    * BitLocker har aktiverats på hello-datorn.
    * hello datorn kan komma åt hello Azure-portalen.

    Om det behövs kan hello kopiera datorn vara hello samma som hello källdatorn.
2. Öppna en upphöjd kommandotolk på hello kopiera dator med hello Azure Disk förberedelse verktyget katalog som hello aktuella och kör följande kommando hello:

    `*.\AzureOfflineBackupDiskPrep.exe*   s:<*Staging Location Path*>   [p:<*Path tooPublishSettingsFile*>]`

    | Parameter | Beskrivning |
    | --- | --- |
    | s:&lt;*sökvägen för Förproduktion*&gt; |Obligatoriska indata som har använt tooprovide hello sökvägen toohello mellanlagringsplatsen som du angav i hello **initiera offlinesäkerhetskopiering** arbetsflöde. |
    | p:&lt;*tooPublishSettingsFile sökväg*&gt; |Valfria indata som har använt tooprovide hello sökvägen toohello **Azure Publiceringsinställningar** -fil som du angav i hello **initiera offlinesäkerhetskopiering** arbetsflöde. |

    > [!NOTE]
    > Hej &lt;sökväg tooPublishSettingFile&gt; värdet är obligatoriskt när hello kopiera datorn och källdatorn är olika.
    >
    >

    När du kör kommandot hello begär hello verktyget hello val av hello Azure-importjobb som motsvarar toohello enheter som behöver toobe förberedd. Om bara en enda importjobbet är associerad med hello tillhandahålls mellanlagringsplatsen, visas en skärm som liknar hello som följer.

    ![Azure Disk förberedelse verktyget indata](./media/backup-azure-backup-import-export/azureDiskPreparationToolDriveInput.png) <br/>
3. Ange hello enhetsbeteckning utan hello avslutande kolon för hello monterade disken som du vill tooprepare för överföring tooAzure. Ange bekräftelse för hello formatering av hello enheten när du uppmanas.

    hello verktyget börjar sedan tooprepare hello disk med hello säkerhetskopierade data. Du kan behöva tooattach ytterligare diskar när du uppmanas av hello-verktyget om hello angivna disk inte har tillräckligt med utrymme för hello säkerhetskopierade data. <br/>

    En eller flera diskar som du angav är förberedda för leverans tooAzure hello slutet av hello verktyget har körts. Dessutom kan ett importjobb med hello namn du angav under hello **initiera offlinesäkerhetskopiering** arbetsflödet har skapats på hello klassiska Azure-portalen. Slutligen hello-verktyget visar hello leverans adress toohello Azure-datacenter där hello diskar måste toobe levererade och hello länken toolocate hello importjobbet på hello klassiska Azure-portalen.

    ![Förberedelse av Azure-disken har slutförts](./media/backup-azure-backup-import-export/azureDiskPreparationToolSuccess.png)<br/>

4. Leverera hello diskar toohello adressen hello verktyget tillhandahålls och hålla hello spårnings-ID för framtida bruk.<br/>

5. När du går toohello länka hello verktyget visas ser du hello Azure storage-konto som du angav i hello **initiera offlinesäkerhetskopiering** arbetsflöde. Här kan du se hello nyskapad importjobbet på hello **IMPORT/EXPORT** fliken hello storage-konto.

    ![Skapade importjobb](./media/backup-azure-backup-import-export/ImportJobCreated.png)<br/>

6. Klicka på **leverans INFO** längst hello hello sidan tooupdate kontakten information som visas i hello följande skärm. Microsoft använder denna information tooship din diskar tillbaka tooyou när hello importerat jobbet har slutförts.

    ![Kontaktuppgifter](./media/backup-azure-backup-import-export/contactInfoAddition.PNG)<br/>

7. Ange hello leveransinformation på hello nästa skärm. Ange hello **leverans operatör** och **spåra antalet** information som motsvarar toohello diskar du levererade toohello Azure-datacenter.

    ![Leverera info](./media/backup-azure-backup-import-export/shippingInfoAddition.PNG)<br/>

### <a name="complete-hello-workflow"></a>Fullständig hello-arbetsflöde
Första säkerhetskopierade informationen är tillgänglig i ditt lagringskonto när hello importjobbet har slutförts. hello Recovery Services-agenten och sedan kopior hello innehållet i hello data från det här kontot toohello säkerhetskopieringsvalvet eller återställningstjänster valvet, beroende på vilket som är tillämpligt. I hello nästa schemalagda tid för säkerhetskopiering utförs hello Azure Backup agent hello inkrementell säkerhetskopiering över hello första säkerhetskopiering.

> [!NOTE]
> hello gäller följande avsnitt toousers av tidigare versioner som inte har åtkomst toohello Azure Disk förberedelseverktyget för Azure Backup.
>
>

### <a name="prepare-a-sata-drive"></a>Förbereda en SATA-enhet
1. Hämta hello [verktyget Azure Import/Export](http://go.microsoft.com/fwlink/?linkid=301900&clcid=0x409) toohello kopiera dator. Se till att hello mellanlagringsplatsen är tillgänglig från hello datorn som du planerar toorun hello nästa uppsättning kommandon. Om det behövs kan hello kopiera datorn vara hello samma som hello källdatorn.

2. Packa upp hello WAImportExport.zip fil. Kör hello WAImportExport verktyg som formaterar hello SATA enheten skriver hello säkerhetskopieringsdata toohello SATA-enhet och krypterar den. Se till att BitLocker är aktiverat på hello datorn innan du kör följande kommando hello. <br/>

    `*.\WAImportExport.exe PrepImport /j:<*JournalFile*>.jrn /id: <*SessionId*> /sk:<*StorageAccountKey*> /BlobType:**PageBlob** /t:<*TargetDriveLetter*> /format /encrypt /srcdir:<*staging location*> /dstdir: <*DestinationBlobVirtualDirectory*>/*`

    > [!NOTE]
    > Om du har installerat hello augusti 2016 update Azure Backup (eller senare), se till att hello mellanlagringsplatsen som du angav är hello samma som hello på hello **Säkerhetskopiera nu** skärmen och innehåller AIB och Base Blob-filer.
    >
    >

| Parameter | Beskrivning |
| --- | --- |
| /j: <*JournalFile*> |hello sökvägen toohello journal-fil. Varje enhet måste ha exakt en journal-fil. hello journal-fil får inte vara på hello målenheten. hello journal filnamnstillägg är .jrn och skapas som en del av det här kommandot. |
| /ID: <*sessions-ID*> |hello sessions-ID identifierar en kopia-session. Det är används tooensure korrekt återställning av en session avbryts kopia. Filer som kopieras i en kopia session lagras i katalogen efter hello sessions-ID på hello målenheten. |
| /Sk: <*StorageAccountKey*> |Hej kontonyckel för hello storage-konto toowhich hello data importeras. hello nyckel måste toobe hello samma som det har angetts under säkerhetskopiering princip/skyddsgrupp skapas. |
| / BlobType |hello typ av blob. Det här arbetsflödet fungerar endast om **PageBlob** har angetts. Detta är inte hello standardalternativet och anges i det här kommandot. |
| / t: <*TargetDriveLetter*> |hello enhetsbeteckning utan hello avslutande kolon hello hårddisktypen som mål för hello aktuella kopia sessionen. |
| / format |hello alternativet tooformat hello-enhet. Ange den här parametern när hello enhet behöver toobe formaterad; Annars utelämna den. Innan hello verktyget formaterar hello enheten, efterfrågar en bekräftelse från hello-konsolen. toosuppress hello bekräftelse, ange hello /silentmode parameter. |
| / encrypt |hello alternativet tooencrypt hello-enhet. Ange den här parametern när hello enhet ännu inte har krypterats med BitLocker och behov toobe som krypterats av hello-verktyget. Om hello enheten redan har krypterats med BitLocker utelämna den här parametern anger hello /bk parametern och ange hello befintlig BitLocker-nyckel. Om du anger hello/format parameter måste du också ange hello / kryptera parametern. |
| /srcdir: <*SourceDirectory*> |hello källkatalog som innehåller filer toobe kopieras toohello målenheten. Se till att det angivna katalognamnet hello har en fullständig snarare än relativ sökväg. |
| /dstdir: <*DestinationBlobVirtualDirectory*> |hello sökvägen toohello mål virtuell katalog i Azure storage-konto. Vara säker på att toouse giltig behållarnamn när du anger hello virtuella kataloger eller BLOB. Tänk på att behållarnamn måste vara gemener.  Det här behållarnamnet ska vara hello som du angav under säkerhetskopiering princip/skyddsgrupp skapas. |

> [!NOTE]
> En journal-fil har skapats i hello WAImportExport mapp som samlar in hello hela information för hello arbetsflöde. Du behöver den här filen när du skapar ett importjobb i hello Azure-portalen.
>
>

  ![PowerShell-utdata](./media/backup-azure-backup-import-export/psoutput.png)

### <a name="create-an-import-job-in-hello-azure-portal"></a>Skapa ett importjobb i hello Azure-portalen
1. Gå tooyour storage-konto i hello [klassiska Azure-portalen](https://manage.windowsazure.com/), klickar du på **Import/Export**, och sedan **skapa importjobbet** hello i åtgärdsfönstret.

    ![Import/export-fliken i hello Azure-portalen](./media/backup-azure-backup-import-export/azureportal.png)

2. I steg 1 av hello guiden anger du att du har förberett din enhet och att du har hello enhet journal-fil tillgänglig.

3. I steg 2 i hello guiden anger du kontaktinformation för hello person som ansvarar för den här importjobbet.

4. Överför hello enhet journal-filer som du fick i förra hello-avsnittet i steg 3.

5. I steg 4, anger du ett beskrivande namn för hello-importjobb som du angav under säkerhetskopiering princip/skyddsgrupp skapas. hello-namnet som du anger kan innehålla endast små bokstäver, siffror, bindestreck och understreck, måste börja med en bokstav och får inte innehålla blanksteg. hello-namn som du har använt tootrack jobb när de pågår och när de har slutförts.

6. Välj därefter datacenter region hello-listan. hello datacenter region anger hello datacenter och adress toowhich måste du levererar paketet.

    ![Välj datacenter region](./media/backup-azure-backup-import-export/dc.png)

7. Välj returnerade prefix hello listan i steg 5 och ange din operatör kontonummer. Microsoft använder det här kontot tooship enheter tillbaka tooyou efter importjobbet har slutförts.

8. Leverera hello disken och ange hello spåra antalet tootrack hello status för hello leverans. När hello disk anländer i hello datacenter, är det kopieras toohello storage-konto och hello status uppdateras.

    ![Slutförd status](./media/backup-azure-backup-import-export/complete.png)

### <a name="complete-hello-workflow"></a>Fullständig hello-arbetsflöde
När hello första säkerhetskopierade informationen är tillgänglig i ditt lagringskonto hello Microsoft Azure Recovery Services-agenten kopierar hello innehållet i hello data från det här kontot toohello säkerhetskopieringsvalvet eller Recovery Services-valvet, är det som tillämpligt. I nästa schema för hello utför säkerhetskopieringstiden hello Azure Backup agent hello inkrementell säkerhetskopiering över hello första säkerhetskopiering.

## <a name="next-steps"></a>Nästa steg
* Eventuella frågor om hello Azure Import/Export arbetsflöde finns för[använda hello Microsoft Azure Import/Export service tootransfer tooBlob datalagring](../storage/common/storage-import-export-service.md).
* Se toohello offlinesäkerhetskopiering avsnittet i hello Azure Backup [vanliga frågor och svar](backup-azure-backup-faq.md) för frågor om hello arbetsflöde.

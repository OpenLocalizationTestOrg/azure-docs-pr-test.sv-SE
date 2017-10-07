---
title: "aaaUsing Azure Import/Export tootransfer data tooand från blob storage | Microsoft Docs"
description: "Lär dig hur toocreate importera och exportera jobb i hello Azure-portalen för att överföra data tooand från blob storage."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 668f53f2-f5a4-48b5-9369-88ec5ea05eb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2017
ms.author: muralikk
ms.openlocfilehash: 84471d736d2433ffee9f49fbef91856d3dd56bb4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-microsoft-azure-importexport-service-tootransfer-data-tooblob-storage"></a>Använd hello Microsoft Azure Import/Export service tootransfer tooblob datalagring
hello Azure Import/Export service kan du toosecurely överför stora mängder data tooAzure blob-lagring av leverans hårddiskar tooan Azure-datacenter. Du kan också använda den här tjänsten tootransfer data från Azure blob storage toohard diskenheter och leverera tooyour lokal plats. Den här tjänsten är lämplig i situationer där du vill tootransfer flera terabyte (TB) data tooor från Azure, men att överföra eller hämta hello nätverket är omöjligt på grund av toolimited bandbredd eller hög nätverk kostnader.

hello-tjänsten kräver att hårddiskar ska vara BitLocker-krypterad för hello säkerheten för dina data. hello tjänsten stöder både hello klassisk och Azure Resource Manager storage-konton (standard och lågfrekvent nivå) finns i alla hello områden av offentliga Azure. Du måste leverera hårddiskar tooone hello stöds platser som beskrivs senare i den här artikeln.

I den här artikeln får du lära dig mer om hello Azure Import/Export-tjänsten och hur tooship enheter för att kopiera data-tooand från Azure Blob storage.

## <a name="when-should-i-use-hello-azure-importexport-service"></a>När bör jag använda hello Azure Import/Export service?
Överväg att använda Azure Import/Export service när du laddar upp eller hämta data via hello nätverket är långsamt eller att få ytterligare nätverksbandbredden är mycket höga kostnaden.

Du kan använda den här tjänsten i scenarier såsom:

* Migrera data toohello molnet: flytta stora mängder data tooAzure snabbt och kostnadseffektivt sätt.
* Innehållsdistribution: skicka data tooyour kundplatser.
* Säkerhetskopiering: Ta säkerhetskopior av dina lokala data toostore i Azure blob storage.
* Återställning av data: återställa stora mängder data som lagras i blob storage och dem tooyour lokal plats.

## <a name="prerequisites"></a>Krav
I det här avsnittet listas vi hello krav krävs toouse den här tjänsten. Granska dem noggrant innan du levererar dina enheter.

### <a name="storage-account"></a>Lagringskonto
Du måste ha en befintlig Azure-prenumeration och en eller flera konton toouse hello Import/Export lagringstjänsten. Varje jobb kanske används tootransfer data tooor från en enda lagringskonto. Med andra ord kan inte ett enda import/export-jobb vara över flera lagringskonton. Information om hur du skapar ett nytt lagringskonto finns [hur tooCreate ett Lagringskonto](storage-create-storage-account.md#create-a-storage-account).

### <a name="blob-types"></a>BLOB-typer
Du kan använda Azure Import/Export service toocopy data för**Block** blobbar eller **sidan** blobbar. Däremot kan du bara exportera **Block** blobar **sidan** blobbar eller **Append** blobbar från Azure storage med hjälp av den här tjänsten.

### <a name="job"></a>Jobb
toobegin hello processen med att importera tooor exportera från Blob storage kan du först skapa ett jobb. Ett jobb kan vara ett importjobb eller ett exportjobb:

* Skapa ett importjobb när du vill tootransfer data som du har lokala tooblobs i Azure storage-konto.
* Skapa ett exportjobb när du vill tootransfer data som lagras som blobar i ditt konto toohard lagringsenheter som är levererade tooyou.s när du skapar ett jobb, meddela hello Import/Export tjänst att du kommer att leverera en eller fler hårda tooan Azure-enheter Datacenter.

* För ett importjobb ska du leverera hårddiskar som innehåller dina data.
* För ett exportjobb ska du leverera tomt hårddiskar.
* Du kan leverera upp too10 hårddiskar per jobb.

Du kan skapa en importera eller exportera jobb med hjälp av hello Azure-portalen eller hello [Azure Storage Import/Export REST API](/rest/api/storageimportexport).

### <a name="waimportexport-tool"></a>WAImportExport-verktyget
hello första steget i att skapa en **importera** jobbet är tooprepare enheter som ska levereras för import. tooprepare enheter, måste du ansluta den lokala tooa-servern och kör hello WAImportExport verktyget på hello lokal server. Verktyget WAImportExport underlättar kopiera data toohello enheten, kryptera hello data på hello-enhet med BitLocker och generera hello enhet journal-filer.

hello journalfiler lagrar grundläggande information om jobbet och enheten, till exempel serienummer för enheten och lagringskontonamn. Ändringsjournalen filen lagras inte på hello enhet. Den används när importen jobbet skapades. Steg för steg finns information om jobb skapas senare i den här artikeln.

Hej WAImportExport verktyget är endast kompatibel med 64-bitars Windows-operativsystem. Se hello [operativsystemet](#operating-system) för specifika OS-versioner som stöds.

Hämta hello senaste versionen av hello [WAImportExport verktyget](http://download.microsoft.com/download/3/6/B/36BFF22A-91C3-4DFC-8717-7567D37D64C5/WAImportExportV2.zip). För mer information om hur du använder hello WAImportExport verktyget, se hello [Using hello WAImportExport verktyget](storage-import-export-tool-how-to.md).

>[!NOTE]
>**Tidigare Version:** kan du [hämta WAImportExpot V1](http://download.microsoft.com/download/0/C/D/0CD6ABA7-024F-4202-91A0-CE2656DCE413/WaImportExportV1.zip) version av hello verktyget och finns för[WAImportExpot V1 användning guiden](storage-import-export-tool-how-to-v1.md). WAImportExpot V1-versionen av verktyget hello ger stöd för **förbereda diskar när data skrivs redan före toohello disk**. Dessutom behöver toouse WAImportExpot V1-verktyget om hello endast nyckeln tillgängliga är SAS-nyckel.

>

### <a name="hard-disk-drives"></a>Hårddiskar
Endast 2,5 tum SSD eller 2,5-tums eller 3,5-tums SATA II eller III interna HDD stöds för användning med hello Import/Export service. Ett enda import/export-jobb kan ha högst 10 Hårddisk/SSD och varje enskild Hårddisk/SSD-enheter kan vara av valfri storlek. Stort antal enheter går att sprida över flera jobb och det finns ingen begränsning hello antalet jobb som kan skapas. 

Endast hello första datavolym på hello enhet för Importera projekt, kommer att bearbetas. hello datavolym måste vara formaterad med NTFS.

> [!IMPORTANT]
> Externa hårddiskar som levereras med en inbyggd USB-adapter stöds inte av den här tjänsten. Hello disk i hello versaler och gemener i en extern Hårddisk kan inte användas; Skicka inte externa hårddiskar.
> 
> 

Nedan visas en lista över externa USB-adaptrar används toocopy data toointernal hårddiskar. Anker 68UPSATAA - 02 för Anker 68UPSHHDS för Startech SATADOCK22UE Orico 6628SUS3 C BK (6628-serien) Thermaltake BlacX frekvent växlingen SATA externa hårddisken enhet dockningsstation (USB 2.0 & eSATA)

### <a name="encryption"></a>Kryptering
hello data på hello enheten måste krypteras med BitLocker-diskkryptering. Detta skyddar dina data när den är i överföringen.

Importera projekt finns det två sätt tooperform hello kryptering. hello första sättet är toospecify hello alternativ när du använder dataset CSV-fil när du kör verktyget för hello WAImportExport vid förberedelsen av enheten. hello andra sätt är tooenable BitLocker-kryptering manuellt på hello enheten och ange hello krypteringsnyckeln i hello driveset CSV när du kör kommandoraden i WAImportExport verktyget vid förberedelsen av enheten.

För exportjobb, när dina data är kopierade toohello enheter krypteras hello service hello-enheten med BitLocker innan den levereras tillbaka tooyou. hello krypteringsnyckeln anges tooyou via hello Azure-portalen.  

### <a name="operating-system"></a>Operativsystem
Du kan använda något av följande 64-bitars operativsystem tooprepare hello hårddisken med hjälp av hello WAImportExport verktyget innan du levererar hello enhet tooAzure hello:

Windows 7 Enterprise, Windows 7 Ultimate Windows 8 Pro, Windows 8 Enterprise, Windows 8.1 Pro, Windows 8.1 Enterprise, Windows 10<sup>1</sup>, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2. Alla dessa operativsystem stöder BitLocker-diskkryptering.

### <a name="locations"></a>Platser
hello Azure Import/Export service stöder kopierade data tooand från alla offentliga Azure-lagringskonton. Du kan leverera hårddiskar tooone av hello följande platser. Om ditt lagringskonto finns i en offentlig Azure-plats som anges här är en alternativ leveransplats anges när du skapar hello jobbet med hello Azure-portalen eller hello Import/Export REST API.

Leverans platser som stöds:

* Östra USA
* Västra USA
* Östra USA 2
* Västra USA 2
* Centrala USA
* Norra centrala USA
* Södra centrala USA
* Västra centrala USA
* Norra Europa
* Västra Europa
* Östasien
* Sydostasien
* Östra Australien
* Sydöstra Australien
* Västra Japan
* Östra Japan
* Indien, centrala
* Södra Indien
* Indien, västra
* Centrala Kanada
* Östra Kanada
* Södra Brasilien
* Centrala Korea
* Virginia (USA-förvaltad region)
* Iowa (USA-förvaltad region)
* US DoD, östra
* US DoD, centrala
* Östra Kina
* Norra Kina
* Storbritannien, södra

### <a name="shipping"></a>Leverans
**Leverans enheter toohello datacenter:**

När du skapar ett import- eller -jobb kan angav du en leveransadress för en av hello stöds platser tooship dina enheter. hello leveransadress som beror på hello platsen för ditt lagringskonto, men kanske inte hello samma som din lagringsplats för kontot.

Du kan använda leverantörer som FedEx DHL, UPS eller hello oss posttjänster tooship dina enheter toohello leveransadress.

**Leverans enheter från hello datacenter:**

När du skapar ett jobb som importeras eller exporteras, måste du ange en avsändaradress för Microsoft toouse när leverans hello enheter tillbaka när jobbet har slutförts. Kontrollera att du anger en giltig avsändaradress tooavoid fördröjningar i bearbetning.

Du kan använda en operatör önskat i ordning tooforward leverera hello hårddisk. hello operatör ska ha rätt spårning i ordning toomaintain spårbarhet. Du måste även ange en giltig FedEx eller DHL operatör konto antalet toobe som används av Microsoft för att leverera hello enheter tillbaka. En FedEx kontonummer krävs för leverans enheter tillbaka från hello USA och Europa platser. En DHL kontonummer krävs för leverans enheter tillbaka från Asien och Australien platser. Du kan skapa en [FedEx](http://www.fedex.com/us/oadr/) (för USA och Europa) eller [DHL](http://www.dhl.com/) (Asien och Australien) operatör konto om du inte har någon. Om du redan har en operatör kontonummer, kontrollerar du att den är giltig.

I leverans dina paket, måste du följa hello villkor på [licensvillkor för Microsoft Azure Service](https://azure.microsoft.com/support/legal/services-terms/).

> [!IMPORTANT]
> Observera att behöva hello fysiska media som du levererar toocross internationella gränser. Du ansvarar för att säkerställa att din fysiska media och dina data importeras eller exporteras i enlighet med hello tillämplig lagstiftning. Kontrollera med din rådgivare tooverify att dina media och data kan vara levererade toohello identifieras Datacenter juridiskt innan du levererar hello fysiska media. Detta hjälper tooensure att den når Microsoft inom rimlig tid. Alla paket som ska passera internationella gränser måste till exempel en faktura toobe tillsammans med hello-paket (utom om passerar gränser inom EU). Du kan skriva ut en fylld kopia av hello faktura från operatör webbplats. Exempel på fakturor är [DHL faktura](http://invoice-template.com/wp-content/uploads/dhl-commercial-invoice-template.pdf) och [FedEx faktura](http://images.fedex.com/downloads/shared/shipdocuments/blankforms/commercialinvoice.pdf). Kontrollera att Microsoft inte har angetts som hello Exportverktyget.
> 
> 

## <a name="how-does-hello-azure-importexport-service-work"></a>Hur fungerar hello Azure Import/Export service?
Du kan överföra data mellan lokal plats och Azure blob storage med hjälp av hello Azure Import/Export service genom att skapa jobb och leverera hårddiskar tooan Azure-datacenter. Varje hårddisk som du levererar är associerad med ett enda jobb. Varje jobb är associerat med ett enda storage-konto. Granska hello [förutsättningar avsnittet](#pre-requisites) noggrant toolearn om hello den här tjänsten som stöd för blob-typer, disktyper, platser och leverans.

I det här avsnittet vi beskriver på en hög nivå hello tillvägagångssättet för att importera och exportera jobben. Senare i hello [Snabbstart avsnittet](#quick-start), vi ger stegvisa instruktioner toocreate importera och exportera jobb.

### <a name="inside-an-import-job"></a>I ett importjobb
På en hög nivå omfattar ett importjobb hello följande steg:

* Bestäm hello data toobe importeras och hello antalet enheter som du behöver.
* Identifiera hello blob målplatsen för dina data i Blob storage.
* Använd hello WAImportExport verktyget toocopy data-tooone eller flera hårddiskar och kryptera dem med BitLocker.
* Skapa ett importjobb i dina mål-lagringskontot med hello Azure-portalen eller hello Import/Export REST API. Om du använder hello Azure-portalen, överför hello journalfiler för enheten.
* Ange hello avsändaradress och operatör konto nummer toobe används för leverans hello enheter tillbaka tooyou.
* Leverera hello hårddiskar toohello leveransadress tillhandahålls när jobbet skapas.
* Uppdatera hello leverans spårnings-ID i hello importera jobbinformation och skicka hello importjobbet.
* Enheter mottas och bearbetas på hello Azure-datacenter.
* Enheter levereras med operatör konto toohello returadressen i hello importjobbet.
  
    ![Bild 1:Import jobbet flöde](./media/storage-import-export-service/importjob.png)

### <a name="inside-an-export-job"></a>I ett exportjobb
På en hög nivå omfattar ett exportjobb hello följande steg:

* Fastställa hello data toobe exporteras och hello antalet enheter som du behöver.
* Identifiera hello källa blobbar eller behållaren sökvägar för data i Blob storage.
* Skapa ett exportjobb i din datakälla storage-konto med hello Azure-portalen eller hello Import/Export REST API.
* Ange hello källa blobbar eller behållaren sökvägar för dina data i hello exportera jobb.
* Ange hello returnerar adress och Operatörsnamn kontonummer för toobe som används för leverans hello enheter tillbaka tooyou.
* Leverera hello hårddiskar toohello leveransadress tillhandahålls när jobbet skapas.
* Uppdatera hello leverans spårnings-ID i hello export jobbinformation och skicka hello exportjobb.
* hello-enheter tas emot och bearbetas på hello Azure-datacenter.
* hello-enheter krypteras med BitLocker; hello nycklar är tillgängliga via hello Azure-portalen.  
* hello enheter levereras med operatör konto toohello returadressen i hello importjobbet.
  
    ![Bild 2:Export jobbet flöde](./media/storage-import-export-service/exportjob.png)

### <a name="viewing-your-job-and-drive-status"></a>Visa status för jobbet och enhet
Du kan övervaka hello status för din importera och exportera jobb från hello Azure-portalen. Klicka på hello **Import/Export** fliken. En lista över jobb visas på sidan hello.

![Visa jobbets status](./media/storage-import-export-service/jobstate.png)

Du ser något av följande jobbstatus beroende på om enheten är i hello processen hello.

| Jobbstatus | Beskrivning |
|:--- |:--- |
| Skapar | När ett jobb skapas, är dess status inställd tooCreating. Medan hello jobb i hello skapa tillstånd, förutsätter hello Import/Export service hello enheter inte har levererade toohello datacenter. Ett jobb kan befinna sig i hello skapa tillstånd för in tootwo veckor, efter vilken det automatiskt bort av hello-tjänsten. |
| Leverans | När du levererar paketet ska du uppdatera hello spårningsinformationen i hello Azure-portalen.  Detta kommer att stänga hello jobbet i ”leverans”. hello jobb finns kvar i hello leverans tillstånd för in tootwo veckor. 
| Tog emot | När alla enheter har tagits emot på hello datacenter, ange hello jobbets status toohello mottagna. |
| Överför | När minst en enhet har startat bearbetning, hello jobbets status anges vara toohello överföra. Se hello Enhetsstatus avsnittet nedan för mer information. |
| Paketering | När alla enheter har bearbetat kommer hello-jobbet att placeras i hello paketering tillstånd tills hello enheter är levererade tillbaka tooyou. |
| Slutfört | När alla enheter har levererade tillbaka toohello kunden om hello jobbet har slutförts utan fel, ställs sedan hello jobb in toohello slutfört tillstånd. hello jobbet tas automatiskt bort efter 90 dagar i hello slutfört tillstånd. |
| Stängd | När alla enheter har levererade tillbaka toohello kund, om det har gjorts några fel under hello bearbetningen av hello jobb, ställs sedan hello jobb in toohello stängd. hello jobbet tas automatiskt bort efter 90 dagar hello stängd. |

hello tabellen nedan beskrivs hello livscykeln för en enskild enhet som den övergår till ett jobb som importeras eller exporteras. hello aktuell status för varje enhet i ett jobb är nu synliga från hello Azure-portalen.
hello beskrivs följande tabell varje tillstånd varje enhet i ett jobb kan passera.

| Enhetsstatus | Beskrivning |
|:--- |:--- |
| Angivna | För ett importjobb är hello ursprungligt tillstånd för en enhet hello angivna tillstånd när hello jobb skapas från hello Azure-portalen. För ett exportjobb är hello inledande Enhetsstatus hello mottagna tillstånd eftersom ingen enhet anges när hello jobb skapas. |
| Tog emot | hello enhet övergår toohello mottagna tillstånd när hello Import/Export service operatorn har bearbetat hello-enheter som har tagits emot från hello leverans företag för importen. För ett exportjobb är hello inledande Enhetsstatus hello mottagna tillstånd. |
| NeverReceived | hello enhet flyttas toohello NeverReceived tillstånd när hello-paket för ett jobb anländer men hello paketet innehåller inte hello enhet. En enhet kan också flytta detta tillstånd om det har två veckor eftersom hello-tjänsten har tagit emot hello leveransinformation men hello paketet har inte kommit på hello datacenter. |
| Överför | En enhet flyttas toohello överföra tillstånd när hello service börjar tootransfer data från hello enhet tooWindows Azure Storage. |
| Slutfört | En enhet flyttas toohello slutförd tillstånd när hello-tjänsten har överförts alla hello data utan fel.
| CompletedMoreInfo | En enhet flyttas toohello CompletedMoreInfo tillstånd när hello tjänsten har påträffat några problem vid kopiering av data från eller toohello enhet. hello informationen kan innehålla fel, varningar och informationsmeddelanden om överskrivning blobbar.
| ShippedBack | hello enhet flyttas toohello ShippedBack tillstånd när den har levererats från hello data center tillbaka toohello avsändaradress. |

Den här avbildningen från hello Azure-portalen visar hello enhet tillståndet för en exempel-jobb:

![Visa status för enhet](./media/storage-import-export-service/drivestate.png)

hello följande tabell beskrivs hello enhet fel tillstånd och hello-åtgärder som vidtas för varje tillstånd.

| Enhetsstatus | Händelse | Lösning / nästa steg |
|:--- |:--- |:--- |
| NeverReceived | En enhet som har markerats som NeverReceived (eftersom den inte skickades som en del av hello jobbet leveransen) tas emot i en annan leveransen. | Hej driftteamet flyttas hello enhet toohello mottagna tillstånd. |
| Saknas | En enhet som inte är en del av jobb som når hello datacenter som en del av ett annat jobb. | hello enheten kommer att markeras som en extra enhet och returneras toohello Kund när hello jobb som är associerade med hello ursprungliga paketet har slutförts. |

### <a name="time-tooprocess-job"></a>Tooprocess jobbet
hello lång tid det tar ett import-/ exportjobb varierar beroende på olika faktorer, till exempel leveranstid jobbtypen tooprocess skriver och storleken på hello data kopieras och hello storleken på hello diskar som angetts. hello Import/Export-tjänsten har inte ett SLA men när hello diskar tas emot hello service strävar efter toocomplete hello kopia 7 too10 dagarna. Du kan använda hello REST API tootrack hello jobbförloppet närmare. Det finns en procent fullständig parameter i hello lista jobb åtgärden, vilket ger en indikation om Kopiera pågår. Nå ut toous om du behöver en uppskattning toocomplete en kritisk import/export av jobbet.

### <a name="pricing"></a>Prissättning
**Enheten som hanterar avgift**

Det finns en avgift för hantering av enheten för varje enhet som behandlas som en del av din importera eller exportera jobb. Visa hello information om hello [priser för Azure Import/Export](https://azure.microsoft.com/pricing/details/storage-import-export/).

**Leverera kostnader**

När du levererar enheter tooAzure betalar hello leverans kostnaden toohello leverans operatör. När Microsoft returnerar hello enheter tooyou debiteras hello leverans kostnad toohello operatör konto som du har angett på hello tid då jobbet skapades.

**Transaktionskostnader**

Det finns inga transaktionskostnader när du importerar data till blob-lagring. hello standard utgång avgifter kan användas när exporteras data från blob storage. Mer information om transaktionskostnader finns [dataöverföring priser.](https://azure.microsoft.com/pricing/details/data-transfers/)

## <a name="quick-start"></a>Snabbstart
I det här avsnittet ger vi stegvisa instruktioner för att skapa en import och exportjobbet. Kontrollera att du uppfyller alla hello [förutsättningar](#pre-requisites) innan du går vidare.

> [!IMPORTANT]
> hello tjänsten stöder en standardlagringskonto per importera eller exportera jobb och har inte stöd för premium-lagringskonton. 
> 
> 
## <a name="create-an-import-job"></a>Skapa ett importjobb
Skapa en import jobbet toocopy data tooyour Azure storage-konto från hårddiskar genom att leverera en eller flera enheter som innehåller data toohello angivna datacenter. hello importjobbet förmedlar information om hårddiskenheter kopieras data toobe, mål-lagringskontot och leverera information toohello Azure Import/Export service. Att skapa ett importjobb är en process i tre steg. Börja med att förbereda dina enheter med hello WAImportExport verktyg. Andra kan skicka ett importjobb med hello Azure-portalen. Det tredje leverera hello enheter toohello leveransadress som angavs under jobbet Skapa och uppdatera hello leverera information i din jobbinformation.   

### <a name="prepare-your-drives"></a>Förbereda dina enheter
Hej första steget när du importerar data med hjälp av hello Azure Import/Export service är tooprepare dina enheter med hello WAImportExport verktyg. Åtgärderna hello nedan tooprepare dina enheter.

1. Identifiera hello data toobe importeras. Detta kan bero på kataloger och filer för fristående på hello lokal server eller en nätverksresurs.  
2. Fastställa hello antalet enheter som du behöver beroende på storleken på hello data. Skaffa hello krävs antalet 2,5 tum SSD eller 2,5-tums eller 3,5-tums SATA II eller III hårddiskar.
3. Identifiera hello mål-lagringskontot, behållare, virtuella kataloger och blobbar.
4.  Fastställa hello kataloger och/eller fristående filer som ska kopieras tooeach hårddisk.
5.  Skapa hello CSV-filer för dataset och driveset.
    
    **DataSet CSV-fil**
    
    Nedan visas ett exempel på exempel dataset CSV-fil:
    
    ```
    BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
    "F:\50M_original\100M_1.csv.txt","containername/100M_1.csv.txt",BlockBlob,rename,"None",None
    "F:\50M_original\","containername/",BlockBlob,rename,"None",None 
    ```
   
    I ovanstående exempel hello, 100M_1.csv.txt inte kopierade toohello rot hello behållare med namnet ”containername”. Om containername ”hello behållarens namn” inte finns, skapas en. Alla filer och mappar under 50M_original blir rekursivt kopieras toocontainername. Mappstruktur bibehålls.

    Lär dig mer om [förbereder hello dataset CSV-filen](storage-import-export-tool-preparing-hard-drives-import.md#prepare-the-dataset-csv-file).
    
    **Kom ihåg**: som standard hello data kommer att importeras som Blockblobar. Du kan använda hello BlobType fältvärdet tooimport data som en Sidblobar. Om du importerar VHD-filer som ska monteras som diskar på en virtuell dator i Azure måste du till exempel importera dem som Sidblobar.

    **Driveset CSV-fil**

    hello-värdet för hello driveset flaggan är en CSV-fil som innehåller hello lista över diskar toowhich hello enhetsbeteckningar mappas för hello verktyget toocorrectly Välj hello lista över diskar toobe förberedd. 

    Nedan finns hello exempel i driveset CSV-fil:
    
    ```
    DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
    G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631 |
    H,Format,SilentMode,Encrypt,
    ```

    I ovanstående exempel hello förutsätts det att två diskar har anslutits och grundläggande NTFS-volymer med volymbeteckning G:\ och H:\ har skapats. hello verktyget kommer att formatera och kryptera hello-disk som är värd för H:\ och formatera inte eller kryptera hello disk som är värd för volymen G:\.

    Lär dig mer om [förbereder hello driveset CSV-filen](storage-import-export-tool-preparing-hard-drives-import.md#prepare-initialdriveset-or-additionaldriveset-csv-file).

6.  Använd hello [WAImportExport verktyget](http://download.microsoft.com/download/3/6/B/36BFF22A-91C3-4DFC-8717-7567D37D64C5/WAImportExport.zip) toocopy data-tooone eller hårddiskar.
7.  Du kan ange ”kryptera” på kryptering fält i drivset CSV tooenable BitLocker-kryptering på hello hårddisk. Du kan också aktivera BitLocker-kryptering manuellt på hello hårddisk och ange ”AlreadyEncrypted” och ange hello hello driveset CSV-nyckel när du kör hello-verktyget.

8. Ändra inte hello data på hello hårddiskar eller hello journal-fil när du har slutfört förberedelse av disken.

> [!IMPORTANT]
> Varje hårddisk som du förbereder resulterar i en journal-fil. När du skapar hello importjobbet med hello Azure-portalen måste du överföra alla hello journalfiler hello-enheter som ingår i det importjobbet. Enheter utan journalen inte kommer att bearbeta filer.
> 
>

Nedan hello kommandon och exempel för att förbereda hello hårddisken använder WAImportExport verktyget.

WAImportExport verktyget PrepImport kommandot för hello först kopiera session toocopy kataloger och/eller filer med en ny kopia-session:

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] [/sk:<StorageAccountKey>] [/silentmode] [/InitialDriveSet:<driveset.csv>] DataSet:<dataset.csv>
```

**Importera exempel 1**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:************* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

I ordning för**lägga till fler enheter**, skapa en ny driveset-fil och kör kommandot hello som nedan. Ange en ny driveset CSV-fil för efterföljande kopiera sessioner toohello olika enheter än vad som anges i InitialDriveset CSV-filen, och ange den som en parameter för värde toohello ”AdditionalDriveSet”. Använd hello **samma journalfil** ge namn och en **nytt sessions-ID**. hello-formatet för AdditionalDriveset CSV-fil är samma som InitialDriveSet format.

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /AdditionalDriveSet:<driveset.csv>
```

**Importera exempel 2**
```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3  /AdditionalDriveSet:driveset-2.csv
```

I ordning tooadd ytterligare data toohello samma driveset WAImportExport verktyget PrepImport kommando kan anropas för efterföljande kopiera sessioner toocopy ytterligare filer/directory: för efterföljande kopiera sessioner toohello samma-hårddiskar anges i InitialDriveset .csv fil, ange hello **samma journalfil** ge namn och en **nytt sessions-ID**; det finns inget behov av tooprovide hello lagringskontonyckel.

```
WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] DataSet:<dataset.csv>
```

**Importera exempel 3**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
```

Se mer information om hur du använder hello WAImportExport i [förbereder hårddiskar för import](storage-import-export-tool-preparing-hard-drives-import.md).

Dessutom finns toohello [exempel arbetsflödet tooprepare hårddiskar för ett importjobb](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow.md) detaljerade steg för steg-instruktioner.  

### <a name="create-hello-import-job"></a>Skapa hello-importjobb
1. När du har förberett din enhet, navigera tooyour storage-konto i hello Azure-portalen och visa hello instrumentpanelen. Under **snabb i korthet**, klickar du på **skapa ett importjobb**. Granska hello steg och markera kryssrutan hello tooindicate att du har förberett din enhet och att du har hello enhet journal-fil tillgänglig.
2. I steg 1, anger du kontaktinformation för hello är ansvarig för det här importjobbet och giltig adress. Om du vill toosave utförlig loggdata för hello importjobbet markerar hello alternativet för**spara hello utförlig logg i min 'waimportexport' blob-behållaren**.
3. Överför hello enhet journal-filer som du fick vid förberedelsen för hello enheten i steg 2. Du behöver tooupload en fil för varje enhet som du har förberett.
   
   ![Skapa importjobb - steg3](./media/storage-import-export-service/import-job-03.png)
4. Ange ett beskrivande namn för hello importjobb i steg3. Observera att hello-namn som du anger kan innehålla endast små bokstäver, siffror, bindestreck och understreck, måste börja med en bokstav och får inte innehålla blanksteg. Du använder hello namn du väljer tootrack jobb när de pågår och när de har slutförts.
   
   Välj sedan din center dataområdet hello-listan. hello dataområdet center visar hello data center och åtgärda toowhich måste du levererar paketet. Se hello vanliga frågor om nedan för mer information.
5. Välj returnerade prefix hello listan i steg 4, och ange ditt operatör kontonummer. Microsoft använder det här kontot tooship hello enheter tillbaka tooyou när importjobbet har slutförts.
   
   Om du har Spårningsnumret till din, Välj leverans prefix hello listan och ange Spårningsnumret till din.
   
   Om du inte har en spårning number ännu, Välj **jag ger Mina leveransinformation för det här importjobbet när jag har levererats min paketet**, Slutför hello importen.
6. tooenter Spårningsnumret till din när du har skickat paketet, returnera toohello **Import/Export** för ditt lagringskonto i hello Azure-portalen, Välj ditt jobb hello listan och välj **leverans Info**. Navigera hello guiden och ange Spårningsnumret till din i steg 2.
   
    Om hello spårnings-ID inte har uppdaterats inom två veckor för att skapa hello jobbet hello jobbet att löpa ut.
   
    Du kan också uppdatera ditt operatör kontonummer i steg 2 hello guiden om hello jobbet i hello skapa, leverans eller överföra läge. När hello jobbet är i hello paketering tillstånd, kan du inte uppdatera din operatör kontonummer för jobbet.
7. Du kan spåra jobbförloppet på hello portalens instrumentpanel. Se vad varje Jobbstatus i föregående avsnitt i hello innebär av [visa status för din](#viewing-your-job-status).

## <a name="create-an-export-job"></a>Skapa ett exportjobb
Skapa en export-jobbet toonotify hello Import/Export service att du kommer att skicka en eller flera tomma enheter toohello Datacenter så att data kan exporteras från din lagringsenheter konto toohello och hello enheter sedan levererats tooyou.

### <a name="prepare-your-drives"></a>Förbereda dina enheter
Inledande kontroller i följande rekommenderas för att förbereda dina enheter för ett exportjobb:

1. Kontrollera hello antalet diskar som krävs med hjälp av verktyget hello-WAImportExport PreviewExport kommando. Mer information finns i [Förhandsgranska enhet användning för ett jobb exportera](https://msdn.microsoft.com/library/azure/dn722414.aspx). Det hjälper dig att förhandsgranska diskanvändning för hello blob som du har valt, baserat på hello storlek hello enheter du kommer toouse.
2. Kontrollera att du kan läsa/skriva toohello hårddisk som ska levereras för hello exportjobb.

### <a name="create-hello-export-job"></a>Skapa hello exportjobb
1. toocreate ett exportjobb navigera tooyour storage-konto i hello Azure-portalen och visa hello instrumentpanelen. Under **snabb i korthet**, klickar du på **skapa ett exportera jobb** och gå igenom guiden hello.
2. Ange kontaktinformation för hello person som ansvarar för den här exportjobb i steg 2. Om du vill toosave utförlig loggdata för hello exportjobb markerar hello alternativet för**spara hello utförlig logg i min 'waimportexport' blob-behållaren**.
3. Ange vilka blob data som du vill tooexport från din lagring konto tooyour tomt enhet eller enheter i steg3. Du kan välja tooexport alla blob-data i hello storage-konto eller du kan ange som BLOB-objekt eller grupper med blobbar tooexport.
   
   toospecify en blob-tooexport använder hello **lika med** selector, och ange hello relativ sökväg toohello blob, från och med hello behållarnamn. Använd *$root* toospecify hello Rotbehållare.
   
   toospecify alla blobbar börjar med ett prefix, Använd hello **börjar med** selector, och ange hello-prefix som börjar med ett snedstreck '/'. hello prefix kanske hello prefixet hello behållarnamn, hello fullständig behållarens namn eller hello fullständig behållarens namn följt av hello prefixet hello blob-namnet.
   
   hello följande tabell visas exempel på giltiga blob-sökvägar:
   
   | Selector | BLOB-sökväg | Beskrivning |
   | --- | --- | --- |
   | Börjar med |/ |Exporterar alla blobbar i hello storage-konto |
   | Börjar med |/$root / |Exporterar alla blobbar i behållaren för hello rot |
   | Börjar med |/Book |Exporterar alla blobbar i en behållare som börjar med prefixet **book** |
   | Börjar med |/Music/ |Exporterar alla blobbar i behållaren **musik** |
   | Börjar med |/ musik/kärlek |Exporterar alla blobbar i behållaren **musik** som börjar med prefixet **gillar** |
   | För är lika med|$root/logo.bmp |Export blob **logo.bmp** i hello Rotbehållare |
   | För är lika med|videos/Story.mp4 |Export blob **story.mp4** i behållaren **videor** |
   
   Du måste ange hello blob sökvägar i ett giltigt format tooavoid fel under bearbetning, som visas i den här skärmbilden.
   
   ![Skapa exportjobb - steg3](./media/storage-import-export-service/export-job-03.png)
4. Ange ett beskrivande namn för hello exportjobb i steg 4. hello-namn som du anger kan innehålla endast små bokstäver, siffror, bindestreck och understreck, måste börja med en bokstav och får inte innehålla blanksteg.
   
   hello dataområdet center visar hello data center toowhich måste du levererar paketet. Se hello vanliga frågor om nedan för mer information.
5. Välj returnerade prefix hello listan i steg 5 och ange din operatör kontonummer. Microsoft kommer att använda det här kontot tooship enheter tillbaka tooyou när exportjobbet har slutförts.
   
   Om du har Spårningsnumret till din, Välj leverans prefix hello listan och ange Spårningsnumret till din.
   
   Om du inte har en spårning number ännu, Välj **jag ger Mina leveransinformation för den här exportjobb när jag har levererats min paketet**, Slutför hello exporten.
6. tooenter Spårningsnumret till din när du har skickat paketet, returnera toohello **Import/Export** för ditt lagringskonto i hello Azure-portalen, Välj ditt jobb hello listan och välj **leverans Info**. Navigera hello guiden och ange Spårningsnumret till din i steg 2.
   
    Om hello spårnings-ID inte har uppdaterats inom två veckor för att skapa hello jobbet hello jobbet att löpa ut.
   
    Du kan också uppdatera ditt operatör kontonummer i steg 2 hello guiden om hello jobbet i hello skapa, leverans eller överföra läge. När hello jobbet är i hello paketering tillstånd, kan du inte uppdatera din operatör kontonummer för jobbet.
   
   > [!NOTE]
   > Om hello blob toobe exporteras används för närvarande hello kopiera toohard enhet, kommer Azure Import/Export service ta en ögonblicksbild av hello blob och kopiera hello ögonblicksbild.
   > 
   > 
7. Du kan spåra jobbförloppet på hello instrumentpanelen i hello Azure-portalen. Se vad varje jobbets status innebär hello föregående avsnitt på ”Visa jobbstatus för”.
8. När du har fått hello enheter med exporterade data kan du visa och kopiera hello BitLocker-nycklar som genereras av hello-tjänsten för enheten. Navigera tooyour storage-konto i hello Azure-portalen och hello Import/Export-fliken. Välj din exportjobb hello listan och klicka hello visa nycklar. hello BitLocker-nycklar visas enligt nedan:
   
   ![Visa BitLocker-nycklar för exportjobb](./media/storage-import-export-service/export-job-bitlocker-keys.png)

Gå igenom hello vanliga frågor om avsnittet nedan som det täcker de vanligaste hello frågor som uppstår när du använder den här tjänsten.

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar

**Kan jag kopiera Azure File storage med hjälp av hello Azure Import/Export service?**

Nej, hello Azure Import/Export service endast har stöd för Blockblobbar och Sidblobbar. Alla andra lagringstyper inklusive Azure File storage, Table Storage och Queue Storage stöds inte.

**Finns hello Azure Import/Export service för CSP-prenumerationer?**

Azure Import/Export-tjänsten stöder CSP-prenumerationer.

**Kan jag hoppa över hello enhet förberedelsen för ett importjobb eller kan jag förbereda en enhet utan att kopiera?**

Alla enheter som du vill tooship för import av data måste förberedas med hello Azure WAImportExport verktyg. Du måste använda hello WAImportExport verktyget toocopy toohello dataenhet.

**Behöver jag tooperform alla förberedelse av disk när du skapar ett exportjobb?**

Inledande kontroller i ingen, men vissa rekommenderas. Kontrollera hello antalet diskar som krävs med hjälp av verktyget hello-WAImportExport PreviewExport kommando. Mer information finns i [Förhandsgranska enhet användning för ett jobb exportera](https://msdn.microsoft.com/library/azure/dn722414.aspx). Det hjälper dig att förhandsgranska diskanvändning för hello blob som du har valt, baserat på hello storlek hello enheter du kommer toouse. Kontrollera att du kan läsa från och skriva toohello hårddisk som ska levereras för hello du också exportera jobb.

**Vad händer om jag av misstag skickar en Hårddisk som inte följer toohello stöds krav?**

hello-enhet som inte följer toohello stöds krav tooyou returnerar hello Azure-datacenter. Om endast vissa hello-enheter i hello paketet uppfyller kraven för stöd av hello dessa enheter kommer att bearbetas och hello-enheter som inte uppfyller kraven för hello returneras tooyou.

**Kan jag avbryts min?**

Du kan avbryta ett jobb när dess status skapades eller leverans.

**Hur länge visar hello status för slutförda jobb i hello Azure-portalen?**

Du kan visa hello status för slutförda jobb för in too90 dagar. Slutförda jobb tas bort efter 90 dagar.

**Vad gör jag om jag vill tooimport eller exportera fler än 10 enheter?**

En import eller exportjobb kan referera endast 10-enheter i ett enda jobb för hello Import/Export-tjänsten. Om du vill tooship fler än 10 enheter, kan du skapa flera jobb. Enheter som är associerade med samma jobb måste levereras tillsammans i hello hello samma paket.
Microsoft erbjuder vägledning och hjälp när datakapacitet sträcker sig över flera disk importera jobb. Kontakta bulkimport@microsoft.com för mer information

**Hello service format hello enheter innan du går tillbaka dem?**

Nej. Alla enheter krypteras med BitLocker.

**Kan jag köpa enheter för import/export av jobb från Microsoft?**

Nej. Du behöver tooship egna enheter för både importera och exportera jobben.

** Hur kan jag komma åt data som importeras av den här tjänsten **

hello data under Azure storage-konto kan nås via hello Azure-portalen eller med ett fristående verktyg som kallas Lagringsutforskaren. https://docs.microsoft.com/en-us/Azure/VS-Azure-Tools-Storage-Manage-with-Storage-Explorer 

**När hello importera jobbet är slutfört, vad kommer Mina data ut i hello storage-konto? Min kataloghierarkin bevaras?**

När du förbereder en hårddisk på en importjobb anges hello mål av hello DstBlobPathOrPrefix fält i datamängden CSV. Detta är hello målbehållare i hello storage-konto toowhich kopiera data från hello hårddisk. I den här målbehållare virtuella kataloger har skapats för mappar från hello hårddisken och blobbar skapas för filer. 

**Om hello enheten innehåller filer som redan finns i mitt lagringskonto, hello service skrivs befintliga blobbar i mitt lagringskonto?**

När du förbereder hello enhet, du kan ange om hello filer ska skrivas över eller ignoreras med hello fält i datamängden CSV-fil med namnet Disposition: < Byt namn på | Nej skriva över | skriva över >. Som standard hello tjänsten kommer Byt namn på hello nya filer i stället skriva över befintliga blobbar.

**Är hello WAImportExport verktyg som är kompatibel med 32-bitars operativsystem?**
Nej. Hej WAImportExport verktyget är endast kompatibel med 64-bitars Windows-operativsystem. Se toohello operativsystem avsnitt i hello [förutsättningar](#pre-requisites) för en fullständig lista över operativsystemversioner som stöds.

**Bör jag ta något annat än hello hårddisk i min paketet?**

Kontrollera levereras endast dina hårddiskar. Inkludera inte ange strömkablar eller USB-kablar.

**Måste jag tooship mina enheter som använder FedEx eller DHL?**

Du kan leverera enheter toohello datacenter med några kända operatör som FedEx DHL, UPS eller oss postgång. För leverans hello enheter tillbaka tooyou från hello datacenter, måste du dock ange en FedEx kontonummer i hello USA och EU eller en DHL kontonummer i hello Asien och Australien regioner.

**Finns det några begränsningar med leverans internationellt min enhet?**

Observera att behöva hello fysiska media som du levererar toocross internationella gränser. Du ansvarar för att säkerställa att din fysiska media och dina data importeras eller exporteras i enlighet med hello tillämplig lagstiftning. Kontrollera med din rådgivare tooverify att dina media och data kan vara levererade toohello identifieras Datacenter juridiskt innan du levererar hello fysiska media. Detta hjälper tooensure att den når Microsoft inom rimlig tid.

**När du skapar ett jobb är en plats som skiljer sig från min lagringskontoplatsen hello leveransadress. Vad ska jag göra?**

Vissa lagringsplatser för kontot är mappade tooalternate leverans platser. Tidigare kan tillgängliga leverans platser också vara tillfälligt mappade tooalternate platser. Kontrollera alltid hello leveransadress tillhandahålls när jobbet skapas innan du levererar dina enheter.

**När leverans enheten begär hello operatör hello data center adress och telefonnummer numret. Vad bör jag ge?**

Hej telefonnummer och DC-adress anges som en del av jobbet skapas.

**Kan jag använda hello Azure Import/Export service toocopy PST postlådor och tooO365 för SharePoint-data?**

Se för[Importera PST-filer eller SharePoint data tooOffice 365](https://technet.microsoft.com/library/ms.o365.cc.ingestionhelp.aspx).

**Kan jag använda hello Azure Import/Export service toocopy min säkerhetskopieringar offline toohello Azure Backup-tjänsten?**

Se för[Offline säkerhetskopiering arbetsflöde i Azure Backup](../backup/backup-azure-backup-import-export.md).

**Vad är hello maximalt antal Hårddisk för i en leverans?**

Valfritt antal hårddiskar kan vara i en leverans och om hello diskar tillhör toomultiple jobb rekommenderas för en) ha hello diskar med hello motsvarande namn i jobbet. b) hello uppdateringsjobb med flera spårning suffixet med -1,-2 osv.
  
**Vad är hello maximala Blockblob och sidstorlek Blob som stöds av Disk Import/Export?**

Max Blockblob är ungefär 4.768TB eller 5,000,000 MB.
Max Page Blob är 1TB.

**Import/Export-Disk som har stöd för AES 256-kryptering?**

Azure Import/Export-tjänsten som standard krypterar med AES 128 bitlocker-kryptering, men det kan vara bättre tooAES 256 genom att manuellt krypteras med bitlocker innan data kopieras. 

Om du använder [WAImportExpot V1](http://download.microsoft.com/download/0/C/D/0CD6ABA7-024F-4202-91A0-CE2656DCE413/WaImportExportV1.zip), nedan visas ett exempel på kommando
```
WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>] 
```
Om du använder [WAImportExport verktyget](http://download.microsoft.com/download/3/6/B/36BFF22A-91C3-4DFC-8717-7567D37D64C5/WAImportExport.zip) ange ”AlreadyEncrypted” och ange hello nyckeln i hello driveset CSV.
```
DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631 |
```
## <a name="next-steps"></a>Nästa steg

* [Konfigurera hello WAImportExport verktyget](storage-import-export-tool-how-to.md)
* [Överföra data med hello kommandoradsverktyget azcopy](storage-use-azcopy.md)
* [Azure Import exportera REST API-exemplet](https://azure.microsoft.com/documentation/samples/storage-dotnet-import-export-job-management/)


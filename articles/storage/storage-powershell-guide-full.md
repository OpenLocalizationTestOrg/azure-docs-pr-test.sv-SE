---
title: aaaUsing Azure PowerShell med Azure Storage | Microsoft Docs
description: "Lär dig hur toouse hello Azure PowerShell-cmdlets för Azure Storage toocreate och hantera lagringskonton; Arbeta med blobbar, tabeller, köer och -filer. Konfigurera och efterfråga storage analytics och skapa signaturer för delad åtkomst."
services: storage
documentationcenter: na
author: robinsh
manager: timlt
ms.assetid: f4704f58-abc6-4f89-8b6d-1b1659746f5a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/03/2017
ms.author: robinsh
ms.openlocfilehash: befe7adda2384f8bcdb8b9f1a063e4eafc158271
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-powershell-with-azure-storage"></a>Använda Azure PowerShell med Azure Storage
## <a name="overview"></a>Översikt
Azure PowerShell är en modul som tillhandahåller cmdlets toomanage Azure via Windows PowerShell. Det är ett uppgiftsbaserat kommandoradsgränssnitt och skriptspråk som utformats specifikt för systemadministration. Du kan enkelt styra och automatisera hello administration av dina Azure-tjänster och program med PowerShell. Du kan till exempel använda samma uppgifter som du kan utföra via hello hello cmdlets tooperform hello [Azure-portalen](https://portal.azure.com).

I den här handboken vi ska visa hur toouse hello [Azure Storage-Cmdlets](/powershell/module/azurerm.storage/#storage) tooperform en rad olika uppgifter för utveckling och administration med Azure Storage.

Den här guiden förutsätter att du har tidigare erfarenhet med [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) och [Windows PowerShell](http://technet.microsoft.com/library/bb978526.aspx). hello guiden ger ett antal skript toodemonstrate hello användning av PowerShell med Azure Storage. Du bör uppdatera hello skriptvariabler baserat på din konfiguration innan du kör varje skript.

hello första delen i den här guiden ger en snabb överblick över vid Azure Storage och PowerShell. Detaljerad information och instruktioner som startar från hello [krav för att använda Azure PowerShell med Azure Storage](#prerequisites-for-using-azure-powershell-with-azure-storage).

## <a name="getting-started-with-azure-storage-and-powershell-in-5-minutes"></a>Komma igång med Azure Storage och PowerShell 5 minuter
Det här avsnittet beskrivs hur du tooaccess Azure Storage via PowerShell 5 minuter.

**Nya tooAzure:** hämta Microsoft Azure-prenumeration och ett Microsoft-konto som är kopplade till den prenumerationen. Mer information om köpalternativ för Azure finns [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/), [köpa alternativ](https://azure.microsoft.com/pricing/purchase-options/), och [Medlemserbjudanden](https://azure.microsoft.com/pricing/member-offers/) (för medlemmar i MSDN, Microsoft Partner Network, BizSpark och att andra Microsoft-program).

Se [Tilldela administratörsroller i Azure Active Directory (AD Azure)](https://msdn.microsoft.com/library/azure/hh531793.aspx) för mer information om Azure-prenumerationer.

**När du har skapat ett Microsoft Azure-prenumeration och konto:**

1. Hämta och installera hello senaste [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/latest).
2. Starta Windows PowerShell Integrated Scripting Environment (ISE): Den lokala datorn, gå i toohello **starta** menyn. Typen **Administrationsverktyg** och klicka på toorun den. I hello **Administrationsverktyg** fönstret, högerklicka på **Windows PowerShell ISE**, klickar du på **kör som administratör**.
3. I **Windows PowerShell ISE**, klickar du på **filen** > **ny** toocreate en ny skriptfil.
4. Nu får du ett enkelt skript som visar grundläggande PowerShell-kommandon tooaccess Azure Storage. hello skript ber först ditt Azure-konto autentiseringsuppgifter tooadd din Azure-konto toohello lokala PowerShell-miljö. Hello-skriptet kommer sedan ange hello standardvärdet Azure-prenumeration och skapa ett nytt lagringskonto i Azure. Hello-skriptet kommer sedan skapa en ny behållare i den här nya storage-konto och överför en befintlig avbildning fil (blob) toothat behållare. När hello-skriptet visar alla blobbar i behållaren, kommer den skapa en ny målkatalog i den lokala datorn och hämta hello image-filen.
5. I följande avsnitt med exempelkod hello, väljer du hello skript mellan hello anmärkningar **#begin** och **#end**. Tryck på CTRL + C toocopy den toohello Urklipp.

    ```powershell
    # begin
    # Update with hello name of your subscription.
    $SubscriptionName = "YourSubscriptionName"
       
    # Give a name tooyour new storage account. It must be lowercase!
    $StorageAccountName = "yourstorageaccountname"
       
    # Choose "West US" as an example.
    $Location = "West US"
       
    # Give a name tooyour new container.
    $ContainerName = "imagecontainer"
       
    # Have an image file and a source directory in your local computer.
    $ImageToUpload = "C:\Images\HelloWorld.png"
       
    # A destination directory in your local computer.
    $DestinationFolder = "C:\DownloadImages"
       
    # Add your Azure account toohello local PowerShell environment.
    Add-AzureAccount
       
    # Set a default Azure subscription.
    Select-AzureSubscription -SubscriptionName $SubscriptionName –Default
       
    # Create a new storage account.
    New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $Location
       
    # Set a default storage account.
    Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName
       
    # Create a new container.
    New-AzureStorageContainer -Name $ContainerName -Permission Off
       
    # Upload a blob into a container.
    Set-AzureStorageBlobContent -Container $ContainerName -File $ImageToUpload
       
    # List all blobs in a container.
    Get-AzureStorageBlob -Container $ContainerName
       
    # Download blobs from hello container:
    # Get a reference tooa list of all blobs in a container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName
       
    # Create hello destination directory.
    New-Item -Path $DestinationFolder -ItemType Directory -Force  
       
    # Download blobs into hello local destination directory.
    $blobs | Get-AzureStorageBlobContent –Destination $DestinationFolder
       
    # end
    ```

6. I **Windows PowerShell ISE**, tryck på CTRL + V toocopy hello skript. Klicka på **filen** > **spara**. I hello **Spara som** dialogruta, hello-typnamn för hello skriptfil, till exempel ”mystoragescript”. Klicka på **Spara**.
7. Du måste nu tooupdate hello skriptvariabler baserat på inställningarna. Du måste uppdatera hello **$SubscriptionName** variabeln med din egen prenumeration. Du kan behålla hello andra variabler som anges i hello skript eller uppdatera dem som du vill.
   
   * **$SubscriptionName:** måste du uppdatera den här variabeln med prenumerationsnamn på din. Gör något av följande tre sätt toolocate hello namnet på din prenumeration hello:
     
    a. I **Windows PowerShell ISE**, klickar du på **filen** > **ny** toocreate en ny skriptfil. Kopiera hello följande skript toohello nya skriptfilen och på **felsöka** > **kör**. hello följande skript kommer först be din Azure-konto autentiseringsuppgifter tooadd din Azure-konto toohello lokala PowerShell-miljö och sedan visa alla hello-prenumerationer som är anslutna toohello lokala PowerShell-session. Anteckna hello namn på hello prenumeration som du vill toouse när följa de här självstudierna:
     
    ```powershell
    Add-AzureAccount 
      Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName
    ```

    b. toolocate och kopiera prenumerationen namn i hello [Azure-portalen](https://portal.azure.com), i hello hubbmenyn på hello vänster, klicka på **prenumerationer**. Kopiera hello namn av prenumeration som du vill toouse när du kör hello skript i den här guiden.
     
     ![Azure Portal](./media/storage-powershell-guide-full/Subscription_Previewportal.png)

    c. toolocate och kopiera prenumerationen namn i hello [klassiska Azure-portalen](https://manage.windowsazure.com/), bläddra nedåt och klicka på **inställningar** på vänster sida av hello portal hello. Klicka på **prenumerationer** toosee en lista över dina prenumerationer. Kopiera hello namn av prenumeration som du vill toouse när du kör hello-skript som anges i den här guiden.
     
     ![Klassisk Azure-portal](./media/storage-powershell-guide-full/Subscription_currentportal.png)

   * **$StorageAccountName:** använda hello får namn i skriptet hello eller ange ett nytt namn för ditt lagringskonto. **Viktigt:** hello namnet på hello storage-konto måste vara unikt i Azure. Det måste vara gemena för!
   * **$Location:** använda hello anges ”USA, västra” i hello skript eller välj andra Azure platser, till exempel östra USA, Norra Europa och så vidare.
   * **$ContainerName:** använda hello får namn i skriptet hello eller ange ett nytt namn för din behållare.
   * **$ImageToUpload:** ange en sökväg tooa bild på den lokala datorn, till exempel: ”C:\Images\HelloWorld.png”.
   * **$DestinationFolder:** ange sökvägen tooa lokal katalog toostore filer som hämtas från Azure Storage: ”C:\DownloadImages”.
8. När du har uppdaterat hello skriptvariabler i hello ”mystoragescript.ps1”-fil klickar du på **filen** > **spara**. Klicka på **felsöka** > **kör** eller tryck på **F5** toorun hello skript.  

När hello skriptet körs, bör du ha en lokal målmapp som innehåller hello hämtade image-filen. hello följande skärmbild visar ett exempel på utdata:

![Ladda ned Blobbar](./media/storage-powershell-guide-full/Blobdownload.png)

> [!NOTE]
> hello ”avsnittet komma igång med Azure Storage och PowerShell 5 minuter” tillhandahålls en snabb introduktion på hur toouse Azure PowerShell med Azure Storage. För detaljerad information och instruktioner, rekommenderar vi att du tooread hello följande avsnitt.
> 

## <a name="prerequisites-for-using-azure-powershell-with-azure-storage"></a>Krav för att använda Azure PowerShell med Azure Storage
Du behöver ett Azure-prenumeration och konto toorun hello PowerShell-cmdlets i den här guiden enligt beskrivningen ovan.

Azure PowerShell är en modul som tillhandahåller cmdlets toomanage Azure via Windows PowerShell. Information om att installera och konfigurera Azure PowerShell finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview). Vi rekommenderar att du hämtar och installerar eller uppgraderar toohello senaste Azure PowerShell-modulen innan du använder den här guiden.

Du kan köra hello-cmdlets i Windows PowerShell-konsol med hello standard eller hello Windows PowerShell Integrated Scripting Environment (ISE). Till exempel tooopen **Windows PowerShell ISE**, gå toohello Start-menyn, Skriv Administrationsverktyg och på toorun den. Högerklicka på Windows PowerShell ISE i hello Administrationsverktyg fönster, klicka på Kör som administratör.

## <a name="how-toomanage-storage-accounts-in-azure"></a>Hur toomanage lagringskonton i Azure

Låt oss ta en titt på Hantera storage-konton i Azure med PowerShell

### <a name="how-tooset-a-default-azure-subscription"></a>Hur tooset standard Azure-prenumeration
toomanage Azure Storage med hjälp av Azure PowerShell, behöver du tooauthenticate din klientmiljö med Azure via Azure Active Directory-autentisering eller certifikatbaserad autentisering. Detaljerad information finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) kursen. Den här guiden använder hello Azure Active Directory-autentisering.

1. I Windows PowerShell ISE skriver du följande kommando tooadd hello din Azure-konto toohello lokala PowerShell-miljö:

    ```powershell
    Add-AzureAccount
    ```

2. I fönstret för hello ”logga in tooMicrosoft Azure”, typen hello e-postadress och lösenord som är associerat med ditt konto. Azure autentiserar sparar hello autentiseringsuppgifter och sedan stänger hello-fönstret.

3. Kör hello efter kommandot tooview hello Azure-konton i din lokala PowerShell-miljö och kontrollera att ditt konto finns:
   
    ```powershell
    Get-AzureAccount
    ```
4. Sedan kör följande cmdlet tooview hello alla hello-prenumerationer som är anslutna toohello lokala PowerShell-session och kontrollerar du att din prenumeration visas:

    ```powershell
    Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName`
    ```
5. tooset standard Azure-prenumeration, kör hello väljer AzureSubscription cmdlet:

    ```powershell
    $SubscriptionName = 'Your subscription Name'
    Select-AzureSubscription -SubscriptionName $SubscriptionName –Default
    ```

6. Kontrollera hello namn på hello standard prenumeration genom att köra hello Get-AzureSubscription cmdlet:

    ```powershell
    Get-AzureSubscription -Default
    ```

7. toosee alla hello tillgängliga PowerShell-cmdlets för Azure Storage, kör:
    
    ```powershell
    Get-Command -Module Azure -Noun *Storage*`
    ```

### <a name="how-toocreate-a-new-azure-storage-account"></a>Hur toocreate ett nytt Azure storage-konto
toouse Azure storage, behöver ett lagringskonto. Du kan skapa ett nytt Azure storage-konto när du har konfigurerat din dator tooconnect tooyour prenumeration.

1. Kör hello Get-AzureLocation cmdlet toofind alla hello tillgängliga datacenter platser:

    ```powershell
    Get-AzureLocation | Format-Table -Property Name, AvailableServices, StorageAccountTypes
    ```

2. Kör hello ny AzureStorageAccount cmdlet toocreate ett nytt lagringskonto. hello följande exempel skapas ett nytt lagringskonto i hello ”USA, västra” datacenter.
   
    ```powershell
    $location = "West US"
    $StorageAccountName = "yourstorageaccount"
    New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $location
    ```

> [!IMPORTANT]
> hello namnet på ditt lagringskonto måste vara unikt i Azure och måste vara gemener. Namnkonventioner och begränsningar finns i [om Azure Lagringskonton](storage-create-storage-account.md) och [namnge och referera till behållare, Blobbar och Metadata](http://msdn.microsoft.com/library/azure/dd135715.aspx).
> 
> 

### <a name="how-tooset-a-default-azure-storage-account"></a>Hur tooset ett standardkonto för Azure storage
Du kan ha flera lagringskonton i din prenumeration. Du kan välja en av dem och ange den som hello standardkontot för lagring för alla hello lagring kommandon i hello samma PowerShell-session. Detta gör toorun Azure PowerShell-kommandon hello lagring utan att ange hello lagring kontexten explicit.

1. Du kan köra hello-cmdlet Set-AzureSubscription tooset en standardkontot för lagring för din prenumeration.

    ```powershell
    $SubscriptionName = "Your subscription name"
    $StorageAccountName = "yourstorageaccount"  
    Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName
    ```

2. Kör hello Get-AzureSubscription cmdlet tooverify som hello storage-konto är kopplad till ditt standardkonto för prenumerationen. Det här kommandot returnerar hello Prenumerationsegenskaper på hello aktuell prenumeration, inklusive dess aktuella storage-konto.

    ```powershell
    Get-AzureSubscription –Current
    ```

### <a name="how-toolist-all-azure-storage-accounts-in-a-subscription"></a>Hur toolist alla Azure storage-konton i en prenumeration
Varje Azure-prenumeration kan ha upp too100 storage-konton. Hello allra senaste informationen om du begränsar finns [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../azure-subscription-service-limits.md).

Kör följande cmdlet toofind ut hello namn och status för hello storage-konton i hello nuvarande prenumeration hello:

```powershell
Get-AzureStorageAccount | Format-Table -Property StorageAccountName, Location, AccountType, StorageAccountStatus
```

### <a name="how-toocreate-an-azure-storage-context"></a>Hur toocreate en Azure storage-kontext
Azure storage-kontexten är ett objekt i PowerShell tooencapsulate hello lagring autentiseringsuppgifter. Med en kontext som lagring när du kör alla efterföljande cmdletar kan du tooauthenticate din begäran utan att ange hello storage-konto och dess åtkomstnyckel explicit. Du kan skapa en kontext för lagring på många sätt, till exempel med hjälp av lagringskontots namn och åtkomstnyckel, signaturtoken för delad åtkomst (SAS), anslutningssträngen, eller anonym. Mer information finns i [ny AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext).  

Använd någon av följande tre sätt toocreate hello en kontext för lagring:

* Kör hello [Get-AzureStorageKey](/powershell/module/azure.storage/get-azurestoragekey) cmdlet toofind ut hello primära lagringsåtkomstnyckel för Azure storage-konto. Därefter anropar hello [ny AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext) cmdlet toocreate en kontext för lagring:

    ```powershell
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    ```

* Generera en token för delad åtkomst signaturen för ett Azure storage-behållare och använda den toocreate en kontext för lagring:

    ```powershell
    $sasToken = New-AzureStorageContainerSASToken -Container abc -Permission rl
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -SasToken $sasToken
    ```

    Mer information finns i [ny AzureStorageContainerSASToken](/powershell/module/azure.storage/new-azurestoragecontainersastoken) och [med delad åtkomst signaturer (SAS)](storage-dotnet-shared-access-signature-part-1.md).

* I vissa fall kan kanske du vill toospecify hello slutpunkter när du skapar en ny lagring-kontext. Detta kan vara nödvändigt när du har registrerat ett anpassat domännamn för ditt lagringskonto med hello Blob-tjänsten eller om du vill toouse en signatur för delad åtkomst för att komma åt lagringsresurser. Ange hello slutpunkter i en anslutningssträng och använda den toocreate en ny lagring kontext enligt nedan:

    ```powershell
    $ConnectionString = "DefaultEndpointsProtocol=http;BlobEndpoint=<blobEndpoint>;QueueEndpoint=<QueueEndpoint>;TableEndpoint=<TableEndpoint>;AccountName=<AccountName>;AccountKey=<AccountKey>"
    $Ctx = New-AzureStorageContext -ConnectionString $ConnectionString
    ```

Mer information om hur tooconfigure en lagringsanslutningssträng finns [konfigurerar anslutningssträngar](storage-configure-connection-string.md).

Nu när du har skapat din dator och lärt dig hur toomanage prenumerationer och storage-konton med hjälp av Azure PowerShell gå toohello nästa avsnitt toolearn hur toomanage Azure BLOB-objekt och blob ögonblicksbilder.

### <a name="how-tooretrieve-and-regenerate-azure-storage-keys"></a>Hur tooretrieve och generera nycklar för Azure storage
Ett Azure Storage-konto innehåller två nycklar. Du kan använda följande cmdlet tooretrieve hello dina nycklar.

```powershell
Get-AzureStorageKey -StorageAccountName "yourstorageaccount"
```

Använd hello följande cmdlet tooretrieve en viss nyckel. Giltiga värden är primär och sekundär.  

```powershell
(Get-AzureStorageKey -StorageAccountName $StorageAccountName).Primary
    
(Get-AzureStorageKey -StorageAccountName $StorageAccountName).Secondary
```

Om du vill att tooregenerate dina nycklar använder hello följande cmdlet. Giltiga värden för - KeyType är ”primär” och ”sekundär”

```powershell
New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType "Primary"
    
New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType "Secondary"
```

## <a name="how-toomanage-azure-blobs"></a>Hur toomanage Azure BLOB-objekt
Azure Blob storage är en tjänst för att lagra stora mängder Ostrukturerade data, till exempel text eller binära data som kan nås från var som helst i hello world via HTTP eller HTTPS. Det här avsnittet förutsätter att du redan är bekant med principerna för hello Azure Blob Storage-tjänsten. Detaljerad information finns i [komma igång med Blob storage med hjälp av .NET](storage-dotnet-how-to-use-blobs.md) och [Blob-Tjänstkoncept](http://msdn.microsoft.com/library/azure/dd179376.aspx).

### <a name="how-toocreate-a-container"></a>Hur toocreate en behållare
Varje blobb i Azure-lagring måste vara i en behållare. Du kan skapa en privat behållare med hello ny AzureStorageContainer cmdlet:

```powershell
$StorageContainerName = "yourcontainername"
New-AzureStorageContainer -Name $StorageContainerName -Permission Off
```

> [!NOTE]
> Det finns tre nivåer av anonym läsbehörighet: **av**, **Blob**, och **behållare**. tooprevent anonym åtkomst till tooblobs parameter för hello behörighet för**av**. Som standard hello ny behållare är privat och kan endast användas av hello Kontoägare. tooallow anonym offentliga läsa åtkomst tooblob resurser, men inte toocontainer metadata eller toohello lista över blobbar i hello behållare genom att ange hello behörighet parametern för**Blob**. tooallow fullständig offentliga läsa komma åt resurser i tooblob, metadata för behållaren och hello lista över blobbar i hello behållare genom att ange hello behörighet parametern för**behållare**. Mer information finns i [Hantera anonym läsbehörighet toocontainers och blobbar](storage-manage-access-to-resources.md).
> 
> 

### <a name="how-tooupload-a-blob-into-a-container"></a>Hur tooupload en blobb till en behållare
Azure Blob Storage stöder blockblobbar och sidblobbar. Mer information finns i [förstå Blockblobbar, Tilläggsblobbar och Sidblobbar](http://msdn.microsoft.com/library/azure/ee691964.aspx).

tooupload blobbar i behållaren tooa, kan du använda hello [Set AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) cmdlet. Som standard överför det här kommandot hello lokala filer tooa blockblob. Du kan använda hello - BlobType parametern toospecify hello typ för hello blob.

hello följande exempel kör hello [Get-ChildItem](http://technet.microsoft.com/library/hh849800.aspx) cmdlet tooget alla hello filer i hello angiven mapp och sedan skickar dem toohello nästa cmdlet genom att använda hello pipeline. Hej [Set AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) cmdlet överför hello lokala filer tooyour behållare:

```powershell
Get-ChildItem –Path C:\Images\* | Set-AzureStorageBlobContent -Container "yourcontainername"
```

### <a name="how-toodownload-blobs-from-a-container"></a>Hur toodownload BLOB-objekt från en behållare
hello som följande exempel visar hur toodownload BLOB-objekt från en behållare. hello exempel först upprättar en anslutning tooAzure lagring med hjälp av hello kontexten för lagringskontot, vilket innefattar hello lagringskontonamn och den primära åtkomstnyckeln. Sedan hello exemplet hämtas en blobbreferens med hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet. Därefter hello exemplet används hello [Get-AzureStorageBlobContent](/powershell/module/azure.storage/get-azurestorageblobcontent) cmdlet toodownload blobbar i hello lokala målmappen.

```powershell
#Define hello variables.
$ContainerName = "yourcontainername"
$DestinationFolder = "C:\DownloadImages"

#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#List all blobs in a container.
$blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

#Download blobs from a container.
New-Item -Path $DestinationFolder -ItemType Directory -Force
$blobs | Get-AzureStorageBlobContent -Destination $DestinationFolder -Context $Ctx
```

### <a name="how-toocopy-blobs-from-one-storage-container-tooanother"></a>Hur toocopy BLOB-objekt från en lagring behållare tooanother
Du kan kopiera BLOB storage-konton och regioner asynkront. hello visar exemplet nedan hur toocopy BLOB-objekt från en lagring behållaren tooanother i två olika lagringskonton. hello exempel först ställer in variabler för käll- och storage-konton och skapar sedan en kontext för lagring för varje konto. Därefter hello exempel kopieras blobbar från hello källa behållaren toohello målbehållare med hello [Start AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) cmdlet. hello exemplet förutsätter att hello käll- och storage-konton och behållare finns redan.

```powershell
#Define hello source storage account and context.
$SourceStorageAccountName = "yoursourcestorageaccount"
$SourceStorageAccountKey = "Storage key for yoursourcestorageaccount"
$SrcContainerName = "yoursrccontainername"
$SourceContext = New-AzureStorageContext -StorageAccountName $SourceStorageAccountName -StorageAccountKey $SourceStorageAccountKey

#Define hello destination storage account and context.
$DestStorageAccountName = "yourdeststorageaccount"
$DestStorageAccountKey = "Storage key for yourdeststorageaccount"
$DestContainerName = "destcontainername"
$DestContext = New-AzureStorageContext -StorageAccountName $DestStorageAccountName -StorageAccountKey $DestStorageAccountKey

#Get a reference tooblobs in hello source container.
$blobs = Get-AzureStorageBlob -Container $SrcContainerName -Context $SourceContext

#Copy blobs from one container tooanother.
$blobs| Start-AzureStorageBlobCopy -DestContainer $DestContainerName -DestContext $DestContext
```

Observera att det här exemplet utför en asynkron kopia. Du kan övervaka hello status för varje kopia genom att köra hello [Get-AzureStorageBlobCopyState](/powershell/module/azure.storage/start-azurestorageblobcopystate) cmdlet.

### <a name="how-toocopy-blobs-from-a-secondary-location"></a>Hur toocopy BLOB-objekt från en sekundär plats
Du kan kopiera blobbar från hello sekundära platsen för RA-GRS-aktiverat konto.

```powershell
#define secondary storage context using a connection string constructed from secondary endpoints.
$SrcContext = New-AzureStorageContext -ConnectionString "DefaultEndpointsProtocol=https;AccountName=***;AccountKey=***;BlobEndpoint=http://***-secondary.blob.core.windows.net;FileEndpoint=http://***-secondary.file.core.windows.net;QueueEndpoint=http://***-secondary.queue.core.windows.net; TableEndpoint=http://***-secondary.table.core.windows.net;"
Start-AzureStorageBlobCopy –Container *** -Blob *** -Context $SrcContext –DestContainer *** -DestBlob *** -DestContext $DestContext
```

### <a name="how-toodelete-a-blob"></a>Hur toodelete en blob
toodelete blob först hämta en blobbreferens och sedan anropa hello Remove-AzureStorageBlob cmdlet på den. hello följande exempel tar bort alla hello blobbar i en viss behållare. hello exempel först ställer in variabler för ett lagringskonto och skapar sedan en kontext för lagring. Därefter hello exemplet hämtas en blobbreferens med hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet och kör hello [ta bort AzureStorageBlob](/powershell/module/azure.storage/remove-azurestorageblob) cmdlet tooremove blobbar från en behållare i Azure-lagring.

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$ContainerName = "containername"
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Get a reference tooall hello blobs in hello container.
$blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

#Delete blobs in a specified container.
$blobs| Remove-AzureStorageBlob
```

## <a name="how-toomanage-azure-blob-snapshots"></a>Hur toomanage Azure blob-ögonblicksbilder
Azure kan du skapa en ögonblicksbild av en blob. En ögonblicksbild är en skrivskyddad version av en blob som utförs i en punkt i tiden. När en ögonblicksbild har skapats, kan den läsa, kopieras, eller ta bort, men inte har ändrats. Ögonblicksbilder ger ett sätt tooback upp en blob som det visas vid en tidpunkt. Mer information finns i [skapa en ögonblicksbild av en Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).

### <a name="how-toocreate-a-blob-snapshot"></a>Hur toocreate en blob-ögonblicksbild
toocreate en snaphot för en blobb först hämta en blobbreferens och sedan anropa hello `ICloudBlob.CreateSnapshot` metod på den. hello följande exempel först ställer in variabler för ett lagringskonto och skapar sedan en kontext för lagring. Därefter hello exemplet hämtas en blobbreferens med hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet och kör hello [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) metoden toocreate en ögonblicksbild.

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$ContainerName = "yourcontainername"
$BlobName = "yourblobname"
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Get a reference tooa blob.
$blob = Get-AzureStorageBlob -Context $Ctx -Container $ContainerName -Blob $BlobName

#Create a snapshot of hello blob.
$snap = $blob.ICloudBlob.CreateSnapshot()
```

### <a name="how-toolist-a-blobs-snapshots"></a>Hur toolist en blob ögonblicksbilder
Du kan skapa så många ögonblicksbilder som du vill använda för en blob. Du kan ange hello ögonblicksbilder kopplade till blob-tootrack din aktuella ögonblicksbilder. hello följande exempel används en fördefinierad blob och anrop hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet toolist hello ögonblicksbilder för blobben.  

```powershell
#Define hello blob name.
$BlobName = "yourblobname"

#List hello snapshots of a blob.
Get-AzureStorageBlob –Context $Ctx -Prefix $BlobName -Container $ContainerName  | Where-Object  { $_.ICloudBlob.IsSnapshot -and $_.Name -eq $BlobName }
```

### <a name="how-toocopy-a-snapshot-of-a-blob"></a>Hur toocopy en ögonblicksbild av en blob
Du kan kopiera en ögonblicksbild av en blob toorestore hello ögonblicksbild. Detaljerad information och begränsningar finns i [skapa en ögonblicksbild av en Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx). hello följande exempel först ställer in variabler för ett lagringskonto och skapar sedan en kontext för lagring. Hello exempel definierar sedan hello-behållaren och blob namn variabler. hello exemplet hämtas en blobbreferens med hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet och kör hello [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) metoden toocreate en ögonblicksbild. Sedan hello exempel kör hello [Start AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) cmdlet toocopy hello ögonblicksbild av en blob med hello ICloudBlob objektet för hello källa blob. Kontrollera att baseras tooupdate hello variabler på din konfiguration innan du kör hello exempel. Den hello följande exempel förutsätter att hello käll- och behållare och hello källa blob finns redan.

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Define hello variables.
$SrcContainerName = "yoursourcecontainername"
$DestContainerName = "yourdestcontainername"
$SrcBlobName = "yourblobname"
$DestBlobName = "CopyBlobName"

#Get a reference tooa blob.
$blob = Get-AzureStorageBlob -Context $Ctx -Container $SrcContainerName -Blob $SrcBlobName

#Create a snapshot of a blob.
$snap = $blob.ICloudBlob.CreateSnapshot()

#Copy hello snapshot tooanother container.
Start-AzureStorageBlobCopy –Context $Ctx -ICloudBlob $snap -DestBlob $DestBlobName -DestContainer $DestContainerName
```

Nu när du har lärt dig hur toomanage Azure BLOB-objekt och blob-ögonblicksbilder med Azure PowerShell gå toohello nästa avsnitt toolearn hur toomanage tabeller, köer och filer.

## <a name="how-toomanage-azure-tables-and-table-entities"></a>Hur toomanage Azure tabeller och tabell entiteter
Azure Table storage-tjänsten är en NoSQL-datalager som du kan använda toostore och fråga stora mängder strukturerad, icke-relationella data. hello huvudkomponenterna i hello-tjänsten är tabeller, enheter och egenskaper. En tabell är en samling med entiteter. En entitet är en uppsättning egenskaper. Varje entitet kan ha upp too252 egenskaper, som är alla namn / värde-par. Det här avsnittet förutsätter att du redan är bekant med principerna för hello Azure Table Storage-tjänsten. Detaljerad information finns i [hello förstå tabelltjänst-datamodellen](http://msdn.microsoft.com/library/azure/dd179338.aspx) och [komma igång med Azure Table storage med hjälp av .NET](storage-dotnet-how-to-use-tables.md).

I följande underavsnitt hello, lär du dig hur toomanage Azure Table storage tjänst med hjälp av Azure PowerShell. hello beskrivs scenarier där **skapar**, **bort**, och **hämtar** **tabeller**, samt **att lägga till**, **frågar**, och **bort tabellentiteter**.

### <a name="how-toocreate-a-table"></a>Hur toocreate en tabell
Varje tabell måste finnas i ett Azure storage-konto. hello exemplet nedan visar hur toocreate en tabell i Azure Storage. hello exempel först upprättar en anslutning tooAzure lagring med hjälp av hello kontexten för lagringskontot, vilket innefattar hello lagringskontonamn och dess snabbtangent. Därefter används hello [ny AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) cmdlet toocreate en tabell i Azure Storage.

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey

#Create a new table.
$tabName = "yourtablename"
New-AzureStorageTable –Name $tabName –Context $Ctx
```

### <a name="how-tooretrieve-a-table"></a>Hur tooretrieve en tabell
Du kan fråga efter och hämta en eller alla tabeller i ett lagringskonto. hello exemplet nedan visar hur en given tabell med hjälp av tooretrieve hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet.

```powershell
#Retrieve a table.
$tabName = "yourtablename"
Get-AzureStorageTable –Name $tabName –Context $Ctx
```

Om du anropar hello Get-AzureStorageTable cmdlet utan några parametrar hämtar alla lagringstabeller som för ett lagringskonto.

### <a name="how-toodelete-a-table"></a>Hur toodelete en tabell
Du kan ta bort en tabell från ett lagringskonto med hjälp av hello [ta bort AzureStorageTable](/powershell/module/azure.storage/remove-azurestoragetable) cmdlet.  

```powershell
#Delete a table.
$tabName = "yourtablename"
Remove-AzureStorageTable –Name $tabName –Context $Ctx
```

### <a name="how-toomanage-table-entities"></a>Hur toomanage tabell entiteter
För närvarande tillhandahålla Azure PowerShell inte cmdlets toomanage tabellentiteter direkt. tooperform åtgärder på tabellentiteter, kan du använda hello klasser i hello [Azure Storage-klientbibliotek för .NET](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx).

#### <a name="how-tooadd-table-entities"></a>Hur tooadd tabell entiteter
tooadd en entitet tooa tabell först skapa ett objekt som definierar egenskaper för enhet. En entitet kan ha upp too255 egenskaper, inklusive 3 Systemegenskaper: **PartitionKey**, **RowKey**, och **tidsstämpel**. Du ansvarar för att lägga till och uppdatera hello värdena för **PartitionKey** och **RowKey**. hello-servern hanterar hello värdet för **tidsstämpel**, som inte ändras. Tillsammans hello **PartitionKey** och **RowKey** identifiera varje entitet i en tabell.

* **PartitionKey**: Anger hello-partition som hello entiteten är lagrad i.
* **RowKey**: identifierar hello entiteten inom hello partition.

Du kan definiera too252 anpassade egenskaper för en entitet. Mer information finns i [hello förstå tabelltjänst-datamodellen](http://msdn.microsoft.com/library/azure/dd179338.aspx).

hello exemplet nedan visar hur tooadd entiteter tooa tabell. hello exempel visas hur tooretrieve hello medarbetare tabell och lägga till flera enheter till den. Först den upprättar en anslutning tooAzure lagring med hjälp av hello kontexten för lagringskontot, vilket innefattar hello lagringskontonamn och dess snabbtangent. Sedan hämtar hello får tabellen med hjälp av hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet. Om hello tabellen inte finns, hello [ny AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) cmdlet har använt toocreate en tabell i Azure Storage. Därefter definierar hello exempel en egen funktion Lägg till entiteten tooadd entiteter toohello tabell genom att ange varje entitet partition och radnyckel. hello Lägg till entiteten funktionen anrop hello [New-Object](http://technet.microsoft.com/library/hh849885.aspx) cmdlet på hello [Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.dynamictableentity.aspx) klassen toocreate ett entitetsobjekt. Senare hello exemplet anropar hello [Microsoft.WindowsAzure.Storage.Table.TableOperation.Insert](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.insert.aspx) -metoden i den här entiteten objektet tooadd den tooa tabell.

```powershell
#Function Add-Entity: Adds an employee entity tooa table.
function Add-Entity() {
    [CmdletBinding()]
    param(
       $table,
       [String]$partitionKey,
       [String]$rowKey,
       [String]$name,
       [Int]$id
    )

  $entity = New-Object -TypeName Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity -ArgumentList $partitionKey, $rowKey
  $entity.Properties.Add("Name", $name)
  $entity.Properties.Add("ID", $id)

  $result = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Insert($entity))
}

#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$TableName = "Employees"

#Retrieve hello table if it already exists.
$table = Get-AzureStorageTable –Name $TableName -Context $Ctx -ErrorAction Ignore

#Create a new table if it does not exist.
if ($table -eq $null)
{
   $table = New-AzureStorageTable –Name $TableName -Context $Ctx
}

#Add multiple entities tooa table.
Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row1 -Name Chris -Id 1
Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row2 -Name Jessie -Id 2
Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row1 -Name Christine -Id 3
Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row2 -Name Steven -Id 4
```

#### <a name="how-tooquery-table-entities"></a>Hur tooquery tabell entiteter
tooquery en tabell, använder hello [Microsoft.WindowsAzure.Storage.Table.TableQuery](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tablequery.aspx) klass. hello följande exempel förutsätter att du redan har kört hello-skriptet som anges i hello hur tooadd entiteter avsnitt i handboken. hello exempel först upprättar en anslutning tooAzure lagring med hello lagring kontext, vilket innefattar hello lagringskontonamn och dess snabbtangent. Försök därefter tooretrieve hello skapat ”anställda” tabell med hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet. Anropar hello [New-Object](http://technet.microsoft.com/library/hh849885.aspx) cmdlet på hello Microsoft.WindowsAzure.Storage.Table.TableQuery klassen skapas ett nytt frågeobjekt. hello exempel söker efter hello entiteter som har en ”ID”-kolumn vars värde är 1 som anges i ett sträng-filter. Detaljerad information finns i [frågor till tabeller och de entiteter](http://msdn.microsoft.com/library/azure/dd894031.aspx). När du kör den här frågan returnerar alla entiteter som matchar hello filtervillkor.

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$TableName = "Employees"

#Get a reference tooa table.
$table = Get-AzureStorageTable –Name $TableName -Context $Ctx

#Create a table query.
$query = New-Object Microsoft.WindowsAzure.Storage.Table.TableQuery

#Define columns tooselect.
$list = New-Object System.Collections.Generic.List[string]
$list.Add("RowKey")
$list.Add("ID")
$list.Add("Name")

#Set query details.
$query.FilterString = "ID gt 0"
$query.SelectColumns = $list
$query.TakeCount = 20

#Execute hello query.
$entities = $table.CloudTable.ExecuteQuery($query)

#Display entity properties with hello table format.
$entities  | Format-Table PartitionKey, RowKey, @{ Label = "Name"; Expression={$_.Properties["Name"].StringValue}}, @{ Label = "ID"; Expression={$_.Properties["ID"].Int32Value}} -AutoSize
```

#### <a name="how-toodelete-table-entities"></a>Hur toodelete tabell entiteter
Du kan ta bort en entitet med dess partition och raden nycklar. hello följande exempel förutsätter att du redan har kört hello-skriptet som anges i hello hur tooadd entiteter avsnitt i handboken. hello exempel först upprättar en anslutning tooAzure lagring med hello lagring kontext, vilket innefattar hello lagringskontonamn och dess snabbtangent. Försök därefter tooretrieve hello skapat ”anställda” tabell med hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet. Om hello tabellen finns hello exemplet anropar hello [Microsoft.WindowsAzure.Storage.Table.TableOperation.Retrieve](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.retrieve.aspx) metoden tooretrieve en entitet baserat på dess partition och raden nyckelvärden. Sedan skicka hello entiteten toohello [Microsoft.WindowsAzure.Storage.Table.TableOperation.Delete](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.delete.aspx) metoden toodelete.

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

#Retrieve hello table.
$TableName = "Employees"
$table = Get-AzureStorageTable -Name $TableName -Context $Ctx -ErrorAction Ignore

#If hello table exists, start deleting its entities.
if ($table -ne $null) 
{
    #Together hello PartitionKey and RowKey uniquely identify every  
    #entity within a table.
    $tableResult = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Retrieve("Partition2", "Row1"))
    $entity = $tableResult.Result
    if ($entity -ne $null)
    {
        #Delete hello entity.
        $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Delete($entity))
    }
}
```

## <a name="how-toomanage-azure-queues-and-queue-messages"></a>Hur toomanage Azure köer och meddelanden i kö
Azure Queue storage är en tjänst för att lagra stora mängder meddelanden som kan nås från var som helst i Hej världen via autentiserade anrop med HTTP eller HTTPS. Det här avsnittet förutsätter att du redan är bekant med principerna för hello Azure Queue Storage-tjänsten. Detaljerad information finns i [komma igång med Azure Queue storage med hjälp av .NET](storage-dotnet-how-to-use-queues.md).

Det här avsnittet visar hur toomanage Azure Queue storage tjänst med hjälp av Azure PowerShell. hello beskrivs scenarier där **infoga** och **bort** kö meddelanden, samt **skapar**, **ta bort**, och **hämtar köer**.

### <a name="how-toocreate-a-queue"></a>Hur toocreate en kö
hello följande exempel först upprättar en anslutning tooAzure lagring med hjälp av hello kontexten för lagringskontot, vilket innefattar hello lagringskontonamn och dess snabbtangent. Därefter anropar [ny AzureStorageQueue](/powershell/module/azure.storage/new-azurestoragequeue) cmdlet toocreate en kö med namnet 'könamn'.

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$QueueName = "queuename"
$Queue = New-AzureStorageQueue –Name $QueueName -Context $Ctx
```

Information om namngivningskonventioner för Azure-kötjänsten finns [namngivning av köer och Metadata](http://msdn.microsoft.com/library/azure/dd179349.aspx).

### <a name="how-tooretrieve-a-queue"></a>Hur tooretrieve en kö
Du kan fråga efter och hämta en särskild kö eller en lista över alla hello köer i ett lagringskonto. hello exemplet nedan visar hur en kö som anges med hjälp av tooretrieve hello [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) cmdlet.

```powershell
#Retrieve a queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue –Name $QueueName –Context $Ctx
```

Om du anropar hello [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) cmdlet utan några parametrar får den en lista över alla hello köer.

### <a name="how-toodelete-a-queue"></a>Hur toodelete en kö
toodelete en kö och alla hälsningsmeddelande finns i den anropet hello Remove-AzureStorageQueue cmdlet. hello som följande exempel visar hur en kö som anges med hjälp av toodelete hello Remove-AzureStorageQueue cmdlet.

```powershell
#Delete a queue.
$QueueName = "yourqueuename"
Remove-AzureStorageQueue –Name $QueueName –Context $Ctx
```

#### <a name="how-tooinsert-a-message-into-a-queue"></a>Hur tooinsert ett meddelande i en kö
tooinsert ett meddelande i en befintlig kö först skapa en ny instans av hello [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) klass. Därefter anropar hello [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) metod. En CloudQueueMessage kan skapas från en sträng (i UTF-8-format) eller en byte-matris.

hello som följande exempel visar hur tooadd meddelande tooa kön. hello exempel först upprättar en anslutning tooAzure lagring med hjälp av hello kontexten för lagringskontot, vilket innefattar hello lagringskontonamn och dess snabbtangent. Sedan hämtar hello kö som anges med hjälp av hello [Get-AzureStorageQueue](https://msdn.microsoft.com/library/azure/dn806377.aspx) cmdlet. Om hello kön finns hello [New-Object](http://technet.microsoft.com/library/hh849885.aspx) cmdlet har använt toocreate en instans av hello [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) klass. Senare hello exemplet anropar hello [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) -metoden i det här meddelandet objektet tooadd den tooa kön. Här är kod som hämtar en kö och infogar 'MessageInfo' hello-meddelande:

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

#Retrieve hello queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

#If hello queue exists, add a new message.
if ($Queue -ne $null) {
   # Create a new message using a constructor of hello CloudQueueMessage class.
   $QueueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList MessageInfo

   # Add a new message toohello queue.
   $Queue.CloudQueue.AddMessage($QueueMessage)
}
```

#### <a name="how-toode-queue-at-hello-next-message"></a>Hur toode-kö på hello nästa meddelande
Koden tar bort ett meddelande från en kö i två steg. När du anropar hello [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.GetMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.getmessage.aspx) metod, du får hello nästa meddelande i en kö. Ett meddelande som returneras från **GetMessage** blir osynligt tooany annan kod läsa meddelanden från den här kön. toofinish att ta bort hello-meddelande från kön hello, måste du också anropa hello [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.DeleteMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.deletemessage.aspx) metod. Den här tvåstegsprocessen för att ta bort ett meddelande säkerställer som om din kod inte tooprocess får ett meddelande på grund av fel toohardware eller programvara, en annan instans av koden hello samma meddelande och försök igen. Koden anropar **DeleteMessage** direkt efter hello-meddelande har bearbetats.

```powershell
# Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

# Retrieve hello queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

$InvisibleTimeout = [System.TimeSpan]::FromSeconds(10)

# Get hello message object from hello queue.
$QueueMessage = $Queue.CloudQueue.GetMessage($InvisibleTimeout)
# Delete hello message.
$Queue.CloudQueue.DeleteMessage($QueueMessage)
```

## <a name="how-toomanage-azure-file-shares-and-files"></a>Hur toomanage Azure filen delar och filer
Azure File storage erbjuder delad lagring för program som använder hello SMB-standardprotokollet. Microsoft Azure-datorer och molntjänster kan dela fildata över programkomponenter via monterade resurser och lokala program kan komma åt fildata på en resurs via hello fil-API för storage eller Azure PowerShell.

Mer information om Azure File storage finns [Kom igång med Azure File storage i Windows](storage-dotnet-how-to-use-files.md) och [File Service REST API](http://msdn.microsoft.com/library/azure/dn167006.aspx).

## <a name="how-tooset-and-query-storage-analytics"></a>Hur tooset och fråga storage analytics
Du kan använda [Azure Storage Analytics](storage-analytics.md) toocollect mått för Azure storage-konton och loggdata om begäranden skickas tooyour storage-konto. Du kan använda lagring mått toomonitor hello hälsotillståndet för ett lagringskonto och lagring loggning toodiagnose och felsöka problem med ditt lagringskonto. Du kan konfigurera övervakning med hjälp av hello Azure-portalen eller Windows PowerShell eller via programmering med hello storage-klientbiblioteket. Lagring loggning sker serversidan och aktiverar toorecord information för både slutförda och misslyckade begäranden på ditt lagringskonto. Dessa loggar aktivera toosee detaljer för Läs-, Skriv- och delete-åtgärder mot dina tabeller, köer och blobbar samt hello orsakerna till misslyckade begäranden.

hur tooenable och visa Storage-mätvärden data med hjälp av PowerShell, se toolearn [hur tooenable Storage-mätvärden med hjälp av PowerShell](http://msdn.microsoft.com/library/azure/dn782843.aspx#HowtoenableStorageMetricsusingPowerShell).

hur tooenable och hämta Storage data för loggning med hjälp av PowerShell, se toolearn [hur tooenable lagring loggning med hjälp av PowerShell](http://msdn.microsoft.com/library/azure/dn782840.aspx#HowtoenableStorageLoggingusingPowerShell) och [hitta din lagring loggning loggdata](http://msdn.microsoft.com/library/azure/dn782840.aspx#FindingyourStorageLogginglogdata).
Detaljerad information om hur du använder Storage-mätvärden och lagring loggning tootroubleshoot lagringsproblem finns [övervakning, diagnostisera och felsöka Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="how-toomanage-shared-access-signature-sas-and-stored-access-policy"></a>Hur toomanage delad Åtkomstsignatur (SAS) och lagras åtkomstprincipen
Signaturer för delad åtkomst är en viktig del av hello säkerhetsmodell för alla program som använder Azure-lagring. De är användbara för att tillhandahålla begränsade behörigheter tooyour storage-konto tooclients som inte ska ha hello kontonyckel. Som standard har bara hello ägare hello storage-konto kan komma åt blobbar, tabeller och köer i kontot. Om tjänsten eller programmet måste du toomake dessa resurser tillgängliga tooother klienter utan att dela din åtkomstnyckel, finns det tre alternativ:

* Ange en behållare behörigheter toopermit anonym läsbehörighet toohello behållaren och dess blobbar. Detta är inte tillåtet för tabeller och köer.
* Använda en signatur för delad åtkomst som ger begränsad åtkomst rättigheter toocontainers, blobbar, köer och tabeller för ett visst tidsintervall.
* Använda en lagrad åtkomst princip tooobtain ytterligare en säkerhetsnivå för kontroll över signaturer för delad åtkomst för en behållare eller dess blobbar, en kö eller en tabell. hello lagrade åtkomstprincip kan du toochange hello starttid, förfallotiden eller behörigheter för en signatur eller toorevoke det efter att det har utfärdats.

En signatur för delad åtkomst kan ha ett av två sätt:

* **Ad hoc-SAS**: när du skapar en ad hoc-SAS, hello starttid, förfallotiden och behörigheter för hello SAS har angetts för hello SAS-URI. Den här typen av SAS kan skapas på en behållare, blob, tabell eller kön och det är icke-återkallningsbar.
* **SAS med lagrade åtkomstprincip**: en lagrade princip har definierats för resursbehållaren en blob-behållare, tabeller eller kö - och du kan använda den toomanage begränsningar för en eller flera signaturer för delad åtkomst. När du kopplar en SAS med en lagrad till hello SAS ärver begränsningarna hello - hello starttid, förfallotiden samt behörigheter - har definierats för hello lagras åtkomstprincip. Den här typen av SAS är återkallningsbar.

Mer information finns i [med delad åtkomst signaturer (SAS)](storage-dotnet-shared-access-signature-part-1.md) och [Hantera anonym läsbehörighet toocontainers och blobbar](storage-manage-access-to-resources.md).

I hello nästa avsnitt får du lära dig hur toocreate en signatur åtkomst-token och lagrade princip för delad åtkomst för Azure-tabeller. Azure PowerShell tillhandahåller liknande cmdlets för behållare, blobbar och köer samt. toorun hello skript i det här avsnittet hämta hello [Azure PowerShell version 0.8.14](http://go.microsoft.com/?linkid=9811175&clcid=0x409) eller senare.

### <a name="how-toocreate-a-policy-based-shared-access-signature-token"></a>Hur toocreate en principbaserad signatur för delad åtkomst-token
Använd hello ny AzureStorageTableStoredAccessPolicy cmdlet toocreate en ny lagrade åtkomstprincip. Anropa sedan hello [ny AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) cmdlet toocreate en ny policy-baserad delad signatur åtkomsttoken för en tabell med Azure Storage.

```powershell
$policy = "policy1"
New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
New-AzureStorageTableSASToken -Name $tableName -Policy $policy -Context $Ctx
```

### <a name="how-toocreate-an-ad-hoc-non-revocable-shared-access-signature-token"></a>Hur toocreate en ad hoc-(icke-återkallningsbar) signatur för delad åtkomst-token
Använd hello [ny AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) cmdlet toocreate en ny ad hoc-(icke-återkallningsbar) signatur för delad åtkomst-token för en tabell med Azure Storage:

```powershell
New-AzureStorageTableSASToken -Name $tableName -Permission "rqud" -StartTime "2015-01-01" -ExpiryTime "2015-02-01" -Context $Ctx
```
    
### <a name="how-toocreate-a-stored-access-policy"></a>Hur toocreate lagrade åtkomstprincipen
Använd hello ny AzureStorageTableStoredAccessPolicy cmdlet toocreate en ny lagrade åtkomstprincip för en tabell med Azure Storage:

```powershell
$policy = "policy1"
New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
```
    
### <a name="how-tooupdate-a-stored-access-policy"></a>Hur tooupdate lagrade åtkomstprincipen
Använd hello Set-AzureStorageTableStoredAccessPolicy cmdlet tooupdate en befintlig lagrade åtkomstprincip för en tabell med Azure Storage:

```powershell
Set-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Permission "rd" -NoExpiryTime -NoStartTime -Context $Ctx
```

### <a name="how-toodelete-a-stored-access-policy"></a>Hur toodelete lagrade åtkomstprincipen
Använd hello Remove-AzureStorageTableStoredAccessPolicy cmdlet toodelete lagrade åtkomstprincipen på en tabell i Azure Storage:

```powershell
Remove-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Context $Ctx
```

## <a name="how-toouse-azure-storage-for-us-government-and-azure-china"></a>Hur toouse Azure Storage för amerikanska myndigheter och Azure Kina
En Azure-miljö är en oberoende distribution av Microsoft Azure som [Azure Government för USA: s regering](https://azure.microsoft.com/features/gov/), [AzureCloud för globala Azure](https://portal.azure.com), och [AzureChinaCloud för Azure som drivs av 21Vianet i Kina](http://www.windowsazure.cn/). Du kan distribuera nya miljöer i Azure för amerikanska myndigheter och Azure Kina.

toouse Azure Storage med AzureChinaCloud måste toocreate en kontext för lagring som är associerad med AzureChinaCloud. Följ dessa steg tooget som du startade:

1. Kör hello [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) cmdlet toosee hello tillgängliga Azure miljöer:
   
    ```powershell
    Get-AzureEnvironment
    ```

2. Lägg till en Azure Kina konto tooWindows PowerShell:
   
    ```powershell
    Add-AzureAccount –Environment AzureChinaCloud
    ```

3. Skapa en kontext för lagring för ett AzureChinaCloud konto:
   
    ```powershell
    $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureChinaCloud
    ```

toouse Azure Storage med [USA Azure Government](https://azure.microsoft.com/features/gov/), bör du definiera en ny miljö och sedan skapa en ny lagring kontext med den här miljön:

1. Kör hello [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) cmdlet toosee hello tillgängliga Azure miljöer:

    ```powershell
    Get-AzureEnvironment
    ```

2. Lägg till en Azure som tillhör amerikanska myndigheter konto tooWindows PowerShell:
   
    ```powershell
    Add-AzureAccount –Environment AzureUSGovernment
    ```

3. Skapa en kontext för lagring för ett AzureUSGovernment konto:
   
    ```powershell
    $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureUSGovernment
    ```
     
Mer information finns i:

* [Utvecklarhandbok för Microsoft Azure Government](../azure-government-developer-guide.md).
* [Översikt över skillnaderna när du skapar ett program på Kina Service](https://msdn.microsoft.com/library/azure/dn578439.aspx)

## <a name="next-steps"></a>Nästa steg
I den här guiden har du lärt dig hur toomanage Azure Storage med Azure PowerShell. Här följer några relaterade artiklar och resurser för att lära dig mer om dem.

* [Azure Storage-dokumentation](https://azure.microsoft.com/documentation/services/storage/)
* [PowerShell-Cmdlets för Azure Storage](/powershell/module/azurerm.storage/#storage)
* [Windows PowerShell-referens](https://msdn.microsoft.com/library/ms714469.aspx)

[Getting started with Azure Storage and PowerShell in 5 minutes]: #getstart
[Prerequisites for using Azure PowerShell with Azure Storage]: #pre
[How toomanage storage accounts in Azure]: #manageaccount
[How tooset a default Azure subscription]: #setdefsub
[How toocreate a new Azure storage account]: #createaccount
[How tooset a default Azure storage account]: #defaultaccount
[How toolist all Azure storage accounts in a subscription]: #listaccounts
[How toocreate an Azure storage context]: #createctx
[How toomanage Azure blobs and blob snapshots]: #manageblobs
[How toocreate a container]: #container
[How tooupload a blob into a container]: #uploadblob
[How toodownload blobs from a container]: #downblob
[How toocopy blobs from one storage container tooanother]: #copyblob
[How toodelete a blob]: #deleteblob
[How toomanage Azure blob snapshots]: #manageshots
[How toocreate a blob snapshot]: #createshot
[How toolist snapshots of a blob]: #listshot
[How toocopy a snapshot of a blob]: #copyshot
[How toomanage Azure tables and table entities]: #managetables
[How toocreate a table]: #createtable
[How tooretrieve a table]: #gettable
[How toodelete a table]: #remtable
[How toomanage table entities]: #mngentity
[How tooadd table entities]: #addentity
[How tooquery table entities]: #queryentity
[How toodelete table entities]: #deleteentity
[How toomanage Azure queues and queue messages]: #managequeues
[How toocreate a queue]: #createqueue
[How tooretrieve a queue]: #getqueue
[How toodelete a queue]: #remqueue
[How toomanage queue messages]: #mngqueuemsg
[How tooinsert a message into a queue]: #addqueuemsg
[How toode-queue at hello next message]: #dequeuemsg
[How toomanage Azure file shares and files]: #files
[How tooset and query storage analytics]: #stganalytics
[How toomanage Shared Access Signature (SAS) and Stored Access Policy]: #sas
[How toouse Azure Storage for U.S. government and Azure China]: #gov
[Next Steps]: #next

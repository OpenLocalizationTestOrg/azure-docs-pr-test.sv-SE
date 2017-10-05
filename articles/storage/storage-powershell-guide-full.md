---
title: "Använda Azure PowerShell med Azure Storage | Microsoft Docs"
description: "Lär dig hur du använder Azure PowerShell-cmdlets för Azure Storage för att skapa och hantera lagringskonton; Arbeta med blobbar, tabeller, köer och -filer. Konfigurera och efterfråga storage analytics och skapa signaturer för delad åtkomst."
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
ms.openlocfilehash: 51e3e93ebedd31370857e61a00139294bcee9237
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="using-azure-powershell-with-azure-storage"></a>Använda Azure PowerShell med Azure Storage
## <a name="overview"></a>Översikt
Azure PowerShell är en modul som tillhandahåller för att hantera Azure via Windows PowerShell-cmdlets. Det är ett uppgiftsbaserat kommandoradsgränssnitt och skriptspråk som utformats specifikt för systemadministration. Du kan enkelt styra och automatisera administrationen av dina Azure-tjänster och program med PowerShell. Du kan till exempel använda cmdlets för att utföra samma uppgifter som du kan utföra via den [Azure-portalen](https://portal.azure.com).

I den här guiden kommer vi hur du använder den [Azure Storage-Cmdlets](/powershell/module/azurerm.storage/#storage) att utföra olika uppgifter för utveckling och administration med Azure Storage.

Den här guiden förutsätter att du har tidigare erfarenhet med [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) och [Windows PowerShell](http://technet.microsoft.com/library/bb978526.aspx). Handboken innehåller ett antal skript för att demonstrera användningen av PowerShell med Azure Storage. Du bör uppdatera skriptvariabler baserat på din konfiguration innan du kör varje skript.

Det första avsnittet i den här guiden ger en snabb överblick över vid Azure Storage och PowerShell. Detaljerad information och instruktioner som startar från den [krav för att använda Azure PowerShell med Azure Storage](#prerequisites-for-using-azure-powershell-with-azure-storage).

## <a name="getting-started-with-azure-storage-and-powershell-in-5-minutes"></a>Komma igång med Azure Storage och PowerShell 5 minuter
Det här avsnittet visar hur du kommer åt Azure Storage via PowerShell 5 minuter.

**Ny till Azure:** hämta Microsoft Azure-prenumeration och ett Microsoft-konto som är kopplade till den prenumerationen. Mer information om köpalternativ för Azure finns [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/), [köpa alternativ](https://azure.microsoft.com/pricing/purchase-options/), och [Medlemserbjudanden](https://azure.microsoft.com/pricing/member-offers/) (för medlemmar i MSDN, Microsoft Partner Network, BizSpark och att andra Microsoft-program).

Se [Tilldela administratörsroller i Azure Active Directory (AD Azure)](https://msdn.microsoft.com/library/azure/hh531793.aspx) för mer information om Azure-prenumerationer.

**När du har skapat ett Microsoft Azure-prenumeration och konto:**

1. Hämta och installera senaste [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/latest).
2. Starta Windows PowerShell Integrated Scripting Environment (ISE): Den lokala datorn, gå till den **starta** menyn. Typen **Administrationsverktyg** och klickar för att köra den. I den **Administrationsverktyg** fönstret, högerklicka på **Windows PowerShell ISE**, klickar du på **kör som administratör**.
3. I **Windows PowerShell ISE**, klickar du på **filen** > **ny** att skapa en ny skriptfil.
4. Nu får du ett enkelt skript som visar grundläggande PowerShell-kommandon för att få åtkomst till Azure Storage. Skriptet kommer först att fråga din Azure autentiseringsuppgifter för att lägga till din Azure-kontot till den lokala PowerShell-miljön. Skriptet kommer sedan standardvärdet Azure-prenumeration och skapa ett nytt lagringskonto i Azure. Skriptet kommer sedan skapa en ny behållare i den här nya storage-konto och ladda upp en befintlig bildfil (blob) till behållaren. När skriptet visar alla blobbar i behållaren, kommer den skapa en ny målkatalog i den lokala datorn och hämta avbildningsfilen.
5. I följande avsnitt i koden väljer du skriptet mellan anmärkningar **#begin** och **#end**. Tryck på CTRL + C för att kopiera den till Urklipp.

    ```powershell
    # begin
    # Update with the name of your subscription.
    $SubscriptionName = "YourSubscriptionName"
       
    # Give a name to your new storage account. It must be lowercase!
    $StorageAccountName = "yourstorageaccountname"
       
    # Choose "West US" as an example.
    $Location = "West US"
       
    # Give a name to your new container.
    $ContainerName = "imagecontainer"
       
    # Have an image file and a source directory in your local computer.
    $ImageToUpload = "C:\Images\HelloWorld.png"
       
    # A destination directory in your local computer.
    $DestinationFolder = "C:\DownloadImages"
       
    # Add your Azure account to the local PowerShell environment.
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
       
    # Download blobs from the container:
    # Get a reference to a list of all blobs in a container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName
       
    # Create the destination directory.
    New-Item -Path $DestinationFolder -ItemType Directory -Force  
       
    # Download blobs into the local destination directory.
    $blobs | Get-AzureStorageBlobContent –Destination $DestinationFolder
       
    # end
    ```

6. I **Windows PowerShell ISE**, tryck på CTRL + V för att kopiera skriptet. Klicka på **filen** > **spara**. I den **Spara som** dialogruta, skriver du namnet på skriptfilen, till exempel ”mystoragescript”. Klicka på **Spara**.
7. Nu kan behöver du uppdatera skriptvariabler baserat på inställningarna. Du måste uppdatera de **$SubscriptionName** variabeln med din egen prenumeration. Du kan behålla variablerna som anges i skriptet eller uppdatera dem som du vill.
   
   * **$SubscriptionName:** måste du uppdatera den här variabeln med prenumerationsnamn på din. Gör något av följande tre sätt att hitta namnet på din prenumeration:
     
    a. I **Windows PowerShell ISE**, klickar du på **filen** > **ny** att skapa en ny skriptfil. Kopiera följande skript till nya skriptfilen och klicka på **felsöka** > **kör**. Följande skript kommer först att fråga din Azure autentiseringsuppgifter för att lägga till din Azure-kontot till den lokala PowerShell-miljön och visa alla prenumerationer som är anslutna till den lokala PowerShell-sessionen. Anteckna namnet på den prenumeration som du vill använda när du följa de här självstudierna:
     
    ```powershell
    Add-AzureAccount 
      Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName
    ```

    b. Hitta och kopiera ditt prenumerationsnamn i den [Azure-portalen](https://portal.azure.com), i hubbmenyn till vänster, klickar du på **prenumerationer**. Kopiera prenumerationsnamnet som du vill använda när du kör skripten i den här guiden.
     
     ![Azure Portal](./media/storage-powershell-guide-full/Subscription_Previewportal.png)

    c. Hitta och kopiera ditt prenumerationsnamn i den [klassiska Azure-portalen](https://manage.windowsazure.com/), bläddra nedåt och klicka på **inställningar** på vänster sida av portalen. Klicka på **prenumerationer** att se en lista över dina prenumerationer. Kopiera prenumerationsnamnet som du vill använda när du kör skripten som anges i den här guiden.
     
     ![Klassisk Azure-portal](./media/storage-powershell-guide-full/Subscription_currentportal.png)

   * **$StorageAccountName:** använder det angivna namnet i skriptet eller ange ett nytt namn för ditt lagringskonto. **Viktigt:** namnet på lagringskontot måste vara unikt i Azure. Det måste vara gemena för!
   * **$Location:** använder den angivna ”USA, västra” i skriptet eller välj andra Azure platser, till exempel östra USA, Norra Europa och så vidare.
   * **$ContainerName:** använder det angivna namnet i skriptet eller ange ett nytt namn för din behållare.
   * **$ImageToUpload:** som anger en sökväg till en bild på den lokala datorn: ”C:\Images\HelloWorld.png”.
   * **$DestinationFolder:** anger en sökväg till en lokal katalog att lagra filer som hämtas från Azure Storage, exempelvis: ”C:\DownloadImages”.
8. När du har uppdaterat skriptvariabler i filen ”mystoragescript.ps1” klickar du på **filen** > **spara**. Klicka på **felsöka** > **kör** eller tryck på **F5** att köra skriptet.  

När skriptet körs, bör du ha en lokal målmapp som innehåller hämtade avbildningsfilen. Följande skärmbild visar ett exempel på utdata:

![Ladda ned Blobbar](./media/storage-powershell-guide-full/Blobdownload.png)

> [!NOTE]
> Avsnittet ”komma igång med Azure Storage och PowerShell 5 minuter” tillhandahålls en snabb introduktion på hur du använder Azure PowerShell med Azure Storage. Mer detaljerad information och instruktioner bör du läsa följande avsnitt.
> 

## <a name="prerequisites-for-using-azure-powershell-with-azure-storage"></a>Krav för att använda Azure PowerShell med Azure Storage
Du behöver ett Azure-prenumeration och konto för att köra PowerShell-cmdlets som anges i den här guiden, enligt beskrivningen ovan.

Azure PowerShell är en modul som tillhandahåller för att hantera Azure via Windows PowerShell-cmdlets. Information om att installera och konfigurera Azure PowerShell finns i [hur du installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview). Vi rekommenderar att du hämtar och installerar eller uppgraderar till senaste Azure PowerShell-modulen innan du använder den här guiden.

Du kan köra cmdlets i Windows PowerShell-konsolen som standard eller på Windows PowerShell Integrated Scripting Environment (ISE). Till exempel för att öppna **Windows PowerShell ISE**, gå till Start-menyn, Skriv Administrationsverktyg och på att köra den. Högerklicka på Windows PowerShell ISE i fönstret Administrationsverktyg, klicka på Kör som administratör.

## <a name="how-to-manage-storage-accounts-in-azure"></a>Så här hanterar du storage-konton i Azure

Låt oss ta en titt på Hantera storage-konton i Azure med PowerShell

### <a name="how-to-set-a-default-azure-subscription"></a>Hur du anger en Azure-prenumeration
Du måste autentisera klienten miljön med Azure via Azure Active Directory-autentisering eller certifikatbaserad autentisering för att hantera Azure Storage med hjälp av Azure PowerShell. Detaljerad information finns i [hur du installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview) kursen. Den här guiden använder Azure Active Directory-autentisering.

1. Skriv följande kommando för att lägga till din Azure i Windows PowerShell ISE-kontot till den lokala PowerShell-miljön:

    ```powershell
    Add-AzureAccount
    ```

2. I fönstret ”logga i till Microsoft Azure” anger du e-postadress och lösenord som är associerat med ditt konto. Azure autentiserar och sparar autentiseringsuppgifterna och stänger sedan fönstret.

3. Kör följande kommando för att visa Azure-konton i din lokala PowerShell-miljö, och kontrollera att ditt konto finns i listan:
   
    ```powershell
    Get-AzureAccount
    ```
4. Sedan kör du följande cmdlet om du vill visa alla prenumerationer som är anslutna till den lokala PowerShell-sessionen och kontrollerar du att din prenumeration visas:

    ```powershell
    Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName`
    ```
5. Om du vill ange en Azure-prenumeration, kör du väljer AzureSubscription-cmdlet:

    ```powershell
    $SubscriptionName = 'Your subscription Name'
    Select-AzureSubscription -SubscriptionName $SubscriptionName –Default
    ```

6. Kontrollera namnet på standard-prenumeration genom att köra cmdlet Get-AzureSubscription:

    ```powershell
    Get-AzureSubscription -Default
    ```

7. Om du vill se alla tillgängliga PowerShell-cmdlets för Azure Storage, kör du:
    
    ```powershell
    Get-Command -Module Azure -Noun *Storage*`
    ```

### <a name="how-to-create-a-new-azure-storage-account"></a>Så här skapar du ett nytt Azure storage-konto
För att använda Azure storage, behöver du ett lagringskonto. Du kan skapa ett nytt Azure storage-konto när du har konfigurerat datorn för att ansluta till din prenumeration.

1. Kör cmdleten Get-AzureLocation för att hitta alla tillgängliga datacenter-platser:

    ```powershell
    Get-AzureLocation | Format-Table -Property Name, AvailableServices, StorageAccountTypes
    ```

2. Kör cmdlet New-AzureStorageAccount om du vill skapa ett nytt lagringskonto. I följande exempel skapas ett nytt lagringskonto i datacentret ”USA, västra”.
   
    ```powershell
    $location = "West US"
    $StorageAccountName = "yourstorageaccount"
    New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $location
    ```

> [!IMPORTANT]
> Namnet på ditt lagringskonto måste vara unikt i Azure och måste vara gemener. Namnkonventioner och begränsningar finns i [om Azure Lagringskonton](storage-create-storage-account.md) och [namnge och referera till behållare, Blobbar och Metadata](http://msdn.microsoft.com/library/azure/dd135715.aspx).
> 
> 

### <a name="how-to-set-a-default-azure-storage-account"></a>Hur du ställer in ett standardkonto för Azure storage
Du kan ha flera lagringskonton i din prenumeration. Du kan välja en av dem och ange den som standardkontot för lagring för kommandona för lagring i samma PowerShell-sessionen. På så sätt kan du köra kommandona Azure PowerShell lagring utan att explicit ange lagring-kontexten.

1. Om du vill ange en standardkontot för lagring för din prenumeration kan du köra cmdlet Set-AzureSubscription.

    ```powershell
    $SubscriptionName = "Your subscription name"
    $StorageAccountName = "yourstorageaccount"  
    Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName
    ```

2. Kör cmdleten Get-AzureSubscription för att verifiera att storage-konto är kopplat till ditt standardkonto för prenumerationen. Det här kommandot returnerar prenumerationens egenskaper på den aktuella prenumerationen, inklusive dess aktuella storage-konto.

    ```powershell
    Get-AzureSubscription –Current
    ```

### <a name="how-to-list-all-azure-storage-accounts-in-a-subscription"></a>Visa en lista över alla Azure storage-konton i en prenumeration
Varje Azure-prenumeration kan ha upp till 100 storage-konton. Den senaste informationen om du begränsar finns [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../azure-subscription-service-limits.md).

Kör följande cmdlet för att ta reda på namn och status för storage-konton i den aktuella prenumerationen:

```powershell
Get-AzureStorageAccount | Format-Table -Property StorageAccountName, Location, AccountType, StorageAccountStatus
```

### <a name="how-to-create-an-azure-storage-context"></a>Så här skapar du en Azure storage-kontext
Azure storage-kontexten är ett objekt i PowerShell för att kapsla in autentiseringsuppgifterna för lagring. Med en kontext för lagring medan köra alla efterföljande cmdlet gör att du kan autentisera din begäran utan att ange storage-konto och dess åtkomstnyckel explicit. Du kan skapa en kontext för lagring på många sätt, till exempel med hjälp av lagringskontots namn och åtkomstnyckel, signaturtoken för delad åtkomst (SAS), anslutningssträngen, eller anonym. Mer information finns i [ny AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext).  

Använd någon av följande tre sätt att skapa en kontext för lagring:

* Kör den [Get-AzureStorageKey](/powershell/module/azure.storage/get-azurestoragekey) för att ta reda på den primära lagringsåtkomstnyckel för Azure storage-konto. Därefter anropar den [ny AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext) för att skapa en kontext för lagring:

    ```powershell
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    ```

* Generera en token för delad åtkomst signaturen för ett Azure storage-behållare och använda den för att skapa en kontext för lagring:

    ```powershell
    $sasToken = New-AzureStorageContainerSASToken -Container abc -Permission rl
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -SasToken $sasToken
    ```

    Mer information finns i [ny AzureStorageContainerSASToken](/powershell/module/azure.storage/new-azurestoragecontainersastoken) och [med delad åtkomst signaturer (SAS)](storage-dotnet-shared-access-signature-part-1.md).

* I vissa fall kanske du vill ange Tjänsteslutpunkter när du skapar en ny lagring-kontext. Detta kan vara nödvändigt när du har registrerat ett anpassat domännamn för ditt lagringskonto med Blob-tjänsten eller du vill använda en signatur för delad åtkomst för att komma åt lagringsresurser. Ange Tjänsteslutpunkter i en anslutningssträng och använda den för att skapa en ny lagring kontext enligt nedan:

    ```powershell
    $ConnectionString = "DefaultEndpointsProtocol=http;BlobEndpoint=<blobEndpoint>;QueueEndpoint=<QueueEndpoint>;TableEndpoint=<TableEndpoint>;AccountName=<AccountName>;AccountKey=<AccountKey>"
    $Ctx = New-AzureStorageContext -ConnectionString $ConnectionString
    ```

Läs mer om hur du konfigurerar en lagringsanslutningssträng [konfigurerar anslutningssträngar](storage-configure-connection-string.md).

Nu när du har skapat din dator och lärt dig hur du hanterar prenumerationer och storage-konton med hjälp av Azure PowerShell, gå till nästa avsnitt för att lära dig hur du hanterar Azure-blobbar och blob ögonblicksbilder.

### <a name="how-to-retrieve-and-regenerate-azure-storage-keys"></a>Hur du hämtar och återskapa nycklar för Azure storage
Ett Azure Storage-konto innehåller två nycklar. Du kan använda följande cmdlet för att hämta dina nycklar.

```powershell
Get-AzureStorageKey -StorageAccountName "yourstorageaccount"
```

Du kan använda följande cmdlet för att hämta en viss nyckel. Giltiga värden är primär och sekundär.  

```powershell
(Get-AzureStorageKey -StorageAccountName $StorageAccountName).Primary
    
(Get-AzureStorageKey -StorageAccountName $StorageAccountName).Secondary
```

Om du vill återskapa dina nycklar, använder du följande cmdlet. Giltiga värden för - KeyType är ”primär” och ”sekundär”

```powershell
New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType "Primary"
    
New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType "Secondary"
```

## <a name="how-to-manage-azure-blobs"></a>Hantera Azure-blobbar
Azure Blob storage är en tjänst för att lagra stora mängder Ostrukturerade data, till exempel text eller binära data som kan nås från var som helst i världen via HTTP eller HTTPS. Det här avsnittet förutsätter att du redan är bekant med principerna för Azure Blob Storage-tjänsten. Detaljerad information finns i [komma igång med Blob storage med hjälp av .NET](storage-dotnet-how-to-use-blobs.md) och [Blob-Tjänstkoncept](http://msdn.microsoft.com/library/azure/dd179376.aspx).

### <a name="how-to-create-a-container"></a>Så här skapar du en behållare
Varje blobb i Azure-lagring måste vara i en behållare. Du kan skapa en privat behållare med hjälp av cmdlet New-AzureStorageContainer:

```powershell
$StorageContainerName = "yourcontainername"
New-AzureStorageContainer -Name $StorageContainerName -Permission Off
```

> [!NOTE]
> Det finns tre nivåer av anonym läsbehörighet: **av**, **Blob**, och **behållare**. Om du vill förhindra anonym åtkomst till blobbar, kan du ange parametern behörighet **av**. Som standard den nya behållaren är privat och kan endast användas av ägare. Du vill tillåta anonym offentlig läsbehörighet till blob-resurser, men inte till metadata för behållaren eller i listan över blobbar i behållaren anger parametern behörighet **Blob**. Om du vill ge fullständig offentlig läsbehörighet till blob resurser, metadata för behållaren och listan över blobbar i behållaren, ange parametern behörighet **behållare**. Mer information finns i [Hantera anonym läsbehörighet till behållare och blobbar](storage-manage-access-to-resources.md).
> 
> 

### <a name="how-to-upload-a-blob-into-a-container"></a>Hur du laddar upp en blobb till en behållare
Azure Blob Storage stöder blockblobbar och sidblobbar. Mer information finns i [förstå Blockblobbar, Tilläggsblobbar och Sidblobbar](http://msdn.microsoft.com/library/azure/ee691964.aspx).

Om du vill överföra blobbar i en behållare, kan du använda den [Set AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) cmdlet. Som standard överför det här kommandot lokala filer till en blockblobb. Du kan använda parametern - BlobType om du vill ange typen för blob.

Följande exempel kör den [Get-ChildItem](http://technet.microsoft.com/library/hh849800.aspx) för att hämta alla filer i den angivna mappen och skickar dem till nästa cmdlet genom att använda pipeline. Den [Set AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) cmdlet överför lokala filer till en behållare:

```powershell
Get-ChildItem –Path C:\Images\* | Set-AzureStorageBlobContent -Container "yourcontainername"
```

### <a name="how-to-download-blobs-from-a-container"></a>Hur du hämtar blobbar från en behållare
I följande exempel visar hur du ladda ned blobbar från en behållare. I exempel först upprättar en anslutning till Azure Storage med hjälp av kontexten för lagringskontot, som innehåller lagringskontonamn och den primära åtkomstnyckeln. Sedan exemplet hämtar ett blob-referens med den [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet. Därefter i exemplet används den [Get-AzureStorageBlobContent](/powershell/module/azure.storage/get-azurestorageblobcontent) för att ladda ned blobbar i den lokala målmappen.

```powershell
#Define the variables.
$ContainerName = "yourcontainername"
$DestinationFolder = "C:\DownloadImages"

#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#List all blobs in a container.
$blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

#Download blobs from a container.
New-Item -Path $DestinationFolder -ItemType Directory -Force
$blobs | Get-AzureStorageBlobContent -Destination $DestinationFolder -Context $Ctx
```

### <a name="how-to-copy-blobs-from-one-storage-container-to-another"></a>Kopiera blobbar från en lagringsbehållare till en annan
Du kan kopiera BLOB storage-konton och regioner asynkront. Exemplet nedan visar hur du kopierar blobbar från en lagringsbehållare till en annan i två olika lagringskonton. I exemplet först ställer in variabler för käll- och storage-konton och skapar sedan en kontext för lagring för varje konto. Därefter exemplet kopierar blobbar från behållaren källa till mål-behållaren med den [Start AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) cmdlet. Exemplet förutsätter att käll- och storage-konton och behållare som redan finns.

```powershell
#Define the source storage account and context.
$SourceStorageAccountName = "yoursourcestorageaccount"
$SourceStorageAccountKey = "Storage key for yoursourcestorageaccount"
$SrcContainerName = "yoursrccontainername"
$SourceContext = New-AzureStorageContext -StorageAccountName $SourceStorageAccountName -StorageAccountKey $SourceStorageAccountKey

#Define the destination storage account and context.
$DestStorageAccountName = "yourdeststorageaccount"
$DestStorageAccountKey = "Storage key for yourdeststorageaccount"
$DestContainerName = "destcontainername"
$DestContext = New-AzureStorageContext -StorageAccountName $DestStorageAccountName -StorageAccountKey $DestStorageAccountKey

#Get a reference to blobs in the source container.
$blobs = Get-AzureStorageBlob -Container $SrcContainerName -Context $SourceContext

#Copy blobs from one container to another.
$blobs| Start-AzureStorageBlobCopy -DestContainer $DestContainerName -DestContext $DestContext
```

Observera att det här exemplet utför en asynkron kopia. Du kan övervaka statusen för varje kopia genom att köra den [Get-AzureStorageBlobCopyState](/powershell/module/azure.storage/start-azurestorageblobcopystate) cmdlet.

### <a name="how-to-copy-blobs-from-a-secondary-location"></a>Hur du kopierar blobar från en sekundär plats
Du kan kopiera blobbar från den sekundära platsen för RA-GRS-aktiverat konto.

```powershell
#define secondary storage context using a connection string constructed from secondary endpoints.
$SrcContext = New-AzureStorageContext -ConnectionString "DefaultEndpointsProtocol=https;AccountName=***;AccountKey=***;BlobEndpoint=http://***-secondary.blob.core.windows.net;FileEndpoint=http://***-secondary.file.core.windows.net;QueueEndpoint=http://***-secondary.queue.core.windows.net; TableEndpoint=http://***-secondary.table.core.windows.net;"
Start-AzureStorageBlobCopy –Container *** -Blob *** -Context $SrcContext –DestContainer *** -DestBlob *** -DestContext $DestContext
```

### <a name="how-to-delete-a-blob"></a>Hur du tar bort en blobb
Om du vill ta bort en blobb först hämta en blobbreferens och anropa sedan cmdleten Remove-AzureStorageBlob på den. I följande exempel tar bort alla blobbar i en viss behållare. I exemplet först ställer in variabler för ett lagringskonto och skapar sedan en kontext för lagring. Därefter exemplet hämtar ett blob-referens med den [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet och kör den [ta bort AzureStorageBlob](/powershell/module/azure.storage/remove-azurestorageblob) för att ta bort blobbar från en behållare i Azure-lagring.

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$ContainerName = "containername"
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Get a reference to all the blobs in the container.
$blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

#Delete blobs in a specified container.
$blobs| Remove-AzureStorageBlob
```

## <a name="how-to-manage-azure-blob-snapshots"></a>Hur du hanterar Azure blob-ögonblicksbilder
Azure kan du skapa en ögonblicksbild av en blob. En ögonblicksbild är en skrivskyddad version av en blob som utförs i en punkt i tiden. När en ögonblicksbild har skapats, kan den läsa, kopieras, eller ta bort, men inte har ändrats. Ögonblicksbilder är ett sätt att säkerhetskopiera en blob som det visas vid en tidpunkt. Mer information finns i [skapa en ögonblicksbild av en Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).

### <a name="how-to-create-a-blob-snapshot"></a>Så här skapar du en blob-ögonblicksbild
Om du vill skapa en snaphot för en blobb först hämta en blobbreferens och sedan anropa den `ICloudBlob.CreateSnapshot` metoden på den. I följande exempel först ställer in variabler för ett lagringskonto och skapar sedan en kontext för lagring. Därefter exemplet hämtar ett blob-referens med den [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet och kör den [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) metod för att skapa en ögonblicksbild.

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$ContainerName = "yourcontainername"
$BlobName = "yourblobname"
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Get a reference to a blob.
$blob = Get-AzureStorageBlob -Context $Ctx -Container $ContainerName -Blob $BlobName

#Create a snapshot of the blob.
$snap = $blob.ICloudBlob.CreateSnapshot()
```

### <a name="how-to-list-a-blobs-snapshots"></a>Visa en lista över en blob ögonblicksbilder
Du kan skapa så många ögonblicksbilder som du vill använda för en blob. Du kan visa ögonblicksbilder kopplade till ditt blob att spåra din aktuella ögonblicksbilder. I följande exempel används en fördefinierad blob och anrop i [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) för att visa en lista med ögonblicksbilder för blobben.  

```powershell
#Define the blob name.
$BlobName = "yourblobname"

#List the snapshots of a blob.
Get-AzureStorageBlob –Context $Ctx -Prefix $BlobName -Container $ContainerName  | Where-Object  { $_.ICloudBlob.IsSnapshot -and $_.Name -eq $BlobName }
```

### <a name="how-to-copy-a-snapshot-of-a-blob"></a>Så här kopierar du en ögonblicksbild av en blob
Du kan kopiera en ögonblicksbild av en blob att återställa ögonblicksbilden. Detaljerad information och begränsningar finns i [skapa en ögonblicksbild av en Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx). I följande exempel först ställer in variabler för ett lagringskonto och skapar sedan en kontext för lagring. Exemplet definierar sedan variablerna behållare och blob namn. I exempel hämtar ett blob-referens med den [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet och kör den [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) metod för att skapa en ögonblicksbild. Sedan exemplet körs den [Start AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) för att kopiera ögonblicksbilden av en blob med ICloudBlob-objektet för käll-blob. Glöm inte att uppdatera variablerna baserat på din konfiguration innan du kör exemplet. Observera att följande exempel förutsätter att källan och målet behållare och käll-blob finns redan.

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Define the variables.
$SrcContainerName = "yoursourcecontainername"
$DestContainerName = "yourdestcontainername"
$SrcBlobName = "yourblobname"
$DestBlobName = "CopyBlobName"

#Get a reference to a blob.
$blob = Get-AzureStorageBlob -Context $Ctx -Container $SrcContainerName -Blob $SrcBlobName

#Create a snapshot of a blob.
$snap = $blob.ICloudBlob.CreateSnapshot()

#Copy the snapshot to another container.
Start-AzureStorageBlobCopy –Context $Ctx -ICloudBlob $snap -DestBlob $DestBlobName -DestContainer $DestContainerName
```

Nu när du har lärt dig hur du hanterar Azure-blobbar och blob-ögonblicksbilder med Azure PowerShell, gå till nästa avsnitt lära dig hur du hanterar tabeller, köer och filer.

## <a name="how-to-manage-azure-tables-and-table-entities"></a>Hantera Azure-tabeller och tabellentiteter
Azure Table storage-tjänsten är en NoSQL-datalager som du kan använda för att lagra och fråga stora mängder strukturerad, icke-relationella data. Huvudkomponenterna i tjänsten är tabeller, enheter och egenskaper. En tabell är en samling med entiteter. En entitet är en uppsättning egenskaper. Varje entitet kan ha upp till 252 egenskaper, som är alla namn / värde-par. Det här avsnittet förutsätter att du redan är bekant med principerna för Azure Table Storage-tjänsten. Detaljerad information finns i [förstå den tabelltjänst-datamodellen](http://msdn.microsoft.com/library/azure/dd179338.aspx) och [komma igång med Azure Table storage med hjälp av .NET](storage-dotnet-how-to-use-tables.md).

I följande underavsnitt lär du dig hur du hanterar Azure Table storage-tjänsten med hjälp av Azure PowerShell. Scenarier som tas upp inkluderar **skapar**, **bort**, och **hämtar** **tabeller**, samt **att lägga till**, **frågar**, och **bort tabellentiteter**.

### <a name="how-to-create-a-table"></a>Så här skapar du en tabell
Varje tabell måste finnas i ett Azure storage-konto. I följande exempel visar hur du skapar en tabell i Azure Storage. I exempel först upprättar en anslutning till Azure Storage med hjälp av kontexten för lagringskontot, som innehåller lagringskontonamn och dess snabbtangent. Därefter används den [ny AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) för att skapa en tabell i Azure Storage.

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey

#Create a new table.
$tabName = "yourtablename"
New-AzureStorageTable –Name $tabName –Context $Ctx
```

### <a name="how-to-retrieve-a-table"></a>Hur du hämtar en tabell
Du kan fråga efter och hämta en eller alla tabeller i ett lagringskonto. I följande exempel visar hur du hämtar en given tabell med hjälp av den [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet.

```powershell
#Retrieve a table.
$tabName = "yourtablename"
Get-AzureStorageTable –Name $tabName –Context $Ctx
```

Om du anropar cmdleten Get-AzureStorageTable utan några parametrar hämtar alla lagringstabeller som för ett lagringskonto.

### <a name="how-to-delete-a-table"></a>Hur du tar bort en tabell
Du kan ta bort en tabell från ett lagringskonto med hjälp av den [ta bort AzureStorageTable](/powershell/module/azure.storage/remove-azurestoragetable) cmdlet.  

```powershell
#Delete a table.
$tabName = "yourtablename"
Remove-AzureStorageTable –Name $tabName –Context $Ctx
```

### <a name="how-to-manage-table-entities"></a>Så här hanterar du tabellentiteter
För närvarande tillhandahåller inte Azure PowerShell-cmdletar för att hantera tabellentiteter direkt. Om du vill utföra åtgärder på tabellentiteter, du kan använda de klasser som anges i den [Azure Storage-klientbibliotek för .NET](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx).

#### <a name="how-to-add-table-entities"></a>Hur du lägger till tabellentiteter
Om du vill lägga till en entitet i en tabell, först skapa ett objekt som definierar egenskaper för enhet. En entitet kan ha upp till 255 egenskaper, inklusive 3 Systemegenskaper: **PartitionKey**, **RowKey**, och **tidsstämpel**. Du ansvarar för att lägga till och uppdatera värdena för **PartitionKey** och **RowKey**. Servern hanterar värde för **tidsstämpel**, som inte ändras. Tillsammans i **PartitionKey** och **RowKey** identifiera varje entitet i en tabell.

* **PartitionKey**: Anger den partition som entiteten lagras i.
* **RowKey**: identifierar entiteten i partitionen.

Du kan ange upp till 252 anpassade egenskaper för en entitet. Mer information finns i [förstå den tabelltjänst-datamodellen](http://msdn.microsoft.com/library/azure/dd179338.aspx).

I följande exempel visar hur du lägger till entiteter i en tabell. Exemplet visar hur du hämtar tabellen Personal och lägga till flera enheter till den. Först upprättar den en anslutning till Azure Storage med hjälp av kontexten för lagringskontot, som innehåller lagringskontonamn och dess snabbtangent. Sedan hämtar den angivna tabellen med hjälp av den [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet. Om tabellen inte finns i [ny AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) cmdlet som används för att skapa en tabell i Azure Storage. Därefter definierar exemplet en egen funktion Add-enheten för att lägga till entiteter i tabellen genom att ange varje entitet partition och radnyckel. Lägg till entiteten funktionsanrop den [New-Object](http://technet.microsoft.com/library/hh849885.aspx) cmdleten på den [Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.dynamictableentity.aspx) klassen för att skapa ett entitetsobjekt. Senare exemplet anropar den [Microsoft.WindowsAzure.Storage.Table.TableOperation.Insert](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.insert.aspx) -metoden i den här enhetsobjekt lägga till den i en tabell.

```powershell
#Function Add-Entity: Adds an employee entity to a table.
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

#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$TableName = "Employees"

#Retrieve the table if it already exists.
$table = Get-AzureStorageTable –Name $TableName -Context $Ctx -ErrorAction Ignore

#Create a new table if it does not exist.
if ($table -eq $null)
{
   $table = New-AzureStorageTable –Name $TableName -Context $Ctx
}

#Add multiple entities to a table.
Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row1 -Name Chris -Id 1
Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row2 -Name Jessie -Id 2
Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row1 -Name Christine -Id 3
Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row2 -Name Steven -Id 4
```

#### <a name="how-to-query-table-entities"></a>Hur man frågar tabellentiteter
Om du vill fråga en tabell, använder den [Microsoft.WindowsAzure.Storage.Table.TableQuery](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tablequery.aspx) klass. Följande exempel förutsätter att du redan har kört skriptet i hur du lägger till entiteter avsnitt i handboken. I exempel först upprättar en anslutning till Azure Storage med hjälp av lagring kontext, som innehåller lagringskontonamn och dess snabbtangent. Därefter görs ett försök att hämta den tidigare skapade ”anställda” tabell genom att använda den [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet. Anropar den [New-Object](http://technet.microsoft.com/library/hh849885.aspx) cmdlet på klassen Microsoft.WindowsAzure.Storage.Table.TableQuery skapar ett nytt frågeobjekt. I exempel söker efter de enheter som har en ”ID”-kolumn vars värde är 1 som anges i ett sträng-filter. Detaljerad information finns i [frågor till tabeller och de entiteter](http://msdn.microsoft.com/library/azure/dd894031.aspx). När du kör den här frågan returnerar alla entiteter som matchar filterkriterierna.

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$TableName = "Employees"

#Get a reference to a table.
$table = Get-AzureStorageTable –Name $TableName -Context $Ctx

#Create a table query.
$query = New-Object Microsoft.WindowsAzure.Storage.Table.TableQuery

#Define columns to select.
$list = New-Object System.Collections.Generic.List[string]
$list.Add("RowKey")
$list.Add("ID")
$list.Add("Name")

#Set query details.
$query.FilterString = "ID gt 0"
$query.SelectColumns = $list
$query.TakeCount = 20

#Execute the query.
$entities = $table.CloudTable.ExecuteQuery($query)

#Display entity properties with the table format.
$entities  | Format-Table PartitionKey, RowKey, @{ Label = "Name"; Expression={$_.Properties["Name"].StringValue}}, @{ Label = "ID"; Expression={$_.Properties["ID"].Int32Value}} -AutoSize
```

#### <a name="how-to-delete-table-entities"></a>Ta bort tabellentiteter
Du kan ta bort en entitet med dess partition och raden nycklar. Följande exempel förutsätter att du redan har kört skriptet i hur du lägger till entiteter avsnitt i handboken. I exempel först upprättar en anslutning till Azure Storage med hjälp av lagring kontext, som innehåller lagringskontonamn och dess snabbtangent. Därefter görs ett försök att hämta den tidigare skapade ”anställda” tabell genom att använda den [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet. Om tabellen finns i exemplet anropar den [Microsoft.WindowsAzure.Storage.Table.TableOperation.Retrieve](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.retrieve.aspx) metod för att hämta en entitet baserat på dess partition och raden nyckelvärden. Skickar sedan enheten till den [Microsoft.WindowsAzure.Storage.Table.TableOperation.Delete](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.delete.aspx) metod för att ta bort.

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

#Retrieve the table.
$TableName = "Employees"
$table = Get-AzureStorageTable -Name $TableName -Context $Ctx -ErrorAction Ignore

#If the table exists, start deleting its entities.
if ($table -ne $null) 
{
    #Together the PartitionKey and RowKey uniquely identify every  
    #entity within a table.
    $tableResult = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Retrieve("Partition2", "Row1"))
    $entity = $tableResult.Result
    if ($entity -ne $null)
    {
        #Delete the entity.
        $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Delete($entity))
    }
}
```

## <a name="how-to-manage-azure-queues-and-queue-messages"></a>Hantera Azure-köer och -meddelanden i kö
Azure Queue Storage är en tjänst för att lagra stora mängder meddelanden som kan nås från var som helst i världen via autentiserade anrop med HTTP eller HTTPS. Det här avsnittet förutsätter att du redan är bekant med Azure Queue Storage Service-principerna. Detaljerad information finns i [komma igång med Azure Queue storage med hjälp av .NET](storage-dotnet-how-to-use-queues.md).

Det här avsnittet visas hur du hanterar Azure Queue storage-tjänst med hjälp av Azure PowerShell. Scenarier som tas upp inkluderar **infoga** och **bort** kö meddelanden, samt **skapar**, **ta bort**, och **hämta köer**.

### <a name="how-to-create-a-queue"></a>Så här skapar du en kö
I följande exempel upprättar en anslutning till Azure Storage med hjälp av kontexten för lagringskontot, som innehåller lagringskontonamn och dess åtkomstnyckel först. Därefter anropar [ny AzureStorageQueue](/powershell/module/azure.storage/new-azurestoragequeue) för att skapa en kö med namnet 'könamn'.

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$QueueName = "queuename"
$Queue = New-AzureStorageQueue –Name $QueueName -Context $Ctx
```

Information om namngivningskonventioner för Azure-kötjänsten finns [namngivning av köer och Metadata](http://msdn.microsoft.com/library/azure/dd179349.aspx).

### <a name="how-to-retrieve-a-queue"></a>Så här hämtar du en kö
Du kan fråga efter och hämta en särskild kö eller en lista över alla köer i ett lagringskonto. I följande exempel visar hur du hämtar en kö som anges med hjälp av den [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) cmdlet.

```powershell
#Retrieve a queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue –Name $QueueName –Context $Ctx
```

Om du anropar den [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) cmdlet utan några parametrar får den en lista över alla köer.

### <a name="how-to-delete-a-queue"></a>Hur du tar bort en kö
Anropa Remove-AzureStorageQueue cmdlet för att ta bort en kö och alla meddelanden i den. I följande exempel visas hur du tar bort en kö som anges med cmdlet Remove-AzureStorageQueue.

```powershell
#Delete a queue.
$QueueName = "yourqueuename"
Remove-AzureStorageQueue –Name $QueueName –Context $Ctx
```

#### <a name="how-to-insert-a-message-into-a-queue"></a>Infoga ett meddelande i en kö
Om du vill infoga ett meddelande i en befintlig kö, först skapa en ny instans av den [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) klass. Därefter anropar du [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx)-metoden. En CloudQueueMessage kan skapas från en sträng (i UTF-8-format) eller en byte-matris.

Exemplet nedan visar hur du lägger till meddelandet till en kö. I exempel först upprättar en anslutning till Azure Storage med hjälp av kontexten för lagringskontot, som innehåller lagringskontonamn och dess snabbtangent. Sedan hämtar den kö som anges med hjälp av den [Get-AzureStorageQueue](https://msdn.microsoft.com/library/azure/dn806377.aspx) cmdlet. Om kön finns på [New-Object](http://technet.microsoft.com/library/hh849885.aspx) cmdlet som används för att skapa en instans av den [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) klass. Senare exemplet anropar den [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) -metoden i det här meddelandeobjektet att lägga till en kö. Här är kod som hämtar en kö och infogar meddelandet ”MessageInfo':

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

#Retrieve the queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

#If the queue exists, add a new message.
if ($Queue -ne $null) {
   # Create a new message using a constructor of the CloudQueueMessage class.
   $QueueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList MessageInfo

   # Add a new message to the queue.
   $Queue.CloudQueue.AddMessage($QueueMessage)
}
```

#### <a name="how-to-de-queue-at-the-next-message"></a>Så här frigör kö nästa meddelande
Koden tar bort ett meddelande från en kö i två steg. När du anropar den [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.GetMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.getmessage.aspx) metod du hämta nästa meddelande i en kö. Ett meddelande som returneras från **GetMessage** blir osynligt för andra meddelanden som läser kod i den här kön. Om du vill slutföra borttagningen av meddelandet från kön, måste du också anropa den [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.DeleteMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.deletemessage.aspx) metod. Den här tvåstegsprocessen för att ta bort ett meddelande säkerställer att om din kod inte kan bearbeta ett meddelande på grund av ett maskin- eller programvarufel så kan en annan instans av koden hämta samma meddelande och försöka igen. Koden anropar **DeleteMessage** direkt efter att meddelandet har bearbetats.

```powershell
# Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

# Retrieve the queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

$InvisibleTimeout = [System.TimeSpan]::FromSeconds(10)

# Get the message object from the queue.
$QueueMessage = $Queue.CloudQueue.GetMessage($InvisibleTimeout)
# Delete the message.
$Queue.CloudQueue.DeleteMessage($QueueMessage)
```

## <a name="how-to-manage-azure-file-shares-and-files"></a>Hantera Azure-filresurser och filer
Azure File storage erbjuder delad lagring för program som använder SMB-standardprotokollet. Microsoft Azure-datorer och molntjänster kan dela fildata över programkomponenter via monterade resurser och lokala program kan komma åt fildata på en resurs via fil-API för storage eller Azure PowerShell.

Mer information om Azure File storage finns [Kom igång med Azure File storage i Windows](storage-dotnet-how-to-use-files.md) och [File Service REST API](http://msdn.microsoft.com/library/azure/dn167006.aspx).

## <a name="how-to-set-and-query-storage-analytics"></a>Hur du ställer in och frågan storage analytics
Du kan använda [Azure Storage Analytics](storage-analytics.md) samla in statistik för Azure storage-konton och logga in data om förfrågningar som skickas till ditt lagringskonto. Du kan använda storage-mätvärden för att övervaka hälsotillståndet för ett lagringskonto och lagring loggning för att diagnostisera och felsöka problem med ditt lagringskonto. Du kan konfigurera övervakning med Azure-portalen eller Windows PowerShell eller via programmering med storage-klientbiblioteket. Lagring loggning sker serversidan och gör att du kan registrera information för både slutförda och misslyckade begäranden på ditt lagringskonto. Dessa loggar kan du se detaljer för Läs-, Skriv- och delete-åtgärder mot dina tabeller, köer, blobbar samt och orsakerna till misslyckade begäranden.

Information om hur du aktiverar och visa Storage Metrics data med hjälp av PowerShell finns [aktivera mätvärden för lagring med hjälp av PowerShell](http://msdn.microsoft.com/library/azure/dn782843.aspx#HowtoenableStorageMetricsusingPowerShell).

Information om hur du aktiverar och hämta data för loggning av lagring med hjälp av PowerShell finns [Aktivera lagring loggning med hjälp av PowerShell](http://msdn.microsoft.com/library/azure/dn782840.aspx#HowtoenableStorageLoggingusingPowerShell) och [hitta din lagring loggning loggdata](http://msdn.microsoft.com/library/azure/dn782840.aspx#FindingyourStorageLogginglogdata).
Detaljerad information om hur du använder Storage-mätvärden och lagring loggning för felsökning av problem med lagring finns [övervakning, diagnostisera och felsöka Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="how-to-manage-shared-access-signature-sas-and-stored-access-policy"></a>Hantera delade signatur åtkomst (SAS) och lagras åtkomstprincipen
Signaturer för delad åtkomst är en viktig del av säkerhetsmodellen för alla program som använder Azure Storage. De är användbara för att tillhandahålla begränsade behörigheter till ditt lagringskonto till klienter som inte ska ha kontonyckel. Som standard kan endast ägaren av storage-konto åtkomst till blobbar, tabeller och köer i kontot. Om tjänsten eller programmet måste göra resurserna tillgängliga för andra klienter utan att dela din åtkomstnyckel, finns det tre alternativ:

* Ange en behållare behörighet för att tillåta anonym läsbehörighet till behållaren och dess blobbar. Detta är inte tillåtet för tabeller och köer.
* Använda en signatur för delad åtkomst som ger begränsad behörighet till behållare, blobbar, köer och tabeller för ett visst tidsintervall.
* Använda en lagrad åtkomstprincip för att hämta ytterligare en säkerhetsnivå för kontroll över signaturer för delad åtkomst för en behållare eller dess blobbar, en kö eller en tabell. Den lagrade åtkomstprincipen kan du ändra starttid, förfallotiden eller behörigheter för en signatur, eller om du vill återkalla efter att det har utfärdats.

En signatur för delad åtkomst kan ha ett av två sätt:

* **Ad hoc-SAS**: när du skapar en ad hoc-SAS starttid, förfallotid, och behörigheter för SAS har angetts för SAS-URI. Den här typen av SAS kan skapas på en behållare, blob, tabell eller kön och det är icke-återkallningsbar.
* **SAS med lagrade åtkomstprincip**: en lagrade åtkomstprincip definieras på resursbehållaren en blob-behållare, tabeller eller kö - och du kan använda för att hantera begränsningar för en eller flera signaturer för delad åtkomst. När du kopplar en SAS med en lagrad till ärver SAS begränsningarna - starttid, förfallotiden och behörigheter - har definierats för den lagrade åtkomstprincipen. Den här typen av SAS är återkallningsbar.

Mer information finns i [med delad åtkomst signaturer (SAS)](storage-dotnet-shared-access-signature-part-1.md) och [Hantera anonym läsbehörighet till behållare och blobbar](storage-manage-access-to-resources.md).

I nästa avsnitt får du lära dig hur du skapar en signatur åtkomst-token och lagrade princip för delad åtkomst för Azure-tabeller. Azure PowerShell tillhandahåller liknande cmdlets för behållare, blobbar och köer samt. Om du vill köra skript i det här avsnittet kan du hämta den [Azure PowerShell version 0.8.14](http://go.microsoft.com/?linkid=9811175&clcid=0x409) eller senare.

### <a name="how-to-create-a-policy-based-shared-access-signature-token"></a>Så här skapar du en principbaserad signatur för delad åtkomst-token
Använd cmdleten New-AzureStorageTableStoredAccessPolicy för att skapa en ny princip för lagrade åtkomst. Anropa sedan den [ny AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) för att skapa en ny policy-baserad delad signatur åtkomsttoken för en tabell med Azure Storage.

```powershell
$policy = "policy1"
New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
New-AzureStorageTableSASToken -Name $tableName -Policy $policy -Context $Ctx
```

### <a name="how-to-create-an-ad-hoc-non-revocable-shared-access-signature-token"></a>Så här skapar du en ad hoc-(icke-återkallningsbar) signatur för delad åtkomst-token
Använd den [ny AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) för att skapa en ny ad hoc-(icke-återkallningsbar) signatur för delad åtkomst-token för en tabell med Azure Storage:

```powershell
New-AzureStorageTableSASToken -Name $tableName -Permission "rqud" -StartTime "2015-01-01" -ExpiryTime "2015-02-01" -Context $Ctx
```
    
### <a name="how-to-create-a-stored-access-policy"></a>Så här skapar du en princip för lagrade åtkomst
Använd cmdlet New-AzureStorageTableStoredAccessPolicy för att skapa lagrade åtkomstprincip för en tabell med Azure Storage:

```powershell
$policy = "policy1"
New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
```
    
### <a name="how-to-update-a-stored-access-policy"></a>Så här uppdaterar du en princip för lagrade åtkomst
Använd cmdlet Set-AzureStorageTableStoredAccessPolicy för att uppdatera en befintlig lagrade åtkomstprincip för en tabell med Azure Storage:

```powershell
Set-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Permission "rd" -NoExpiryTime -NoStartTime -Context $Ctx
```

### <a name="how-to-delete-a-stored-access-policy"></a>Hur du tar bort en princip för lagrade åtkomst
Använd cmdleten Remove-AzureStorageTableStoredAccessPolicy för att ta bort en princip för lagrade åtkomst på en tabell i Azure Storage:

```powershell
Remove-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Context $Ctx
```

## <a name="how-to-use-azure-storage-for-us-government-and-azure-china"></a>Hur du använder Azure Storage för amerikanska myndigheter och Azure Kina
En Azure-miljö är en oberoende distribution av Microsoft Azure som [Azure Government för USA: s regering](https://azure.microsoft.com/features/gov/), [AzureCloud för globala Azure](https://portal.azure.com), och [AzureChinaCloud för Azure som drivs av 21Vianet i Kina](http://www.windowsazure.cn/). Du kan distribuera nya miljöer i Azure för amerikanska myndigheter och Azure Kina.

Om du vill använda Azure Storage med AzureChinaCloud, måste du skapa en kontext för lagring som är associerad med AzureChinaCloud. Följ dessa steg om du vill komma igång:

1. Kör den [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) för att visa tillgängliga Azure miljöer:
   
    ```powershell
    Get-AzureEnvironment
    ```

2. Lägg till ett Azure Kina konto i Windows PowerShell:
   
    ```powershell
    Add-AzureAccount –Environment AzureChinaCloud
    ```

3. Skapa en kontext för lagring för ett AzureChinaCloud konto:
   
    ```powershell
    $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureChinaCloud
    ```

Använda Azure Storage med [USA Azure Government](https://azure.microsoft.com/features/gov/), bör du definiera en ny miljö och sedan skapa en ny lagring kontext med den här miljön:

1. Kör den [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) för att visa tillgängliga Azure miljöer:

    ```powershell
    Get-AzureEnvironment
    ```

2. Lägg till ett konto i Azure som tillhör amerikanska myndigheter i Windows PowerShell:
   
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
I den här guiden har du lärt dig hur du hanterar Azure Storage med Azure PowerShell. Här följer några relaterade artiklar och resurser för att lära dig mer om dem.

* [Azure Storage-dokumentation](https://azure.microsoft.com/documentation/services/storage/)
* [PowerShell-Cmdlets för Azure Storage](/powershell/module/azurerm.storage/#storage)
* [Windows PowerShell-referens](https://msdn.microsoft.com/library/ms714469.aspx)

[Getting started with Azure Storage and PowerShell in 5 minutes]: #getstart
[Prerequisites for using Azure PowerShell with Azure Storage]: #pre
[How to manage storage accounts in Azure]: #manageaccount
[How to set a default Azure subscription]: #setdefsub
[How to create a new Azure storage account]: #createaccount
[How to set a default Azure storage account]: #defaultaccount
[How to list all Azure storage accounts in a subscription]: #listaccounts
[How to create an Azure storage context]: #createctx
[How to manage Azure blobs and blob snapshots]: #manageblobs
[How to create a container]: #container
[How to upload a blob into a container]: #uploadblob
[How to download blobs from a container]: #downblob
[How to copy blobs from one storage container to another]: #copyblob
[How to delete a blob]: #deleteblob
[How to manage Azure blob snapshots]: #manageshots
[How to create a blob snapshot]: #createshot
[How to list snapshots of a blob]: #listshot
[How to copy a snapshot of a blob]: #copyshot
[How to manage Azure tables and table entities]: #managetables
[How to create a table]: #createtable
[How to retrieve a table]: #gettable
[How to delete a table]: #remtable
[How to manage table entities]: #mngentity
[How to add table entities]: #addentity
[How to query table entities]: #queryentity
[How to delete table entities]: #deleteentity
[How to manage Azure queues and queue messages]: #managequeues
[How to create a queue]: #createqueue
[How to retrieve a queue]: #getqueue
[How to delete a queue]: #remqueue
[How to manage queue messages]: #mngqueuemsg
[How to insert a message into a queue]: #addqueuemsg
[How to de-queue at the next message]: #dequeuemsg
[How to manage Azure file shares and files]: #files
[How to set and query storage analytics]: #stganalytics
[How to manage Shared Access Signature (SAS) and Stored Access Policy]: #sas
[How to use Azure Storage for U.S. government and Azure China]: #gov
[Next Steps]: #next

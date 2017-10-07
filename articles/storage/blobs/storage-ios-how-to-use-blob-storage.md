---
title: "aaaHow toouse Azure Blob storage från iOS | Microsoft Docs"
description: Lagra Ostrukturerade data i hello moln med Azure Blob storage (objektlagring).
services: storage
documentationcenter: ios
author: michaelhauss
manager: vamshik
editor: tysonn
ms.assetid: df188021-86fc-4d31-a810-1b0e7bcd814b
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: objective-c
ms.topic: article
ms.date: 05/11/2017
ms.author: michaelhauss
ms.openlocfilehash: cc08b76b682537a9a51e970c76bd76c7c06a4ccb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blob-storage-from-ios"></a>Hur toouse Blob storage från iOS
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-blobs](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Översikt
Den här artikeln visar hur tooperform vanliga scenarier med hjälp av Microsoft Azure Blob storage. hello exemplen är skrivna i Objective-C och använder hello [Azure Storage-klientbibliotek för iOS](https://github.com/Azure/azure-storage-ios). hello beskrivs scenarier där **överför**, **lista**, **hämtar**, och **bort** blobbar. Mer information om BLOB finns hello [nästa steg](#next-steps) avsnitt. Du kan också hämta hello [exempelapp](https://github.com/Azure/azure-storage-ios/tree/master/BlobSample) tooquickly finns hello användningen av Azure Storage i ett iOS-program.

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="import-hello-azure-storage-ios-library-into-your-application"></a>Importera hello Azure Storage iOS biblioteket i ditt program
Du kan importera hello Azure Storage iOS biblioteket till ditt program genom att använda hello [Azure Storage CocoaPod](https://cocoapods.org/pods/AZSClient) eller genom att importera hello **Framework** fil. CocoaPod är hello rekommenderade sätt som gör det integrerande hello biblioteket enklare, men importera från hello framework-filen är mindre påverkan för ditt befintliga projekt.

toouse det här biblioteket måste hello följande:
- iOS 8 +
- Xcode 7 +

## <a name="cocoapod"></a>CocoaPod
1. Om du inte redan gjort det, [installera CocoaPods](https://guides.cocoapods.org/using/getting-started.html#toc_3) på datorn genom att öppna ett terminalfönster och kör följande kommando hello
    
    ```shell   
    sudo gem install cocoapods
    ```

2. Skapa sedan en ny fil med namnet i hello projektkatalogen (hello katalog som innehåller filen .xcodeproj) _Podfile_(utan filtillägget). Lägg till följande too_Podfile_ hello och spara.

    ```ruby
    platform :ios, '8.0'

    target 'TargetName' do
      pod 'AZSClient'
    end
    ```

3. Navigera i hello terminalfönster, toohello projektkatalogen och kör hello följande kommando

    ```shell    
    pod install
    ```

4. Stäng om din .xcodeproj är öppen i Xcode. I projektet directory öppna hello nyskapade projekt som kommer att ha hello .xcworkspace tillägg. Detta är hello-fil som du ska arbeta från för nu på.

## <a name="framework"></a>Framework
hello andra sätt toouse hello biblioteket är toobuild hello framework manuellt:

1. Första, hämta eller klona hello [lagringsplatsen för azure-lagring – ios](https://github.com/azure/azure-storage-ios).
2. Gå till *azure-lagring – ios* -> *Lib* -> *Azure Storage-klientbibliotek*, och öppna `AZSClient.xcodeproj` i Xcode.
3. Vid hello övre vänstra xcode, ändra hello active schemat från ”Azure Storage-klientbibliotek” för ”Framework”.
4. Skapa hello projekt (⌘ + B). Detta skapar en `AZSClient.framework` filen på skrivbordet.

Du kan sedan importera hello framework filen till programmet hello följande:

1. Skapa ett nytt projekt eller öppna ett befintligt projekt i Xcode.
2. Dra och släpp hello `AZSClient.framework` i projektnavigeringen Xcode.
3. Välj *kopiera objekt vid behov*, och klicka på *Slutför*.
4. Klicka på ditt projekt i hello vänstra navigeringsfönstret och klickar på hello *allmänna* fliken hello överst i hello projektet editor.
5. Under hello *länkade ramverk och bibliotek* klickar du på knappen Lägg till för hello (+).
6. Sök efter i hello listan över bibliotek som redan tillhandahålls `libxml2.2.tbd` och lägga till den tooyour projekt.

## <a name="import-hello-library"></a>Importera hello bibliotek 
```objc
// Include hello following import statement toouse blob APIs.
#import <AZSClient/AZSClient.h>
```

Om du använder Swift kommer du behöver toocreate datacenterbryggning rubrik och importera < AZSClient/AZSClient.h > det:

1. Skapa en huvudfilen `Bridging-Header.h`, och Lägg till hello ovan import-sats.
2. Gå toohello *Versionsinställningar* fliken och Sök efter *Interimshuvudfilen med Objective-C*.
3. Dubbelklicka på hello-fältet för *Interimshuvudfilen med Objective-C* och Lägg till huvudfilen för hello sökvägen tooyour:`ProjectName/Bridging-Header.h`
4. Build hello projekt (⌘ + B) tooverify som hello datacenterbryggning huvud hämtades av Xcode.
5. Börja använda hello biblioteket direkt i Swift-filen, om det behövs ingen importuttryck.

[!INCLUDE [storage-mobile-authentication-guidance](../../../includes/storage-mobile-authentication-guidance.md)]

## <a name="asynchronous-operations"></a>Asynkrona åtgärder
> [!NOTE]
> Alla metoder som utför en begäran mot hello-tjänsten är asynkrona åtgärder. Du hittar att metoderna har en hanterare för slutförande i hello-kodexempel. Kod inuti hello slutförande hanterare körs **när** hello begäran har slutförts. Code när hello slutförande hanteraren kommer att köras **medan** hello begäran görs.
> 
> 

## <a name="create-a-container"></a>Skapa en behållare
Varje blobb i Azure Storage måste finnas i en behållare. hello följande exempel visas hur toocreate en behållare kallas *newcontainer*, i ditt lagringskonto om den inte redan finns. När du väljer ett namn för din behållare, Tänk också på hello namngivningsregler som nämns ovan.

```objc
-(void)createContainer{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"newcontainer"];

    // Create container in your Storage account if hello container doesn't already exist
    [blobContainer createContainerIfNotExistsWithCompletionHandler:^(NSError *error, BOOL exists) {
        if (error){
            NSLog(@"Error in creating container.");
        }
    }];
}
```

Du kan bekräfta att det fungerar genom att titta på hello [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com) och verifiera att *newcontainer* i hello lista över behållare för ditt lagringskonto.

## <a name="set-container-permissions"></a>Ange behörigheter för behållaren
En behållare behörigheter har konfigurerats för **privata** åtkomst som standard. Behållare ger dock några olika alternativ för behållaren åtkomst:

* **Privata**: behållare och blob-data kan läsas av hello Kontoägare.
* **BLOB**: Blob-data i den här behållaren kan läsas via anonym begäran, men behållardata är inte tillgänglig. Klienter kan inte räkna upp blobbar i behållaren hello via anonym begäran.
* **Behållaren**: behållare och blob-data kan läsas via anonym begäran. Klienter kan räkna upp blobbar i behållaren hello via anonym begäran, men det går inte att räkna upp behållare i hello storage-konto.

hello följande exempel visas hur toocreate en behållare med **behållare** åtkomstbehörigheter som kommer att tillåta offentlig, skrivskyddad åtkomst för alla användare på hello Internet:

```objc
-(void)createContainerWithPublicAccess{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    // Create container in your Storage account if hello container doesn't already exist
    [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists){
        if (error){
            NSLog(@"Error in creating container.");
        }
    }];
}
```

## <a name="upload-a-blob-into-a-container"></a>Ladda upp en blobb till en behållare
Som anges i hello [Blob-Tjänstkoncept](#blob-service-concepts) avsnittet Blob Storage erbjuder tre olika typer av blobbar: blockblobbar, tilläggsblobbar och sidblobbar. hello Azure Storage iOS bibliotek stöder alla tre typer av blobbar. I de flesta fall är blockblob hello rekommenderade typen toouse.

hello som följande exempel visar hur tooupload ett block blob från en NSString. Om en blob med samma namn finns redan i den här behållaren hello skrivs hello innehållet i det här blob över.

```objc
-(void)uploadBlobToContainer{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists)
        {
            if (error){
                NSLog(@"Error in creating container.");
            }
            else{
                // Create a local blob object
                AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

                // Upload blob tooStorage
                [blockBlob uploadFromText:@"This text will be uploaded tooBlob Storage." completionHandler:^(NSError *error) {
                    if (error){
                        NSLog(@"Error in creating blob.");
                    }
                }];
            }
        }];
}
```

Du kan bekräfta att det fungerar genom att titta på hello [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com) och verifiera hello behållaren, *containerpublic*, innehåller hello blob *sampleblob*. I det här exemplet använde vi en offentlig behållare, så du kan också kontrollera att programmet fungerar genom att gå toohello blobbar URI:

    https://nameofyourstorageaccount.blob.core.windows.net/containerpublic/sampleblob

Dessutom toouploading en blockblobb från en NSString liknande metoder finns för NSData, NSInputStream eller en lokal fil.

## <a name="list-hello-blobs-in-a-container"></a>Lista hello blobbar i en behållare
hello följande exempel visas hur toolist alla blobbar i en behållare. När du utför den här åtgärden måste du vara uppmärksam på hello följande parametrar:     

* **continuationToken** -hello fortsättning token representerar var hello lista åtgärden ska börja. Om ingen token anges, den visar en lista över blobbar från hello början. Valfritt antal blobbar kan visas från noll in tooa ange maximalt. Även om den här metoden returnerar resultat, om `results.continuationToken` inte är noll, det kan finnas fler blobbar på hello-tjänsten som inte finns.
* **prefixet** -du kan ange hello prefixet toouse blob-lista. Blobbar som börjar med prefixet visas.
* **useFlatBlobListing** – som anges i hello [namnge och referera till behållare och blobbar](#naming-and-referencing-containers-and-blobs) avsnitt, även om hello Blob-tjänsten är en platt lagring-schemat, du kan skapa en virtuell hierarki genom att namnge blobbar med sökvägen information. Dock stöds inte platt lista för närvarande inte. Den här funktionen kommer snart. Nu är det här värdet ska vara **Ja**.
* **blobListingDetails** -du kan ange vilka objekt tooinclude när du visar en lista över blobbar
  * _AZSBlobListingDetailsNone_: Visa endast allokerad blobbar och inte returnerar blobbmetadata.
  * _AZSBlobListingDetailsSnapshots_: Visa spara blobbar och blob ögonblicksbilder.
  * _AZSBlobListingDetailsMetadata_: blob att hämta metadata för varje blob som returneras i hello lista.
  * _AZSBlobListingDetailsUncommittedBlobs_: Visa bekräftade och ogenomförda blobbar.
  * _AZSBlobListingDetailsCopy_: kopia egenskaper i hello lista.
  * _AZSBlobListingDetailsAll_: visa en lista över alla tillgängliga allokerat blobbar ogenomförda blobbar och ögonblicksbilder och returnera alla metadata och kopiera status för de blobbar.
* **maxResults** -hello maximalt antal resultat tooreturn för den här åtgärden. Använd-1 toonot ange en gräns.
* **completionHandler** -hello block med koden tooexecute med hello resultat hello lista igen.

I det här exemplet är en hjälpmetod används toorecursively anrop hello listan blobbar metod varje gång en fortsättningstoken returneras.

```objc
-(void)listBlobsInContainer{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    //List all blobs in container
    [self listBlobsInContainerHelper:blobContainer continuationToken:nil prefix:nil blobListingDetails:AZSBlobListingDetailsAll maxResults:-1 completionHandler:^(NSError *error) {
        if (error != nil){
            NSLog(@"Error in creating container.");
        }
    }];
}

//List blobs helper method
-(void)listBlobsInContainerHelper:(AZSCloudBlobContainer *)container continuationToken:(AZSContinuationToken *)continuationToken prefix:(NSString *)prefix blobListingDetails:(AZSBlobListingDetails)blobListingDetails maxResults:(NSUInteger)maxResults completionHandler:(void (^)(NSError *))completionHandler
{
    [container listBlobsSegmentedWithContinuationToken:continuationToken prefix:prefix useFlatBlobListing:YES blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:^(NSError *error, AZSBlobResultSegment *results) {
        if (error)
        {
            completionHandler(error);
        }
        else
        {
            for (int i = 0; i < results.blobs.count; i++) {
                NSLog(@"%@",[(AZSCloudBlockBlob *)results.blobs[i] blobName]);
            }
            if (results.continuationToken)
            {
                [self listBlobsInContainerHelper:container continuationToken:results.continuationToken prefix:prefix blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:completionHandler];
            }
            else
            {
                completionHandler(nil);
            }
        }
    }];
}
```

## <a name="download-a-blob"></a>Ladda ned en blob
följande exempel visar hur hello toodownload en blob tooa NSString-objektet.

```objc
-(void)downloadBlobToString{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    // Create a local blob object
    AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

    // Download blob
    [blockBlob downloadToTextWithCompletionHandler:^(NSError *error, NSString *text) {
        if (error) {
            NSLog(@"Error in downloading blob");
        }
        else{
            NSLog(@"%@",text);
        }
    }];
}
```

## <a name="delete-a-blob"></a>Ta bort en blob
följande exempel visar hur hello toodelete en blob.

```objc
-(void)deleteBlob{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    // Create a local blob object
    AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob1"];

    // Delete blob
    [blockBlob deleteWithCompletionHandler:^(NSError *error) {
        if (error) {
            NSLog(@"Error in deleting blob.");
        }
    }];
}
```

## <a name="delete-a-blob-container"></a>Ta bort en blob-behållare
följande exempel visar hur hello toodelete en behållare.

```objc
-(void)deleteContainer{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    // Delete container
    [blobContainer deleteContainerIfExistsWithCompletionHandler:^(NSError *error, BOOL success) {
        if(error){
            NSLog(@"Error in deleting container");
        }
    }];
}
```

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig hur toouse Blob Storage från iOS, följa dessa länkar toolearn mer om hello iOS-biblioteket och hello Storage-tjänst.

* [Azure Storage-klientbibliotek för iOS](https://github.com/azure/azure-storage-ios)
* [Azure Storage iOS referensdokumentationen](http://azure.github.io/azure-storage-ios/)
* [REST-API för Azure Storage Services](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Azure Storage Teamblogg](http://blogs.msdn.com/b/windowsazurestorage)

Om du har frågor om det här biblioteket känna sig fria toopost tooour [MSDN Azure-forumet](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=windowsazuredata) eller [Stack Overflow](http://stackoverflow.com/questions/tagged/windows-azure-storage+or+windows-azure-storage+or+azure-storage-blobs+or+azure-storage-tables+or+azure-table-storage+or+windows-azure-queues+or+azure-storage-queues+or+azure-storage-emulator+or+azure-storage-files).
Om du har förslag på funktioner för Azure Storage efter du för[Azure Storage Feedback](https://feedback.azure.com/forums/217298-storage/).


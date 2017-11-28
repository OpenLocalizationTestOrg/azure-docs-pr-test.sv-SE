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
ms.openlocfilehash: 474c4263a4bfbd61bfa39e4fdb01ddd9c3829c77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blob-storage-from-ios"></a><span data-ttu-id="6190f-103">Hur toouse Blob storage från iOS</span><span class="sxs-lookup"><span data-stu-id="6190f-103">How toouse Blob storage from iOS</span></span>
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="6190f-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="6190f-104">Overview</span></span>
<span data-ttu-id="6190f-105">Den här artikeln visar hur tooperform vanliga scenarier med hjälp av Microsoft Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="6190f-105">This article will show you how tooperform common scenarios using Microsoft Azure Blob storage.</span></span> <span data-ttu-id="6190f-106">hello exemplen är skrivna i Objective-C och använder hello [Azure Storage-klientbibliotek för iOS](https://github.com/Azure/azure-storage-ios).</span><span class="sxs-lookup"><span data-stu-id="6190f-106">hello samples are written in Objective-C and use hello [Azure Storage Client Library for iOS](https://github.com/Azure/azure-storage-ios).</span></span> <span data-ttu-id="6190f-107">hello beskrivs scenarier där **överför**, **lista**, **hämtar**, och **bort** blobbar.</span><span class="sxs-lookup"><span data-stu-id="6190f-107">hello scenarios covered include **uploading**, **listing**, **downloading**, and **deleting** blobs.</span></span> <span data-ttu-id="6190f-108">Mer information om BLOB finns hello [nästa steg](#next-steps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="6190f-108">For more information on blobs, see hello [Next Steps](#next-steps) section.</span></span> <span data-ttu-id="6190f-109">Du kan också hämta hello [exempelapp](https://github.com/Azure/azure-storage-ios/tree/master/BlobSample) tooquickly finns hello användningen av Azure Storage i ett iOS-program.</span><span class="sxs-lookup"><span data-stu-id="6190f-109">You can also download hello [sample app](https://github.com/Azure/azure-storage-ios/tree/master/BlobSample) tooquickly see hello use of Azure Storage in an iOS application.</span></span>

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="import-hello-azure-storage-ios-library-into-your-application"></a><span data-ttu-id="6190f-110">Importera hello Azure Storage iOS biblioteket i ditt program</span><span class="sxs-lookup"><span data-stu-id="6190f-110">Import hello Azure Storage iOS library into your application</span></span>
<span data-ttu-id="6190f-111">Du kan importera hello Azure Storage iOS biblioteket till ditt program genom att använda hello [Azure Storage CocoaPod](https://cocoapods.org/pods/AZSClient) eller genom att importera hello **Framework** fil.</span><span class="sxs-lookup"><span data-stu-id="6190f-111">You can import hello Azure Storage iOS library into your application either by using hello [Azure Storage CocoaPod](https://cocoapods.org/pods/AZSClient) or by importing hello **Framework** file.</span></span> <span data-ttu-id="6190f-112">CocoaPod är hello rekommenderade sätt som gör det integrerande hello biblioteket enklare, men importera från hello framework-filen är mindre påverkan för ditt befintliga projekt.</span><span class="sxs-lookup"><span data-stu-id="6190f-112">CocoaPod is hello recommended way as it makes integrating hello library easier, however importing from hello framework file is less intrusive for your existing project.</span></span>

<span data-ttu-id="6190f-113">toouse det här biblioteket måste hello följande:</span><span class="sxs-lookup"><span data-stu-id="6190f-113">toouse this library, you need hello following:</span></span>
- <span data-ttu-id="6190f-114">iOS 8 +</span><span class="sxs-lookup"><span data-stu-id="6190f-114">iOS 8+</span></span>
- <span data-ttu-id="6190f-115">Xcode 7 +</span><span class="sxs-lookup"><span data-stu-id="6190f-115">Xcode 7+</span></span>

## <a name="cocoapod"></a><span data-ttu-id="6190f-116">CocoaPod</span><span class="sxs-lookup"><span data-stu-id="6190f-116">CocoaPod</span></span>
1. <span data-ttu-id="6190f-117">Om du inte redan gjort det, [installera CocoaPods](https://guides.cocoapods.org/using/getting-started.html#toc_3) på datorn genom att öppna ett terminalfönster och kör följande kommando hello</span><span class="sxs-lookup"><span data-stu-id="6190f-117">If you haven't done so already, [Install CocoaPods](https://guides.cocoapods.org/using/getting-started.html#toc_3) on your computer by opening a terminal window and running hello following command</span></span>
    
    ```shell   
    sudo gem install cocoapods
    ```

2. <span data-ttu-id="6190f-118">Skapa sedan en ny fil med namnet i hello projektkatalogen (hello katalog som innehåller filen .xcodeproj) _Podfile_(utan filtillägget).</span><span class="sxs-lookup"><span data-stu-id="6190f-118">Next, in hello project directory (hello directory containing your .xcodeproj file), create a new file called _Podfile_(no file extension).</span></span> <span data-ttu-id="6190f-119">Lägg till följande too_Podfile_ hello och spara.</span><span class="sxs-lookup"><span data-stu-id="6190f-119">Add hello following too_Podfile_ and save.</span></span>

    ```ruby
    platform :ios, '8.0'

    target 'TargetName' do
      pod 'AZSClient'
    end
    ```

3. <span data-ttu-id="6190f-120">Navigera i hello terminalfönster, toohello projektkatalogen och kör hello följande kommando</span><span class="sxs-lookup"><span data-stu-id="6190f-120">In hello terminal window, navigate toohello project directory and run hello following command</span></span>

    ```shell    
    pod install
    ```

4. <span data-ttu-id="6190f-121">Stäng om din .xcodeproj är öppen i Xcode.</span><span class="sxs-lookup"><span data-stu-id="6190f-121">If your .xcodeproj is open in Xcode, close it.</span></span> <span data-ttu-id="6190f-122">I projektet directory öppna hello nyskapade projekt som kommer att ha hello .xcworkspace tillägg.</span><span class="sxs-lookup"><span data-stu-id="6190f-122">In your project directory open hello newly created project file which will have hello .xcworkspace extension.</span></span> <span data-ttu-id="6190f-123">Detta är hello-fil som du ska arbeta från för nu på.</span><span class="sxs-lookup"><span data-stu-id="6190f-123">This is hello file you'll work from for now on.</span></span>

## <a name="framework"></a><span data-ttu-id="6190f-124">Framework</span><span class="sxs-lookup"><span data-stu-id="6190f-124">Framework</span></span>
<span data-ttu-id="6190f-125">hello andra sätt toouse hello biblioteket är toobuild hello framework manuellt:</span><span class="sxs-lookup"><span data-stu-id="6190f-125">hello other way toouse hello library is toobuild hello framework manually:</span></span>

1. <span data-ttu-id="6190f-126">Första, hämta eller klona hello [lagringsplatsen för azure-lagring – ios](https://github.com/azure/azure-storage-ios).</span><span class="sxs-lookup"><span data-stu-id="6190f-126">First, download or clone hello [azure-storage-ios repo](https://github.com/azure/azure-storage-ios).</span></span>
2. <span data-ttu-id="6190f-127">Gå till *azure-lagring – ios* -> *Lib* -> *Azure Storage-klientbibliotek*, och öppna `AZSClient.xcodeproj` i Xcode.</span><span class="sxs-lookup"><span data-stu-id="6190f-127">Go into *azure-storage-ios* -> *Lib* -> *Azure Storage Client Library*, and open `AZSClient.xcodeproj` in Xcode.</span></span>
3. <span data-ttu-id="6190f-128">Vid hello övre vänstra xcode, ändra hello active schemat från ”Azure Storage-klientbibliotek” för ”Framework”.</span><span class="sxs-lookup"><span data-stu-id="6190f-128">At hello top-left of Xcode, change hello active scheme from "Azure Storage Client Library" too"Framework".</span></span>
4. <span data-ttu-id="6190f-129">Skapa hello projekt (⌘ + B).</span><span class="sxs-lookup"><span data-stu-id="6190f-129">Build hello project (⌘+B).</span></span> <span data-ttu-id="6190f-130">Detta skapar en `AZSClient.framework` filen på skrivbordet.</span><span class="sxs-lookup"><span data-stu-id="6190f-130">This will create an `AZSClient.framework` file on your Desktop.</span></span>

<span data-ttu-id="6190f-131">Du kan sedan importera hello framework filen till programmet hello följande:</span><span class="sxs-lookup"><span data-stu-id="6190f-131">You can then import hello framework file into your application by doing hello following:</span></span>

1. <span data-ttu-id="6190f-132">Skapa ett nytt projekt eller öppna ett befintligt projekt i Xcode.</span><span class="sxs-lookup"><span data-stu-id="6190f-132">Create a new project or open up your existing project in Xcode.</span></span>
2. <span data-ttu-id="6190f-133">Dra och släpp hello `AZSClient.framework` i projektnavigeringen Xcode.</span><span class="sxs-lookup"><span data-stu-id="6190f-133">Drag and drop hello `AZSClient.framework` into your Xcode project navigator.</span></span>
3. <span data-ttu-id="6190f-134">Välj *kopiera objekt vid behov*, och klicka på *Slutför*.</span><span class="sxs-lookup"><span data-stu-id="6190f-134">Select *Copy items if needed*, and click on *Finish*.</span></span>
4. <span data-ttu-id="6190f-135">Klicka på ditt projekt i hello vänstra navigeringsfönstret och klickar på hello *allmänna* fliken hello överst i hello projektet editor.</span><span class="sxs-lookup"><span data-stu-id="6190f-135">Click on your project in hello left-hand navigation and click hello *General* tab at hello top of hello project editor.</span></span>
5. <span data-ttu-id="6190f-136">Under hello *länkade ramverk och bibliotek* klickar du på knappen Lägg till för hello (+).</span><span class="sxs-lookup"><span data-stu-id="6190f-136">Under hello *Linked Frameworks and Libraries* section, click hello Add button (+).</span></span>
6. <span data-ttu-id="6190f-137">Sök efter i hello listan över bibliotek som redan tillhandahålls `libxml2.2.tbd` och lägga till den tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="6190f-137">In hello list of libraries already provided, search for `libxml2.2.tbd` and add it tooyour project.</span></span>

## <a name="import-hello-library"></a><span data-ttu-id="6190f-138">Importera hello bibliotek</span><span class="sxs-lookup"><span data-stu-id="6190f-138">Import hello Library</span></span> 
```objc
// Include hello following import statement toouse blob APIs.
#import <AZSClient/AZSClient.h>
```

<span data-ttu-id="6190f-139">Om du använder Swift kommer du behöver toocreate datacenterbryggning rubrik och importera < AZSClient/AZSClient.h > det:</span><span class="sxs-lookup"><span data-stu-id="6190f-139">If you are using Swift, you will need toocreate a bridging header and import <AZSClient/AZSClient.h> there:</span></span>

1. <span data-ttu-id="6190f-140">Skapa en huvudfilen `Bridging-Header.h`, och Lägg till hello ovan import-sats.</span><span class="sxs-lookup"><span data-stu-id="6190f-140">Create a header file `Bridging-Header.h`, and add hello above import statement.</span></span>
2. <span data-ttu-id="6190f-141">Gå toohello *Versionsinställningar* fliken och Sök efter *Interimshuvudfilen med Objective-C*.</span><span class="sxs-lookup"><span data-stu-id="6190f-141">Go toohello *Build Settings* tab, and search for *Objective-C Bridging Header*.</span></span>
3. <span data-ttu-id="6190f-142">Dubbelklicka på hello-fältet för *Interimshuvudfilen med Objective-C* och Lägg till huvudfilen för hello sökvägen tooyour:`ProjectName/Bridging-Header.h`</span><span class="sxs-lookup"><span data-stu-id="6190f-142">Double-click on hello field of *Objective-C Bridging Header* and add hello path tooyour header file: `ProjectName/Bridging-Header.h`</span></span>
4. <span data-ttu-id="6190f-143">Build hello projekt (⌘ + B) tooverify som hello datacenterbryggning huvud hämtades av Xcode.</span><span class="sxs-lookup"><span data-stu-id="6190f-143">Build hello project (⌘+B) tooverify that hello bridging header was picked up by Xcode.</span></span>
5. <span data-ttu-id="6190f-144">Börja använda hello biblioteket direkt i Swift-filen, om det behövs ingen importuttryck.</span><span class="sxs-lookup"><span data-stu-id="6190f-144">Start using hello library directly in any Swift file, there is no need for import statements.</span></span>

[!INCLUDE [storage-mobile-authentication-guidance](../../includes/storage-mobile-authentication-guidance.md)]

## <a name="asynchronous-operations"></a><span data-ttu-id="6190f-145">Asynkrona åtgärder</span><span class="sxs-lookup"><span data-stu-id="6190f-145">Asynchronous Operations</span></span>
> [!NOTE]
> <span data-ttu-id="6190f-146">Alla metoder som utför en begäran mot hello-tjänsten är asynkrona åtgärder.</span><span class="sxs-lookup"><span data-stu-id="6190f-146">All methods that perform a request against hello service are asynchronous operations.</span></span> <span data-ttu-id="6190f-147">Du hittar att metoderna har en hanterare för slutförande i hello-kodexempel.</span><span class="sxs-lookup"><span data-stu-id="6190f-147">In hello code samples, you'll find that these methods have a completion handler.</span></span> <span data-ttu-id="6190f-148">Kod inuti hello slutförande hanterare körs **när** hello begäran har slutförts.</span><span class="sxs-lookup"><span data-stu-id="6190f-148">Code inside hello completion handler will run **after** hello request is completed.</span></span> <span data-ttu-id="6190f-149">Code när hello slutförande hanteraren kommer att köras **medan** hello begäran görs.</span><span class="sxs-lookup"><span data-stu-id="6190f-149">Code after hello completion handler will run **while** hello request is being made.</span></span>
> 
> 

## <a name="create-a-container"></a><span data-ttu-id="6190f-150">Skapa en behållare</span><span class="sxs-lookup"><span data-stu-id="6190f-150">Create a container</span></span>
<span data-ttu-id="6190f-151">Varje blobb i Azure Storage måste finnas i en behållare.</span><span class="sxs-lookup"><span data-stu-id="6190f-151">Every blob in Azure Storage must reside in a container.</span></span> <span data-ttu-id="6190f-152">hello följande exempel visas hur toocreate en behållare kallas *newcontainer*, i ditt lagringskonto om den inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="6190f-152">hello following example shows how toocreate a container, called *newcontainer*, in your Storage account if it doesn't already exist.</span></span> <span data-ttu-id="6190f-153">När du väljer ett namn för din behållare, Tänk också på hello namngivningsregler som nämns ovan.</span><span class="sxs-lookup"><span data-stu-id="6190f-153">When choosing a name for your container, be mindful of hello naming rules mentioned above.</span></span>

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

<span data-ttu-id="6190f-154">Du kan bekräfta att det fungerar genom att titta på hello [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com) och verifiera att *newcontainer* i hello lista över behållare för ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="6190f-154">You can confirm that this works by looking at hello [Microsoft Azure Storage Explorer](http://storageexplorer.com) and verifying that *newcontainer* is in hello list of containers for your Storage account.</span></span>

## <a name="set-container-permissions"></a><span data-ttu-id="6190f-155">Ange behörigheter för behållaren</span><span class="sxs-lookup"><span data-stu-id="6190f-155">Set Container Permissions</span></span>
<span data-ttu-id="6190f-156">En behållare behörigheter har konfigurerats för **privata** åtkomst som standard.</span><span class="sxs-lookup"><span data-stu-id="6190f-156">A container's permissions are configured for **Private** access by default.</span></span> <span data-ttu-id="6190f-157">Behållare ger dock några olika alternativ för behållaren åtkomst:</span><span class="sxs-lookup"><span data-stu-id="6190f-157">However, containers provide a few different options for container access:</span></span>

* <span data-ttu-id="6190f-158">**Privata**: behållare och blob-data kan läsas av hello Kontoägare.</span><span class="sxs-lookup"><span data-stu-id="6190f-158">**Private**: Container and blob data can be read by hello account owner only.</span></span>
* <span data-ttu-id="6190f-159">**BLOB**: Blob-data i den här behållaren kan läsas via anonym begäran, men behållardata är inte tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="6190f-159">**Blob**: Blob data within this container can be read via anonymous request, but container data is not available.</span></span> <span data-ttu-id="6190f-160">Klienter kan inte räkna upp blobbar i behållaren hello via anonym begäran.</span><span class="sxs-lookup"><span data-stu-id="6190f-160">Clients cannot enumerate blobs within hello container via anonymous request.</span></span>
* <span data-ttu-id="6190f-161">**Behållaren**: behållare och blob-data kan läsas via anonym begäran.</span><span class="sxs-lookup"><span data-stu-id="6190f-161">**Container**: Container and blob data can be read via anonymous request.</span></span> <span data-ttu-id="6190f-162">Klienter kan räkna upp blobbar i behållaren hello via anonym begäran, men det går inte att räkna upp behållare i hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="6190f-162">Clients can enumerate blobs within hello container via anonymous request, but cannot enumerate containers within hello storage account.</span></span>

<span data-ttu-id="6190f-163">hello följande exempel visas hur toocreate en behållare med **behållare** åtkomstbehörigheter som kommer att tillåta offentlig, skrivskyddad åtkomst för alla användare på hello Internet:</span><span class="sxs-lookup"><span data-stu-id="6190f-163">hello following example shows you how toocreate a container with **Container** access permissions, which will allow public, read-only access for all users on hello Internet:</span></span>

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

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="6190f-164">Ladda upp en blobb till en behållare</span><span class="sxs-lookup"><span data-stu-id="6190f-164">Upload a blob into a container</span></span>
<span data-ttu-id="6190f-165">Som anges i hello [Blob-Tjänstkoncept](#blob-service-concepts) avsnittet Blob Storage erbjuder tre olika typer av blobbar: blockblobbar, tilläggsblobbar och sidblobbar.</span><span class="sxs-lookup"><span data-stu-id="6190f-165">As mentioned in hello [Blob service concepts](#blob-service-concepts) section, Blob Storage offers three different types of blobs: block blobs, append blobs, and page blobs.</span></span> <span data-ttu-id="6190f-166">hello Azure Storage iOS bibliotek stöder alla tre typer av blobbar.</span><span class="sxs-lookup"><span data-stu-id="6190f-166">hello Azure Storage iOS library supports all three types of blobs.</span></span> <span data-ttu-id="6190f-167">I de flesta fall är blockblob hello rekommenderade typen toouse.</span><span class="sxs-lookup"><span data-stu-id="6190f-167">In most cases, block blob is hello recommended type toouse.</span></span>

<span data-ttu-id="6190f-168">hello som följande exempel visar hur tooupload ett block blob från en NSString.</span><span class="sxs-lookup"><span data-stu-id="6190f-168">hello following example shows how tooupload a block blob from an NSString.</span></span> <span data-ttu-id="6190f-169">Om en blob med samma namn finns redan i den här behållaren hello skrivs hello innehållet i det här blob över.</span><span class="sxs-lookup"><span data-stu-id="6190f-169">If a blob with hello same name already exists in this container, hello contents of this blob will be overwritten.</span></span>

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

<span data-ttu-id="6190f-170">Du kan bekräfta att det fungerar genom att titta på hello [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com) och verifiera hello behållaren, *containerpublic*, innehåller hello blob *sampleblob*.</span><span class="sxs-lookup"><span data-stu-id="6190f-170">You can confirm that this works by looking at hello [Microsoft Azure Storage Explorer](http://storageexplorer.com) and verifying that hello container, *containerpublic*, contains hello blob, *sampleblob*.</span></span> <span data-ttu-id="6190f-171">I det här exemplet använde vi en offentlig behållare, så du kan också kontrollera att programmet fungerar genom att gå toohello blobbar URI:</span><span class="sxs-lookup"><span data-stu-id="6190f-171">In this sample, we used a public container so you can also verify that this application worked by going toohello blobs URI:</span></span>

    https://nameofyourstorageaccount.blob.core.windows.net/containerpublic/sampleblob

<span data-ttu-id="6190f-172">Dessutom toouploading en blockblobb från en NSString liknande metoder finns för NSData, NSInputStream eller en lokal fil.</span><span class="sxs-lookup"><span data-stu-id="6190f-172">In addition toouploading a block blob from an NSString, similar methods exist for NSData, NSInputStream, or a local file.</span></span>

## <a name="list-hello-blobs-in-a-container"></a><span data-ttu-id="6190f-173">Lista hello blobbar i en behållare</span><span class="sxs-lookup"><span data-stu-id="6190f-173">List hello blobs in a container</span></span>
<span data-ttu-id="6190f-174">hello följande exempel visas hur toolist alla blobbar i en behållare.</span><span class="sxs-lookup"><span data-stu-id="6190f-174">hello following example shows how toolist all blobs in a container.</span></span> <span data-ttu-id="6190f-175">När du utför den här åtgärden måste du vara uppmärksam på hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="6190f-175">When performing this operation, be mindful of hello following parameters:</span></span>     

* <span data-ttu-id="6190f-176">**continuationToken** -hello fortsättning token representerar var hello lista åtgärden ska börja.</span><span class="sxs-lookup"><span data-stu-id="6190f-176">**continuationToken** - hello continuation token represents where hello listing operation should start.</span></span> <span data-ttu-id="6190f-177">Om ingen token anges, den visar en lista över blobbar från hello början.</span><span class="sxs-lookup"><span data-stu-id="6190f-177">If no token is provided, it will list blobs from hello beginning.</span></span> <span data-ttu-id="6190f-178">Valfritt antal blobbar kan visas från noll in tooa ange maximalt.</span><span class="sxs-lookup"><span data-stu-id="6190f-178">Any number of blobs can be listed, from zero up tooa set maximum.</span></span> <span data-ttu-id="6190f-179">Även om den här metoden returnerar resultat, om `results.continuationToken` inte är noll, det kan finnas fler blobbar på hello-tjänsten som inte finns.</span><span class="sxs-lookup"><span data-stu-id="6190f-179">Even if this method returns zero results, if `results.continuationToken` is not nil, there may be more blobs on hello service that have not been listed.</span></span>
* <span data-ttu-id="6190f-180">**prefixet** -du kan ange hello prefixet toouse blob-lista.</span><span class="sxs-lookup"><span data-stu-id="6190f-180">**prefix** - You can specify hello prefix toouse for blob listing.</span></span> <span data-ttu-id="6190f-181">Blobbar som börjar med prefixet visas.</span><span class="sxs-lookup"><span data-stu-id="6190f-181">Only blobs that begin with this prefix will be listed.</span></span>
* <span data-ttu-id="6190f-182">**useFlatBlobListing** – som anges i hello [namnge och referera till behållare och blobbar](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata) avsnitt, även om hello Blob-tjänsten är en platt lagring-schemat, du kan skapa en virtuell hierarki genom att namnge blobbar med sökvägen information.</span><span class="sxs-lookup"><span data-stu-id="6190f-182">**useFlatBlobListing** - As mentioned in hello [Naming and referencing containers and blobs](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata) section, although hello Blob service is a flat storage scheme, you can create a virtual hierarchy by naming blobs with path information.</span></span> <span data-ttu-id="6190f-183">Dock stöds inte platt lista för närvarande inte.</span><span class="sxs-lookup"><span data-stu-id="6190f-183">However, non-flat listing is currently not supported.</span></span> <span data-ttu-id="6190f-184">Den här funktionen kommer snart.</span><span class="sxs-lookup"><span data-stu-id="6190f-184">This feature is coming soon.</span></span> <span data-ttu-id="6190f-185">Nu är det här värdet ska vara **Ja**.</span><span class="sxs-lookup"><span data-stu-id="6190f-185">For now, this value should be **YES**.</span></span>

* <span data-ttu-id="6190f-186">**blobListingDetails** -du kan ange vilka objekt tooinclude när du visar en lista över blobbar</span><span class="sxs-lookup"><span data-stu-id="6190f-186">**blobListingDetails** - You can specify which items tooinclude when listing blobs</span></span>
  * <span data-ttu-id="6190f-187">_AZSBlobListingDetailsNone_: Visa endast allokerad blobbar och inte returnerar blobbmetadata.</span><span class="sxs-lookup"><span data-stu-id="6190f-187">_AZSBlobListingDetailsNone_: List only committed blobs, and do not return blob metadata.</span></span>
  * <span data-ttu-id="6190f-188">_AZSBlobListingDetailsSnapshots_: Visa spara blobbar och blob ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="6190f-188">_AZSBlobListingDetailsSnapshots_: List committed blobs and blob snapshots.</span></span>
  * <span data-ttu-id="6190f-189">_AZSBlobListingDetailsMetadata_: blob att hämta metadata för varje blob som returneras i hello lista.</span><span class="sxs-lookup"><span data-stu-id="6190f-189">_AZSBlobListingDetailsMetadata_: Retrieve blob metadata for each blob returned in hello listing.</span></span>
  * <span data-ttu-id="6190f-190">_AZSBlobListingDetailsUncommittedBlobs_: Visa bekräftade och ogenomförda blobbar.</span><span class="sxs-lookup"><span data-stu-id="6190f-190">_AZSBlobListingDetailsUncommittedBlobs_: List committed and uncommitted blobs.</span></span>
  * <span data-ttu-id="6190f-191">_AZSBlobListingDetailsCopy_: kopia egenskaper i hello lista.</span><span class="sxs-lookup"><span data-stu-id="6190f-191">_AZSBlobListingDetailsCopy_: Include copy properties in hello listing.</span></span>
  * <span data-ttu-id="6190f-192">_AZSBlobListingDetailsAll_: visa en lista över alla tillgängliga allokerat blobbar ogenomförda blobbar och ögonblicksbilder och returnera alla metadata och kopiera status för de blobbar.</span><span class="sxs-lookup"><span data-stu-id="6190f-192">_AZSBlobListingDetailsAll_: List all available committed blobs, uncommitted blobs, and snapshots, and return all metadata and copy status for those blobs.</span></span>
* <span data-ttu-id="6190f-193">**maxResults** -hello maximalt antal resultat tooreturn för den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="6190f-193">**maxResults** - hello maximum number of results tooreturn for this operation.</span></span> <span data-ttu-id="6190f-194">Använd-1 toonot ange en gräns.</span><span class="sxs-lookup"><span data-stu-id="6190f-194">Use -1 toonot set a limit.</span></span>
* <span data-ttu-id="6190f-195">**completionHandler** -hello block med koden tooexecute med hello resultat hello lista igen.</span><span class="sxs-lookup"><span data-stu-id="6190f-195">**completionHandler** - hello block of code tooexecute with hello results of hello listing operation.</span></span>

<span data-ttu-id="6190f-196">I det här exemplet är en hjälpmetod används toorecursively anrop hello listan blobbar metod varje gång en fortsättningstoken returneras.</span><span class="sxs-lookup"><span data-stu-id="6190f-196">In this example, a helper method is used toorecursively call hello list blobs method every time a continuation token is returned.</span></span>

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

## <a name="download-a-blob"></a><span data-ttu-id="6190f-197">Ladda ned en blob</span><span class="sxs-lookup"><span data-stu-id="6190f-197">Download a blob</span></span>
<span data-ttu-id="6190f-198">följande exempel visar hur hello toodownload en blob tooa NSString-objektet.</span><span class="sxs-lookup"><span data-stu-id="6190f-198">hello following example shows how toodownload a blob tooa NSString object.</span></span>

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

## <a name="delete-a-blob"></a><span data-ttu-id="6190f-199">Ta bort en blob</span><span class="sxs-lookup"><span data-stu-id="6190f-199">Delete a blob</span></span>
<span data-ttu-id="6190f-200">följande exempel visar hur hello toodelete en blob.</span><span class="sxs-lookup"><span data-stu-id="6190f-200">hello following example shows how toodelete a blob.</span></span>

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

## <a name="delete-a-blob-container"></a><span data-ttu-id="6190f-201">Ta bort en blob-behållare</span><span class="sxs-lookup"><span data-stu-id="6190f-201">Delete a blob container</span></span>
<span data-ttu-id="6190f-202">följande exempel visar hur hello toodelete en behållare.</span><span class="sxs-lookup"><span data-stu-id="6190f-202">hello following example shows how toodelete a container.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="6190f-203">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6190f-203">Next steps</span></span>
<span data-ttu-id="6190f-204">Nu när du har lärt dig hur toouse Blob Storage från iOS, följa dessa länkar toolearn mer om hello iOS-biblioteket och hello Storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="6190f-204">Now that you've learned how toouse Blob Storage from iOS, follow these links toolearn more about hello iOS library and hello Storage service.</span></span>

* [<span data-ttu-id="6190f-205">Azure Storage-klientbibliotek för iOS</span><span class="sxs-lookup"><span data-stu-id="6190f-205">Azure Storage Client Library for iOS</span></span>](https://github.com/azure/azure-storage-ios)
* [<span data-ttu-id="6190f-206">Azure Storage iOS referensdokumentationen</span><span class="sxs-lookup"><span data-stu-id="6190f-206">Azure Storage iOS Reference Documentation</span></span>](http://azure.github.io/azure-storage-ios/)
* [<span data-ttu-id="6190f-207">REST-API för Azure Storage Services</span><span class="sxs-lookup"><span data-stu-id="6190f-207">Azure Storage Services REST API</span></span>](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [<span data-ttu-id="6190f-208">Azure Storage Teamblogg</span><span class="sxs-lookup"><span data-stu-id="6190f-208">Azure Storage Team Blog</span></span>](http://blogs.msdn.com/b/windowsazurestorage)

<span data-ttu-id="6190f-209">Om du har frågor om det här biblioteket känna sig fria toopost tooour [MSDN Azure-forumet](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=windowsazuredata) eller [Stack Overflow](http://stackoverflow.com/questions/tagged/windows-azure-storage+or+windows-azure-storage+or+azure-storage-blobs+or+azure-storage-tables+or+azure-table-storage+or+windows-azure-queues+or+azure-storage-queues+or+azure-storage-emulator+or+azure-storage-files).</span><span class="sxs-lookup"><span data-stu-id="6190f-209">If you have questions regarding this library, feel free toopost tooour [MSDN Azure forum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=windowsazuredata) or [Stack Overflow](http://stackoverflow.com/questions/tagged/windows-azure-storage+or+windows-azure-storage+or+azure-storage-blobs+or+azure-storage-tables+or+azure-table-storage+or+windows-azure-queues+or+azure-storage-queues+or+azure-storage-emulator+or+azure-storage-files).</span></span>
<span data-ttu-id="6190f-210">Om du har förslag på funktioner för Azure Storage efter du för[Azure Storage Feedback](https://feedback.azure.com/forums/217298-storage/).</span><span class="sxs-lookup"><span data-stu-id="6190f-210">If you have feature suggestions for Azure Storage, please post too[Azure Storage Feedback](https://feedback.azure.com/forums/217298-storage/).</span></span>


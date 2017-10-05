---
title: "Använda Azure Blob storage från iOS | Microsoft Docs"
description: Lagra ostrukturerade data i molnet med Azure Blob Storage (objektlagring).
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
ms.openlocfilehash: cb2810636c8c23dbd476dc2adf58b17d1887d575
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-blob-storage-from-ios"></a><span data-ttu-id="fdb6d-103">Hur du använder Blob storage från iOS</span><span class="sxs-lookup"><span data-stu-id="fdb6d-103">How to use Blob storage from iOS</span></span>
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="fdb6d-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="fdb6d-104">Overview</span></span>
<span data-ttu-id="fdb6d-105">Den här artikeln visar hur du utför vanliga scenarier med hjälp av Microsoft Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-105">This article will show you how to perform common scenarios using Microsoft Azure Blob storage.</span></span> <span data-ttu-id="fdb6d-106">Exemplen är skrivna i Objective-C och använda den [Azure Storage-klientbibliotek för iOS](https://github.com/Azure/azure-storage-ios).</span><span class="sxs-lookup"><span data-stu-id="fdb6d-106">The samples are written in Objective-C and use the [Azure Storage Client Library for iOS](https://github.com/Azure/azure-storage-ios).</span></span> <span data-ttu-id="fdb6d-107">Scenarier som tas upp inkluderar **överför**, **lista**, **hämtar**, och **bort** blobbar.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-107">The scenarios covered include **uploading**, **listing**, **downloading**, and **deleting** blobs.</span></span> <span data-ttu-id="fdb6d-108">Mer information om blobbar finns i [nästa steg](#next-steps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-108">For more information on blobs, see the [Next Steps](#next-steps) section.</span></span> <span data-ttu-id="fdb6d-109">Du kan också hämta de [exempelapp](https://github.com/Azure/azure-storage-ios/tree/master/BlobSample) snabbt se användningen av Azure Storage i ett iOS-program.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-109">You can also download the [sample app](https://github.com/Azure/azure-storage-ios/tree/master/BlobSample) to quickly see the use of Azure Storage in an iOS application.</span></span>

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="import-the-azure-storage-ios-library-into-your-application"></a><span data-ttu-id="fdb6d-110">Importera Azure Storage iOS biblioteket i ditt program</span><span class="sxs-lookup"><span data-stu-id="fdb6d-110">Import the Azure Storage iOS library into your application</span></span>
<span data-ttu-id="fdb6d-111">Du kan importera iOS-biblioteket för Azure Storage till ditt program genom att använda den [Azure Storage CocoaPod](https://cocoapods.org/pods/AZSClient) eller genom att importera den **Framework** fil.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-111">You can import the Azure Storage iOS library into your application either by using the [Azure Storage CocoaPod](https://cocoapods.org/pods/AZSClient) or by importing the **Framework** file.</span></span> <span data-ttu-id="fdb6d-112">CocoaPod är det rekommenderade sättet som gör det integrera biblioteket enklare, men importera från framework-filen är mindre påverkan för ditt befintliga projekt.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-112">CocoaPod is the recommended way as it makes integrating the library easier, however importing from the framework file is less intrusive for your existing project.</span></span>

<span data-ttu-id="fdb6d-113">Om du vill använda det här biblioteket, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="fdb6d-113">To use this library, you need the following:</span></span>
- <span data-ttu-id="fdb6d-114">iOS 8 +</span><span class="sxs-lookup"><span data-stu-id="fdb6d-114">iOS 8+</span></span>
- <span data-ttu-id="fdb6d-115">Xcode 7 +</span><span class="sxs-lookup"><span data-stu-id="fdb6d-115">Xcode 7+</span></span>

## <a name="cocoapod"></a><span data-ttu-id="fdb6d-116">CocoaPod</span><span class="sxs-lookup"><span data-stu-id="fdb6d-116">CocoaPod</span></span>
1. <span data-ttu-id="fdb6d-117">Om du inte redan gjort det, [installera CocoaPods](https://guides.cocoapods.org/using/getting-started.html#toc_3) på datorn genom att öppna ett terminalfönster och kör följande kommando</span><span class="sxs-lookup"><span data-stu-id="fdb6d-117">If you haven't done so already, [Install CocoaPods](https://guides.cocoapods.org/using/getting-started.html#toc_3) on your computer by opening a terminal window and running the following command</span></span>
    
    ```shell   
    sudo gem install cocoapods
    ```

2. <span data-ttu-id="fdb6d-118">Skapa sedan en ny fil med namnet i projektkatalogen (den katalog som innehåller filen .xcodeproj) _Podfile_(utan filtillägget).</span><span class="sxs-lookup"><span data-stu-id="fdb6d-118">Next, in the project directory (the directory containing your .xcodeproj file), create a new file called _Podfile_(no file extension).</span></span> <span data-ttu-id="fdb6d-119">Lägg till följande till _Podfile_ och spara.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-119">Add the following to _Podfile_ and save.</span></span>

    ```ruby
    platform :ios, '8.0'

    target 'TargetName' do
      pod 'AZSClient'
    end
    ```

3. <span data-ttu-id="fdb6d-120">Navigera till projektkatalogen i fönstret terminal och kör följande kommando</span><span class="sxs-lookup"><span data-stu-id="fdb6d-120">In the terminal window, navigate to the project directory and run the following command</span></span>

    ```shell    
    pod install
    ```

4. <span data-ttu-id="fdb6d-121">Stäng om din .xcodeproj är öppen i Xcode.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-121">If your .xcodeproj is open in Xcode, close it.</span></span> <span data-ttu-id="fdb6d-122">Öppna filen nyligen skapade projekt som har filnamnstillägget .xcworkspace i projektkatalogen.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-122">In your project directory open the newly created project file which will have the .xcworkspace extension.</span></span> <span data-ttu-id="fdb6d-123">Detta är den fil som du ska arbeta från för nu på.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-123">This is the file you'll work from for now on.</span></span>

## <a name="framework"></a><span data-ttu-id="fdb6d-124">Framework</span><span class="sxs-lookup"><span data-stu-id="fdb6d-124">Framework</span></span>
<span data-ttu-id="fdb6d-125">Ett annat sätt att använda biblioteket är att bygga ramen manuellt:</span><span class="sxs-lookup"><span data-stu-id="fdb6d-125">The other way to use the library is to build the framework manually:</span></span>

1. <span data-ttu-id="fdb6d-126">Hämta först eller klona den [lagringsplatsen för azure-lagring – ios](https://github.com/azure/azure-storage-ios).</span><span class="sxs-lookup"><span data-stu-id="fdb6d-126">First, download or clone the [azure-storage-ios repo](https://github.com/azure/azure-storage-ios).</span></span>
2. <span data-ttu-id="fdb6d-127">Gå till *azure-lagring – ios* -> *Lib* -> *Azure Storage-klientbibliotek*, och öppna `AZSClient.xcodeproj` i Xcode.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-127">Go into *azure-storage-ios* -> *Lib* -> *Azure Storage Client Library*, and open `AZSClient.xcodeproj` in Xcode.</span></span>
3. <span data-ttu-id="fdb6d-128">I det övre vänstra xcode, ändra schemat från ”Azure Storage-klientbibliotek” till ”Framework”.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-128">At the top-left of Xcode, change the active scheme from "Azure Storage Client Library" to "Framework".</span></span>
4. <span data-ttu-id="fdb6d-129">Skapa projektet (⌘ + B).</span><span class="sxs-lookup"><span data-stu-id="fdb6d-129">Build the project (⌘+B).</span></span> <span data-ttu-id="fdb6d-130">Detta skapar en `AZSClient.framework` filen på skrivbordet.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-130">This will create an `AZSClient.framework` file on your Desktop.</span></span>

<span data-ttu-id="fdb6d-131">Du kan sedan importera filen framework till ditt program genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="fdb6d-131">You can then import the framework file into your application by doing the following:</span></span>

1. <span data-ttu-id="fdb6d-132">Skapa ett nytt projekt eller öppna ett befintligt projekt i Xcode.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-132">Create a new project or open up your existing project in Xcode.</span></span>
2. <span data-ttu-id="fdb6d-133">Dra och släpp den `AZSClient.framework` i projektnavigeringen Xcode.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-133">Drag and drop the `AZSClient.framework` into your Xcode project navigator.</span></span>
3. <span data-ttu-id="fdb6d-134">Välj *kopiera objekt vid behov*, och klicka på *Slutför*.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-134">Select *Copy items if needed*, and click on *Finish*.</span></span>
4. <span data-ttu-id="fdb6d-135">Klicka på ditt projekt i det vänstra navigeringsfönstret och klicka på den *allmänna* fliken överst i Redigeraren för projektet.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-135">Click on your project in the left-hand navigation and click the *General* tab at the top of the project editor.</span></span>
5. <span data-ttu-id="fdb6d-136">Under den *länkade ramverk och bibliotek* klickar du på knappen Lägg till (+).</span><span class="sxs-lookup"><span data-stu-id="fdb6d-136">Under the *Linked Frameworks and Libraries* section, click the Add button (+).</span></span>
6. <span data-ttu-id="fdb6d-137">I listan över bibliotek som redan tillhandahålls, söker du efter `libxml2.2.tbd` och lägga till den i projektet.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-137">In the list of libraries already provided, search for `libxml2.2.tbd` and add it to your project.</span></span>

## <a name="import-the-library"></a><span data-ttu-id="fdb6d-138">Importera biblioteket</span><span class="sxs-lookup"><span data-stu-id="fdb6d-138">Import the Library</span></span> 
```objc
// Include the following import statement to use blob APIs.
#import <AZSClient/AZSClient.h>
```

<span data-ttu-id="fdb6d-139">Om du använder Swift behöver du skapa en datacenterbryggning rubrik och importera < AZSClient/AZSClient.h > det:</span><span class="sxs-lookup"><span data-stu-id="fdb6d-139">If you are using Swift, you will need to create a bridging header and import <AZSClient/AZSClient.h> there:</span></span>

1. <span data-ttu-id="fdb6d-140">Skapa en huvudfilen `Bridging-Header.h`, och Lägg till instruktionen ovan import.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-140">Create a header file `Bridging-Header.h`, and add the above import statement.</span></span>
2. <span data-ttu-id="fdb6d-141">Gå till den *Versionsinställningar* fliken och Sök efter *Interimshuvudfilen med Objective-C*.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-141">Go to the *Build Settings* tab, and search for *Objective-C Bridging Header*.</span></span>
3. <span data-ttu-id="fdb6d-142">Dubbelklicka på fältet i *Interimshuvudfilen med Objective-C* och Lägg till sökvägen till huvudfil:`ProjectName/Bridging-Header.h`</span><span class="sxs-lookup"><span data-stu-id="fdb6d-142">Double-click on the field of *Objective-C Bridging Header* and add the path to your header file: `ProjectName/Bridging-Header.h`</span></span>
4. <span data-ttu-id="fdb6d-143">Skapa projektet (⌘ + B) om du vill verifiera att rubriken datacenterbryggning hämtades av Xcode.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-143">Build the project (⌘+B) to verify that the bridging header was picked up by Xcode.</span></span>
5. <span data-ttu-id="fdb6d-144">Börja använda biblioteket direkt i Swift-filen, om det behövs ingen importuttryck.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-144">Start using the library directly in any Swift file, there is no need for import statements.</span></span>

[!INCLUDE [storage-mobile-authentication-guidance](../../includes/storage-mobile-authentication-guidance.md)]

## <a name="asynchronous-operations"></a><span data-ttu-id="fdb6d-145">Asynkrona åtgärder</span><span class="sxs-lookup"><span data-stu-id="fdb6d-145">Asynchronous Operations</span></span>
> [!NOTE]
> <span data-ttu-id="fdb6d-146">Alla metoder som utför en begäran mot tjänsten är asynkrona åtgärder.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-146">All methods that perform a request against the service are asynchronous operations.</span></span> <span data-ttu-id="fdb6d-147">I kodexemplen hittar du att dessa metoder har en slutförande-hanterare.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-147">In the code samples, you'll find that these methods have a completion handler.</span></span> <span data-ttu-id="fdb6d-148">Kod inuti hanteraren slutförande körs **när** begäran har slutförts.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-148">Code inside the completion handler will run **after** the request is completed.</span></span> <span data-ttu-id="fdb6d-149">Kod efter slutförande hanteraren kommer att köras **medan** begäran görs.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-149">Code after the completion handler will run **while** the request is being made.</span></span>
> 
> 

## <a name="create-a-container"></a><span data-ttu-id="fdb6d-150">Skapa en behållare</span><span class="sxs-lookup"><span data-stu-id="fdb6d-150">Create a container</span></span>
<span data-ttu-id="fdb6d-151">Varje blobb i Azure Storage måste finnas i en behållare.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-151">Every blob in Azure Storage must reside in a container.</span></span> <span data-ttu-id="fdb6d-152">I följande exempel visas hur du skapar en behållare som kallas *newcontainer*, i ditt lagringskonto om den inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-152">The following example shows how to create a container, called *newcontainer*, in your Storage account if it doesn't already exist.</span></span> <span data-ttu-id="fdb6d-153">När du väljer ett namn för din behållare, Tänk också på namngivningsregler som nämns ovan.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-153">When choosing a name for your container, be mindful of the naming rules mentioned above.</span></span>

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

    // Create container in your Storage account if the container doesn't already exist
    [blobContainer createContainerIfNotExistsWithCompletionHandler:^(NSError *error, BOOL exists) {
        if (error){
            NSLog(@"Error in creating container.");
        }
    }];
}
```

<span data-ttu-id="fdb6d-154">Du kan bekräfta att det fungerar genom att titta på den [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com) och verifiera att *newcontainer* finns i listan över behållare för ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-154">You can confirm that this works by looking at the [Microsoft Azure Storage Explorer](http://storageexplorer.com) and verifying that *newcontainer* is in the list of containers for your Storage account.</span></span>

## <a name="set-container-permissions"></a><span data-ttu-id="fdb6d-155">Ange behörigheter för behållaren</span><span class="sxs-lookup"><span data-stu-id="fdb6d-155">Set Container Permissions</span></span>
<span data-ttu-id="fdb6d-156">En behållare behörigheter har konfigurerats för **privata** åtkomst som standard.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-156">A container's permissions are configured for **Private** access by default.</span></span> <span data-ttu-id="fdb6d-157">Behållare ger dock några olika alternativ för behållaren åtkomst:</span><span class="sxs-lookup"><span data-stu-id="fdb6d-157">However, containers provide a few different options for container access:</span></span>

* <span data-ttu-id="fdb6d-158">**Privata**: behållare och blob-data kan läsas av endast kontoägaren.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-158">**Private**: Container and blob data can be read by the account owner only.</span></span>
* <span data-ttu-id="fdb6d-159">**BLOB**: Blob-data i den här behållaren kan läsas via anonym begäran, men behållardata är inte tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-159">**Blob**: Blob data within this container can be read via anonymous request, but container data is not available.</span></span> <span data-ttu-id="fdb6d-160">Klienter kan inte räkna upp blobbar i behållaren via anonym begäran.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-160">Clients cannot enumerate blobs within the container via anonymous request.</span></span>
* <span data-ttu-id="fdb6d-161">**Behållaren**: behållare och blob-data kan läsas via anonym begäran.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-161">**Container**: Container and blob data can be read via anonymous request.</span></span> <span data-ttu-id="fdb6d-162">Klienter kan räkna upp blobbar i behållaren via anonym begäran, men det går inte att räkna upp behållare i lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-162">Clients can enumerate blobs within the container via anonymous request, but cannot enumerate containers within the storage account.</span></span>

<span data-ttu-id="fdb6d-163">I följande exempel visas hur du skapar en behållare med **behållare** åtkomstbehörigheter som kommer att tillåta offentlig, skrivskyddad åtkomst för alla användare på Internet:</span><span class="sxs-lookup"><span data-stu-id="fdb6d-163">The following example shows you how to create a container with **Container** access permissions, which will allow public, read-only access for all users on the Internet:</span></span>

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

    // Create container in your Storage account if the container doesn't already exist
    [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists){
        if (error){
            NSLog(@"Error in creating container.");
        }
    }];
}
```

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="fdb6d-164">Ladda upp en blobb till en behållare</span><span class="sxs-lookup"><span data-stu-id="fdb6d-164">Upload a blob into a container</span></span>
<span data-ttu-id="fdb6d-165">Som anges i den [Blob-Tjänstkoncept](#blob-service-concepts) avsnittet Blob Storage erbjuder tre olika typer av blobbar: blockblobbar, tilläggsblobbar och sidblobbar.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-165">As mentioned in the [Blob service concepts](#blob-service-concepts) section, Blob Storage offers three different types of blobs: block blobs, append blobs, and page blobs.</span></span> <span data-ttu-id="fdb6d-166">Azure Storage iOS bibliotek stöder alla tre typer av blobbar.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-166">The Azure Storage iOS library supports all three types of blobs.</span></span> <span data-ttu-id="fdb6d-167">I de flesta fall är blockblob den rekommenderade typen.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-167">In most cases, block blob is the recommended type to use.</span></span>

<span data-ttu-id="fdb6d-168">I följande exempel visas hur du överför en blockblobb från en NSString.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-168">The following example shows how to upload a block blob from an NSString.</span></span> <span data-ttu-id="fdb6d-169">Om det redan finns en blob med samma namn i den här behållaren skrivs innehållet i det här blob över.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-169">If a blob with the same name already exists in this container, the contents of this blob will be overwritten.</span></span>

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

                // Upload blob to Storage
                [blockBlob uploadFromText:@"This text will be uploaded to Blob Storage." completionHandler:^(NSError *error) {
                    if (error){
                        NSLog(@"Error in creating blob.");
                    }
                }];
            }
        }];
}
```

<span data-ttu-id="fdb6d-170">Du kan bekräfta att det fungerar genom att titta på den [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com) och verifiera att behållaren, *containerpublic*, innehåller blob, *sampleblob*.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-170">You can confirm that this works by looking at the [Microsoft Azure Storage Explorer](http://storageexplorer.com) and verifying that the container, *containerpublic*, contains the blob, *sampleblob*.</span></span> <span data-ttu-id="fdb6d-171">I det här exemplet använde vi en offentlig behållare, så du kan också kontrollera att programmet fungerar genom att gå till BLOB-URI:</span><span class="sxs-lookup"><span data-stu-id="fdb6d-171">In this sample, we used a public container so you can also verify that this application worked by going to the blobs URI:</span></span>

    https://nameofyourstorageaccount.blob.core.windows.net/containerpublic/sampleblob

<span data-ttu-id="fdb6d-172">Förutom att överföra en blockblobb från en NSString finns liknande metoder för NSData, NSInputStream eller en lokal fil.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-172">In addition to uploading a block blob from an NSString, similar methods exist for NSData, NSInputStream, or a local file.</span></span>

## <a name="list-the-blobs-in-a-container"></a><span data-ttu-id="fdb6d-173">Visa en lista över blobbarna i en behållare</span><span class="sxs-lookup"><span data-stu-id="fdb6d-173">List the blobs in a container</span></span>
<span data-ttu-id="fdb6d-174">I följande exempel visas hur du listar alla blobbar i en behållare.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-174">The following example shows how to list all blobs in a container.</span></span> <span data-ttu-id="fdb6d-175">När du utför den här åtgärden, Tänk på följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="fdb6d-175">When performing this operation, be mindful of the following parameters:</span></span>     

* <span data-ttu-id="fdb6d-176">**continuationToken** -fortsättning token representerar i var lista åtgärden ska börja.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-176">**continuationToken** - The continuation token represents where the listing operation should start.</span></span> <span data-ttu-id="fdb6d-177">Om ingen token anges, den visar en lista över blobbar från början.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-177">If no token is provided, it will list blobs from the beginning.</span></span> <span data-ttu-id="fdb6d-178">Valfritt antal blobbar kan visas från noll upp till en maximal mängd.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-178">Any number of blobs can be listed, from zero up to a set maximum.</span></span> <span data-ttu-id="fdb6d-179">Även om den här metoden returnerar resultat, om `results.continuationToken` inte är noll, det kan finnas fler blobbar på tjänsten som inte finns.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-179">Even if this method returns zero results, if `results.continuationToken` is not nil, there may be more blobs on the service that have not been listed.</span></span>
* <span data-ttu-id="fdb6d-180">**prefixet** -du kan ange prefixet som ska användas för blob-lista.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-180">**prefix** - You can specify the prefix to use for blob listing.</span></span> <span data-ttu-id="fdb6d-181">Blobbar som börjar med prefixet visas.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-181">Only blobs that begin with this prefix will be listed.</span></span>
* <span data-ttu-id="fdb6d-182">**useFlatBlobListing** – som anges i den [namnge och referera till behållare och blobbar](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata) avsnitt, även om Blob-tjänsten är en platt lagring-schemat, du kan skapa en virtuell hierarki genom att namnge blobbar med sökvägsinformation.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-182">**useFlatBlobListing** - As mentioned in the [Naming and referencing containers and blobs](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata) section, although the Blob service is a flat storage scheme, you can create a virtual hierarchy by naming blobs with path information.</span></span> <span data-ttu-id="fdb6d-183">Dock stöds inte platt lista för närvarande inte.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-183">However, non-flat listing is currently not supported.</span></span> <span data-ttu-id="fdb6d-184">Den här funktionen kommer snart.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-184">This feature is coming soon.</span></span> <span data-ttu-id="fdb6d-185">Nu är det här värdet ska vara **Ja**.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-185">For now, this value should be **YES**.</span></span>

* <span data-ttu-id="fdb6d-186">**blobListingDetails** -du kan ange vilka objekt som ska tas med när du visar en lista över blobbar</span><span class="sxs-lookup"><span data-stu-id="fdb6d-186">**blobListingDetails** - You can specify which items to include when listing blobs</span></span>
  * <span data-ttu-id="fdb6d-187">_AZSBlobListingDetailsNone_: Visa endast allokerad blobbar och inte returnerar blobbmetadata.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-187">_AZSBlobListingDetailsNone_: List only committed blobs, and do not return blob metadata.</span></span>
  * <span data-ttu-id="fdb6d-188">_AZSBlobListingDetailsSnapshots_: Visa spara blobbar och blob ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-188">_AZSBlobListingDetailsSnapshots_: List committed blobs and blob snapshots.</span></span>
  * <span data-ttu-id="fdb6d-189">_AZSBlobListingDetailsMetadata_: blob att hämta metadata för varje blob som returneras i listan.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-189">_AZSBlobListingDetailsMetadata_: Retrieve blob metadata for each blob returned in the listing.</span></span>
  * <span data-ttu-id="fdb6d-190">_AZSBlobListingDetailsUncommittedBlobs_: Visa bekräftade och ogenomförda blobbar.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-190">_AZSBlobListingDetailsUncommittedBlobs_: List committed and uncommitted blobs.</span></span>
  * <span data-ttu-id="fdb6d-191">_AZSBlobListingDetailsCopy_: kopia egenskaper i listan.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-191">_AZSBlobListingDetailsCopy_: Include copy properties in the listing.</span></span>
  * <span data-ttu-id="fdb6d-192">_AZSBlobListingDetailsAll_: visa en lista över alla tillgängliga allokerat blobbar ogenomförda blobbar och ögonblicksbilder och returnera alla metadata och kopiera status för de blobbar.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-192">_AZSBlobListingDetailsAll_: List all available committed blobs, uncommitted blobs, and snapshots, and return all metadata and copy status for those blobs.</span></span>
* <span data-ttu-id="fdb6d-193">**maxResults** -det maximala antalet resultat som ska returneras för den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-193">**maxResults** - The maximum number of results to return for this operation.</span></span> <span data-ttu-id="fdb6d-194">Använd -1 om du inte ange en gräns.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-194">Use -1 to not set a limit.</span></span>
* <span data-ttu-id="fdb6d-195">**completionHandler** - kodblocket ska köras med resultatet av åtgärden lista.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-195">**completionHandler** - The block of code to execute with the results of the listing operation.</span></span>

<span data-ttu-id="fdb6d-196">I det här exemplet används en hjälpmetod för rekursivt anrop listan blobbar metoden varje gång en fortsättningstoken returneras.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-196">In this example, a helper method is used to recursively call the list blobs method every time a continuation token is returned.</span></span>

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

## <a name="download-a-blob"></a><span data-ttu-id="fdb6d-197">Ladda ned en blob</span><span class="sxs-lookup"><span data-stu-id="fdb6d-197">Download a blob</span></span>
<span data-ttu-id="fdb6d-198">I följande exempel visas hur du hämtar en blobb till en NSString-objekt.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-198">The following example shows how to download a blob to a NSString object.</span></span>

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

## <a name="delete-a-blob"></a><span data-ttu-id="fdb6d-199">Ta bort en blob</span><span class="sxs-lookup"><span data-stu-id="fdb6d-199">Delete a blob</span></span>
<span data-ttu-id="fdb6d-200">I följande exempel visas hur du tar bort en blob.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-200">The following example shows how to delete a blob.</span></span>

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

## <a name="delete-a-blob-container"></a><span data-ttu-id="fdb6d-201">Ta bort en blob-behållare</span><span class="sxs-lookup"><span data-stu-id="fdb6d-201">Delete a blob container</span></span>
<span data-ttu-id="fdb6d-202">I följande exempel visas hur du tar bort en behållare.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-202">The following example shows how to delete a container.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="fdb6d-203">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fdb6d-203">Next steps</span></span>
<span data-ttu-id="fdb6d-204">Nu när du har lärt dig hur du använder Blob Storage från iOS följa dessa länkar om du vill veta mer om iOS-bibliotek och Storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="fdb6d-204">Now that you've learned how to use Blob Storage from iOS, follow these links to learn more about the iOS library and the Storage service.</span></span>

* [<span data-ttu-id="fdb6d-205">Azure Storage-klientbibliotek för iOS</span><span class="sxs-lookup"><span data-stu-id="fdb6d-205">Azure Storage Client Library for iOS</span></span>](https://github.com/azure/azure-storage-ios)
* [<span data-ttu-id="fdb6d-206">Azure Storage iOS referensdokumentationen</span><span class="sxs-lookup"><span data-stu-id="fdb6d-206">Azure Storage iOS Reference Documentation</span></span>](http://azure.github.io/azure-storage-ios/)
* [<span data-ttu-id="fdb6d-207">REST-API för Azure Storage Services</span><span class="sxs-lookup"><span data-stu-id="fdb6d-207">Azure Storage Services REST API</span></span>](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [<span data-ttu-id="fdb6d-208">Azure Storage Teamblogg</span><span class="sxs-lookup"><span data-stu-id="fdb6d-208">Azure Storage Team Blog</span></span>](http://blogs.msdn.com/b/windowsazurestorage)

<span data-ttu-id="fdb6d-209">Om du har frågor om det här biblioteket gärna skicka till vår [MSDN Azure-forumet](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=windowsazuredata) eller [Stack Overflow](http://stackoverflow.com/questions/tagged/windows-azure-storage+or+windows-azure-storage+or+azure-storage-blobs+or+azure-storage-tables+or+azure-table-storage+or+windows-azure-queues+or+azure-storage-queues+or+azure-storage-emulator+or+azure-storage-files).</span><span class="sxs-lookup"><span data-stu-id="fdb6d-209">If you have questions regarding this library, feel free to post to our [MSDN Azure forum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=windowsazuredata) or [Stack Overflow](http://stackoverflow.com/questions/tagged/windows-azure-storage+or+windows-azure-storage+or+azure-storage-blobs+or+azure-storage-tables+or+azure-table-storage+or+windows-azure-queues+or+azure-storage-queues+or+azure-storage-emulator+or+azure-storage-files).</span></span>
<span data-ttu-id="fdb6d-210">Om du har förslag på funktioner för Azure Storage, efter att [Azure Storage Feedback](https://feedback.azure.com/forums/217298-storage/).</span><span class="sxs-lookup"><span data-stu-id="fdb6d-210">If you have feature suggestions for Azure Storage, please post to [Azure Storage Feedback](https://feedback.azure.com/forums/217298-storage/).</span></span>


---
title: "aaaDevelop för Azure File storage med .NET | Microsoft Docs"
description: "Lär dig hur toodevelop .NET-program och tjänster som använder Azure File storage toostore fildata."
services: storage
documentationcenter: .net
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 6a889ee1-1e60-46ec-a592-ae854f9fb8b6
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/27/2017
ms.author: renash
ms.openlocfilehash: 79855f178111483edea13014b8eeecc3376dd4e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="develop-for-azure-file-storage-with-net"></a><span data-ttu-id="0bd6c-103">Utveckla för Azure File Storage med .NET</span><span class="sxs-lookup"><span data-stu-id="0bd6c-103">Develop for Azure File storage with .NET</span></span> 
> [!NOTE]
> <span data-ttu-id="0bd6c-104">Den här artikeln visar hur toomanage Azure File storage med .NET-kod.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-104">This article shows how toomanage Azure File storage with .NET code.</span></span> <span data-ttu-id="0bd6c-105">toolearn mer om Azure File storage finns hello [introduktion tooAzure fillagring](storage-files-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="0bd6c-105">toolearn more about Azure File storage, please see hello [Introduction tooAzure File storage](storage-files-introduction.md).</span></span>
>

[!INCLUDE [storage-selector-file-include](../../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-check-out-samples-dotnet](../../../includes/storage-check-out-samples-dotnet.md)]

## <a name="about-this-tutorial"></a><span data-ttu-id="0bd6c-106">Om den här självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="0bd6c-106">About this tutorial</span></span>
<span data-ttu-id="0bd6c-107">Den här kursen visar hello grunderna i toodevelop .NET-program eller tjänster som använder Azure File storage toostore fildata.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-107">This tutorial will demonstrate hello basics of using .NET toodevelop applications or services that use Azure File storage toostore file data.</span></span> <span data-ttu-id="0bd6c-108">I den här kursen ska vi skapa ett enkelt konsolprogram och visa hur tooperform grundläggande åtgärder med .NET och Azure File storage:</span><span class="sxs-lookup"><span data-stu-id="0bd6c-108">In this tutorial, we will create a simple console application and show how tooperform basic actions with .NET and Azure File storage:</span></span>

* <span data-ttu-id="0bd6c-109">Hämta hello innehållet i en fil</span><span class="sxs-lookup"><span data-stu-id="0bd6c-109">Get hello contents of a file</span></span>
* <span data-ttu-id="0bd6c-110">Ange hello kvot (största tillåtna storleken) för hello filresurs.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-110">Set hello quota (maximum size) for hello file share.</span></span>
* <span data-ttu-id="0bd6c-111">Skapa en signatur för delad åtkomst (SAS-nyckel) för en fil som använder en princip för delad åtkomst som definierats på hello-resurs.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-111">Create a shared access signature (SAS key) for a file that uses a shared access policy defined on hello share.</span></span>
* <span data-ttu-id="0bd6c-112">Kopiera en fil för tooanother i hello samma lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-112">Copy a file tooanother file in hello same storage account.</span></span>
* <span data-ttu-id="0bd6c-113">Kopiera en fil tooa blob i hello samma lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-113">Copy a file tooa blob in hello same storage account.</span></span>
* <span data-ttu-id="0bd6c-114">Använd Azure Storage-mätvärden för felsökning</span><span class="sxs-lookup"><span data-stu-id="0bd6c-114">Use Azure Storage Metrics for troubleshooting</span></span>

> [!Note]  
> <span data-ttu-id="0bd6c-115">Eftersom Azure File storage kan nås över SMB, är det möjligt toowrite enkla program som har åtkomst till hello Azure filresurs med hjälp av hello standard System.IO klasser för i/o.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-115">Because Azure File storage may be accessed over SMB, it is possible toowrite simple applications that access hello Azure File share using hello standard System.IO classes for File I/O.</span></span> <span data-ttu-id="0bd6c-116">Den här artikeln beskriver hur toowrite program som använder hello Azure Storage .NET SDK, som använder hello [Azure File storage REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure File storage.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-116">This article will describe how toowrite applications that use hello Azure Storage .NET SDK, which uses hello [Azure File storage REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure File storage.</span></span> 


## <a name="create-hello-console-application-and-obtain-hello-assembly"></a><span data-ttu-id="0bd6c-117">Skapa hello konsolprogram och få hello sammansättning</span><span class="sxs-lookup"><span data-stu-id="0bd6c-117">Create hello console application and obtain hello assembly</span></span>
<span data-ttu-id="0bd6c-118">Skapa ett nytt Windows-konsolprogram i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-118">In Visual Studio, create a new Windows console application.</span></span> <span data-ttu-id="0bd6c-119">hello följande steg visar hur toocreate ett konsolprogram i Visual Studio 2017 dock hello stegen är ungefär i andra versioner av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-119">hello following steps show you how toocreate a console application in Visual Studio 2017, however, hello steps are similar in other versions of Visual Studio.</span></span>

1. <span data-ttu-id="0bd6c-120">Välj **Arkiv** > **Nytt** > **Projekt**</span><span class="sxs-lookup"><span data-stu-id="0bd6c-120">Select **File** > **New** > **Project**</span></span>
2. <span data-ttu-id="0bd6c-121">Välj **Installerat** > **Mallar** > **Visual C#** > **Windows Classic Desktop**</span><span class="sxs-lookup"><span data-stu-id="0bd6c-121">Select **Installed** > **Templates** > **Visual C#** > **Windows Classic Desktop**</span></span>
3. <span data-ttu-id="0bd6c-122">Välj **Konsolprogram (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="0bd6c-122">Select **Console App (.NET Framework)**</span></span>
4. <span data-ttu-id="0bd6c-123">Ange ett namn för ditt program i hello **Name:** fält</span><span class="sxs-lookup"><span data-stu-id="0bd6c-123">Enter a name for your application in hello **Name:** field</span></span>
5. <span data-ttu-id="0bd6c-124">Välj **OK**</span><span class="sxs-lookup"><span data-stu-id="0bd6c-124">Select **OK**</span></span>

<span data-ttu-id="0bd6c-125">Alla kodexempel i den här kursen kan läggas till toohello `Main()` -metoden i konsolen programmets `Program.cs` fil.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-125">All code examples in this tutorial can be added toohello `Main()` method of your console application's `Program.cs` file.</span></span>

<span data-ttu-id="0bd6c-126">Du kan använda hello Azure Storage-klientbibliotek för någon typ av .NET-program, inklusive en Azure cloud service eller web app, och stationära och mobila program.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-126">You can use hello Azure Storage Client Library in any type of .NET application, including an Azure cloud service or web app, and desktop and mobile applications.</span></span> <span data-ttu-id="0bd6c-127">I den här guiden använder vi oss av en konsolapp för enkelhetens skull.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-127">In this guide, we use a console application for simplicity.</span></span>

## <a name="use-nuget-tooinstall-hello-required-packages"></a><span data-ttu-id="0bd6c-128">Använd NuGet tooinstall krävs hello-paket</span><span class="sxs-lookup"><span data-stu-id="0bd6c-128">Use NuGet tooinstall hello required packages</span></span>
<span data-ttu-id="0bd6c-129">Det finns två paket som du behöver tooreference i ditt projekt toocomplete för den här kursen:</span><span class="sxs-lookup"><span data-stu-id="0bd6c-129">There are two packages you need tooreference in your project toocomplete this tutorial:</span></span>

* <span data-ttu-id="0bd6c-130">[Microsoft Azure Storage-klientbibliotek för .NET](https://www.nuget.org/packages/WindowsAzure.Storage/): det här paketet ger programmatisk åtkomst toodata resurser i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-130">[Microsoft Azure Storage Client Library for .NET](https://www.nuget.org/packages/WindowsAzure.Storage/): This package provides programmatic access toodata resources in your storage account.</span></span>
* <span data-ttu-id="0bd6c-131">[Microsoft Azure Configuration Manager-biblioteket för .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/): det här paketet tillhandahåller en klass för parsning av en anslutningssträng i en konfigurationsfil, oavsett var ditt program körs.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-131">[Microsoft Azure Configuration Manager library for .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/): This package provides a class for parsing a connection string in a configuration file, regardless of where your application is running.</span></span>

<span data-ttu-id="0bd6c-132">Du kan använda NuGet tooobtain båda paketen.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-132">You can use NuGet tooobtain both packages.</span></span> <span data-ttu-id="0bd6c-133">Följ de här stegen:</span><span class="sxs-lookup"><span data-stu-id="0bd6c-133">Follow these steps:</span></span>

1. <span data-ttu-id="0bd6c-134">Högerklicka på ditt projekt i **Solution Explorer** och välj **Hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-134">Right-click your project in **Solution Explorer** and choose **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="0bd6c-135">Sök online efter ”WindowsAzure.Storage” och klicka på **installera** tooinstall hello Storage-klientbiblioteket och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-135">Search online for "WindowsAzure.Storage" and click **Install** tooinstall hello Storage Client Library and its dependencies.</span></span>
3. <span data-ttu-id="0bd6c-136">Sök online efter ”WindowsAzure.ConfigurationManager” och klicka på **installera** tooinstall hello Azure Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-136">Search online for "WindowsAzure.ConfigurationManager" and click **Install** tooinstall hello Azure Configuration Manager.</span></span>

## <a name="save-your-storage-account-credentials-toohello-appconfig-file"></a><span data-ttu-id="0bd6c-137">Spara storage-konto autentiseringsuppgifter toohello app.config-filen</span><span class="sxs-lookup"><span data-stu-id="0bd6c-137">Save your storage account credentials toohello app.config file</span></span>
<span data-ttu-id="0bd6c-138">Nu ska du spara dina autentiseringsuppgifter i projektets app.config-fil.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-138">Next, save your credentials in your project's app.config file.</span></span> <span data-ttu-id="0bd6c-139">Redigera hello app.config-filen så att den visas liknande toohello följande exempel, ersätter `myaccount` med namnet på ditt lagringskonto, och `mykey` med din lagringskontonyckel.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-139">Edit hello app.config file so that it appears similar toohello following example, replacing `myaccount` with your storage account name, and `mykey` with your storage account key.</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <startup>
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
    </startup>
    <appSettings>
        <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=StorageAccountKeyEndingIn==" />
    </appSettings>
</configuration>
```

> [!NOTE]
> hello senaste versionen av hello Azure storage-emulatorn har inte stöd för Azure File storage. <span data-ttu-id="0bd6c-141">Anslutningssträngen måste ha ett Azure Storage-konto i hello molnet toowork med Azure File storage.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-141">Your connection string must target an Azure Storage Account in hello cloud toowork with Azure File storage.</span></span>

## <a name="add-using-directives"></a><span data-ttu-id="0bd6c-142">Lägga till med hjälp av direktiv</span><span class="sxs-lookup"><span data-stu-id="0bd6c-142">Add using directives</span></span>
<span data-ttu-id="0bd6c-143">Öppna hello `Program.cs` från Solution Explorer och Lägg till följande hello med direktiven toohello överkant hello-filen.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-143">Open hello `Program.cs` file from Solution Explorer, and add hello following using directives toohello top of hello file.</span></span>

```csharp
using Microsoft.Azure; // Namespace for Azure Configuration Manager
using Microsoft.WindowsAzure.Storage; // Namespace for Storage Client Library
using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Azure Blobs
using Microsoft.WindowsAzure.Storage.File; // Namespace for Azure File storage
```

[!INCLUDE [storage-cloud-configuration-manager-include](../../../includes/storage-cloud-configuration-manager-include.md)]

## <a name="access-hello-file-share-programmatically"></a><span data-ttu-id="0bd6c-144">Åtkomst hello filresursen via programmering</span><span class="sxs-lookup"><span data-stu-id="0bd6c-144">Access hello file share programmatically</span></span>
<span data-ttu-id="0bd6c-145">Lägg till följande kod toohello hello `Main()` metod (efter hello koden som visas ovan) tooretrieve hello anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-145">Next, add hello following code toohello `Main()` method (after hello code shown above) tooretrieve hello connection string.</span></span> <span data-ttu-id="0bd6c-146">Den här koden hämtar en referens toohello fil som vi skapade tidigare och matar ut sitt innehåll toohello konsolfönster.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-146">This code gets a reference toohello file we created earlier and outputs its contents toohello console window.</span></span>

```csharp
// Create a CloudFileClient object for credentialed access tooAzure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference toohello file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that hello share exists.
if (share.Exists())
{
    // Get a reference toohello root directory for hello share.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();

    // Get a reference toohello directory we created previously.
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");

    // Ensure that hello directory exists.
    if (sampleDir.Exists())
    {
        // Get a reference toohello file we created previously.
        CloudFile file = sampleDir.GetFileReference("Log1.txt");

        // Ensure that hello file exists.
        if (file.Exists())
        {
            // Write hello contents of hello file toohello console window.
            Console.WriteLine(file.DownloadTextAsync().Result);
        }
    }
}
```

<span data-ttu-id="0bd6c-147">Kör hello konsolens programmet toosee hello utdata.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-147">Run hello console application toosee hello output.</span></span>

## <a name="set-hello-maximum-size-for-a-file-share"></a><span data-ttu-id="0bd6c-148">Ange hello största storleken för en filresurs</span><span class="sxs-lookup"><span data-stu-id="0bd6c-148">Set hello maximum size for a file share</span></span>
<span data-ttu-id="0bd6c-149">Från och med version 5.x av hello Azure Storage-klientbibliotek, du kan ange set hello kvot eller en maximal storlek för en filresurs, i gigabyte.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-149">Beginning with version 5.x of hello Azure Storage Client Library, you can set set hello quota (or maximum size) for a file share, in gigabytes.</span></span> <span data-ttu-id="0bd6c-150">Du kan också kontrollera hur mycket data som lagras på resursen hello toosee.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-150">You can also check toosee how much data is currently stored on hello share.</span></span>

<span data-ttu-id="0bd6c-151">Genom att ange hello kvoten för en resurs, kan du begränsa hello totala storleken för hello-filer som lagras på hello-resurs.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-151">By setting hello quota for a share, you can limit hello total size of hello files stored on hello share.</span></span> <span data-ttu-id="0bd6c-152">Om hello totala storleken på filerna på hello resursen överskrider ange hello kvot för hello resursen och klienter ska tooincrease hello storleken på befintliga filer eller skapa nya filer såvida inte dessa filer är tomma.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-152">If hello total size of files on hello share exceeds hello quota set on hello share, then clients will be unable tooincrease hello size of existing files or create new files, unless those files are empty.</span></span>

<span data-ttu-id="0bd6c-153">hello exemplet nedan visar hur toocheck hello aktuella användningen av en resurs och hur tooset hello kvoten för hello resursen.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-153">hello example below shows how toocheck hello current usage for a share and how tooset hello quota for hello share.</span></span>

```csharp
// Parse hello connection string for hello storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access tooAzure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference toohello file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that hello share exists.
if (share.Exists())
{
    // Check current usage stats for hello share.
    // Note that hello ShareStats object is part of hello protocol layer for hello File service.
    Microsoft.WindowsAzure.Storage.File.Protocol.ShareStats stats = share.GetStats();
    Console.WriteLine("Current share usage: {0} GB", stats.Usage.ToString());

    // Specify hello maximum size of hello share, in GB.
    // This line sets hello quota toobe 10 GB greater than hello current usage of hello share.
    share.Properties.Quota = 10 + stats.Usage;
    share.SetProperties();

    // Now check hello quota for hello share. Call FetchAttributes() toopopulate hello share's properties.
    share.FetchAttributes();
    Console.WriteLine("Current share quota: {0} GB", share.Properties.Quota);
}
```

### <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a><span data-ttu-id="0bd6c-154">Generera en signatur för delad åtkomst för en fil eller filresurs</span><span class="sxs-lookup"><span data-stu-id="0bd6c-154">Generate a shared access signature for a file or file share</span></span>
<span data-ttu-id="0bd6c-155">Från och med version 5.x av hello Azure Storage-klientbibliotek, du kan generera en signatur för delad åtkomst (SAS) för en filresurs eller för en enskild fil.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-155">Beginning with version 5.x of hello Azure Storage Client Library, you can generate a shared access signature (SAS) for a file share or for an individual file.</span></span> <span data-ttu-id="0bd6c-156">Du kan också skapa en princip för delad åtkomst på en toomanage delade filresurser signaturer.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-156">You can also create a shared access policy on a file share toomanage shared access signatures.</span></span> <span data-ttu-id="0bd6c-157">Skapa en princip för delad åtkomst rekommenderas eftersom det ger dig möjlighet att återkalla hello SAS om det behövs.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-157">Creating a shared access policy is recommended, as it provides a means of revoking hello SAS if it should be compromised.</span></span>

<span data-ttu-id="0bd6c-158">hello följande exempel skapar en princip för delad åtkomst på en resurs och använder sedan som delar tooprovide hello principbegränsningar för en SAS för en fil i hello.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-158">hello following example creates a shared access policy on a share, and then uses that policy tooprovide hello constraints for a SAS on a file in hello share.</span></span>

```csharp
// Parse hello connection string for hello storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access tooAzure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference toohello file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that hello share exists.
if (share.Exists())
{
    string policyName = "sampleSharePolicy" + DateTime.UtcNow.Ticks;

    // Create a new shared access policy and define its constraints.
    SharedAccessFilePolicy sharedPolicy = new SharedAccessFilePolicy()
        {
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessFilePermissions.Read | SharedAccessFilePermissions.Write
        };

    // Get existing permissions for hello share.
    FileSharePermissions permissions = share.GetPermissions();

    // Add hello shared access policy toohello share's policies. Note that each policy must have a unique name.
    permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
    share.SetPermissions(permissions);

    // Generate a SAS for a file in hello share and associate this access policy with it.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");
    CloudFile file = sampleDir.GetFileReference("Log1.txt");
    string sasToken = file.GetSharedAccessSignature(null, policyName);
    Uri fileSasUri = new Uri(file.StorageUri.PrimaryUri.ToString() + sasToken);

    // Create a new CloudFile object from hello SAS, and write some text toohello file.
    CloudFile fileSas = new CloudFile(fileSasUri);
    fileSas.UploadText("This write operation is authenticated via SAS.");
    Console.WriteLine(fileSas.DownloadText());
}
```

<span data-ttu-id="0bd6c-159">Mer information om hur du skapar och använder signaturer för delad åtkomst finns i [Använda signaturer för delad åtkomst (SAS)](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) och [Skapa och använda en SAS med Azure Blobs](../blobs/storage-dotnet-shared-access-signature-part-2.md).</span><span class="sxs-lookup"><span data-stu-id="0bd6c-159">For more information about creating and using shared access signatures, see [Using Shared Access Signatures (SAS)](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) and [Create and use a SAS with Azure Blobs](../blobs/storage-dotnet-shared-access-signature-part-2.md).</span></span>

## <a name="copy-files"></a><span data-ttu-id="0bd6c-160">Kopiera filer</span><span class="sxs-lookup"><span data-stu-id="0bd6c-160">Copy files</span></span>
<span data-ttu-id="0bd6c-161">Från och med version 5.x av hello Azure Storage-klientbibliotek, kan du kopiera en fil för tooanother, en tooa blob-fil eller en tooa blob-fil.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-161">Beginning with version 5.x of hello Azure Storage Client Library, you can copy a file tooanother file, a file tooa blob, or a blob tooa file.</span></span> <span data-ttu-id="0bd6c-162">I nästa avsnitt hello visar vi hur tooperform dessa kopiera operations programmässigt.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-162">In hello next sections, we demonstrate how tooperform these copy operations programmatically.</span></span>

<span data-ttu-id="0bd6c-163">Du kan också använda AzCopy toocopy en fil tooanother eller toocopy en blob tooa fil eller tvärtom.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-163">You can also use AzCopy toocopy one file tooanother or toocopy a blob tooa file or vice versa.</span></span> <span data-ttu-id="0bd6c-164">Se [överföra data med kommandoradsverktyget Azcopy hello](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0bd6c-164">See [Transfer data with hello AzCopy Command-Line Utility](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).</span></span>

> [!NOTE]
> <span data-ttu-id="0bd6c-165">Om du kopierar en tooa blob-fil eller en fil tooa blob, måste du använda en delad åtkomst (SAS)-signaturen tooauthenticate hello källobjektet, även om du kopierar inom hello samma lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-165">If you are copying a blob tooa file, or a file tooa blob, you must use a shared access signature (SAS) tooauthenticate hello source object, even if you are copying within hello same storage account.</span></span>
> 
> 

<span data-ttu-id="0bd6c-166">**Kopiera en fil tooanother** hello följande exempel kopierar en fil för tooanother i hello samma resurs.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-166">**Copy a file tooanother file** hello following example copies a file tooanother file in hello same share.</span></span> <span data-ttu-id="0bd6c-167">Eftersom den här kopieringsåtgärden kopierar mellan filer i hello samma lagringskonto kan du använda delad nyckel autentisering tooperform hello kopia.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-167">Because this copy operation copies between files in hello same storage account, you can use Shared Key authentication tooperform hello copy.</span></span>

```csharp
// Parse hello connection string for hello storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access tooAzure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference toohello file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that hello share exists.
if (share.Exists())
{
    // Get a reference toohello root directory for hello share.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();

    // Get a reference toohello directory we created previously.
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");

    // Ensure that hello directory exists.
    if (sampleDir.Exists())
    {
        // Get a reference toohello file we created previously.
        CloudFile sourceFile = sampleDir.GetFileReference("Log1.txt");

        // Ensure that hello source file exists.
        if (sourceFile.Exists())
        {
            // Get a reference toohello destination file.
            CloudFile destFile = sampleDir.GetFileReference("Log1Copy.txt");

            // Start hello copy operation.
            destFile.StartCopy(sourceFile);

            // Write hello contents of hello destination file toohello console window.
            Console.WriteLine(destFile.DownloadText());
        }
    }
}
```

<span data-ttu-id="0bd6c-168">**Kopiera en fil tooa blob** hello följande exempel skapas en fil och kopierar den tooa blob inom hello samma lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-168">**Copy a file tooa blob** hello following example creates a file and copies it tooa blob within hello same storage account.</span></span> <span data-ttu-id="0bd6c-169">hello exemplet skapas en SAS för källfilen hello, vilka hello-tjänsten använder tooauthenticate åtkomst toohello källfilen under kopieringen hello.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-169">hello example creates a SAS for hello source file, which hello service uses tooauthenticate access toohello source file during hello copy operation.</span></span>

```csharp
// Parse hello connection string for hello storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access tooAzure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Create a new file share, if it does not already exist.
CloudFileShare share = fileClient.GetShareReference("sample-share");
share.CreateIfNotExists();

// Create a new file in hello root directory.
CloudFile sourceFile = share.GetRootDirectoryReference().GetFileReference("sample-file.txt");
sourceFile.UploadText("A sample file in hello root directory.");

// Get a reference toohello blob toowhich hello file will be copied.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
container.CreateIfNotExists();
CloudBlockBlob destBlob = container.GetBlockBlobReference("sample-blob.txt");

// Create a SAS for hello file that's valid for 24 hours.
// Note that when you are copying a file tooa blob, or a blob tooa file, you must use a SAS
// tooauthenticate access toohello source object, even if you are copying within hello same
// storage account.
string fileSas = sourceFile.GetSharedAccessSignature(new SharedAccessFilePolicy()
{
    // Only read permissions are required for hello source file.
    Permissions = SharedAccessFilePermissions.Read,
    SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24)
});

// Construct hello URI toohello source file, including hello SAS token.
Uri fileSasUri = new Uri(sourceFile.StorageUri.PrimaryUri.ToString() + fileSas);

// Copy hello file toohello blob.
destBlob.StartCopy(fileSasUri);

// Write hello contents of hello file toohello console window.
Console.WriteLine("Source file contents: {0}", sourceFile.DownloadText());
Console.WriteLine("Destination blob contents: {0}", destBlob.DownloadText());
```

<span data-ttu-id="0bd6c-170">Du kan kopiera en tooa blob-fil i hello samma sätt.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-170">You can copy a blob tooa file in hello same way.</span></span> <span data-ttu-id="0bd6c-171">Om hello källobjektet är en blobb skapar du en SAS tooauthenticate åtkomst toothat blob under hello kopieringen.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-171">If hello source object is a blob, then create a SAS tooauthenticate access toothat blob during hello copy operation.</span></span>

## <a name="troubleshooting-azure-file-storage-using-metrics"></a><span data-ttu-id="0bd6c-172">Felsöka Azure File Storage med hjälp av mätvärden</span><span class="sxs-lookup"><span data-stu-id="0bd6c-172">Troubleshooting Azure File storage using metrics</span></span>
<span data-ttu-id="0bd6c-173">Nu stöder Azure Storage Analytics mätvärden för Azure File Storage.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-173">Azure Storage Analytics now supports metrics for Azure File storage.</span></span> <span data-ttu-id="0bd6c-174">Med hjälp av mätvärdesdata kan du spåra begäranden och diagnostisera problem.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-174">With metrics data, you can trace requests and diagnose issues.</span></span>


<span data-ttu-id="0bd6c-175">Du kan aktivera mätvärden för Azure File storage från hello [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="0bd6c-175">You can enable metrics for Azure File storage from hello [Azure Portal](https://portal.azure.com).</span></span> <span data-ttu-id="0bd6c-176">Du kan också aktivera mätvärden via programmering genom att anropa hello åtgärden Ange egenskaper för filtjänsten via hello REST-API, eller en av dess motsvarigheter i hello Storage-klientbiblioteket.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-176">You can also enable metrics programmatically by calling hello Set File Service Properties operation via hello REST API, or one of its analogues in hello Storage Client Library.</span></span>


<span data-ttu-id="0bd6c-177">hello följande kodexempel visar hur toouse hello Storage-klientbibliotek för .NET tooenable mätvärden för Azure File storage.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-177">hello following code example shows how toouse hello Storage Client Library for .NET tooenable metrics for Azure File storage.</span></span>

<span data-ttu-id="0bd6c-178">Lägg först till hello följande `using` direktiven tooyour `Program.cs` filen dessutom toothose du lades till ovan:</span><span class="sxs-lookup"><span data-stu-id="0bd6c-178">First, add hello following `using` directives tooyour `Program.cs` file, in addition toothose you added above:</span></span>

```csharp
using Microsoft.WindowsAzure.Storage.File.Protocol;
using Microsoft.WindowsAzure.Storage.Shared.Protocol;
```

<span data-ttu-id="0bd6c-179">Observera att medan Azure BLOB, Azure Table och Azure köer använder hello delade `ServiceProperties` typen i hello `Microsoft.WindowsAzure.Storage.Shared.Protocol` namnområde, Azure File storage använder sin egen typ hello `FileServiceProperties` typen i hello `Microsoft.WindowsAzure.Storage.File.Protocol` namnområde.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-179">Note that while Azure Blobs, Azure Table, and Azure Queues use hello shared `ServiceProperties` type in hello `Microsoft.WindowsAzure.Storage.Shared.Protocol` namespace, Azure File storage uses its own type, hello `FileServiceProperties` type in hello `Microsoft.WindowsAzure.Storage.File.Protocol` namespace.</span></span> <span data-ttu-id="0bd6c-180">Båda namnområden måste refereras från din kod, men för hello följande kod toocompile.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-180">Both namespaces must be referenced from your code, however, for hello following code toocompile.</span></span>

```csharp
// Parse your storage connection string from your application's configuration file.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));
// Create hello File service client.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Set metrics properties for File service.
// Note that hello File service currently uses its own service properties type,
// available in hello Microsoft.WindowsAzure.Storage.File.Protocol namespace.
fileClient.SetServiceProperties(new FileServiceProperties()
{
    // Set hour metrics
    HourMetrics = new MetricsProperties()
    {
        MetricsLevel = MetricsLevel.ServiceAndApi,
        RetentionDays = 14,
        Version = "1.0"
    },
    // Set minute metrics
    MinuteMetrics = new MetricsProperties()
    {
        MetricsLevel = MetricsLevel.ServiceAndApi,
        RetentionDays = 7,
        Version = "1.0"
    }
});

// Read hello metrics properties we just set.
FileServiceProperties serviceProperties = fileClient.GetServiceProperties();
Console.WriteLine("Hour metrics:");
Console.WriteLine(serviceProperties.HourMetrics.MetricsLevel);
Console.WriteLine(serviceProperties.HourMetrics.RetentionDays);
Console.WriteLine(serviceProperties.HourMetrics.Version);
Console.WriteLine();
Console.WriteLine("Minute metrics:");
Console.WriteLine(serviceProperties.MinuteMetrics.MetricsLevel);
Console.WriteLine(serviceProperties.MinuteMetrics.RetentionDays);
Console.WriteLine(serviceProperties.MinuteMetrics.Version);
```

<span data-ttu-id="0bd6c-181">Dessutom kan du referera för[Azure File storage felsökning artikel](storage-troubleshoot-windows-file-connection-problems.md) för slutpunkt till slutpunkt felsökningsinformation.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-181">Also, you can refer too[Azure File storage Troubleshooting Article](storage-troubleshoot-windows-file-connection-problems.md) for end-to-end troubleshooting guidance.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0bd6c-182">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0bd6c-182">Next steps</span></span>
<span data-ttu-id="0bd6c-183">Mer information om Azure File Storage finns på följande länkar.</span><span class="sxs-lookup"><span data-stu-id="0bd6c-183">See these links for more information about Azure File storage.</span></span>

### <a name="conceptual-articles-and-videos"></a><span data-ttu-id="0bd6c-184">Begreppsrelaterade artiklar och videoklipp</span><span class="sxs-lookup"><span data-stu-id="0bd6c-184">Conceptual articles and videos</span></span>
* [<span data-ttu-id="0bd6c-185">Azure Files Storage: ett friktionslöst SMB-filsystem i molnet för Windows och Linux</span><span class="sxs-lookup"><span data-stu-id="0bd6c-185">Azure File storage: a frictionless cloud SMB file system for Windows and Linux</span></span>](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [<span data-ttu-id="0bd6c-186">Hur toouse Azure File storage med Linux</span><span class="sxs-lookup"><span data-stu-id="0bd6c-186">How toouse Azure File storage with Linux</span></span>](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-file-storage"></a><span data-ttu-id="0bd6c-187">Verktygsstöd för File Storage</span><span class="sxs-lookup"><span data-stu-id="0bd6c-187">Tooling support for File storage</span></span>
* [<span data-ttu-id="0bd6c-188">Hur toouse AzCopy med Microsoft Azure Storage</span><span class="sxs-lookup"><span data-stu-id="0bd6c-188">How toouse AzCopy with Microsoft Azure Storage</span></span>](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)
* [<span data-ttu-id="0bd6c-189">Använda hello Azure CLI med Azure Storage</span><span class="sxs-lookup"><span data-stu-id="0bd6c-189">Using hello Azure CLI with Azure Storage</span></span>](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#create-and-manage-file-shares)
* [<span data-ttu-id="0bd6c-190">Felsökning av problem i Azure File Storage</span><span class="sxs-lookup"><span data-stu-id="0bd6c-190">Troubleshooting Azure File storage problems</span></span>](https://docs.microsoft.com/azure/storage/storage-troubleshoot-file-connection-problems)

### <a name="reference"></a><span data-ttu-id="0bd6c-191">Referens</span><span class="sxs-lookup"><span data-stu-id="0bd6c-191">Reference</span></span>
* [<span data-ttu-id="0bd6c-192">Storage-klientbibliotek för .NET-referens</span><span class="sxs-lookup"><span data-stu-id="0bd6c-192">Storage Client Library for .NET reference</span></span>](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [<span data-ttu-id="0bd6c-193">File Service REST API referens</span><span class="sxs-lookup"><span data-stu-id="0bd6c-193">File Service REST API reference</span></span>](http://msdn.microsoft.com/library/azure/dn167006.aspx)

### <a name="blog-posts"></a><span data-ttu-id="0bd6c-194">Blogginlägg</span><span class="sxs-lookup"><span data-stu-id="0bd6c-194">Blog posts</span></span>
* [<span data-ttu-id="0bd6c-195">Azure File Storage finns nu allmänt tillgänglig</span><span class="sxs-lookup"><span data-stu-id="0bd6c-195">Azure File storage is now generally available</span></span>](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [<span data-ttu-id="0bd6c-196">Inuti Azure File Storage</span><span class="sxs-lookup"><span data-stu-id="0bd6c-196">Inside Azure File storage</span></span>](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [<span data-ttu-id="0bd6c-197">Introduktion till Microsoft Azure File Service</span><span class="sxs-lookup"><span data-stu-id="0bd6c-197">Introducing Microsoft Azure File Service</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [<span data-ttu-id="0bd6c-198">Spara anslutningar tooMicrosoft Azure File storage</span><span class="sxs-lookup"><span data-stu-id="0bd6c-198">Persisting connections tooMicrosoft Azure File storage</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx)

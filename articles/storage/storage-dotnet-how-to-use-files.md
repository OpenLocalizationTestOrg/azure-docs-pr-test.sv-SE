---
title: "Utveckla för Azure File Storage med .NET | Microsoft Docs"
description: "Lär dig hur du utvecklar .NET-program och tjänster som använder Azure File Storage för att lagra fildata."
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
ms.openlocfilehash: e26da88ef1803d47d7286c5ae836ac9c041dadd1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="develop-for-azure-file-storage-with-net"></a><span data-ttu-id="d5582-103">Utveckla för Azure File Storage med .NET</span><span class="sxs-lookup"><span data-stu-id="d5582-103">Develop for Azure File storage with .NET</span></span> 
> [!NOTE]
> <span data-ttu-id="d5582-104">Den här artikeln visar hur du hanterar Azure File Storage med .NET-kod.</span><span class="sxs-lookup"><span data-stu-id="d5582-104">This article shows how to manage Azure File storage with .NET code.</span></span> <span data-ttu-id="d5582-105">Mer information om Azure File Storage finns i [Introduction to Azure File storage](storage-files-introduction.md) (Introduktion till Azure File Storage).</span><span class="sxs-lookup"><span data-stu-id="d5582-105">To learn more about Azure File storage, please see the [Introduction to Azure File storage](storage-files-introduction.md).</span></span>
>

[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-check-out-samples-dotnet](../../includes/storage-check-out-samples-dotnet.md)]

## <a name="about-this-tutorial"></a><span data-ttu-id="d5582-106">Om den här självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="d5582-106">About this tutorial</span></span>
<span data-ttu-id="d5582-107">Den här kursen visar grunderna i hur du använder .NET för att utveckla program eller tjänster som lagrar fildata med hjälp av Azure File Storage.</span><span class="sxs-lookup"><span data-stu-id="d5582-107">This tutorial will demonstrate the basics of using .NET to develop applications or services that use Azure File storage to store file data.</span></span> <span data-ttu-id="d5582-108">I den här självstudiekursen skapar vi ett enkelt konsolprogram och visar hur du utför grundläggande åtgärder med .NET och Azure File Storage:</span><span class="sxs-lookup"><span data-stu-id="d5582-108">In this tutorial, we will create a simple console application and show how to perform basic actions with .NET and Azure File storage:</span></span>

* <span data-ttu-id="d5582-109">Hämta innehållet i en fil</span><span class="sxs-lookup"><span data-stu-id="d5582-109">Get the contents of a file</span></span>
* <span data-ttu-id="d5582-110">Ange kvoten (den största tillåtna storleken) för filresursen.</span><span class="sxs-lookup"><span data-stu-id="d5582-110">Set the quota (maximum size) for the file share.</span></span>
* <span data-ttu-id="d5582-111">Skapa en signatur för delad åtkomst (SAS-nyckel) för en fil som använder en princip för delad åtkomst som definierats för resursen.</span><span class="sxs-lookup"><span data-stu-id="d5582-111">Create a shared access signature (SAS key) for a file that uses a shared access policy defined on the share.</span></span>
* <span data-ttu-id="d5582-112">Kopiera en fil till en annan fil i samma lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="d5582-112">Copy a file to another file in the same storage account.</span></span>
* <span data-ttu-id="d5582-113">Kopiera en fil till en blobb i samma lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="d5582-113">Copy a file to a blob in the same storage account.</span></span>
* <span data-ttu-id="d5582-114">Använd Azure Storage-mätvärden för felsökning</span><span class="sxs-lookup"><span data-stu-id="d5582-114">Use Azure Storage Metrics for troubleshooting</span></span>

> [!Note]  
> <span data-ttu-id="d5582-115">Eftersom Azure File Storage kan nås över SMB kan du skriva enkla program som har åtkomst till Azure-filresursen med System.IO-standardklasser för fil-I/O.</span><span class="sxs-lookup"><span data-stu-id="d5582-115">Because Azure File storage may be accessed over SMB, it is possible to write simple applications that access the Azure File share using the standard System.IO classes for File I/O.</span></span> <span data-ttu-id="d5582-116">Den här artikeln visar hur du skriver program som använder Azure Storage .NET SDK, som använder [REST-API:et för Azure File Storage](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) för att kommunicera med Azure File Storage.</span><span class="sxs-lookup"><span data-stu-id="d5582-116">This article will describe how to write applications that use the Azure Storage .NET SDK, which uses the [Azure File storage REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) to talk to Azure File storage.</span></span> 


## <a name="create-the-console-application-and-obtain-the-assembly"></a><span data-ttu-id="d5582-117">Skapa konsolprogrammet och hämta monteringen</span><span class="sxs-lookup"><span data-stu-id="d5582-117">Create the console application and obtain the assembly</span></span>
<span data-ttu-id="d5582-118">Skapa ett nytt Windows-konsolprogram i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d5582-118">In Visual Studio, create a new Windows console application.</span></span> <span data-ttu-id="d5582-119">Följande steg beskriver hur du skapar ett konsolprogram i Visual Studio 2017, men stegen är liknande i andra versioner av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d5582-119">The following steps show you how to create a console application in Visual Studio 2017, however, the steps are similar in other versions of Visual Studio.</span></span>

1. <span data-ttu-id="d5582-120">Välj **Arkiv** > **Nytt** > **Projekt**</span><span class="sxs-lookup"><span data-stu-id="d5582-120">Select **File** > **New** > **Project**</span></span>
2. <span data-ttu-id="d5582-121">Välj **Installerat** > **Mallar** > **Visual C#** > **Windows Classic Desktop**</span><span class="sxs-lookup"><span data-stu-id="d5582-121">Select **Installed** > **Templates** > **Visual C#** > **Windows Classic Desktop**</span></span>
3. <span data-ttu-id="d5582-122">Välj **Konsolprogram (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="d5582-122">Select **Console App (.NET Framework)**</span></span>
4. <span data-ttu-id="d5582-123">Ange ett namn för ditt program i fältet **Namn**</span><span class="sxs-lookup"><span data-stu-id="d5582-123">Enter a name for your application in the **Name:** field</span></span>
5. <span data-ttu-id="d5582-124">Välj **OK**</span><span class="sxs-lookup"><span data-stu-id="d5582-124">Select **OK**</span></span>

<span data-ttu-id="d5582-125">Alla kodexempel i den här självstudiekursen kan läggas till i `Main()`-metoden i konsolprogrammets `Program.cs`-fil.</span><span class="sxs-lookup"><span data-stu-id="d5582-125">All code examples in this tutorial can be added to the `Main()` method of your console application's `Program.cs` file.</span></span>

<span data-ttu-id="d5582-126">Du kan använda Azure Storage-klientbiblioteket i alla typer av .NET-program, t.ex. en Azure-molntjänst eller webbapp, eller i dator- och mobilprogram.</span><span class="sxs-lookup"><span data-stu-id="d5582-126">You can use the Azure Storage Client Library in any type of .NET application, including an Azure cloud service or web app, and desktop and mobile applications.</span></span> <span data-ttu-id="d5582-127">I den här guiden använder vi oss av en konsolapp för enkelhetens skull.</span><span class="sxs-lookup"><span data-stu-id="d5582-127">In this guide, we use a console application for simplicity.</span></span>

## <a name="use-nuget-to-install-the-required-packages"></a><span data-ttu-id="d5582-128">Använd NuGet för att installera de paket som behövs</span><span class="sxs-lookup"><span data-stu-id="d5582-128">Use NuGet to install the required packages</span></span>
<span data-ttu-id="d5582-129">Det finns två paket som du måste referera till i ditt projekt för att slutföra den här kursen:</span><span class="sxs-lookup"><span data-stu-id="d5582-129">There are two packages you need to reference in your project to complete this tutorial:</span></span>

* <span data-ttu-id="d5582-130">[Microsoft Azure Storage-klientbibliotek för .NET](https://www.nuget.org/packages/WindowsAzure.Storage/): det här paketet ger programmatisk åtkomst till dataresurser i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="d5582-130">[Microsoft Azure Storage Client Library for .NET](https://www.nuget.org/packages/WindowsAzure.Storage/): This package provides programmatic access to data resources in your storage account.</span></span>
* <span data-ttu-id="d5582-131">[Microsoft Azure Configuration Manager-biblioteket för .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/): det här paketet tillhandahåller en klass för parsning av en anslutningssträng i en konfigurationsfil, oavsett var ditt program körs.</span><span class="sxs-lookup"><span data-stu-id="d5582-131">[Microsoft Azure Configuration Manager library for .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/): This package provides a class for parsing a connection string in a configuration file, regardless of where your application is running.</span></span>

<span data-ttu-id="d5582-132">Du kan använda NuGet för att hämta båda paketen.</span><span class="sxs-lookup"><span data-stu-id="d5582-132">You can use NuGet to obtain both packages.</span></span> <span data-ttu-id="d5582-133">Följ de här stegen:</span><span class="sxs-lookup"><span data-stu-id="d5582-133">Follow these steps:</span></span>

1. <span data-ttu-id="d5582-134">Högerklicka på ditt projekt i **Solution Explorer** och välj **Hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="d5582-134">Right-click your project in **Solution Explorer** and choose **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="d5582-135">Sök online efter ”WindowsAzure.Storage” och klicka på **Installera** för att installera Storage-klientbiblioteket och alla dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="d5582-135">Search online for "WindowsAzure.Storage" and click **Install** to install the Storage Client Library and its dependencies.</span></span>
3. <span data-ttu-id="d5582-136">Sök online efter ”WindowsAzure.ConfigurationManager” och klicka på **Installera** för att installera Azure Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="d5582-136">Search online for "WindowsAzure.ConfigurationManager" and click **Install** to install the Azure Configuration Manager.</span></span>

## <a name="save-your-storage-account-credentials-to-the-appconfig-file"></a><span data-ttu-id="d5582-137">Spara autentiseringsuppgifterna för ditt lagringskonto i app.config-filen</span><span class="sxs-lookup"><span data-stu-id="d5582-137">Save your storage account credentials to the app.config file</span></span>
<span data-ttu-id="d5582-138">Nu ska du spara dina autentiseringsuppgifter i projektets app.config-fil.</span><span class="sxs-lookup"><span data-stu-id="d5582-138">Next, save your credentials in your project's app.config file.</span></span> <span data-ttu-id="d5582-139">Redigera app.config-filen så att den ser ut som i följande exempel. Ersätt `myaccount` med namnet på ditt lagringskonto och `mykey` med din åtkomstnyckel för lagring.</span><span class="sxs-lookup"><span data-stu-id="d5582-139">Edit the app.config file so that it appears similar to the following example, replacing `myaccount` with your storage account name, and `mykey` with your storage account key.</span></span>

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
> Den senaste versionen av Azure Storage-emulatorn har inte stöd för Azure File Storage. <span data-ttu-id="d5582-141">Anslutningssträngen måste peka på ett Azure Storage-konto i molnet för att fungera med Azure File Storage.</span><span class="sxs-lookup"><span data-stu-id="d5582-141">Your connection string must target an Azure Storage Account in the cloud to work with Azure File storage.</span></span>

## <a name="add-using-directives"></a><span data-ttu-id="d5582-142">Lägga till med hjälp av direktiv</span><span class="sxs-lookup"><span data-stu-id="d5582-142">Add using directives</span></span>
<span data-ttu-id="d5582-143">Öppna filen `Program.cs` från Solution Explorer och lägg till följande med hjälp av direktiv överst i filen.</span><span class="sxs-lookup"><span data-stu-id="d5582-143">Open the `Program.cs` file from Solution Explorer, and add the following using directives to the top of the file.</span></span>

```csharp
using Microsoft.Azure; // Namespace for Azure Configuration Manager
using Microsoft.WindowsAzure.Storage; // Namespace for Storage Client Library
using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Azure Blobs
using Microsoft.WindowsAzure.Storage.File; // Namespace for Azure File storage
```

[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

## <a name="access-the-file-share-programmatically"></a><span data-ttu-id="d5582-144">Ansluta till filresursen via programmering</span><span class="sxs-lookup"><span data-stu-id="d5582-144">Access the file share programmatically</span></span>
<span data-ttu-id="d5582-145">Lägg till följande kod i `Main()`-metoden (efter koden som visas ovan) för att hämta anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="d5582-145">Next, add the following code to the `Main()` method (after the code shown above) to retrieve the connection string.</span></span> <span data-ttu-id="d5582-146">Den här koden hämtar en referens till den fil som vi skapade tidigare och returnerar filens innehåll i konsolfönstret.</span><span class="sxs-lookup"><span data-stu-id="d5582-146">This code gets a reference to the file we created earlier and outputs its contents to the console window.</span></span>

```csharp
// Create a CloudFileClient object for credentialed access to Azure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference to the file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that the share exists.
if (share.Exists())
{
    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();

    // Get a reference to the directory we created previously.
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");

    // Ensure that the directory exists.
    if (sampleDir.Exists())
    {
        // Get a reference to the file we created previously.
        CloudFile file = sampleDir.GetFileReference("Log1.txt");

        // Ensure that the file exists.
        if (file.Exists())
        {
            // Write the contents of the file to the console window.
            Console.WriteLine(file.DownloadTextAsync().Result);
        }
    }
}
```

<span data-ttu-id="d5582-147">Visa resultatet genom att köra konsolprogrammet.</span><span class="sxs-lookup"><span data-stu-id="d5582-147">Run the console application to see the output.</span></span>

## <a name="set-the-maximum-size-for-a-file-share"></a><span data-ttu-id="d5582-148">Ange den största storleken för en filresurs</span><span class="sxs-lookup"><span data-stu-id="d5582-148">Set the maximum size for a file share</span></span>
<span data-ttu-id="d5582-149">Från och med version 5.x av klientbiblioteket för Azure Storage kan du ange kvoten (eller den största storleken) för en filresurs, i antal gigabyte.</span><span class="sxs-lookup"><span data-stu-id="d5582-149">Beginning with version 5.x of the Azure Storage Client Library, you can set set the quota (or maximum size) for a file share, in gigabytes.</span></span> <span data-ttu-id="d5582-150">Du kan också kontrollera hur mycket data som lagras på resursen för närvarande.</span><span class="sxs-lookup"><span data-stu-id="d5582-150">You can also check to see how much data is currently stored on the share.</span></span>

<span data-ttu-id="d5582-151">Genom att ange kvoten för en resurs kan du begränsa den totala storleken på filerna som lagras på resursen.</span><span class="sxs-lookup"><span data-stu-id="d5582-151">By setting the quota for a share, you can limit the total size of the files stored on the share.</span></span> <span data-ttu-id="d5582-152">Om den totala storleken på filerna som lagras på resursen överskrider kvoten som angetts för resursen kan klienterna inte öka storleken på befintliga filer eller skapa nya filer såvida inte dessa filer är tomma.</span><span class="sxs-lookup"><span data-stu-id="d5582-152">If the total size of files on the share exceeds the quota set on the share, then clients will be unable to increase the size of existing files or create new files, unless those files are empty.</span></span>

<span data-ttu-id="d5582-153">Exemplet nedan visar hur du kontrollerar användningen av en resurs och hur du ställer in kvoten för resursen.</span><span class="sxs-lookup"><span data-stu-id="d5582-153">The example below shows how to check the current usage for a share and how to set the quota for the share.</span></span>

```csharp
// Parse the connection string for the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access to Azure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference to the file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that the share exists.
if (share.Exists())
{
    // Check current usage stats for the share.
    // Note that the ShareStats object is part of the protocol layer for the File service.
    Microsoft.WindowsAzure.Storage.File.Protocol.ShareStats stats = share.GetStats();
    Console.WriteLine("Current share usage: {0} GB", stats.Usage.ToString());

    // Specify the maximum size of the share, in GB.
    // This line sets the quota to be 10 GB greater than the current usage of the share.
    share.Properties.Quota = 10 + stats.Usage;
    share.SetProperties();

    // Now check the quota for the share. Call FetchAttributes() to populate the share's properties.
    share.FetchAttributes();
    Console.WriteLine("Current share quota: {0} GB", share.Properties.Quota);
}
```

### <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a><span data-ttu-id="d5582-154">Generera en signatur för delad åtkomst för en fil eller filresurs</span><span class="sxs-lookup"><span data-stu-id="d5582-154">Generate a shared access signature for a file or file share</span></span>
<span data-ttu-id="d5582-155">Från och med version 5.x av klientbiblioteket för Azure Storage kan du generera en signatur för delad åtkomst (SAS) för en filresurs eller för en enskild fil.</span><span class="sxs-lookup"><span data-stu-id="d5582-155">Beginning with version 5.x of the Azure Storage Client Library, you can generate a shared access signature (SAS) for a file share or for an individual file.</span></span> <span data-ttu-id="d5582-156">Du kan också skapa en princip för delad åtkomst på en filresurs för att hantera signaturer för delad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="d5582-156">You can also create a shared access policy on a file share to manage shared access signatures.</span></span> <span data-ttu-id="d5582-157">Vi rekommenderar att du skapar en princip för delad åtkomst eftersom det ger dig möjlighet att återkalla signaturen för delad åtkomst om det behövs.</span><span class="sxs-lookup"><span data-stu-id="d5582-157">Creating a shared access policy is recommended, as it provides a means of revoking the SAS if it should be compromised.</span></span>

<span data-ttu-id="d5582-158">I följande exempel skapar vi en princip för delad åtkomst på en resurs och använder sedan principen för att ange begränsningarna för en signatur för delad åtkomst på en fil i resursen.</span><span class="sxs-lookup"><span data-stu-id="d5582-158">The following example creates a shared access policy on a share, and then uses that policy to provide the constraints for a SAS on a file in the share.</span></span>

```csharp
// Parse the connection string for the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access to Azure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference to the file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that the share exists.
if (share.Exists())
{
    string policyName = "sampleSharePolicy" + DateTime.UtcNow.Ticks;

    // Create a new shared access policy and define its constraints.
    SharedAccessFilePolicy sharedPolicy = new SharedAccessFilePolicy()
        {
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessFilePermissions.Read | SharedAccessFilePermissions.Write
        };

    // Get existing permissions for the share.
    FileSharePermissions permissions = share.GetPermissions();

    // Add the shared access policy to the share's policies. Note that each policy must have a unique name.
    permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
    share.SetPermissions(permissions);

    // Generate a SAS for a file in the share and associate this access policy with it.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");
    CloudFile file = sampleDir.GetFileReference("Log1.txt");
    string sasToken = file.GetSharedAccessSignature(null, policyName);
    Uri fileSasUri = new Uri(file.StorageUri.PrimaryUri.ToString() + sasToken);

    // Create a new CloudFile object from the SAS, and write some text to the file.
    CloudFile fileSas = new CloudFile(fileSasUri);
    fileSas.UploadText("This write operation is authenticated via SAS.");
    Console.WriteLine(fileSas.DownloadText());
}
```

<span data-ttu-id="d5582-159">Mer information om hur du skapar och använder signaturer för delad åtkomst finns i [Använda signaturer för delad åtkomst (SAS)](storage-dotnet-shared-access-signature-part-1.md) och [Skapa och använda en SAS med Azure Blobs](storage-dotnet-shared-access-signature-part-2.md).</span><span class="sxs-lookup"><span data-stu-id="d5582-159">For more information about creating and using shared access signatures, see [Using Shared Access Signatures (SAS)](storage-dotnet-shared-access-signature-part-1.md) and [Create and use a SAS with Azure Blobs](storage-dotnet-shared-access-signature-part-2.md).</span></span>

## <a name="copy-files"></a><span data-ttu-id="d5582-160">Kopiera filer</span><span class="sxs-lookup"><span data-stu-id="d5582-160">Copy files</span></span>
<span data-ttu-id="d5582-161">Från och med version 5.x av klientbiblioteket för Azure Storage kan du kopiera en fil till en annan fil, en fil till en blobb eller en blobb till en fil.</span><span class="sxs-lookup"><span data-stu-id="d5582-161">Beginning with version 5.x of the Azure Storage Client Library, you can copy a file to another file, a file to a blob, or a blob to a file.</span></span> <span data-ttu-id="d5582-162">I nästa avsnitt ser du hur du utför kopieringsåtgärderna genom programmering.</span><span class="sxs-lookup"><span data-stu-id="d5582-162">In the next sections, we demonstrate how to perform these copy operations programmatically.</span></span>

<span data-ttu-id="d5582-163">Du kan också använda AzCopy för att kopiera en fil till en annan eller för att kopiera en blobb till en fil eller tvärtom.</span><span class="sxs-lookup"><span data-stu-id="d5582-163">You can also use AzCopy to copy one file to another or to copy a blob to a file or vice versa.</span></span> <span data-ttu-id="d5582-164">Mer information finns i [Överföra data med kommandoradsverktyget AzCopy](storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="d5582-164">See [Transfer data with the AzCopy Command-Line Utility](storage-use-azcopy.md).</span></span>

> [!NOTE]
> <span data-ttu-id="d5582-165">Om du kopierar en blobb till en fil eller en fil till en blobb måste du använda en signatur för delad åtkomst (SAS) för att autentisera källobjektet, även om du kopierar inom samma lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="d5582-165">If you are copying a blob to a file, or a file to a blob, you must use a shared access signature (SAS) to authenticate the source object, even if you are copying within the same storage account.</span></span>
> 
> 

<span data-ttu-id="d5582-166">**Kopiera en fil till en annan fil** I följande exempel kopierar vi en fil till en annan fil i samma resurs.</span><span class="sxs-lookup"><span data-stu-id="d5582-166">**Copy a file to another file** The following example copies a file to another file in the same share.</span></span> <span data-ttu-id="d5582-167">Eftersom den här kopieringsåtgärden kopierar mellan filer i samma lagringskonto kan du använda autentisering med delad nyckel för att utföra kopieringen.</span><span class="sxs-lookup"><span data-stu-id="d5582-167">Because this copy operation copies between files in the same storage account, you can use Shared Key authentication to perform the copy.</span></span>

```csharp
// Parse the connection string for the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access to Azure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference to the file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that the share exists.
if (share.Exists())
{
    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();

    // Get a reference to the directory we created previously.
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");

    // Ensure that the directory exists.
    if (sampleDir.Exists())
    {
        // Get a reference to the file we created previously.
        CloudFile sourceFile = sampleDir.GetFileReference("Log1.txt");

        // Ensure that the source file exists.
        if (sourceFile.Exists())
        {
            // Get a reference to the destination file.
            CloudFile destFile = sampleDir.GetFileReference("Log1Copy.txt");

            // Start the copy operation.
            destFile.StartCopy(sourceFile);

            // Write the contents of the destination file to the console window.
            Console.WriteLine(destFile.DownloadText());
        }
    }
}
```

<span data-ttu-id="d5582-168">**Kopiera en fil till en blob** I följande exempel skapar vi en fil och kopierar den till en blob inom samma lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="d5582-168">**Copy a file to a blob** The following example creates a file and copies it to a blob within the same storage account.</span></span> <span data-ttu-id="d5582-169">I exemplet skapas en SAS för källfilen, som tjänsten använder för att autentisera åtkomsten till källfilen under kopieringen.</span><span class="sxs-lookup"><span data-stu-id="d5582-169">The example creates a SAS for the source file, which the service uses to authenticate access to the source file during the copy operation.</span></span>

```csharp
// Parse the connection string for the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access to Azure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Create a new file share, if it does not already exist.
CloudFileShare share = fileClient.GetShareReference("sample-share");
share.CreateIfNotExists();

// Create a new file in the root directory.
CloudFile sourceFile = share.GetRootDirectoryReference().GetFileReference("sample-file.txt");
sourceFile.UploadText("A sample file in the root directory.");

// Get a reference to the blob to which the file will be copied.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
container.CreateIfNotExists();
CloudBlockBlob destBlob = container.GetBlockBlobReference("sample-blob.txt");

// Create a SAS for the file that's valid for 24 hours.
// Note that when you are copying a file to a blob, or a blob to a file, you must use a SAS
// to authenticate access to the source object, even if you are copying within the same
// storage account.
string fileSas = sourceFile.GetSharedAccessSignature(new SharedAccessFilePolicy()
{
    // Only read permissions are required for the source file.
    Permissions = SharedAccessFilePermissions.Read,
    SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24)
});

// Construct the URI to the source file, including the SAS token.
Uri fileSasUri = new Uri(sourceFile.StorageUri.PrimaryUri.ToString() + fileSas);

// Copy the file to the blob.
destBlob.StartCopy(fileSasUri);

// Write the contents of the file to the console window.
Console.WriteLine("Source file contents: {0}", sourceFile.DownloadText());
Console.WriteLine("Destination blob contents: {0}", destBlob.DownloadText());
```

<span data-ttu-id="d5582-170">Du kan kopiera en blobb till en fil på samma sätt.</span><span class="sxs-lookup"><span data-stu-id="d5582-170">You can copy a blob to a file in the same way.</span></span> <span data-ttu-id="d5582-171">Om källobjektet är en blobb skapar du en SAS för att autentisera åtkomsten till blobben under kopieringen.</span><span class="sxs-lookup"><span data-stu-id="d5582-171">If the source object is a blob, then create a SAS to authenticate access to that blob during the copy operation.</span></span>

## <a name="troubleshooting-azure-file-storage-using-metrics"></a><span data-ttu-id="d5582-172">Felsöka Azure File Storage med hjälp av mätvärden</span><span class="sxs-lookup"><span data-stu-id="d5582-172">Troubleshooting Azure File storage using metrics</span></span>
<span data-ttu-id="d5582-173">Nu stöder Azure Storage Analytics mätvärden för Azure File Storage.</span><span class="sxs-lookup"><span data-stu-id="d5582-173">Azure Storage Analytics now supports metrics for Azure File storage.</span></span> <span data-ttu-id="d5582-174">Med hjälp av mätvärdesdata kan du spåra begäranden och diagnostisera problem.</span><span class="sxs-lookup"><span data-stu-id="d5582-174">With metrics data, you can trace requests and diagnose issues.</span></span>


<span data-ttu-id="d5582-175">Du kan aktivera mätvärden för Azure File Storage från [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d5582-175">You can enable metrics for Azure File storage from the [Azure Portal](https://portal.azure.com).</span></span> <span data-ttu-id="d5582-176">Du kan också aktivera mätvärden via programmering genom att anropa åtgärden Ange egenskaper för filtjänsten via REST-API:et eller någon av dess motsvarigheter i klientbiblioteket för Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="d5582-176">You can also enable metrics programmatically by calling the Set File Service Properties operation via the REST API, or one of its analogues in the Storage Client Library.</span></span>


<span data-ttu-id="d5582-177">Följande exempel visar hur du använder Azure Storage-klientbiblioteket för .NET för att aktivera mätvärden för Azure File Storage.</span><span class="sxs-lookup"><span data-stu-id="d5582-177">The following code example shows how to use the Storage Client Library for .NET to enable metrics for Azure File storage.</span></span>

<span data-ttu-id="d5582-178">Lägg först till följande `using`-direktiv i `Program.cs`-filen, förutom de som du lade till ovan:</span><span class="sxs-lookup"><span data-stu-id="d5582-178">First, add the following `using` directives to your `Program.cs` file, in addition to those you added above:</span></span>

```csharp
using Microsoft.WindowsAzure.Storage.File.Protocol;
using Microsoft.WindowsAzure.Storage.Shared.Protocol;
```

<span data-ttu-id="d5582-179">Observera att Azure Blobs, Azure Table och Azure Queues använder den delade `ServiceProperties`-typen i namnrymden `Microsoft.WindowsAzure.Storage.Shared.Protocol` och att Azure File Storage använder en egen typ, det vill säga typen `FileServiceProperties` i namnrymden `Microsoft.WindowsAzure.Storage.File.Protocol`.</span><span class="sxs-lookup"><span data-stu-id="d5582-179">Note that while Azure Blobs, Azure Table, and Azure Queues use the shared `ServiceProperties` type in the `Microsoft.WindowsAzure.Storage.Shared.Protocol` namespace, Azure File storage uses its own type, the `FileServiceProperties` type in the `Microsoft.WindowsAzure.Storage.File.Protocol` namespace.</span></span> <span data-ttu-id="d5582-180">Din kod måste dock referera till båda namnrymderna för att följande kod ska kompileras.</span><span class="sxs-lookup"><span data-stu-id="d5582-180">Both namespaces must be referenced from your code, however, for the following code to compile.</span></span>

```csharp
// Parse your storage connection string from your application's configuration file.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));
// Create the File service client.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Set metrics properties for File service.
// Note that the File service currently uses its own service properties type,
// available in the Microsoft.WindowsAzure.Storage.File.Protocol namespace.
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

// Read the metrics properties we just set.
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

<span data-ttu-id="d5582-181">Du kan också gå till [Felsökningsartikeln om Azure File Storage](storage-troubleshoot-file-connection-problems.md) för felsökningsinformation från slutpunkt till slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="d5582-181">Also, you can refer to [Azure File storage Troubleshooting Article](storage-troubleshoot-file-connection-problems.md) for end-to-end troubleshooting guidance.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d5582-182">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d5582-182">Next steps</span></span>
<span data-ttu-id="d5582-183">Mer information om Azure File Storage finns på följande länkar.</span><span class="sxs-lookup"><span data-stu-id="d5582-183">See these links for more information about Azure File storage.</span></span>

### <a name="conceptual-articles-and-videos"></a><span data-ttu-id="d5582-184">Begreppsrelaterade artiklar och videoklipp</span><span class="sxs-lookup"><span data-stu-id="d5582-184">Conceptual articles and videos</span></span>
* [<span data-ttu-id="d5582-185">Azure Files Storage: ett friktionslöst SMB-filsystem i molnet för Windows och Linux</span><span class="sxs-lookup"><span data-stu-id="d5582-185">Azure File storage: a frictionless cloud SMB file system for Windows and Linux</span></span>](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [<span data-ttu-id="d5582-186">Använd Azure File Storage med Linux</span><span class="sxs-lookup"><span data-stu-id="d5582-186">How to use Azure File storage with Linux</span></span>](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-file-storage"></a><span data-ttu-id="d5582-187">Verktygsstöd för File Storage</span><span class="sxs-lookup"><span data-stu-id="d5582-187">Tooling support for File storage</span></span>
* [<span data-ttu-id="d5582-188">Använd Azure PowerShell med Azure Storage</span><span class="sxs-lookup"><span data-stu-id="d5582-188">Using Azure PowerShell with Azure Storage</span></span>](storage-powershell-guide-full.md)
* [<span data-ttu-id="d5582-189">Använd AzCopy med Microsoft Azure Storage</span><span class="sxs-lookup"><span data-stu-id="d5582-189">How to use AzCopy with Microsoft Azure Storage</span></span>](storage-use-azcopy.md)
* [<span data-ttu-id="d5582-190">Använd Azure CLI:et med Azure Storage</span><span class="sxs-lookup"><span data-stu-id="d5582-190">Using the Azure CLI with Azure Storage</span></span>](storage-azure-cli.md#create-and-manage-file-shares)
* [<span data-ttu-id="d5582-191">Felsökning av problem i Azure File Storage</span><span class="sxs-lookup"><span data-stu-id="d5582-191">Troubleshooting Azure File storage problems</span></span>](https://docs.microsoft.com/azure/storage/storage-troubleshoot-file-connection-problems)

### <a name="reference"></a><span data-ttu-id="d5582-192">Referens</span><span class="sxs-lookup"><span data-stu-id="d5582-192">Reference</span></span>
* [<span data-ttu-id="d5582-193">Storage-klientbibliotek för .NET-referens</span><span class="sxs-lookup"><span data-stu-id="d5582-193">Storage Client Library for .NET reference</span></span>](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [<span data-ttu-id="d5582-194">File Service REST API referens</span><span class="sxs-lookup"><span data-stu-id="d5582-194">File Service REST API reference</span></span>](http://msdn.microsoft.com/library/azure/dn167006.aspx)

### <a name="blog-posts"></a><span data-ttu-id="d5582-195">Blogginlägg</span><span class="sxs-lookup"><span data-stu-id="d5582-195">Blog posts</span></span>
* [<span data-ttu-id="d5582-196">Azure File Storage finns nu allmänt tillgänglig</span><span class="sxs-lookup"><span data-stu-id="d5582-196">Azure File storage is now generally available</span></span>](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [<span data-ttu-id="d5582-197">Inuti Azure File Storage</span><span class="sxs-lookup"><span data-stu-id="d5582-197">Inside Azure File storage</span></span>](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [<span data-ttu-id="d5582-198">Introduktion till Microsoft Azure File Service</span><span class="sxs-lookup"><span data-stu-id="d5582-198">Introducing Microsoft Azure File Service</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [<span data-ttu-id="d5582-199">Bevara anslutningar till Microsoft Azure File Storage</span><span class="sxs-lookup"><span data-stu-id="d5582-199">Persisting connections to Microsoft Azure File storage</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx)
---
title: "Överföra Data med Microsoft Azure Storage Data Movement Library | Microsoft Docs"
description: "Använd Data Movement Library att flytta eller kopiera data till och från blob- och innehåll. Kopiera data till Azure Storage från lokala filer eller kopiera data inom eller mellan lagringskonton. Enkelt migrera dina data till Azure Storage."
services: storage
documentationcenter: 
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/22/2017
ms.author: seguler
ms.openlocfilehash: 7db1761a9a3b8a74a39b2d441849fb89d44cd42b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="transfer-data-with-the-microsoft-azure-storage-data-movement-library"></a><span data-ttu-id="199b8-105">Överföra Data med Microsoft Azure Storage Data Movement Library</span><span class="sxs-lookup"><span data-stu-id="199b8-105">Transfer Data with the Microsoft Azure Storage Data Movement Library</span></span>

## <a name="overview"></a><span data-ttu-id="199b8-106">Översikt</span><span class="sxs-lookup"><span data-stu-id="199b8-106">Overview</span></span>
<span data-ttu-id="199b8-107">Microsoft Azure Storage Data Movement Library är ett bibliotek med öppen källkod för flera plattformar som är utformat för högpresterande överföringen, hämtar och kopiering av Azure Storage-Blobbar och filer.</span><span class="sxs-lookup"><span data-stu-id="199b8-107">The Microsoft Azure Storage Data Movement Library is a cross-platform open source library that is designed for high performance uploading, downloading, and copying of Azure Storage Blobs and Files.</span></span> <span data-ttu-id="199b8-108">Det här biblioteket är core data movement framework som används av [AzCopy](../storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="199b8-108">This library is the core data movement framework that powers [AzCopy](../storage-use-azcopy.md).</span></span> <span data-ttu-id="199b8-109">Data Movement Library ger praktiska metoder som inte är tillgängliga i vår traditionella [.NET Azure Storage-klientbibliotek](../blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="199b8-109">The Data Movement Library provides convenient methods that aren't available in our traditional [.NET Azure Storage Client Library](../blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="199b8-110">Detta inkluderar möjligheten att ange antalet parallella åtgärder, spåra Överföringsförlopp, enkelt återuppta en annullerad överföring och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="199b8-110">This includes the ability to set the number of parallel operations, track transfer progress, easily resume a canceled transfer, and much more.</span></span>  

<span data-ttu-id="199b8-111">.NET Core, vilket innebär att du kan använda den när du skapar .NET appar för Windows, Linux och macOS används också i det här biblioteket.</span><span class="sxs-lookup"><span data-stu-id="199b8-111">This library also uses .NET Core, which means you can use it when building .NET apps for Windows, Linux and macOS.</span></span> <span data-ttu-id="199b8-112">Om du vill veta mer om .NET Core kan referera till den [.NET Core-dokumentation](https://dotnet.github.io/).</span><span class="sxs-lookup"><span data-stu-id="199b8-112">To learn more about .NET Core, refer to the [.NET Core documentation](https://dotnet.github.io/).</span></span> <span data-ttu-id="199b8-113">Det här biblioteket fungerar även för traditionella .NET Framework-appar för Windows.</span><span class="sxs-lookup"><span data-stu-id="199b8-113">This library also works for traditional .NET Framework apps for Windows.</span></span> 

<span data-ttu-id="199b8-114">Det här dokumentet visar hur du skapar ett .NET Core-konsolprogram som som körs på Windows, Linux och macOS och utför följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="199b8-114">This document demonstrates how to create a .NET Core console application that that runs on Windows, Linux, and macOS and performs the following scenarios:</span></span>

- <span data-ttu-id="199b8-115">Ladda upp filer och kataloger till Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="199b8-115">Upload files and directories to Blob Storage.</span></span>
- <span data-ttu-id="199b8-116">Definiera antalet parallella åtgärder vid överföring av data.</span><span class="sxs-lookup"><span data-stu-id="199b8-116">Define the number of parallel operations when transferring data.</span></span>
- <span data-ttu-id="199b8-117">Spåra förloppet för data transfer.</span><span class="sxs-lookup"><span data-stu-id="199b8-117">Track data transfer progress.</span></span>
- <span data-ttu-id="199b8-118">Återuppta avbrutna dataöverföring.</span><span class="sxs-lookup"><span data-stu-id="199b8-118">Resume canceled data transfer.</span></span> 
- <span data-ttu-id="199b8-119">Kopiera filen från URL: en till Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="199b8-119">Copy file from URL to Blob Storage.</span></span> 
- <span data-ttu-id="199b8-120">Kopiera från Blob Storage till Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="199b8-120">Copy from Blob Storage to Blob Storage.</span></span>

<span data-ttu-id="199b8-121">**Vad du behöver:**</span><span class="sxs-lookup"><span data-stu-id="199b8-121">**What you need:**</span></span>

* [<span data-ttu-id="199b8-122">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="199b8-122">Visual Studio Code</span></span>](https://code.visualstudio.com/)
* <span data-ttu-id="199b8-123">Ett [Azure Storage-konto](storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="199b8-123">An [Azure storage account](storage-create-storage-account.md#create-a-storage-account)</span></span>

> [!NOTE]
> <span data-ttu-id="199b8-124">Den här handboken förutsätts att du redan är bekant med [Azure Storage](https://azure.microsoft.com/services/storage/).</span><span class="sxs-lookup"><span data-stu-id="199b8-124">This guide assumes that you are already familiar with [Azure Storage](https://azure.microsoft.com/services/storage/).</span></span> <span data-ttu-id="199b8-125">Om inte, läser den [introduktion till Azure Storage](storage-introduction.md) dokumentation är bra.</span><span class="sxs-lookup"><span data-stu-id="199b8-125">If not, reading the [Introduction to Azure Storage](storage-introduction.md) documentation is helpful.</span></span> <span data-ttu-id="199b8-126">Viktigast av allt du behöver [skapa ett lagringskonto](storage-create-storage-account.md#create-a-storage-account) att börja använda Data Movement Library.</span><span class="sxs-lookup"><span data-stu-id="199b8-126">Most importantly, you need to [create a Storage account](storage-create-storage-account.md#create-a-storage-account) to start using the Data Movement Library.</span></span>
> 
> 

## <a name="setup"></a><span data-ttu-id="199b8-127">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="199b8-127">Setup</span></span>  

1. <span data-ttu-id="199b8-128">Besök den [.NET Core installationsguiden](https://www.microsoft.com/net/core) att installera .NET Core.</span><span class="sxs-lookup"><span data-stu-id="199b8-128">Visit the [.NET Core Installation Guide](https://www.microsoft.com/net/core) to install .NET Core.</span></span> <span data-ttu-id="199b8-129">När du väljer din miljö, väljer du kommandoradsalternativet.</span><span class="sxs-lookup"><span data-stu-id="199b8-129">When selecting your environment, choose the command-line option.</span></span> 
2. <span data-ttu-id="199b8-130">Skapa en katalog för ditt projekt från kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="199b8-130">From the command line, create a directory for your project.</span></span> <span data-ttu-id="199b8-131">Navigera till den här katalogen, Skriv `dotnet new` att skapa en C# console-projekt.</span><span class="sxs-lookup"><span data-stu-id="199b8-131">Navigate into this directory, then type `dotnet new` to create a C# console project.</span></span>
3. <span data-ttu-id="199b8-132">Öppna den här katalogen i Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="199b8-132">Open this directory in Visual Studio Code.</span></span> <span data-ttu-id="199b8-133">Det här steget kan utföras snabbt via kommandoraden genom att skriva `code .`.</span><span class="sxs-lookup"><span data-stu-id="199b8-133">This step can be quickly done via the command line by typing `code .`.</span></span>  
4. <span data-ttu-id="199b8-134">Installera den [C#-tillägget](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) från Visual Studio Code Marketplace.</span><span class="sxs-lookup"><span data-stu-id="199b8-134">Install the [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) from the Visual Studio Code Marketplace.</span></span> <span data-ttu-id="199b8-135">Starta om Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="199b8-135">Restart Visual Studio Code.</span></span> 
5. <span data-ttu-id="199b8-136">Du bör nu se två frågor.</span><span class="sxs-lookup"><span data-stu-id="199b8-136">At this point, you should see two prompts.</span></span> <span data-ttu-id="199b8-137">En är för att lägga till ”nödvändiga resurser för att bygga och debug”.</span><span class="sxs-lookup"><span data-stu-id="199b8-137">One is for adding "required assets to build and debug."</span></span> <span data-ttu-id="199b8-138">Klicka på ”Ja”.</span><span class="sxs-lookup"><span data-stu-id="199b8-138">Click "yes."</span></span> <span data-ttu-id="199b8-139">En annan frågan är för att återställa Olösta beroenden.</span><span class="sxs-lookup"><span data-stu-id="199b8-139">Another prompt is for restoring unresolved dependencies.</span></span> <span data-ttu-id="199b8-140">Klicka på ”Återställ”.</span><span class="sxs-lookup"><span data-stu-id="199b8-140">Click "restore."</span></span>
6. <span data-ttu-id="199b8-141">Programmet innehåller nu en `launch.json` filen den `.vscode` directory.</span><span class="sxs-lookup"><span data-stu-id="199b8-141">Your application should now contain a `launch.json` file under the `.vscode` directory.</span></span> <span data-ttu-id="199b8-142">I den här filen, ändrar du den `externalConsole` värde till `true`.</span><span class="sxs-lookup"><span data-stu-id="199b8-142">In this file, change the `externalConsole` value to `true`.</span></span>
7. <span data-ttu-id="199b8-143">Visual Studio Code kan du felsöka .NET Core-program.</span><span class="sxs-lookup"><span data-stu-id="199b8-143">Visual Studio Code allows you to debug .NET Core applications.</span></span> <span data-ttu-id="199b8-144">Träffa `F5` att köra ditt program och kontrollera att inställningarna fungerar.</span><span class="sxs-lookup"><span data-stu-id="199b8-144">Hit `F5` to run your application and verify that your setup is working.</span></span> <span data-ttu-id="199b8-145">Du bör se ”Hello World”!</span><span class="sxs-lookup"><span data-stu-id="199b8-145">You should see "Hello World!"</span></span> <span data-ttu-id="199b8-146">ut till konsolen.</span><span class="sxs-lookup"><span data-stu-id="199b8-146">printed to the console.</span></span> 

## <a name="add-data-movement-library-to-your-project"></a><span data-ttu-id="199b8-147">Lägg till Data Movement Library i projektet</span><span class="sxs-lookup"><span data-stu-id="199b8-147">Add Data Movement Library to your project</span></span>

1. <span data-ttu-id="199b8-148">Lägg till den senaste versionen av Data Movement Library till den `dependencies` avsnitt i din `project.json` fil.</span><span class="sxs-lookup"><span data-stu-id="199b8-148">Add the latest version of the Data Movement Library to the `dependencies` section of your `project.json` file.</span></span> <span data-ttu-id="199b8-149">Den här versionen är vid tidpunkten för skrivning`"Microsoft.Azure.Storage.DataMovement": "0.5.0"`</span><span class="sxs-lookup"><span data-stu-id="199b8-149">At the time of writing, this version would be `"Microsoft.Azure.Storage.DataMovement": "0.5.0"`</span></span> 
2. <span data-ttu-id="199b8-150">Lägg till `"portable-net45+win8"` till den `imports` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="199b8-150">Add `"portable-net45+win8"` to the `imports` section.</span></span> 
3. <span data-ttu-id="199b8-151">En uppmaning visas om du vill återställa ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="199b8-151">A prompt should display to restore your project.</span></span> <span data-ttu-id="199b8-152">Klicka på återställningsknappen ””.</span><span class="sxs-lookup"><span data-stu-id="199b8-152">Click the "restore" button.</span></span> <span data-ttu-id="199b8-153">Du kan även återställa projektet från kommandoraden genom att skriva kommandot `dotnet restore` i roten av projektkatalogen.</span><span class="sxs-lookup"><span data-stu-id="199b8-153">You can also restore your project from the command line by typing the command `dotnet restore` in the root of your project directory.</span></span>

<span data-ttu-id="199b8-154">Ändra `project.json`:</span><span class="sxs-lookup"><span data-stu-id="199b8-154">Modify `project.json`:</span></span>

    {
      "version": "1.0.0-*",
      "buildOptions": {
        "debugType": "portable",
        "emitEntryPoint": true
      },
      "dependencies": {
        "Microsoft.Azure.Storage.DataMovement": "0.5.0"
      },
      "frameworks": {
        "netcoreapp1.1": {
          "dependencies": {
            "Microsoft.NETCore.App": {
              "type": "platform",
              "version": "1.1.0"
            }
          },
          "imports": [
            "dnxcore50",
            "portable-net45+win8"
          ]
        }
      }
    }

## <a name="set-up-the-skeleton-of-your-application"></a><span data-ttu-id="199b8-155">Ställ in stommen i ditt program</span><span class="sxs-lookup"><span data-stu-id="199b8-155">Set up the skeleton of your application</span></span>
<span data-ttu-id="199b8-156">Det första vi gör ställs in ”stommen” koden för vårt program.</span><span class="sxs-lookup"><span data-stu-id="199b8-156">The first thing we do is set up the "skeleton" code of our application.</span></span> <span data-ttu-id="199b8-157">Den här koden oss uppmanas att ange ett namn och lagringskontonyckel och använder dessa autentiseringsuppgifter för att skapa en `CloudStorageAccount` objekt.</span><span class="sxs-lookup"><span data-stu-id="199b8-157">This code prompts us for a Storage account name and account key and uses those credentials to create a `CloudStorageAccount` object.</span></span> <span data-ttu-id="199b8-158">Det här objektet används för att interagera med våra Storage-konto i alla scenarier för överföring.</span><span class="sxs-lookup"><span data-stu-id="199b8-158">This object is used to interact with our Storage account in all transfer scenarios.</span></span> <span data-ttu-id="199b8-159">Koden uppmanar oss att välja vilken typ av överföringen som vi vill köra.</span><span class="sxs-lookup"><span data-stu-id="199b8-159">The code also prompts us to choose the type of transfer operation we would like to execute.</span></span> 

<span data-ttu-id="199b8-160">Ändra `Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="199b8-160">Modify `Program.cs`:</span></span>

```csharp
using System;
using System.Threading;
using System.Diagnostics;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.WindowsAzure.Storage.DataMovement;

namespace DMLibSample
{
    public class Program
    {
        public static void Main()
        {
            Console.WriteLine("Enter Storage account name:");           
            string accountName = Console.ReadLine();

            Console.WriteLine("\nEnter Storage account key:");           
            string accountKey = Console.ReadLine();

            string storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=" + accountName + ";AccountKey=" + accountKey;
            CloudStorageAccount account = CloudStorageAccount.Parse(storageConnectionString);

            ExecuteChoice(account);
        }

        public static void ExecuteChoice(CloudStorageAccount account)
        {
            Console.WriteLine("\nWhat type of transfer would you like to execute?\n1. Local file --> Azure Blob\n2. Local directory --> Azure Blob directory\n3. URL (e.g. Amazon S3 file) --> Azure Blob\n4. Azure Blob --> Azure Blob");
            int choice = int.Parse(Console.ReadLine());

            if(choice == 1)
            {
                TransferLocalFileToAzureBlob(account).Wait();
            }
            else if(choice == 2)
            {
                TransferLocalDirectoryToAzureBlobDirectory(account).Wait();
            }
            else if(choice == 3)
            {
                TransferUrlToAzureBlob(account).Wait();
            }
            else if(choice == 4)
            {
                TransferAzureBlobToAzureBlob(account).Wait();
            }
        }

        public static async Task TransferLocalFileToAzureBlob(CloudStorageAccount account)
        { 
            
        }

        public static async Task TransferLocalDirectoryToAzureBlobDirectory(CloudStorageAccount account)
        { 
            
        }

        public static async Task TransferUrlToAzureBlob(CloudStorageAccount account)
        {

        }

        public static async Task TransferAzureBlobToAzureBlob(CloudStorageAccount account)
        {

        }
    }
}
```

## <a name="transfer-local-file-to-azure-blob"></a><span data-ttu-id="199b8-161">Överför lokal fil till Azure-Blob</span><span class="sxs-lookup"><span data-stu-id="199b8-161">Transfer local file to Azure Blob</span></span>
<span data-ttu-id="199b8-162">Lägg till metoder `GetSourcePath` och `GetBlob` till `Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="199b8-162">Add the methods `GetSourcePath` and `GetBlob` to `Program.cs`:</span></span>

```csharp
public static string GetSourcePath()
{
    Console.WriteLine("\nProvide path for source:");
    string sourcePath = Console.ReadLine();

    return sourcePath;
}

public static CloudBlockBlob GetBlob(CloudStorageAccount account)
{
    CloudBlobClient blobClient = account.CreateCloudBlobClient();

    Console.WriteLine("\nProvide name of Blob container:");
    string containerName = Console.ReadLine();
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);
    container.CreateIfNotExistsAsync().Wait();

    Console.WriteLine("\nProvide name of new Blob:");
    string blobName = Console.ReadLine();
    CloudBlockBlob blob = container.GetBlockBlobReference(blobName);

    return blob;
}
```

<span data-ttu-id="199b8-163">Ändra den `TransferLocalFileToAzureBlob` metoden:</span><span class="sxs-lookup"><span data-stu-id="199b8-163">Modify the `TransferLocalFileToAzureBlob` method:</span></span>

```csharp
public static async Task TransferLocalFileToAzureBlob(CloudStorageAccount account)
{ 
    string localFilePath = GetSourcePath();
    CloudBlockBlob blob = GetBlob(account);
    Console.WriteLine("\nTransfer started...");
    await TransferManager.UploadAsync(localFilePath, blob);
    Console.WriteLine("\nTransfer operation complete.");
    ExecuteChoice(account);
}
```

<span data-ttu-id="199b8-164">Den här koden uppmanar oss att sökvägen till en lokal fil, namnet på en ny eller befintlig behållare och namnet på en ny blob.</span><span class="sxs-lookup"><span data-stu-id="199b8-164">This code prompts us for the path to a local file, the name of a new or existing container, and the name of a new blob.</span></span> <span data-ttu-id="199b8-165">Den `TransferManager.UploadAsync` metoden utför uppladdningen med den här informationen.</span><span class="sxs-lookup"><span data-stu-id="199b8-165">The `TransferManager.UploadAsync` method performs the upload using this information.</span></span> 

<span data-ttu-id="199b8-166">Träffa `F5` att köra programmet.</span><span class="sxs-lookup"><span data-stu-id="199b8-166">Hit `F5` to run your application.</span></span> <span data-ttu-id="199b8-167">Du kan kontrollera att överföringen uppstod genom att visa ditt lagringskonto med den [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="199b8-167">You can verify that the upload occurred by viewing your Storage account with the [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span></span>

## <a name="set-number-of-parallel-operations"></a><span data-ttu-id="199b8-168">Ange antalet parallella åtgärder</span><span class="sxs-lookup"><span data-stu-id="199b8-168">Set number of parallel operations</span></span>
<span data-ttu-id="199b8-169">En bra funktion som erbjuds av Data Movement Library är möjligheten att ange antalet parallella åtgärder för att öka genomflödet data transfer.</span><span class="sxs-lookup"><span data-stu-id="199b8-169">A great feature offered by the Data Movement Library is the ability to set the number of parallel operations to increase the data transfer throughput.</span></span> <span data-ttu-id="199b8-170">Som standard Data Movement Library anger antalet parallella åtgärder till 8 * antal kärnor på datorn.</span><span class="sxs-lookup"><span data-stu-id="199b8-170">By default, the Data Movement Library sets the number of parallel operations to 8 * the number of cores on your machine.</span></span> 

<span data-ttu-id="199b8-171">Tänk på att många parallella åtgärder i en miljö med låg bandbredd kan överväldigande nätverksanslutningen och faktiskt förhindra operations fullständigt slutförs.</span><span class="sxs-lookup"><span data-stu-id="199b8-171">Keep in mind that many parallel operations in a low-bandwidth environment may overwhelm the network connection and actually prevent operations from fully completing.</span></span> <span data-ttu-id="199b8-172">Du behöver experimentera med den här inställningen för att avgöra vad som fungerar bäst baserat på din tillgängliga nätverksbandbredd.</span><span class="sxs-lookup"><span data-stu-id="199b8-172">You'll need to experiment with this setting to determine what works best based on your available network bandwidth.</span></span> 

<span data-ttu-id="199b8-173">Lägg till kod som gör att vi kan ange antalet parallella åtgärder.</span><span class="sxs-lookup"><span data-stu-id="199b8-173">Let's add some code that allows us to set the number of parallel operations.</span></span> <span data-ttu-id="199b8-174">Nu ska vi också lägga till kod som gånger hur lång tid det tar att slutföra överföringen.</span><span class="sxs-lookup"><span data-stu-id="199b8-174">Let's also add code that times how long it takes for the transfer to complete.</span></span>

<span data-ttu-id="199b8-175">Lägg till en `SetNumberOfParallelOperations` metod för att `Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="199b8-175">Add a `SetNumberOfParallelOperations` method to `Program.cs`:</span></span>

```csharp
public static void SetNumberOfParallelOperations()
{
    Console.WriteLine("\nHow many parallel operations would you like to use?");
    string parallelOperations = Console.ReadLine();
    TransferManager.Configurations.ParallelOperations = int.Parse(parallelOperations);
}
```

<span data-ttu-id="199b8-176">Ändra den `ExecuteChoice` metod du vill använda `SetNumberOfParallelOperations`:</span><span class="sxs-lookup"><span data-stu-id="199b8-176">Modify the `ExecuteChoice` method to use `SetNumberOfParallelOperations`:</span></span>

```csharp
public static void ExecuteChoice(CloudStorageAccount account)
{
    Console.WriteLine("\nWhat type of transfer would you like to execute?\n1. Local file --> Azure Blob\n2. Local directory --> Azure Blob directory\n3. URL (e.g. Amazon S3 file) --> Azure Blob\n4. Azure Blob --> Azure Blob");
    int choice = int.Parse(Console.ReadLine());

    SetNumberOfParallelOperations();

    if(choice == 1)
    {
        TransferLocalFileToAzureBlob(account).Wait();
    }
    else if(choice == 2)
    {
        TransferLocalDirectoryToAzureBlobDirectory(account).Wait();
    }
    else if(choice == 3)
    {
        TransferUrlToAzureBlob(account).Wait();
    }
    else if(choice == 4)
    {
        TransferAzureBlobToAzureBlob(account).Wait();
    }
}
```

<span data-ttu-id="199b8-177">Ändra den `TransferLocalFileToAzureBlob` metod du vill använda en timer:</span><span class="sxs-lookup"><span data-stu-id="199b8-177">Modify the `TransferLocalFileToAzureBlob` method to use a timer:</span></span>

```csharp
public static async Task TransferLocalFileToAzureBlob(CloudStorageAccount account)
{ 
    string localFilePath = GetSourcePath();
    CloudBlockBlob blob = GetBlob(account);
    Console.WriteLine("\nTransfer started...");
    Stopwatch stopWatch = Stopwatch.StartNew();
    await TransferManager.UploadAsync(localFilePath, blob);
    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

## <a name="track-transfer-progress"></a><span data-ttu-id="199b8-178">Spåra förloppet för överföring</span><span class="sxs-lookup"><span data-stu-id="199b8-178">Track transfer progress</span></span>
<span data-ttu-id="199b8-179">Det är bra att känna till hur lång tid det tog för våra data att överföra.</span><span class="sxs-lookup"><span data-stu-id="199b8-179">Knowing how long it took for our data to transfer is great.</span></span> <span data-ttu-id="199b8-180">Dock att du kan se förloppet för vår överföring *under* överföringen blir ännu bättre.</span><span class="sxs-lookup"><span data-stu-id="199b8-180">However, being able to see the progress of our transfer *during* the transfer operation would be even better.</span></span> <span data-ttu-id="199b8-181">För att uppnå det här scenariot måste vi skapa en `TransferContext` objekt.</span><span class="sxs-lookup"><span data-stu-id="199b8-181">To achieve this scenario, we need to create a `TransferContext` object.</span></span> <span data-ttu-id="199b8-182">Den `TransferContext` objektet kommer på två sätt: `SingleTransferContext` och `DirectoryTransferContext`.</span><span class="sxs-lookup"><span data-stu-id="199b8-182">The `TransferContext` object comes in two forms: `SingleTransferContext` and `DirectoryTransferContext`.</span></span> <span data-ttu-id="199b8-183">Är den förra för att överföra en fil (vilket är vad vi gör nu) och denna är för att överföra en katalog med filer (som vi lägger till senare).</span><span class="sxs-lookup"><span data-stu-id="199b8-183">The former is for transferring a single file (which is what we're doing now) and the latter is for transferring a directory of files (which we are adding later).</span></span>

<span data-ttu-id="199b8-184">Lägg till metoder `GetSingleTransferContext` och `GetDirectoryTransferContext` till `Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="199b8-184">Add the methods `GetSingleTransferContext` and `GetDirectoryTransferContext` to `Program.cs`:</span></span> 

```csharp
public static SingleTransferContext GetSingleTransferContext(TransferCheckpoint checkpoint)
{
    SingleTransferContext context = new SingleTransferContext(checkpoint);

    context.ProgressHandler = new Progress<TransferStatus>((progress) =>
    {
        Console.Write("\rBytes transferred: {0}", progress.BytesTransferred );
    });
    
    return context;
}

public static DirectoryTransferContext GetDirectoryTransferContext(TransferCheckpoint checkpoint)
{
    DirectoryTransferContext context = new DirectoryTransferContext(checkpoint);

    context.ProgressHandler = new Progress<TransferStatus>((progress) =>
    {
        Console.Write("\rBytes transferred: {0}", progress.BytesTransferred );
    });
    
    return context;
}
```

<span data-ttu-id="199b8-185">Ändra den `TransferLocalFileToAzureBlob` metod du vill använda `GetSingleTransferContext`:</span><span class="sxs-lookup"><span data-stu-id="199b8-185">Modify the `TransferLocalFileToAzureBlob` method to use `GetSingleTransferContext`:</span></span>

```csharp
public static async Task TransferLocalFileToAzureBlob(CloudStorageAccount account)
{ 
    string localFilePath = GetSourcePath();
    CloudBlockBlob blob = GetBlob(account);
    TransferCheckpoint checkpoint = null;
    SingleTransferContext context = GetSingleTransferContext(checkpoint);
    Console.WriteLine("\nTransfer started...\n");
    Stopwatch stopWatch = Stopwatch.StartNew();
    await TransferManager.UploadAsync(localFilePath, blob, null, context);
    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

## <a name="resume-a-canceled-transfer"></a><span data-ttu-id="199b8-186">Återuppta en annullerad överföring</span><span class="sxs-lookup"><span data-stu-id="199b8-186">Resume a canceled transfer</span></span>
<span data-ttu-id="199b8-187">En annan lämplig funktion som erbjuds av Data Movement Library är möjligheten att återuppta en annullerad överföring.</span><span class="sxs-lookup"><span data-stu-id="199b8-187">Another convenient feature offered by the Data Movement Library is the ability to resume a canceled transfer.</span></span> <span data-ttu-id="199b8-188">Lägg till kod som gör att vi kan tillfälligt avbryta överföringen genom att skriva `c`, och sedan fortsätta överföringen 3 sekunder senare.</span><span class="sxs-lookup"><span data-stu-id="199b8-188">Let's add some code that allows us to temporarily cancel the transfer by typing `c`, and then resume the transfer 3 seconds later.</span></span>

<span data-ttu-id="199b8-189">Ändra `TransferLocalFileToAzureBlob`:</span><span class="sxs-lookup"><span data-stu-id="199b8-189">Modify `TransferLocalFileToAzureBlob`:</span></span>

```csharp
public static async Task TransferLocalFileToAzureBlob(CloudStorageAccount account)
{ 
    string localFilePath = GetSourcePath();
    CloudBlockBlob blob = GetBlob(account); 
    TransferCheckpoint checkpoint = null;
    SingleTransferContext context = GetSingleTransferContext(checkpoint); 
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' to temporarily cancel your transfer...\n");

    Stopwatch stopWatch = Stopwatch.StartNew();
    Task task;
    ConsoleKeyInfo keyinfo;
    try
    {
        task = TransferManager.UploadAsync(localFilePath, blob, null, context, cancellationSource.Token);
        while(!task.IsCompleted)
        {
            if(Console.KeyAvailable)
            {
                keyinfo = Console.ReadKey(true);
                if(keyinfo.Key == ConsoleKey.C)
                {
                    cancellationSource.Cancel();
                }
            }
        }
        await task;
    }
    catch(Exception e)
    {
        Console.WriteLine("\nThe transfer is canceled: {0}", e.Message);  
    }

    if(cancellationSource.IsCancellationRequested)
    {
        Console.WriteLine("\nTransfer will resume in 3 seconds...");
        Thread.Sleep(3000);
        checkpoint = context.LastCheckpoint;
        context = GetSingleTransferContext(checkpoint);
        Console.WriteLine("\nResuming transfer...\n");
        await TransferManager.UploadAsync(localFilePath, blob, null, context);
    }

    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

<span data-ttu-id="199b8-190">Fram till nu vår `checkpoint` värdet alltid har angetts till `null`.</span><span class="sxs-lookup"><span data-stu-id="199b8-190">Up until now, our `checkpoint` value has always been set to `null`.</span></span> <span data-ttu-id="199b8-191">Om vi avbryter överföringen vi nu hämta den senaste kontrollpunkten på vår överföring och använda den här nya kontrollpunkten i vår kontext för överföring.</span><span class="sxs-lookup"><span data-stu-id="199b8-191">Now, if we cancel the transfer, we retrieve the last checkpoint of our transfer, then use this new checkpoint in our transfer context.</span></span> 

## <a name="transfer-local-directory-to-azure-blob-directory"></a><span data-ttu-id="199b8-192">Överföra lokala katalog till Azure Blob-katalog</span><span class="sxs-lookup"><span data-stu-id="199b8-192">Transfer local directory to Azure Blob directory</span></span>
<span data-ttu-id="199b8-193">Det vore förväntar dig om Data Movement Library kan bara överföra en fil i taget.</span><span class="sxs-lookup"><span data-stu-id="199b8-193">It would be disappointing if the Data Movement Library could only transfer one file at a time.</span></span> <span data-ttu-id="199b8-194">Som tur är är detta inte fallet.</span><span class="sxs-lookup"><span data-stu-id="199b8-194">Luckily, this is not the case.</span></span> <span data-ttu-id="199b8-195">Data Movement Library ger möjlighet att överföra en katalog med filer och alla dess underkataloger.</span><span class="sxs-lookup"><span data-stu-id="199b8-195">The Data Movement Library provides the ability to transfer a directory of files and all of its subdirectories.</span></span> <span data-ttu-id="199b8-196">Lägg till kod som gör det möjligt för oss att göra just.</span><span class="sxs-lookup"><span data-stu-id="199b8-196">Let's add some code that allows us to do just that.</span></span>

<span data-ttu-id="199b8-197">Lägg först till metoden `GetBlobDirectory` till `Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="199b8-197">First, add the method `GetBlobDirectory` to `Program.cs`:</span></span>

```csharp
public static CloudBlobDirectory GetBlobDirectory(CloudStorageAccount account)
{
    CloudBlobClient blobClient = account.CreateCloudBlobClient();

    Console.WriteLine("\nProvide name of Blob container. This can be a new or existing Blob container:");
    string containerName = Console.ReadLine();
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);
    container.CreateIfNotExistsAsync().Wait();

    CloudBlobDirectory blobDirectory = container.GetDirectoryReference("");

    return blobDirectory;
}
```

<span data-ttu-id="199b8-198">Ändra sedan `TransferLocalDirectoryToAzureBlobDirectory`:</span><span class="sxs-lookup"><span data-stu-id="199b8-198">Then, modify `TransferLocalDirectoryToAzureBlobDirectory`:</span></span>

```csharp
public static async Task TransferLocalDirectoryToAzureBlobDirectory(CloudStorageAccount account)
{ 
    string localDirectoryPath = GetSourcePath();
    CloudBlobDirectory blobDirectory = GetBlobDirectory(account); 
    TransferCheckpoint checkpoint = null;
    DirectoryTransferContext context = GetDirectoryTransferContext(checkpoint); 
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' to temporarily cancel your transfer...\n");

    Stopwatch stopWatch = Stopwatch.StartNew();
    Task task;
    ConsoleKeyInfo keyinfo;
    UploadDirectoryOptions options = new UploadDirectoryOptions()
    {
        Recursive = true
    };

    try
    {
        task = TransferManager.UploadDirectoryAsync(localDirectoryPath, blobDirectory, options, context, cancellationSource.Token);
        while(!task.IsCompleted)
        {
            if(Console.KeyAvailable)
            {
                keyinfo = Console.ReadKey(true);
                if(keyinfo.Key == ConsoleKey.C)
                {
                    cancellationSource.Cancel();
                }
            }
        }
        await task;
    }
    catch(Exception e)
    {
        Console.WriteLine("\nThe transfer is canceled: {0}", e.Message);  
    }

    if(cancellationSource.IsCancellationRequested)
    {
        Console.WriteLine("\nTransfer will resume in 3 seconds...");
        Thread.Sleep(3000);
        checkpoint = context.LastCheckpoint;
        context = GetDirectoryTransferContext(checkpoint);
        Console.WriteLine("\nResuming transfer...\n");
        await TransferManager.UploadDirectoryAsync(localDirectoryPath, blobDirectory, options, context);
    }

    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

<span data-ttu-id="199b8-199">Det finns några skillnader mellan den här metoden och metod för att överföra en fil.</span><span class="sxs-lookup"><span data-stu-id="199b8-199">There are a few differences between this method and the method for uploading a single file.</span></span> <span data-ttu-id="199b8-200">Nu ska du använda `TransferManager.UploadDirectoryAsync` och `getDirectoryTransferContext` metoden som vi skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="199b8-200">We're now using `TransferManager.UploadDirectoryAsync` and the `getDirectoryTransferContext` method we created earlier.</span></span> <span data-ttu-id="199b8-201">Dessutom kan vi ger nu en `options` värde till vår överföringen, vilket gör att vi kan tyda på att vi vill inkludera undermappar i vår överföringen.</span><span class="sxs-lookup"><span data-stu-id="199b8-201">In addition, we now provide an `options` value to our upload operation, which allows us to indicate that we want to include subdirectories in our upload.</span></span> 

## <a name="copy-file-from-url-to-azure-blob"></a><span data-ttu-id="199b8-202">Kopiera filen från URL: en till Azure-Blob</span><span class="sxs-lookup"><span data-stu-id="199b8-202">Copy file from URL to Azure Blob</span></span>
<span data-ttu-id="199b8-203">Nu ska vi lägga till kod som gör att vi kan kopiera en fil från en URL till en Azure-Blob.</span><span class="sxs-lookup"><span data-stu-id="199b8-203">Now, let's add code that allows us to copy a file from a URL to an Azure Blob.</span></span> 

<span data-ttu-id="199b8-204">Ändra `TransferUrlToAzureBlob`:</span><span class="sxs-lookup"><span data-stu-id="199b8-204">Modify `TransferUrlToAzureBlob`:</span></span>

```csharp
public static async Task TransferUrlToAzureBlob(CloudStorageAccount account)
{
    Uri uri = new Uri(GetSourcePath());
    CloudBlockBlob blob = GetBlob(account); 
    TransferCheckpoint checkpoint = null;
    SingleTransferContext context = GetSingleTransferContext(checkpoint); 
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' to temporarily cancel your transfer...\n");

    Stopwatch stopWatch = Stopwatch.StartNew();
    Task task;
    ConsoleKeyInfo keyinfo;
    try
    {
        task = TransferManager.CopyAsync(uri, blob, true, null, context, cancellationSource.Token);
        while(!task.IsCompleted)
        {
            if(Console.KeyAvailable)
            {
                keyinfo = Console.ReadKey(true);
                if(keyinfo.Key == ConsoleKey.C)
                {
                    cancellationSource.Cancel();
                }
            }
        }
        await task;
    }
    catch(Exception e)
    {
        Console.WriteLine("\nThe transfer is canceled: {0}", e.Message);  
    }

    if(cancellationSource.IsCancellationRequested)
    {
        Console.WriteLine("\nTransfer will resume in 3 seconds...");
        Thread.Sleep(3000);
        checkpoint = context.LastCheckpoint;
        context = GetSingleTransferContext(checkpoint);
        Console.WriteLine("\nResuming transfer...\n");
        await TransferManager.CopyAsync(uri, blob, true, null, context, cancellationSource.Token);
    }

    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

<span data-ttu-id="199b8-205">Det är en viktig användningsfall för den här funktionen när du behöver flytta data från en annan molntjänst (t.ex. AWS) till Azure.</span><span class="sxs-lookup"><span data-stu-id="199b8-205">One important use case for this feature is when you need to move data from another cloud service (e.g. AWS) to Azure.</span></span> <span data-ttu-id="199b8-206">Så länge som du har en URL som ger dig tillgång till resursen du kan enkelt flytta resursen till Azure-BLOB med hjälp av den `TransferManager.CopyAsync` metoden.</span><span class="sxs-lookup"><span data-stu-id="199b8-206">As long as you have a URL that gives you access to the resource, you can easily move that resource into Azure Blobs by using the `TransferManager.CopyAsync` method.</span></span> <span data-ttu-id="199b8-207">Den här metoden skapar också en ny boolesk parameter.</span><span class="sxs-lookup"><span data-stu-id="199b8-207">This method also introduces a new boolean parameter.</span></span> <span data-ttu-id="199b8-208">Parametern `true` anger att vi vill göra en asynkron serversidan kopia.</span><span class="sxs-lookup"><span data-stu-id="199b8-208">Setting this parameter to `true` indicates that we want to do an asynchronous server-side copy.</span></span> <span data-ttu-id="199b8-209">Parametern `false` anger en synkron kopia - vilket innebär att resursen är laddas ned till våra lokala datorn först och sedan överförs till Azure Blob.</span><span class="sxs-lookup"><span data-stu-id="199b8-209">Setting this parameter to `false` indicates a synchronous copy - meaning the resource is downloaded to our local machine first, then uploaded to Azure Blob.</span></span> <span data-ttu-id="199b8-210">Dock finns synkron kopia för närvarande bara för att kopiera från en Azure Storage-resurs till en annan.</span><span class="sxs-lookup"><span data-stu-id="199b8-210">However, synchronous copy is currently only available for copying from one Azure Storage resource to another.</span></span> 

## <a name="transfer-azure-blob-to-azure-blob"></a><span data-ttu-id="199b8-211">Överför Azure Blob till Azure Blob</span><span class="sxs-lookup"><span data-stu-id="199b8-211">Transfer Azure Blob to Azure Blob</span></span>
<span data-ttu-id="199b8-212">En annan funktion som unikt tillhandahålls av Data Movement Library är möjligheten att kopiera från en Azure Storage-resurs till en annan.</span><span class="sxs-lookup"><span data-stu-id="199b8-212">Another feature that's uniquely provided by the Data Movement Library is the ability to copy from one Azure Storage resource to another.</span></span> 

<span data-ttu-id="199b8-213">Ändra `TransferAzureBlobToAzureBlob`:</span><span class="sxs-lookup"><span data-stu-id="199b8-213">Modify `TransferAzureBlobToAzureBlob`:</span></span>

```csharp
public static async Task TransferAzureBlobToAzureBlob(CloudStorageAccount account)
{
    CloudBlockBlob sourceBlob = GetBlob(account);
    CloudBlockBlob destinationBlob = GetBlob(account); 
    TransferCheckpoint checkpoint = null;
    SingleTransferContext context = GetSingleTransferContext(checkpoint); 
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' to temporarily cancel your transfer...\n");

    Stopwatch stopWatch = Stopwatch.StartNew();
    Task task;
    ConsoleKeyInfo keyinfo;
    try
    {
        task = TransferManager.CopyAsync(sourceBlob, destinationBlob, true, null, context, cancellationSource.Token);
        while(!task.IsCompleted)
        {
            if(Console.KeyAvailable)
            {
                keyinfo = Console.ReadKey(true);
                if(keyinfo.Key == ConsoleKey.C)
                {
                    cancellationSource.Cancel();
                }
            }
        }
        await task;
    }
    catch(Exception e)
    {
        Console.WriteLine("\nThe transfer is canceled: {0}", e.Message);  
    }

    if(cancellationSource.IsCancellationRequested)
    {
        Console.WriteLine("\nTransfer will resume in 3 seconds...");
        Thread.Sleep(3000);
        checkpoint = context.LastCheckpoint;
        context = GetSingleTransferContext(checkpoint);
        Console.WriteLine("\nResuming transfer...\n");
        await TransferManager.CopyAsync(sourceBlob, destinationBlob, false, null, context, cancellationSource.Token);
    }

    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

<span data-ttu-id="199b8-214">I det här exemplet anger vi boolesk parameter i `TransferManager.CopyAsync` till `false` att indikera att vi vill göra en synkron kopia.</span><span class="sxs-lookup"><span data-stu-id="199b8-214">In this example, we set the boolean parameter in `TransferManager.CopyAsync` to `false` to indicate that we want to do a synchronous copy.</span></span> <span data-ttu-id="199b8-215">Detta innebär att resursen är laddas ned till våra lokala datorn först och sedan har överförts till Azure-Blob.</span><span class="sxs-lookup"><span data-stu-id="199b8-215">This means that the resource is downloaded to our local machine first, then uploaded to Azure Blob.</span></span> <span data-ttu-id="199b8-216">Alternativet synkrona kopior är ett bra sätt att kontrollera att din Kopieringsåtgärden har en konsekvent hastighet.</span><span class="sxs-lookup"><span data-stu-id="199b8-216">The synchronous copy option is a great way to ensure that your copy operation has a consistent speed.</span></span> <span data-ttu-id="199b8-217">Däremot är hastigheten för en asynkron serversidan kopia beroende av den tillgängliga nätverksbandbredden på servern, vilket kan variera.</span><span class="sxs-lookup"><span data-stu-id="199b8-217">In contrast, the speed of an asynchronous server-side copy is dependent on the available network bandwidth on the server, which can fluctuate.</span></span> <span data-ttu-id="199b8-218">Synkron copy kan dock ge ytterligare utgång kostnaden jämfört med asynkron kopia.</span><span class="sxs-lookup"><span data-stu-id="199b8-218">However, synchronous copy may generate additional egress cost compared to asynchronous copy.</span></span> <span data-ttu-id="199b8-219">Den rekommenderade metoden är att använda synkron kopia i en Azure VM är i samma region som ditt källa storage-konto för att undvika kostnader för utgående trafik.</span><span class="sxs-lookup"><span data-stu-id="199b8-219">The recommended approach is to use synchronous copy in an Azure VM that is in the same region as your source storage account to avoid egress cost.</span></span>

## <a name="conclusion"></a><span data-ttu-id="199b8-220">Slutsats</span><span class="sxs-lookup"><span data-stu-id="199b8-220">Conclusion</span></span>
<span data-ttu-id="199b8-221">Vårt program för flytt av data är slutförd.</span><span class="sxs-lookup"><span data-stu-id="199b8-221">Our data movement application is now complete.</span></span> <span data-ttu-id="199b8-222">[Fullständig kodexemplet finns på GitHub](https://github.com/azure-samples/storage-dotnet-data-movement-library-app).</span><span class="sxs-lookup"><span data-stu-id="199b8-222">[The full code sample is available on GitHub](https://github.com/azure-samples/storage-dotnet-data-movement-library-app).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="199b8-223">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="199b8-223">Next steps</span></span>
<span data-ttu-id="199b8-224">I den här komma igång, skapat vi ett program som samverkar med Azure Storage och körs på Windows, Linux och macOS.</span><span class="sxs-lookup"><span data-stu-id="199b8-224">In this getting started, we created an application that interacts with Azure Storage and runs on Windows, Linux, and macOS.</span></span> <span data-ttu-id="199b8-225">Den här komma igång fokuserar på Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="199b8-225">This getting started focused on Blob Storage.</span></span> <span data-ttu-id="199b8-226">Denna samma kunskap kan dock användas för lagring.</span><span class="sxs-lookup"><span data-stu-id="199b8-226">However, this same knowledge can be applied to File Storage.</span></span> <span data-ttu-id="199b8-227">Lär dig mer genom att checka ut [referensdokumentationen för Azure Storage Data Movement Library](https://azure.github.io/azure-storage-net-data-movement).</span><span class="sxs-lookup"><span data-stu-id="199b8-227">To learn more, check out [Azure Storage Data Movement Library reference documentation](https://azure.github.io/azure-storage-net-data-movement).</span></span>

[!INCLUDE [storage-try-azure-tools-blobs](../../../includes/storage-try-azure-tools-blobs.md)]





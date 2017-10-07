---
title: aaaTransfer Data med hello Microsoft Azure Storage Data Movement Library | Microsoft Docs
description: "Använd hello Data Movement Library toomove eller kopiera data tooor från blob- och innehåll. Kopiera data tooAzure lagring från lokala filer eller kopiera data inom eller mellan lagringskonton. Enkelt migrera dina data tooAzure lagring."
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
ms.openlocfilehash: 9aec6cb171f794cc6ca432938ce499079e7dfdec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="transfer-data-with-hello-microsoft-azure-storage-data-movement-library"></a><span data-ttu-id="2fdc1-105">Överföra Data med hello Microsoft Azure Storage Data Movement Library</span><span class="sxs-lookup"><span data-stu-id="2fdc1-105">Transfer Data with hello Microsoft Azure Storage Data Movement Library</span></span>

## <a name="overview"></a><span data-ttu-id="2fdc1-106">Översikt</span><span class="sxs-lookup"><span data-stu-id="2fdc1-106">Overview</span></span>
<span data-ttu-id="2fdc1-107">hello Microsoft Azure Storage Data Movement Library är ett bibliotek med öppen källkod för flera plattformar som är utformat för högpresterande överföringen, hämtar och kopiering av Azure Storage-Blobbar och filer.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-107">hello Microsoft Azure Storage Data Movement Library is a cross-platform open source library that is designed for high performance uploading, downloading, and copying of Azure Storage Blobs and Files.</span></span> <span data-ttu-id="2fdc1-108">Det här biblioteket är hello core data movement framework som används av [AzCopy](../storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="2fdc1-108">This library is hello core data movement framework that powers [AzCopy](../storage-use-azcopy.md).</span></span> <span data-ttu-id="2fdc1-109">hello Data Movement Library ger praktiska metoder som inte är tillgängliga i vår traditionella [.NET Azure Storage-klientbibliotek](../blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="2fdc1-109">hello Data Movement Library provides convenient methods that aren't available in our traditional [.NET Azure Storage Client Library](../blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="2fdc1-110">Detta inkluderar hello möjlighet tooset hello antalet parallella åtgärder, spåra Överföringsförlopp, återuppta enkelt en annullerad överföring och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-110">This includes hello ability tooset hello number of parallel operations, track transfer progress, easily resume a canceled transfer, and much more.</span></span>  

<span data-ttu-id="2fdc1-111">.NET Core, vilket innebär att du kan använda den när du skapar .NET appar för Windows, Linux och macOS används också i det här biblioteket.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-111">This library also uses .NET Core, which means you can use it when building .NET apps for Windows, Linux and macOS.</span></span> <span data-ttu-id="2fdc1-112">toolearn mer om .NET Core finns toohello [.NET Core-dokumentation](https://dotnet.github.io/).</span><span class="sxs-lookup"><span data-stu-id="2fdc1-112">toolearn more about .NET Core, refer toohello [.NET Core documentation](https://dotnet.github.io/).</span></span> <span data-ttu-id="2fdc1-113">Det här biblioteket fungerar även för traditionella .NET Framework-appar för Windows.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-113">This library also works for traditional .NET Framework apps for Windows.</span></span> 

<span data-ttu-id="2fdc1-114">Det här dokumentet visar hur toocreate en .NET Core konsolen program som körs på Windows, Linux och macOS och utför hello följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="2fdc1-114">This document demonstrates how toocreate a .NET Core console application that that runs on Windows, Linux, and macOS and performs hello following scenarios:</span></span>

- <span data-ttu-id="2fdc1-115">Ladda upp filer och kataloger tooBlob lagring.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-115">Upload files and directories tooBlob Storage.</span></span>
- <span data-ttu-id="2fdc1-116">Definiera hello antalet parallella åtgärder vid överföring av data.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-116">Define hello number of parallel operations when transferring data.</span></span>
- <span data-ttu-id="2fdc1-117">Spåra förloppet för data transfer.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-117">Track data transfer progress.</span></span>
- <span data-ttu-id="2fdc1-118">Återuppta avbrutna dataöverföring.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-118">Resume canceled data transfer.</span></span> 
- <span data-ttu-id="2fdc1-119">Kopiera filen från URL: en tooBlob lagring.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-119">Copy file from URL tooBlob Storage.</span></span> 
- <span data-ttu-id="2fdc1-120">Kopiera från Blob Storage tooBlob lagring.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-120">Copy from Blob Storage tooBlob Storage.</span></span>

<span data-ttu-id="2fdc1-121">**Vad du behöver:**</span><span class="sxs-lookup"><span data-stu-id="2fdc1-121">**What you need:**</span></span>

* [<span data-ttu-id="2fdc1-122">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2fdc1-122">Visual Studio Code</span></span>](https://code.visualstudio.com/)
* <span data-ttu-id="2fdc1-123">Ett [Azure Storage-konto](storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="2fdc1-123">An [Azure storage account](storage-create-storage-account.md#create-a-storage-account)</span></span>

> [!NOTE]
> <span data-ttu-id="2fdc1-124">Den här handboken förutsätts att du redan är bekant med [Azure Storage](https://azure.microsoft.com/services/storage/).</span><span class="sxs-lookup"><span data-stu-id="2fdc1-124">This guide assumes that you are already familiar with [Azure Storage](https://azure.microsoft.com/services/storage/).</span></span> <span data-ttu-id="2fdc1-125">Om inte, läsning hello [introduktion tooAzure lagring](storage-introduction.md) dokumentation är bra.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-125">If not, reading hello [Introduction tooAzure Storage](storage-introduction.md) documentation is helpful.</span></span> <span data-ttu-id="2fdc1-126">Viktigast av allt du behöver för[skapa ett lagringskonto](storage-create-storage-account.md#create-a-storage-account) toostart med hello Data Movement Library.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-126">Most importantly, you need too[create a Storage account](storage-create-storage-account.md#create-a-storage-account) toostart using hello Data Movement Library.</span></span>
> 
> 

## <a name="setup"></a><span data-ttu-id="2fdc1-127">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="2fdc1-127">Setup</span></span>  

1. <span data-ttu-id="2fdc1-128">Besök hello [.NET Core installationsguiden](https://www.microsoft.com/net/core) tooinstall .NET Core.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-128">Visit hello [.NET Core Installation Guide](https://www.microsoft.com/net/core) tooinstall .NET Core.</span></span> <span data-ttu-id="2fdc1-129">När du väljer din miljö Välj hello kommandoradsalternativet.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-129">When selecting your environment, choose hello command-line option.</span></span> 
2. <span data-ttu-id="2fdc1-130">Skapa en katalog för ditt projekt från hello kommandorad.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-130">From hello command line, create a directory for your project.</span></span> <span data-ttu-id="2fdc1-131">Navigera till den här katalogen, Skriv `dotnet new` console toocreate C#-projekt.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-131">Navigate into this directory, then type `dotnet new` toocreate a C# console project.</span></span>
3. <span data-ttu-id="2fdc1-132">Öppna den här katalogen i Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-132">Open this directory in Visual Studio Code.</span></span> <span data-ttu-id="2fdc1-133">Det här steget kan utföras snabbt via hello kommandoraden genom att skriva `code .`.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-133">This step can be quickly done via hello command line by typing `code .`.</span></span>  
4. <span data-ttu-id="2fdc1-134">Installera hello [C#-tillägget](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) från hello Visual Studio Code Marketplace.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-134">Install hello [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) from hello Visual Studio Code Marketplace.</span></span> <span data-ttu-id="2fdc1-135">Starta om Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-135">Restart Visual Studio Code.</span></span> 
5. <span data-ttu-id="2fdc1-136">Du bör nu se två frågor.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-136">At this point, you should see two prompts.</span></span> <span data-ttu-id="2fdc1-137">En är för att lägga till ”krävs tillgångar toobuild och debug”.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-137">One is for adding "required assets toobuild and debug."</span></span> <span data-ttu-id="2fdc1-138">Klicka på ”Ja”.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-138">Click "yes."</span></span> <span data-ttu-id="2fdc1-139">En annan frågan är för att återställa Olösta beroenden.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-139">Another prompt is for restoring unresolved dependencies.</span></span> <span data-ttu-id="2fdc1-140">Klicka på ”Återställ”.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-140">Click "restore."</span></span>
6. <span data-ttu-id="2fdc1-141">Programmet innehåller nu en `launch.json` fil under hello `.vscode` directory.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-141">Your application should now contain a `launch.json` file under hello `.vscode` directory.</span></span> <span data-ttu-id="2fdc1-142">I den här filen, ändrar du hello `externalConsole` värdet för`true`.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-142">In this file, change hello `externalConsole` value too`true`.</span></span>
7. <span data-ttu-id="2fdc1-143">Visual Studio Code kan du toodebug .NET Core program.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-143">Visual Studio Code allows you toodebug .NET Core applications.</span></span> <span data-ttu-id="2fdc1-144">Träffa `F5` toorun ditt program och kontrollera att inställningarna fungerar.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-144">Hit `F5` toorun your application and verify that your setup is working.</span></span> <span data-ttu-id="2fdc1-145">Du bör se ”Hello World”!</span><span class="sxs-lookup"><span data-stu-id="2fdc1-145">You should see "Hello World!"</span></span> <span data-ttu-id="2fdc1-146">utskrivna toohello konsol.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-146">printed toohello console.</span></span> 

## <a name="add-data-movement-library-tooyour-project"></a><span data-ttu-id="2fdc1-147">Lägga till Data Movement Library tooyour projekt</span><span class="sxs-lookup"><span data-stu-id="2fdc1-147">Add Data Movement Library tooyour project</span></span>

1. <span data-ttu-id="2fdc1-148">Lägg till hello senaste versionen av hello Data Movement Library toohello `dependencies` avsnitt i din `project.json` fil.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-148">Add hello latest version of hello Data Movement Library toohello `dependencies` section of your `project.json` file.</span></span> <span data-ttu-id="2fdc1-149">Den här versionen är samtidigt hello skrivning`"Microsoft.Azure.Storage.DataMovement": "0.5.0"`</span><span class="sxs-lookup"><span data-stu-id="2fdc1-149">At hello time of writing, this version would be `"Microsoft.Azure.Storage.DataMovement": "0.5.0"`</span></span> 
2. <span data-ttu-id="2fdc1-150">Lägg till `"portable-net45+win8"` toohello `imports` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-150">Add `"portable-net45+win8"` toohello `imports` section.</span></span> 
3. <span data-ttu-id="2fdc1-151">En fråga ska visa toorestore projektet.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-151">A prompt should display toorestore your project.</span></span> <span data-ttu-id="2fdc1-152">Klicka hello ”återställa”.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-152">Click hello "restore" button.</span></span> <span data-ttu-id="2fdc1-153">Du kan även återställa projektet från hello Kommandotolken genom att skriva hello `dotnet restore` i projektkatalogen hello rot.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-153">You can also restore your project from hello command line by typing hello command `dotnet restore` in hello root of your project directory.</span></span>

<span data-ttu-id="2fdc1-154">Ändra `project.json`:</span><span class="sxs-lookup"><span data-stu-id="2fdc1-154">Modify `project.json`:</span></span>

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

## <a name="set-up-hello-skeleton-of-your-application"></a><span data-ttu-id="2fdc1-155">Ställ in hello stommen i ditt program</span><span class="sxs-lookup"><span data-stu-id="2fdc1-155">Set up hello skeleton of your application</span></span>
<span data-ttu-id="2fdc1-156">hello ställs första vi gör in hello ”stommen” koden för vårt program.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-156">hello first thing we do is set up hello "skeleton" code of our application.</span></span> <span data-ttu-id="2fdc1-157">Den här koden oss uppmanas att ange ett namn och lagringskontonyckel och använder dessa autentiseringsuppgifter toocreate en `CloudStorageAccount` objekt.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-157">This code prompts us for a Storage account name and account key and uses those credentials toocreate a `CloudStorageAccount` object.</span></span> <span data-ttu-id="2fdc1-158">Det här objektet är används toointeract med våra Storage-konto i alla scenarier för överföring.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-158">This object is used toointeract with our Storage account in all transfer scenarios.</span></span> <span data-ttu-id="2fdc1-159">hello kod uppmanar oss också toochoose hello typ av överföringen som vi vill gärna tooexecute.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-159">hello code also prompts us toochoose hello type of transfer operation we would like tooexecute.</span></span> 

<span data-ttu-id="2fdc1-160">Ändra `Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="2fdc1-160">Modify `Program.cs`:</span></span>

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
            Console.WriteLine("\nWhat type of transfer would you like tooexecute?\n1. Local file --> Azure Blob\n2. Local directory --> Azure Blob directory\n3. URL (e.g. Amazon S3 file) --> Azure Blob\n4. Azure Blob --> Azure Blob");
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

## <a name="transfer-local-file-tooazure-blob"></a><span data-ttu-id="2fdc1-161">Överför lokal fil tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="2fdc1-161">Transfer local file tooAzure Blob</span></span>
<span data-ttu-id="2fdc1-162">Lägga till hello metoder `GetSourcePath` och `GetBlob` för`Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="2fdc1-162">Add hello methods `GetSourcePath` and `GetBlob` too`Program.cs`:</span></span>

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

<span data-ttu-id="2fdc1-163">Ändra hello `TransferLocalFileToAzureBlob` metoden:</span><span class="sxs-lookup"><span data-stu-id="2fdc1-163">Modify hello `TransferLocalFileToAzureBlob` method:</span></span>

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

<span data-ttu-id="2fdc1-164">Den här koden efterfrågar oss hello sökvägen tooa lokal fil och hello namnet på en ny eller befintlig behållare hello namnet på en ny blob.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-164">This code prompts us for hello path tooa local file, hello name of a new or existing container, and hello name of a new blob.</span></span> <span data-ttu-id="2fdc1-165">Hej `TransferManager.UploadAsync` metoden utför hello Överför med den här informationen.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-165">hello `TransferManager.UploadAsync` method performs hello upload using this information.</span></span> 

<span data-ttu-id="2fdc1-166">Träffa `F5` toorun ditt program.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-166">Hit `F5` toorun your application.</span></span> <span data-ttu-id="2fdc1-167">Du kan verifiera att hello överför uppstod genom att visa ditt lagringskonto med hello [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="2fdc1-167">You can verify that hello upload occurred by viewing your Storage account with hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span></span>

## <a name="set-number-of-parallel-operations"></a><span data-ttu-id="2fdc1-168">Ange antalet parallella åtgärder</span><span class="sxs-lookup"><span data-stu-id="2fdc1-168">Set number of parallel operations</span></span>
<span data-ttu-id="2fdc1-169">En bra funktion som erbjuds av hello Data Movement Library är hello möjlighet tooset hello antalet parallella åtgärder tooincrease hello data transfer genomflöde.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-169">A great feature offered by hello Data Movement Library is hello ability tooset hello number of parallel operations tooincrease hello data transfer throughput.</span></span> <span data-ttu-id="2fdc1-170">Som standard anges hello Data Movement Library hello antalet parallella åtgärder too8 * hello antal kärnor på datorn.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-170">By default, hello Data Movement Library sets hello number of parallel operations too8 * hello number of cores on your machine.</span></span> 

<span data-ttu-id="2fdc1-171">Tänk på att många parallella åtgärder i en miljö med låg bandbredd kan överväldigande hello nätverksanslutning och faktiskt förhindra operations fullständigt slutförs.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-171">Keep in mind that many parallel operations in a low-bandwidth environment may overwhelm hello network connection and actually prevent operations from fully completing.</span></span> <span data-ttu-id="2fdc1-172">Du behöver tooexperiment med den här inställningen toodetermine vad som fungerar bäst baserat på din tillgängliga nätverksbandbredd.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-172">You'll need tooexperiment with this setting toodetermine what works best based on your available network bandwidth.</span></span> 

<span data-ttu-id="2fdc1-173">Lägg till kod som gör att vi tooset hello antalet parallella åtgärder.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-173">Let's add some code that allows us tooset hello number of parallel operations.</span></span> <span data-ttu-id="2fdc1-174">Lägg till kod som gånger hur lång tid det tar för hello överföring toocomplete också.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-174">Let's also add code that times how long it takes for hello transfer toocomplete.</span></span>

<span data-ttu-id="2fdc1-175">Lägg till en `SetNumberOfParallelOperations` metod för`Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="2fdc1-175">Add a `SetNumberOfParallelOperations` method too`Program.cs`:</span></span>

```csharp
public static void SetNumberOfParallelOperations()
{
    Console.WriteLine("\nHow many parallel operations would you like toouse?");
    string parallelOperations = Console.ReadLine();
    TransferManager.Configurations.ParallelOperations = int.Parse(parallelOperations);
}
```

<span data-ttu-id="2fdc1-176">Ändra hello `ExecuteChoice` metoden toouse `SetNumberOfParallelOperations`:</span><span class="sxs-lookup"><span data-stu-id="2fdc1-176">Modify hello `ExecuteChoice` method toouse `SetNumberOfParallelOperations`:</span></span>

```csharp
public static void ExecuteChoice(CloudStorageAccount account)
{
    Console.WriteLine("\nWhat type of transfer would you like tooexecute?\n1. Local file --> Azure Blob\n2. Local directory --> Azure Blob directory\n3. URL (e.g. Amazon S3 file) --> Azure Blob\n4. Azure Blob --> Azure Blob");
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

<span data-ttu-id="2fdc1-177">Ändra hello `TransferLocalFileToAzureBlob` metoden toouse en timer:</span><span class="sxs-lookup"><span data-stu-id="2fdc1-177">Modify hello `TransferLocalFileToAzureBlob` method toouse a timer:</span></span>

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

## <a name="track-transfer-progress"></a><span data-ttu-id="2fdc1-178">Spåra förloppet för överföring</span><span class="sxs-lookup"><span data-stu-id="2fdc1-178">Track transfer progress</span></span>
<span data-ttu-id="2fdc1-179">Det är bra att känna till hur lång tid det tog för våra data tootransfer.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-179">Knowing how long it took for our data tootransfer is great.</span></span> <span data-ttu-id="2fdc1-180">Men som kan toosee hello förloppet för vår överföring *under* hello överföringen blir ännu bättre.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-180">However, being able toosee hello progress of our transfer *during* hello transfer operation would be even better.</span></span> <span data-ttu-id="2fdc1-181">tooachieve i det här scenariot måste toocreate en `TransferContext` objekt.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-181">tooachieve this scenario, we need toocreate a `TransferContext` object.</span></span> <span data-ttu-id="2fdc1-182">Hej `TransferContext` objektet kommer på två sätt: `SingleTransferContext` och `DirectoryTransferContext`.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-182">hello `TransferContext` object comes in two forms: `SingleTransferContext` and `DirectoryTransferContext`.</span></span> <span data-ttu-id="2fdc1-183">hello tidigare är för att överföra en fil (vilket är vad vi gör nu) och hello senare för att överföra en katalog med filer (som vi lägger till senare).</span><span class="sxs-lookup"><span data-stu-id="2fdc1-183">hello former is for transferring a single file (which is what we're doing now) and hello latter is for transferring a directory of files (which we are adding later).</span></span>

<span data-ttu-id="2fdc1-184">Lägga till hello metoder `GetSingleTransferContext` och `GetDirectoryTransferContext` för`Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="2fdc1-184">Add hello methods `GetSingleTransferContext` and `GetDirectoryTransferContext` too`Program.cs`:</span></span> 

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

<span data-ttu-id="2fdc1-185">Ändra hello `TransferLocalFileToAzureBlob` metoden toouse `GetSingleTransferContext`:</span><span class="sxs-lookup"><span data-stu-id="2fdc1-185">Modify hello `TransferLocalFileToAzureBlob` method toouse `GetSingleTransferContext`:</span></span>

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

## <a name="resume-a-canceled-transfer"></a><span data-ttu-id="2fdc1-186">Återuppta en annullerad överföring</span><span class="sxs-lookup"><span data-stu-id="2fdc1-186">Resume a canceled transfer</span></span>
<span data-ttu-id="2fdc1-187">En annan lämplig funktion som erbjuds av hello Data Movement Library är hello möjlighet tooresume avbrutna överföring.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-187">Another convenient feature offered by hello Data Movement Library is hello ability tooresume a canceled transfer.</span></span> <span data-ttu-id="2fdc1-188">Lägg till kod som gör att vi tootemporarily Avbryt hello överföring genom att skriva `c`, och sedan fortsätta hello överföring 3 sekunder senare.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-188">Let's add some code that allows us tootemporarily cancel hello transfer by typing `c`, and then resume hello transfer 3 seconds later.</span></span>

<span data-ttu-id="2fdc1-189">Ändra `TransferLocalFileToAzureBlob`:</span><span class="sxs-lookup"><span data-stu-id="2fdc1-189">Modify `TransferLocalFileToAzureBlob`:</span></span>

```csharp
public static async Task TransferLocalFileToAzureBlob(CloudStorageAccount account)
{ 
    string localFilePath = GetSourcePath();
    CloudBlockBlob blob = GetBlob(account); 
    TransferCheckpoint checkpoint = null;
    SingleTransferContext context = GetSingleTransferContext(checkpoint); 
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' tootemporarily cancel your transfer...\n");

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

<span data-ttu-id="2fdc1-190">Fram till nu vår `checkpoint` alltid värdet för`null`.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-190">Up until now, our `checkpoint` value has always been set too`null`.</span></span> <span data-ttu-id="2fdc1-191">Om vi avbryter hello överföring vi nu hämta hello senaste kontrollpunkten på vår överföring och använda den här nya kontrollpunkten i vår kontext för överföring.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-191">Now, if we cancel hello transfer, we retrieve hello last checkpoint of our transfer, then use this new checkpoint in our transfer context.</span></span> 

## <a name="transfer-local-directory-tooazure-blob-directory"></a><span data-ttu-id="2fdc1-192">Överför lokal katalog tooAzure Blob directory</span><span class="sxs-lookup"><span data-stu-id="2fdc1-192">Transfer local directory tooAzure Blob directory</span></span>
<span data-ttu-id="2fdc1-193">Det vore förväntar dig om hello Data Movement Library kan bara överföra en fil i taget.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-193">It would be disappointing if hello Data Movement Library could only transfer one file at a time.</span></span> <span data-ttu-id="2fdc1-194">Detta är som tur är inte hello fallet.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-194">Luckily, this is not hello case.</span></span> <span data-ttu-id="2fdc1-195">hello Data Movement Library innehåller hello möjlighet tootransfer en katalog med filer och alla dess underkataloger.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-195">hello Data Movement Library provides hello ability tootransfer a directory of files and all of its subdirectories.</span></span> <span data-ttu-id="2fdc1-196">Lägg till kod som gör att vi toodo precis så.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-196">Let's add some code that allows us toodo just that.</span></span>

<span data-ttu-id="2fdc1-197">Lägg först till hello metoden `GetBlobDirectory` för`Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="2fdc1-197">First, add hello method `GetBlobDirectory` too`Program.cs`:</span></span>

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

<span data-ttu-id="2fdc1-198">Ändra sedan `TransferLocalDirectoryToAzureBlobDirectory`:</span><span class="sxs-lookup"><span data-stu-id="2fdc1-198">Then, modify `TransferLocalDirectoryToAzureBlobDirectory`:</span></span>

```csharp
public static async Task TransferLocalDirectoryToAzureBlobDirectory(CloudStorageAccount account)
{ 
    string localDirectoryPath = GetSourcePath();
    CloudBlobDirectory blobDirectory = GetBlobDirectory(account); 
    TransferCheckpoint checkpoint = null;
    DirectoryTransferContext context = GetDirectoryTransferContext(checkpoint); 
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' tootemporarily cancel your transfer...\n");

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

<span data-ttu-id="2fdc1-199">Det finns några skillnader mellan den här metoden och hello metod för att överföra en fil.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-199">There are a few differences between this method and hello method for uploading a single file.</span></span> <span data-ttu-id="2fdc1-200">Nu ska du använda `TransferManager.UploadDirectoryAsync` och hello `getDirectoryTransferContext` metoden som vi skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-200">We're now using `TransferManager.UploadDirectoryAsync` and hello `getDirectoryTransferContext` method we created earlier.</span></span> <span data-ttu-id="2fdc1-201">Dessutom kan vi ger nu en `options` värdet tooour överföringen, vilket gör att vi tooindicate som vi vill tooinclude underkataloger i vår överföringen.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-201">In addition, we now provide an `options` value tooour upload operation, which allows us tooindicate that we want tooinclude subdirectories in our upload.</span></span> 

## <a name="copy-file-from-url-tooazure-blob"></a><span data-ttu-id="2fdc1-202">Kopiera filen från URL: en tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="2fdc1-202">Copy file from URL tooAzure Blob</span></span>
<span data-ttu-id="2fdc1-203">Nu ska vi lägga till kod som gör att vi toocopy en fil från en URL-tooan Azure Blob.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-203">Now, let's add code that allows us toocopy a file from a URL tooan Azure Blob.</span></span> 

<span data-ttu-id="2fdc1-204">Ändra `TransferUrlToAzureBlob`:</span><span class="sxs-lookup"><span data-stu-id="2fdc1-204">Modify `TransferUrlToAzureBlob`:</span></span>

```csharp
public static async Task TransferUrlToAzureBlob(CloudStorageAccount account)
{
    Uri uri = new Uri(GetSourcePath());
    CloudBlockBlob blob = GetBlob(account); 
    TransferCheckpoint checkpoint = null;
    SingleTransferContext context = GetSingleTransferContext(checkpoint); 
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' tootemporarily cancel your transfer...\n");

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

<span data-ttu-id="2fdc1-205">Det är en viktig användningsfall för den här funktionen när du behöver toomove data från en annan cloud service (t.ex. AWS) tooAzure.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-205">One important use case for this feature is when you need toomove data from another cloud service (e.g. AWS) tooAzure.</span></span> <span data-ttu-id="2fdc1-206">Så länge som du har en URL som ger du åtkomst till toohello resurs, kan du enkelt flytta resursen i Azure BLOB med hjälp av hello `TransferManager.CopyAsync` metod.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-206">As long as you have a URL that gives you access toohello resource, you can easily move that resource into Azure Blobs by using hello `TransferManager.CopyAsync` method.</span></span> <span data-ttu-id="2fdc1-207">Den här metoden skapar också en ny boolesk parameter.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-207">This method also introduces a new boolean parameter.</span></span> <span data-ttu-id="2fdc1-208">Ange den här parametern för`true` betyder det att vi vill toodo asynkron serversidan kopia.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-208">Setting this parameter too`true` indicates that we want toodo an asynchronous server-side copy.</span></span> <span data-ttu-id="2fdc1-209">Inställningen för den här parametern`false` anger en synkron kopia - vilket innebär att hello resursen är hämtade tooour lokala datorn först, sedan överföra tooAzure Blob.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-209">Setting this parameter too`false` indicates a synchronous copy - meaning hello resource is downloaded tooour local machine first, then uploaded tooAzure Blob.</span></span> <span data-ttu-id="2fdc1-210">Dock finns synkron kopia för närvarande bara för att kopiera från en tooanother för Azure Storage-resurs.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-210">However, synchronous copy is currently only available for copying from one Azure Storage resource tooanother.</span></span> 

## <a name="transfer-azure-blob-tooazure-blob"></a><span data-ttu-id="2fdc1-211">Överför Azure Blob tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="2fdc1-211">Transfer Azure Blob tooAzure Blob</span></span>
<span data-ttu-id="2fdc1-212">En annan funktion som unikt tillhandahålls av hello Data Movement Library är hello möjlighet toocopy från en tooanother för Azure Storage-resurs.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-212">Another feature that's uniquely provided by hello Data Movement Library is hello ability toocopy from one Azure Storage resource tooanother.</span></span> 

<span data-ttu-id="2fdc1-213">Ändra `TransferAzureBlobToAzureBlob`:</span><span class="sxs-lookup"><span data-stu-id="2fdc1-213">Modify `TransferAzureBlobToAzureBlob`:</span></span>

```csharp
public static async Task TransferAzureBlobToAzureBlob(CloudStorageAccount account)
{
    CloudBlockBlob sourceBlob = GetBlob(account);
    CloudBlockBlob destinationBlob = GetBlob(account); 
    TransferCheckpoint checkpoint = null;
    SingleTransferContext context = GetSingleTransferContext(checkpoint); 
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' tootemporarily cancel your transfer...\n");

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

<span data-ttu-id="2fdc1-214">I det här exemplet anger vi hello boolesk parameter i `TransferManager.CopyAsync` för`false` tooindicate som vi vill toodo en synkron kopia.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-214">In this example, we set hello boolean parameter in `TransferManager.CopyAsync` too`false` tooindicate that we want toodo a synchronous copy.</span></span> <span data-ttu-id="2fdc1-215">Det innebär att hello resursen är hämtade tooour lokala datorn först och sedan överföra tooAzure Blob.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-215">This means that hello resource is downloaded tooour local machine first, then uploaded tooAzure Blob.</span></span> <span data-ttu-id="2fdc1-216">hello synkron kopiera alternativet är ett bra sätt tooensure att din Kopieringsåtgärden har en konsekvent hastighet.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-216">hello synchronous copy option is a great way tooensure that your copy operation has a consistent speed.</span></span> <span data-ttu-id="2fdc1-217">Däremot är hello hastigheten för en asynkron serversidan kopia beroende av hello bandbredden på hello servern som kan variera.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-217">In contrast, hello speed of an asynchronous server-side copy is dependent on hello available network bandwidth on hello server, which can fluctuate.</span></span> <span data-ttu-id="2fdc1-218">Synkron copy kan dock ge ytterligare utgång kostnaden jämfört med tooasynchronous kopia.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-218">However, synchronous copy may generate additional egress cost compared tooasynchronous copy.</span></span> <span data-ttu-id="2fdc1-219">hello rekommenderade metoden är toouse synkron kopia i en Azure VM i hello samma region som din datakälla konto tooavoid utgång lagringskostnaden.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-219">hello recommended approach is toouse synchronous copy in an Azure VM that is in hello same region as your source storage account tooavoid egress cost.</span></span>

## <a name="conclusion"></a><span data-ttu-id="2fdc1-220">Slutsats</span><span class="sxs-lookup"><span data-stu-id="2fdc1-220">Conclusion</span></span>
<span data-ttu-id="2fdc1-221">Vårt program för flytt av data är slutförd.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-221">Our data movement application is now complete.</span></span> <span data-ttu-id="2fdc1-222">[hello fullständig kodexempel är tillgängliga på GitHub](https://github.com/azure-samples/storage-dotnet-data-movement-library-app).</span><span class="sxs-lookup"><span data-stu-id="2fdc1-222">[hello full code sample is available on GitHub](https://github.com/azure-samples/storage-dotnet-data-movement-library-app).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="2fdc1-223">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2fdc1-223">Next steps</span></span>
<span data-ttu-id="2fdc1-224">I den här komma igång, skapat vi ett program som samverkar med Azure Storage och körs på Windows, Linux och macOS.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-224">In this getting started, we created an application that interacts with Azure Storage and runs on Windows, Linux, and macOS.</span></span> <span data-ttu-id="2fdc1-225">Den här komma igång fokuserar på Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-225">This getting started focused on Blob Storage.</span></span> <span data-ttu-id="2fdc1-226">Den här samma kunskapen kan dock vara tillämpade tooFile lagring.</span><span class="sxs-lookup"><span data-stu-id="2fdc1-226">However, this same knowledge can be applied tooFile Storage.</span></span> <span data-ttu-id="2fdc1-227">toolearn fler kolla [referensdokumentationen för Azure Storage Data Movement Library](https://azure.github.io/azure-storage-net-data-movement).</span><span class="sxs-lookup"><span data-stu-id="2fdc1-227">toolearn more, check out [Azure Storage Data Movement Library reference documentation](https://azure.github.io/azure-storage-net-data-movement).</span></span>

[!INCLUDE [storage-try-azure-tools-blobs](../../../includes/storage-try-azure-tools-blobs.md)]





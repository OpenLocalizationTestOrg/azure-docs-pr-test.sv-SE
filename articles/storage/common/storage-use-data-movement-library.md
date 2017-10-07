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
# <a name="transfer-data-with-hello-microsoft-azure-storage-data-movement-library"></a>Överföra Data med hello Microsoft Azure Storage Data Movement Library

## <a name="overview"></a>Översikt
hello Microsoft Azure Storage Data Movement Library är ett bibliotek med öppen källkod för flera plattformar som är utformat för högpresterande överföringen, hämtar och kopiering av Azure Storage-Blobbar och filer. Det här biblioteket är hello core data movement framework som används av [AzCopy](../storage-use-azcopy.md). hello Data Movement Library ger praktiska metoder som inte är tillgängliga i vår traditionella [.NET Azure Storage-klientbibliotek](../blobs/storage-dotnet-how-to-use-blobs.md). Detta inkluderar hello möjlighet tooset hello antalet parallella åtgärder, spåra Överföringsförlopp, återuppta enkelt en annullerad överföring och mycket mer.  

.NET Core, vilket innebär att du kan använda den när du skapar .NET appar för Windows, Linux och macOS används också i det här biblioteket. toolearn mer om .NET Core finns toohello [.NET Core-dokumentation](https://dotnet.github.io/). Det här biblioteket fungerar även för traditionella .NET Framework-appar för Windows. 

Det här dokumentet visar hur toocreate en .NET Core konsolen program som körs på Windows, Linux och macOS och utför hello följande scenarier:

- Ladda upp filer och kataloger tooBlob lagring.
- Definiera hello antalet parallella åtgärder vid överföring av data.
- Spåra förloppet för data transfer.
- Återuppta avbrutna dataöverföring. 
- Kopiera filen från URL: en tooBlob lagring. 
- Kopiera från Blob Storage tooBlob lagring.

**Vad du behöver:**

* [Visual Studio Code](https://code.visualstudio.com/)
* Ett [Azure Storage-konto](storage-create-storage-account.md#create-a-storage-account)

> [!NOTE]
> Den här handboken förutsätts att du redan är bekant med [Azure Storage](https://azure.microsoft.com/services/storage/). Om inte, läsning hello [introduktion tooAzure lagring](storage-introduction.md) dokumentation är bra. Viktigast av allt du behöver för[skapa ett lagringskonto](storage-create-storage-account.md#create-a-storage-account) toostart med hello Data Movement Library.
> 
> 

## <a name="setup"></a>Konfiguration  

1. Besök hello [.NET Core installationsguiden](https://www.microsoft.com/net/core) tooinstall .NET Core. När du väljer din miljö Välj hello kommandoradsalternativet. 
2. Skapa en katalog för ditt projekt från hello kommandorad. Navigera till den här katalogen, Skriv `dotnet new` console toocreate C#-projekt.
3. Öppna den här katalogen i Visual Studio-koden. Det här steget kan utföras snabbt via hello kommandoraden genom att skriva `code .`.  
4. Installera hello [C#-tillägget](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) från hello Visual Studio Code Marketplace. Starta om Visual Studio-koden. 
5. Du bör nu se två frågor. En är för att lägga till ”krävs tillgångar toobuild och debug”. Klicka på ”Ja”. En annan frågan är för att återställa Olösta beroenden. Klicka på ”Återställ”.
6. Programmet innehåller nu en `launch.json` fil under hello `.vscode` directory. I den här filen, ändrar du hello `externalConsole` värdet för`true`.
7. Visual Studio Code kan du toodebug .NET Core program. Träffa `F5` toorun ditt program och kontrollera att inställningarna fungerar. Du bör se ”Hello World”! utskrivna toohello konsol. 

## <a name="add-data-movement-library-tooyour-project"></a>Lägga till Data Movement Library tooyour projekt

1. Lägg till hello senaste versionen av hello Data Movement Library toohello `dependencies` avsnitt i din `project.json` fil. Den här versionen är samtidigt hello skrivning`"Microsoft.Azure.Storage.DataMovement": "0.5.0"` 
2. Lägg till `"portable-net45+win8"` toohello `imports` avsnitt. 
3. En fråga ska visa toorestore projektet. Klicka hello ”återställa”. Du kan även återställa projektet från hello Kommandotolken genom att skriva hello `dotnet restore` i projektkatalogen hello rot.

Ändra `project.json`:

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

## <a name="set-up-hello-skeleton-of-your-application"></a>Ställ in hello stommen i ditt program
hello ställs första vi gör in hello ”stommen” koden för vårt program. Den här koden oss uppmanas att ange ett namn och lagringskontonyckel och använder dessa autentiseringsuppgifter toocreate en `CloudStorageAccount` objekt. Det här objektet är används toointeract med våra Storage-konto i alla scenarier för överföring. hello kod uppmanar oss också toochoose hello typ av överföringen som vi vill gärna tooexecute. 

Ändra `Program.cs`:

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

## <a name="transfer-local-file-tooazure-blob"></a>Överför lokal fil tooAzure Blob
Lägga till hello metoder `GetSourcePath` och `GetBlob` för`Program.cs`:

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

Ändra hello `TransferLocalFileToAzureBlob` metoden:

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

Den här koden efterfrågar oss hello sökvägen tooa lokal fil och hello namnet på en ny eller befintlig behållare hello namnet på en ny blob. Hej `TransferManager.UploadAsync` metoden utför hello Överför med den här informationen. 

Träffa `F5` toorun ditt program. Du kan verifiera att hello överför uppstod genom att visa ditt lagringskonto med hello [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com/).

## <a name="set-number-of-parallel-operations"></a>Ange antalet parallella åtgärder
En bra funktion som erbjuds av hello Data Movement Library är hello möjlighet tooset hello antalet parallella åtgärder tooincrease hello data transfer genomflöde. Som standard anges hello Data Movement Library hello antalet parallella åtgärder too8 * hello antal kärnor på datorn. 

Tänk på att många parallella åtgärder i en miljö med låg bandbredd kan överväldigande hello nätverksanslutning och faktiskt förhindra operations fullständigt slutförs. Du behöver tooexperiment med den här inställningen toodetermine vad som fungerar bäst baserat på din tillgängliga nätverksbandbredd. 

Lägg till kod som gör att vi tooset hello antalet parallella åtgärder. Lägg till kod som gånger hur lång tid det tar för hello överföring toocomplete också.

Lägg till en `SetNumberOfParallelOperations` metod för`Program.cs`:

```csharp
public static void SetNumberOfParallelOperations()
{
    Console.WriteLine("\nHow many parallel operations would you like toouse?");
    string parallelOperations = Console.ReadLine();
    TransferManager.Configurations.ParallelOperations = int.Parse(parallelOperations);
}
```

Ändra hello `ExecuteChoice` metoden toouse `SetNumberOfParallelOperations`:

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

Ändra hello `TransferLocalFileToAzureBlob` metoden toouse en timer:

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

## <a name="track-transfer-progress"></a>Spåra förloppet för överföring
Det är bra att känna till hur lång tid det tog för våra data tootransfer. Men som kan toosee hello förloppet för vår överföring *under* hello överföringen blir ännu bättre. tooachieve i det här scenariot måste toocreate en `TransferContext` objekt. Hej `TransferContext` objektet kommer på två sätt: `SingleTransferContext` och `DirectoryTransferContext`. hello tidigare är för att överföra en fil (vilket är vad vi gör nu) och hello senare för att överföra en katalog med filer (som vi lägger till senare).

Lägga till hello metoder `GetSingleTransferContext` och `GetDirectoryTransferContext` för`Program.cs`: 

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

Ändra hello `TransferLocalFileToAzureBlob` metoden toouse `GetSingleTransferContext`:

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

## <a name="resume-a-canceled-transfer"></a>Återuppta en annullerad överföring
En annan lämplig funktion som erbjuds av hello Data Movement Library är hello möjlighet tooresume avbrutna överföring. Lägg till kod som gör att vi tootemporarily Avbryt hello överföring genom att skriva `c`, och sedan fortsätta hello överföring 3 sekunder senare.

Ändra `TransferLocalFileToAzureBlob`:

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

Fram till nu vår `checkpoint` alltid värdet för`null`. Om vi avbryter hello överföring vi nu hämta hello senaste kontrollpunkten på vår överföring och använda den här nya kontrollpunkten i vår kontext för överföring. 

## <a name="transfer-local-directory-tooazure-blob-directory"></a>Överför lokal katalog tooAzure Blob directory
Det vore förväntar dig om hello Data Movement Library kan bara överföra en fil i taget. Detta är som tur är inte hello fallet. hello Data Movement Library innehåller hello möjlighet tootransfer en katalog med filer och alla dess underkataloger. Lägg till kod som gör att vi toodo precis så.

Lägg först till hello metoden `GetBlobDirectory` för`Program.cs`:

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

Ändra sedan `TransferLocalDirectoryToAzureBlobDirectory`:

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

Det finns några skillnader mellan den här metoden och hello metod för att överföra en fil. Nu ska du använda `TransferManager.UploadDirectoryAsync` och hello `getDirectoryTransferContext` metoden som vi skapade tidigare. Dessutom kan vi ger nu en `options` värdet tooour överföringen, vilket gör att vi tooindicate som vi vill tooinclude underkataloger i vår överföringen. 

## <a name="copy-file-from-url-tooazure-blob"></a>Kopiera filen från URL: en tooAzure Blob
Nu ska vi lägga till kod som gör att vi toocopy en fil från en URL-tooan Azure Blob. 

Ändra `TransferUrlToAzureBlob`:

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

Det är en viktig användningsfall för den här funktionen när du behöver toomove data från en annan cloud service (t.ex. AWS) tooAzure. Så länge som du har en URL som ger du åtkomst till toohello resurs, kan du enkelt flytta resursen i Azure BLOB med hjälp av hello `TransferManager.CopyAsync` metod. Den här metoden skapar också en ny boolesk parameter. Ange den här parametern för`true` betyder det att vi vill toodo asynkron serversidan kopia. Inställningen för den här parametern`false` anger en synkron kopia - vilket innebär att hello resursen är hämtade tooour lokala datorn först, sedan överföra tooAzure Blob. Dock finns synkron kopia för närvarande bara för att kopiera från en tooanother för Azure Storage-resurs. 

## <a name="transfer-azure-blob-tooazure-blob"></a>Överför Azure Blob tooAzure Blob
En annan funktion som unikt tillhandahålls av hello Data Movement Library är hello möjlighet toocopy från en tooanother för Azure Storage-resurs. 

Ändra `TransferAzureBlobToAzureBlob`:

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

I det här exemplet anger vi hello boolesk parameter i `TransferManager.CopyAsync` för`false` tooindicate som vi vill toodo en synkron kopia. Det innebär att hello resursen är hämtade tooour lokala datorn först och sedan överföra tooAzure Blob. hello synkron kopiera alternativet är ett bra sätt tooensure att din Kopieringsåtgärden har en konsekvent hastighet. Däremot är hello hastigheten för en asynkron serversidan kopia beroende av hello bandbredden på hello servern som kan variera. Synkron copy kan dock ge ytterligare utgång kostnaden jämfört med tooasynchronous kopia. hello rekommenderade metoden är toouse synkron kopia i en Azure VM i hello samma region som din datakälla konto tooavoid utgång lagringskostnaden.

## <a name="conclusion"></a>Slutsats
Vårt program för flytt av data är slutförd. [hello fullständig kodexempel är tillgängliga på GitHub](https://github.com/azure-samples/storage-dotnet-data-movement-library-app). 

## <a name="next-steps"></a>Nästa steg
I den här komma igång, skapat vi ett program som samverkar med Azure Storage och körs på Windows, Linux och macOS. Den här komma igång fokuserar på Blob Storage. Den här samma kunskapen kan dock vara tillämpade tooFile lagring. toolearn fler kolla [referensdokumentationen för Azure Storage Data Movement Library](https://azure.github.io/azure-storage-net-data-movement).

[!INCLUDE [storage-try-azure-tools-blobs](../../../includes/storage-try-azure-tools-blobs.md)]





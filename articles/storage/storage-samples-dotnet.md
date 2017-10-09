---
title: "aaaAzure lagring exempel med hjälp av .NET | Microsoft Docs"
description: "Visa, hämta och köra exempelkod och program för Azure Storage. Identifiera komma igång prover för blobbar, köer, tabeller och filer med hjälp av hello lagringsklientbiblioteken för .NET."
services: storage
documentationcenter: na
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: 
ms.service: storage
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 01/12/2017
ms.author: seguler
ms.openlocfilehash: 9a7055645b0f0658b850f024b8b19ab19840330e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-samples-using-net"></a><span data-ttu-id="f1437-104">Azure Storage-exempel med hjälp av .NET</span><span class="sxs-lookup"><span data-stu-id="f1437-104">Azure Storage samples using .NET</span></span>

## <a name="net-sample-index"></a><span data-ttu-id="f1437-105">Index för .NET-exempel</span><span class="sxs-lookup"><span data-stu-id="f1437-105">.NET sample index</span></span>

<span data-ttu-id="f1437-106">hello följande tabell ger en översikt över våra exempel databasen och hello scenarier beskrivs i varje prov.</span><span class="sxs-lookup"><span data-stu-id="f1437-106">hello following table provides an overview of our samples repository and hello scenarios covered in each sample.</span></span> <span data-ttu-id="f1437-107">Klicka på hello länkar tooview hello motsvarande exempelkoden i GitHub.</span><span class="sxs-lookup"><span data-stu-id="f1437-107">Click on hello links tooview hello corresponding sample code in GitHub.</span></span>

<table style="font-size:90%"><thead><tr><th style="font-size:110%"><span data-ttu-id="f1437-108">Slutpunkt</span><span class="sxs-lookup"><span data-stu-id="f1437-108">Endpoint</span></span></th><th style="font-size:110%"><span data-ttu-id="f1437-109">Scenario</span><span class="sxs-lookup"><span data-stu-id="f1437-109">Scenario</span></span></th><th style="font-size:110%"><span data-ttu-id="f1437-110">Exempelkod</span><span class="sxs-lookup"><span data-stu-id="f1437-110">Sample Code</span></span></th></tr></thead><tbody> 
<tr> 
<td rowspan="16"><span data-ttu-id="f1437-111"><b>BLOB</b></span><span class="sxs-lookup"><span data-stu-id="f1437-111"><b>Blob</b></span></span></td>
<td><span data-ttu-id="f1437-112">Lägg till Blob</span><span class="sxs-lookup"><span data-stu-id="f1437-112">Append Blob</span></span></td> 
<td><span data-ttu-id="f1437-113"><a href="https://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.getappendblobreference.aspx">Metoden CloudBlobContainer.GetAppendBlobReference exempel</a></span><span class="sxs-lookup"><span data-stu-id="f1437-113"><a href="https://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.getappendblobreference.aspx">CloudBlobContainer.GetAppendBlobReference Method Example</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f1437-114">Blockblob</span><span class="sxs-lookup"><span data-stu-id="f1437-114">Block Blob</span></span></td>
<td><span data-ttu-id="f1437-115"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery webbprogram</a></span><span class="sxs-lookup"><span data-stu-id="f1437-115"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery Web Application</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="f1437-116">Kryptering av klientsidan</span><span class="sxs-lookup"><span data-stu-id="f1437-116">Client-Side Encryption</span></span></td>
<td><span data-ttu-id="f1437-117"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/BlobGettingStarted/Program.cs">Exempel för BLOB-kryptering</a></span><span class="sxs-lookup"><span data-stu-id="f1437-117"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/BlobGettingStarted/Program.cs">Blob Encryption Samples</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="f1437-118">Kopiera Blob</span><span class="sxs-lookup"><span data-stu-id="f1437-118">Copy Blob</span></span></td>
<td><span data-ttu-id="f1437-119"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Komma igång med Blobbar</a></span><span class="sxs-lookup"><span data-stu-id="f1437-119"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="f1437-120">Skapa en behållare</span><span class="sxs-lookup"><span data-stu-id="f1437-120">Create Container</span></span></td>
<td><span data-ttu-id="f1437-121"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery webbprogram</a></span><span class="sxs-lookup"><span data-stu-id="f1437-121"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery Web Application</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="f1437-122">Ta bort blobben</span><span class="sxs-lookup"><span data-stu-id="f1437-122">Delete Blob</span></span></td>
<td><span data-ttu-id="f1437-123"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery webbprogram</a></span><span class="sxs-lookup"><span data-stu-id="f1437-123"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery Web Application</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="f1437-124">Ta bort behållaren</span><span class="sxs-lookup"><span data-stu-id="f1437-124">Delete Container</span></span></td>
<td><span data-ttu-id="f1437-125"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Komma igång med Blobbar</a></span><span class="sxs-lookup"><span data-stu-id="f1437-125"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="f1437-126">BLOB Metadata/egenskaper/Stats</span><span class="sxs-lookup"><span data-stu-id="f1437-126">Blob Metadata/Properties/Stats</span></span></td>
<td><span data-ttu-id="f1437-127"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Komma igång med Blobbar</a></span><span class="sxs-lookup"><span data-stu-id="f1437-127"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="f1437-128">Behållaren-ACL/metadataegenskaper</span><span class="sxs-lookup"><span data-stu-id="f1437-128">Container ACL/Metadata/Properties</span></span></td>
<td><span data-ttu-id="f1437-129"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery webbprogram</a></span><span class="sxs-lookup"><span data-stu-id="f1437-129"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery Web Application</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="f1437-130">Get-sidintervall</span><span class="sxs-lookup"><span data-stu-id="f1437-130">Get Page Ranges</span></span></td>
<td><span data-ttu-id="f1437-131"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Komma igång med Blobbar</a></span><span class="sxs-lookup"><span data-stu-id="f1437-131"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="f1437-132">Lånet Blobbehållaren</span><span class="sxs-lookup"><span data-stu-id="f1437-132">Lease Blob/Container</span></span></td>
<td><span data-ttu-id="f1437-133"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Komma igång med Blobbar</a></span><span class="sxs-lookup"><span data-stu-id="f1437-133"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="f1437-134">Lista Blobbehållaren</span><span class="sxs-lookup"><span data-stu-id="f1437-134">List Blob/Container</span></span></td>
<td><span data-ttu-id="f1437-135"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">Komma igång med Blobbar</a></span><span class="sxs-lookup"><span data-stu-id="f1437-135"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="f1437-136">Sidblob</span><span class="sxs-lookup"><span data-stu-id="f1437-136">Page Blob</span></span></td>
<td><span data-ttu-id="f1437-137"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">Komma igång med Blobbar</a></span><span class="sxs-lookup"><span data-stu-id="f1437-137"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">Getting Started with Blobs</a></span></span></td>
</tr>
<tr> 
<td><span data-ttu-id="f1437-138">SAS</span><span class="sxs-lookup"><span data-stu-id="f1437-138">SAS</span></span></td>
<td><span data-ttu-id="f1437-139"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Komma igång med Blobbar</a></span><span class="sxs-lookup"><span data-stu-id="f1437-139"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr>   
<tr> 
<td><span data-ttu-id="f1437-140">Tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="f1437-140">Service Properties</span></span></td>
<td><span data-ttu-id="f1437-141"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Komma igång med Blobbar</a></span><span class="sxs-lookup"><span data-stu-id="f1437-141"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr>           
<tr> 
<td><span data-ttu-id="f1437-142">Snapshot-Blob</span><span class="sxs-lookup"><span data-stu-id="f1437-142">Snapshot Blob</span></span></td>
<td><span data-ttu-id="f1437-143"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-back-up-with-incremental-snapshots/blob/master/Program.cs">Säkerhetskopiering Azure virtuella diskar med inkrementell ögonblicksbilder</a></span><span class="sxs-lookup"><span data-stu-id="f1437-143"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-back-up-with-incremental-snapshots/blob/master/Program.cs">Backup Azure Virtual Machine Disks with Incremental Snapshots</a></span></span></td>
</tr> 
<tr> 
<td rowspan="9"><span data-ttu-id="f1437-144"><b>Fil</b></span><span class="sxs-lookup"><span data-stu-id="f1437-144"><b>File</b></span></span></td>
<td><span data-ttu-id="f1437-145">Skapa resurser-kataloger-filer</span><span class="sxs-lookup"><span data-stu-id="f1437-145">Create Shares/Directories/Files</span></span></td> 
<td><span data-ttu-id="f1437-146"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET lagring exempel</a></span><span class="sxs-lookup"><span data-stu-id="f1437-146"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="f1437-147">Ta bort resurser-kataloger-filer</span><span class="sxs-lookup"><span data-stu-id="f1437-147">Delete Shares/Directories/Files</span></span></td> 
<td><span data-ttu-id="f1437-148"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/master/FileStorage/GettingStarted.cs">Komma igång med Azure File Service i .NET</a></span><span class="sxs-lookup"><span data-stu-id="f1437-148"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/master/FileStorage/GettingStarted.cs">Getting Started with Azure File Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f1437-149">Directory egenskaper/Metadata</span><span class="sxs-lookup"><span data-stu-id="f1437-149">Directory Properties/Metadata</span></span></td> 
<td><span data-ttu-id="f1437-150"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET lagring exempel</a></span><span class="sxs-lookup"><span data-stu-id="f1437-150"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f1437-151">Hämta filer</span><span class="sxs-lookup"><span data-stu-id="f1437-151">Download Files</span></span></td> 
<td><span data-ttu-id="f1437-152"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET lagring exempel</a></span><span class="sxs-lookup"><span data-stu-id="f1437-152"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f1437-153">Filen mått-egenskaper/Metadata</span><span class="sxs-lookup"><span data-stu-id="f1437-153">File Properties/Metadata/Metrics</span></span></td> 
<td><span data-ttu-id="f1437-154"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET lagring exempel</a></span><span class="sxs-lookup"><span data-stu-id="f1437-154"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f1437-155">Egenskaper för filtjänsten</span><span class="sxs-lookup"><span data-stu-id="f1437-155">File Service Properties</span></span></td> 
<td><span data-ttu-id="f1437-156"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET lagring exempel</a></span><span class="sxs-lookup"><span data-stu-id="f1437-156"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f1437-157">Lista över kataloger och filer</span><span class="sxs-lookup"><span data-stu-id="f1437-157">List Directories and Files</span></span></td> 
<td><span data-ttu-id="f1437-158"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET lagring exempel</a></span><span class="sxs-lookup"><span data-stu-id="f1437-158"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="f1437-159">Lista över resurser</span><span class="sxs-lookup"><span data-stu-id="f1437-159">List Shares</span></span></td> 
<td><span data-ttu-id="f1437-160"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET lagring exempel</a></span><span class="sxs-lookup"><span data-stu-id="f1437-160"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="f1437-161">Dela Stats-egenskaper/Metadata</span><span class="sxs-lookup"><span data-stu-id="f1437-161">Share Properties/Metadata/Stats</span></span></td> 
<td><span data-ttu-id="f1437-162"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET lagring exempel</a></span><span class="sxs-lookup"><span data-stu-id="f1437-162"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr>
<tr> 
<td rowspan="8"><span data-ttu-id="f1437-163"><b>Kön</b></span><span class="sxs-lookup"><span data-stu-id="f1437-163"><b>Queue</b></span></span></td>
<td><span data-ttu-id="f1437-164">Lägga till meddelande</span><span class="sxs-lookup"><span data-stu-id="f1437-164">Add Message</span></span></td> 
<td><span data-ttu-id="f1437-165"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Komma igång med Azure-kötjänsten i .NET</a></span><span class="sxs-lookup"><span data-stu-id="f1437-165"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f1437-166">Kryptering av klientsidan</span><span class="sxs-lookup"><span data-stu-id="f1437-166">Client-Side Encryption</span></span></td> 
<td><span data-ttu-id="f1437-167"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/QueueGettingStarted/Program.cs">Azure Storage .NET kön klientsidan kryptering</a></span><span class="sxs-lookup"><span data-stu-id="f1437-167"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/QueueGettingStarted/Program.cs">Azure Storage .NET Queue Client-Side Encryption</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f1437-168">Skapa köer</span><span class="sxs-lookup"><span data-stu-id="f1437-168">Create Queues</span></span></td> 
<td><span data-ttu-id="f1437-169"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Komma igång med Azure-kötjänsten i .NET</a></span><span class="sxs-lookup"><span data-stu-id="f1437-169"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f1437-170">Ta bort meddelandekö /</span><span class="sxs-lookup"><span data-stu-id="f1437-170">Delete Message/Queue</span></span></td> 
<td><span data-ttu-id="f1437-171"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Komma igång med Azure-kötjänsten i .NET</a></span><span class="sxs-lookup"><span data-stu-id="f1437-171"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f1437-172">Granska meddelande</span><span class="sxs-lookup"><span data-stu-id="f1437-172">Peek Message</span></span></td> 
<td><span data-ttu-id="f1437-173"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Komma igång med Azure-kötjänsten i .NET</a></span><span class="sxs-lookup"><span data-stu-id="f1437-173"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f1437-174">Kön ACL/Metadata/Stats</span><span class="sxs-lookup"><span data-stu-id="f1437-174">Queue ACL/Metadata/Stats</span></span></td> 
<td><span data-ttu-id="f1437-175"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">Komma igång med Azure-kötjänsten i .NET</a></span><span class="sxs-lookup"><span data-stu-id="f1437-175"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f1437-176">Egenskaper för kön</span><span class="sxs-lookup"><span data-stu-id="f1437-176">Queue Service Properties</span></span></td> 
<td><span data-ttu-id="f1437-177"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">Komma igång med Azure-kötjänsten i .NET</a></span><span class="sxs-lookup"><span data-stu-id="f1437-177"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f1437-178">Uppdatera meddelande</span><span class="sxs-lookup"><span data-stu-id="f1437-178">Update Message</span></span></td> 
<td><span data-ttu-id="f1437-179"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Komma igång med Azure-kötjänsten i .NET</a></span><span class="sxs-lookup"><span data-stu-id="f1437-179"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td rowspan="7"><span data-ttu-id="f1437-180"><b>Tabell</b></span><span class="sxs-lookup"><span data-stu-id="f1437-180"><b>Table</b></span></span></td>
<td><span data-ttu-id="f1437-181">Skapa tabell</span><span class="sxs-lookup"><span data-stu-id="f1437-181">Create Table</span></span></td> 
<td><span data-ttu-id="f1437-182"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Hantera samtidighet med hjälp av Azure Storage - exempelprogram</a></span><span class="sxs-lookup"><span data-stu-id="f1437-182"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Managing Concurrency using Azure Storage - Sample Application</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f1437-183">Ta bort en entitet/tabell</span><span class="sxs-lookup"><span data-stu-id="f1437-183">Delete Entity/Table</span></span></td> 
<td><span data-ttu-id="f1437-184"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Komma igång med Azure Table Storage i .NET</a></span><span class="sxs-lookup"><span data-stu-id="f1437-184"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Getting Started with Azure Table Storage in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f1437-185">Infoga/Merge/ersätta entitet</span><span class="sxs-lookup"><span data-stu-id="f1437-185">Insert/Merge/Replace Entity</span></span></td> 
<td><span data-ttu-id="f1437-186"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Hantera samtidighet med hjälp av Azure Storage - exempelprogram</a></span><span class="sxs-lookup"><span data-stu-id="f1437-186"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Managing Concurrency using Azure Storage - Sample Application</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f1437-187">Fråga entiteter</span><span class="sxs-lookup"><span data-stu-id="f1437-187">Query Entities</span></span></td> 
<td><span data-ttu-id="f1437-188"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Komma igång med Azure Table Storage i .NET</a></span><span class="sxs-lookup"><span data-stu-id="f1437-188"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Getting Started with Azure Table Storage in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f1437-189">Frågetabeller</span><span class="sxs-lookup"><span data-stu-id="f1437-189">Query Tables</span></span></td> 
<td><span data-ttu-id="f1437-190"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Komma igång med Azure Table Storage i .NET</a></span><span class="sxs-lookup"><span data-stu-id="f1437-190"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Getting Started with Azure Table Storage in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f1437-191">ACL/Tabellegenskaper</span><span class="sxs-lookup"><span data-stu-id="f1437-191">Table ACL/Properties</span></span></td> 
<td><span data-ttu-id="f1437-192"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/AdvancedSamples.cs">Komma igång med Azure Table Storage i .NET</a></span><span class="sxs-lookup"><span data-stu-id="f1437-192"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/AdvancedSamples.cs">Getting Started with Azure Table Storage in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f1437-193">Uppdatera entitet</span><span class="sxs-lookup"><span data-stu-id="f1437-193">Update Entity</span></span></td> 
<td><span data-ttu-id="f1437-194"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Hantera samtidighet med hjälp av Azure Storage - exempelprogram</a></span><span class="sxs-lookup"><span data-stu-id="f1437-194"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Managing Concurrency using Azure Storage - Sample Application</a></span></span></td> 
</tr> 
</tbody> 
</table>
<br/>

## <a name="azure-code-samples-library"></a><span data-ttu-id="f1437-195">Azure-kodexempel-bibliotek</span><span class="sxs-lookup"><span data-stu-id="f1437-195">Azure Code Samples library</span></span>

<span data-ttu-id="f1437-196">tooview hello fullständigt exempel bibliotek, gå toohello [Azure kodexempel](https://azure.microsoft.com/resources/samples/?service=storage) biblioteket, som innehåller exempel för Azure Storage som du kan hämta och kör lokalt.</span><span class="sxs-lookup"><span data-stu-id="f1437-196">tooview hello complete sample library, go toohello [Azure Code Samples](https://azure.microsoft.com/resources/samples/?service=storage) library, which includes samples for Azure Storage that you can download and run locally.</span></span> <span data-ttu-id="f1437-197">hello kod exempel biblioteket innehåller exempelkod i ZIP-format.</span><span class="sxs-lookup"><span data-stu-id="f1437-197">hello Code Sample Library provides sample code in .zip format.</span></span> <span data-ttu-id="f1437-198">Du kan också bläddra och klona hello GitHub-lagringsplatsen för varje prov.</span><span class="sxs-lookup"><span data-stu-id="f1437-198">Alternatively, you can browse and clone hello GitHub repository for each sample.</span></span>

[!INCLUDE [storage-dotnet-samples-include](../../includes/storage-dotnet-samples-include.md)]

## <a name="getting-started-guides"></a><span data-ttu-id="f1437-199">Komma igång guider</span><span class="sxs-lookup"><span data-stu-id="f1437-199">Getting started guides</span></span>

<span data-ttu-id="f1437-200">Kolla in hello följande guider om du letar efter information om hur tooinstall och komma igång med hello Azure Storage, Klientbiblioteken.</span><span class="sxs-lookup"><span data-stu-id="f1437-200">Check out hello following guides if you are looking for instructions on how tooinstall and get started with hello Azure Storage Client Libraries.</span></span>

* [<span data-ttu-id="f1437-201">Komma igång med Azure Blob-tjänsten i .NET</span><span class="sxs-lookup"><span data-stu-id="f1437-201">Getting Started with Azure Blob Service in .NET</span></span>](storage-dotnet-how-to-use-blobs.md)
* [<span data-ttu-id="f1437-202">Komma igång med Azure-kötjänsten i .NET</span><span class="sxs-lookup"><span data-stu-id="f1437-202">Getting Started with Azure Queue Service in .NET</span></span>](storage-dotnet-how-to-use-queues.md)
* [<span data-ttu-id="f1437-203">Komma igång med Azure-tabellen Service i .NET</span><span class="sxs-lookup"><span data-stu-id="f1437-203">Getting Started with Azure Table Service in .NET</span></span>](storage-dotnet-how-to-use-tables.md)
* [<span data-ttu-id="f1437-204">Komma igång med Azure File Service i .NET</span><span class="sxs-lookup"><span data-stu-id="f1437-204">Getting Started with Azure File Service in .NET</span></span>](storage-dotnet-how-to-use-files.md)

## <a name="next-steps"></a><span data-ttu-id="f1437-205">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f1437-205">Next steps</span></span>

<span data-ttu-id="f1437-206">Information om prover för andra språk:</span><span class="sxs-lookup"><span data-stu-id="f1437-206">For information on samples for other languages:</span></span>

* <span data-ttu-id="f1437-207">Java: [Azure Storage-exempel som använder Java](storage-samples-java.md)</span><span class="sxs-lookup"><span data-stu-id="f1437-207">Java: [Azure Storage samples using Java](storage-samples-java.md)</span></span>
* <span data-ttu-id="f1437-208">Alla andra språk: [Azure Storage-exempel](storage-samples.md)</span><span class="sxs-lookup"><span data-stu-id="f1437-208">All other languages: [Azure Storage samples](storage-samples.md)</span></span>

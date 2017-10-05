---
title: "Azure Storage-exempel med hjälp av .NET | Microsoft Docs"
description: "Visa, hämta och köra exempelkod och program för Azure Storage. Identifiera komma igång prover för blobbar, köer, tabeller och filer, använda lagringsklientbiblioteken för .NET."
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
ms.openlocfilehash: d2b6b3d9483f230ad25ae47255a4f28c1a67e064
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-storage-samples-using-net"></a><span data-ttu-id="a1770-104">Azure Storage-exempel med hjälp av .NET</span><span class="sxs-lookup"><span data-stu-id="a1770-104">Azure Storage samples using .NET</span></span>

## <a name="net-sample-index"></a><span data-ttu-id="a1770-105">Index för .NET-exempel</span><span class="sxs-lookup"><span data-stu-id="a1770-105">.NET sample index</span></span>

<span data-ttu-id="a1770-106">Följande tabell innehåller en översikt över scenarier som tas upp i varje prov och våra exempel-databasen.</span><span class="sxs-lookup"><span data-stu-id="a1770-106">The following table provides an overview of our samples repository and the scenarios covered in each sample.</span></span> <span data-ttu-id="a1770-107">Klicka på länkar för att visa motsvarande exempelkoden i GitHub.</span><span class="sxs-lookup"><span data-stu-id="a1770-107">Click on the links to view the corresponding sample code in GitHub.</span></span>

<table style="font-size:90%"><thead><tr><th style="font-size:110%"><span data-ttu-id="a1770-108">Slutpunkt</span><span class="sxs-lookup"><span data-stu-id="a1770-108">Endpoint</span></span></th><th style="font-size:110%"><span data-ttu-id="a1770-109">Scenario</span><span class="sxs-lookup"><span data-stu-id="a1770-109">Scenario</span></span></th><th style="font-size:110%"><span data-ttu-id="a1770-110">Exempelkod</span><span class="sxs-lookup"><span data-stu-id="a1770-110">Sample Code</span></span></th></tr></thead><tbody> 
<tr> 
<td rowspan="16"><span data-ttu-id="a1770-111"><b>BLOB</b></span><span class="sxs-lookup"><span data-stu-id="a1770-111"><b>Blob</b></span></span></td>
<td><span data-ttu-id="a1770-112">Lägg till Blob</span><span class="sxs-lookup"><span data-stu-id="a1770-112">Append Blob</span></span></td> 
<td><span data-ttu-id="a1770-113"><a href="https://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.getappendblobreference.aspx">Metoden CloudBlobContainer.GetAppendBlobReference exempel</a></span><span class="sxs-lookup"><span data-stu-id="a1770-113"><a href="https://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.getappendblobreference.aspx">CloudBlobContainer.GetAppendBlobReference Method Example</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="a1770-114">Blockblob</span><span class="sxs-lookup"><span data-stu-id="a1770-114">Block Blob</span></span></td>
<td><span data-ttu-id="a1770-115"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery webbprogram</a></span><span class="sxs-lookup"><span data-stu-id="a1770-115"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery Web Application</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="a1770-116">Kryptering av klientsidan</span><span class="sxs-lookup"><span data-stu-id="a1770-116">Client-Side Encryption</span></span></td>
<td><span data-ttu-id="a1770-117"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/BlobGettingStarted/Program.cs">Exempel för BLOB-kryptering</a></span><span class="sxs-lookup"><span data-stu-id="a1770-117"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/BlobGettingStarted/Program.cs">Blob Encryption Samples</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="a1770-118">Kopiera Blob</span><span class="sxs-lookup"><span data-stu-id="a1770-118">Copy Blob</span></span></td>
<td><span data-ttu-id="a1770-119"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Komma igång med Blobbar</a></span><span class="sxs-lookup"><span data-stu-id="a1770-119"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="a1770-120">Skapa en behållare</span><span class="sxs-lookup"><span data-stu-id="a1770-120">Create Container</span></span></td>
<td><span data-ttu-id="a1770-121"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery webbprogram</a></span><span class="sxs-lookup"><span data-stu-id="a1770-121"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery Web Application</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="a1770-122">Ta bort blobben</span><span class="sxs-lookup"><span data-stu-id="a1770-122">Delete Blob</span></span></td>
<td><span data-ttu-id="a1770-123"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery webbprogram</a></span><span class="sxs-lookup"><span data-stu-id="a1770-123"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery Web Application</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="a1770-124">Ta bort behållaren</span><span class="sxs-lookup"><span data-stu-id="a1770-124">Delete Container</span></span></td>
<td><span data-ttu-id="a1770-125"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Komma igång med Blobbar</a></span><span class="sxs-lookup"><span data-stu-id="a1770-125"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="a1770-126">BLOB Metadata/egenskaper/Stats</span><span class="sxs-lookup"><span data-stu-id="a1770-126">Blob Metadata/Properties/Stats</span></span></td>
<td><span data-ttu-id="a1770-127"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Komma igång med Blobbar</a></span><span class="sxs-lookup"><span data-stu-id="a1770-127"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="a1770-128">Behållaren-ACL/metadataegenskaper</span><span class="sxs-lookup"><span data-stu-id="a1770-128">Container ACL/Metadata/Properties</span></span></td>
<td><span data-ttu-id="a1770-129"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery webbprogram</a></span><span class="sxs-lookup"><span data-stu-id="a1770-129"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery Web Application</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="a1770-130">Get-sidintervall</span><span class="sxs-lookup"><span data-stu-id="a1770-130">Get Page Ranges</span></span></td>
<td><span data-ttu-id="a1770-131"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Komma igång med Blobbar</a></span><span class="sxs-lookup"><span data-stu-id="a1770-131"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="a1770-132">Lånet Blobbehållaren</span><span class="sxs-lookup"><span data-stu-id="a1770-132">Lease Blob/Container</span></span></td>
<td><span data-ttu-id="a1770-133"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Komma igång med Blobbar</a></span><span class="sxs-lookup"><span data-stu-id="a1770-133"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="a1770-134">Lista Blobbehållaren</span><span class="sxs-lookup"><span data-stu-id="a1770-134">List Blob/Container</span></span></td>
<td><span data-ttu-id="a1770-135"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">Komma igång med Blobbar</a></span><span class="sxs-lookup"><span data-stu-id="a1770-135"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="a1770-136">Sidblob</span><span class="sxs-lookup"><span data-stu-id="a1770-136">Page Blob</span></span></td>
<td><span data-ttu-id="a1770-137"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">Komma igång med Blobbar</a></span><span class="sxs-lookup"><span data-stu-id="a1770-137"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">Getting Started with Blobs</a></span></span></td>
</tr>
<tr> 
<td><span data-ttu-id="a1770-138">SAS</span><span class="sxs-lookup"><span data-stu-id="a1770-138">SAS</span></span></td>
<td><span data-ttu-id="a1770-139"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Komma igång med Blobbar</a></span><span class="sxs-lookup"><span data-stu-id="a1770-139"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr>   
<tr> 
<td><span data-ttu-id="a1770-140">Tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="a1770-140">Service Properties</span></span></td>
<td><span data-ttu-id="a1770-141"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Komma igång med Blobbar</a></span><span class="sxs-lookup"><span data-stu-id="a1770-141"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr>           
<tr> 
<td><span data-ttu-id="a1770-142">Snapshot-Blob</span><span class="sxs-lookup"><span data-stu-id="a1770-142">Snapshot Blob</span></span></td>
<td><span data-ttu-id="a1770-143"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-back-up-with-incremental-snapshots/blob/master/Program.cs">Säkerhetskopiering Azure virtuella diskar med inkrementell ögonblicksbilder</a></span><span class="sxs-lookup"><span data-stu-id="a1770-143"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-back-up-with-incremental-snapshots/blob/master/Program.cs">Backup Azure Virtual Machine Disks with Incremental Snapshots</a></span></span></td>
</tr> 
<tr> 
<td rowspan="9"><span data-ttu-id="a1770-144"><b>Fil</b></span><span class="sxs-lookup"><span data-stu-id="a1770-144"><b>File</b></span></span></td>
<td><span data-ttu-id="a1770-145">Skapa resurser-kataloger-filer</span><span class="sxs-lookup"><span data-stu-id="a1770-145">Create Shares/Directories/Files</span></span></td> 
<td><span data-ttu-id="a1770-146"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET lagring exempel</a></span><span class="sxs-lookup"><span data-stu-id="a1770-146"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="a1770-147">Ta bort resurser-kataloger-filer</span><span class="sxs-lookup"><span data-stu-id="a1770-147">Delete Shares/Directories/Files</span></span></td> 
<td><span data-ttu-id="a1770-148"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/master/FileStorage/GettingStarted.cs">Komma igång med Azure File Service i .NET</a></span><span class="sxs-lookup"><span data-stu-id="a1770-148"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/master/FileStorage/GettingStarted.cs">Getting Started with Azure File Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="a1770-149">Directory egenskaper/Metadata</span><span class="sxs-lookup"><span data-stu-id="a1770-149">Directory Properties/Metadata</span></span></td> 
<td><span data-ttu-id="a1770-150"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET lagring exempel</a></span><span class="sxs-lookup"><span data-stu-id="a1770-150"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="a1770-151">Hämta filer</span><span class="sxs-lookup"><span data-stu-id="a1770-151">Download Files</span></span></td> 
<td><span data-ttu-id="a1770-152"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET lagring exempel</a></span><span class="sxs-lookup"><span data-stu-id="a1770-152"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="a1770-153">Filen mått-egenskaper/Metadata</span><span class="sxs-lookup"><span data-stu-id="a1770-153">File Properties/Metadata/Metrics</span></span></td> 
<td><span data-ttu-id="a1770-154"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET lagring exempel</a></span><span class="sxs-lookup"><span data-stu-id="a1770-154"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="a1770-155">Egenskaper för filtjänsten</span><span class="sxs-lookup"><span data-stu-id="a1770-155">File Service Properties</span></span></td> 
<td><span data-ttu-id="a1770-156"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET lagring exempel</a></span><span class="sxs-lookup"><span data-stu-id="a1770-156"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="a1770-157">Lista över kataloger och filer</span><span class="sxs-lookup"><span data-stu-id="a1770-157">List Directories and Files</span></span></td> 
<td><span data-ttu-id="a1770-158"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET lagring exempel</a></span><span class="sxs-lookup"><span data-stu-id="a1770-158"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="a1770-159">Lista över resurser</span><span class="sxs-lookup"><span data-stu-id="a1770-159">List Shares</span></span></td> 
<td><span data-ttu-id="a1770-160"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET lagring exempel</a></span><span class="sxs-lookup"><span data-stu-id="a1770-160"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="a1770-161">Dela Stats-egenskaper/Metadata</span><span class="sxs-lookup"><span data-stu-id="a1770-161">Share Properties/Metadata/Stats</span></span></td> 
<td><span data-ttu-id="a1770-162"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET lagring exempel</a></span><span class="sxs-lookup"><span data-stu-id="a1770-162"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr>
<tr> 
<td rowspan="8"><span data-ttu-id="a1770-163"><b>Kön</b></span><span class="sxs-lookup"><span data-stu-id="a1770-163"><b>Queue</b></span></span></td>
<td><span data-ttu-id="a1770-164">Lägga till meddelande</span><span class="sxs-lookup"><span data-stu-id="a1770-164">Add Message</span></span></td> 
<td><span data-ttu-id="a1770-165"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Komma igång med Azure-kötjänsten i .NET</a></span><span class="sxs-lookup"><span data-stu-id="a1770-165"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="a1770-166">Kryptering av klientsidan</span><span class="sxs-lookup"><span data-stu-id="a1770-166">Client-Side Encryption</span></span></td> 
<td><span data-ttu-id="a1770-167"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/QueueGettingStarted/Program.cs">Azure Storage .NET kön klientsidan kryptering</a></span><span class="sxs-lookup"><span data-stu-id="a1770-167"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/QueueGettingStarted/Program.cs">Azure Storage .NET Queue Client-Side Encryption</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="a1770-168">Skapa köer</span><span class="sxs-lookup"><span data-stu-id="a1770-168">Create Queues</span></span></td> 
<td><span data-ttu-id="a1770-169"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Komma igång med Azure-kötjänsten i .NET</a></span><span class="sxs-lookup"><span data-stu-id="a1770-169"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="a1770-170">Ta bort meddelandekö /</span><span class="sxs-lookup"><span data-stu-id="a1770-170">Delete Message/Queue</span></span></td> 
<td><span data-ttu-id="a1770-171"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Komma igång med Azure-kötjänsten i .NET</a></span><span class="sxs-lookup"><span data-stu-id="a1770-171"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="a1770-172">Granska meddelande</span><span class="sxs-lookup"><span data-stu-id="a1770-172">Peek Message</span></span></td> 
<td><span data-ttu-id="a1770-173"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Komma igång med Azure-kötjänsten i .NET</a></span><span class="sxs-lookup"><span data-stu-id="a1770-173"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="a1770-174">Kön ACL/Metadata/Stats</span><span class="sxs-lookup"><span data-stu-id="a1770-174">Queue ACL/Metadata/Stats</span></span></td> 
<td><span data-ttu-id="a1770-175"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">Komma igång med Azure-kötjänsten i .NET</a></span><span class="sxs-lookup"><span data-stu-id="a1770-175"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="a1770-176">Egenskaper för kön</span><span class="sxs-lookup"><span data-stu-id="a1770-176">Queue Service Properties</span></span></td> 
<td><span data-ttu-id="a1770-177"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">Komma igång med Azure-kötjänsten i .NET</a></span><span class="sxs-lookup"><span data-stu-id="a1770-177"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="a1770-178">Uppdatera meddelande</span><span class="sxs-lookup"><span data-stu-id="a1770-178">Update Message</span></span></td> 
<td><span data-ttu-id="a1770-179"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Komma igång med Azure-kötjänsten i .NET</a></span><span class="sxs-lookup"><span data-stu-id="a1770-179"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td rowspan="7"><span data-ttu-id="a1770-180"><b>Tabell</b></span><span class="sxs-lookup"><span data-stu-id="a1770-180"><b>Table</b></span></span></td>
<td><span data-ttu-id="a1770-181">Skapa tabell</span><span class="sxs-lookup"><span data-stu-id="a1770-181">Create Table</span></span></td> 
<td><span data-ttu-id="a1770-182"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Hantera samtidighet med hjälp av Azure Storage - exempelprogram</a></span><span class="sxs-lookup"><span data-stu-id="a1770-182"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Managing Concurrency using Azure Storage - Sample Application</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="a1770-183">Ta bort en entitet/tabell</span><span class="sxs-lookup"><span data-stu-id="a1770-183">Delete Entity/Table</span></span></td> 
<td><span data-ttu-id="a1770-184"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Komma igång med Azure Table Storage i .NET</a></span><span class="sxs-lookup"><span data-stu-id="a1770-184"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Getting Started with Azure Table Storage in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="a1770-185">Infoga/Merge/ersätta entitet</span><span class="sxs-lookup"><span data-stu-id="a1770-185">Insert/Merge/Replace Entity</span></span></td> 
<td><span data-ttu-id="a1770-186"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Hantera samtidighet med hjälp av Azure Storage - exempelprogram</a></span><span class="sxs-lookup"><span data-stu-id="a1770-186"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Managing Concurrency using Azure Storage - Sample Application</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="a1770-187">Fråga entiteter</span><span class="sxs-lookup"><span data-stu-id="a1770-187">Query Entities</span></span></td> 
<td><span data-ttu-id="a1770-188"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Komma igång med Azure Table Storage i .NET</a></span><span class="sxs-lookup"><span data-stu-id="a1770-188"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Getting Started with Azure Table Storage in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="a1770-189">Frågetabeller</span><span class="sxs-lookup"><span data-stu-id="a1770-189">Query Tables</span></span></td> 
<td><span data-ttu-id="a1770-190"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Komma igång med Azure Table Storage i .NET</a></span><span class="sxs-lookup"><span data-stu-id="a1770-190"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Getting Started with Azure Table Storage in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="a1770-191">ACL/Tabellegenskaper</span><span class="sxs-lookup"><span data-stu-id="a1770-191">Table ACL/Properties</span></span></td> 
<td><span data-ttu-id="a1770-192"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/AdvancedSamples.cs">Komma igång med Azure Table Storage i .NET</a></span><span class="sxs-lookup"><span data-stu-id="a1770-192"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/AdvancedSamples.cs">Getting Started with Azure Table Storage in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="a1770-193">Uppdatera entitet</span><span class="sxs-lookup"><span data-stu-id="a1770-193">Update Entity</span></span></td> 
<td><span data-ttu-id="a1770-194"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Hantera samtidighet med hjälp av Azure Storage - exempelprogram</a></span><span class="sxs-lookup"><span data-stu-id="a1770-194"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Managing Concurrency using Azure Storage - Sample Application</a></span></span></td> 
</tr> 
</tbody> 
</table>
<br/>

## <a name="azure-code-samples-library"></a><span data-ttu-id="a1770-195">Azure-kodexempel-bibliotek</span><span class="sxs-lookup"><span data-stu-id="a1770-195">Azure Code Samples library</span></span>

<span data-ttu-id="a1770-196">Om du vill visa hela exemplet biblioteket, gå till den [Azure kodexempel](https://azure.microsoft.com/resources/samples/?service=storage) biblioteket, som innehåller exempel för Azure Storage som du kan hämta och kör lokalt.</span><span class="sxs-lookup"><span data-stu-id="a1770-196">To view the complete sample library, go to the [Azure Code Samples](https://azure.microsoft.com/resources/samples/?service=storage) library, which includes samples for Azure Storage that you can download and run locally.</span></span> <span data-ttu-id="a1770-197">Koden exempel biblioteket innehåller exempelkod i ZIP-format.</span><span class="sxs-lookup"><span data-stu-id="a1770-197">The Code Sample Library provides sample code in .zip format.</span></span> <span data-ttu-id="a1770-198">Du kan också bläddra och klona lagringsplatsen för varje prov GitHub.</span><span class="sxs-lookup"><span data-stu-id="a1770-198">Alternatively, you can browse and clone the GitHub repository for each sample.</span></span>

[!INCLUDE [storage-dotnet-samples-include](../../includes/storage-dotnet-samples-include.md)]

## <a name="getting-started-guides"></a><span data-ttu-id="a1770-199">Komma igång guider</span><span class="sxs-lookup"><span data-stu-id="a1770-199">Getting started guides</span></span>

<span data-ttu-id="a1770-200">Se följande guider om du letar efter information om hur du installerar och kom igång med Azure Storage-klientbibliotek.</span><span class="sxs-lookup"><span data-stu-id="a1770-200">Check out the following guides if you are looking for instructions on how to install and get started with the Azure Storage Client Libraries.</span></span>

* [<span data-ttu-id="a1770-201">Komma igång med Azure Blob-tjänsten i .NET</span><span class="sxs-lookup"><span data-stu-id="a1770-201">Getting Started with Azure Blob Service in .NET</span></span>](storage-dotnet-how-to-use-blobs.md)
* [<span data-ttu-id="a1770-202">Komma igång med Azure-kötjänsten i .NET</span><span class="sxs-lookup"><span data-stu-id="a1770-202">Getting Started with Azure Queue Service in .NET</span></span>](storage-dotnet-how-to-use-queues.md)
* [<span data-ttu-id="a1770-203">Komma igång med Azure-tabellen Service i .NET</span><span class="sxs-lookup"><span data-stu-id="a1770-203">Getting Started with Azure Table Service in .NET</span></span>](storage-dotnet-how-to-use-tables.md)
* [<span data-ttu-id="a1770-204">Komma igång med Azure File Service i .NET</span><span class="sxs-lookup"><span data-stu-id="a1770-204">Getting Started with Azure File Service in .NET</span></span>](storage-dotnet-how-to-use-files.md)

## <a name="next-steps"></a><span data-ttu-id="a1770-205">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a1770-205">Next steps</span></span>

<span data-ttu-id="a1770-206">Information om prover för andra språk:</span><span class="sxs-lookup"><span data-stu-id="a1770-206">For information on samples for other languages:</span></span>

* <span data-ttu-id="a1770-207">Java: [Azure Storage-exempel som använder Java](storage-samples-java.md)</span><span class="sxs-lookup"><span data-stu-id="a1770-207">Java: [Azure Storage samples using Java](storage-samples-java.md)</span></span>
* <span data-ttu-id="a1770-208">Alla andra språk: [Azure Storage-exempel](storage-samples.md)</span><span class="sxs-lookup"><span data-stu-id="a1770-208">All other languages: [Azure Storage samples](storage-samples.md)</span></span>

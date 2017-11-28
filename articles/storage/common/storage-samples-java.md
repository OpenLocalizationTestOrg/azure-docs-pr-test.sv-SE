---
title: "Azure Storage-exempel som använder Java | Microsoft Docs"
description: "Visa, hämta och köra exempelkod och program för Azure Storage. Identifiera komma igång prover för blobbar, köer, tabeller och filer med hjälp av klientbibliotek för Java-lagring."
services: storage
documentationcenter: na
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: 
ms.service: storage
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 01/12/2017
ms.author: seguler
ms.openlocfilehash: fd27e1ac9a773e7b0f5245aa74acdb0521cd098c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-storage-samples-using-java"></a><span data-ttu-id="0787b-104">Azure Storage-exempel som använder Java</span><span class="sxs-lookup"><span data-stu-id="0787b-104">Azure Storage samples using Java</span></span>

## <a name="java-sample-index"></a><span data-ttu-id="0787b-105">Index för Java-exempel</span><span class="sxs-lookup"><span data-stu-id="0787b-105">Java sample index</span></span>

<span data-ttu-id="0787b-106">Följande tabell innehåller en översikt över scenarier som tas upp i varje prov och våra exempel-databasen.</span><span class="sxs-lookup"><span data-stu-id="0787b-106">The following table provides an overview of our samples repository and the scenarios covered in each sample.</span></span> <span data-ttu-id="0787b-107">Klicka på länkar för att visa motsvarande exempelkoden i GitHub.</span><span class="sxs-lookup"><span data-stu-id="0787b-107">Click on the links to view the corresponding sample code in GitHub.</span></span>

<table style="font-size:90%"><thead><tr><th style="font-size:110%"><span data-ttu-id="0787b-108">Slutpunkt</span><span class="sxs-lookup"><span data-stu-id="0787b-108">Endpoint</span></span></th><th style="font-size:110%"><span data-ttu-id="0787b-109">Scenario</span><span class="sxs-lookup"><span data-stu-id="0787b-109">Scenario</span></span></th><th style="font-size:110%"><span data-ttu-id="0787b-110">Exempelkod</span><span class="sxs-lookup"><span data-stu-id="0787b-110">Sample Code</span></span></th></tr></thead><tbody> 
<tr> 
<td rowspan="16"><span data-ttu-id="0787b-111"><b>BLOB</b></span><span class="sxs-lookup"><span data-stu-id="0787b-111"><b>Blob</b></span></span></td>
<td><span data-ttu-id="0787b-112">Lägg till Blob</span><span class="sxs-lookup"><span data-stu-id="0787b-112">Append Blob</span></span></td> 
<td><span data-ttu-id="0787b-113"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Komma igång med Azure Blob-tjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="0787b-113"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="0787b-114">Blockblob</span><span class="sxs-lookup"><span data-stu-id="0787b-114">Block Blob</span></span></td>
<td><span data-ttu-id="0787b-115"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Komma igång med Azure Blob-tjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="0787b-115"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="0787b-116">Kryptering av klientsidan</span><span class="sxs-lookup"><span data-stu-id="0787b-116">Client-Side Encryption</span></span></td>
<td><span data-ttu-id="0787b-117"><a href="https://github.com/Azure-Samples/storage-java-client-side-encryption">Komma igång med Azure Client Side Encryption i Java</a></span><span class="sxs-lookup"><span data-stu-id="0787b-117"><a href="https://github.com/Azure-Samples/storage-java-client-side-encryption">Getting Started with Azure Client Side Encryption in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="0787b-118">Kopiera Blob</span><span class="sxs-lookup"><span data-stu-id="0787b-118">Copy Blob</span></span></td>
<td><span data-ttu-id="0787b-119"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Komma igång med Azure Blob-tjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="0787b-119"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="0787b-120">Skapa en behållare</span><span class="sxs-lookup"><span data-stu-id="0787b-120">Create Container</span></span></td>
<td><span data-ttu-id="0787b-121"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Komma igång med Azure Blob-tjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="0787b-121"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="0787b-122">Ta bort blobben</span><span class="sxs-lookup"><span data-stu-id="0787b-122">Delete Blob</span></span></td>
<td><span data-ttu-id="0787b-123"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Komma igång med Azure Blob-tjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="0787b-123"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="0787b-124">Ta bort behållaren</span><span class="sxs-lookup"><span data-stu-id="0787b-124">Delete Container</span></span></td>
<td><span data-ttu-id="0787b-125"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Komma igång med Azure Blob-tjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="0787b-125"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="0787b-126">BLOB Metadata/egenskaper/Stats</span><span class="sxs-lookup"><span data-stu-id="0787b-126">Blob Metadata/Properties/Stats</span></span></td>
<td><span data-ttu-id="0787b-127"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Komma igång med Azure Blob-tjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="0787b-127"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="0787b-128">Behållaren-ACL/metadataegenskaper</span><span class="sxs-lookup"><span data-stu-id="0787b-128">Container ACL/Metadata/Properties</span></span></td>
<td><span data-ttu-id="0787b-129"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Komma igång med Azure Blob-tjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="0787b-129"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="0787b-130">Get-sidintervall</span><span class="sxs-lookup"><span data-stu-id="0787b-130">Get Page Ranges</span></span></td>
<td><span data-ttu-id="0787b-131"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/CloudPageBlobTests.java">Sidblob testar exempel</a></span><span class="sxs-lookup"><span data-stu-id="0787b-131"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/CloudPageBlobTests.java">Page Blob Tests Sample</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="0787b-132">Lånet Blobbehållaren</span><span class="sxs-lookup"><span data-stu-id="0787b-132">Lease Blob/Container</span></span></td>
<td><span data-ttu-id="0787b-133"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Komma igång med Azure Blob-tjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="0787b-133"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="0787b-134">Lista Blobbehållaren</span><span class="sxs-lookup"><span data-stu-id="0787b-134">List Blob/Container</span></span></td>
<td><span data-ttu-id="0787b-135"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Komma igång med Azure Blob-tjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="0787b-135"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="0787b-136">Sidblob</span><span class="sxs-lookup"><span data-stu-id="0787b-136">Page Blob</span></span></td>
<td><span data-ttu-id="0787b-137"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Komma igång med Azure Blob-tjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="0787b-137"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr>
<tr> 
<td><span data-ttu-id="0787b-138">SAS</span><span class="sxs-lookup"><span data-stu-id="0787b-138">SAS</span></span></td>
<td><span data-ttu-id="0787b-139"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/SasTests.java">SAS tester exempel</a></span><span class="sxs-lookup"><span data-stu-id="0787b-139"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/SasTests.java">SAS Tests Sample</a></span></span></td>
</tr>   
<tr> 
<td><span data-ttu-id="0787b-140">Tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="0787b-140">Service Properties</span></span></td>
<td><span data-ttu-id="0787b-141"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Komma igång med Azure Blob-tjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="0787b-141"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr>           
<tr> 
<td><span data-ttu-id="0787b-142">Snapshot-Blob</span><span class="sxs-lookup"><span data-stu-id="0787b-142">Snapshot Blob</span></span></td>
<td><span data-ttu-id="0787b-143"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Komma igång med Azure Blob-tjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="0787b-143"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td rowspan="9"><span data-ttu-id="0787b-144"><b>Fil</b></span><span class="sxs-lookup"><span data-stu-id="0787b-144"><b>File</b></span></span></td>
<td><span data-ttu-id="0787b-145">Skapa resurser-kataloger-filer</span><span class="sxs-lookup"><span data-stu-id="0787b-145">Create Shares/Directories/Files</span></span></td> 
<td><span data-ttu-id="0787b-146"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Komma igång med Azure File Service i Java</a></span><span class="sxs-lookup"><span data-stu-id="0787b-146"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="0787b-147">Ta bort resurser-kataloger-filer</span><span class="sxs-lookup"><span data-stu-id="0787b-147">Delete Shares/Directories/Files</span></span></td> 
<td><span data-ttu-id="0787b-148"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Komma igång med Azure File Service i Java</a></span><span class="sxs-lookup"><span data-stu-id="0787b-148"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="0787b-149">Directory egenskaper/Metadata</span><span class="sxs-lookup"><span data-stu-id="0787b-149">Directory Properties/Metadata</span></span></td> 
<td><span data-ttu-id="0787b-150"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Komma igång med Azure File Service i Java</a></span><span class="sxs-lookup"><span data-stu-id="0787b-150"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="0787b-151">Hämta filer</span><span class="sxs-lookup"><span data-stu-id="0787b-151">Download Files</span></span></td> 
<td><span data-ttu-id="0787b-152"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Komma igång med Azure File Service i Java</a></span><span class="sxs-lookup"><span data-stu-id="0787b-152"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="0787b-153">Filen mått-egenskaper/Metadata</span><span class="sxs-lookup"><span data-stu-id="0787b-153">File Properties/Metadata/Metrics</span></span></td> 
<td><span data-ttu-id="0787b-154"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Komma igång med Azure File Service i Java</a></span><span class="sxs-lookup"><span data-stu-id="0787b-154"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="0787b-155">Egenskaper för filtjänsten</span><span class="sxs-lookup"><span data-stu-id="0787b-155">File Service Properties</span></span></td> 
<td><span data-ttu-id="0787b-156"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Komma igång med Azure File Service i Java</a></span><span class="sxs-lookup"><span data-stu-id="0787b-156"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="0787b-157">Lista över kataloger och filer</span><span class="sxs-lookup"><span data-stu-id="0787b-157">List Directories and Files</span></span></td> 
<td><span data-ttu-id="0787b-158"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Komma igång med Azure File Service i Java</a></span><span class="sxs-lookup"><span data-stu-id="0787b-158"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="0787b-159">Lista över resurser</span><span class="sxs-lookup"><span data-stu-id="0787b-159">List Shares</span></span></td> 
<td><span data-ttu-id="0787b-160"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Komma igång med Azure File Service i Java</a></span><span class="sxs-lookup"><span data-stu-id="0787b-160"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="0787b-161">Dela Stats-egenskaper/Metadata</span><span class="sxs-lookup"><span data-stu-id="0787b-161">Share Properties/Metadata/Stats</span></span></td> 
<td><span data-ttu-id="0787b-162"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Komma igång med Azure File Service i Java</a></span><span class="sxs-lookup"><span data-stu-id="0787b-162"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr>
<tr> 
<td rowspan="8"><span data-ttu-id="0787b-163"><b>Kön</b></span><span class="sxs-lookup"><span data-stu-id="0787b-163"><b>Queue</b></span></span></td>
<td><span data-ttu-id="0787b-164">Lägga till meddelande</span><span class="sxs-lookup"><span data-stu-id="0787b-164">Add Message</span></span></td> 
<td><span data-ttu-id="0787b-165"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/queue/gettingstarted/QueueBasics.java">Lagring Java klienten biblioteket prover</a></span><span class="sxs-lookup"><span data-stu-id="0787b-165"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/queue/gettingstarted/QueueBasics.java">Storage Java Client Library Samples</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="0787b-166">Kryptering av klientsidan</span><span class="sxs-lookup"><span data-stu-id="0787b-166">Client-Side Encryption</span></span></td> 
<td><span data-ttu-id="0787b-167"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/encryption/queue/gettingstarted/QueueGettingStarted.java">Lagring Java klienten biblioteket prover</a></span><span class="sxs-lookup"><span data-stu-id="0787b-167"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/encryption/queue/gettingstarted/QueueGettingStarted.java">Storage Java Client Library Samples</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="0787b-168">Skapa köer</span><span class="sxs-lookup"><span data-stu-id="0787b-168">Create Queues</span></span></td> 
<td><span data-ttu-id="0787b-169"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Komma igång med Azure-kötjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="0787b-169"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="0787b-170">Ta bort meddelandekö /</span><span class="sxs-lookup"><span data-stu-id="0787b-170">Delete Message/Queue</span></span></td> 
<td><span data-ttu-id="0787b-171"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Komma igång med Azure-kötjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="0787b-171"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="0787b-172">Granska meddelande</span><span class="sxs-lookup"><span data-stu-id="0787b-172">Peek Message</span></span></td> 
<td><span data-ttu-id="0787b-173"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Komma igång med Azure-kötjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="0787b-173"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="0787b-174">Kön ACL/Metadata/Stats</span><span class="sxs-lookup"><span data-stu-id="0787b-174">Queue ACL/Metadata/Stats</span></span></td> 
<td><span data-ttu-id="0787b-175"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Komma igång med Azure-kötjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="0787b-175"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="0787b-176">Egenskaper för kön</span><span class="sxs-lookup"><span data-stu-id="0787b-176">Queue Service Properties</span></span></td> 
<td><span data-ttu-id="0787b-177"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Komma igång med Azure-kötjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="0787b-177"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="0787b-178">Uppdatera meddelande</span><span class="sxs-lookup"><span data-stu-id="0787b-178">Update Message</span></span></td> 
<td><span data-ttu-id="0787b-179"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Komma igång med Azure-kötjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="0787b-179"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td rowspan="7"><span data-ttu-id="0787b-180"><b>Tabell</b></span><span class="sxs-lookup"><span data-stu-id="0787b-180"><b>Table</b></span></span></td>
<td><span data-ttu-id="0787b-181">Skapa tabell</span><span class="sxs-lookup"><span data-stu-id="0787b-181">Create Table</span></span></td> 
<td><span data-ttu-id="0787b-182"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Komma igång med Azure-tabellen Service i Java</a></span><span class="sxs-lookup"><span data-stu-id="0787b-182"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="0787b-183">Ta bort en entitet/tabell</span><span class="sxs-lookup"><span data-stu-id="0787b-183">Delete Entity/Table</span></span></td> 
<td><span data-ttu-id="0787b-184"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Komma igång med Azure-tabellen Service i Java</a></span><span class="sxs-lookup"><span data-stu-id="0787b-184"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="0787b-185">Infoga/Merge/ersätta entitet</span><span class="sxs-lookup"><span data-stu-id="0787b-185">Insert/Merge/Replace Entity</span></span></td> 
<td><span data-ttu-id="0787b-186"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Lagring Java klienten biblioteket prover</a></span><span class="sxs-lookup"><span data-stu-id="0787b-186"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Storage Java Client Library Samples</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="0787b-187">Fråga entiteter</span><span class="sxs-lookup"><span data-stu-id="0787b-187">Query Entities</span></span></td> 
<td><span data-ttu-id="0787b-188"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Komma igång med Azure-tabellen Service i Java</a></span><span class="sxs-lookup"><span data-stu-id="0787b-188"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="0787b-189">Frågetabeller</span><span class="sxs-lookup"><span data-stu-id="0787b-189">Query Tables</span></span></td> 
<td><span data-ttu-id="0787b-190"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Komma igång med Azure-tabellen Service i Java</a></span><span class="sxs-lookup"><span data-stu-id="0787b-190"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="0787b-191">ACL/Tabellegenskaper</span><span class="sxs-lookup"><span data-stu-id="0787b-191">Table ACL/Properties</span></span></td> 
<td><span data-ttu-id="0787b-192"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableAdvanced.java">Komma igång med Azure-tabellen Service i Java</a></span><span class="sxs-lookup"><span data-stu-id="0787b-192"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableAdvanced.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="0787b-193">Uppdatera entitet</span><span class="sxs-lookup"><span data-stu-id="0787b-193">Update Entity</span></span></td> 
<td><span data-ttu-id="0787b-194"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Lagring Java klienten biblioteket prover</a></span><span class="sxs-lookup"><span data-stu-id="0787b-194"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Storage Java Client Library Samples</a></span></span></td> 
</tr> 
</tbody> 
</table>
<br/>

## <a name="azure-code-samples-library"></a><span data-ttu-id="0787b-195">Azure-kodexempel-bibliotek</span><span class="sxs-lookup"><span data-stu-id="0787b-195">Azure Code Samples library</span></span>

<span data-ttu-id="0787b-196">Om du vill visa hela exemplet biblioteket, gå till den [Azure kodexempel](https://azure.microsoft.com/resources/samples/?service=storage) biblioteket, som innehåller exempel för Azure Storage som du kan hämta och kör lokalt.</span><span class="sxs-lookup"><span data-stu-id="0787b-196">To view the complete sample library, go to the [Azure Code Samples](https://azure.microsoft.com/resources/samples/?service=storage) library, which includes samples for Azure Storage that you can download and run locally.</span></span> <span data-ttu-id="0787b-197">Koden exempel biblioteket innehåller exempelkod i ZIP-format.</span><span class="sxs-lookup"><span data-stu-id="0787b-197">The Code Sample Library provides sample code in .zip format.</span></span> <span data-ttu-id="0787b-198">Du kan också bläddra och klona lagringsplatsen för varje prov GitHub.</span><span class="sxs-lookup"><span data-stu-id="0787b-198">Alternatively, you can browse and clone the GitHub repository for each sample.</span></span>

[!INCLUDE [storage-java-samples-include](../../../includes/storage-java-samples-include.md)]

## <a name="getting-started-guides"></a><span data-ttu-id="0787b-199">Komma igång guider</span><span class="sxs-lookup"><span data-stu-id="0787b-199">Getting started guides</span></span>

<span data-ttu-id="0787b-200">Se följande guider om du letar efter information om hur du installerar och kom igång med Azure Storage-klientbibliotek.</span><span class="sxs-lookup"><span data-stu-id="0787b-200">Check out the following guides if you are looking for instructions on how to install and get started with the Azure Storage Client Libraries.</span></span>

* [<span data-ttu-id="0787b-201">Komma igång med Azure Blob-tjänsten i Java</span><span class="sxs-lookup"><span data-stu-id="0787b-201">Getting Started with Azure Blob Service in Java</span></span>](../blobs/storage-java-how-to-use-blob-storage.md)
* [<span data-ttu-id="0787b-202">Komma igång med Azure-kötjänsten i Java</span><span class="sxs-lookup"><span data-stu-id="0787b-202">Getting Started with Azure Queue Service in Java</span></span>](../storage-java-how-to-use-queue-storage.md)
* [<span data-ttu-id="0787b-203">Komma igång med Azure-tabellen Service i Java</span><span class="sxs-lookup"><span data-stu-id="0787b-203">Getting Started with Azure Table Service in Java</span></span>](../../cosmos-db/table-storage-how-to-use-java.md)
* [<span data-ttu-id="0787b-204">Komma igång med Azure File Service i Java</span><span class="sxs-lookup"><span data-stu-id="0787b-204">Getting Started with Azure File Service in Java</span></span>](../storage-java-how-to-use-file-storage.md)

## <a name="next-steps"></a><span data-ttu-id="0787b-205">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0787b-205">Next steps</span></span>

<span data-ttu-id="0787b-206">Information om prover för andra språk:</span><span class="sxs-lookup"><span data-stu-id="0787b-206">For information on samples for other languages:</span></span>

* <span data-ttu-id="0787b-207">.NET: [azure Storage-exempel med hjälp av .NET](../storage-samples-dotnet.md)</span><span class="sxs-lookup"><span data-stu-id="0787b-207">.NET: [Azure Storage samples using .NET](../storage-samples-dotnet.md)</span></span>
* <span data-ttu-id="0787b-208">Alla andra språk: [Azure Storage-exempel](../storage-samples.md)</span><span class="sxs-lookup"><span data-stu-id="0787b-208">All other languages: [Azure Storage samples](../storage-samples.md)</span></span>

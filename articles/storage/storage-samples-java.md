---
title: "aaaAzure lagring exempel som använder Java | Microsoft Docs"
description: "Visa, hämta och köra exempelkod och program för Azure Storage. Identifiera komma igång prover för blobbar, köer, tabeller och filer med hjälp av hello Java storage, klientbiblioteken."
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
ms.openlocfilehash: e3b8fbe86e82dd58c2a13a3c68760cbf6e9a6e4b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-samples-using-java"></a><span data-ttu-id="dcdbf-104">Azure Storage-exempel som använder Java</span><span class="sxs-lookup"><span data-stu-id="dcdbf-104">Azure Storage samples using Java</span></span>

## <a name="java-sample-index"></a><span data-ttu-id="dcdbf-105">Index för Java-exempel</span><span class="sxs-lookup"><span data-stu-id="dcdbf-105">Java sample index</span></span>

<span data-ttu-id="dcdbf-106">hello följande tabell ger en översikt över våra exempel databasen och hello scenarier beskrivs i varje prov.</span><span class="sxs-lookup"><span data-stu-id="dcdbf-106">hello following table provides an overview of our samples repository and hello scenarios covered in each sample.</span></span> <span data-ttu-id="dcdbf-107">Klicka på hello länkar tooview hello motsvarande exempelkoden i GitHub.</span><span class="sxs-lookup"><span data-stu-id="dcdbf-107">Click on hello links tooview hello corresponding sample code in GitHub.</span></span>

<table style="font-size:90%"><thead><tr><th style="font-size:110%"><span data-ttu-id="dcdbf-108">Slutpunkt</span><span class="sxs-lookup"><span data-stu-id="dcdbf-108">Endpoint</span></span></th><th style="font-size:110%"><span data-ttu-id="dcdbf-109">Scenario</span><span class="sxs-lookup"><span data-stu-id="dcdbf-109">Scenario</span></span></th><th style="font-size:110%"><span data-ttu-id="dcdbf-110">Exempelkod</span><span class="sxs-lookup"><span data-stu-id="dcdbf-110">Sample Code</span></span></th></tr></thead><tbody> 
<tr> 
<td rowspan="16"><span data-ttu-id="dcdbf-111"><b>BLOB</b></span><span class="sxs-lookup"><span data-stu-id="dcdbf-111"><b>Blob</b></span></span></td>
<td><span data-ttu-id="dcdbf-112">Lägg till Blob</span><span class="sxs-lookup"><span data-stu-id="dcdbf-112">Append Blob</span></span></td> 
<td><span data-ttu-id="dcdbf-113"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Komma igång med Azure Blob-tjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-113"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="dcdbf-114">Blockblob</span><span class="sxs-lookup"><span data-stu-id="dcdbf-114">Block Blob</span></span></td>
<td><span data-ttu-id="dcdbf-115"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Komma igång med Azure Blob-tjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-115"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="dcdbf-116">Kryptering av klientsidan</span><span class="sxs-lookup"><span data-stu-id="dcdbf-116">Client-Side Encryption</span></span></td>
<td><span data-ttu-id="dcdbf-117"><a href="https://github.com/Azure-Samples/storage-java-client-side-encryption">Komma igång med Azure Client Side Encryption i Java</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-117"><a href="https://github.com/Azure-Samples/storage-java-client-side-encryption">Getting Started with Azure Client Side Encryption in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="dcdbf-118">Kopiera Blob</span><span class="sxs-lookup"><span data-stu-id="dcdbf-118">Copy Blob</span></span></td>
<td><span data-ttu-id="dcdbf-119"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Komma igång med Azure Blob-tjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-119"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="dcdbf-120">Skapa en behållare</span><span class="sxs-lookup"><span data-stu-id="dcdbf-120">Create Container</span></span></td>
<td><span data-ttu-id="dcdbf-121"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Komma igång med Azure Blob-tjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-121"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="dcdbf-122">Ta bort blobben</span><span class="sxs-lookup"><span data-stu-id="dcdbf-122">Delete Blob</span></span></td>
<td><span data-ttu-id="dcdbf-123"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Komma igång med Azure Blob-tjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-123"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="dcdbf-124">Ta bort behållaren</span><span class="sxs-lookup"><span data-stu-id="dcdbf-124">Delete Container</span></span></td>
<td><span data-ttu-id="dcdbf-125"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Komma igång med Azure Blob-tjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-125"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="dcdbf-126">BLOB Metadata/egenskaper/Stats</span><span class="sxs-lookup"><span data-stu-id="dcdbf-126">Blob Metadata/Properties/Stats</span></span></td>
<td><span data-ttu-id="dcdbf-127"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Komma igång med Azure Blob-tjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-127"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="dcdbf-128">Behållaren-ACL/metadataegenskaper</span><span class="sxs-lookup"><span data-stu-id="dcdbf-128">Container ACL/Metadata/Properties</span></span></td>
<td><span data-ttu-id="dcdbf-129"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Komma igång med Azure Blob-tjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-129"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="dcdbf-130">Get-sidintervall</span><span class="sxs-lookup"><span data-stu-id="dcdbf-130">Get Page Ranges</span></span></td>
<td><span data-ttu-id="dcdbf-131"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/CloudPageBlobTests.java">Sidblob testar exempel</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-131"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/CloudPageBlobTests.java">Page Blob Tests Sample</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="dcdbf-132">Lånet Blobbehållaren</span><span class="sxs-lookup"><span data-stu-id="dcdbf-132">Lease Blob/Container</span></span></td>
<td><span data-ttu-id="dcdbf-133"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Komma igång med Azure Blob-tjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-133"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="dcdbf-134">Lista Blobbehållaren</span><span class="sxs-lookup"><span data-stu-id="dcdbf-134">List Blob/Container</span></span></td>
<td><span data-ttu-id="dcdbf-135"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Komma igång med Azure Blob-tjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-135"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="dcdbf-136">Sidblob</span><span class="sxs-lookup"><span data-stu-id="dcdbf-136">Page Blob</span></span></td>
<td><span data-ttu-id="dcdbf-137"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Komma igång med Azure Blob-tjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-137"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr>
<tr> 
<td><span data-ttu-id="dcdbf-138">SAS</span><span class="sxs-lookup"><span data-stu-id="dcdbf-138">SAS</span></span></td>
<td><span data-ttu-id="dcdbf-139"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/SasTests.java">SAS tester exempel</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-139"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/SasTests.java">SAS Tests Sample</a></span></span></td>
</tr>   
<tr> 
<td><span data-ttu-id="dcdbf-140">Tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="dcdbf-140">Service Properties</span></span></td>
<td><span data-ttu-id="dcdbf-141"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Komma igång med Azure Blob-tjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-141"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr>           
<tr> 
<td><span data-ttu-id="dcdbf-142">Snapshot-Blob</span><span class="sxs-lookup"><span data-stu-id="dcdbf-142">Snapshot Blob</span></span></td>
<td><span data-ttu-id="dcdbf-143"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Komma igång med Azure Blob-tjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-143"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td rowspan="9"><span data-ttu-id="dcdbf-144"><b>Fil</b></span><span class="sxs-lookup"><span data-stu-id="dcdbf-144"><b>File</b></span></span></td>
<td><span data-ttu-id="dcdbf-145">Skapa resurser-kataloger-filer</span><span class="sxs-lookup"><span data-stu-id="dcdbf-145">Create Shares/Directories/Files</span></span></td> 
<td><span data-ttu-id="dcdbf-146"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Komma igång med Azure File Service i Java</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-146"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="dcdbf-147">Ta bort resurser-kataloger-filer</span><span class="sxs-lookup"><span data-stu-id="dcdbf-147">Delete Shares/Directories/Files</span></span></td> 
<td><span data-ttu-id="dcdbf-148"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Komma igång med Azure File Service i Java</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-148"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="dcdbf-149">Directory egenskaper/Metadata</span><span class="sxs-lookup"><span data-stu-id="dcdbf-149">Directory Properties/Metadata</span></span></td> 
<td><span data-ttu-id="dcdbf-150"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Komma igång med Azure File Service i Java</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-150"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="dcdbf-151">Hämta filer</span><span class="sxs-lookup"><span data-stu-id="dcdbf-151">Download Files</span></span></td> 
<td><span data-ttu-id="dcdbf-152"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Komma igång med Azure File Service i Java</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-152"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="dcdbf-153">Filen mått-egenskaper/Metadata</span><span class="sxs-lookup"><span data-stu-id="dcdbf-153">File Properties/Metadata/Metrics</span></span></td> 
<td><span data-ttu-id="dcdbf-154"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Komma igång med Azure File Service i Java</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-154"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="dcdbf-155">Egenskaper för filtjänsten</span><span class="sxs-lookup"><span data-stu-id="dcdbf-155">File Service Properties</span></span></td> 
<td><span data-ttu-id="dcdbf-156"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Komma igång med Azure File Service i Java</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-156"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="dcdbf-157">Lista över kataloger och filer</span><span class="sxs-lookup"><span data-stu-id="dcdbf-157">List Directories and Files</span></span></td> 
<td><span data-ttu-id="dcdbf-158"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Komma igång med Azure File Service i Java</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-158"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="dcdbf-159">Lista över resurser</span><span class="sxs-lookup"><span data-stu-id="dcdbf-159">List Shares</span></span></td> 
<td><span data-ttu-id="dcdbf-160"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Komma igång med Azure File Service i Java</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-160"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="dcdbf-161">Dela Stats-egenskaper/Metadata</span><span class="sxs-lookup"><span data-stu-id="dcdbf-161">Share Properties/Metadata/Stats</span></span></td> 
<td><span data-ttu-id="dcdbf-162"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Komma igång med Azure File Service i Java</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-162"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr>
<tr> 
<td rowspan="8"><span data-ttu-id="dcdbf-163"><b>Kön</b></span><span class="sxs-lookup"><span data-stu-id="dcdbf-163"><b>Queue</b></span></span></td>
<td><span data-ttu-id="dcdbf-164">Lägga till meddelande</span><span class="sxs-lookup"><span data-stu-id="dcdbf-164">Add Message</span></span></td> 
<td><span data-ttu-id="dcdbf-165"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/queue/gettingstarted/QueueBasics.java">Lagring Java klienten biblioteket prover</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-165"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/queue/gettingstarted/QueueBasics.java">Storage Java Client Library Samples</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="dcdbf-166">Kryptering av klientsidan</span><span class="sxs-lookup"><span data-stu-id="dcdbf-166">Client-Side Encryption</span></span></td> 
<td><span data-ttu-id="dcdbf-167"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/encryption/queue/gettingstarted/QueueGettingStarted.java">Lagring Java klienten biblioteket prover</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-167"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/encryption/queue/gettingstarted/QueueGettingStarted.java">Storage Java Client Library Samples</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="dcdbf-168">Skapa köer</span><span class="sxs-lookup"><span data-stu-id="dcdbf-168">Create Queues</span></span></td> 
<td><span data-ttu-id="dcdbf-169"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Komma igång med Azure-kötjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-169"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="dcdbf-170">Ta bort meddelandekö /</span><span class="sxs-lookup"><span data-stu-id="dcdbf-170">Delete Message/Queue</span></span></td> 
<td><span data-ttu-id="dcdbf-171"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Komma igång med Azure-kötjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-171"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="dcdbf-172">Granska meddelande</span><span class="sxs-lookup"><span data-stu-id="dcdbf-172">Peek Message</span></span></td> 
<td><span data-ttu-id="dcdbf-173"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Komma igång med Azure-kötjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-173"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="dcdbf-174">Kön ACL/Metadata/Stats</span><span class="sxs-lookup"><span data-stu-id="dcdbf-174">Queue ACL/Metadata/Stats</span></span></td> 
<td><span data-ttu-id="dcdbf-175"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Komma igång med Azure-kötjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-175"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="dcdbf-176">Egenskaper för kön</span><span class="sxs-lookup"><span data-stu-id="dcdbf-176">Queue Service Properties</span></span></td> 
<td><span data-ttu-id="dcdbf-177"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Komma igång med Azure-kötjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-177"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="dcdbf-178">Uppdatera meddelande</span><span class="sxs-lookup"><span data-stu-id="dcdbf-178">Update Message</span></span></td> 
<td><span data-ttu-id="dcdbf-179"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Komma igång med Azure-kötjänsten i Java</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-179"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td rowspan="7"><span data-ttu-id="dcdbf-180"><b>Tabell</b></span><span class="sxs-lookup"><span data-stu-id="dcdbf-180"><b>Table</b></span></span></td>
<td><span data-ttu-id="dcdbf-181">Skapa tabell</span><span class="sxs-lookup"><span data-stu-id="dcdbf-181">Create Table</span></span></td> 
<td><span data-ttu-id="dcdbf-182"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Komma igång med Azure-tabellen Service i Java</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-182"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="dcdbf-183">Ta bort en entitet/tabell</span><span class="sxs-lookup"><span data-stu-id="dcdbf-183">Delete Entity/Table</span></span></td> 
<td><span data-ttu-id="dcdbf-184"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Komma igång med Azure-tabellen Service i Java</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-184"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="dcdbf-185">Infoga/Merge/ersätta entitet</span><span class="sxs-lookup"><span data-stu-id="dcdbf-185">Insert/Merge/Replace Entity</span></span></td> 
<td><span data-ttu-id="dcdbf-186"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Lagring Java klienten biblioteket prover</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-186"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Storage Java Client Library Samples</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="dcdbf-187">Fråga entiteter</span><span class="sxs-lookup"><span data-stu-id="dcdbf-187">Query Entities</span></span></td> 
<td><span data-ttu-id="dcdbf-188"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Komma igång med Azure-tabellen Service i Java</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-188"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="dcdbf-189">Frågetabeller</span><span class="sxs-lookup"><span data-stu-id="dcdbf-189">Query Tables</span></span></td> 
<td><span data-ttu-id="dcdbf-190"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Komma igång med Azure-tabellen Service i Java</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-190"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="dcdbf-191">ACL/Tabellegenskaper</span><span class="sxs-lookup"><span data-stu-id="dcdbf-191">Table ACL/Properties</span></span></td> 
<td><span data-ttu-id="dcdbf-192"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableAdvanced.java">Komma igång med Azure-tabellen Service i Java</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-192"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableAdvanced.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="dcdbf-193">Uppdatera entitet</span><span class="sxs-lookup"><span data-stu-id="dcdbf-193">Update Entity</span></span></td> 
<td><span data-ttu-id="dcdbf-194"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Lagring Java klienten biblioteket prover</a></span><span class="sxs-lookup"><span data-stu-id="dcdbf-194"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Storage Java Client Library Samples</a></span></span></td> 
</tr> 
</tbody> 
</table>
<br/>

## <a name="azure-code-samples-library"></a><span data-ttu-id="dcdbf-195">Azure-kodexempel-bibliotek</span><span class="sxs-lookup"><span data-stu-id="dcdbf-195">Azure Code Samples library</span></span>

<span data-ttu-id="dcdbf-196">tooview hello fullständigt exempel bibliotek, gå toohello [Azure kodexempel](https://azure.microsoft.com/resources/samples/?service=storage) biblioteket, som innehåller exempel för Azure Storage som du kan hämta och kör lokalt.</span><span class="sxs-lookup"><span data-stu-id="dcdbf-196">tooview hello complete sample library, go toohello [Azure Code Samples](https://azure.microsoft.com/resources/samples/?service=storage) library, which includes samples for Azure Storage that you can download and run locally.</span></span> <span data-ttu-id="dcdbf-197">hello kod exempel biblioteket innehåller exempelkod i ZIP-format.</span><span class="sxs-lookup"><span data-stu-id="dcdbf-197">hello Code Sample Library provides sample code in .zip format.</span></span> <span data-ttu-id="dcdbf-198">Du kan också bläddra och klona hello GitHub-lagringsplatsen för varje prov.</span><span class="sxs-lookup"><span data-stu-id="dcdbf-198">Alternatively, you can browse and clone hello GitHub repository for each sample.</span></span>

[!INCLUDE [storage-java-samples-include](../../includes/storage-java-samples-include.md)]

## <a name="getting-started-guides"></a><span data-ttu-id="dcdbf-199">Komma igång guider</span><span class="sxs-lookup"><span data-stu-id="dcdbf-199">Getting started guides</span></span>

<span data-ttu-id="dcdbf-200">Kolla in hello följande guider om du letar efter information om hur tooinstall och komma igång med hello Azure Storage, Klientbiblioteken.</span><span class="sxs-lookup"><span data-stu-id="dcdbf-200">Check out hello following guides if you are looking for instructions on how tooinstall and get started with hello Azure Storage Client Libraries.</span></span>

* [<span data-ttu-id="dcdbf-201">Komma igång med Azure Blob-tjänsten i Java</span><span class="sxs-lookup"><span data-stu-id="dcdbf-201">Getting Started with Azure Blob Service in Java</span></span>](storage-java-how-to-use-blob-storage.md)
* [<span data-ttu-id="dcdbf-202">Komma igång med Azure-kötjänsten i Java</span><span class="sxs-lookup"><span data-stu-id="dcdbf-202">Getting Started with Azure Queue Service in Java</span></span>](storage-java-how-to-use-queue-storage.md)
* [<span data-ttu-id="dcdbf-203">Komma igång med Azure-tabellen Service i Java</span><span class="sxs-lookup"><span data-stu-id="dcdbf-203">Getting Started with Azure Table Service in Java</span></span>](storage-java-how-to-use-table-storage.md)
* [<span data-ttu-id="dcdbf-204">Komma igång med Azure File Service i Java</span><span class="sxs-lookup"><span data-stu-id="dcdbf-204">Getting Started with Azure File Service in Java</span></span>](storage-java-how-to-use-file-storage.md)

## <a name="next-steps"></a><span data-ttu-id="dcdbf-205">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dcdbf-205">Next steps</span></span>

<span data-ttu-id="dcdbf-206">Information om prover för andra språk:</span><span class="sxs-lookup"><span data-stu-id="dcdbf-206">For information on samples for other languages:</span></span>

* <span data-ttu-id="dcdbf-207">.NET: [azure Storage-exempel med hjälp av .NET](storage-samples-dotnet.md)</span><span class="sxs-lookup"><span data-stu-id="dcdbf-207">.NET: [Azure Storage samples using .NET](storage-samples-dotnet.md)</span></span>
* <span data-ttu-id="dcdbf-208">Alla andra språk: [Azure Storage-exempel](storage-samples.md)</span><span class="sxs-lookup"><span data-stu-id="dcdbf-208">All other languages: [Azure Storage samples](storage-samples.md)</span></span>

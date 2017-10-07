---
title: aaaIntroduction tooAzure Blob storage | Microsoft Docs
description: Introduktion tooAzure Blob storage
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: robinsh
ms.openlocfilehash: 3431f826ae51d42dbced084ee60f9ff70a8168d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooblob-storage"></a><span data-ttu-id="9675a-103">Introduktion tooBlob lagring</span><span class="sxs-lookup"><span data-stu-id="9675a-103">Introduction tooBlob storage</span></span>

<span data-ttu-id="9675a-104">Azure Blob storage är en tjänst för att lagra stora mängder Ostrukturerade objektdata, exempelvis text eller binära data som kan nås från var som helst i hello world via HTTP eller HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9675a-104">Azure Blob storage is a service for storing large amounts of unstructured object data, such as text or binary data, that can be accessed from anywhere in hello world via HTTP or HTTPS.</span></span> <span data-ttu-id="9675a-105">Du kan använda Blob storage tooexpose data offentligt toohello world eller toostore programdata privat.</span><span class="sxs-lookup"><span data-stu-id="9675a-105">You can use Blob storage tooexpose data publicly toohello world, or toostore application data privately.</span></span>

<span data-ttu-id="9675a-106">Vanliga användningsområden för Blob Storage är:</span><span class="sxs-lookup"><span data-stu-id="9675a-106">Common uses of Blob storage include:</span></span>

* <span data-ttu-id="9675a-107">Betjänar bilder eller dokument direkt tooa webbläsare</span><span class="sxs-lookup"><span data-stu-id="9675a-107">Serving images or documents directly tooa browser</span></span>
* <span data-ttu-id="9675a-108">Lagra filer för distribuerad åtkomst</span><span class="sxs-lookup"><span data-stu-id="9675a-108">Storing files for distributed access</span></span>
* <span data-ttu-id="9675a-109">Direktuppspelning av video och ljud</span><span class="sxs-lookup"><span data-stu-id="9675a-109">Streaming video and audio</span></span>
* <span data-ttu-id="9675a-110">Lagra data för säkerhetskopiering och återställning, haveriberedskap och arkivering</span><span class="sxs-lookup"><span data-stu-id="9675a-110">Storing data for backup and restore, disaster recovery, and archiving</span></span>
* <span data-ttu-id="9675a-111">Lagra data för analys av en tjänst som kan vara lokal eller Azure-baserad</span><span class="sxs-lookup"><span data-stu-id="9675a-111">Storing data for analysis by an on-premises or Azure-hosted service</span></span>

## <a name="blob-service-concepts"></a><span data-ttu-id="9675a-112">Blob Service-koncept</span><span class="sxs-lookup"><span data-stu-id="9675a-112">Blob service concepts</span></span>

<span data-ttu-id="9675a-113">hello Blob-tjänsten innehåller hello följande komponenter:</span><span class="sxs-lookup"><span data-stu-id="9675a-113">hello Blob service contains hello following components:</span></span>

![Blobb-arkitektur](./media/storage-blobs-introduction/blob1.png)

* <span data-ttu-id="9675a-115">**Storage-konto:** komma åt tooAzure Storage görs genom ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="9675a-115">**Storage Account:** All access tooAzure Storage is done through a storage account.</span></span> <span data-ttu-id="9675a-116">Det här lagringskontot kan vara en **Allmänt lagringskonto** eller en **Blob storage-konto** som är specialiserat för lagring av objekt/blobbar.</span><span class="sxs-lookup"><span data-stu-id="9675a-116">This storage account can be a **General-purpose storage account** or a **Blob storage account** that is specialized for storing objects/blobs.</span></span> <span data-ttu-id="9675a-117">Mer information finns i [Om Azure Storage-konton](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9675a-117">See [About Azure storage accounts](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) for more information.</span></span>

* <span data-ttu-id="9675a-118">**Behållare:** En behållare grupperar en uppsättning blobbar.</span><span class="sxs-lookup"><span data-stu-id="9675a-118">**Container:** A container provides a grouping of a set of blobs.</span></span> <span data-ttu-id="9675a-119">Alla blobbar måste vara i en behållare.</span><span class="sxs-lookup"><span data-stu-id="9675a-119">All blobs must be in a container.</span></span> <span data-ttu-id="9675a-120">Ett konto kan innehålla ett obegränsat antal behållare.</span><span class="sxs-lookup"><span data-stu-id="9675a-120">An account can contain an unlimited number of containers.</span></span> <span data-ttu-id="9675a-121">En behållare kan lagra ett obegränsat antal blobbar.</span><span class="sxs-lookup"><span data-stu-id="9675a-121">A container can store an unlimited number of blobs.</span></span> <span data-ttu-id="9675a-122">Observera att hello behållarens namn måste vara gemener.</span><span class="sxs-lookup"><span data-stu-id="9675a-122">Note that hello container name must be lowercase.</span></span>

* <span data-ttu-id="9675a-123">**Blob:** en fil av valfri typ och storlek.</span><span class="sxs-lookup"><span data-stu-id="9675a-123">**Blob:** A file of any type and size.</span></span> <span data-ttu-id="9675a-124">Azure Storage erbjuder tre typer av blobbar: blockblobbar, sidblobbar och tilläggsblobbar.</span><span class="sxs-lookup"><span data-stu-id="9675a-124">Azure Storage offers three types of blobs: block blobs, page blobs, and append blobs.</span></span>
  
    <span data-ttu-id="9675a-125">*Blockblobbar* lämpar sig för lagring av text eller binära filer, exempelvis dokument och mediafiler.</span><span class="sxs-lookup"><span data-stu-id="9675a-125">*Block blobs* are ideal for storing text or binary files, such as documents and media files.</span></span> <span data-ttu-id="9675a-126">*Tilläggsblobbar* är liknande tooblock blobbar i att de består av block, men de är optimerade för tilläggsåtgärder, så att de är väl för loggningsscenarier.</span><span class="sxs-lookup"><span data-stu-id="9675a-126">*Append blobs* are similar tooblock blobs in that they are made up of blocks, but they are optimized for append operations, so they are useful for logging scenarios.</span></span> <span data-ttu-id="9675a-127">En enda blockblobb kan innehålla upp too50 000 block med upp too100 MB vardera, ger en total storlek på lite över 4,75 TB (100 MB X 50 000).</span><span class="sxs-lookup"><span data-stu-id="9675a-127">A single block blob can contain up too50,000 blocks of up too100 MB each, for a total size of slightly more than 4.75 TB (100 MB X 50,000).</span></span> <span data-ttu-id="9675a-128">En enda tilläggsblobb kan innehålla upp too50 000 block med upp too4 MB vardera, för en total storlek på lite över 195 GB (4 MB X 50 000).</span><span class="sxs-lookup"><span data-stu-id="9675a-128">A single append blob can contain up too50,000 blocks of up too4 MB each, for a total size of slightly more than 195 GB (4 MB X 50,000).</span></span>
  
    <span data-ttu-id="9675a-129">*Sidblobbar* kan vara upp too1 TB i storlek och är bäst för frekventa läs-/ skrivåtgärder.</span><span class="sxs-lookup"><span data-stu-id="9675a-129">*Page blobs* can be up too1 TB in size, and are more efficient for frequent read/write operations.</span></span> <span data-ttu-id="9675a-130">Virtuella Azure-datorer använder sidblobbar som Operativsystemet och datadiskarna.</span><span class="sxs-lookup"><span data-stu-id="9675a-130">Azure Virtual Machines uses page blobs as OS and data disks.</span></span>
  
    <span data-ttu-id="9675a-131">Mer information om namngivning av behållare och blobbar finns i [Namngivning och referens av behållare, blobbar och metadata](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata).</span><span class="sxs-lookup"><span data-stu-id="9675a-131">For details about naming containers and blobs, see [Naming and Referencing Containers, Blobs, and Metadata](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata).</span></span>

## <a name="next-steps"></a><span data-ttu-id="9675a-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9675a-132">Next steps</span></span>

* [<span data-ttu-id="9675a-133">Skapa ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="9675a-133">Create a storage account</span></span>](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [<span data-ttu-id="9675a-134">Komma igång med Blob storage med hjälp av .NET</span><span class="sxs-lookup"><span data-stu-id="9675a-134">Getting started with Blob storage using .NET</span></span>](storage-dotnet-how-to-use-blobs.md)
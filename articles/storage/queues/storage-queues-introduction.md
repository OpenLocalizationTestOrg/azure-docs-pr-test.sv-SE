---
title: aaaIntroduction tooAzure Queue storage | Microsoft Docs
description: Introduktion tooAzure Queue storage
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
ms.date: 08/07/2017
ms.author: robinsh
ms.openlocfilehash: 669effedff7beedde8a119c350a2a70edafedcf0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooqueues"></a><span data-ttu-id="973f6-103">Introduktion tooQueues</span><span class="sxs-lookup"><span data-stu-id="973f6-103">Introduction tooQueues</span></span>

<span data-ttu-id="973f6-104">Azure Queue storage är en tjänst för att lagra stora mängder meddelanden som kan nås från var som helst i Hej världen via autentiserade anrop med HTTP eller HTTPS.</span><span class="sxs-lookup"><span data-stu-id="973f6-104">Azure Queue storage is a service for storing large numbers of messages that can be accessed from anywhere in hello world via authenticated calls using HTTP or HTTPS.</span></span> <span data-ttu-id="973f6-105">Ett enda kömeddelande kan vara upp too64 KB i storlek och en kö kan innehålla miljontals meddelanden, upp toohello totala kapacitetsgränsen för ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="973f6-105">A single queue message can be up too64 KB in size, and a queue can contain millions of messages, up toohello total capacity limit of a storage account.</span></span>

## <a name="common-uses"></a><span data-ttu-id="973f6-106">Vanliga användningsområden</span><span class="sxs-lookup"><span data-stu-id="973f6-106">Common uses</span></span>

<span data-ttu-id="973f6-107">Vanliga användningsområden för Queue Storage är:</span><span class="sxs-lookup"><span data-stu-id="973f6-107">Common uses of Queue storage include:</span></span>

* <span data-ttu-id="973f6-108">Skapa en eftersläpning i arbetet tooprocess asynkront</span><span class="sxs-lookup"><span data-stu-id="973f6-108">Creating a backlog of work tooprocess asynchronously</span></span>
* <span data-ttu-id="973f6-109">Skicka meddelanden från en Azure web rollen tooan Azure-arbetsroll</span><span class="sxs-lookup"><span data-stu-id="973f6-109">Passing messages from an Azure web role tooan Azure worker role</span></span>

## <a name="queue-service-concepts"></a><span data-ttu-id="973f6-110">Kötjänst-koncept</span><span class="sxs-lookup"><span data-stu-id="973f6-110">Queue service concepts</span></span>

<span data-ttu-id="973f6-111">hello kötjänsten innehåller hello följande komponenter:</span><span class="sxs-lookup"><span data-stu-id="973f6-111">hello Queue service contains hello following components:</span></span>

![Kön begrepp](./media/storage-queues-introduction/queue1.png)

* <span data-ttu-id="973f6-113">**URL-format:** köer är adresserbara via följande URL-format hello:</span><span class="sxs-lookup"><span data-stu-id="973f6-113">**URL format:** Queues are addressable using hello following URL format:</span></span>   
    <span data-ttu-id="973f6-114">http://`<storage account>`.queue.core.windows.net/`<queue>`</span><span class="sxs-lookup"><span data-stu-id="973f6-114">http://`<storage account>`.queue.core.windows.net/`<queue>`</span></span> 
  
    <span data-ttu-id="973f6-115">hello följande URL adresserar en kö i hello diagram:</span><span class="sxs-lookup"><span data-stu-id="973f6-115">hello following URL addresses a queue in hello diagram:</span></span>  
  
    `http://myaccount.queue.core.windows.net/images-to-download`

* <span data-ttu-id="973f6-116">**Storage-konto:** komma åt tooAzure Storage görs genom ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="973f6-116">**Storage account:** All access tooAzure Storage is done through a storage account.</span></span> <span data-ttu-id="973f6-117">Se [Skalbarhets- och prestandamål för Azure Storage](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) för information om kapacitet för lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="973f6-117">See [Azure Storage Scalability and Performance Targets](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) for details about storage account capacity.</span></span>

* <span data-ttu-id="973f6-118">**Kö:** en kö innehåller en uppsättning meddelanden.</span><span class="sxs-lookup"><span data-stu-id="973f6-118">**Queue:** A queue contains a set of messages.</span></span> <span data-ttu-id="973f6-119">Alla meddelanden måste vara i en kö.</span><span class="sxs-lookup"><span data-stu-id="973f6-119">All messages must be in a queue.</span></span> <span data-ttu-id="973f6-120">Observera att hello könamnet måste vara gemener.</span><span class="sxs-lookup"><span data-stu-id="973f6-120">Note that hello queue name must be all lowercase.</span></span> <span data-ttu-id="973f6-121">Mer information om namngivning av köer finns i [namngivning av köer och metadata](https://msdn.microsoft.com/library/azure/dd179349.aspx).</span><span class="sxs-lookup"><span data-stu-id="973f6-121">For information on naming queues, see [Naming Queues and Metadata](https://msdn.microsoft.com/library/azure/dd179349.aspx).</span></span>

* <span data-ttu-id="973f6-122">**Meddelande:** A meddelandet, i valfritt format för in too64 KB.</span><span class="sxs-lookup"><span data-stu-id="973f6-122">**Message:** A message, in any format, of up too64 KB.</span></span> <span data-ttu-id="973f6-123">hello maximala tid som ett meddelande kan finnas i kön hello är sju dagar.</span><span class="sxs-lookup"><span data-stu-id="973f6-123">hello maximum time that a message can remain in hello queue is seven days.</span></span>

## <a name="next-steps"></a><span data-ttu-id="973f6-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="973f6-124">Next steps</span></span>

* [<span data-ttu-id="973f6-125">Skapa ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="973f6-125">Create a storage account</span></span>](../storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [<span data-ttu-id="973f6-126">Komma igång med köer med hjälp av .NET</span><span class="sxs-lookup"><span data-stu-id="973f6-126">Getting started with Queues using .NET</span></span>](storage-dotnet-how-to-use-queues.md)

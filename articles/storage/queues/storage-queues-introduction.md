---
title: Introduktion till Azure Queue storage | Microsoft Docs
description: Introduktion till Azure Queue storage
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
ms.openlocfilehash: 4db7552a1b76c89151405c55c8682abbb5326bb6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="introduction-to-queues"></a><span data-ttu-id="eb427-103">Introduktion till köer</span><span class="sxs-lookup"><span data-stu-id="eb427-103">Introduction to Queues</span></span>

<span data-ttu-id="eb427-104">Azure Queue Storage är en tjänst för att lagra stora mängder meddelanden som kan nås från var som helst i världen via autentiserade anrop med HTTP eller HTTPS.</span><span class="sxs-lookup"><span data-stu-id="eb427-104">Azure Queue storage is a service for storing large numbers of messages that can be accessed from anywhere in the world via authenticated calls using HTTP or HTTPS.</span></span> <span data-ttu-id="eb427-105">Ett enda kömeddelande kan vara upp till 64 KB stort och en kö kan innehålla miljontals meddelanden, upp till den totala kapacitetsgränsen för ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="eb427-105">A single queue message can be up to 64 KB in size, and a queue can contain millions of messages, up to the total capacity limit of a storage account.</span></span>

## <a name="common-uses"></a><span data-ttu-id="eb427-106">Vanliga användningsområden</span><span class="sxs-lookup"><span data-stu-id="eb427-106">Common uses</span></span>

<span data-ttu-id="eb427-107">Vanliga användningsområden för Queue Storage är:</span><span class="sxs-lookup"><span data-stu-id="eb427-107">Common uses of Queue storage include:</span></span>

* <span data-ttu-id="eb427-108">Skapa en lista med kvarvarande uppgifter att bearbeta asynkront</span><span class="sxs-lookup"><span data-stu-id="eb427-108">Creating a backlog of work to process asynchronously</span></span>
* <span data-ttu-id="eb427-109">Skicka meddelanden från en Azure-webbroll till en Azure-arbetsroll</span><span class="sxs-lookup"><span data-stu-id="eb427-109">Passing messages from an Azure web role to an Azure worker role</span></span>

## <a name="queue-service-concepts"></a><span data-ttu-id="eb427-110">Kötjänst-koncept</span><span class="sxs-lookup"><span data-stu-id="eb427-110">Queue service concepts</span></span>

<span data-ttu-id="eb427-111">Kötjänsten består av följande komponenter:</span><span class="sxs-lookup"><span data-stu-id="eb427-111">The Queue service contains the following components:</span></span>

![Kön begrepp](./media/storage-queues-introduction/queue1.png)

* <span data-ttu-id="eb427-113">**URL-format:** köer är adresserbara via följande URL-format:</span><span class="sxs-lookup"><span data-stu-id="eb427-113">**URL format:** Queues are addressable using the following URL format:</span></span>   
    <span data-ttu-id="eb427-114">http://`<storage account>`.queue.core.windows.net/`<queue>`</span><span class="sxs-lookup"><span data-stu-id="eb427-114">http://`<storage account>`.queue.core.windows.net/`<queue>`</span></span> 
  
    <span data-ttu-id="eb427-115">Följande URL adresserar en kö i diagrammet:</span><span class="sxs-lookup"><span data-stu-id="eb427-115">The following URL addresses a queue in the diagram:</span></span>  
  
    `http://myaccount.queue.core.windows.net/images-to-download`

* <span data-ttu-id="eb427-116">**Storage-konto:** All åtkomst till Azure Storage görs genom ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="eb427-116">**Storage account:** All access to Azure Storage is done through a storage account.</span></span> <span data-ttu-id="eb427-117">Se [Skalbarhets- och prestandamål för Azure Storage](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) för information om kapacitet för lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="eb427-117">See [Azure Storage Scalability and Performance Targets](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) for details about storage account capacity.</span></span>

* <span data-ttu-id="eb427-118">**Kö:** en kö innehåller en uppsättning meddelanden.</span><span class="sxs-lookup"><span data-stu-id="eb427-118">**Queue:** A queue contains a set of messages.</span></span> <span data-ttu-id="eb427-119">Alla meddelanden måste vara i en kö.</span><span class="sxs-lookup"><span data-stu-id="eb427-119">All messages must be in a queue.</span></span> <span data-ttu-id="eb427-120">Observera att könamnet måste vara helt i gemener.</span><span class="sxs-lookup"><span data-stu-id="eb427-120">Note that the queue name must be all lowercase.</span></span> <span data-ttu-id="eb427-121">Mer information om namngivning av köer finns i [namngivning av köer och metadata](https://msdn.microsoft.com/library/azure/dd179349.aspx).</span><span class="sxs-lookup"><span data-stu-id="eb427-121">For information on naming queues, see [Naming Queues and Metadata](https://msdn.microsoft.com/library/azure/dd179349.aspx).</span></span>

* <span data-ttu-id="eb427-122">**Meddelande:** ett meddelande i valfritt format, som är upp till 64 KB.</span><span class="sxs-lookup"><span data-stu-id="eb427-122">**Message:** A message, in any format, of up to 64 KB.</span></span> <span data-ttu-id="eb427-123">Den maximala tid som ett meddelande kan finnas i kön är sju dagar.</span><span class="sxs-lookup"><span data-stu-id="eb427-123">The maximum time that a message can remain in the queue is seven days.</span></span>

## <a name="next-steps"></a><span data-ttu-id="eb427-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="eb427-124">Next steps</span></span>

* [<span data-ttu-id="eb427-125">Skapa ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="eb427-125">Create a storage account</span></span>](../storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [<span data-ttu-id="eb427-126">Komma igång med köer med hjälp av .NET</span><span class="sxs-lookup"><span data-stu-id="eb427-126">Getting started with Queues using .NET</span></span>](storage-dotnet-how-to-use-queues.md)

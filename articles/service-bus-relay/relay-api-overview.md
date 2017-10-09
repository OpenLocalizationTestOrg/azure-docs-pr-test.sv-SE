---
title: "aaaAzure Relay API översikt | Microsoft Docs"
description: "Översikt över tillgängliga Azure Relay API: er"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: fdaa1d2b-bd80-4e75-abb9-0c3d0773af2d
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: sethm
ms.openlocfilehash: 3c4d737d5fee9a8babce094fa6dfddb28910834b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="available-relay-apis"></a><span data-ttu-id="7a609-103">Tillgängliga Relay-API: er</span><span class="sxs-lookup"><span data-stu-id="7a609-103">Available Relay APIs</span></span>

## <a name="runtime-apis"></a><span data-ttu-id="7a609-104">Runtime-API: er</span><span class="sxs-lookup"><span data-stu-id="7a609-104">Runtime APIs</span></span>

<span data-ttu-id="7a609-105">hello visas följande tabell alla tillgängliga Relay runtime-klienter.</span><span class="sxs-lookup"><span data-stu-id="7a609-105">hello following table lists all currently available Relay runtime clients.</span></span>

<span data-ttu-id="7a609-106">Hej [ytterligare information](#additional-information) avsnitt innehåller mer information om hello status för varje runtime-bibliotek.</span><span class="sxs-lookup"><span data-stu-id="7a609-106">hello [additional information](#additional-information) section contains more information about hello status of each runtime library.</span></span>

| <span data-ttu-id="7a609-107">Språk-plattformen</span><span class="sxs-lookup"><span data-stu-id="7a609-107">Language/Platform</span></span> | <span data-ttu-id="7a609-108">Tillgänglig funktion</span><span class="sxs-lookup"><span data-stu-id="7a609-108">Available feature</span></span> | <span data-ttu-id="7a609-109">Klientpaketet</span><span class="sxs-lookup"><span data-stu-id="7a609-109">Client package</span></span> | <span data-ttu-id="7a609-110">Databasen</span><span class="sxs-lookup"><span data-stu-id="7a609-110">Repository</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="7a609-111">Standard för .NET</span><span class="sxs-lookup"><span data-stu-id="7a609-111">.NET Standard</span></span> | <span data-ttu-id="7a609-112">Hybridanslutningar</span><span class="sxs-lookup"><span data-stu-id="7a609-112">Hybrid Connections</span></span> | [<span data-ttu-id="7a609-113">Microsoft.Azure.Relay</span><span class="sxs-lookup"><span data-stu-id="7a609-113">Microsoft.Azure.Relay</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Relay/) | [<span data-ttu-id="7a609-114">GitHub</span><span class="sxs-lookup"><span data-stu-id="7a609-114">GitHub</span></span>](https://github.com/azure/azure-relay-dotnet) |
| <span data-ttu-id="7a609-115">.NET framework</span><span class="sxs-lookup"><span data-stu-id="7a609-115">.NET Framework</span></span> | <span data-ttu-id="7a609-116">WCF-relä</span><span class="sxs-lookup"><span data-stu-id="7a609-116">WCF Relay</span></span> | [<span data-ttu-id="7a609-117">WindowsAzure.ServiceBus</span><span class="sxs-lookup"><span data-stu-id="7a609-117">WindowsAzure.ServiceBus</span></span>](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | <span data-ttu-id="7a609-118">Saknas</span><span class="sxs-lookup"><span data-stu-id="7a609-118">N/A</span></span> |
| <span data-ttu-id="7a609-119">Node</span><span class="sxs-lookup"><span data-stu-id="7a609-119">Node</span></span> | <span data-ttu-id="7a609-120">Hybridanslutningar</span><span class="sxs-lookup"><span data-stu-id="7a609-120">Hybrid Connections</span></span> | [`hyco-ws`](https://www.npmjs.com/package/hyco-ws)<br/>[`hyco-websocket`](https://www.npmjs.com/package/hyco-websocket) | [<span data-ttu-id="7a609-121">GitHub</span><span class="sxs-lookup"><span data-stu-id="7a609-121">GitHub</span></span>](https://github.com/Azure/azure-relay-node) |

### <a name="additional-information"></a><span data-ttu-id="7a609-122">Ytterligare information</span><span class="sxs-lookup"><span data-stu-id="7a609-122">Additional information</span></span>

#### <a name="net"></a><span data-ttu-id="7a609-123">.NET</span><span class="sxs-lookup"><span data-stu-id="7a609-123">.NET</span></span>
<span data-ttu-id="7a609-124">hello .NET ekosystemet har flera körningar, så det finns flera .NET-bibliotek för Händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="7a609-124">hello .NET ecosystem has multiple runtimes, hence there are multiple .NET libraries for Event Hubs.</span></span> <span data-ttu-id="7a609-125">hello .NET standardbibliotek kan köras med .NET Core eller hello .NET Framework, medan hello-biblioteket för .NET Framework kan endast köras i en miljö med .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="7a609-125">hello .NET Standard library can be run using either .NET Core or hello .NET Framework, while hello .NET Framework library can only be run in a .NET Framework environment.</span></span> <span data-ttu-id="7a609-126">Läs mer på .NET Frameworks [framework-versioner](/dotnet/articles/standard/frameworks#framework-versions).</span><span class="sxs-lookup"><span data-stu-id="7a609-126">For more information on .NET Frameworks, see [framework versions](/dotnet/articles/standard/frameworks#framework-versions).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7a609-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7a609-127">Next steps</span></span>
<span data-ttu-id="7a609-128">toolearn mer om Azure Relay finns i följande länkar:</span><span class="sxs-lookup"><span data-stu-id="7a609-128">toolearn more about Azure Relay, visit these links:</span></span>
* [<span data-ttu-id="7a609-129">Vad är Azure Relay?</span><span class="sxs-lookup"><span data-stu-id="7a609-129">What is Azure Relay?</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="7a609-130">Vanliga frågor och svar om Relay</span><span class="sxs-lookup"><span data-stu-id="7a609-130">Relay FAQ</span></span>](relay-faq.md)
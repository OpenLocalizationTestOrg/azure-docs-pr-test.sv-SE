---
title: "Översikt över Azure Relay API | Microsoft Docs"
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
ms.openlocfilehash: 8d93a0344adc3b0b7617f3a7d85124142d7a7555
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="available-relay-apis"></a><span data-ttu-id="81971-103">Tillgängliga Relay-API: er</span><span class="sxs-lookup"><span data-stu-id="81971-103">Available Relay APIs</span></span>

## <a name="runtime-apis"></a><span data-ttu-id="81971-104">Runtime-API: er</span><span class="sxs-lookup"><span data-stu-id="81971-104">Runtime APIs</span></span>

<span data-ttu-id="81971-105">I följande tabell visas alla tillgängliga Relay runtime-klienter.</span><span class="sxs-lookup"><span data-stu-id="81971-105">The following table lists all currently available Relay runtime clients.</span></span>

<span data-ttu-id="81971-106">Den [ytterligare information](#additional-information) avsnitt innehåller mer information om statusen för varje runtime-bibliotek.</span><span class="sxs-lookup"><span data-stu-id="81971-106">The [additional information](#additional-information) section contains more information about the status of each runtime library.</span></span>

| <span data-ttu-id="81971-107">Språk-plattformen</span><span class="sxs-lookup"><span data-stu-id="81971-107">Language/Platform</span></span> | <span data-ttu-id="81971-108">Tillgänglig funktion</span><span class="sxs-lookup"><span data-stu-id="81971-108">Available feature</span></span> | <span data-ttu-id="81971-109">Klientpaketet</span><span class="sxs-lookup"><span data-stu-id="81971-109">Client package</span></span> | <span data-ttu-id="81971-110">Databasen</span><span class="sxs-lookup"><span data-stu-id="81971-110">Repository</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="81971-111">Standard för .NET</span><span class="sxs-lookup"><span data-stu-id="81971-111">.NET Standard</span></span> | <span data-ttu-id="81971-112">Hybridanslutningar</span><span class="sxs-lookup"><span data-stu-id="81971-112">Hybrid Connections</span></span> | [<span data-ttu-id="81971-113">Microsoft.Azure.Relay</span><span class="sxs-lookup"><span data-stu-id="81971-113">Microsoft.Azure.Relay</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Relay/) | [<span data-ttu-id="81971-114">GitHub</span><span class="sxs-lookup"><span data-stu-id="81971-114">GitHub</span></span>](https://github.com/azure/azure-relay-dotnet) |
| <span data-ttu-id="81971-115">.NET framework</span><span class="sxs-lookup"><span data-stu-id="81971-115">.NET Framework</span></span> | <span data-ttu-id="81971-116">WCF-relä</span><span class="sxs-lookup"><span data-stu-id="81971-116">WCF Relay</span></span> | [<span data-ttu-id="81971-117">WindowsAzure.ServiceBus</span><span class="sxs-lookup"><span data-stu-id="81971-117">WindowsAzure.ServiceBus</span></span>](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | <span data-ttu-id="81971-118">Saknas</span><span class="sxs-lookup"><span data-stu-id="81971-118">N/A</span></span> |
| <span data-ttu-id="81971-119">Node</span><span class="sxs-lookup"><span data-stu-id="81971-119">Node</span></span> | <span data-ttu-id="81971-120">Hybridanslutningar</span><span class="sxs-lookup"><span data-stu-id="81971-120">Hybrid Connections</span></span> | [`hyco-ws`](https://www.npmjs.com/package/hyco-ws)<br/>[`hyco-websocket`](https://www.npmjs.com/package/hyco-websocket) | [<span data-ttu-id="81971-121">GitHub</span><span class="sxs-lookup"><span data-stu-id="81971-121">GitHub</span></span>](https://github.com/Azure/azure-relay-node) |

### <a name="additional-information"></a><span data-ttu-id="81971-122">Ytterligare information</span><span class="sxs-lookup"><span data-stu-id="81971-122">Additional information</span></span>

#### <a name="net"></a><span data-ttu-id="81971-123">.NET</span><span class="sxs-lookup"><span data-stu-id="81971-123">.NET</span></span>
<span data-ttu-id="81971-124">.NET-ekosystemet har flera körningar, så det finns flera .NET-bibliotek för Händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="81971-124">The .NET ecosystem has multiple runtimes, hence there are multiple .NET libraries for Event Hubs.</span></span> <span data-ttu-id="81971-125">.NET standardbibliotek kan köras med .NET Core eller .NET Framework, medan biblioteket för .NET Framework kan endast köras i en miljö med .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="81971-125">The .NET Standard library can be run using either .NET Core or the .NET Framework, while the .NET Framework library can only be run in a .NET Framework environment.</span></span> <span data-ttu-id="81971-126">Läs mer på .NET Frameworks [framework-versioner](/dotnet/articles/standard/frameworks#framework-versions).</span><span class="sxs-lookup"><span data-stu-id="81971-126">For more information on .NET Frameworks, see [framework versions](/dotnet/articles/standard/frameworks#framework-versions).</span></span>

## <a name="next-steps"></a><span data-ttu-id="81971-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="81971-127">Next steps</span></span>
<span data-ttu-id="81971-128">Mer information om Azure Relay finns i följande länkar:</span><span class="sxs-lookup"><span data-stu-id="81971-128">To learn more about Azure Relay, visit these links:</span></span>
* [<span data-ttu-id="81971-129">Vad är Azure Relay?</span><span class="sxs-lookup"><span data-stu-id="81971-129">What is Azure Relay?</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="81971-130">Vanliga frågor och svar om Relay</span><span class="sxs-lookup"><span data-stu-id="81971-130">Relay FAQ</span></span>](relay-faq.md)
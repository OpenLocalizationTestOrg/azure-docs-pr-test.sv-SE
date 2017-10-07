---
title: "aaaAzure händelse API: er översikt | Microsoft Docs"
description: "Översikt över tillgängliga Azure Event Hubs API: er"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 3f221a0c-182d-4e39-9f3d-3a3c16c5c6ed
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: 46dfcc544ff92642cfd7a967f9ec38a0d8e2bd5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="available-event-hubs-apis"></a><span data-ttu-id="1bae7-103">Tillgängliga Event Hubs API: er</span><span class="sxs-lookup"><span data-stu-id="1bae7-103">Available Event Hubs APIs</span></span>

## <a name="runtime-apis"></a><span data-ttu-id="1bae7-104">Runtime-API: er</span><span class="sxs-lookup"><span data-stu-id="1bae7-104">Runtime APIs</span></span>

<span data-ttu-id="1bae7-105">hello följer en beskrivning av alla tillgängliga Azure Event Hubs runtime-klienter.</span><span class="sxs-lookup"><span data-stu-id="1bae7-105">hello following is a description of all currently available Azure Event Hubs runtime clients.</span></span> <span data-ttu-id="1bae7-106">Några av dessa bibliotek är också begränsad hanteringsfunktioner, men det finns också [vissa bibliotek](#management-apis) dedikerade toomanagement åtgärder.</span><span class="sxs-lookup"><span data-stu-id="1bae7-106">While some of these libraries also include limited management functionality, there are also [specific libraries](#management-apis) dedicated toomanagement operations.</span></span> <span data-ttu-id="1bae7-107">hello core fokus för dessa bibliotek är toosend och ta emot meddelanden från en händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="1bae7-107">hello core focus of these libraries is toosend and receive messages from an event hub.</span></span>

<span data-ttu-id="1bae7-108">Se [ytterligare information](#additional-information) för mer information om hello aktuell status för varje runtime-bibliotek.</span><span class="sxs-lookup"><span data-stu-id="1bae7-108">See [additional information](#additional-information) for more details on hello current status of each runtime library.</span></span>

| <span data-ttu-id="1bae7-109">Språk-plattformen</span><span class="sxs-lookup"><span data-stu-id="1bae7-109">Language/Platform</span></span> | <span data-ttu-id="1bae7-110">Klientpaketet</span><span class="sxs-lookup"><span data-stu-id="1bae7-110">Client package</span></span> | <span data-ttu-id="1bae7-111">Paketet EventProcessorHost</span><span class="sxs-lookup"><span data-stu-id="1bae7-111">EventProcessorHost package</span></span> | <span data-ttu-id="1bae7-112">Databasen</span><span class="sxs-lookup"><span data-stu-id="1bae7-112">Repository</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="1bae7-113">Standard för .NET</span><span class="sxs-lookup"><span data-stu-id="1bae7-113">.NET Standard</span></span> | [<span data-ttu-id="1bae7-114">NuGet</span><span class="sxs-lookup"><span data-stu-id="1bae7-114">NuGet</span></span>](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) | [<span data-ttu-id="1bae7-115">NuGet</span><span class="sxs-lookup"><span data-stu-id="1bae7-115">NuGet</span></span>](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) | [<span data-ttu-id="1bae7-116">GitHub</span><span class="sxs-lookup"><span data-stu-id="1bae7-116">GitHub</span></span>](https://github.com/azure/azure-event-hubs-dotnet) |
| <span data-ttu-id="1bae7-117">.NET framework</span><span class="sxs-lookup"><span data-stu-id="1bae7-117">.NET Framework</span></span> | [<span data-ttu-id="1bae7-118">NuGet</span><span class="sxs-lookup"><span data-stu-id="1bae7-118">NuGet</span></span>](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | [<span data-ttu-id="1bae7-119">NuGet</span><span class="sxs-lookup"><span data-stu-id="1bae7-119">NuGet</span></span>](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) | <span data-ttu-id="1bae7-120">Saknas</span><span class="sxs-lookup"><span data-stu-id="1bae7-120">N/A</span></span> |
| <span data-ttu-id="1bae7-121">Java</span><span class="sxs-lookup"><span data-stu-id="1bae7-121">Java</span></span> | [<span data-ttu-id="1bae7-122">Maven 3.</span><span class="sxs-lookup"><span data-stu-id="1bae7-122">Maven</span></span>](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22) | [<span data-ttu-id="1bae7-123">Maven 3.</span><span class="sxs-lookup"><span data-stu-id="1bae7-123">Maven</span></span>](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22) | [<span data-ttu-id="1bae7-124">GitHub</span><span class="sxs-lookup"><span data-stu-id="1bae7-124">GitHub</span></span>](https://github.com/Azure/azure-event-hubs-java) |
| <span data-ttu-id="1bae7-125">Node</span><span class="sxs-lookup"><span data-stu-id="1bae7-125">Node</span></span> | [<span data-ttu-id="1bae7-126">NPM</span><span class="sxs-lookup"><span data-stu-id="1bae7-126">NPM</span></span>](https://www.npmjs.com/package/azure-event-hubs) | <span data-ttu-id="1bae7-127">Saknas</span><span class="sxs-lookup"><span data-stu-id="1bae7-127">N/A</span></span> | [<span data-ttu-id="1bae7-128">GitHub</span><span class="sxs-lookup"><span data-stu-id="1bae7-128">GitHub</span></span>](https://github.com/Azure/azure-event-hubs-node) |
| <span data-ttu-id="1bae7-129">C</span><span class="sxs-lookup"><span data-stu-id="1bae7-129">C</span></span> | <span data-ttu-id="1bae7-130">Saknas</span><span class="sxs-lookup"><span data-stu-id="1bae7-130">N/A</span></span> | <span data-ttu-id="1bae7-131">Saknas</span><span class="sxs-lookup"><span data-stu-id="1bae7-131">N/A</span></span> | [<span data-ttu-id="1bae7-132">GitHub</span><span class="sxs-lookup"><span data-stu-id="1bae7-132">GitHub</span></span>](https://github.com/Azure/azure-event-hubs-c) |

### <a name="additional-information"></a><span data-ttu-id="1bae7-133">Ytterligare information</span><span class="sxs-lookup"><span data-stu-id="1bae7-133">Additional information</span></span>

#### <a name="net"></a><span data-ttu-id="1bae7-134">.NET</span><span class="sxs-lookup"><span data-stu-id="1bae7-134">.NET</span></span>
<span data-ttu-id="1bae7-135">hello .NET ekosystemet har flera körningar, så det finns flera .NET-bibliotek för Händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="1bae7-135">hello .NET ecosystem has multiple runtimes, hence there are multiple .NET libraries for Event Hubs.</span></span> <span data-ttu-id="1bae7-136">hello .NET standardbibliotek kan köras med .NET Core eller hello .NET Framework, medan hello-biblioteket för .NET Framework kan endast köras i en miljö med .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="1bae7-136">hello .NET Standard library can be run using either .NET Core or hello .NET Framework, while hello .NET Framework library can only be run in a .NET Framework environment.</span></span> <span data-ttu-id="1bae7-137">Läs mer på .NET Frameworks [framework-versioner](https://docs.microsoft.com/dotnet/articles/standard/frameworks#framework-versions).</span><span class="sxs-lookup"><span data-stu-id="1bae7-137">For more information on .NET Frameworks, see [framework versions](https://docs.microsoft.com/dotnet/articles/standard/frameworks#framework-versions).</span></span>

#### <a name="node"></a><span data-ttu-id="1bae7-138">Node</span><span class="sxs-lookup"><span data-stu-id="1bae7-138">Node</span></span>

<span data-ttu-id="1bae7-139">Hej Node.js-bibliotek är för närvarande under förhandsgranskning och hanteras som ett sida-projekt av Microsofts anställda och externa deltagare.</span><span class="sxs-lookup"><span data-stu-id="1bae7-139">hello Node.js library is currently in preview and is maintained as a side project by Microsoft employees and external contributors.</span></span> <span data-ttu-id="1bae7-140">Alla bidrag inklusive källkoden Välkommen och kommer att granskas.</span><span class="sxs-lookup"><span data-stu-id="1bae7-140">All contributions including source code are welcome and will be reviewed.</span></span>

## <a name="management-apis"></a><span data-ttu-id="1bae7-141">Management-API: er</span><span class="sxs-lookup"><span data-stu-id="1bae7-141">Management APIs</span></span>

<span data-ttu-id="1bae7-142">hello nedan följer en lista över alla tillgängliga management specifika bibliotek.</span><span class="sxs-lookup"><span data-stu-id="1bae7-142">hello following is a listing of all currently available management specific libraries.</span></span> <span data-ttu-id="1bae7-143">Ingen av dessa bibliotek innehåller runtime åtgärder och gäller hello uteslutande för hantering av Händelsehubbar entiteter.</span><span class="sxs-lookup"><span data-stu-id="1bae7-143">None of these libraries contain runtime operations, and are for hello sole purpose of managing Event Hubs entities.</span></span>

| <span data-ttu-id="1bae7-144">Språk-plattformen</span><span class="sxs-lookup"><span data-stu-id="1bae7-144">Language/Platform</span></span> | <span data-ttu-id="1bae7-145">Hanteringspaket</span><span class="sxs-lookup"><span data-stu-id="1bae7-145">Management package</span></span> | <span data-ttu-id="1bae7-146">Databasen</span><span class="sxs-lookup"><span data-stu-id="1bae7-146">Repository</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="1bae7-147">Standard för .NET</span><span class="sxs-lookup"><span data-stu-id="1bae7-147">.NET Standard</span></span> | [<span data-ttu-id="1bae7-148">NuGet</span><span class="sxs-lookup"><span data-stu-id="1bae7-148">NuGet</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Management.EventHub) | [<span data-ttu-id="1bae7-149">GitHub</span><span class="sxs-lookup"><span data-stu-id="1bae7-149">GitHub</span></span>](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/ResourceManagement/EventHub) |

## <a name="next-steps"></a><span data-ttu-id="1bae7-150">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1bae7-150">Next steps</span></span>
<span data-ttu-id="1bae7-151">Mer information om Händelsehubbar genom att besöka hello följande länkar:</span><span class="sxs-lookup"><span data-stu-id="1bae7-151">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="1bae7-152">Event Hubs-översikt</span><span class="sxs-lookup"><span data-stu-id="1bae7-152">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="1bae7-153">Skapa en Event Hub</span><span class="sxs-lookup"><span data-stu-id="1bae7-153">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="1bae7-154">Vanliga frågor och svar om Event Hubs</span><span class="sxs-lookup"><span data-stu-id="1bae7-154">Event Hubs FAQ</span></span>](event-hubs-faq.md)
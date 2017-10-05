---
title: WebJobs i Azure App Service
description: "Lär dig hur du skapar WebJobs för att köra tester bakgrund, interagera med tjänster som lagring och Service Bus och skapa schemalagda aktiviteter."
services: app-service
documentationcenter: 
author: christopheranderson
manager: erikre
editor: mollybos
ms.assetid: 85975432-04c9-4b83-b937-b30c082d52a1
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/10/2015
ms.author: chrande
ms.openlocfilehash: 1ca6d2eabe9781a8bb09fc5948ed306e3e8b013c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="using-webjobs-in-azure-app-service"></a><span data-ttu-id="edb66-103">Använda WebJobs i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="edb66-103">Using WebJobs in Azure App Service</span></span>
<span data-ttu-id="edb66-104">Den här innehåller artikellänkar till dokumentation om hur du använder Azure WebJobs och Azure WebJobs-SDK.</span><span class="sxs-lookup"><span data-stu-id="edb66-104">This article links to documentation resources about how to use Azure WebJobs and the Azure WebJobs SDK.</span></span> <span data-ttu-id="edb66-105">Azure WebJobs ger ett enkelt sätt att köra skript eller program som bakgrundsprocesser på [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="edb66-105">Azure WebJobs provide an easy way to run scripts or programs as background processes on [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> <span data-ttu-id="edb66-106">Du kan ladda upp och köra en körbar fil som som cmd bat exe (.NET) ps1, del, php, kopiera, js och jar.</span><span class="sxs-lookup"><span data-stu-id="edb66-106">You can upload and run an executable file such as as cmd, bat, exe (.NET), ps1, sh, php, py, js and jar.</span></span> <span data-ttu-id="edb66-107">Dessa program fungerar som WebJobs enligt ett schema (cron) eller kontinuerligt.</span><span class="sxs-lookup"><span data-stu-id="edb66-107">These programs run as WebJobs on a schedule (cron) or continuously.</span></span>

<span data-ttu-id="edb66-108">WebJobs SDK gör det enklare att använda Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="edb66-108">The WebJobs SDK makes it easier to use Azure Storage.</span></span> <span data-ttu-id="edb66-109">WebJobs SDK har en bindning och utlösare system som fungerar med Microsoft Azure Storage-Blobbar, köer och tabeller samt Service Bus-köer.</span><span class="sxs-lookup"><span data-stu-id="edb66-109">The WebJobs SDK has a binding and trigger system which works with Microsoft Azure Storage Blobs, Queues and Tables as well as Service Bus Queues.</span></span>

<span data-ttu-id="edb66-110">Skapa, distribuera och hantera WebJobs är integrerad med integrerad verktygsuppsättning i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="edb66-110">Creating, deploying, and managing WebJobs is seamless with integrated tooling in Visual Studio.</span></span> <span data-ttu-id="edb66-111">Du kan skapa WebJobs från mallar, publicera och hantera (kör/stop/Övervakare/debug) dem.</span><span class="sxs-lookup"><span data-stu-id="edb66-111">You can create WebJobs from templates, publish, and manage (run/stop/monitor/debug) them.</span></span>

<span data-ttu-id="edb66-112">WebJobs-instrumentpanelen i Azure portal tillhandahåller kraftfulla funktioner som ger dig fullständig kontroll över körningen av WebJobs, inklusive möjligheten att anropa enskilda funktioner i WebJobs.</span><span class="sxs-lookup"><span data-stu-id="edb66-112">The WebJobs dashboard in the Azure portal provides powerful management capabilities that give you full control over the execution of WebJobs, including the ability to invoke individual functions within WebJobs.</span></span> <span data-ttu-id="edb66-113">Instrumentpanelen visar även funktionen körningar och loggning utdata.</span><span class="sxs-lookup"><span data-stu-id="edb66-113">The dashboard also displays function runtimes and logging output.</span></span>

[!INCLUDE [app-service-blueprint-webjobs](../../includes/app-service-blueprint-webjobs.md)]


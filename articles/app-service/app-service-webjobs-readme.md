---
title: aaaWebJobs i Azure App Service
description: "Lär dig hur toobuild WebJobs toorun bakgrund testar, interagera med tjänster som lagring och Service Bus och skapa schemalagda aktiviteter."
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
ms.openlocfilehash: 25c24bfe71a64036cd48e58f471995b4a06e3b33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-webjobs-in-azure-app-service"></a><span data-ttu-id="b4aa4-103">Använda WebJobs i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b4aa4-103">Using WebJobs in Azure App Service</span></span>
<span data-ttu-id="b4aa4-104">Den här artikeln länkar toodocumentation resurser om hur toouse Azure WebJobs och hello Azure WebJobs SDK.</span><span class="sxs-lookup"><span data-stu-id="b4aa4-104">This article links toodocumentation resources about how toouse Azure WebJobs and hello Azure WebJobs SDK.</span></span> <span data-ttu-id="b4aa4-105">Azure WebJobs ger ett enkelt sätt toorun skript eller program som bakgrund processer på [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="b4aa4-105">Azure WebJobs provide an easy way toorun scripts or programs as background processes on [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> <span data-ttu-id="b4aa4-106">Du kan ladda upp och köra en körbar fil som som cmd bat exe (.NET) ps1, del, php, kopiera, js och jar.</span><span class="sxs-lookup"><span data-stu-id="b4aa4-106">You can upload and run an executable file such as as cmd, bat, exe (.NET), ps1, sh, php, py, js and jar.</span></span> <span data-ttu-id="b4aa4-107">Dessa program fungerar som WebJobs enligt ett schema (cron) eller kontinuerligt.</span><span class="sxs-lookup"><span data-stu-id="b4aa4-107">These programs run as WebJobs on a schedule (cron) or continuously.</span></span>

<span data-ttu-id="b4aa4-108">Hej WebJobs SDK gör det enklare toouse Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="b4aa4-108">hello WebJobs SDK makes it easier toouse Azure Storage.</span></span> <span data-ttu-id="b4aa4-109">Hej WebJobs SDK har en bindning och utlösare system som fungerar med Microsoft Azure Storage-Blobbar, köer och tabeller samt Service Bus-köer.</span><span class="sxs-lookup"><span data-stu-id="b4aa4-109">hello WebJobs SDK has a binding and trigger system which works with Microsoft Azure Storage Blobs, Queues and Tables as well as Service Bus Queues.</span></span>

<span data-ttu-id="b4aa4-110">Skapa, distribuera och hantera WebJobs är integrerad med integrerad verktygsuppsättning i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b4aa4-110">Creating, deploying, and managing WebJobs is seamless with integrated tooling in Visual Studio.</span></span> <span data-ttu-id="b4aa4-111">Du kan skapa WebJobs från mallar, publicera och hantera (kör/stop/Övervakare/debug) dem.</span><span class="sxs-lookup"><span data-stu-id="b4aa4-111">You can create WebJobs from templates, publish, and manage (run/stop/monitor/debug) them.</span></span>

<span data-ttu-id="b4aa4-112">Hej WebJobs-instrumentpanelen i hello Azure-portalen innehåller kraftfulla funktioner som ger dig fullständig kontroll över hello körningen av WebJobs, inklusive hello möjlighet tooinvoke enskilda funktioner i WebJobs.</span><span class="sxs-lookup"><span data-stu-id="b4aa4-112">hello WebJobs dashboard in hello Azure portal provides powerful management capabilities that give you full control over hello execution of WebJobs, including hello ability tooinvoke individual functions within WebJobs.</span></span> <span data-ttu-id="b4aa4-113">hello instrumentpanelen visar även funktionen körningar och loggning utdata.</span><span class="sxs-lookup"><span data-stu-id="b4aa4-113">hello dashboard also displays function runtimes and logging output.</span></span>

[!INCLUDE [app-service-blueprint-webjobs](../../includes/app-service-blueprint-webjobs.md)]


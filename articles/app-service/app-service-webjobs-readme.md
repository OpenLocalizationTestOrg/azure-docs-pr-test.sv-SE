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
# <a name="using-webjobs-in-azure-app-service"></a>Använda WebJobs i Azure App Service
Den här innehåller artikellänkar till dokumentation om hur du använder Azure WebJobs och Azure WebJobs-SDK. Azure WebJobs ger ett enkelt sätt att köra skript eller program som bakgrundsprocesser på [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714). Du kan ladda upp och köra en körbar fil som som cmd bat exe (.NET) ps1, del, php, kopiera, js och jar. Dessa program fungerar som WebJobs enligt ett schema (cron) eller kontinuerligt.

WebJobs SDK gör det enklare att använda Azure Storage. WebJobs SDK har en bindning och utlösare system som fungerar med Microsoft Azure Storage-Blobbar, köer och tabeller samt Service Bus-köer.

Skapa, distribuera och hantera WebJobs är integrerad med integrerad verktygsuppsättning i Visual Studio. Du kan skapa WebJobs från mallar, publicera och hantera (kör/stop/Övervakare/debug) dem.

WebJobs-instrumentpanelen i Azure portal tillhandahåller kraftfulla funktioner som ger dig fullständig kontroll över körningen av WebJobs, inklusive möjligheten att anropa enskilda funktioner i WebJobs. Instrumentpanelen visar även funktionen körningar och loggning utdata.

[!INCLUDE [app-service-blueprint-webjobs](../../includes/app-service-blueprint-webjobs.md)]


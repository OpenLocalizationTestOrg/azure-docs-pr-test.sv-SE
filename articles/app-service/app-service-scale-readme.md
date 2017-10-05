---
title: "Azure Apptjänst: Skala program för Apptjänst"
description: "Läs och nackdelar med skalning programmet i App Service."
keywords: app service, azure app service, scale, scalable, app service plan, app service cost
services: app-service
documentationcenter: 
author: btardif
manager: erikre
editor: 
ms.assetid: f403c971-4450-432b-8cea-3eeb426c0147
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/07/2016
ms.author: byvinyal
ms.openlocfilehash: 4ebaafe69fc1f91c7890611b14e8600df5c40cde
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-scaling-app-service-applications"></a><span data-ttu-id="60d09-104">Azure Apptjänst: Skala program för Apptjänst</span><span class="sxs-lookup"><span data-stu-id="60d09-104">Azure App Service: Scaling App Service Applications</span></span>
<span data-ttu-id="60d09-105">Program som finns i Azure App Service kan [uppnå massiv skala](https://azure.microsoft.com/blog/canadian-broadcasting-corporation-radio-canada-leverage-azure-for-smooth-election-coverage/).</span><span class="sxs-lookup"><span data-stu-id="60d09-105">Applications hosted in Azure App Service can [achieve massive scale](https://azure.microsoft.com/blog/canadian-broadcasting-corporation-radio-canada-leverage-azure-for-smooth-election-coverage/).</span></span>
<span data-ttu-id="60d09-106">Skala ett program är dock komplexa problem som inte har en ”passar alla”-lösning.</span><span class="sxs-lookup"><span data-stu-id="60d09-106">However, scaling an application is a complex problem that does not have a "one size fits all" solution.</span></span> <span data-ttu-id="60d09-107">Korrekt skala ditt program det är 3 viktiga områden som bidrar till din fungerande program:</span><span class="sxs-lookup"><span data-stu-id="60d09-107">To correctly scale your application there are 3 key areas that will contribute to your applications success:</span></span>

1. <span data-ttu-id="60d09-108">Förstå din programarkitektur och dess svagheter.</span><span class="sxs-lookup"><span data-stu-id="60d09-108">Understanding your application architecture and its weaknesses.</span></span>
   * <span data-ttu-id="60d09-109">Är programmet-Stateful?</span><span class="sxs-lookup"><span data-stu-id="60d09-109">Is your Application Stateful?</span></span> <span data-ttu-id="60d09-110">Tillståndslösa?</span><span class="sxs-lookup"><span data-stu-id="60d09-110">Stateless?</span></span>
   * <span data-ttu-id="60d09-111">Vad är alla komponenter i ditt program?</span><span class="sxs-lookup"><span data-stu-id="60d09-111">What are all the components of your application?</span></span>
     * <span data-ttu-id="60d09-112">Var finns flaskhalsar i programmet?</span><span class="sxs-lookup"><span data-stu-id="60d09-112">Where are the bottlenecks in the application?</span></span>
   * <span data-ttu-id="60d09-113">När belastningen tillämpas på appen, vad bryts första?</span><span class="sxs-lookup"><span data-stu-id="60d09-113">When load is applied to your app, what will break first?</span></span>
2. <span data-ttu-id="60d09-114">För att förstå kraven på förväntad belastning och prestanda.</span><span class="sxs-lookup"><span data-stu-id="60d09-114">Understanding the expected load and performance requirements.</span></span>
   * <span data-ttu-id="60d09-115">Behöver programmet fungerar tusen användare? eller en miljon?</span><span class="sxs-lookup"><span data-stu-id="60d09-115">Does the application need to serve one thousand users? or one million?</span></span>
   * <span data-ttu-id="60d09-116">Kommer trafik från en enda geografisk plats eller globalt?</span><span class="sxs-lookup"><span data-stu-id="60d09-116">Will traffic come from a single geographic location or globally?</span></span>
   * <span data-ttu-id="60d09-117">Finns det säsongsbaserade variationer? trafik toppar?</span><span class="sxs-lookup"><span data-stu-id="60d09-117">Are there seasonal variations? traffic peaks?</span></span>
   * <span data-ttu-id="60d09-118">Hur snabbt svara appen?</span><span class="sxs-lookup"><span data-stu-id="60d09-118">How fast should the app respond?</span></span> <span data-ttu-id="60d09-119">1 sekund?</span><span class="sxs-lookup"><span data-stu-id="60d09-119">1 second?</span></span> <span data-ttu-id="60d09-120">1 millisekund?</span><span class="sxs-lookup"><span data-stu-id="60d09-120">1 millisecond?</span></span>
3. <span data-ttu-id="60d09-121">Förstå och korrekt utnyttja den plattform som värd för din app.</span><span class="sxs-lookup"><span data-stu-id="60d09-121">Understanding and correctly leverage the platform hosting your app.</span></span>
   * <span data-ttu-id="60d09-122">Vilka funktioner ska använda för att uppnå Mina skala mål?</span><span class="sxs-lookup"><span data-stu-id="60d09-122">What features should I leverage to achieve my scale goals?</span></span>

<span data-ttu-id="60d09-123">Det här avsnittet hjälper dig att förstå alla faktorer och hjälp du utforma en strategi som drar nytta av de nödvändiga Apptjänst-funktioner för att nå dina skalbarhetsmål.</span><span class="sxs-lookup"><span data-stu-id="60d09-123">This section will help you understand all the factors and help you devise a strategy that takes advantage of the necessary App Service features to achieve your scalability goals.</span></span>

[!INCLUDE [app-service-blueprint-scaling-app-service-applications](../../includes/app-service-blueprint-scaling-app-service-applications.md)]


---
title: "Azure Apptjänst: Skala program för Apptjänst"
description: "Lär dig hello och nackdelar med skalning programmet i App Service."
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
ms.openlocfilehash: d8cd12f73915a916a75d46da2f751a40d567b189
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-scaling-app-service-applications"></a><span data-ttu-id="856b9-104">Azure Apptjänst: Skala program för Apptjänst</span><span class="sxs-lookup"><span data-stu-id="856b9-104">Azure App Service: Scaling App Service Applications</span></span>
<span data-ttu-id="856b9-105">Program som finns i Azure App Service kan [uppnå massiv skala](https://azure.microsoft.com/blog/canadian-broadcasting-corporation-radio-canada-leverage-azure-for-smooth-election-coverage/).</span><span class="sxs-lookup"><span data-stu-id="856b9-105">Applications hosted in Azure App Service can [achieve massive scale](https://azure.microsoft.com/blog/canadian-broadcasting-corporation-radio-canada-leverage-azure-for-smooth-election-coverage/).</span></span>
<span data-ttu-id="856b9-106">Skala ett program är dock komplexa problem som inte har en ”passar alla”-lösning.</span><span class="sxs-lookup"><span data-stu-id="856b9-106">However, scaling an application is a complex problem that does not have a "one size fits all" solution.</span></span> <span data-ttu-id="856b9-107">toocorrectly skala ditt program finns på 3 viktiga områden som kommer att bidra tooyour program lyckades:</span><span class="sxs-lookup"><span data-stu-id="856b9-107">toocorrectly scale your application there are 3 key areas that will contribute tooyour applications success:</span></span>

1. <span data-ttu-id="856b9-108">Förstå din programarkitektur och dess svagheter.</span><span class="sxs-lookup"><span data-stu-id="856b9-108">Understanding your application architecture and its weaknesses.</span></span>
   * <span data-ttu-id="856b9-109">Är programmet-Stateful?</span><span class="sxs-lookup"><span data-stu-id="856b9-109">Is your Application Stateful?</span></span> <span data-ttu-id="856b9-110">Tillståndslösa?</span><span class="sxs-lookup"><span data-stu-id="856b9-110">Stateless?</span></span>
   * <span data-ttu-id="856b9-111">Vad är alla hello komponenter i ditt program?</span><span class="sxs-lookup"><span data-stu-id="856b9-111">What are all hello components of your application?</span></span>
     * <span data-ttu-id="856b9-112">Var finns hello flaskhalsar i hello program?</span><span class="sxs-lookup"><span data-stu-id="856b9-112">Where are hello bottlenecks in hello application?</span></span>
   * <span data-ttu-id="856b9-113">När belastningen är tillämpade tooyour app, vad bryts första?</span><span class="sxs-lookup"><span data-stu-id="856b9-113">When load is applied tooyour app, what will break first?</span></span>
2. <span data-ttu-id="856b9-114">Förstå hello förväntad belastning och prestandakrav.</span><span class="sxs-lookup"><span data-stu-id="856b9-114">Understanding hello expected load and performance requirements.</span></span>
   * <span data-ttu-id="856b9-115">Behöver programmet hello tooserve tusen användare? eller en miljon?</span><span class="sxs-lookup"><span data-stu-id="856b9-115">Does hello application need tooserve one thousand users? or one million?</span></span>
   * <span data-ttu-id="856b9-116">Kommer trafik från en enda geografisk plats eller globalt?</span><span class="sxs-lookup"><span data-stu-id="856b9-116">Will traffic come from a single geographic location or globally?</span></span>
   * <span data-ttu-id="856b9-117">Finns det säsongsbaserade variationer? trafik toppar?</span><span class="sxs-lookup"><span data-stu-id="856b9-117">Are there seasonal variations? traffic peaks?</span></span>
   * <span data-ttu-id="856b9-118">Hur snabbt svara hello app?</span><span class="sxs-lookup"><span data-stu-id="856b9-118">How fast should hello app respond?</span></span> <span data-ttu-id="856b9-119">1 sekund?</span><span class="sxs-lookup"><span data-stu-id="856b9-119">1 second?</span></span> <span data-ttu-id="856b9-120">1 millisekund?</span><span class="sxs-lookup"><span data-stu-id="856b9-120">1 millisecond?</span></span>
3. <span data-ttu-id="856b9-121">Förstå och korrekt utnyttjar hello plattform som är värd för din app.</span><span class="sxs-lookup"><span data-stu-id="856b9-121">Understanding and correctly leverage hello platform hosting your app.</span></span>
   * <span data-ttu-id="856b9-122">Funktioner ska jag använda tooachieve Mina skala mål?</span><span class="sxs-lookup"><span data-stu-id="856b9-122">What features should I leverage tooachieve my scale goals?</span></span>

<span data-ttu-id="856b9-123">Det här avsnittet hjälper dig att förstå alla hello faktorer och hjälper dig att du utforma en strategi som drar nytta av hello nödvändiga Apptjänst funktioner tooachieve dina skalbarhetsmål.</span><span class="sxs-lookup"><span data-stu-id="856b9-123">This section will help you understand all hello factors and help you devise a strategy that takes advantage of hello necessary App Service features tooachieve your scalability goals.</span></span>

[!INCLUDE [app-service-blueprint-scaling-app-service-applications](../../includes/app-service-blueprint-scaling-app-service-applications.md)]


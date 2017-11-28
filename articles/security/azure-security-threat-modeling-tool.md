---
title: aaaMicrosoft hot Modeling verktyget - Azure | Microsoft Docs
description: "startsidan för hello hot Modeling verktyget, som innehåller information om att komma igång med hello verktyget, inklusive hello hot modellera processen"
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 923225a30c592ffdb1d254000451cfaac54a0e68
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-threat-modeling-tool"></a><span data-ttu-id="9c4a3-103">Microsoft Threat modellering verktyget</span><span class="sxs-lookup"><span data-stu-id="9c4a3-103">Microsoft Threat Modeling Tool</span></span>

<span data-ttu-id="9c4a3-104">hello hot Modeling verktyget utgör kärnan i hello Microsoft Security Development Lifecycle (SDL).</span><span class="sxs-lookup"><span data-stu-id="9c4a3-104">hello Threat Modeling Tool is a core element of hello Microsoft Security Development Lifecycle (SDL).</span></span> <span data-ttu-id="9c4a3-105">Det tillåter programvara utformar tooidentify och minimera potentiella säkerhetsproblem tidigt, när de är relativt enkel och kostnadseffektiv tooresolve.</span><span class="sxs-lookup"><span data-stu-id="9c4a3-105">It allows software architects tooidentify and mitigate potential security issues early, when they are relatively easy and cost-effective tooresolve.</span></span> <span data-ttu-id="9c4a3-106">Därför kan antalet det minskar hello totalkostnaden för utveckling.</span><span class="sxs-lookup"><span data-stu-id="9c4a3-106">As a result, it greatly reduces hello total cost of development.</span></span> <span data-ttu-id="9c4a3-107">Dessutom utformat vi hello verktyget med ej säkerhet experter ihåg förenklar hotmodellering för alla utvecklare genom att ge klara riktlinjer för att skapa och analysera hot modeller.</span><span class="sxs-lookup"><span data-stu-id="9c4a3-107">Also, we designed hello tool with non-security experts in mind, making threat modeling easier for all developers by providing clear guidance on creating and analyzing threat models.</span></span> 

<span data-ttu-id="9c4a3-108">hello verktyget gör att vem som helst:</span><span class="sxs-lookup"><span data-stu-id="9c4a3-108">hello tool enables anyone to:</span></span>

* <span data-ttu-id="9c4a3-109">Meddela om hello säkerhet utformning av sina system</span><span class="sxs-lookup"><span data-stu-id="9c4a3-109">Communicate about hello security design of their systems</span></span>
* <span data-ttu-id="9c4a3-110">Analysera layouten för potentiella säkerhetsproblem med hjälp av beprövade metoder</span><span class="sxs-lookup"><span data-stu-id="9c4a3-110">Analyze those designs for potential security issues using a proven methodology</span></span>
* <span data-ttu-id="9c4a3-111">Föreslå och hantera ändringar för säkerhetsproblem</span><span class="sxs-lookup"><span data-stu-id="9c4a3-111">Suggest and manage mitigations for security issues</span></span>

<span data-ttu-id="9c4a3-112">Här följer några funktioner för verktygsuppsättning och innovationer bara tooname några:</span><span class="sxs-lookup"><span data-stu-id="9c4a3-112">Here are some tooling capabilities and innovations, just tooname a few:</span></span>

* <span data-ttu-id="9c4a3-113">**Automation:** vägledning och feedback i Rita en modell</span><span class="sxs-lookup"><span data-stu-id="9c4a3-113">**Automation:** Guidance and feedback in drawing a model</span></span>
* <span data-ttu-id="9c4a3-114">**STRIDE per Element:** interaktiv analys av hot och åtgärder</span><span class="sxs-lookup"><span data-stu-id="9c4a3-114">**STRIDE per Element:** Guided analysis of threats and mitigations</span></span>
* <span data-ttu-id="9c4a3-115">**Rapportering:** säkerhetsaktiviteter och testning i hello verifiering fas</span><span class="sxs-lookup"><span data-stu-id="9c4a3-115">**Reporting:** Security activities and testing in hello verification phase</span></span>
* <span data-ttu-id="9c4a3-116">**Unik metod:** låter användare toobetter visualisera och förstå hot</span><span class="sxs-lookup"><span data-stu-id="9c4a3-116">**Unique Methodology:** Enables users toobetter visualize and understand threats</span></span>
* <span data-ttu-id="9c4a3-117">**Utformat för utvecklare och centrerad på programvara:** många metoder centreras på tillgångar eller angripare.</span><span class="sxs-lookup"><span data-stu-id="9c4a3-117">**Designed for Developers and Centered on Software:** many approaches are centered on assets or attackers.</span></span> <span data-ttu-id="9c4a3-118">Vi är uppbyggd på programvara.</span><span class="sxs-lookup"><span data-stu-id="9c4a3-118">We are centered on software.</span></span> <span data-ttu-id="9c4a3-119">Vi bygger på aktiviteter som alla utvecklare och arkitekter är bekant med – till exempel rita bilder för sina programvaruarkitektur</span><span class="sxs-lookup"><span data-stu-id="9c4a3-119">We build on activities that all software developers and architects are familiar with -- such as drawing pictures for their software architecture</span></span>
* <span data-ttu-id="9c4a3-120">**Fokuserar på designen analys:** hello termen ”hotmodellering” kan referera tooeither en krav eller en teknik för analys av design.</span><span class="sxs-lookup"><span data-stu-id="9c4a3-120">**Focused on Design Analysis:** hello term "threat modeling" can refer tooeither a requirements or a design analysis technique.</span></span> <span data-ttu-id="9c4a3-121">Ibland refererar tooa komplexa blandning av hello två.</span><span class="sxs-lookup"><span data-stu-id="9c4a3-121">Sometimes, it refers tooa complex blend of hello two.</span></span> <span data-ttu-id="9c4a3-122">hello Microsoft SDL metoden toothreat modellering är en teknik som fokuserad design analys</span><span class="sxs-lookup"><span data-stu-id="9c4a3-122">hello Microsoft SDL approach toothreat modeling is a focused design analysis technique</span></span>

## <a name="next-steps"></a><span data-ttu-id="9c4a3-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9c4a3-123">Next steps</span></span>

<span data-ttu-id="9c4a3-124">hello tabellen nedan innehåller viktiga länkar tooget du utgick från hello hot Modeling verktyget:</span><span class="sxs-lookup"><span data-stu-id="9c4a3-124">hello table below contains important links tooget you started with hello Threat Modeling Tool:</span></span>

| <span data-ttu-id="9c4a3-125">Steg</span><span class="sxs-lookup"><span data-stu-id="9c4a3-125">Step</span></span>  | <span data-ttu-id="9c4a3-126">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="9c4a3-126">Description</span></span>                                                                                   |
| ----- | --------------------------------------------------------------------------------------------- |
| <span data-ttu-id="9c4a3-127">**1**</span><span class="sxs-lookup"><span data-stu-id="9c4a3-127">**1**</span></span> | [<span data-ttu-id="9c4a3-128">Hämta hello hot Modeling verktyget</span><span class="sxs-lookup"><span data-stu-id="9c4a3-128">Download hello Threat Modeling Tool</span></span>](https://aka.ms/tmtpreview)                                |
| <span data-ttu-id="9c4a3-129">**2**</span><span class="sxs-lookup"><span data-stu-id="9c4a3-129">**2**</span></span> | [<span data-ttu-id="9c4a3-130">Läs våra komma igång</span><span class="sxs-lookup"><span data-stu-id="9c4a3-130">Read Our getting started guide</span></span>](./azure-security-threat-modeling-tool-getting-started.md)    |
| <span data-ttu-id="9c4a3-131">**3**</span><span class="sxs-lookup"><span data-stu-id="9c4a3-131">**3**</span></span> | [<span data-ttu-id="9c4a3-132">Bekanta dig med hello-funktioner</span><span class="sxs-lookup"><span data-stu-id="9c4a3-132">Get familiar with hello features</span></span>](./azure-security-threat-modeling-tool-feature-overview.md)   |
| <span data-ttu-id="9c4a3-133">**4**</span><span class="sxs-lookup"><span data-stu-id="9c4a3-133">**4**</span></span> | [<span data-ttu-id="9c4a3-134">Lär dig mer om genererade hot kategorier</span><span class="sxs-lookup"><span data-stu-id="9c4a3-134">Learn about generated threat categories</span></span>](./azure-security-threat-modeling-tool-threats.md)   |
| <span data-ttu-id="9c4a3-135">**5**</span><span class="sxs-lookup"><span data-stu-id="9c4a3-135">**5**</span></span> | [<span data-ttu-id="9c4a3-136">Hitta åtgärder toogenerated hot</span><span class="sxs-lookup"><span data-stu-id="9c4a3-136">Find mitigations toogenerated threats</span></span>](./azure-security-threat-modeling-tool-mitigations.md) |

## <a name="resources"></a><span data-ttu-id="9c4a3-137">Resurser</span><span class="sxs-lookup"><span data-stu-id="9c4a3-137">Resources</span></span>

<span data-ttu-id="9c4a3-138">Här följer några äldre artiklar relevant toothreat modeling idag:</span><span class="sxs-lookup"><span data-stu-id="9c4a3-138">Here are a few older articles still relevant toothreat modeling today:</span></span>

* [<span data-ttu-id="9c4a3-139">Artikel om hello vikten av hot modellering</span><span class="sxs-lookup"><span data-stu-id="9c4a3-139">Article on hello Importance of Threat Modeling</span></span>](https://msdn.microsoft.com/magazine/dd347831.aspx)
* [<span data-ttu-id="9c4a3-140">Utbildning som publicerats av Trustworthy Computing</span><span class="sxs-lookup"><span data-stu-id="9c4a3-140">Training Published by Trustworthy Computing</span></span>](https://www.microsoft.com/download/details.aspx?id=16420)

<span data-ttu-id="9c4a3-141">Kolla vad några hot Modeling verktyget experter har gjort:</span><span class="sxs-lookup"><span data-stu-id="9c4a3-141">Check out what a few Threat Modeling Tool experts have done:</span></span>

* [<span data-ttu-id="9c4a3-142">Hot Manager</span><span class="sxs-lookup"><span data-stu-id="9c4a3-142">Threats Manager</span></span>](https://simoneonsecurity.com/threatsmanagersetup-v1-5-10/)
* [<span data-ttu-id="9c4a3-143">Magnusson Curzi Säkerhetsblogg</span><span class="sxs-lookup"><span data-stu-id="9c4a3-143">Simone Curzi Security Blog</span></span>](https://simoneonsecurity.com/)
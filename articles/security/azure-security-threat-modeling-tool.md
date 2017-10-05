---
title: Microsoft Threat Modeling verktyget - Azure | Microsoft Docs
description: "startsidan för hot Modeling verktyget Microsoft, som innehåller information om att komma igång med verktyget, inklusive hot modellera processen"
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
ms.openlocfilehash: 6e26b0af2a16a872c8e02b736e24019b47ed5780
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="microsoft-threat-modeling-tool"></a><span data-ttu-id="81129-103">Microsoft Threat modellering verktyget</span><span class="sxs-lookup"><span data-stu-id="81129-103">Microsoft Threat Modeling Tool</span></span>

<span data-ttu-id="81129-104">Verktyget Modeling hot utgör kärnan i Microsoft Security Development Lifecycle (SDL).</span><span class="sxs-lookup"><span data-stu-id="81129-104">The Threat Modeling Tool is a core element of the Microsoft Security Development Lifecycle (SDL).</span></span> <span data-ttu-id="81129-105">Det gör programvaruarkitekter att identifiera och åtgärda eventuella säkerhetsfrågor tidigt, när de är relativt enkel och kostnadseffektiv att lösa.</span><span class="sxs-lookup"><span data-stu-id="81129-105">It allows software architects to identify and mitigate potential security issues early, when they are relatively easy and cost-effective to resolve.</span></span> <span data-ttu-id="81129-106">Därför kan minskar det den totala kostnaden för utveckling.</span><span class="sxs-lookup"><span data-stu-id="81129-106">As a result, it greatly reduces the total cost of development.</span></span> <span data-ttu-id="81129-107">Dessutom utformat vi verktyget med ej säkerhet experter ihåg förenklar hotmodellering för alla utvecklare genom att ge klara riktlinjer för att skapa och analysera hot modeller.</span><span class="sxs-lookup"><span data-stu-id="81129-107">Also, we designed the tool with non-security experts in mind, making threat modeling easier for all developers by providing clear guidance on creating and analyzing threat models.</span></span> 

<span data-ttu-id="81129-108">Verktyget gör att vem som helst:</span><span class="sxs-lookup"><span data-stu-id="81129-108">The tool enables anyone to:</span></span>

* <span data-ttu-id="81129-109">Meddela om säkerhetsdesignen av sina system</span><span class="sxs-lookup"><span data-stu-id="81129-109">Communicate about the security design of their systems</span></span>
* <span data-ttu-id="81129-110">Analysera layouten för potentiella säkerhetsproblem med hjälp av beprövade metoder</span><span class="sxs-lookup"><span data-stu-id="81129-110">Analyze those designs for potential security issues using a proven methodology</span></span>
* <span data-ttu-id="81129-111">Föreslå och hantera ändringar för säkerhetsproblem</span><span class="sxs-lookup"><span data-stu-id="81129-111">Suggest and manage mitigations for security issues</span></span>

<span data-ttu-id="81129-112">Här följer några funktioner för verktygsuppsättning och innovationer, utan bara för att nämna några få:</span><span class="sxs-lookup"><span data-stu-id="81129-112">Here are some tooling capabilities and innovations, just to name a few:</span></span>

* <span data-ttu-id="81129-113">**Automation:** vägledning och feedback i Rita en modell</span><span class="sxs-lookup"><span data-stu-id="81129-113">**Automation:** Guidance and feedback in drawing a model</span></span>
* <span data-ttu-id="81129-114">**STRIDE per Element:** interaktiv analys av hot och åtgärder</span><span class="sxs-lookup"><span data-stu-id="81129-114">**STRIDE per Element:** Guided analysis of threats and mitigations</span></span>
* <span data-ttu-id="81129-115">**Rapportering:** säkerhetsaktiviteter och testa verifieringen fas</span><span class="sxs-lookup"><span data-stu-id="81129-115">**Reporting:** Security activities and testing in the verification phase</span></span>
* <span data-ttu-id="81129-116">**Unik metod:** kan du lättare kan visualisera och förstå hot</span><span class="sxs-lookup"><span data-stu-id="81129-116">**Unique Methodology:** Enables users to better visualize and understand threats</span></span>
* <span data-ttu-id="81129-117">**Utformat för utvecklare och centrerad på programvara:** många metoder centreras på tillgångar eller angripare.</span><span class="sxs-lookup"><span data-stu-id="81129-117">**Designed for Developers and Centered on Software:** many approaches are centered on assets or attackers.</span></span> <span data-ttu-id="81129-118">Vi är uppbyggd på programvara.</span><span class="sxs-lookup"><span data-stu-id="81129-118">We are centered on software.</span></span> <span data-ttu-id="81129-119">Vi bygger på aktiviteter som alla utvecklare och arkitekter är bekant med – till exempel rita bilder för sina programvaruarkitektur</span><span class="sxs-lookup"><span data-stu-id="81129-119">We build on activities that all software developers and architects are familiar with -- such as drawing pictures for their software architecture</span></span>
* <span data-ttu-id="81129-120">**Fokuserar på designen analys:** termen ”hotmodellering” kan referera till ett krav eller en teknik för analys av design.</span><span class="sxs-lookup"><span data-stu-id="81129-120">**Focused on Design Analysis:** The term "threat modeling" can refer to either a requirements or a design analysis technique.</span></span> <span data-ttu-id="81129-121">Ibland, refererar den till en komplex blandning av båda.</span><span class="sxs-lookup"><span data-stu-id="81129-121">Sometimes, it refers to a complex blend of the two.</span></span> <span data-ttu-id="81129-122">Microsoft SDL-metoden för hotmodellering är en teknik för analys av fokuserad design</span><span class="sxs-lookup"><span data-stu-id="81129-122">The Microsoft SDL approach to threat modeling is a focused design analysis technique</span></span>

## <a name="next-steps"></a><span data-ttu-id="81129-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="81129-123">Next steps</span></span>

<span data-ttu-id="81129-124">Tabellen nedan innehåller viktiga länkar om du vill komma igång med verktyget Modeling hot:</span><span class="sxs-lookup"><span data-stu-id="81129-124">The table below contains important links to get you started with the Threat Modeling Tool:</span></span>

| <span data-ttu-id="81129-125">Steg</span><span class="sxs-lookup"><span data-stu-id="81129-125">Step</span></span>  | <span data-ttu-id="81129-126">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="81129-126">Description</span></span>                                                                                   |
| ----- | --------------------------------------------------------------------------------------------- |
| <span data-ttu-id="81129-127">**1**</span><span class="sxs-lookup"><span data-stu-id="81129-127">**1**</span></span> | [<span data-ttu-id="81129-128">Hämta hotet Modeling verktyget</span><span class="sxs-lookup"><span data-stu-id="81129-128">Download the Threat Modeling Tool</span></span>](https://aka.ms/tmtpreview)                                |
| <span data-ttu-id="81129-129">**2**</span><span class="sxs-lookup"><span data-stu-id="81129-129">**2**</span></span> | [<span data-ttu-id="81129-130">Läs våra komma igång</span><span class="sxs-lookup"><span data-stu-id="81129-130">Read Our getting started guide</span></span>](./azure-security-threat-modeling-tool-getting-started.md)    |
| <span data-ttu-id="81129-131">**3**</span><span class="sxs-lookup"><span data-stu-id="81129-131">**3**</span></span> | [<span data-ttu-id="81129-132">Bekanta dig med funktionerna för</span><span class="sxs-lookup"><span data-stu-id="81129-132">Get familiar with the features</span></span>](./azure-security-threat-modeling-tool-feature-overview.md)   |
| <span data-ttu-id="81129-133">**4**</span><span class="sxs-lookup"><span data-stu-id="81129-133">**4**</span></span> | [<span data-ttu-id="81129-134">Lär dig mer om genererade hot kategorier</span><span class="sxs-lookup"><span data-stu-id="81129-134">Learn about generated threat categories</span></span>](./azure-security-threat-modeling-tool-threats.md)   |
| <span data-ttu-id="81129-135">**5**</span><span class="sxs-lookup"><span data-stu-id="81129-135">**5**</span></span> | [<span data-ttu-id="81129-136">Hitta åtgärder genererade hot</span><span class="sxs-lookup"><span data-stu-id="81129-136">Find mitigations to generated threats</span></span>](./azure-security-threat-modeling-tool-mitigations.md) |

## <a name="resources"></a><span data-ttu-id="81129-137">Resurser</span><span class="sxs-lookup"><span data-stu-id="81129-137">Resources</span></span>

<span data-ttu-id="81129-138">Här följer några äldre artiklar relevant hot Modeling idag:</span><span class="sxs-lookup"><span data-stu-id="81129-138">Here are a few older articles still relevant to threat modeling today:</span></span>

* [<span data-ttu-id="81129-139">Artikel om vikten av Hotmodellering</span><span class="sxs-lookup"><span data-stu-id="81129-139">Article on the Importance of Threat Modeling</span></span>](https://msdn.microsoft.com/magazine/dd347831.aspx)
* [<span data-ttu-id="81129-140">Utbildning som publicerats av Trustworthy Computing</span><span class="sxs-lookup"><span data-stu-id="81129-140">Training Published by Trustworthy Computing</span></span>](https://www.microsoft.com/download/details.aspx?id=16420)

<span data-ttu-id="81129-141">Kolla vad några hot Modeling verktyget experter har gjort:</span><span class="sxs-lookup"><span data-stu-id="81129-141">Check out what a few Threat Modeling Tool experts have done:</span></span>

* [<span data-ttu-id="81129-142">Hot Manager</span><span class="sxs-lookup"><span data-stu-id="81129-142">Threats Manager</span></span>](https://simoneonsecurity.com/threatsmanagersetup-v1-5-10/)
* [<span data-ttu-id="81129-143">Magnusson Curzi Säkerhetsblogg</span><span class="sxs-lookup"><span data-stu-id="81129-143">Simone Curzi Security Blog</span></span>](https://simoneonsecurity.com/)
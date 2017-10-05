---
title: "Integrera säkerhet i konstruktionerna för Azure-arkitektur | Microsoft Docs"
description: " Den här artikeln hjälper dig att förstå arkitektur för program och tjänster i Azure för att göra det enklare att integrera säkerhet i designen och implementeringen. "
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TomSh
ms.assetid: 4f2d9386-bda3-4ec8-8b1a-cd0c11242ffc
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2017
ms.author: terrylan
ms.openlocfilehash: 91e46d690d3e7c298bc3b4020cc383ca99c43c4f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="application-architecture-on-azure"></a><span data-ttu-id="9ca9c-103">Programarkitektur på Azure</span><span class="sxs-lookup"><span data-stu-id="9ca9c-103">Application architecture on Azure</span></span>
<span data-ttu-id="9ca9c-104">För att säkra din molnbaserade lösningar på Microsoft Azure utgör arkitektur grundläggande är viktigt.</span><span class="sxs-lookup"><span data-stu-id="9ca9c-104">To help secure your cloud-based solutions on Microsoft Azure, a strong architectural foundation is critical.</span></span> <span data-ttu-id="9ca9c-105">Arkitekter, Designer och implementerare nytta av stora kunskaper om arkitektur för program och tjänster.</span><span class="sxs-lookup"><span data-stu-id="9ca9c-105">Architects, designers, and implementers all benefit from a strong knowledge of application and services architecture.</span></span> <span data-ttu-id="9ca9c-106">Denna grundläggande kunskap hjälper dig att förstå alla komponenter i din molnbaserade lösningar och gör det enklare att integrera säkerhet i alla aspekter av din design- och implementering.</span><span class="sxs-lookup"><span data-stu-id="9ca9c-106">This foundational knowledge helps you understand all the components of your cloud-based solutions and make it easier to integrate security into all aspects of your design and implementation.</span></span>

<span data-ttu-id="9ca9c-107">Vi har följande för att hjälpa dig med undersökningar arkitektur och utformning:</span><span class="sxs-lookup"><span data-stu-id="9ca9c-107">We have the following to help you with your architectural investigations and designs:</span></span>

* <span data-ttu-id="9ca9c-108">Arkitektur infographics</span><span class="sxs-lookup"><span data-stu-id="9ca9c-108">Architectural infographics</span></span>
* <span data-ttu-id="9ca9c-109">Arkitektur ritningarna</span><span class="sxs-lookup"><span data-stu-id="9ca9c-109">Architectural blueprints</span></span>
* <span data-ttu-id="9ca9c-110">Molnet och företagets symboluppsättningen</span><span class="sxs-lookup"><span data-stu-id="9ca9c-110">Cloud and enterprise symbol set</span></span>
* <span data-ttu-id="9ca9c-111">3D-modell Visio-mall</span><span class="sxs-lookup"><span data-stu-id="9ca9c-111">3D blueprint Visio template</span></span>

## <a name="architectural-infographics"></a><span data-ttu-id="9ca9c-112">Arkitektur infographics</span><span class="sxs-lookup"><span data-stu-id="9ca9c-112">Architectural infographics</span></span>
<span data-ttu-id="9ca9c-113">Microsoft publicerar flera arkitektur relaterade affischer/infographics.</span><span class="sxs-lookup"><span data-stu-id="9ca9c-113">Microsoft publishes several architectural related posters/infographics.</span></span> <span data-ttu-id="9ca9c-114">De omfattar:</span><span class="sxs-lookup"><span data-stu-id="9ca9c-114">They include:</span></span>

* [<span data-ttu-id="9ca9c-115">Skapa verkliga molnprogram</span><span class="sxs-lookup"><span data-stu-id="9ca9c-115">Building Real-World Cloud Applications</span></span>](https://azure.microsoft.com/documentation/infographics/building-real-world-cloud-apps/)
* [<span data-ttu-id="9ca9c-116">Skalning med molntjänster</span><span class="sxs-lookup"><span data-stu-id="9ca9c-116">Scaling with Cloud Services</span></span>](https://azure.microsoft.com/documentation/infographics/cloud-services/)

## <a name="architectural-blueprints"></a><span data-ttu-id="9ca9c-117">Arkitektur ritningarna</span><span class="sxs-lookup"><span data-stu-id="9ca9c-117">Architectural blueprints</span></span>
<span data-ttu-id="9ca9c-118">Microsoft publicerar en uppsättning övergripande [arkitektur ritningarna](http://aka.ms/azblueprints) visar hur du skapar vissa typer av system som använder Microsoft-produkter.</span><span class="sxs-lookup"><span data-stu-id="9ca9c-118">Microsoft publishes a set of high-level [architectural blueprints](http://aka.ms/azblueprints) showing how to build specific types of systems using Microsoft products.</span></span>
<span data-ttu-id="9ca9c-119">Varje modell inkluderar följande:</span><span class="sxs-lookup"><span data-stu-id="9ca9c-119">Each blueprint includes a:</span></span>

* <span data-ttu-id="9ca9c-120">Flat 2D Visio 2003-fil som du kan hämta och ändra</span><span class="sxs-lookup"><span data-stu-id="9ca9c-120">Flat 2D Visio 2003-based file that you can download and modify</span></span>
* <span data-ttu-id="9ca9c-121">Färg 3D-perspektiv PDF-fil för att kunna introducera modell till mindre tekniska målgrupper</span><span class="sxs-lookup"><span data-stu-id="9ca9c-121">Colorful 3D perspective PDF file to introduce the blueprint to less technical audiences</span></span>
* <span data-ttu-id="9ca9c-122">Video som går igenom 3D-version</span><span class="sxs-lookup"><span data-stu-id="9ca9c-122">Video that walks through the 3D version</span></span>

## <a name="cloud-and-enterprise-symbol-set"></a><span data-ttu-id="9ca9c-123">Molnet och företagets symboluppsättningen</span><span class="sxs-lookup"><span data-stu-id="9ca9c-123">Cloud and enterprise symbol set</span></span>
<span data-ttu-id="9ca9c-124">[Visa Visio och symboler utbildning video](http://aka.ms/CnESymbolsVideo) och sedan [hämta molnet och företagets Symbol ange](http://aka.ms/CnESymbols) för att skapa tekniskt material som beskriver Azure, Windows Server, SQL Server och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="9ca9c-124">[View the Visio and symbols training video](http://aka.ms/CnESymbolsVideo) and then [download the Cloud and Enterprise Symbol set](http://aka.ms/CnESymbols) to help create technical materials that describe Azure, Windows Server, SQL Server and more.</span></span> <span data-ttu-id="9ca9c-125">Du kan använda symboler i nätverksdiagram, utbildningsmaterial, presentationer, datablad, infographics, faktablad och även från tredje part böcker om boken tränar personer som ska använda Microsoft-produkter.</span><span class="sxs-lookup"><span data-stu-id="9ca9c-125">You can use the symbols in architecture diagrams, training materials, presentations, datasheets, infographics, whitepapers, and even third party books if the book trains people to use Microsoft products.</span></span> <span data-ttu-id="9ca9c-126">De är inte avsedda för användning i användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="9ca9c-126">However, they are not meant for use in user interfaces.</span></span>

## <a name="3d-blueprint-visio-template"></a><span data-ttu-id="9ca9c-127">3D-modell Visio-mall</span><span class="sxs-lookup"><span data-stu-id="9ca9c-127">3D blueprint Visio template</span></span>
<span data-ttu-id="9ca9c-128">3D-versioner av den [Microsoft arkitektur ritningarna](http://aka.ms/azblueprints) ursprungligen har skapats i en icke-Microsoft-verktyget.</span><span class="sxs-lookup"><span data-stu-id="9ca9c-128">The 3D versions of the [Microsoft Architecture Blueprints](http://aka.ms/azblueprints) were initially created in a non-Microsoft tool.</span></span> <span data-ttu-id="9ca9c-129">En ny mall för Visio 2013 (och senare) levereras på 5 augusti 2015 som en del av en [Microsoft Architecture certifikatutfärdare kursen distribueras på EDX.ORG](https://docs.microsoft.com/azure/architecture/#microsoft-architecture-certification-course).</span><span class="sxs-lookup"><span data-stu-id="9ca9c-129">A new Visio 2013 (and later) template shipped on August 5, 2015 as part of a [Microsoft Architecture certification course distributed on EDX.ORG](https://docs.microsoft.com/azure/architecture/#microsoft-architecture-certification-course).</span></span>

<span data-ttu-id="9ca9c-130">Mallen är också tillgängliga utanför kursen.</span><span class="sxs-lookup"><span data-stu-id="9ca9c-130">The template is also available outside the course.</span></span>

* <span data-ttu-id="9ca9c-131">[Videon utbildning](http://aka.ms/3dBlueprintTemplateVideo) första så att du vet vad du kan göra</span><span class="sxs-lookup"><span data-stu-id="9ca9c-131">[View the training video](http://aka.ms/3dBlueprintTemplateVideo) first so you know what it can do</span></span>
* <span data-ttu-id="9ca9c-132">Hämta den [Microsoft Visio-mall 3D-modell](http://aka.ms/3DBlueprintTemplate)</span><span class="sxs-lookup"><span data-stu-id="9ca9c-132">Download the [Microsoft 3d Blueprint Visio Template](http://aka.ms/3DBlueprintTemplate)</span></span>
* <span data-ttu-id="9ca9c-133">Hämta den [Cloud och Enterprise symboler](https://docs.microsoft.com/azure/architecture/#drawing-symbol-and-icon-sets) ska användas med 3D-mall</span><span class="sxs-lookup"><span data-stu-id="9ca9c-133">Download the [Cloud and Enterprise Symbols](https://docs.microsoft.com/azure/architecture/#drawing-symbol-and-icon-sets) to use with the 3D template</span></span>

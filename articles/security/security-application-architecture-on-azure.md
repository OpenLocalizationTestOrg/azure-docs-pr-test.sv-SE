---
title: "aaaIntegrate säkerheten för konstruktionerna för Azure-arkitektur | Microsoft Docs"
description: " Den här artikeln hjälper dig att förstå hello arkitektur för program och tjänster på Azure toomake den enklare toointegrate säkerhet i designen och implementeringen. "
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
ms.openlocfilehash: cfca8a1a2766f72bc3340c4e3df0019eb8b5a1e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-architecture-on-azure"></a><span data-ttu-id="188c4-103">Programarkitektur på Azure</span><span class="sxs-lookup"><span data-stu-id="188c4-103">Application architecture on Azure</span></span>
<span data-ttu-id="188c4-104">toohelp skydda dina molnbaserade lösningar på Microsoft Azure utgör arkitektur grundläggande är avgörande.</span><span class="sxs-lookup"><span data-stu-id="188c4-104">toohelp secure your cloud-based solutions on Microsoft Azure, a strong architectural foundation is critical.</span></span> <span data-ttu-id="188c4-105">Arkitekter, Designer och implementerare nytta av stora kunskaper om arkitektur för program och tjänster.</span><span class="sxs-lookup"><span data-stu-id="188c4-105">Architects, designers, and implementers all benefit from a strong knowledge of application and services architecture.</span></span> <span data-ttu-id="188c4-106">Denna grundläggande kunskap hjälper dig att förstå alla hello komponenter för din molnbaserade lösningar och gör det enklare toointegrate säkerhet i alla aspekter av din design- och implementering.</span><span class="sxs-lookup"><span data-stu-id="188c4-106">This foundational knowledge helps you understand all hello components of your cloud-based solutions and make it easier toointegrate security into all aspects of your design and implementation.</span></span>

<span data-ttu-id="188c4-107">Vi har hello följande toohelp du med undersökningar arkitektur och utformning:</span><span class="sxs-lookup"><span data-stu-id="188c4-107">We have hello following toohelp you with your architectural investigations and designs:</span></span>

* <span data-ttu-id="188c4-108">Arkitektur infographics</span><span class="sxs-lookup"><span data-stu-id="188c4-108">Architectural infographics</span></span>
* <span data-ttu-id="188c4-109">Arkitektur ritningarna</span><span class="sxs-lookup"><span data-stu-id="188c4-109">Architectural blueprints</span></span>
* <span data-ttu-id="188c4-110">Molnet och företagets symboluppsättningen</span><span class="sxs-lookup"><span data-stu-id="188c4-110">Cloud and enterprise symbol set</span></span>
* <span data-ttu-id="188c4-111">3D-modell Visio-mall</span><span class="sxs-lookup"><span data-stu-id="188c4-111">3D blueprint Visio template</span></span>

## <a name="architectural-infographics"></a><span data-ttu-id="188c4-112">Arkitektur infographics</span><span class="sxs-lookup"><span data-stu-id="188c4-112">Architectural infographics</span></span>
<span data-ttu-id="188c4-113">Microsoft publicerar flera arkitektur relaterade affischer/infographics.</span><span class="sxs-lookup"><span data-stu-id="188c4-113">Microsoft publishes several architectural related posters/infographics.</span></span> <span data-ttu-id="188c4-114">De omfattar:</span><span class="sxs-lookup"><span data-stu-id="188c4-114">They include:</span></span>

* [<span data-ttu-id="188c4-115">Skapa verkliga molnprogram</span><span class="sxs-lookup"><span data-stu-id="188c4-115">Building Real-World Cloud Applications</span></span>](https://azure.microsoft.com/documentation/infographics/building-real-world-cloud-apps/)
* [<span data-ttu-id="188c4-116">Skalning med molntjänster</span><span class="sxs-lookup"><span data-stu-id="188c4-116">Scaling with Cloud Services</span></span>](https://azure.microsoft.com/documentation/infographics/cloud-services/)

## <a name="architectural-blueprints"></a><span data-ttu-id="188c4-117">Arkitektur ritningarna</span><span class="sxs-lookup"><span data-stu-id="188c4-117">Architectural blueprints</span></span>
<span data-ttu-id="188c4-118">Microsoft publicerar en uppsättning övergripande [arkitektur ritningarna](http://aka.ms/azblueprints) visar hur toobuild specifika typer av system som använder Microsoft-produkter.</span><span class="sxs-lookup"><span data-stu-id="188c4-118">Microsoft publishes a set of high-level [architectural blueprints](http://aka.ms/azblueprints) showing how toobuild specific types of systems using Microsoft products.</span></span>
<span data-ttu-id="188c4-119">Varje modell inkluderar följande:</span><span class="sxs-lookup"><span data-stu-id="188c4-119">Each blueprint includes a:</span></span>

* <span data-ttu-id="188c4-120">Flat 2D Visio 2003-fil som du kan hämta och ändra</span><span class="sxs-lookup"><span data-stu-id="188c4-120">Flat 2D Visio 2003-based file that you can download and modify</span></span>
* <span data-ttu-id="188c4-121">Färg 3D-perspektiv PDF-filen toointroduce hello utkast tooless tekniska målgrupper</span><span class="sxs-lookup"><span data-stu-id="188c4-121">Colorful 3D perspective PDF file toointroduce hello blueprint tooless technical audiences</span></span>
* <span data-ttu-id="188c4-122">Video som går igenom hello 3D-version</span><span class="sxs-lookup"><span data-stu-id="188c4-122">Video that walks through hello 3D version</span></span>

## <a name="cloud-and-enterprise-symbol-set"></a><span data-ttu-id="188c4-123">Molnet och företagets symboluppsättningen</span><span class="sxs-lookup"><span data-stu-id="188c4-123">Cloud and enterprise symbol set</span></span>
<span data-ttu-id="188c4-124">[Visa hello Visio och symboler utbildning video](http://aka.ms/CnESymbolsVideo) och sedan [hämta hello Cloud och Enterprise-symbolen som](http://aka.ms/CnESymbols) toohelp skapa tekniska material som beskriver Azure, Windows Server, SQL Server och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="188c4-124">[View hello Visio and symbols training video](http://aka.ms/CnESymbolsVideo) and then [download hello Cloud and Enterprise Symbol set](http://aka.ms/CnESymbols) toohelp create technical materials that describe Azure, Windows Server, SQL Server and more.</span></span> <span data-ttu-id="188c4-125">Du kan använda hello symboler i nätverksdiagram, utbildningsmaterial, presentationer, datablad, infographics, faktablad och även från tredje part böcker om hello book tåg personer toouse Microsoft-produkter.</span><span class="sxs-lookup"><span data-stu-id="188c4-125">You can use hello symbols in architecture diagrams, training materials, presentations, datasheets, infographics, whitepapers, and even third party books if hello book trains people toouse Microsoft products.</span></span> <span data-ttu-id="188c4-126">De är inte avsedda för användning i användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="188c4-126">However, they are not meant for use in user interfaces.</span></span>

## <a name="3d-blueprint-visio-template"></a><span data-ttu-id="188c4-127">3D-modell Visio-mall</span><span class="sxs-lookup"><span data-stu-id="188c4-127">3D blueprint Visio template</span></span>
<span data-ttu-id="188c4-128">Hej 3D-versioner av hello [Microsoft arkitektur ritningarna](http://aka.ms/azblueprints) ursprungligen har skapats i en icke-Microsoft-verktyget.</span><span class="sxs-lookup"><span data-stu-id="188c4-128">hello 3D versions of hello [Microsoft Architecture Blueprints](http://aka.ms/azblueprints) were initially created in a non-Microsoft tool.</span></span> <span data-ttu-id="188c4-129">En ny mall för Visio 2013 (och senare) levereras på 5 augusti 2015 som en del av en [Microsoft Architecture certifikatutfärdare kursen distribueras på EDX.ORG](https://docs.microsoft.com/azure/architecture/#microsoft-architecture-certification-course).</span><span class="sxs-lookup"><span data-stu-id="188c4-129">A new Visio 2013 (and later) template shipped on August 5, 2015 as part of a [Microsoft Architecture certification course distributed on EDX.ORG](https://docs.microsoft.com/azure/architecture/#microsoft-architecture-certification-course).</span></span>

<span data-ttu-id="188c4-130">hello mallen är också tillgängliga utanför hello kursen.</span><span class="sxs-lookup"><span data-stu-id="188c4-130">hello template is also available outside hello course.</span></span>

* <span data-ttu-id="188c4-131">[Visa hello utbildningsvideo](http://aka.ms/3dBlueprintTemplateVideo) första så att du vet vad du kan göra</span><span class="sxs-lookup"><span data-stu-id="188c4-131">[View hello training video](http://aka.ms/3dBlueprintTemplateVideo) first so you know what it can do</span></span>
* <span data-ttu-id="188c4-132">Hämta hello [Microsoft Visio-mall 3D-modell](http://aka.ms/3DBlueprintTemplate)</span><span class="sxs-lookup"><span data-stu-id="188c4-132">Download hello [Microsoft 3d Blueprint Visio Template](http://aka.ms/3DBlueprintTemplate)</span></span>
* <span data-ttu-id="188c4-133">Hämta hello [Cloud och Enterprise symboler](https://docs.microsoft.com/azure/architecture/#drawing-symbol-and-icon-sets) toouse med hello 3D-mall</span><span class="sxs-lookup"><span data-stu-id="188c4-133">Download hello [Cloud and Enterprise Symbols](https://docs.microsoft.com/azure/architecture/#drawing-symbol-and-icon-sets) toouse with hello 3D template</span></span>

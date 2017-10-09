---
title: "aaaRestrict Azure CDN innehåll efter land | Microsoft Docs"
description: "Lär dig hur toorestrict tooyour Azure CDN innehåll med hjälp av hello filtrering av Geo-funktionen."
services: cdn
documentationcenter: 
author: lichard
manager: akucer
editor: 
ms.assetid: 12c17cc5-28ee-4b0b-ba22-2266be2e786a
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: rli
ms.openlocfilehash: ffdd994612b6c9cfbf1a6e29d260709b4afa86e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="restrict-azure-cdn-content-by-country"></a><span data-ttu-id="6bce8-103">Begränsa Azure CDN innehåll efter land</span><span class="sxs-lookup"><span data-stu-id="6bce8-103">Restrict Azure CDN content by country</span></span>

## <a name="overview"></a><span data-ttu-id="6bce8-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="6bce8-104">Overview</span></span>
<span data-ttu-id="6bce8-105">När en användare begär innehåll, som standard, hanteras hello innehållet oavsett om hello användare gjort denna begäran från.</span><span class="sxs-lookup"><span data-stu-id="6bce8-105">When a user requests your content, by default, hello content is served regardless of where hello user made this request from.</span></span> <span data-ttu-id="6bce8-106">I vissa fall kan kanske du vill toorestrict åtkomst till tooyour innehåll efter land.</span><span class="sxs-lookup"><span data-stu-id="6bce8-106">In some cases, you may want toorestrict access tooyour content by country.</span></span> <span data-ttu-id="6bce8-107">Det här avsnittet beskrivs hur toouse hello **Geo-filtrering** funktion i ordning tooconfigure hello service tooallow eller blockera åtkomst av land.</span><span class="sxs-lookup"><span data-stu-id="6bce8-107">This topic explains how toouse hello **Geo-Filtering** feature in order tooconfigure hello service tooallow or block access by country.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6bce8-108">hello Verizon och Akamai produkter har hello samma funktioner för filtrering av geo men har en liten skillnad i te landskoder som de stöder.</span><span class="sxs-lookup"><span data-stu-id="6bce8-108">hello Verizon and Akamai products provide hello same geo-filtering functionality but have a small difference in te country codes they support.</span></span> <span data-ttu-id="6bce8-109">Se steg3 för en länk toohello skillnader.</span><span class="sxs-lookup"><span data-stu-id="6bce8-109">See Step 3 for a link toohello differences.</span></span>


<span data-ttu-id="6bce8-110">Den här typen av begränsning finns information om saker som gäller tooconfiguring hello [överväganden](cdn-restrict-access-by-country.md#considerations) avsnittet hello slutet av hello-avsnittet.</span><span class="sxs-lookup"><span data-stu-id="6bce8-110">For information about considerations that apply tooconfiguring this type of restriction, see hello [Considerations](cdn-restrict-access-by-country.md#considerations) section at hello end of hello topic.</span></span>  

![Landsfiltrering](./media/cdn-filtering/cdn-country-filtering-akamai.png)

## <a name="step-1-define-hello-directory-path"></a><span data-ttu-id="6bce8-112">Steg 1: Definiera hello sökväg</span><span class="sxs-lookup"><span data-stu-id="6bce8-112">Step 1: Define hello directory path</span></span>
<span data-ttu-id="6bce8-113">Välj din slutpunkt i hello-portalen och hitta hello Geo-filtrering fliken på hello vänstra navigeringsfönstret toofind den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="6bce8-113">Select your endpoint within hello portal, and find hello Geo-Filtering tab on hello left-hand navigation toofind this feature.</span></span>

<span data-ttu-id="6bce8-114">När du konfigurerar land-filtret, måste du ange hello relativ sökväg toohello plats toowhich användare ska tillåtas eller nekas åtkomst.</span><span class="sxs-lookup"><span data-stu-id="6bce8-114">When configuring a country filter, you must specify hello relative path toohello location toowhich users will be allowed or denied access.</span></span> <span data-ttu-id="6bce8-115">Du kan använda geo-filtrering för alla filer med ”/” eller valda mappar genom att ange katalogsökvägar ”/ bilder /”.</span><span class="sxs-lookup"><span data-stu-id="6bce8-115">You can apply geo-filtering for all your files with "/" or selected folders by specifying directory paths "/pictures/".</span></span> <span data-ttu-id="6bce8-116">Du kan också använda filtrering av geo tooa en fil genom att ange hello-fil och lämnar hello avslutande snedstreck ”/ pictures/city.png”.</span><span class="sxs-lookup"><span data-stu-id="6bce8-116">You can also apply geo-filtering tooa single file by specifying hello file, and leaving out hello trailing slash "/pictures/city.png".</span></span>

<span data-ttu-id="6bce8-117">Exempel directory sökvägen filter:</span><span class="sxs-lookup"><span data-stu-id="6bce8-117">Example directory path filter:</span></span>

    /                                 
    /Photos/
    /Photos/Strasbourg/
      /Photos/Strasbourg/city.png

## <a name="step-2-define-hello-action-block-or-allow"></a><span data-ttu-id="6bce8-118">Steg 2: Definiera hello åtgärd: blockera eller tillåta</span><span class="sxs-lookup"><span data-stu-id="6bce8-118">Step 2: Define hello action: block or allow</span></span>
<span data-ttu-id="6bce8-119">**Blockering:** användare från hello angivna länder kommer att nekas åtkomst tooassets begärs från den rekursiva sökvägen.</span><span class="sxs-lookup"><span data-stu-id="6bce8-119">**Block:** Users from hello specified countries will be denied access tooassets requested from that recursive path.</span></span> <span data-ttu-id="6bce8-120">Om inga andra land filtreringsalternativ har konfigurerats för den platsen, sedan får alla andra användare åtkomst.</span><span class="sxs-lookup"><span data-stu-id="6bce8-120">If no other country filtering options have been configured for that location, then all other users will be allowed access.</span></span>

<span data-ttu-id="6bce8-121">**Tillåt:** endast användare från hello angivna länder tillåts åtkomst tooassets begärs från den rekursiva sökvägen.</span><span class="sxs-lookup"><span data-stu-id="6bce8-121">**Allow:** Only users from hello specified countries will be allowed access tooassets requested from that recursive path.</span></span>

## <a name="step-3-define-hello-countries"></a><span data-ttu-id="6bce8-122">Steg 3: Definiera hello länder</span><span class="sxs-lookup"><span data-stu-id="6bce8-122">Step 3: Define hello countries</span></span>
<span data-ttu-id="6bce8-123">Välj hello länder du vill tooblock eller tillåta hello sökväg.</span><span class="sxs-lookup"><span data-stu-id="6bce8-123">Select hello countries that you want tooblock or allow for hello path.</span></span> 

<span data-ttu-id="6bce8-124">Till exempel filtrerar hello regeln för blockeringen /Photos/Strasbourg/filer, inklusive:</span><span class="sxs-lookup"><span data-stu-id="6bce8-124">For example, hello rule of blocking /Photos/Strasbourg/ will filter files including:</span></span>

    http://<endpoint>.azureedge.net/Photos/Strasbourg/1000.jpg
    http://<endpoint>.azureedge.net/Photos/Strasbourg/Cathedral/1000.jpg


### <a name="country-codes"></a><span data-ttu-id="6bce8-125">Landskoder</span><span class="sxs-lookup"><span data-stu-id="6bce8-125">Country codes</span></span>
<span data-ttu-id="6bce8-126">Hej **Geo-filtrering** funktionen använder land koder toodefine hello länder där en begäran ska tillåtas eller blockeras för en skyddad katalog.</span><span class="sxs-lookup"><span data-stu-id="6bce8-126">hello **Geo-Filtering** feature uses country codes toodefine hello countries from which a request will be allowed or blocked for a secured directory.</span></span> <span data-ttu-id="6bce8-127">Du hittar hello landskoder i [Azure CDN landskoder](https://msdn.microsoft.com/library/mt761717.aspx).</span><span class="sxs-lookup"><span data-stu-id="6bce8-127">You will find hello country codes in [Azure CDN  Country Codes](https://msdn.microsoft.com/library/mt761717.aspx).</span></span> 

## <span data-ttu-id="6bce8-128"><a id="considerations"></a>Överväganden</span><span class="sxs-lookup"><span data-stu-id="6bce8-128"><a id="considerations"></a>Considerations</span></span>
* <span data-ttu-id="6bce8-129">Det kan ta upp too90 minuter för Verizon, eller ett par minuter med Akamai, för ändringar tooyour land filtrering configuration tootake effekt.</span><span class="sxs-lookup"><span data-stu-id="6bce8-129">It may take up too90 minutes for Verizon, or a couple minutes with Akamai, for changes tooyour country filtering configuration tootake effect.</span></span>
* <span data-ttu-id="6bce8-130">Den här funktionen har inte stöd för jokertecken (till exempel ' *').</span><span class="sxs-lookup"><span data-stu-id="6bce8-130">This feature does not support wildcard characters (for example, ‘*’).</span></span>
* <span data-ttu-id="6bce8-131">hello geo-filtrering konfiguration som är associerad med hello relativa sökvägen kommer att tillämpas rekursivt toothat sökväg.</span><span class="sxs-lookup"><span data-stu-id="6bce8-131">hello geo-filtering configuration associated with hello relative path will be applied recursively toothat path.</span></span>
* <span data-ttu-id="6bce8-132">Endast en regel kan vara tillämpade toohello samma relativa sökväg (du kan inte skapa flera land filter som punkt toohello samma relativa sökväg.</span><span class="sxs-lookup"><span data-stu-id="6bce8-132">Only one rule can be applied toohello same relative path (you cannot create multiple country filters that point toohello same relative path.</span></span> <span data-ttu-id="6bce8-133">En mapp kan dock ha flera land filter.</span><span class="sxs-lookup"><span data-stu-id="6bce8-133">However, a folder may have multiple country filters.</span></span> <span data-ttu-id="6bce8-134">Detta är på grund av toohello rekursiv uppbyggnad land filter.</span><span class="sxs-lookup"><span data-stu-id="6bce8-134">This is due toohello recursive nature of country filters.</span></span> <span data-ttu-id="6bce8-135">Med andra ord en undermapp till en tidigare konfigurerade mapp som kan tilldelas ett annat land filter.</span><span class="sxs-lookup"><span data-stu-id="6bce8-135">In other words, a subfolder of a previously configured folder can be assigned a different country filter.</span></span>


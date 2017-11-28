---
title: "Begränsa Azure CDN innehåll efter land | Microsoft Docs"
description: "Lär dig hur du begränsar åtkomsten till ditt Azure CDN-innehåll med hjälp av funktionen Geo-filtrering."
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
ms.openlocfilehash: 30160088d9c770400f342e67527e1cf1cabc4f6b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="restrict-azure-cdn-content-by-country"></a><span data-ttu-id="4b0b0-103">Begränsa Azure CDN innehåll efter land</span><span class="sxs-lookup"><span data-stu-id="4b0b0-103">Restrict Azure CDN content by country</span></span>

## <a name="overview"></a><span data-ttu-id="4b0b0-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="4b0b0-104">Overview</span></span>
<span data-ttu-id="4b0b0-105">När en användare begär innehåll, som standard, hanteras innehållet oavsett om användaren har gjort denna begäran från.</span><span class="sxs-lookup"><span data-stu-id="4b0b0-105">When a user requests your content, by default, the content is served regardless of where the user made this request from.</span></span> <span data-ttu-id="4b0b0-106">I vissa fall kanske du vill begränsa åtkomsten till ditt innehåll efter land.</span><span class="sxs-lookup"><span data-stu-id="4b0b0-106">In some cases, you may want to restrict access to your content by country.</span></span> <span data-ttu-id="4b0b0-107">Det här avsnittet beskriver hur du använder den **Geo-filtrering** funktionen för att konfigurera tjänsten om du vill tillåta eller blockera åtkomst efter land.</span><span class="sxs-lookup"><span data-stu-id="4b0b0-107">This topic explains how to use the **Geo-Filtering** feature in order to configure the service to allow or block access by country.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4b0b0-108">Verizon och Akamai produkter ge samma funktioner för filtrering av geo men har en liten skillnad i te landskoder som de stöder.</span><span class="sxs-lookup"><span data-stu-id="4b0b0-108">The Verizon and Akamai products provide the same geo-filtering functionality but have a small difference in te country codes they support.</span></span> <span data-ttu-id="4b0b0-109">En länk till skillnaderna finns i steg3.</span><span class="sxs-lookup"><span data-stu-id="4b0b0-109">See Step 3 for a link to the differences.</span></span>


<span data-ttu-id="4b0b0-110">Information om saker som gäller för att konfigurera den här typen av begränsning finns på [överväganden](cdn-restrict-access-by-country.md#considerations) avsnittet i slutet av ämnet.</span><span class="sxs-lookup"><span data-stu-id="4b0b0-110">For information about considerations that apply to configuring this type of restriction, see the [Considerations](cdn-restrict-access-by-country.md#considerations) section at the end of the topic.</span></span>  

![Landsfiltrering](./media/cdn-filtering/cdn-country-filtering-akamai.png)

## <a name="step-1-define-the-directory-path"></a><span data-ttu-id="4b0b0-112">Steg 1: Definiera katalogsökvägen</span><span class="sxs-lookup"><span data-stu-id="4b0b0-112">Step 1: Define the directory path</span></span>
<span data-ttu-id="4b0b0-113">Välj din slutpunkt i portalen och hitta fliken Geo-filtrering på det vänstra navigeringsfönstret att hitta den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="4b0b0-113">Select your endpoint within the portal, and find the Geo-Filtering tab on the left-hand navigation to find this feature.</span></span>

<span data-ttu-id="4b0b0-114">När du konfigurerar land-filtret, måste du ange den relativa sökvägen till den plats som användare beviljas eller nekas åtkomst.</span><span class="sxs-lookup"><span data-stu-id="4b0b0-114">When configuring a country filter, you must specify the relative path to the location to which users will be allowed or denied access.</span></span> <span data-ttu-id="4b0b0-115">Du kan använda geo-filtrering för alla filer med ”/” eller valda mappar genom att ange katalogsökvägar ”/ bilder /”.</span><span class="sxs-lookup"><span data-stu-id="4b0b0-115">You can apply geo-filtering for all your files with "/" or selected folders by specifying directory paths "/pictures/".</span></span> <span data-ttu-id="4b0b0-116">Du kan också använda filtrering av geo-replikering till en enda fil genom att ange filen och lämnar ut avslutande snedstreck ”/ pictures/city.png”.</span><span class="sxs-lookup"><span data-stu-id="4b0b0-116">You can also apply geo-filtering to a single file by specifying the file, and leaving out the trailing slash "/pictures/city.png".</span></span>

<span data-ttu-id="4b0b0-117">Exempel directory sökvägen filter:</span><span class="sxs-lookup"><span data-stu-id="4b0b0-117">Example directory path filter:</span></span>

    /                                 
    /Photos/
    /Photos/Strasbourg/
      /Photos/Strasbourg/city.png

## <a name="step-2-define-the-action-block-or-allow"></a><span data-ttu-id="4b0b0-118">Steg 2: Definiera åtgärden: blockera eller tillåta</span><span class="sxs-lookup"><span data-stu-id="4b0b0-118">Step 2: Define the action: block or allow</span></span>
<span data-ttu-id="4b0b0-119">**Blockering:** användare från de angivna länderna nekas åtkomst till tillgångar som begärs från den rekursiva sökvägen.</span><span class="sxs-lookup"><span data-stu-id="4b0b0-119">**Block:** Users from the specified countries will be denied access to assets requested from that recursive path.</span></span> <span data-ttu-id="4b0b0-120">Om inga andra land filtreringsalternativ har konfigurerats för den platsen, sedan får alla andra användare åtkomst.</span><span class="sxs-lookup"><span data-stu-id="4b0b0-120">If no other country filtering options have been configured for that location, then all other users will be allowed access.</span></span>

<span data-ttu-id="4b0b0-121">**Tillåt:** endast användare från de angivna länderna får åtkomst till tillgångar som begärs från den rekursiva sökvägen.</span><span class="sxs-lookup"><span data-stu-id="4b0b0-121">**Allow:** Only users from the specified countries will be allowed access to assets requested from that recursive path.</span></span>

## <a name="step-3-define-the-countries"></a><span data-ttu-id="4b0b0-122">Steg 3: Definiera länderna</span><span class="sxs-lookup"><span data-stu-id="4b0b0-122">Step 3: Define the countries</span></span>
<span data-ttu-id="4b0b0-123">Välj land som du vill blockera eller tillåta för sökvägen.</span><span class="sxs-lookup"><span data-stu-id="4b0b0-123">Select the countries that you want to block or allow for the path.</span></span> 

<span data-ttu-id="4b0b0-124">Regeln för blockeringen /Photos/Strasbourg/kommer till exempel filtrera filer, inklusive:</span><span class="sxs-lookup"><span data-stu-id="4b0b0-124">For example, the rule of blocking /Photos/Strasbourg/ will filter files including:</span></span>

    http://<endpoint>.azureedge.net/Photos/Strasbourg/1000.jpg
    http://<endpoint>.azureedge.net/Photos/Strasbourg/Cathedral/1000.jpg


### <a name="country-codes"></a><span data-ttu-id="4b0b0-125">Landskoder</span><span class="sxs-lookup"><span data-stu-id="4b0b0-125">Country codes</span></span>
<span data-ttu-id="4b0b0-126">Den **Geo-filtrering** funktionen använder landskoder för att definiera de länder där en begäran ska tillåtas eller blockeras för en skyddad katalog.</span><span class="sxs-lookup"><span data-stu-id="4b0b0-126">The **Geo-Filtering** feature uses country codes to define the countries from which a request will be allowed or blocked for a secured directory.</span></span> <span data-ttu-id="4b0b0-127">Du hittar landskoder i [Azure CDN landskoder](https://msdn.microsoft.com/library/mt761717.aspx).</span><span class="sxs-lookup"><span data-stu-id="4b0b0-127">You will find the country codes in [Azure CDN  Country Codes](https://msdn.microsoft.com/library/mt761717.aspx).</span></span> 

## <span data-ttu-id="4b0b0-128"><a id="considerations"></a>Överväganden</span><span class="sxs-lookup"><span data-stu-id="4b0b0-128"><a id="considerations"></a>Considerations</span></span>
* <span data-ttu-id="4b0b0-129">Det kan ta upp till 90 minuter för Verizon eller ett par minuter med Akamai, ändringar i ditt land filtrering konfigurationen ska börja gälla.</span><span class="sxs-lookup"><span data-stu-id="4b0b0-129">It may take up to 90 minutes for Verizon, or a couple minutes with Akamai, for changes to your country filtering configuration to take effect.</span></span>
* <span data-ttu-id="4b0b0-130">Den här funktionen har inte stöd för jokertecken (till exempel ' *').</span><span class="sxs-lookup"><span data-stu-id="4b0b0-130">This feature does not support wildcard characters (for example, ‘*’).</span></span>
* <span data-ttu-id="4b0b0-131">Geo-filtrering konfigurationen som är associerade med den relativa sökvägen ska tillämpas rekursivt till denna sökväg.</span><span class="sxs-lookup"><span data-stu-id="4b0b0-131">The geo-filtering configuration associated with the relative path will be applied recursively to that path.</span></span>
* <span data-ttu-id="4b0b0-132">Endast en regel kan tillämpas på samma relativa sökväg (du kan skapa flera land filter som pekar på samma relativa sökväg.</span><span class="sxs-lookup"><span data-stu-id="4b0b0-132">Only one rule can be applied to the same relative path (you cannot create multiple country filters that point to the same relative path.</span></span> <span data-ttu-id="4b0b0-133">En mapp kan dock ha flera land filter.</span><span class="sxs-lookup"><span data-stu-id="4b0b0-133">However, a folder may have multiple country filters.</span></span> <span data-ttu-id="4b0b0-134">Detta beror på den rekursiva natur land filter.</span><span class="sxs-lookup"><span data-stu-id="4b0b0-134">This is due to the recursive nature of country filters.</span></span> <span data-ttu-id="4b0b0-135">Med andra ord en undermapp till en tidigare konfigurerade mapp som kan tilldelas ett annat land filter.</span><span class="sxs-lookup"><span data-stu-id="4b0b0-135">In other words, a subfolder of a previously configured folder can be assigned a different country filter.</span></span>


---
title: "Hantera datatillgångar i Azure Data Catalog | Microsoft Docs"
description: "Artikeln visar hur du styr synligheten och ägarskapet för datatillgångar som har registrerats i Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 623f5ed4-8da7-48f5-943a-448d0b7cba69
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 8b9159b7bc4f7b2dac12d9012c6c903e75a6ac16
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="manage-data-assets-in-azure-data-catalog"></a><span data-ttu-id="d3284-103">Hantera datatillgångar i Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="d3284-103">Manage data assets in Azure Data Catalog</span></span>
## <a name="introduction"></a><span data-ttu-id="d3284-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="d3284-104">Introduction</span></span>
<span data-ttu-id="d3284-105">Azure Data Catalog är utformad för identifiering av datakällan, så att du lätt kan identifiera och förstå datakällorna måste du utföra analyser och fatta beslut.</span><span class="sxs-lookup"><span data-stu-id="d3284-105">Azure Data Catalog is designed for data-source discovery, so that you can easily discover and understand the data sources you need to perform analysis and make decisions.</span></span> <span data-ttu-id="d3284-106">Dessa funktioner för identifiering av få den största effekten när du och andra användare kan hitta och förstå mycket stort antal typer av datakällor.</span><span class="sxs-lookup"><span data-stu-id="d3284-106">These discovery capabilities make the biggest impact when you and other users can find and understand the broadest range of available data sources.</span></span> <span data-ttu-id="d3284-107">Med de här elementen i åtanke är standardbeteendet för Data Catalog för alla registrerade datakällor ska vara synlig för och kan identifieras av alla användare i katalogen.</span><span class="sxs-lookup"><span data-stu-id="d3284-107">With these elements in mind, the default behavior of Data Catalog is for all registered data sources to be visible to and discoverable by all catalog users.</span></span>

<span data-ttu-id="d3284-108">Data Catalog ger inte dig tillgång till själva informationen.</span><span class="sxs-lookup"><span data-stu-id="d3284-108">Data Catalog does not give you access to the data itself.</span></span> <span data-ttu-id="d3284-109">Dataåtkomst styrs av ägaren till datakällan.</span><span class="sxs-lookup"><span data-stu-id="d3284-109">Data access is controlled by the owner of the data source.</span></span> <span data-ttu-id="d3284-110">Du kan identifiera datakällor och visa de metadata som är relaterade till datakällor som är registrerade i katalogen med Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="d3284-110">With Data Catalog, you can discover data sources and view the metadata that's related to the sources that are registered in the catalog.</span></span>

<span data-ttu-id="d3284-111">Det kan finnas situationer, men var datakällor bara ska visas för specifika användare eller medlemmar i specifika grupper.</span><span class="sxs-lookup"><span data-stu-id="d3284-111">There might be situations, however, where data sources should only be visible to specific users, or to members of specific groups.</span></span> <span data-ttu-id="d3284-112">I sådana fall kan användarna bli ägare till registrerade datatillgångar i katalogen och styra visningen av de resurser som de äger.</span><span class="sxs-lookup"><span data-stu-id="d3284-112">In such scenarios, users can take ownership of registered data assets within the catalog and then control the visibility of the assets they own.</span></span>

> [!NOTE]
> <span data-ttu-id="d3284-113">Funktioner som beskrivs i den här artikeln är endast tillgänglig i Standard Edition av Azure Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="d3284-113">The functionality described in this article is available only in the Standard Edition of Azure Data Catalog.</span></span> <span data-ttu-id="d3284-114">Den kostnadsfria versionen ger inte några funktioner för ägare och begränsning datatillgång synlighet.</span><span class="sxs-lookup"><span data-stu-id="d3284-114">The Free Edition does not provide capabilities for ownership and restricting data-asset visibility.</span></span>
>
>

## <a name="manage-ownership-of-data-assets"></a><span data-ttu-id="d3284-115">Hantera ägarskapet för datatillgångar</span><span class="sxs-lookup"><span data-stu-id="d3284-115">Manage ownership of data assets</span></span>
<span data-ttu-id="d3284-116">Som standard är datatillgångar som är registrerade i Data Catalog oägd.</span><span class="sxs-lookup"><span data-stu-id="d3284-116">By default, data assets that are registered in Data Catalog are unowned.</span></span> <span data-ttu-id="d3284-117">En användare med behörighet att få åtkomst till katalogen kan identifiera och kommentera dessa tillgångar.</span><span class="sxs-lookup"><span data-stu-id="d3284-117">Any user with permission to access the catalog can discover and annotate these assets.</span></span> <span data-ttu-id="d3284-118">Användare kan bli ägare till oägd datatillgångar och begränsar synligheten för de resurser som de äger.</span><span class="sxs-lookup"><span data-stu-id="d3284-118">Users can take ownership of unowned data assets and then limit the visibility of the assets they own.</span></span>

<span data-ttu-id="d3284-119">När en datatillgång i Data Catalog ägs endast användare som är godkända av ägarna kan identifiera tillgången och visa dess metadata och endast ägare kan ta bort tillgången från katalogen.</span><span class="sxs-lookup"><span data-stu-id="d3284-119">When a data asset in Data Catalog is owned, only users who are authorized by the owners can discover the asset and view its metadata, and only the owners can delete the asset from the catalog.</span></span>

> [!NOTE]
> <span data-ttu-id="d3284-120">Ägarskap i Data Catalog påverkar endast de metadata som lagras i katalogen.</span><span class="sxs-lookup"><span data-stu-id="d3284-120">Ownership in Data Catalog affects only the metadata that's stored in the catalog.</span></span> <span data-ttu-id="d3284-121">Ägarskap ger inte några behörigheter på den underliggande datakällan.</span><span class="sxs-lookup"><span data-stu-id="d3284-121">Ownership does not confer any permissions on the underlying data source.</span></span>
>
>

### <a name="take-ownership"></a><span data-ttu-id="d3284-122">Bli ägare</span><span class="sxs-lookup"><span data-stu-id="d3284-122">Take ownership</span></span>
<span data-ttu-id="d3284-123">Användare kan ta över ägarskapet för datatillgångar genom att välja den **bli ägare** alternativ i Data Catalog-portalen.</span><span class="sxs-lookup"><span data-stu-id="d3284-123">Users can take ownership of data assets by selecting the **Take Ownership** option in the Data Catalog portal.</span></span> <span data-ttu-id="d3284-124">Inga särskilda behörigheter krävs för att överta ägarskapet för en datatillgång oägd.</span><span class="sxs-lookup"><span data-stu-id="d3284-124">No special permissions are required to take ownership of an unowned data asset.</span></span> <span data-ttu-id="d3284-125">Alla användare kan bli ägare för en datatillgång oägd.</span><span class="sxs-lookup"><span data-stu-id="d3284-125">Any user can take ownership of an unowned data asset.</span></span>

### <a name="add-owners-and-co-owners"></a><span data-ttu-id="d3284-126">Lägg till ägare och Medägare</span><span class="sxs-lookup"><span data-stu-id="d3284-126">Add owners and co-owners</span></span>
<span data-ttu-id="d3284-127">Om en datatillgång ägs redan, kan inte andra användare bara bli ägare.</span><span class="sxs-lookup"><span data-stu-id="d3284-127">If a data asset is already owned, other users cannot simply take ownership.</span></span> <span data-ttu-id="d3284-128">De måste läggas till som delägare av en befintlig ägare.</span><span class="sxs-lookup"><span data-stu-id="d3284-128">They must be added as co-owners by an existing owner.</span></span> <span data-ttu-id="d3284-129">Alla ägare kan lägga till fler användare eller säkerhetsgrupper som delägare.</span><span class="sxs-lookup"><span data-stu-id="d3284-129">Any owner can add additional users or security groups as co-owners.</span></span>

> [!NOTE]
> <span data-ttu-id="d3284-130">Det är bäst att ha minst två personer som ägare för alla företagsägda datatillgångar.</span><span class="sxs-lookup"><span data-stu-id="d3284-130">It is a best practice to have at least two individuals as owners for any owned data asset.</span></span>
>
>

### <a name="remove-owners"></a><span data-ttu-id="d3284-131">Ta bort ägare</span><span class="sxs-lookup"><span data-stu-id="d3284-131">Remove owners</span></span>
<span data-ttu-id="d3284-132">Precis som alla tillgångens ägare kan lägga till delägare måste alla tillgångens ägare kan ta bort alla Medägare.</span><span class="sxs-lookup"><span data-stu-id="d3284-132">Just as any asset owner can add co-owners, any asset owner can remove any co-owner.</span></span>

<span data-ttu-id="d3284-133">En tillgångsägare som tar bort honom eller sig själv som ägare kan inte längre hantera tillgången.</span><span class="sxs-lookup"><span data-stu-id="d3284-133">An asset owner who removes him or herself as an owner can no longer manage the asset.</span></span> <span data-ttu-id="d3284-134">Om tillgångens ägare tar bort honom eller sig själv som en ägare och det finns inga andra delägare, återgår tillgången till oägd status.</span><span class="sxs-lookup"><span data-stu-id="d3284-134">If the asset owner removes him or herself as an owner and there are no other co-owners, the asset reverts to an unowned state.</span></span>

## <a name="control-visibility"></a><span data-ttu-id="d3284-135">Kontrollen synlighet</span><span class="sxs-lookup"><span data-stu-id="d3284-135">Control visibility</span></span>
<span data-ttu-id="d3284-136">Datatillgång ägare kan styra visningen av datatillgångar som de äger.</span><span class="sxs-lookup"><span data-stu-id="d3284-136">Data-asset owners can control the visibility of the data assets they own.</span></span> <span data-ttu-id="d3284-137">Om du vill begränsa synligheten som standard, där alla Data Catalog-användare kan identifiera och visa data tillgången, tillgångens ägare kan växla mellan inställningar från **alla** till **ägare och dessa användare** i Egenskaper för tillgången.</span><span class="sxs-lookup"><span data-stu-id="d3284-137">To restrict visibility as the default, where all Data Catalog users can discover and view the data asset, the asset owner can toggle the visibility setting from **Everyone** to **Owners & These Users** in the properties for the asset.</span></span> <span data-ttu-id="d3284-138">Ägare kan sedan lägga till specifika användare och säkerhetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="d3284-138">Owners can then add specific users and security groups.</span></span>

> [!NOTE]
> <span data-ttu-id="d3284-139">När det är möjligt ska tillgången ägarskap och synlighet behörigheter tilldelas till säkerhetsgrupper och inte till enskilda användare.</span><span class="sxs-lookup"><span data-stu-id="d3284-139">Whenever possible, asset ownership and visibility permissions should be assigned to security groups and not to individual users.</span></span>
>
>

## <a name="catalog-administrators"></a><span data-ttu-id="d3284-140">Katalogadministratörer</span><span class="sxs-lookup"><span data-stu-id="d3284-140">Catalog administrators</span></span>
<span data-ttu-id="d3284-141">Data Catalog administratörer är implicit delägare av alla tillgångar i katalogen.</span><span class="sxs-lookup"><span data-stu-id="d3284-141">Data Catalog administrators are implicitly co-owners of all assets in the catalog.</span></span> <span data-ttu-id="d3284-142">Tillgångsinformation ägare kan inte ta bort synlighet från administratörer och administratörer kan hantera ägarskap och synlighet för alla datatillgångar i katalogen.</span><span class="sxs-lookup"><span data-stu-id="d3284-142">Asset owners cannot remove visibility from administrators, and administrators can manage ownership and visibility for all data assets in the catalog.</span></span>

## <a name="summary"></a><span data-ttu-id="d3284-143">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="d3284-143">Summary</span></span>
<span data-ttu-id="d3284-144">Data Catalog gemensam modell för metadata och data tillgången identifiering kan alla katalogens användare bidra och identifiera.</span><span class="sxs-lookup"><span data-stu-id="d3284-144">The Data Catalog crowdsourcing model to metadata and data asset discovery allows all catalog users to contribute and discover.</span></span> <span data-ttu-id="d3284-145">I Standard Edition av Data Catalog är utformad för ägare och hantering att begränsa synlighet och användning av specifika datatillgångar.</span><span class="sxs-lookup"><span data-stu-id="d3284-145">The Standard Edition of Data Catalog is designed for ownership and management to limit the visibility and use of specific data assets.</span></span>

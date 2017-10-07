---
title: "aaaManage datatillgångar i Azure Data Catalog | Microsoft Docs"
description: "hello artikeln visar hur toocontrol synlighet och ägarskapet för datatillgångar som är registrerade i Azure Data Catalog."
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
ms.openlocfilehash: 48a634b92d7da19c32c9e551f295eec257f54f1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-data-assets-in-azure-data-catalog"></a><span data-ttu-id="686be-103">Hantera datatillgångar i Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="686be-103">Manage data assets in Azure Data Catalog</span></span>
## <a name="introduction"></a><span data-ttu-id="686be-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="686be-104">Introduction</span></span>
<span data-ttu-id="686be-105">Azure Data Catalog är utformad för identifiering av datakällan, så att du lätt kan identifiera och förstå hello datakällor du behöver tooperform analyser och fatta beslut.</span><span class="sxs-lookup"><span data-stu-id="686be-105">Azure Data Catalog is designed for data-source discovery, so that you can easily discover and understand hello data sources you need tooperform analysis and make decisions.</span></span> <span data-ttu-id="686be-106">Dessa funktioner för identifiering av gör hello största effekten när du och andra användare kan hitta och förstå hello mycket stort antal typer av datakällor.</span><span class="sxs-lookup"><span data-stu-id="686be-106">These discovery capabilities make hello biggest impact when you and other users can find and understand hello broadest range of available data sources.</span></span> <span data-ttu-id="686be-107">Med de här elementen i åtanke är hello standardbeteendet för Data Catalog för alla registrerade datakällor toobe synliga tooand kan upptäckas av alla användare i katalogen.</span><span class="sxs-lookup"><span data-stu-id="686be-107">With these elements in mind, hello default behavior of Data Catalog is for all registered data sources toobe visible tooand discoverable by all catalog users.</span></span>

<span data-ttu-id="686be-108">Data Catalog inte ge dig åtkomst till data i toohello sig själv.</span><span class="sxs-lookup"><span data-stu-id="686be-108">Data Catalog does not give you access toohello data itself.</span></span> <span data-ttu-id="686be-109">Dataåtkomst styrs av hello ägare hello-datakälla.</span><span class="sxs-lookup"><span data-stu-id="686be-109">Data access is controlled by hello owner of hello data source.</span></span> <span data-ttu-id="686be-110">Du kan identifiera datakällor och visa hello metadata som är relaterade toohello källor som är registrerade i hello-katalogen med Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="686be-110">With Data Catalog, you can discover data sources and view hello metadata that's related toohello sources that are registered in hello catalog.</span></span>

<span data-ttu-id="686be-111">Det kan finnas situationer, men där datakällor ska bara synliga toospecific användare eller toomembers för specifika grupper.</span><span class="sxs-lookup"><span data-stu-id="686be-111">There might be situations, however, where data sources should only be visible toospecific users, or toomembers of specific groups.</span></span> <span data-ttu-id="686be-112">I sådana fall kan användare bli ägare till registrerade datatillgångar hello Catalog och sedan kontrollera hello visning av hello tillgångar som de äger.</span><span class="sxs-lookup"><span data-stu-id="686be-112">In such scenarios, users can take ownership of registered data assets within hello catalog and then control hello visibility of hello assets they own.</span></span>

> [!NOTE]
> <span data-ttu-id="686be-113">hello-funktioner som beskrivs i den här artikeln är endast tillgänglig i hello Standard Edition av Azure Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="686be-113">hello functionality described in this article is available only in hello Standard Edition of Azure Data Catalog.</span></span> <span data-ttu-id="686be-114">hello ger Free Edition inte några funktioner för ägare och begränsa datatillgång synlighet.</span><span class="sxs-lookup"><span data-stu-id="686be-114">hello Free Edition does not provide capabilities for ownership and restricting data-asset visibility.</span></span>
>
>

## <a name="manage-ownership-of-data-assets"></a><span data-ttu-id="686be-115">Hantera ägarskapet för datatillgångar</span><span class="sxs-lookup"><span data-stu-id="686be-115">Manage ownership of data assets</span></span>
<span data-ttu-id="686be-116">Som standard är datatillgångar som är registrerade i Data Catalog oägd.</span><span class="sxs-lookup"><span data-stu-id="686be-116">By default, data assets that are registered in Data Catalog are unowned.</span></span> <span data-ttu-id="686be-117">Alla användare med behörighet tooaccess hello katalog kan identifiera och kommentera dessa tillgångar.</span><span class="sxs-lookup"><span data-stu-id="686be-117">Any user with permission tooaccess hello catalog can discover and annotate these assets.</span></span> <span data-ttu-id="686be-118">Användare kan bli ägare till oägd datatillgångar och begränsar hello synligheten för hello tillgångar som de äger.</span><span class="sxs-lookup"><span data-stu-id="686be-118">Users can take ownership of unowned data assets and then limit hello visibility of hello assets they own.</span></span>

<span data-ttu-id="686be-119">När en datatillgång i Data Catalog ägs endast användare som är godkända av hello ägare kan identifiera hello tillgången och visa dess metadata och endast hello ägare kan ta bort hello tillgång hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="686be-119">When a data asset in Data Catalog is owned, only users who are authorized by hello owners can discover hello asset and view its metadata, and only hello owners can delete hello asset from hello catalog.</span></span>

> [!NOTE]
> <span data-ttu-id="686be-120">Ägarskap i Data Catalog påverkar endast hello metadata som lagras i hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="686be-120">Ownership in Data Catalog affects only hello metadata that's stored in hello catalog.</span></span> <span data-ttu-id="686be-121">Ägarskap ger inte några behörigheter på hello underliggande data.</span><span class="sxs-lookup"><span data-stu-id="686be-121">Ownership does not confer any permissions on hello underlying data source.</span></span>
>
>

### <a name="take-ownership"></a><span data-ttu-id="686be-122">Bli ägare</span><span class="sxs-lookup"><span data-stu-id="686be-122">Take ownership</span></span>
<span data-ttu-id="686be-123">Användare kan ta över ägarskapet för datatillgångar genom att välja hello **bli ägare** alternativ i hello Data Catalog-portalen.</span><span class="sxs-lookup"><span data-stu-id="686be-123">Users can take ownership of data assets by selecting hello **Take Ownership** option in hello Data Catalog portal.</span></span> <span data-ttu-id="686be-124">Inga särskilda behörigheter är obligatoriska tootake ägarskap för en datatillgång oägd.</span><span class="sxs-lookup"><span data-stu-id="686be-124">No special permissions are required tootake ownership of an unowned data asset.</span></span> <span data-ttu-id="686be-125">Alla användare kan bli ägare för en datatillgång oägd.</span><span class="sxs-lookup"><span data-stu-id="686be-125">Any user can take ownership of an unowned data asset.</span></span>

### <a name="add-owners-and-co-owners"></a><span data-ttu-id="686be-126">Lägg till ägare och Medägare</span><span class="sxs-lookup"><span data-stu-id="686be-126">Add owners and co-owners</span></span>
<span data-ttu-id="686be-127">Om en datatillgång ägs redan, kan inte andra användare bara bli ägare.</span><span class="sxs-lookup"><span data-stu-id="686be-127">If a data asset is already owned, other users cannot simply take ownership.</span></span> <span data-ttu-id="686be-128">De måste läggas till som delägare av en befintlig ägare.</span><span class="sxs-lookup"><span data-stu-id="686be-128">They must be added as co-owners by an existing owner.</span></span> <span data-ttu-id="686be-129">Alla ägare kan lägga till fler användare eller säkerhetsgrupper som delägare.</span><span class="sxs-lookup"><span data-stu-id="686be-129">Any owner can add additional users or security groups as co-owners.</span></span>

> [!NOTE]
> <span data-ttu-id="686be-130">Det är en bästa praxis toohave minst två personer som ägare för någon ägs datatillgång.</span><span class="sxs-lookup"><span data-stu-id="686be-130">It is a best practice toohave at least two individuals as owners for any owned data asset.</span></span>
>
>

### <a name="remove-owners"></a><span data-ttu-id="686be-131">Ta bort ägare</span><span class="sxs-lookup"><span data-stu-id="686be-131">Remove owners</span></span>
<span data-ttu-id="686be-132">Precis som alla tillgångens ägare kan lägga till delägare måste alla tillgångens ägare kan ta bort alla Medägare.</span><span class="sxs-lookup"><span data-stu-id="686be-132">Just as any asset owner can add co-owners, any asset owner can remove any co-owner.</span></span>

<span data-ttu-id="686be-133">En tillgångsägare som tar bort honom eller sig själv som ägare kan inte längre hantera hello tillgången.</span><span class="sxs-lookup"><span data-stu-id="686be-133">An asset owner who removes him or herself as an owner can no longer manage hello asset.</span></span> <span data-ttu-id="686be-134">Om hello tillgångens ägare tar bort honom eller sig själv som en ägare och det finns inga andra delägare, återgår hello tillgången tooan Ej ägd tillstånd.</span><span class="sxs-lookup"><span data-stu-id="686be-134">If hello asset owner removes him or herself as an owner and there are no other co-owners, hello asset reverts tooan unowned state.</span></span>

## <a name="control-visibility"></a><span data-ttu-id="686be-135">Kontrollen synlighet</span><span class="sxs-lookup"><span data-stu-id="686be-135">Control visibility</span></span>
<span data-ttu-id="686be-136">Datatillgång ägare kan styra hello synlighet för hello datatillgångar som de äger.</span><span class="sxs-lookup"><span data-stu-id="686be-136">Data-asset owners can control hello visibility of hello data assets they own.</span></span> <span data-ttu-id="686be-137">toorestrict synlighet som hello standard, där alla Data Catalog användare kan identifiera och visa hello datatillgång, hello tillgångens ägare kan växla hello inställningar från **alla** för**ägare och dessa användare** i hello egenskaper för hello tillgång.</span><span class="sxs-lookup"><span data-stu-id="686be-137">toorestrict visibility as hello default, where all Data Catalog users can discover and view hello data asset, hello asset owner can toggle hello visibility setting from **Everyone** too**Owners & These Users** in hello properties for hello asset.</span></span> <span data-ttu-id="686be-138">Ägare kan sedan lägga till specifika användare och säkerhetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="686be-138">Owners can then add specific users and security groups.</span></span>

> [!NOTE]
> <span data-ttu-id="686be-139">När det är möjligt ska tillgången ägarskap och synlighet behörigheter tilldelas toosecurity grupper och inte tooindividual användare.</span><span class="sxs-lookup"><span data-stu-id="686be-139">Whenever possible, asset ownership and visibility permissions should be assigned toosecurity groups and not tooindividual users.</span></span>
>
>

## <a name="catalog-administrators"></a><span data-ttu-id="686be-140">Katalogadministratörer</span><span class="sxs-lookup"><span data-stu-id="686be-140">Catalog administrators</span></span>
<span data-ttu-id="686be-141">Data Catalog administratörer är implicit delägare av alla tillgångar i hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="686be-141">Data Catalog administrators are implicitly co-owners of all assets in hello catalog.</span></span> <span data-ttu-id="686be-142">Tillgångsinformation ägare kan inte ta bort synlighet från administratörer och administratörer kan hantera ägarskap och synlighet för alla datatillgångar i hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="686be-142">Asset owners cannot remove visibility from administrators, and administrators can manage ownership and visibility for all data assets in hello catalog.</span></span>

## <a name="summary"></a><span data-ttu-id="686be-143">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="686be-143">Summary</span></span>
<span data-ttu-id="686be-144">hello Data Catalog gemensamt skapade modellen toometadata och data tillgången discovery gör att alla toocontribute för användare av katalogen och identifiera.</span><span class="sxs-lookup"><span data-stu-id="686be-144">hello Data Catalog crowdsourcing model toometadata and data asset discovery allows all catalog users toocontribute and discover.</span></span> <span data-ttu-id="686be-145">hello Standard Edition av Data Catalog är utformad för ägare och ledning toolimit hello synlighet och användning av specifika datatillgångar.</span><span class="sxs-lookup"><span data-stu-id="686be-145">hello Standard Edition of Data Catalog is designed for ownership and management toolimit hello visibility and use of specific data assets.</span></span>

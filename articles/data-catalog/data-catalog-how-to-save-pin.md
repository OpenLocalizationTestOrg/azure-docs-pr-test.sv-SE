---
title: "aaaSave sökningar och PIN-kod datatillgångar i Azure Data Catalog | Microsoft Docs"
description: "Hur tooarticle färgmarkera funktioner i Azure Data Catalog för att spara datakällor och datatillgångar för senare användning."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 6bd00a81-820d-4b7c-91fa-ab09e575474c
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 0ad0a31d4b84782fed9d80acc2734912eecd6d74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="save-searches-and-pin-data-assets-in-azure-data-catalog"></a><span data-ttu-id="6c922-103">Spara sökningar och PIN-kod datatillgångar i Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="6c922-103">Save searches and pin data assets in Azure Data Catalog</span></span>
## <a name="introduction"></a><span data-ttu-id="6c922-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="6c922-104">Introduction</span></span>
<span data-ttu-id="6c922-105">Azure Data Catalog innehåller funktioner för upptäckt av datakälla.</span><span class="sxs-lookup"><span data-stu-id="6c922-105">Azure Data Catalog provides capabilities for data source discovery.</span></span> <span data-ttu-id="6c922-106">Du kan snabbt söka och filtrera hello katalogen toolocate datakällor och förstå deras syfte, vilket gör det enklare toofind hello rätt data för hello jobbet till hands.</span><span class="sxs-lookup"><span data-stu-id="6c922-106">You can quickly search and filter hello catalog toolocate data sources and understand their intended purpose, making it easier toofind hello right data for hello job at hand.</span></span>

<span data-ttu-id="6c922-107">Men vad händer om du behöver tooregularly arbeta med hello samma data?</span><span class="sxs-lookup"><span data-stu-id="6c922-107">But what if you need tooregularly work with hello same data?</span></span> <span data-ttu-id="6c922-108">Och vad händer om du och andra användare regelbundet bidra knowledge-toohello samma datakällor i hello katalogen?</span><span class="sxs-lookup"><span data-stu-id="6c922-108">And what if you and other users regularly contribute your knowledge toohello same data sources in hello catalog?</span></span> <span data-ttu-id="6c922-109">I sådana situationer med toorepeatedly problemet hello samma sökningar kan vara ineffektiv.</span><span class="sxs-lookup"><span data-stu-id="6c922-109">In these situations, having toorepeatedly issue hello same searches can be inefficient.</span></span> <span data-ttu-id="6c922-110">Detta är där med hjälp av sparad sökning och Fäst datatillgångar.</span><span class="sxs-lookup"><span data-stu-id="6c922-110">This is where saved search and pinned data assets can help.</span></span>

## <a name="saved-searches"></a><span data-ttu-id="6c922-111">Sparade sökningar</span><span class="sxs-lookup"><span data-stu-id="6c922-111">Saved searches</span></span>
<span data-ttu-id="6c922-112">En sparad sökning i Data Catalog är en återanvändbar per användare Sök definition.</span><span class="sxs-lookup"><span data-stu-id="6c922-112">A saved search in Data Catalog is a reusable, per-user search definition.</span></span> <span data-ttu-id="6c922-113">Du kan definiera en sökning, till exempel sökvillkor, taggar och andra filter och sparar den.</span><span class="sxs-lookup"><span data-stu-id="6c922-113">You can define a search, including search terms, tags, and other filters, and then save it.</span></span> <span data-ttu-id="6c922-114">Du kan köra hello sparade sökningen definition igen senare tooreturn alla datatillgångar som matchar sökvillkoren.</span><span class="sxs-lookup"><span data-stu-id="6c922-114">You can re-run hello saved search definition later tooreturn any data assets that match its search criteria.</span></span>

### <a name="create-a-saved-search"></a><span data-ttu-id="6c922-115">Skapa en sparad sökning</span><span class="sxs-lookup"><span data-stu-id="6c922-115">Create a saved search</span></span>
<span data-ttu-id="6c922-116">toocreate en sparad sökning hello följande:</span><span class="sxs-lookup"><span data-stu-id="6c922-116">toocreate a saved search, do hello following:</span></span>
1. <span data-ttu-id="6c922-117">I hello Azure Data Catalog-portalen i hello **aktuella sökningen** -fönstret klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="6c922-117">In hello Azure Data Catalog portal, in hello **Current Search** window, click **Save**.</span></span> 

    ![Aktuella Sök inställningar spara länken](./media/data-catalog-how-to-save-pin/01-save-option.png) 

2. <span data-ttu-id="6c922-119">Ange hello sökkriterier du vill tooreuse och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="6c922-119">Enter hello search criteria that you want tooreuse, and then click **Save**.</span></span>

    ![Aktuella sökinställningar sparade söknamn](./media/data-catalog-how-to-save-pin/02-name.png)

3. <span data-ttu-id="6c922-121">När du uppmanas ange ett namn för hello sparad sökning.</span><span class="sxs-lookup"><span data-stu-id="6c922-121">When you are prompted, enter a name for hello saved search.</span></span> <span data-ttu-id="6c922-122">Välj ett beskrivande namn och som beskriver hello datatillgångar som returneras av hello sökning.</span><span class="sxs-lookup"><span data-stu-id="6c922-122">Pick a name that is meaningful and that describes hello data assets that will be returned by hello search.</span></span>

### <a name="manage-saved-searches"></a><span data-ttu-id="6c922-123">Hantera sparade sökningar</span><span class="sxs-lookup"><span data-stu-id="6c922-123">Manage saved searches</span></span>
<span data-ttu-id="6c922-124">När du har sparat en eller flera sökningar, en **sparade sökningar** alternativet visas under hello **aktuella sökningen** rutan.</span><span class="sxs-lookup"><span data-stu-id="6c922-124">After you have saved one or more searches, a **Saved Searches** option is displayed beneath hello **Current Search** box.</span></span> <span data-ttu-id="6c922-125">När hello listan är expanderad visas alla sparade sökningar.</span><span class="sxs-lookup"><span data-stu-id="6c922-125">When hello list is expanded, all saved searches are displayed.</span></span>

 ![Listan över sparade sökningar](./media/data-catalog-how-to-save-pin/03-list.png)

<span data-ttu-id="6c922-127">Gör något av följande hello:</span><span class="sxs-lookup"><span data-stu-id="6c922-127">Do any of hello following:</span></span>

* <span data-ttu-id="6c922-128">tooexecute en sökning, markera en sparad sökning hello.</span><span class="sxs-lookup"><span data-stu-id="6c922-128">tooexecute a search, select a saved search in hello list.</span></span>

* <span data-ttu-id="6c922-129">tooview en lista med alternativ för en sparad sökning klickar du på hello ned pilen Nästa toohello Söknamn.</span><span class="sxs-lookup"><span data-stu-id="6c922-129">tooview a list of management options for a saved search, click hello down arrow next toohello search name.</span></span>

    ![Alternativ för att hantera sparade sökningar](./media/data-catalog-how-to-save-pin/04-managing.png)

* <span data-ttu-id="6c922-131">Välj tooenter ett nytt namn för hello sparade sökningen **Byt namn på**.</span><span class="sxs-lookup"><span data-stu-id="6c922-131">tooenter a new name for hello saved search, select **Rename**.</span></span> <span data-ttu-id="6c922-132">hello Sök definition ändras inte.</span><span class="sxs-lookup"><span data-stu-id="6c922-132">hello search definition is not changed.</span></span>

* <span data-ttu-id="6c922-133">tooremove hello sparade sökningen från listan, Välj **ta bort**, och sedan bekräfta hello borttagning.</span><span class="sxs-lookup"><span data-stu-id="6c922-133">tooremove hello saved search from your list, select **Delete**, and then confirm hello deletion.</span></span>

* <span data-ttu-id="6c922-134">toomark hello sparade sökningen som standardsökning, Välj **sparar som standard**.</span><span class="sxs-lookup"><span data-stu-id="6c922-134">toomark hello saved search as your default search, select **Save As Default**.</span></span> <span data-ttu-id="6c922-135">Om du utför en ”tom” sökning från startsidan för hello Azure Data Catalog körs standardsökningen.</span><span class="sxs-lookup"><span data-stu-id="6c922-135">If you perform an “empty” search from hello Azure Data Catalog home page, your default search is executed.</span></span> <span data-ttu-id="6c922-136">Dessutom hello som har markerats som hello standardsökningen visas överst hello i hello **sparade sökningar** lista.</span><span class="sxs-lookup"><span data-stu-id="6c922-136">In addition, hello search that's marked as hello default search is displayed at hello top of hello **Saved Searches** list.</span></span>

### <a name="organizational-saved-searches"></a><span data-ttu-id="6c922-137">Organisationens sparade sökningar</span><span class="sxs-lookup"><span data-stu-id="6c922-137">Organizational saved searches</span></span>
<span data-ttu-id="6c922-138">Alla användare i din organisation kan spara sökningar för eget bruk.</span><span class="sxs-lookup"><span data-stu-id="6c922-138">All user in your organization can save searches for their own use.</span></span> <span data-ttu-id="6c922-139">Data Catalog administratörer kan också spara sökningar för alla användare inom hello organisation.</span><span class="sxs-lookup"><span data-stu-id="6c922-139">Data Catalog administrators can also save searches for all users within hello organization.</span></span> <span data-ttu-id="6c922-140">När administratörer spara en sökning, de kan välja mellan en **resurs inom hello företag** alternativet.</span><span class="sxs-lookup"><span data-stu-id="6c922-140">When administrators save a search, they're presented with a **Share within hello company** option.</span></span> <span data-ttu-id="6c922-141">Om du väljer det här alternativet resurser hello sparad sökning för alla användare i hello organisation.</span><span class="sxs-lookup"><span data-stu-id="6c922-141">Selecting this option shares hello saved search for all users in hello organization.</span></span>

 ![Organisationens sparade sökningar](./media/data-catalog-how-to-save-pin/08-organizational-saved-search.png)

## <a name="pinned-data-assets"></a><span data-ttu-id="6c922-143">Fäst datatillgångar</span><span class="sxs-lookup"><span data-stu-id="6c922-143">Pinned data assets</span></span>
<span data-ttu-id="6c922-144">Du kan spara och återanvända Sök definitioner med sparade sökningar.</span><span class="sxs-lookup"><span data-stu-id="6c922-144">With saved searches, you can save and reuse search definitions.</span></span> <span data-ttu-id="6c922-145">Hej datatillgångar som returneras av hello sökningar kan ändras med tiden som hello hello katalogen ändras.</span><span class="sxs-lookup"><span data-stu-id="6c922-145">hello data assets that are returned by hello searches might change over time as hello contents of hello catalog change.</span></span> <span data-ttu-id="6c922-146">När du fäster datatillgångar du explicit kan identifiera specifika data tillgångar toomake dem lättare tooaccess utan att behöva toouse en sökning.</span><span class="sxs-lookup"><span data-stu-id="6c922-146">When you pin data assets, you can explicitly identify specific data assets toomake them easier tooaccess without needing toouse a search.</span></span>

<span data-ttu-id="6c922-147">Det är enkelt att fästa en datatillgång.</span><span class="sxs-lookup"><span data-stu-id="6c922-147">Pinning a data asset is straightforward.</span></span> <span data-ttu-id="6c922-148">tooadd hello data tooyour Fäst lista med tillgångsinformation, klickar du på hello **PIN-kod** ikon.</span><span class="sxs-lookup"><span data-stu-id="6c922-148">tooadd hello data asset tooyour pinned list, you simply click hello **pin** icon.</span></span> <span data-ttu-id="6c922-149">hello ikon för hello hörn hello tillgången panelen i vyn sida vid sida för hello och i hello vänstra kolumnen i hello listvyn i hello Azure Data Catalog-portalen.</span><span class="sxs-lookup"><span data-stu-id="6c922-149">hello icon is displayed in hello corner of hello asset tile in hello tile view, and in hello left-most column in hello list view in hello Azure Data Catalog portal.</span></span>

![ikon för hello datatillgång PIN-kod](./media/data-catalog-how-to-save-pin/05-pinning.png)

<span data-ttu-id="6c922-151">Det är lika enkelt att ta bort en datatillgång.</span><span class="sxs-lookup"><span data-stu-id="6c922-151">Unpinning a data asset is equally straightforward.</span></span> <span data-ttu-id="6c922-152">Klicka bara på hello **ner** ikonen tootoggle hello inställning för hello markerade tillgången.</span><span class="sxs-lookup"><span data-stu-id="6c922-152">Simply click hello **unpin** icon tootoggle hello setting for hello selected asset.</span></span>

![hello datatillgång ner ikon](./media/data-catalog-how-to-save-pin/06-unpinning.png)

## <a name="hello-my-assets-section"></a><span data-ttu-id="6c922-154">hello avsnittet Mina tillgångar</span><span class="sxs-lookup"><span data-stu-id="6c922-154">hello My Assets section</span></span>
<span data-ttu-id="6c922-155">hello Data Catalog portalens startsida innehåller en **Mina tillgångar** avsnitt som visar tillgångar intresse toohello aktuella användarens.</span><span class="sxs-lookup"><span data-stu-id="6c922-155">hello Data Catalog portal home page includes a **My Assets** section that displays assets of interest toohello current user.</span></span> <span data-ttu-id="6c922-156">Det här avsnittet innehåller både fasta tillgångar och sparade sökningar.</span><span class="sxs-lookup"><span data-stu-id="6c922-156">This section includes both pinned assets and saved searches.</span></span>

![hello Mina tillgångar avsnitt på hello startsida](./media/data-catalog-how-to-save-pin/07-my-assets.png)

## <a name="summary"></a><span data-ttu-id="6c922-158">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="6c922-158">Summary</span></span>
<span data-ttu-id="6c922-159">Azure Data Catalog innehåller funktioner som gör det enklare toodiscover hello datakällor som du behöver, så att du och andra organisationsmedlemmar i kan ägna mindre tid som söker efter data och mer tid arbeta med den.</span><span class="sxs-lookup"><span data-stu-id="6c922-159">Azure Data Catalog provides capabilities that make it easier toodiscover hello data sources you need, so you and other organization members can spend less time looking for data and more time working with it.</span></span> <span data-ttu-id="6c922-160">Sparade sökningar och Fäst data tillgångar som bygger på dessa kärnfunktioner så att användarna enkelt kan identifiera datakällor som de arbetar med flera gånger.</span><span class="sxs-lookup"><span data-stu-id="6c922-160">Saved searches and pinned data assets build on these core capabilities so users can easily identify data sources that they work with repeatedly.</span></span>

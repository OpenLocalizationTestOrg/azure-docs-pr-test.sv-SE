---
title: aaaCreate och dela instrumentpaneler med Azure portal | Microsoft Docs
description: "Den här artikeln förklarar hur toocreate och redigera instrumentpaneler i hello Azure-portalen."
services: azure-portal
documentationcenter: 
author: sewatson
manager: timlt
editor: tysonn
ms.assetid: ff422f36-47d2-409b-8a19-02e24b03ffe7
ms.service: multiple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 09/06/2016
ms.author: sewatson
ms.openlocfilehash: 0facd10fe3284d7ad9a9e29791e5af5b5b95c97f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-share-dashboards-in-hello-azure-portal"></a><span data-ttu-id="ab8ec-103">Skapa och dela instrumentpaneler i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ab8ec-103">Create and share dashboards in hello Azure portal</span></span>
<span data-ttu-id="ab8ec-104">Du kan skapa flera instrumentpaneler och dela dem med andra användare har åtkomst tooyour Azure prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-104">You can create multiple dashboards and share them with others who have access tooyour Azure subscriptions.</span></span>  <span data-ttu-id="ab8ec-105">Den här artikeln går igenom hello grunderna för att skapa, redigera, publicera och hantera åtkomst toodashboards.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-105">This article goes through hello basics of creating, editing, publishing, and managing access toodashboards.</span></span>

## <a name="create-a-dashboard"></a><span data-ttu-id="ab8ec-106">Skapa en instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="ab8ec-106">Create a dashboard</span></span>
<span data-ttu-id="ab8ec-107">toocreate en instrumentpanel, Välj hello **ny instrumentpanel** knappen Nästa toohello aktuella instrumentpanelens namn.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-107">toocreate a dashboard, select hello **New dashboard** button next toohello current dashboard's name.</span></span>  

![Skapa instrumentpanel](./media/azure-portal-dashboards/new-dashboard.png)

<span data-ttu-id="ab8ec-109">Den här åtgärden skapar en ny, tom, privata instrumentpanel och placerar du i anpassning läge där du kan kalla instrumentpanelen och lägga till eller ordna om paneler.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-109">This action creates a new, empty, private dashboard and puts you into customization mode where you can name your dashboard and add or rearrange tiles.</span></span>  <span data-ttu-id="ab8ec-110">I detta läge panelen hello döljas galleriet tar över hello vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-110">When in this mode, hello collapsible tile gallery takes over hello left navigation menu.</span></span>  <span data-ttu-id="ab8ec-111">hello panelen galleriet kan du hitta paneler för Azure-resurser på olika sätt: du kan bläddra genom [resursgruppen](../azure-resource-manager/resource-group-overview.md#resource-groups), av resurstyp, av [taggen](../azure-resource-manager/resource-group-using-tags.md), eller genom att söka efter resurs med namnet.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-111">hello tile gallery lets you find tiles for your Azure resources in various ways: you can browse by [resource group](../azure-resource-manager/resource-group-overview.md#resource-groups), by resource type, by [tag](../azure-resource-manager/resource-group-using-tags.md), or by searching for your resource by name.</span></span>  

![Anpassa instrumentpanel](./media/azure-portal-dashboards/customize-dashboard.png)

<span data-ttu-id="ab8ec-113">Lägg till paneler genom att dra och släppa dem i hello instrumentpanelen yta var du vill.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-113">Add tiles by dragging and dropping them onto hello dashboard surface wherever you want.</span></span>

<span data-ttu-id="ab8ec-114">Det finns en ny kategori som heter **allmänna** för brickor som inte är associerade med en viss resurs.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-114">There's a new category called **General** for tiles that are not associated with a particular resource.</span></span>  <span data-ttu-id="ab8ec-115">I det här exemplet Fäst vi hello Markdown sida vid sida.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-115">In this example, we pin hello Markdown tile.</span></span>  <span data-ttu-id="ab8ec-116">Du kan använda den här panelen tooadd anpassade innehåll tooyour instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-116">You use this tile tooadd custom content tooyour dashboard.</span></span>  <span data-ttu-id="ab8ec-117">hello panelen stöder oformaterad text [markdownsyntax](https://daringfireball.net/projects/markdown/syntax), och en begränsad uppsättning HTML.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-117">hello tile supports plain text, [Markdown syntax](https://daringfireball.net/projects/markdown/syntax), and a limited set of HTML.</span></span>  <span data-ttu-id="ab8ec-118">(För säkerhet, kan du göra saker som mata in `<script>` taggar eller använda vissa formatmalls-element i CSS som kan störa hello-portalen.)</span><span class="sxs-lookup"><span data-stu-id="ab8ec-118">(For safety, you can't do things like inject `<script>` tags or use certain styling element of CSS that might interfere with hello portal.)</span></span> 

![Lägg till markdown](./media/azure-portal-dashboards/add-markdown.png)

## <a name="edit-a-dashboard"></a><span data-ttu-id="ab8ec-120">Redigera en instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="ab8ec-120">Edit a dashboard</span></span>
<span data-ttu-id="ab8ec-121">Du kan fästa paneler i hello panelen gallery eller hello panelen representation av blad när du har skapat din instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-121">After creating your dashboard, you can pin tiles from hello tile gallery or hello tile representation of blades.</span></span> <span data-ttu-id="ab8ec-122">Vi Fäst hello representation av våra resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-122">Let's pin hello representation of our resource group.</span></span> <span data-ttu-id="ab8ec-123">Du kan antingen fästa när du bläddrar hello objektet, eller från hello blad för resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-123">You can either pin when browsing hello item, or from hello resource group blade.</span></span> <span data-ttu-id="ab8ec-124">Båda metoderna resultera i att fästa hello panelen representation av hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-124">Both approaches result in pinning hello tile representation of hello resource group.</span></span>

![PIN-kod toodashboard](./media/azure-portal-dashboards/pin-to-dashboard.png)

<span data-ttu-id="ab8ec-126">Efter att fästa hello objekt, visas det på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-126">After pinning hello item, it appears on your dashboard.</span></span>

![visa instrumentpanelen](./media/azure-portal-dashboards/view-dashboard.png)

<span data-ttu-id="ab8ec-128">Nu när vi har en Markdown-panelen och en resurs grupp Fäst toohello instrumentpanel kan vi ändra storlek på och flytta hello paneler i en lämplig layout.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-128">Now that we have a Markdown tile and a resource group pinned toohello dashboard, we can resize and rearrange hello tiles into a suitable layout.</span></span>

<span data-ttu-id="ab8ec-129">Du kan se alla hello kontextuella kommandon för att sida vid sida av hovra och välja ”...” eller högerklicka på en panel.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-129">By hovering and selecting "…" or right-clicking on a tile you can see all hello contextual commands for that tile.</span></span> <span data-ttu-id="ab8ec-130">Det finns två objekt som standard:</span><span class="sxs-lookup"><span data-stu-id="ab8ec-130">By default, there are two items:</span></span>

1. <span data-ttu-id="ab8ec-131">**Ta bort från instrumentpanelen** – tar bort hello panelen hello instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="ab8ec-131">**Unpin from dashboard** – removes hello tile from hello dashboard</span></span>
2. <span data-ttu-id="ab8ec-132">**Anpassa** – anger anpassa läge</span><span class="sxs-lookup"><span data-stu-id="ab8ec-132">**Customize** – enters customize mode</span></span>

![Anpassa sida vid sida](./media/azure-portal-dashboards/customize-tile.png)

<span data-ttu-id="ab8ec-134">Genom att välja Anpassa, kan du ändra storlek på och ordna om paneler.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-134">By selecting customize, you can resize and reorder tiles.</span></span> <span data-ttu-id="ab8ec-135">tooresize en panel, Välj hello nya storlek från hello snabbmenyn som visas i följande bild hello.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-135">tooresize a tile, select hello new size from hello contextual menu, as shown in hello following image.</span></span>

![Ändra storlek på panelen](./media/azure-portal-dashboards/resize-tile.png)

<span data-ttu-id="ab8ec-137">Eller, om hello panelen stöder alla storlekar, kan du dra hello nedre högra hörnet toohello önskad storlek.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-137">Or, if hello tile supports any size, you can drag hello bottom right-hand corner toohello desired size.</span></span>

![Ändra storlek på panelen](./media/azure-portal-dashboards/resize-corner.png)

<span data-ttu-id="ab8ec-139">När du ändrar storlek på panelerna, visa hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-139">After resizing tiles, view hello dashboard.</span></span>

![vyn sida vid sida](./media/azure-portal-dashboards/view-tile.png)

<span data-ttu-id="ab8ec-141">När du är klar med att anpassa en instrumentpanel, markerar du bara hello **klar anpassa** tooexit anpassningsläge eller högerklicka och välj **klar anpassa** hello snabbmenyn.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-141">Once you are finished customizing a dashboard, simply select hello **Done customizing** tooexit customize mode or right-click and select **Done customizing** from hello context menu.</span></span>

## <a name="publish-a-dashboard-and-manage-access-control"></a><span data-ttu-id="ab8ec-142">Publicera en instrumentpanel och hantera åtkomstkontrollen</span><span class="sxs-lookup"><span data-stu-id="ab8ec-142">Publish a dashboard and manage access control</span></span>
<span data-ttu-id="ab8ec-143">Den är privat som standard, vilket innebär att du är hello enda som kan se den när du skapar en instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-143">When you create a dashboard, it is private by default, which means you are hello only person who can see it.</span></span>  <span data-ttu-id="ab8ec-144">toomake den synliga tooothers använda hello **resursen** knappen bredvid hello andra kommandon i instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-144">toomake it visible tooothers, use hello **Share** button that appears alongside hello other dashboard commands.</span></span>

![dela instrumentpanelen](./media/azure-portal-dashboards/share-dashboard.png)

<span data-ttu-id="ab8ec-146">Du uppmanas toochoose en prenumeration och resurs grupp för din instrumentpanel toobe publiceras.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-146">You are asked toochoose a subscription and resource group for your dashboard toobe published to.</span></span> <span data-ttu-id="ab8ec-147">tooseamlessly integrera instrumentpaneler i hello-ekosystemet, vi har implementerat delade instrumentpaneler som Azure-resurser (så att du inte kan dela genom att skriva en e-postadress).</span><span class="sxs-lookup"><span data-stu-id="ab8ec-147">tooseamlessly integrate dashboards into hello ecosystem, we've implemented shared dashboards as Azure resources (so you can't share by typing an email address).</span></span>  <span data-ttu-id="ab8ec-148">Komma åt toohello information visas i de flesta hello paneler i hello portal styrs av [Azure rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="ab8ec-148">Access toohello information displayed by most of hello tiles in hello portal are governed by [Azure Role Based Access Control](../active-directory/role-based-access-control-configure.md).</span></span> <span data-ttu-id="ab8ec-149">Ur ett access control skiljer delade instrumentpaneler sig inte från en virtuell dator eller ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-149">From an access control perspective, shared dashboards are no different from a virtual machine or a storage account.</span></span>  

<span data-ttu-id="ab8ec-150">Anta att du har en Azure-prenumeration och gruppmedlemmarna har tilldelats hello roller i **ägare**, **deltagare**, eller **reader** hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-150">Let's say you have an Azure subscription and members of your team have been assigned hello roles of **owner**, **contributor**, or **reader** of hello subscription.</span></span>  <span data-ttu-id="ab8ec-151">Användare som är ägare eller deltagare är kan toolist, visa, skapa, ändra eller ta bort instrumentpaneler i den prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-151">Users who are owners or contributors are able toolist, view, create, modify, or delete dashboards within that subscription.</span></span>  <span data-ttu-id="ab8ec-152">Användare som är läsare kan inte är kan toolist och visa instrumentpaneler, men ändra eller ta bort dem.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-152">Users who are readers are able toolist and view dashboards, but cannot modify or delete them.</span></span>  <span data-ttu-id="ab8ec-153">Användare med åtkomst läsaren är kan toomake lokala redigeringar tooa delade instrumentpanelen, men är inte toopublish dessa ändringar tillbaka toohello server.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-153">Users with reader access are able toomake local edits tooa shared dashboard, but are not able toopublish those changes back toohello server.</span></span>  <span data-ttu-id="ab8ec-154">De kan dock göra en privat kopia av hello instrumentpanelen för eget bruk.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-154">However, they can make a private copy of hello dashboard for their own use.</span></span>  <span data-ttu-id="ab8ec-155">Enskilda paneler på hello instrumentpanelen framtvinga som alltid sina egna regler för åtkomstkontroll baserat på hello resurser som de motsvarar.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-155">As always, individual tiles on hello dashboard enforce their own access control rules based on hello resources they correspond to.</span></span>  

<span data-ttu-id="ab8ec-156">För enkelhetens skull hello-portalen publicering upplevelse guider du mot ett mönster som placerar du instrumentpaneler i en resursgrupp kallas **instrumentpaneler**.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-156">For convenience, hello portal's publishing experience guides you towards a pattern where you place dashboards in a resource group called **dashboards**.</span></span>  

![publicera instrumentpanelen](./media/azure-portal-dashboards/publish-dashboard.png)

<span data-ttu-id="ab8ec-158">Du kan också välja toopublish en instrumentpanel tooa viss resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-158">You can also choose toopublish a dashboard tooa particular resource group.</span></span>  <span data-ttu-id="ab8ec-159">hello åtkomstkontroll för instrumentpanelen matchar hello åtkomstkontroll för hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-159">hello access control for that dashboard matches hello access control for hello resource group.</span></span>  <span data-ttu-id="ab8ec-160">Användare som kan hantera hello resurser i resursgruppen har också åtkomst toohello instrumentpaneler.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-160">Users that can manage hello resources in that resource group also have access toohello dashboards.</span></span>

![publicera instrumentpanelen tooresource grupp](./media/azure-portal-dashboards/publish-to-resource-group.png)

<span data-ttu-id="ab8ec-162">När instrumentpanelen har publicerats hello **delning + åtkomst** kontrollen ska uppdatera och visa information om hello publicerade instrumentpanel, till exempel en länk toomanage åtkomst toohello instrumentpanel för användare.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-162">After your dashboard is published, hello **Sharing + access** control pane will refresh and show you information about hello published dashboard, including a link toomanage user access toohello dashboard.</span></span>  <span data-ttu-id="ab8ec-163">Den här länken startar hello standard rollbaserad åtkomstkontroll bladet används toomanage åtkomst för Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-163">This link launches hello standard Role Based Access Control blade used toomanage access for any Azure resource.</span></span>  <span data-ttu-id="ab8ec-164">Du kan alltid komma toothis vyn genom att välja **resursen**.</span><span class="sxs-lookup"><span data-stu-id="ab8ec-164">You can always get back toothis view by selecting **Share**.</span></span>

![Hantera åtkomstkontroll](./media/azure-portal-dashboards/manage-access.png)

## <a name="next-steps"></a><span data-ttu-id="ab8ec-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ab8ec-166">Next steps</span></span>
* <span data-ttu-id="ab8ec-167">toomanage resurser, se [hantera Azure-resurser via portalen](../azure-resource-manager/resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ab8ec-167">toomanage resources, see [Manage Azure resources through portal](../azure-resource-manager/resource-group-portal.md).</span></span>
* <span data-ttu-id="ab8ec-168">toodeploy resurser, se [distribuera resurser med Resource Manager-mallar och Azure-portalen](../azure-resource-manager/resource-group-template-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ab8ec-168">toodeploy resources, see [Deploy resources with Resource Manager templates and Azure portal](../azure-resource-manager/resource-group-template-deploy-portal.md).</span></span>


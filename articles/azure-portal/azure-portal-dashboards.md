---
title: Skapa och dela instrumentpaneler med Azure portal | Microsoft Docs
description: "Den här artikeln beskriver hur du skapar och redigerar instrumentpaneler i Azure-portalen."
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
ms.openlocfilehash: 5429e68723448ff5db6ef0ed8da1b927e97e6dd9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-and-share-dashboards-in-the-azure-portal"></a><span data-ttu-id="b414b-103">Skapa och dela instrumentpaneler i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b414b-103">Create and share dashboards in the Azure portal</span></span>
<span data-ttu-id="b414b-104">Du kan skapa flera instrumentpaneler och dela dem med andra användare har åtkomst till dina Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="b414b-104">You can create multiple dashboards and share them with others who have access to your Azure subscriptions.</span></span>  <span data-ttu-id="b414b-105">Den här artikeln går igenom grunderna i att skapa, redigera, publicera och hantera åtkomst till instrumentpaneler.</span><span class="sxs-lookup"><span data-stu-id="b414b-105">This article goes through the basics of creating, editing, publishing, and managing access to dashboards.</span></span>

## <a name="create-a-dashboard"></a><span data-ttu-id="b414b-106">Skapa en instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="b414b-106">Create a dashboard</span></span>
<span data-ttu-id="b414b-107">Om du vill skapa en instrumentpanel, Välj den **ny instrumentpanel** knappen bredvid namnet på den aktuella instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="b414b-107">To create a dashboard, select the **New dashboard** button next to the current dashboard's name.</span></span>  

![Skapa instrumentpanel](./media/azure-portal-dashboards/new-dashboard.png)

<span data-ttu-id="b414b-109">Den här åtgärden skapar en ny, tom, privata instrumentpanel och placerar du i anpassning läge där du kan kalla instrumentpanelen och lägga till eller ordna om paneler.</span><span class="sxs-lookup"><span data-stu-id="b414b-109">This action creates a new, empty, private dashboard and puts you into customization mode where you can name your dashboard and add or rearrange tiles.</span></span>  <span data-ttu-id="b414b-110">I detta läge tar döljas panelen galleriet över den vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="b414b-110">When in this mode, the collapsible tile gallery takes over the left navigation menu.</span></span>  <span data-ttu-id="b414b-111">Galleriet sida vid sida kan du hitta paneler för Azure-resurser på olika sätt: du kan bläddra genom [resursgruppen](../azure-resource-manager/resource-group-overview.md#resource-groups), av resurstyp, av [taggen](../azure-resource-manager/resource-group-using-tags.md), eller genom att söka efter resurs efter namn.</span><span class="sxs-lookup"><span data-stu-id="b414b-111">The tile gallery lets you find tiles for your Azure resources in various ways: you can browse by [resource group](../azure-resource-manager/resource-group-overview.md#resource-groups), by resource type, by [tag](../azure-resource-manager/resource-group-using-tags.md), or by searching for your resource by name.</span></span>  

![Anpassa instrumentpanel](./media/azure-portal-dashboards/customize-dashboard.png)

<span data-ttu-id="b414b-113">Lägg till paneler genom att dra och släppa dem i instrumentpanelen ytan var du vill.</span><span class="sxs-lookup"><span data-stu-id="b414b-113">Add tiles by dragging and dropping them onto the dashboard surface wherever you want.</span></span>

<span data-ttu-id="b414b-114">Det finns en ny kategori som heter **allmänna** för brickor som inte är associerade med en viss resurs.</span><span class="sxs-lookup"><span data-stu-id="b414b-114">There's a new category called **General** for tiles that are not associated with a particular resource.</span></span>  <span data-ttu-id="b414b-115">I det här exemplet Fäst vi panelen Markdown.</span><span class="sxs-lookup"><span data-stu-id="b414b-115">In this example, we pin the Markdown tile.</span></span>  <span data-ttu-id="b414b-116">Du kan använda den här panelen för att lägga till anpassade innehåll på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="b414b-116">You use this tile to add custom content to your dashboard.</span></span>  <span data-ttu-id="b414b-117">Panelen stöder oformaterad text [markdownsyntax](https://daringfireball.net/projects/markdown/syntax), och en begränsad uppsättning HTML.</span><span class="sxs-lookup"><span data-stu-id="b414b-117">The tile supports plain text, [Markdown syntax](https://daringfireball.net/projects/markdown/syntax), and a limited set of HTML.</span></span>  <span data-ttu-id="b414b-118">(För säkerhet, kan du göra saker som mata in `<script>` taggar eller använda vissa formatmalls-element i CSS som kan störa portalen.)</span><span class="sxs-lookup"><span data-stu-id="b414b-118">(For safety, you can't do things like inject `<script>` tags or use certain styling element of CSS that might interfere with the portal.)</span></span> 

![Lägg till markdown](./media/azure-portal-dashboards/add-markdown.png)

## <a name="edit-a-dashboard"></a><span data-ttu-id="b414b-120">Redigera en instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="b414b-120">Edit a dashboard</span></span>
<span data-ttu-id="b414b-121">Du kan fästa paneler i galleriet panelen eller sida vid sida-representation av blad när du har skapat din instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="b414b-121">After creating your dashboard, you can pin tiles from the tile gallery or the tile representation of blades.</span></span> <span data-ttu-id="b414b-122">Vi Fäst representation av våra resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="b414b-122">Let's pin the representation of our resource group.</span></span> <span data-ttu-id="b414b-123">Du kan antingen PIN-kod när du bläddrar objektet, eller från bladet för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="b414b-123">You can either pin when browsing the item, or from the resource group blade.</span></span> <span data-ttu-id="b414b-124">Båda metoderna resultera i att fästa panelen representation av resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="b414b-124">Both approaches result in pinning the tile representation of the resource group.</span></span>

![Fäst på instrumentpanelen](./media/azure-portal-dashboards/pin-to-dashboard.png)

<span data-ttu-id="b414b-126">När filen har fästs objektet, visas det på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="b414b-126">After pinning the item, it appears on your dashboard.</span></span>

![visa instrumentpanelen](./media/azure-portal-dashboards/view-dashboard.png)

<span data-ttu-id="b414b-128">Nu när vi har en Markdown-panelen och en resursgrupp fäst på instrumentpanelen kan vi ändra storlek på och flytta panelerna i en lämplig layout.</span><span class="sxs-lookup"><span data-stu-id="b414b-128">Now that we have a Markdown tile and a resource group pinned to the dashboard, we can resize and rearrange the tiles into a suitable layout.</span></span>

<span data-ttu-id="b414b-129">Du kan se alla kontextuella kommandon för brickan av hovra och välja ”...” eller högerklicka på en panel.</span><span class="sxs-lookup"><span data-stu-id="b414b-129">By hovering and selecting "…" or right-clicking on a tile you can see all the contextual commands for that tile.</span></span> <span data-ttu-id="b414b-130">Det finns två objekt som standard:</span><span class="sxs-lookup"><span data-stu-id="b414b-130">By default, there are two items:</span></span>

1. <span data-ttu-id="b414b-131">**Ta bort från instrumentpanelen** – tar bort panelen från instrumentpanelen</span><span class="sxs-lookup"><span data-stu-id="b414b-131">**Unpin from dashboard** – removes the tile from the dashboard</span></span>
2. <span data-ttu-id="b414b-132">**Anpassa** – anger anpassa läge</span><span class="sxs-lookup"><span data-stu-id="b414b-132">**Customize** – enters customize mode</span></span>

![Anpassa sida vid sida](./media/azure-portal-dashboards/customize-tile.png)

<span data-ttu-id="b414b-134">Genom att välja Anpassa, kan du ändra storlek på och ordna om paneler.</span><span class="sxs-lookup"><span data-stu-id="b414b-134">By selecting customize, you can resize and reorder tiles.</span></span> <span data-ttu-id="b414b-135">Om du vill ändra storlek på en panel, Välj den nya storleken i snabbmenyn som visas i följande bild.</span><span class="sxs-lookup"><span data-stu-id="b414b-135">To resize a tile, select the new size from the contextual menu, as shown in the following image.</span></span>

![Ändra storlek på panelen](./media/azure-portal-dashboards/resize-tile.png)

<span data-ttu-id="b414b-137">Eller, om panelen stöder alla storlekar, kan du dra det nedre högra hörnet till önskad storlek.</span><span class="sxs-lookup"><span data-stu-id="b414b-137">Or, if the tile supports any size, you can drag the bottom right-hand corner to the desired size.</span></span>

![Ändra storlek på panelen](./media/azure-portal-dashboards/resize-corner.png)

<span data-ttu-id="b414b-139">När du ändrar storlek på panelerna, visa instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="b414b-139">After resizing tiles, view the dashboard.</span></span>

![vyn sida vid sida](./media/azure-portal-dashboards/view-tile.png)

<span data-ttu-id="b414b-141">När du är klar med att anpassa en instrumentpanel, markerar du bara den **klar anpassa** avsluta anpassa läge eller högerklicka och välj **klar anpassa** på snabbmenyn.</span><span class="sxs-lookup"><span data-stu-id="b414b-141">Once you are finished customizing a dashboard, simply select the **Done customizing** to exit customize mode or right-click and select **Done customizing** from the context menu.</span></span>

## <a name="publish-a-dashboard-and-manage-access-control"></a><span data-ttu-id="b414b-142">Publicera en instrumentpanel och hantera åtkomstkontrollen</span><span class="sxs-lookup"><span data-stu-id="b414b-142">Publish a dashboard and manage access control</span></span>
<span data-ttu-id="b414b-143">Den är privat som standard, vilket innebär att du är den enda som kan se den när du skapar en instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="b414b-143">When you create a dashboard, it is private by default, which means you are the only person who can see it.</span></span>  <span data-ttu-id="b414b-144">Använd för att göra den synlig för andra i **resursen** som visas tillsammans med andra kommandon för instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="b414b-144">To make it visible to others, use the **Share** button that appears alongside the other dashboard commands.</span></span>

![dela instrumentpanelen](./media/azure-portal-dashboards/share-dashboard.png)

<span data-ttu-id="b414b-146">Du uppmanas att välja en prenumeration och resursgrupp för din instrumentpanel som ska publiceras.</span><span class="sxs-lookup"><span data-stu-id="b414b-146">You are asked to choose a subscription and resource group for your dashboard to be published to.</span></span> <span data-ttu-id="b414b-147">Om du vill integrera sömlöst instrumentpaneler i ekosystemet, har vi implementerat delade instrumentpaneler som Azure-resurser (så att du inte kan dela genom att skriva en e-postadress).</span><span class="sxs-lookup"><span data-stu-id="b414b-147">To seamlessly integrate dashboards into the ecosystem, we've implemented shared dashboards as Azure resources (so you can't share by typing an email address).</span></span>  <span data-ttu-id="b414b-148">Åtkomst till den information som visas i de flesta paneler i portalen regleras av [Azure rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="b414b-148">Access to the information displayed by most of the tiles in the portal are governed by [Azure Role Based Access Control](../active-directory/role-based-access-control-configure.md).</span></span> <span data-ttu-id="b414b-149">Ur ett access control skiljer delade instrumentpaneler sig inte från en virtuell dator eller ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="b414b-149">From an access control perspective, shared dashboards are no different from a virtual machine or a storage account.</span></span>  

<span data-ttu-id="b414b-150">Anta att du har en Azure-prenumeration och gruppmedlemmarna har tilldelats rollerna i **ägare**, **deltagare**, eller **reader** för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="b414b-150">Let's say you have an Azure subscription and members of your team have been assigned the roles of **owner**, **contributor**, or **reader** of the subscription.</span></span>  <span data-ttu-id="b414b-151">Användare som är ägare eller deltagare kan visa, visa, skapa, ändra eller ta bort instrumentpaneler i den prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="b414b-151">Users who are owners or contributors are able to list, view, create, modify, or delete dashboards within that subscription.</span></span>  <span data-ttu-id="b414b-152">Användare som är läsare kan inte kan listan och visa instrumentpaneler, men ändra eller ta bort dem.</span><span class="sxs-lookup"><span data-stu-id="b414b-152">Users who are readers are able to list and view dashboards, but cannot modify or delete them.</span></span>  <span data-ttu-id="b414b-153">Användare med reader åtkomst kan göra lokala redigeringar i en delad instrumentpanel, men det går inte att publicera ändringarna på servern.</span><span class="sxs-lookup"><span data-stu-id="b414b-153">Users with reader access are able to make local edits to a shared dashboard, but are not able to publish those changes back to the server.</span></span>  <span data-ttu-id="b414b-154">De kan dock göra en privat kopia av instrumentpanelen för eget bruk.</span><span class="sxs-lookup"><span data-stu-id="b414b-154">However, they can make a private copy of the dashboard for their own use.</span></span>  <span data-ttu-id="b414b-155">Enskilda paneler på instrumentpanelen framtvinga som alltid sina egna regler för åtkomstkontroll baserat på de resurser som de motsvarar.</span><span class="sxs-lookup"><span data-stu-id="b414b-155">As always, individual tiles on the dashboard enforce their own access control rules based on the resources they correspond to.</span></span>  

<span data-ttu-id="b414b-156">För enkelhetens skull portalen har publicering upplevelse guider du mot ett mönster som placerar du instrumentpaneler i en resursgrupp kallas **instrumentpaneler**.</span><span class="sxs-lookup"><span data-stu-id="b414b-156">For convenience, the portal's publishing experience guides you towards a pattern where you place dashboards in a resource group called **dashboards**.</span></span>  

![publicera instrumentpanelen](./media/azure-portal-dashboards/publish-dashboard.png)

<span data-ttu-id="b414b-158">Du kan också välja att publicera en instrumentpanel till en viss resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="b414b-158">You can also choose to publish a dashboard to a particular resource group.</span></span>  <span data-ttu-id="b414b-159">Åtkomstkontroll för instrumentpanelen matchar åtkomstkontroll för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="b414b-159">The access control for that dashboard matches the access control for the resource group.</span></span>  <span data-ttu-id="b414b-160">Användare som kan hantera resurser i resursgruppen har också åtkomst till instrumentpanelerna.</span><span class="sxs-lookup"><span data-stu-id="b414b-160">Users that can manage the resources in that resource group also have access to the dashboards.</span></span>

![publicera instrumentpanelen till resursgruppen](./media/azure-portal-dashboards/publish-to-resource-group.png)

<span data-ttu-id="b414b-162">När instrumentpanelen har publicerats i **delning + åtkomst** kontrollen ska uppdatera och visa information om publicerade instrumentpanelen, inklusive en länk för att hantera användarnas åtkomst till instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="b414b-162">After your dashboard is published, the **Sharing + access** control pane will refresh and show you information about the published dashboard, including a link to manage user access to the dashboard.</span></span>  <span data-ttu-id="b414b-163">Den här länken öppnar bladet standard rollbaserad åtkomstkontroll används för att hantera åtkomsten för Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="b414b-163">This link launches the standard Role Based Access Control blade used to manage access for any Azure resource.</span></span>  <span data-ttu-id="b414b-164">Du alltid kan komma tillbaka till den här vyn genom att välja **resursen**.</span><span class="sxs-lookup"><span data-stu-id="b414b-164">You can always get back to this view by selecting **Share**.</span></span>

![Hantera åtkomstkontroll](./media/azure-portal-dashboards/manage-access.png)

## <a name="next-steps"></a><span data-ttu-id="b414b-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b414b-166">Next steps</span></span>
* <span data-ttu-id="b414b-167">För att hantera resurser, se [hantera Azure-resurser via portalen](../azure-resource-manager/resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b414b-167">To manage resources, see [Manage Azure resources through portal](../azure-resource-manager/resource-group-portal.md).</span></span>
* <span data-ttu-id="b414b-168">Om du vill distribuera resurser, se [distribuera resurser med Resource Manager-mallar och Azure-portalen](../azure-resource-manager/resource-group-template-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b414b-168">To deploy resources, see [Deploy resources with Resource Manager templates and Azure portal](../azure-resource-manager/resource-group-template-deploy-portal.md).</span></span>


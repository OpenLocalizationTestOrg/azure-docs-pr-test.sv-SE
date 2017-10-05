---
title: Uppdatera Azure RemoteApp-samlingen | Microsoft Docs
description: "Lär dig hur du uppdaterar din Azure RemoteApp-samling"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
editor: 
ms.assetid: e553d432-e581-48fe-b996-c432357eb64a
ms.service: remoteapp
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: compute
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 454d78445d6092aec9eaa383e4c50cf15195848c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="update-a-collection-in-azure-remoteapp"></a><span data-ttu-id="47284-103">Uppdatera samlingen i Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="47284-103">Update a collection in Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="47284-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="47284-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="47284-105">Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.</span><span class="sxs-lookup"><span data-stu-id="47284-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="47284-106">Det kommer en tid, ändras när du behöver uppdatera appar eller avbildning i Azure RemoteApp-samlingen.</span><span class="sxs-lookup"><span data-stu-id="47284-106">There will come a time, inevitably, when you need to update the apps or image in your Azure RemoteApp collection.</span></span> <span data-ttu-id="47284-107">Om du använder en av avbildningarna som ingår i din Azure RemoteApp-prenumeration i ett moln eller hybrid-samling hanteras alla uppdateringar av Azure RemoteApp, så du kan vara enkel.</span><span class="sxs-lookup"><span data-stu-id="47284-107">If you are using one of the images included with your Azure RemoteApp subscription, in either a cloud or hybrid collection, any and all updates are handled by Azure RemoteApp itself, so you can rest easy.</span></span>

<span data-ttu-id="47284-108">Om du använder en anpassad avbildning (du skapat från grunden eller som du skapade genom att ändra någon av våra bilderna) kan emellertid du ansvar för underhållet av avbildningen och programmen.</span><span class="sxs-lookup"><span data-stu-id="47284-108">However, if you are using a custom image (either that you built from scratch or that you created by modifying one of our images), you are in charge of maintaining the image and apps.</span></span> <span data-ttu-id="47284-109">Om du behöver uppdatera avbildningen eller apparna inuti måste du skapa en ny, uppdaterad version av bilden och Ersätt den befintliga avbildningen i samlingen med den här nya uppdaterade avbildningen.</span><span class="sxs-lookup"><span data-stu-id="47284-109">If you need to update your image or any of the apps inside it, you need to create a new, updated version of the image, and then replace the existing image in your collection with this new updated image.</span></span>

<span data-ttu-id="47284-110">Så, hur skaffar du uppdaterar din samling?</span><span class="sxs-lookup"><span data-stu-id="47284-110">So, how do you go about updating your collection?</span></span> <span data-ttu-id="47284-111">Det är ganska enkelt:</span><span class="sxs-lookup"><span data-stu-id="47284-111">It's fairly straightforward:</span></span>

1. <span data-ttu-id="47284-112">Uppdatera den bild som du använde i samlingen.</span><span class="sxs-lookup"><span data-stu-id="47284-112">Update the image that you used in your collection.</span></span> <span data-ttu-id="47284-113">Tillämpa alla korrigeringsfiler eller uppdateringar som krävs och sedan spara det med ett nytt namn.</span><span class="sxs-lookup"><span data-stu-id="47284-113">Apply any patches or updates needed, and then save it with a new name.</span></span>
2. <span data-ttu-id="47284-114">[Överför](remoteapp-uploadimage.md) eller [importera](remoteapp-image-on-azurevm.md) avbildningen till RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="47284-114">[Upload](remoteapp-uploadimage.md) or [import](remoteapp-image-on-azurevm.md) that image to RemoteApp.</span></span>
3. <span data-ttu-id="47284-115">På sidan samling som du **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="47284-115">Now, on the collection page, click **Update**.</span></span>
4. <span data-ttu-id="47284-116">Välj den nya avbildningen från den **Mallavbildningen** lista.</span><span class="sxs-lookup"><span data-stu-id="47284-116">Choose the new image from the **Template Image** list.</span></span>
5. <span data-ttu-id="47284-117">Här ingår komplicerade - måste du bestämma hur du arbetar med alla användare som använder en app i samlingen.</span><span class="sxs-lookup"><span data-stu-id="47284-117">Here's the tricky part - you need to decide how to deal with any users that are currently using an app in the collection.</span></span> <span data-ttu-id="47284-118">Du har följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="47284-118">You have the following choices:</span></span>
   
   * <span data-ttu-id="47284-119">**Ge användarna 60 minuter efter uppdateringen**.</span><span class="sxs-lookup"><span data-stu-id="47284-119">**Give users 60 minutes after the update**.</span></span> <span data-ttu-id="47284-120">När uppdateringen är klar visas ett meddelande i Azure RemoteApp till alla aktiva användare som talar om för att spara arbetet och logga ut och logga in igen.</span><span class="sxs-lookup"><span data-stu-id="47284-120">As soon as the update is finished, Azure RemoteApp will display a message to any active users telling them to save their work and log off and log back in.</span></span> <span data-ttu-id="47284-121">Efter 60 minuter, aktiva användare som inte har loggat ut automatiskt att loggas ut.</span><span class="sxs-lookup"><span data-stu-id="47284-121">After 60 minutes, any active users who have not logged off will be automatically logged off.</span></span> <span data-ttu-id="47284-122">Användare kan omedelbart logga in igen.</span><span class="sxs-lookup"><span data-stu-id="47284-122">Users can immediately log back on.</span></span>
   * <span data-ttu-id="47284-123">**Logga ut användarna omedelbart**.</span><span class="sxs-lookup"><span data-stu-id="47284-123">**Sign users out immediately**.</span></span> <span data-ttu-id="47284-124">När uppdateringen är klar, logga ut alla användare automatiskt utan varning.</span><span class="sxs-lookup"><span data-stu-id="47284-124">As soon as the update is finished, log off all users automatically without any warning.</span></span> <span data-ttu-id="47284-125">Om du väljer det här alternativet kan användare förlora data.</span><span class="sxs-lookup"><span data-stu-id="47284-125">If you choose this option, users might lose data.</span></span> <span data-ttu-id="47284-126">Men kan de återansluta till appen omedelbart.</span><span class="sxs-lookup"><span data-stu-id="47284-126">However, they can reconnect to the app immediately.</span></span>
6. <span data-ttu-id="47284-127">Klicka på kryssmarkeringen för att starta uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="47284-127">Click the check mark to start the update.</span></span>


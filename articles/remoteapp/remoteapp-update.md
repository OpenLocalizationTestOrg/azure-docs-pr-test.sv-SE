---
title: aaaUpdate Azure RemoteApp-samlingen | Microsoft Docs
description: "Lär dig hur tooupdate Azure RemoteApp-samling"
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
ms.openlocfilehash: 849d7abfdfad4dbe6a235d2a28c71f7943eb812c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="update-a-collection-in-azure-remoteapp"></a><span data-ttu-id="fe752-103">Uppdatera samlingen i Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="fe752-103">Update a collection in Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="fe752-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="fe752-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="fe752-105">Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.</span><span class="sxs-lookup"><span data-stu-id="fe752-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="fe752-106">Det kommer en tid, ändras när du behöver tooupdate hello appar eller avbildning i Azure RemoteApp-samlingen.</span><span class="sxs-lookup"><span data-stu-id="fe752-106">There will come a time, inevitably, when you need tooupdate hello apps or image in your Azure RemoteApp collection.</span></span> <span data-ttu-id="fe752-107">Om du använder en hello avbildningarna som ingår i din Azure RemoteApp-prenumeration i ett moln eller hybrid-samling hanteras alla uppdateringar av Azure RemoteApp, så du kan vara enkel.</span><span class="sxs-lookup"><span data-stu-id="fe752-107">If you are using one of hello images included with your Azure RemoteApp subscription, in either a cloud or hybrid collection, any and all updates are handled by Azure RemoteApp itself, so you can rest easy.</span></span>

<span data-ttu-id="fe752-108">Om du använder en anpassad avbildning (du skapat från grunden eller som du skapade genom att ändra någon av våra bilderna) kan emellertid du ansvar för underhållet hello avbildningen och programmen.</span><span class="sxs-lookup"><span data-stu-id="fe752-108">However, if you are using a custom image (either that you built from scratch or that you created by modifying one of our images), you are in charge of maintaining hello image and apps.</span></span> <span data-ttu-id="fe752-109">Om du behöver tooupdate avbildningen eller någon av hello apparna inuti den, måste toocreate en nya, uppdaterade version hello-avbildningen och Ersätt hello befintlig avbildning i samlingen med den här nya uppdaterade avbildningen.</span><span class="sxs-lookup"><span data-stu-id="fe752-109">If you need tooupdate your image or any of hello apps inside it, you need toocreate a new, updated version of hello image, and then replace hello existing image in your collection with this new updated image.</span></span>

<span data-ttu-id="fe752-110">Så, hur skaffar du uppdaterar din samling?</span><span class="sxs-lookup"><span data-stu-id="fe752-110">So, how do you go about updating your collection?</span></span> <span data-ttu-id="fe752-111">Det är ganska enkelt:</span><span class="sxs-lookup"><span data-stu-id="fe752-111">It's fairly straightforward:</span></span>

1. <span data-ttu-id="fe752-112">Uppdatera hello-avbildning som du använde i samlingen.</span><span class="sxs-lookup"><span data-stu-id="fe752-112">Update hello image that you used in your collection.</span></span> <span data-ttu-id="fe752-113">Tillämpa alla korrigeringsfiler eller uppdateringar som krävs och sedan spara det med ett nytt namn.</span><span class="sxs-lookup"><span data-stu-id="fe752-113">Apply any patches or updates needed, and then save it with a new name.</span></span>
2. <span data-ttu-id="fe752-114">[Överför](remoteapp-uploadimage.md) eller [importera](remoteapp-image-on-azurevm.md) tooRemoteApp det bild.</span><span class="sxs-lookup"><span data-stu-id="fe752-114">[Upload](remoteapp-uploadimage.md) or [import](remoteapp-image-on-azurevm.md) that image tooRemoteApp.</span></span>
3. <span data-ttu-id="fe752-115">Nu på hello samlingen klickar du på **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="fe752-115">Now, on hello collection page, click **Update**.</span></span>
4. <span data-ttu-id="fe752-116">Välj ny avbildning för hello hello **Mallavbildningen** lista.</span><span class="sxs-lookup"><span data-stu-id="fe752-116">Choose hello new image from hello **Template Image** list.</span></span>
5. <span data-ttu-id="fe752-117">Här ingår hello komplicerade – du behöver toodecide hur toodeal med alla användare som använder en app i hello samling.</span><span class="sxs-lookup"><span data-stu-id="fe752-117">Here's hello tricky part - you need toodecide how toodeal with any users that are currently using an app in hello collection.</span></span> <span data-ttu-id="fe752-118">Du har hello följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="fe752-118">You have hello following choices:</span></span>
   
   * <span data-ttu-id="fe752-119">**Ge användarna 60 minuter efter uppdateringen hello**.</span><span class="sxs-lookup"><span data-stu-id="fe752-119">**Give users 60 minutes after hello update**.</span></span> <span data-ttu-id="fe752-120">Så snart hello uppdateringen har slutförts visas ett meddelande tooany aktiva användare talar toosave arbetets och logg inaktiverat och logga in igen med Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="fe752-120">As soon as hello update is finished, Azure RemoteApp will display a message tooany active users telling them toosave their work and log off and log back in.</span></span> <span data-ttu-id="fe752-121">Efter 60 minuter, aktiva användare som inte har loggat ut automatiskt att loggas ut.</span><span class="sxs-lookup"><span data-stu-id="fe752-121">After 60 minutes, any active users who have not logged off will be automatically logged off.</span></span> <span data-ttu-id="fe752-122">Användare kan omedelbart logga in igen.</span><span class="sxs-lookup"><span data-stu-id="fe752-122">Users can immediately log back on.</span></span>
   * <span data-ttu-id="fe752-123">**Logga ut användarna omedelbart**.</span><span class="sxs-lookup"><span data-stu-id="fe752-123">**Sign users out immediately**.</span></span> <span data-ttu-id="fe752-124">Så snart hello uppdateringen har slutförts kan du logga ut alla användare automatiskt utan varning.</span><span class="sxs-lookup"><span data-stu-id="fe752-124">As soon as hello update is finished, log off all users automatically without any warning.</span></span> <span data-ttu-id="fe752-125">Om du väljer det här alternativet kan användare förlora data.</span><span class="sxs-lookup"><span data-stu-id="fe752-125">If you choose this option, users might lose data.</span></span> <span data-ttu-id="fe752-126">De kan dock återansluta toohello app omedelbart.</span><span class="sxs-lookup"><span data-stu-id="fe752-126">However, they can reconnect toohello app immediately.</span></span>
6. <span data-ttu-id="fe752-127">Klicka på hello markerat toostart hello uppdatera.</span><span class="sxs-lookup"><span data-stu-id="fe752-127">Click hello check mark toostart hello update.</span></span>


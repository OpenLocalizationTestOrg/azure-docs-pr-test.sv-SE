---
title: aaaPublish en app i Azure RemoteApp | Microsoft Docs
description: "Lär dig hur toopublish program och resurser i Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: c7e1a2cd-8e1f-4a33-bd43-8032ec9ac952
ms.service: remoteapp
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: d7d92187e9ed999ac79554c9bb61f56a8eceeb31
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toopublish-an-app-in-remoteapp"></a><span data-ttu-id="4b57a-103">Hur toopublish en app i RemoteApp</span><span class="sxs-lookup"><span data-stu-id="4b57a-103">How toopublish an app in RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="4b57a-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="4b57a-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="4b57a-105">Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.</span><span class="sxs-lookup"><span data-stu-id="4b57a-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="4b57a-106">När du har skapat din RemoteApp-samling behöver du toopublish hello appar eller resurser som du vill toomake som är tillgängliga för användarna.</span><span class="sxs-lookup"><span data-stu-id="4b57a-106">After you create your RemoteApp collection, you need toopublish hello apps or resources that you want toomake available for your users.</span></span> <span data-ttu-id="4b57a-107">Hej mallavbildningarna som följer med din prenumeration bara ha några appar publiceras som standard - tooshare hello andra appar måste du toopublish dem.</span><span class="sxs-lookup"><span data-stu-id="4b57a-107">hello template images provided with your subscription only have a few apps published by default - tooshare hello other apps, you need toopublish them.</span></span>

> [!NOTE]
> <span data-ttu-id="4b57a-108">Behöver du tooupdate en app?</span><span class="sxs-lookup"><span data-stu-id="4b57a-108">Do you need tooupdate an app?</span></span> <span data-ttu-id="4b57a-109">Du behöver för[uppdatering hello avbildningen](remoteapp-update.md) första.</span><span class="sxs-lookup"><span data-stu-id="4b57a-109">You'll need too[update hello image](remoteapp-update.md) first.</span></span>
> 
> 

<span data-ttu-id="4b57a-110">På hello **Publishing** hello-portalen klickar du på **publicera**.</span><span class="sxs-lookup"><span data-stu-id="4b57a-110">On hello **Publishing** tab in hello portal, click **Publish**.</span></span> <span data-ttu-id="4b57a-111">Du kan antingen lägga till en app från din mallavbildningen **starta** menyn eller ange hello sökvägen toowhere hello appen är installerad på hello mallavbildningen.</span><span class="sxs-lookup"><span data-stu-id="4b57a-111">You can either add an app from your template image's **Start** menu or provide hello path toowhere hello app is installed on hello template image.</span></span> <span data-ttu-id="4b57a-112">Om du väljer tooadd från hello **starta** -menyn väljer hello app toopublish hello-listan.</span><span class="sxs-lookup"><span data-stu-id="4b57a-112">If you choose tooadd from hello **Start** menu, choose hello app toopublish from hello list.</span></span> <span data-ttu-id="4b57a-113">Om du väljer tooprovide hello sökvägen toohello app måste ange ett namn för hello app och hello sökvägen toohello app.</span><span class="sxs-lookup"><span data-stu-id="4b57a-113">If you choose tooprovide hello path toohello app, enter a name for hello app and hello path toohello app.</span></span> <span data-ttu-id="4b57a-114">Använda variabler i hello sökväg – till exempel ”% systemdrive %” i stället för ”c:\".</span><span class="sxs-lookup"><span data-stu-id="4b57a-114">Use variables in hello path - for example, "%systemdrive%" instead of "c:\".</span></span>

> [!NOTE]
> <span data-ttu-id="4b57a-115">Om du vill tooadd appen från hello **starta** menyn måste toohave *lagts till som app-toohello **starta** menyn på mallavbildningen.*</span><span class="sxs-lookup"><span data-stu-id="4b57a-115">If you want tooadd your app from hello **Start** menu, you need toohave *added that app toohello **Start** menu on your template image.*</span></span> <span data-ttu-id="4b57a-116">Annars RemoteApp visas bara vad *är* på hello **starta** -menyn och ska förväxlas.</span><span class="sxs-lookup"><span data-stu-id="4b57a-116">Otherwise, RemoteApp will only see what *is* on hello **Start** menu, and you will be confused.</span></span> 
> 
> <span data-ttu-id="4b57a-117">toomake att dina appar är i hello **starta** menyn placera genvägsfilen - **lnk** - inuti hello %systemdrive%\ProgramData\Microsoft\Windows\Start %ALLUSERSPROFILE%\Microsoft\Windows\Start-meny\Program.</span><span class="sxs-lookup"><span data-stu-id="4b57a-117">toomake sure your app is in hello **Start** menu, place a shortcut file - **.lnk** - inside hello %systemdrive%\ProgramData\Microsoft\Windows\Start Menu\Programs folder.</span></span>
> 
> <span data-ttu-id="4b57a-118">Om du har glömt tooadd hello app toohello **starta** menyn när du skapade hello mall och välj tooadd hello sökvägen toohello app.</span><span class="sxs-lookup"><span data-stu-id="4b57a-118">If you forgot tooadd hello app toohello **Start** menu when you created hello template, choose tooadd hello path toohello app.</span></span> <span data-ttu-id="4b57a-119">(Eller återskapa din mallavbildningen, men det är ganska är lite mer arbete.)</span><span class="sxs-lookup"><span data-stu-id="4b57a-119">(Or recreate your template image, but that's quite a bit more work.)</span></span>
> 
> 


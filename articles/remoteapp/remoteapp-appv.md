---
title: aaaUsing App-V-appar med Azure RemoteApp | Microsoft Docs
description: "Lär dig hur toouse App-V-program i Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
ms.assetid: e2292cb2-5c89-4b2b-ab11-74dbacd07c31
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 9cf5c2eeee2a0ce2cf98e1cf6497dffbc6eff016
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-app-v-apps-in-azure-remoteapp"></a><span data-ttu-id="92f24-103">Använda App-V-appar i Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="92f24-103">Using App-V apps in Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="92f24-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="92f24-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="92f24-105">Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.</span><span class="sxs-lookup"><span data-stu-id="92f24-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="92f24-106">Du kan använda App-V-program i en Azure RemoteApp-hybridsamling som kräver domänanslutning.</span><span class="sxs-lookup"><span data-stu-id="92f24-106">You can use App-V applications in a Azure RemoteApp hybrid collection, which requires domain join.</span></span>

<span data-ttu-id="92f24-107">Innan du börjar, kontrollera att tooinstall hello App-V 5.1 klienten med hello senaste uppdateringarna.</span><span class="sxs-lookup"><span data-stu-id="92f24-107">Before you get started, make sure tooinstall hello App-V 5.1 client with hello latest updates.</span></span> <span data-ttu-id="92f24-108">Du behöver toocreate en [anpassad avbildning](remoteapp-create-custom-image.md) som innehåller hello App-V-klienten.</span><span class="sxs-lookup"><span data-stu-id="92f24-108">You will need toocreate a [custom image](remoteapp-create-custom-image.md) that includes hello App-V client.</span></span>  

<span data-ttu-id="92f24-109">Det är enkelt toouse din befintliga App-V-infrastruktur med Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="92f24-109">It’s easy toouse your existing App-V infrastructure with Azure RemoteApp.</span></span> <span data-ttu-id="92f24-110">Eftersom en hybridsamling distribueras till ett virtuellt Azure-nätverk som har åtkomst tooyour domänkontrollant och hello är ansluten till en domän, kan du utnyttja befintliga App-v-infrastruktur och distribution metoder tooeasyily App-V värdprogrammet i Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="92f24-110">Since a hybrid collection is deployed into an Azure VNET that has access tooyour domain controller and hello VMs are domain joined, you can leverage your existing App-v infrastructure and deployment methods tooeasyily host App-V application in Azure RemoteApp.</span></span> <span data-ttu-id="92f24-111">Här följer ett par saker som du bör vara medveten om baserat på hello typen av App-V-distribution som du har för tillfället:</span><span class="sxs-lookup"><span data-stu-id="92f24-111">Here are some considerations that you should be aware of based on hello type of App-V deployment you currently have:</span></span>

| <span data-ttu-id="92f24-112">Konfigurationsalternativ</span><span class="sxs-lookup"><span data-stu-id="92f24-112">Configuration options</span></span> |  | <span data-ttu-id="92f24-113">Positivt</span><span class="sxs-lookup"><span data-stu-id="92f24-113">Positive</span></span> | <span data-ttu-id="92f24-114">Negativt</span><span class="sxs-lookup"><span data-stu-id="92f24-114">Negative</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="92f24-115">Leveransmetod</span><span class="sxs-lookup"><span data-stu-id="92f24-115">Delivery method</span></span> |<span data-ttu-id="92f24-116">Streaming (på begäran)</span><span class="sxs-lookup"><span data-stu-id="92f24-116">Streaming (on-demand)</span></span> |<span data-ttu-id="92f24-117">Appen är alltid hello senaste och uppdaterad</span><span class="sxs-lookup"><span data-stu-id="92f24-117">App is always hello latest and fresh</span></span> |<span data-ttu-id="92f24-118">Första tidsfördröjningen</span><span class="sxs-lookup"><span data-stu-id="92f24-118">First time latency</span></span> |
| <span data-ttu-id="92f24-119">Montera</span><span class="sxs-lookup"><span data-stu-id="92f24-119">Mounted</span></span> |<span data-ttu-id="92f24-120">Snabbaste; appen finns redan på hello VM</span><span class="sxs-lookup"><span data-stu-id="92f24-120">Fastest; app is already present on hello VM</span></span> |<span data-ttu-id="92f24-121">Överdriven storlek - tar upp avbildningen utrymme (gräns 127 GB)</span><span class="sxs-lookup"><span data-stu-id="92f24-121">Bloat - takes up image space (127 GB limit)</span></span> | |
| <span data-ttu-id="92f24-122">Appen plats lagring</span><span class="sxs-lookup"><span data-stu-id="92f24-122">App location storage</span></span> |<span data-ttu-id="92f24-123">Delat innehåll</span><span class="sxs-lookup"><span data-stu-id="92f24-123">Shared content</span></span> |<span data-ttu-id="92f24-124">Appen körs i minnet för Azure RemoteApp-instans</span><span class="sxs-lookup"><span data-stu-id="92f24-124">App runs in memory of Azure RemoteApp instance</span></span> |<span data-ttu-id="92f24-125">Eats minne och bra anslutning toostreaming (fil) server där app hello finns</span><span class="sxs-lookup"><span data-stu-id="92f24-125">Eats memory and good connection toostreaming (file) server where hello app resides</span></span> |
| <span data-ttu-id="92f24-126">Disken (Zeroed)</span><span class="sxs-lookup"><span data-stu-id="92f24-126">Disk (Cached)</span></span> |<span data-ttu-id="92f24-127">Snabb körning.</span><span class="sxs-lookup"><span data-stu-id="92f24-127">Fast execution.</span></span> <span data-ttu-id="92f24-128">Appen är inte beroende av tillgängligheten för innehållskälla</span><span class="sxs-lookup"><span data-stu-id="92f24-128">App not dependent on availability of Content Source</span></span> |<span data-ttu-id="92f24-129">Överdriven storlek - tar upp avbildningen utrymme (gräns 127 GB)</span><span class="sxs-lookup"><span data-stu-id="92f24-129">Bloat - takes up image space (127 GB limit)</span></span> | |
| <span data-ttu-id="92f24-130">Mål</span><span class="sxs-lookup"><span data-stu-id="92f24-130">Targeting</span></span> |<span data-ttu-id="92f24-131">Användare</span><span class="sxs-lookup"><span data-stu-id="92f24-131">User</span></span> |<span data-ttu-id="92f24-132">Kräver fullständig fristående App-V-infrastruktur</span><span class="sxs-lookup"><span data-stu-id="92f24-132">Requires full standalone App-V infrastructure</span></span> | |
| <span data-ttu-id="92f24-133">Global (dator)</span><span class="sxs-lookup"><span data-stu-id="92f24-133">Global (machine)</span></span> |<span data-ttu-id="92f24-134">Publicera före eller mål med publicering server</span><span class="sxs-lookup"><span data-stu-id="92f24-134">Pre-publish or target using Publishing server</span></span> |<span data-ttu-id="92f24-135">Måste tooupdate Azure bilden om du vill tooupdate hello app (stora).</span><span class="sxs-lookup"><span data-stu-id="92f24-135">Need tooupdate your Azure image if you want tooupdate hello app (huge).</span></span> <span data-ttu-id="92f24-136">Tar upp utrymme på avbildningen.</span><span class="sxs-lookup"><span data-stu-id="92f24-136">Takes up some space on image.</span></span> | |

 <span data-ttu-id="92f24-137">När du har skapat den anpassade avbildningen och hybridsamlingen publicera programmet, tilldela användare och få dina befintliga App-V-program finns i Azure RemoteApp levereras tooany enheten var som helst.</span><span class="sxs-lookup"><span data-stu-id="92f24-137">After you create your custom image and your hybrid collection, publish your application, assign users and enjoy your existing App-V applications hosted in Azure RemoteApp delivered tooany device anywhere.</span></span>


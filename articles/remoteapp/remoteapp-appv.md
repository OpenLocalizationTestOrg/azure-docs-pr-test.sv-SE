---
title: "Använda App-V-appar med Azure RemoteApp | Microsoft Docs"
description: "Lär dig hur du använder App-V-appar i Azure RemoteApp."
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
ms.openlocfilehash: e55bb8db83c04025c46b383a9ebbef4399178116
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="using-app-v-apps-in-azure-remoteapp"></a><span data-ttu-id="f9336-103">Använda App-V-appar i Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="f9336-103">Using App-V apps in Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="f9336-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="f9336-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="f9336-105">Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.</span><span class="sxs-lookup"><span data-stu-id="f9336-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="f9336-106">Du kan använda App-V-program i en Azure RemoteApp-hybridsamling som kräver domänanslutning.</span><span class="sxs-lookup"><span data-stu-id="f9336-106">You can use App-V applications in a Azure RemoteApp hybrid collection, which requires domain join.</span></span>

<span data-ttu-id="f9336-107">Innan du börjar måste du se till att installera App-V 5.1-klienten med de senaste uppdateringarna.</span><span class="sxs-lookup"><span data-stu-id="f9336-107">Before you get started, make sure to install the App-V 5.1 client with the latest updates.</span></span> <span data-ttu-id="f9336-108">Du behöver skapa en [anpassad avbildning](remoteapp-create-custom-image.md) som innehåller App-V-klienten.</span><span class="sxs-lookup"><span data-stu-id="f9336-108">You will need to create a [custom image](remoteapp-create-custom-image.md) that includes the App-V client.</span></span>  

<span data-ttu-id="f9336-109">Det är enkelt att använda din befintliga App-V-infrastruktur med Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="f9336-109">It’s easy to use your existing App-V infrastructure with Azure RemoteApp.</span></span> <span data-ttu-id="f9336-110">Eftersom en hybridsamling distribueras till ett virtuellt Azure-nätverk som har åtkomst till domänkontrollanten och de virtuella datorerna är domänansluten, kan du utnyttja din befintliga App-v-infrastrukturen och distribution av metoder för att easyily värden App-V-program i Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="f9336-110">Since a hybrid collection is deployed into an Azure VNET that has access to your domain controller and the VMs are domain joined, you can leverage your existing App-v infrastructure and deployment methods to easyily host App-V application in Azure RemoteApp.</span></span> <span data-ttu-id="f9336-111">Här följer ett par saker som du bör vara medveten om baserat på typen av App-V-distribution som du har för tillfället:</span><span class="sxs-lookup"><span data-stu-id="f9336-111">Here are some considerations that you should be aware of based on the type of App-V deployment you currently have:</span></span>

| <span data-ttu-id="f9336-112">Konfigurationsalternativ</span><span class="sxs-lookup"><span data-stu-id="f9336-112">Configuration options</span></span> |  | <span data-ttu-id="f9336-113">Positivt</span><span class="sxs-lookup"><span data-stu-id="f9336-113">Positive</span></span> | <span data-ttu-id="f9336-114">Negativt</span><span class="sxs-lookup"><span data-stu-id="f9336-114">Negative</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="f9336-115">Leveransmetod</span><span class="sxs-lookup"><span data-stu-id="f9336-115">Delivery method</span></span> |<span data-ttu-id="f9336-116">Streaming (på begäran)</span><span class="sxs-lookup"><span data-stu-id="f9336-116">Streaming (on-demand)</span></span> |<span data-ttu-id="f9336-117">Appen är alltid senast och nytt fönster</span><span class="sxs-lookup"><span data-stu-id="f9336-117">App is always the latest and fresh</span></span> |<span data-ttu-id="f9336-118">Första tidsfördröjningen</span><span class="sxs-lookup"><span data-stu-id="f9336-118">First time latency</span></span> |
| <span data-ttu-id="f9336-119">Montera</span><span class="sxs-lookup"><span data-stu-id="f9336-119">Mounted</span></span> |<span data-ttu-id="f9336-120">Snabbaste; appen finns redan på den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="f9336-120">Fastest; app is already present on the VM</span></span> |<span data-ttu-id="f9336-121">Överdriven storlek - tar upp avbildningen utrymme (gräns 127 GB)</span><span class="sxs-lookup"><span data-stu-id="f9336-121">Bloat - takes up image space (127 GB limit)</span></span> | |
| <span data-ttu-id="f9336-122">Appen plats lagring</span><span class="sxs-lookup"><span data-stu-id="f9336-122">App location storage</span></span> |<span data-ttu-id="f9336-123">Delat innehåll</span><span class="sxs-lookup"><span data-stu-id="f9336-123">Shared content</span></span> |<span data-ttu-id="f9336-124">Appen körs i minnet för Azure RemoteApp-instans</span><span class="sxs-lookup"><span data-stu-id="f9336-124">App runs in memory of Azure RemoteApp instance</span></span> |<span data-ttu-id="f9336-125">Eats minne och bra anslutning till streaming (filserver) där appen finns</span><span class="sxs-lookup"><span data-stu-id="f9336-125">Eats memory and good connection to streaming (file) server where the app resides</span></span> |
| <span data-ttu-id="f9336-126">Disken (Zeroed)</span><span class="sxs-lookup"><span data-stu-id="f9336-126">Disk (Cached)</span></span> |<span data-ttu-id="f9336-127">Snabb körning.</span><span class="sxs-lookup"><span data-stu-id="f9336-127">Fast execution.</span></span> <span data-ttu-id="f9336-128">Appen är inte beroende av tillgängligheten för innehållskälla</span><span class="sxs-lookup"><span data-stu-id="f9336-128">App not dependent on availability of Content Source</span></span> |<span data-ttu-id="f9336-129">Överdriven storlek - tar upp avbildningen utrymme (gräns 127 GB)</span><span class="sxs-lookup"><span data-stu-id="f9336-129">Bloat - takes up image space (127 GB limit)</span></span> | |
| <span data-ttu-id="f9336-130">Mål</span><span class="sxs-lookup"><span data-stu-id="f9336-130">Targeting</span></span> |<span data-ttu-id="f9336-131">Användare</span><span class="sxs-lookup"><span data-stu-id="f9336-131">User</span></span> |<span data-ttu-id="f9336-132">Kräver fullständig fristående App-V-infrastruktur</span><span class="sxs-lookup"><span data-stu-id="f9336-132">Requires full standalone App-V infrastructure</span></span> | |
| <span data-ttu-id="f9336-133">Global (dator)</span><span class="sxs-lookup"><span data-stu-id="f9336-133">Global (machine)</span></span> |<span data-ttu-id="f9336-134">Publicera före eller mål med publicering server</span><span class="sxs-lookup"><span data-stu-id="f9336-134">Pre-publish or target using Publishing server</span></span> |<span data-ttu-id="f9336-135">Om du behöver uppdatera din Azure-avbildning om du vill uppdatera appen (stora).</span><span class="sxs-lookup"><span data-stu-id="f9336-135">Need to update your Azure image if you want to update the app (huge).</span></span> <span data-ttu-id="f9336-136">Tar upp utrymme på avbildningen.</span><span class="sxs-lookup"><span data-stu-id="f9336-136">Takes up some space on image.</span></span> | |

 <span data-ttu-id="f9336-137">När du har skapat den anpassade avbildningen och hybridsamlingen publicera programmet, tilldela användare och få dina befintliga App-V-program finns i Azure RemoteApp som levereras till vilken enhet som helst var som helst.</span><span class="sxs-lookup"><span data-stu-id="f9336-137">After you create your custom image and your hybrid collection, publish your application, assign users and enjoy your existing App-V applications hosted in Azure RemoteApp delivered to any device anywhere.</span></span>


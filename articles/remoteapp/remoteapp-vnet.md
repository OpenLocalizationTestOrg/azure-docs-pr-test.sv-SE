---
title: aaaValidate hello Azure VNET toouse med Azure RemoteApp | Microsoft Docs
description: "Lär dig hur toomake till ditt Azure VNET är klar toouse med Azure RemoteApp"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: b573ba02-4587-4be5-9821-27bd891a73b2
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: 5587556c264356e6ab6039b983a38cb2b95ed268
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="validate-hello-azure-vnet-toouse-with-azure-remoteapp"></a><span data-ttu-id="a24af-103">Validera hello Azure VNET toouse med Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="a24af-103">Validate hello Azure VNET toouse with Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="a24af-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="a24af-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="a24af-105">Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.</span><span class="sxs-lookup"><span data-stu-id="a24af-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="a24af-106">Du kanske vill toovalidate hello VNET innan du använder ett virtuellt Azure-nätverk med Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="a24af-106">Before you use an Azure VNET with Azure RemoteApp, you might want toovalidate hello VNET.</span></span> <span data-ttu-id="a24af-107">Detta förhindrar att problem med anslutningen.</span><span class="sxs-lookup"><span data-stu-id="a24af-107">This helps prevent issues with connectivity.</span></span>

<span data-ttu-id="a24af-108">toovalidate ditt Azure VNET hello följande:</span><span class="sxs-lookup"><span data-stu-id="a24af-108">toovalidate your Azure VNET, do hello following:</span></span>

1. <span data-ttu-id="a24af-109">Skapa en virtuell Azure-dator i hello undernätet för hello Azure VNET som du vill toouse med Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="a24af-109">Create an Azure virtual machine inside hello subnet of hello Azure VNET you want toouse with Azure RemoteApp.</span></span>
2. <span data-ttu-id="a24af-110">Ansluta toothat VM med hjälp av hello **Anslut** alternativet i hello-hanteringsportalen.</span><span class="sxs-lookup"><span data-stu-id="a24af-110">Connect toothat VM by using hello **Connect** option in hello management portal.</span></span>
3. <span data-ttu-id="a24af-111">Anslut hello virtuella toohello samma domän som du vill toouse med Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="a24af-111">Join hello virtual machine toohello same domain that you want toouse with Azure RemoteApp.</span></span> <span data-ttu-id="a24af-112">Om du skapar en hybridsamling som ansluter tooyour lokalt nätverk att ansluta hello virtuella tooyour lokala domänen.</span><span class="sxs-lookup"><span data-stu-id="a24af-112">If you are creating a hybrid collection that connects tooyour on-premises network, join hello virtual machine tooyour local domain.</span></span>

<span data-ttu-id="a24af-113">Om detta lyckas är hello Azure VNET klar toouse med RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="a24af-113">If this is successful, hello Azure VNET is ready toouse with RemoteApp.</span></span>

<span data-ttu-id="a24af-114">Mer information om hello slutpunkt till slutpunkt hybrid samling arbetsflöde finns hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="a24af-114">For more information about hello end-to-end hybrid collection workflow, see hello following articles:</span></span>

* [<span data-ttu-id="a24af-115">Hur tooplan ditt virtuella nätverk för Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="a24af-115">How tooplan your virtual network for Azure RemoteApp</span></span>](remoteapp-planvnet.md)
* [<span data-ttu-id="a24af-116">Skapa en hybridsamling</span><span class="sxs-lookup"><span data-stu-id="a24af-116">Create a hybrid collection</span></span>](remoteapp-create-hybrid-deployment.md)
* [<span data-ttu-id="a24af-117">Distribuera Azure RemoteApp-samlingen tooyour Azure Virtual Network (med stöd för ExpressRoute)</span><span class="sxs-lookup"><span data-stu-id="a24af-117">Deploy Azure RemoteApp collection tooyour Azure Virtual Network (with support for ExpressRoute)</span></span>](http://blogs.msdn.com/b/rds/archive/2015/04/23/deploy-azure-remoteapp-collection-to-your-azure-virtual-network-with-support-for-expressroute.aspx)


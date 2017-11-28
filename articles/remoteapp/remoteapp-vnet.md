---
title: "Verifiera Azure VNET att använda med Azure RemoteApp | Microsoft Docs"
description: "Lär dig att kontrollera att ditt Azure VNET är redo att användas med Azure RemoteApp"
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
ms.openlocfilehash: 05c8a0ff04293947cec391b6467cc4adddb6a7b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="validate-the-azure-vnet-to-use-with-azure-remoteapp"></a><span data-ttu-id="a78b0-103">Verifiera Azure VNET att använda med Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="a78b0-103">Validate the Azure VNET to use with Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="a78b0-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="a78b0-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="a78b0-105">Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.</span><span class="sxs-lookup"><span data-stu-id="a78b0-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="a78b0-106">Innan du använder ett virtuellt Azure-nätverk med Azure RemoteApp kan du vill validera VNET.</span><span class="sxs-lookup"><span data-stu-id="a78b0-106">Before you use an Azure VNET with Azure RemoteApp, you might want to validate the VNET.</span></span> <span data-ttu-id="a78b0-107">Detta förhindrar att problem med anslutningen.</span><span class="sxs-lookup"><span data-stu-id="a78b0-107">This helps prevent issues with connectivity.</span></span>

<span data-ttu-id="a78b0-108">För att verifiera ditt Azure VNET, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="a78b0-108">To validate your Azure VNET, do the following:</span></span>

1. <span data-ttu-id="a78b0-109">Skapa en virtuell Azure-dator i undernätet i Azure-VNET som du vill använda med Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="a78b0-109">Create an Azure virtual machine inside the subnet of the Azure VNET you want to use with Azure RemoteApp.</span></span>
2. <span data-ttu-id="a78b0-110">Anslut till den virtuella datorn med hjälp av den **Anslut** alternativet i hanteringsportalen.</span><span class="sxs-lookup"><span data-stu-id="a78b0-110">Connect to that VM by using the **Connect** option in the management portal.</span></span>
3. <span data-ttu-id="a78b0-111">Anslut den virtuella datorn till samma domän som du vill använda med Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="a78b0-111">Join the virtual machine to the same domain that you want to use with Azure RemoteApp.</span></span> <span data-ttu-id="a78b0-112">Om du skapar en hybridsamling som ansluter till ditt lokala nätverk, Anslut den virtuella datorn till den lokala domänen.</span><span class="sxs-lookup"><span data-stu-id="a78b0-112">If you are creating a hybrid collection that connects to your on-premises network, join the virtual machine to your local domain.</span></span>

<span data-ttu-id="a78b0-113">Om detta lyckas är Azure VNET redo att användas med RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="a78b0-113">If this is successful, the Azure VNET is ready to use with RemoteApp.</span></span>

<span data-ttu-id="a78b0-114">Mer information om arbetsflödet för slutpunkt till slutpunkt hybrid-samling finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="a78b0-114">For more information about the end-to-end hybrid collection workflow, see the following articles:</span></span>

* [<span data-ttu-id="a78b0-115">Planera ditt virtuella nätverk för Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="a78b0-115">How to plan your virtual network for Azure RemoteApp</span></span>](remoteapp-planvnet.md)
* [<span data-ttu-id="a78b0-116">Skapa en hybridsamling</span><span class="sxs-lookup"><span data-stu-id="a78b0-116">Create a hybrid collection</span></span>](remoteapp-create-hybrid-deployment.md)
* [<span data-ttu-id="a78b0-117">Distribuera Azure RemoteApp-samling till ditt Azure-nätverk (med stöd för ExpressRoute)</span><span class="sxs-lookup"><span data-stu-id="a78b0-117">Deploy Azure RemoteApp collection to your Azure Virtual Network (with support for ExpressRoute)</span></span>](http://blogs.msdn.com/b/rds/archive/2015/04/23/deploy-azure-remoteapp-collection-to-your-azure-virtual-network-with-support-for-expressroute.aspx)


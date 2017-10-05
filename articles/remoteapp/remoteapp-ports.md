---
title: "Lista över URL: er och portar som godkända för Azure RemoteApp distribueras i kundens virtuella nätverk | Microsoft Docs"
description: "Lär dig mer om vilka portar och URL: er måste du konfigurera för kommunikation via Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: mghosh1616
manager: mbaldwin
ms.assetid: 5a001ff7-14c9-47fa-9b39-78fd5a5f0250
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: c17ff8d5441ca92f7b893edb541a1e9730c2a847
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="list-of-ports-and-urls-to-permit-access-for-azure-remoteapp-deployed-in-customer-virtual-network"></a><span data-ttu-id="040aa-103">Lista över portar och URL: er för att tillåta åtkomst för Azure RemoteApp distribueras i kundens virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="040aa-103">List of Ports and URLs to permit access for Azure RemoteApp Deployed in customer Virtual Network</span></span>
> [!IMPORTANT]
> <span data-ttu-id="040aa-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="040aa-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="040aa-105">Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.</span><span class="sxs-lookup"><span data-stu-id="040aa-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="040aa-106">Om du distribuerar en Azure RemoteApp-moln eller hybrid samling i ett virtuellt nätverk (VNET), kan du granska följande portinformationen.</span><span class="sxs-lookup"><span data-stu-id="040aa-106">If you are deploying an Azure RemoteApp cloud or hybrid collection in a virtual network (VNET), review the following port information.</span></span> <span data-ttu-id="040aa-107">Mer information om virtuella nätverk [översikt över virtuella nätverk](../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="040aa-107">For more information on virtual networks, read [Virtual Network Overview](../virtual-network/virtual-networks-overview.md).</span></span> <span data-ttu-id="040aa-108">Om du har skapat en nätverkssäkerhetsgrupp (NSG) som begränsar trafiken till virtuella resurser i samlingen, se till att följande portar är tillgänglig och tillåts via säkerhetsprinciper på det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="040aa-108">If you have created a network security group (NSG) restricting traffic to the virtual network resources in your collection, make sure the following ports are accessible and allowed through the security policies on the virtual network.</span></span> <span data-ttu-id="040aa-109">Mer information om nätverkssäkerhetsgrupper [vad är en Nätverkssäkerhetsgrupp? (NSG) ](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="040aa-109">For more information on network security groups, read [What is a Network Security Group? (NSG)](../virtual-network/virtual-networks-nsg.md).</span></span>

## <a name="azure-remoteapp-subnet-needs-access-to-these-endpoints-and-urls"></a><span data-ttu-id="040aa-110">Azure RemoteApp-undernätet måste ha åtkomst till dessa slutpunkter och URL: er:</span><span class="sxs-lookup"><span data-stu-id="040aa-110">Azure RemoteApp subnet needs access to these endpoints and URLs:</span></span>
* <span data-ttu-id="040aa-111">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="040aa-111">*.servicebus.windows.net</span></span>
* <span data-ttu-id="040aa-112">*. servicebus.net</span><span class="sxs-lookup"><span data-stu-id="040aa-112">*.servicebus.net</span></span>
* <span data-ttu-id="040aa-113">https://*.RemoteApp.windowsazure.com</span><span class="sxs-lookup"><span data-stu-id="040aa-113">https://*.remoteapp.windowsazure.com</span></span>  
* <span data-ttu-id="040aa-114">https://www.RemoteApp.windowsazure.com</span><span class="sxs-lookup"><span data-stu-id="040aa-114">https://www.remoteapp.windowsazure.com</span></span> 
* <span data-ttu-id="040aa-115">https://*RemoteApp.windowsazure.com</span><span class="sxs-lookup"><span data-stu-id="040aa-115">https://*remoteapp.windowsazure.com</span></span>  
* <span data-ttu-id="040aa-116">https://*.Core.Windows.NET</span><span class="sxs-lookup"><span data-stu-id="040aa-116">https://*.core.windows.net</span></span>  
* <span data-ttu-id="040aa-117">Utgående: TCP: TCP: 443, 9351, 9352, 10101 10175</span><span class="sxs-lookup"><span data-stu-id="040aa-117">Outbound: TCP: TCP: 443, 9351, 9352, 10101-10175</span></span> 
* <span data-ttu-id="040aa-118">Valfri – UDP: 10201 10275</span><span class="sxs-lookup"><span data-stu-id="040aa-118">Optional – UDP: 10201-10275</span></span>  

## <a name="azure-remoteapp-clients-need-access-to-these-endpoints-and-urls"></a><span data-ttu-id="040aa-119">Azure RemoteApp-klienter behöver åtkomst till dessa slutpunkter och URL: er:</span><span class="sxs-lookup"><span data-stu-id="040aa-119">Azure RemoteApp clients need access to these endpoints and URLs:</span></span>
<span data-ttu-id="040aa-120">Av klienter avses stationära datorer, enheter, etc. som användare använder för att ansluta till appar som distribuerats i Azure RemoteApp-samling.</span><span class="sxs-lookup"><span data-stu-id="040aa-120">By clients I mean the desktops, devices etc. that people use to connect to the apps deployed in the Azure RemoteApp collection.</span></span>

* <span data-ttu-id="040aa-121">https://telemetry.RemoteApp.windowsazure.com</span><span class="sxs-lookup"><span data-stu-id="040aa-121">https://telemetry.remoteapp.windowsazure.com</span></span>  
* <span data-ttu-id="040aa-122">https://*.RemoteApp.windowsazure.com (är ett valfritt UDP-portar för den här adressen)</span><span class="sxs-lookup"><span data-stu-id="040aa-122">https://*.remoteapp.windowsazure.com (the optional UDP ports are for this address)</span></span> 
* <span data-ttu-id="040aa-123">https://login.windows.net</span><span class="sxs-lookup"><span data-stu-id="040aa-123">https://login.windows.net</span></span>  
* <span data-ttu-id="040aa-124">https://login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="040aa-124">https://login.microsoftonline.com</span></span>  
* <span data-ttu-id="040aa-125">https://www.RemoteApp.windowsazure.com</span><span class="sxs-lookup"><span data-stu-id="040aa-125">https://www.remoteapp.windowsazure.com</span></span> 
* <span data-ttu-id="040aa-126">https://*.Core.Windows.NET</span><span class="sxs-lookup"><span data-stu-id="040aa-126">https://*.core.windows.net</span></span>  
* <span data-ttu-id="040aa-127">Utgående: TCP: 443</span><span class="sxs-lookup"><span data-stu-id="040aa-127">Outbound: TCP: 443</span></span>  
* <span data-ttu-id="040aa-128">Valfritt - UDP: 3391</span><span class="sxs-lookup"><span data-stu-id="040aa-128">Optional - UDP: 3391</span></span> 


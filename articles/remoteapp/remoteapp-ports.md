---
title: "aaaList för URL: er och portar toowhitelist för Azure RemoteApp distribueras i kundens virtuella nätverk | Microsoft Docs"
description: "Lär dig vilka portar och URL: er som du behöver tooconfigure för kommunikation via Azure RemoteApp."
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
ms.openlocfilehash: 039866f7b64ac763ca833d66031ade3def1d3543
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="list-of-ports-and-urls-toopermit-access-for-azure-remoteapp-deployed-in-customer-virtual-network"></a><span data-ttu-id="72932-103">Listan över URL: er och portar toopermit åtkomst för Azure RemoteApp distribueras i kundens virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="72932-103">List of Ports and URLs toopermit access for Azure RemoteApp Deployed in customer Virtual Network</span></span>
> [!IMPORTANT]
> <span data-ttu-id="72932-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="72932-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="72932-105">Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.</span><span class="sxs-lookup"><span data-stu-id="72932-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="72932-106">Om du distribuerar en Azure RemoteApp-moln eller hybrid samling i ett virtuellt nätverk (VNET), port granska hello följande information.</span><span class="sxs-lookup"><span data-stu-id="72932-106">If you are deploying an Azure RemoteApp cloud or hybrid collection in a virtual network (VNET), review hello following port information.</span></span> <span data-ttu-id="72932-107">Mer information om virtuella nätverk [översikt över virtuella nätverk](../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="72932-107">For more information on virtual networks, read [Virtual Network Overview](../virtual-network/virtual-networks-overview.md).</span></span> <span data-ttu-id="72932-108">Om du har skapat en nätverkssäkerhetsgrupp (NSG) att begränsa trafik toohello virtuella resurser i samlingen, kontrollera hello följande portar är tillgänglig och tillåts via hello säkerhetsprinciper på hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="72932-108">If you have created a network security group (NSG) restricting traffic toohello virtual network resources in your collection, make sure hello following ports are accessible and allowed through hello security policies on hello virtual network.</span></span> <span data-ttu-id="72932-109">Mer information om nätverkssäkerhetsgrupper [vad är en Nätverkssäkerhetsgrupp? (NSG) ](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="72932-109">For more information on network security groups, read [What is a Network Security Group? (NSG)](../virtual-network/virtual-networks-nsg.md).</span></span>

## <a name="azure-remoteapp-subnet-needs-access-toothese-endpoints-and-urls"></a><span data-ttu-id="72932-110">Azure undernät RemoteApp åtkomst toothese slutpunkter och URL: er:</span><span class="sxs-lookup"><span data-stu-id="72932-110">Azure RemoteApp subnet needs access toothese endpoints and URLs:</span></span>
* <span data-ttu-id="72932-111">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="72932-111">*.servicebus.windows.net</span></span>
* <span data-ttu-id="72932-112">*. servicebus.net</span><span class="sxs-lookup"><span data-stu-id="72932-112">*.servicebus.net</span></span>
* <span data-ttu-id="72932-113">https://*.RemoteApp.windowsazure.com</span><span class="sxs-lookup"><span data-stu-id="72932-113">https://*.remoteapp.windowsazure.com</span></span>  
* <span data-ttu-id="72932-114">https://www.RemoteApp.windowsazure.com</span><span class="sxs-lookup"><span data-stu-id="72932-114">https://www.remoteapp.windowsazure.com</span></span> 
* <span data-ttu-id="72932-115">https://*RemoteApp.windowsazure.com</span><span class="sxs-lookup"><span data-stu-id="72932-115">https://*remoteapp.windowsazure.com</span></span>  
* <span data-ttu-id="72932-116">https://*.Core.Windows.NET</span><span class="sxs-lookup"><span data-stu-id="72932-116">https://*.core.windows.net</span></span>  
* <span data-ttu-id="72932-117">Utgående: TCP: TCP: 443, 9351, 9352, 10101 10175</span><span class="sxs-lookup"><span data-stu-id="72932-117">Outbound: TCP: TCP: 443, 9351, 9352, 10101-10175</span></span> 
* <span data-ttu-id="72932-118">Valfri – UDP: 10201 10275</span><span class="sxs-lookup"><span data-stu-id="72932-118">Optional – UDP: 10201-10275</span></span>  

## <a name="azure-remoteapp-clients-need-access-toothese-endpoints-and-urls"></a><span data-ttu-id="72932-119">Azure RemoteApp-klienter behöver komma åt toothese slutpunkter och URL: er:</span><span class="sxs-lookup"><span data-stu-id="72932-119">Azure RemoteApp clients need access toothese endpoints and URLs:</span></span>
<span data-ttu-id="72932-120">Av klienter som avses hello stationära datorer, enheter osv som personer detta Använd tooconnect toohello appar som har distribuerats i hello-Azure RemoteApp-samling.</span><span class="sxs-lookup"><span data-stu-id="72932-120">By clients I mean hello desktops, devices etc. that people use tooconnect toohello apps deployed in hello Azure RemoteApp collection.</span></span>

* <span data-ttu-id="72932-121">https://telemetry.RemoteApp.windowsazure.com</span><span class="sxs-lookup"><span data-stu-id="72932-121">https://telemetry.remoteapp.windowsazure.com</span></span>  
* <span data-ttu-id="72932-122">https://*.RemoteApp.windowsazure.com (hello valfria UDP-portarna är för den här adressen)</span><span class="sxs-lookup"><span data-stu-id="72932-122">https://*.remoteapp.windowsazure.com (hello optional UDP ports are for this address)</span></span> 
* <span data-ttu-id="72932-123">https://login.windows.net</span><span class="sxs-lookup"><span data-stu-id="72932-123">https://login.windows.net</span></span>  
* <span data-ttu-id="72932-124">https://login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="72932-124">https://login.microsoftonline.com</span></span>  
* <span data-ttu-id="72932-125">https://www.RemoteApp.windowsazure.com</span><span class="sxs-lookup"><span data-stu-id="72932-125">https://www.remoteapp.windowsazure.com</span></span> 
* <span data-ttu-id="72932-126">https://*.Core.Windows.NET</span><span class="sxs-lookup"><span data-stu-id="72932-126">https://*.core.windows.net</span></span>  
* <span data-ttu-id="72932-127">Utgående: TCP: 443</span><span class="sxs-lookup"><span data-stu-id="72932-127">Outbound: TCP: 443</span></span>  
* <span data-ttu-id="72932-128">Valfritt - UDP: 3391</span><span class="sxs-lookup"><span data-stu-id="72932-128">Optional - UDP: 3391</span></span> 


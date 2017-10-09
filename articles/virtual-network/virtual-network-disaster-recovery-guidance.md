---
title: "aaaWhat toodo hello ett avbrott i Azure-tjänsten påverkar virtuella Azure-nätverk för händelsen | Microsoft Docs"
description: "Lär dig vilka toodo i hello händelse av ett avbrott i Azure-tjänsten påverkar virtuella Azure-nätverk."
services: virtual-network
documentationcenter: 
author: NarayanAnnamalai
manager: jefco
editor: 
ms.assetid: ad260ab9-d873-43b3-8896-f9a1db9858a5
ms.service: virtual-network
ms.workload: virtual-network
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2016
ms.author: narayan;aglick
ms.openlocfilehash: db022d2a042d255cf8ec6afb68cd8436aeecfe08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="virtual-network--business-continuity"></a><span data-ttu-id="ef0e3-103">Virtuellt nätverk – kontinuitet för företag</span><span class="sxs-lookup"><span data-stu-id="ef0e3-103">Virtual Network – Business Continuity</span></span>
## <a name="overview"></a><span data-ttu-id="ef0e3-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="ef0e3-104">Overview</span></span>
<span data-ttu-id="ef0e3-105">Ett virtuellt nätverk (VNet) är en logisk representation av nätverket i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="ef0e3-105">A Virtual Network (VNet) is a logical representation of your network in hello cloud.</span></span> <span data-ttu-id="ef0e3-106">Det gör att du toodefine egna privata IP-adress utrymme och segment hello nätverk i undernät.</span><span class="sxs-lookup"><span data-stu-id="ef0e3-106">It allows you toodefine your own private IP address space and segment hello network into subnets.</span></span> <span data-ttu-id="ef0e3-107">Vnet som fungerar som ett förtroende gräns toohost dina beräkningsresurser som Azure virtuella datorer och molntjänster (web/worker-roller).</span><span class="sxs-lookup"><span data-stu-id="ef0e3-107">VNets serves as a trust boundary toohost your compute resources such as Azure Virtual Machines and Cloud Services (web/worker roles).</span></span> <span data-ttu-id="ef0e3-108">Ett virtuellt nätverk kan direkt privata IP-kommunikation mellan hello resurser som finns i den.</span><span class="sxs-lookup"><span data-stu-id="ef0e3-108">A VNet allows direct private IP communication between hello resources hosted in it.</span></span> <span data-ttu-id="ef0e3-109">Ett virtuellt nätverk kan också vara länkade tooan lokalt nätverk via ett av hello hybrid alternativ, till exempel en VPN-Gateway eller ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="ef0e3-109">A Virtual Network can also be linked tooan on-premises network through one of hello hybrid options such as a VPN Gateway or ExpressRoute.</span></span>

<span data-ttu-id="ef0e3-110">Ett virtuellt nätverk skapas inom hello omfånget för en region.</span><span class="sxs-lookup"><span data-stu-id="ef0e3-110">A VNet is created within hello scope of a region.</span></span> <span data-ttu-id="ef0e3-111">Du kan skapa Vnet med samma adressutrymme i två olika regioner (d.v.s. oss Öst och oss Väst men det går inte att ansluta dem tooone en annan direkt).</span><span class="sxs-lookup"><span data-stu-id="ef0e3-111">You can create VNets with same address space in two different regions (i.e. US East and US West but cannot connect them tooone another directly).</span></span> 

## <a name="business-continuity"></a><span data-ttu-id="ef0e3-112">Verksamhetskontinuitet</span><span class="sxs-lookup"><span data-stu-id="ef0e3-112">Business Continuity</span></span>
<span data-ttu-id="ef0e3-113">Det kan finnas flera olika sätt att programmet kan avbrytas.</span><span class="sxs-lookup"><span data-stu-id="ef0e3-113">There could be several different ways that your application could be disrupted.</span></span> <span data-ttu-id="ef0e3-114">En viss region kan vara helt beskärs förfallodatum tooa naturkatastrof eller en partiell katastrof på grund av tooa fel på flera enheter och tjänster.</span><span class="sxs-lookup"><span data-stu-id="ef0e3-114">A given region could be completely cut off due tooa natural disaster or a partial disaster due tooa failure of multiple devices/services.</span></span> <span data-ttu-id="ef0e3-115">i alla dessa situationer skiljer hello inverkan på hello VNet-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ef0e3-115">hello impact on hello VNet service is different in each of these situations.</span></span>

<span data-ttu-id="ef0e3-116">**F: Vad gör i hello händelse av ett avbrott tooan hela region? dvs. Om en region är helt klara på grund av tooa naturkatastrof? Vad händer toohello virtuella nätverk finns i hello region?**</span><span class="sxs-lookup"><span data-stu-id="ef0e3-116">**Q: What do you do in hello event of an outage tooan entire region? i.e. if a region is completely cutoff due tooa natural disaster? What happens toohello Virtual Networks hosted in hello region?**</span></span>

<span data-ttu-id="ef0e3-117">S: hello virtuella nätverk och hello resurser i hello påverkas region förblir oåtkomlig under hello tiden för hello avbrott i tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ef0e3-117">A: hello Virtual Network and hello resources in hello affected region remains inaccessible during hello time of hello service disruption.</span></span>

![Enkel virtuella nätverksdiagram](./media/virtual-network-disaster-recovery-guidance/vnet.png)

<span data-ttu-id="ef0e3-119">**F: Vad kan jag toodo återskapa hello samma virtuella nätverk i en annan region?**</span><span class="sxs-lookup"><span data-stu-id="ef0e3-119">**Q: What can I toodo re-create hello same Virtual Network in a different region?**</span></span>

<span data-ttu-id="ef0e3-120">S: virtuella nätverk (VNet) är ganska enkel resurs.</span><span class="sxs-lookup"><span data-stu-id="ef0e3-120">A: Virtual Network (VNet) is fairly lightweight resource.</span></span> <span data-ttu-id="ef0e3-121">Du kan anropa Azure API: erna toocreate ett VNet med hello samma adressutrymmet i en annan region.</span><span class="sxs-lookup"><span data-stu-id="ef0e3-121">You can invoke Azure APIs toocreate a VNet with hello same address space in a different region.</span></span> <span data-ttu-id="ef0e3-122">toore-skapa hello samma miljö som fanns i hello påverkas region har du toomake API-anrop toore-distribuera dina molntjänster (web/worker-roller) och virtuella datorer som du hade.</span><span class="sxs-lookup"><span data-stu-id="ef0e3-122">toore-create hello same environment that was present in hello affected region, you have toomake API calls toore-deploy your Cloud Services (web/worker roles) and Virtual Machines that you had.</span></span> <span data-ttu-id="ef0e3-123">Du måste även ha toospin upp en VPN-Gateway och ansluta tooyour lokalt nätverk om du har lokala anslutningen (till exempel i en hybriddistribution).</span><span class="sxs-lookup"><span data-stu-id="ef0e3-123">You will also have toospin up a VPN Gateway and connect tooyour on-premises network if you had on-premises connectivity (such as in a hybrid deployment).</span></span>

<span data-ttu-id="ef0e3-124">hello anvisningar för att skapa ett virtuellt nätverk finns [här](virtual-networks-create-vnet-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="ef0e3-124">hello instructions for creating a VNet are found [here](virtual-networks-create-vnet-arm-pportal.md).</span></span> 

<span data-ttu-id="ef0e3-125">**F: kan en replik av ett VNet i en viss region skapas på nytt i en annan region i förväg?**</span><span class="sxs-lookup"><span data-stu-id="ef0e3-125">**Q: Can a replica of a VNet in a given region be re-created in another region ahead of time?**</span></span>

<span data-ttu-id="ef0e3-126">S: Ja, kan du skapa två Vnet med hjälp av hello samma privata IP-adressutrymme och resurser i två olika regioner före tid.</span><span class="sxs-lookup"><span data-stu-id="ef0e3-126">A: Yes, you can create two VNets using hello same private IP address space and resources in two different regions ahead of time.</span></span> <span data-ttu-id="ef0e3-127">Om en kund värd för tjänster i hello VNet mot internet, kan de har ställt in toogeo Traffic Manager-väg trafik toohello region som är aktiv.</span><span class="sxs-lookup"><span data-stu-id="ef0e3-127">If a customer was hosting internet facing services in hello VNet, they could have set up Traffic Manager toogeo-route traffic toohello region that is active.</span></span> <span data-ttu-id="ef0e3-128">Men en kund kan inte ansluta två Vnet med hello samma adress utrymme tootheir lokalt nätverk eftersom det skulle orsaka problem med routning.</span><span class="sxs-lookup"><span data-stu-id="ef0e3-128">However, a customer cannot connect two VNets with hello same address space tootheir on-premises network as it would cause routing issues.</span></span> <span data-ttu-id="ef0e3-129">Hello tiden för en katastrofåterställning och förlust av ett VNet i en region, en kund kan ansluta hello andra VNet i hello tillgängliga region med matchande adressutrymme tootheir lokalt nätverk.</span><span class="sxs-lookup"><span data-stu-id="ef0e3-129">At hello time of a disaster and loss of a VNet in one region, a customer can connect hello other VNet in hello available region with matching address space tootheir on-premises network.</span></span>

<span data-ttu-id="ef0e3-130">hello anvisningar för att skapa ett virtuellt nätverk finns [här](virtual-networks-create-vnet-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="ef0e3-130">hello instructions for creating a VNet are found [here](virtual-networks-create-vnet-arm-pportal.md).</span></span>


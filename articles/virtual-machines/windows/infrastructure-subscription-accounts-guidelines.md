---
title: "aaaSubscription konton för virtuella Windows-datorer i Azure | Microsoft Docs"
description: "Läs mer om hello viktiga design och implementeringslösning riktlinjer för prenumerationer och konton i Azure."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 761fa847-78b0-4078-a33a-d95d198d1029
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f9dc712af559b04490be1dc721a9b9f7fe9ed88f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-subscription-and-accounts-guidelines-for-windows-vms"></a><span data-ttu-id="2e7d0-103">Riktlinjer för Azure-prenumeration och konton för virtuella Windows-datorer</span><span class="sxs-lookup"><span data-stu-id="2e7d0-103">Azure subscription and accounts guidelines for Windows VMs</span></span>

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

<span data-ttu-id="2e7d0-104">Den här artikeln fokuserar på att förstå hur tooapproach prenumeration och konto för hantering av din miljö och användarbas växer.</span><span class="sxs-lookup"><span data-stu-id="2e7d0-104">This article focuses on understanding how tooapproach subscription and account management as your environment and user base grows.</span></span>

## <a name="implementation-guidelines-for-subscriptions-and-accounts"></a><span data-ttu-id="2e7d0-105">Implementeringsriktlinjer för prenumerationer och konton</span><span class="sxs-lookup"><span data-stu-id="2e7d0-105">Implementation guidelines for subscriptions and accounts</span></span>
<span data-ttu-id="2e7d0-106">Beslut:</span><span class="sxs-lookup"><span data-stu-id="2e7d0-106">Decisions:</span></span>

* <span data-ttu-id="2e7d0-107">Vilken uppsättning av prenumerationer och konton behöver du toohost din IT-arbetsbelastning eller infrastrukturen?</span><span class="sxs-lookup"><span data-stu-id="2e7d0-107">What set of subscriptions and accounts do you need toohost your IT workload or infrastructure?</span></span>
* <span data-ttu-id="2e7d0-108">Hur toobreak ned hello hierarkin toofit din organisation?</span><span class="sxs-lookup"><span data-stu-id="2e7d0-108">How toobreak down hello hierarchy toofit your organization?</span></span>

<span data-ttu-id="2e7d0-109">Aktiviteter:</span><span class="sxs-lookup"><span data-stu-id="2e7d0-109">Tasks:</span></span>

* <span data-ttu-id="2e7d0-110">Definiera logiska organisationens hierarki som du vill att toomanage från en prenumerationsnivå.</span><span class="sxs-lookup"><span data-stu-id="2e7d0-110">Define your logical organization hierarchy as you would like toomanage it from a subscription level.</span></span>
* <span data-ttu-id="2e7d0-111">toomatch den här logiska hierarkin hello konton som krävs och prenumerationer under varje konto.</span><span class="sxs-lookup"><span data-stu-id="2e7d0-111">toomatch this logical hierarchy, define hello accounts required and subscriptions under each account.</span></span>
* <span data-ttu-id="2e7d0-112">Skapa hello uppsättning prenumerationer och konton med hjälp av din namngivningskonvention.</span><span class="sxs-lookup"><span data-stu-id="2e7d0-112">Create hello set of subscriptions and accounts using your naming convention.</span></span>

## <a name="subscriptions-and-accounts"></a><span data-ttu-id="2e7d0-113">Prenumeration och konton</span><span class="sxs-lookup"><span data-stu-id="2e7d0-113">Subscriptions and accounts</span></span>
<span data-ttu-id="2e7d0-114">toowork med Azure, behöver du en eller flera Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="2e7d0-114">toowork with Azure, you need one or more Azure subscriptions.</span></span> <span data-ttu-id="2e7d0-115">Resurser som virtuella datorer (VM) eller virtuella nätverk finns i dessa prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="2e7d0-115">Resources like virtual machines (VMs) or virtual networks exist in of those subscriptions.</span></span>

* <span data-ttu-id="2e7d0-116">Enterprise-kunder har vanligtvis ett Enterprise-registreringen, vilket är hello översta resurs i hello hierarki och associerade tooone eller fler konton.</span><span class="sxs-lookup"><span data-stu-id="2e7d0-116">Enterprise customers typically have an Enterprise Enrollment, which is hello top-most resource in hello hierarchy, and is associated tooone or more accounts.</span></span>
* <span data-ttu-id="2e7d0-117">Hello översta resursen är för konsumenter och kunder utan en Enterprise-registrering hello-konto.</span><span class="sxs-lookup"><span data-stu-id="2e7d0-117">For consumers and customers without an Enterprise Enrollment, hello top-most resource is hello account.</span></span>
* <span data-ttu-id="2e7d0-118">Prenumerationer är associerade tooaccounts och det kan finnas en eller flera prenumerationer per konto.</span><span class="sxs-lookup"><span data-stu-id="2e7d0-118">Subscriptions are associated tooaccounts, and there can be one or more subscriptions per account.</span></span> <span data-ttu-id="2e7d0-119">Azure poster faktureringsinformation på hello prenumerationsnivå.</span><span class="sxs-lookup"><span data-stu-id="2e7d0-119">Azure records billing information at hello subscription level.</span></span>

<span data-ttu-id="2e7d0-120">På grund av toohello högst två hierarkinivåer i hello konto eller prenumeration på relationen är det viktigt tooalign hello namngivningskonvention konton och prenumerationer toohello fakturering behov.</span><span class="sxs-lookup"><span data-stu-id="2e7d0-120">Due toohello limit of two hierarchy levels on hello Account/Subscription relationship, it is important tooalign hello naming convention of accounts and subscriptions toohello billing needs.</span></span> <span data-ttu-id="2e7d0-121">Till exempel om ett globalt företag använder Azure, kan de välja toohave ett konto per region och har prenumerationer hanteras på hello regionsnivån:</span><span class="sxs-lookup"><span data-stu-id="2e7d0-121">For instance, if a global company uses Azure, they might choose toohave one account per region, and have subscriptions managed at hello region level:</span></span>

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub01.png)

<span data-ttu-id="2e7d0-122">Du kan exempelvis använda hello följande struktur:</span><span class="sxs-lookup"><span data-stu-id="2e7d0-122">For instance, you might use hello following structure:</span></span>

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub02.png)

<span data-ttu-id="2e7d0-123">Om en region beslutar toohave mer än en prenumeration associerade tooa viss grupp, hello namngivningskonvention bör omfatta en sätt tooencode hello extra data på hello konto eller hello prenumerationsnamn.</span><span class="sxs-lookup"><span data-stu-id="2e7d0-123">If a region decides toohave more than one subscription associated tooa particular group, hello naming convention should incorporate a way tooencode hello extra data on either hello account or hello subscription name.</span></span> <span data-ttu-id="2e7d0-124">Den här organisationen kan massaging fakturering toogenerate hello nya nivåer i hierarkin under fakturering rapporter:</span><span class="sxs-lookup"><span data-stu-id="2e7d0-124">This organization allows massaging billing data toogenerate hello new levels of hierarchy during billing reports:</span></span>

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub03.png)

<span data-ttu-id="2e7d0-125">hello organisation kan se ut som följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="2e7d0-125">hello organization could look like hello following example:</span></span>

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub04.png)

<span data-ttu-id="2e7d0-126">Vi ger detaljerad fakturering via en hämtningsbar fil för ett enskilt konto eller för alla konton i ett enterprise-avtal.</span><span class="sxs-lookup"><span data-stu-id="2e7d0-126">We provide detailed billing via a downloadable file for a single account, or for all accounts in an enterprise agreement.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2e7d0-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2e7d0-127">Next steps</span></span>
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]


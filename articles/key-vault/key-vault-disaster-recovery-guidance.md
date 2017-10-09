---
title: "aaaWhat toodo hello ett avbrott i Azure-tjänsten som påverkar Azure Key Vault för händelsen | Microsoft Docs"
description: "Lär dig vilka toodo i ett avbrott i Azure-tjänsten som påverkar Azure Key Vault hello-händelse."
services: key-vault
documentationcenter: 
author: adamglick
manager: mbaldwin
editor: 
ms.assetid: 19a9af63-3032-447b-9d1a-b0125f384edb
ms.service: key-vault
ms.workload: key-vault
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: sumedhb;aglick
ms.openlocfilehash: 88eec82ada401a28323b3eea126168185ba4cdb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-availability-and-redundancy"></a><span data-ttu-id="6642d-103">Azure Key Vault tillgänglighet och redundans</span><span class="sxs-lookup"><span data-stu-id="6642d-103">Azure Key Vault availability and redundancy</span></span>
<span data-ttu-id="6642d-104">Azure Key Vault har flera lager för redundans toomake till att dina nycklar och hemligheter förblir tillgängliga tooyour programmet även om enskilda komponenter i hello tjänsten misslyckas.</span><span class="sxs-lookup"><span data-stu-id="6642d-104">Azure Key Vault features multiple layers of redundancy toomake sure that your keys and secrets remain available tooyour application even if individual components of hello service fail.</span></span>

<span data-ttu-id="6642d-105">hello innehållet i nyckelvalvet replikeras inom hello och tooa sekundär region minst 150 mil bort men inom hello samma geografisk plats.</span><span class="sxs-lookup"><span data-stu-id="6642d-105">hello contents of your key vault are replicated within hello region and tooa secondary region at least 150 miles away but within hello same geography.</span></span> <span data-ttu-id="6642d-106">Detta upprätthåller en hög hållbarhet för dina nycklar och hemligheter.</span><span class="sxs-lookup"><span data-stu-id="6642d-106">This maintains high durability of your keys and secrets.</span></span> <span data-ttu-id="6642d-107">Se hello [Azure länkas regioner](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions) dokumentet för mer information om specifika region par.</span><span class="sxs-lookup"><span data-stu-id="6642d-107">See hello [Azure paired regions](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions) document for details on specific region pairs.</span></span>

<span data-ttu-id="6642d-108">Om enskilda komponenter i hello key vault-tjänsten misslyckas, steg alternativa komponenter inom hello region i tooserve din begäran toomake till att det finns ingen försämring av funktioner.</span><span class="sxs-lookup"><span data-stu-id="6642d-108">If individual components within hello key vault service fail, alternate components within hello region step in tooserve your request toomake sure that there is no degradation of functionality.</span></span> <span data-ttu-id="6642d-109">Du behöver inte tootake någon åtgärd tootrigger detta.</span><span class="sxs-lookup"><span data-stu-id="6642d-109">You do not need tootake any action tootrigger this.</span></span> <span data-ttu-id="6642d-110">Det sker automatiskt och blir transparent tooyou.</span><span class="sxs-lookup"><span data-stu-id="6642d-110">It happens automatically and will be transparent tooyou.</span></span>

<span data-ttu-id="6642d-111">I hello sällsynt händelse att en hela Azure-region är tillgängligt, hello-begäranden som du gör av Azure Key Vault i regionen dirigeras automatiskt (*redundansväxlats*) tooa sekundär region.</span><span class="sxs-lookup"><span data-stu-id="6642d-111">In hello rare event that an entire Azure region is unavailable, hello requests that you make of Azure Key Vault in that region are automatically routed (*failed over*) tooa secondary region.</span></span> <span data-ttu-id="6642d-112">När hello primära regionen är tillgänglig igen begäranden dirigeras tillbaka (*misslyckades tillbaka*) toohello primär region.</span><span class="sxs-lookup"><span data-stu-id="6642d-112">When hello primary region is available again, requests are routed back (*failed back*) toohello primary region.</span></span> <span data-ttu-id="6642d-113">Igen och behöver du inte tootake någon åtgärd eftersom detta sker automatiskt.</span><span class="sxs-lookup"><span data-stu-id="6642d-113">Again, you do not need tootake any action because this happens automatically.</span></span>

<span data-ttu-id="6642d-114">Det finns några varningar toobe medveten om:</span><span class="sxs-lookup"><span data-stu-id="6642d-114">There are a few caveats toobe aware of:</span></span>

* <span data-ttu-id="6642d-115">Det kan ta några minuter för hello service toofail i hello-händelsen för en region växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="6642d-115">In hello event of a region failover, it may take a few minutes for hello service toofail over.</span></span> <span data-ttu-id="6642d-116">Begäranden som görs under den här tiden kan misslyckas tills hello redundansväxlingen är klar.</span><span class="sxs-lookup"><span data-stu-id="6642d-116">Requests that are made during this time may fail until hello failover completes.</span></span>
* <span data-ttu-id="6642d-117">Efter en växling vid fel är nyckelvalvet i skrivskyddat läge.</span><span class="sxs-lookup"><span data-stu-id="6642d-117">After a failover is complete, your key vault is in read-only mode.</span></span> <span data-ttu-id="6642d-118">Begäranden som stöds i det här läget är:</span><span class="sxs-lookup"><span data-stu-id="6642d-118">Requests that are supported in this mode are:</span></span>
  * <span data-ttu-id="6642d-119">Lista nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="6642d-119">List key vaults</span></span>
  * <span data-ttu-id="6642d-120">Hämta egenskaper för nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="6642d-120">Get properties of key vaults</span></span>
  * <span data-ttu-id="6642d-121">Lista hemligheter</span><span class="sxs-lookup"><span data-stu-id="6642d-121">List secrets</span></span>
  * <span data-ttu-id="6642d-122">Hämta hemligheter</span><span class="sxs-lookup"><span data-stu-id="6642d-122">Get secrets</span></span>
  * <span data-ttu-id="6642d-123">Lista nycklar</span><span class="sxs-lookup"><span data-stu-id="6642d-123">List keys</span></span>
  * <span data-ttu-id="6642d-124">Hämta nycklar (egenskaperna för)</span><span class="sxs-lookup"><span data-stu-id="6642d-124">Get (properties of) keys</span></span>
  * <span data-ttu-id="6642d-125">Kryptera</span><span class="sxs-lookup"><span data-stu-id="6642d-125">Encrypt</span></span>
  * <span data-ttu-id="6642d-126">Dekryptera</span><span class="sxs-lookup"><span data-stu-id="6642d-126">Decrypt</span></span>
  * <span data-ttu-id="6642d-127">Omsluta</span><span class="sxs-lookup"><span data-stu-id="6642d-127">Wrap</span></span>
  * <span data-ttu-id="6642d-128">Packa upp</span><span class="sxs-lookup"><span data-stu-id="6642d-128">Unwrap</span></span>
  * <span data-ttu-id="6642d-129">Verifiera</span><span class="sxs-lookup"><span data-stu-id="6642d-129">Verify</span></span>
  * <span data-ttu-id="6642d-130">Logga</span><span class="sxs-lookup"><span data-stu-id="6642d-130">Sign</span></span>
  * <span data-ttu-id="6642d-131">Säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="6642d-131">Backup</span></span>
* <span data-ttu-id="6642d-132">Efter en redundansväxling har misslyckats tillbaka, alla typer av begäranden (inklusive Läs *och* skrivåtgärder) är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="6642d-132">After a failover is failed back, all request types (including read *and* write requests) are available.</span></span>


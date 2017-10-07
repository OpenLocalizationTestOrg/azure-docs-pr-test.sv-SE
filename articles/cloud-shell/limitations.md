---
title: "begränsningar för aaaAzure moln Shell (förhandsversion) | Microsoft Docs"
description: "Översikt över begränsningar i Azure Cloud Shell"
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: juluk
ms.openlocfilehash: 8462b0b9850fcde790a386433009439bbab52c0f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="limitations-of-azure-cloud-shell"></a><span data-ttu-id="98d91-103">Begränsningar i Azure-molnet Shell</span><span class="sxs-lookup"><span data-stu-id="98d91-103">Limitations of Azure Cloud Shell</span></span>
<span data-ttu-id="98d91-104">Azure Cloud Shell har hello följande kända begränsningar:</span><span class="sxs-lookup"><span data-stu-id="98d91-104">Azure Cloud Shell has hello following known limitations:</span></span>

## <a name="system-state-and-persistence"></a><span data-ttu-id="98d91-105">Systemtillstånd och beständiga</span><span class="sxs-lookup"><span data-stu-id="98d91-105">System state and persistence</span></span>
<span data-ttu-id="98d91-106">hello-dator som innehåller molnet Shell sessionen är temporär och den återanvänds när sessionen har varit inaktiv i 20 minuter.</span><span class="sxs-lookup"><span data-stu-id="98d91-106">hello machine that provides your Cloud Shell session is temporary, and it is recycled after your session is inactive for 20 minutes.</span></span> <span data-ttu-id="98d91-107">Molnet Shell kräver en filresursen toobe monterade.</span><span class="sxs-lookup"><span data-stu-id="98d91-107">Cloud Shell requires a file share toobe mounted.</span></span> <span data-ttu-id="98d91-108">Din prenumeration måste därför vara kan tooset in lagring resurser tooaccess moln Shell.</span><span class="sxs-lookup"><span data-stu-id="98d91-108">As a result, your subscription must be able tooset up storage resources tooaccess Cloud Shell.</span></span> <span data-ttu-id="98d91-109">Andra överväganden omfattar:</span><span class="sxs-lookup"><span data-stu-id="98d91-109">Other considerations include:</span></span>
* <span data-ttu-id="98d91-110">Med monterade storage kan endast ändringar i din `$Home` directory eller `clouddrive` directory sparas.</span><span class="sxs-lookup"><span data-stu-id="98d91-110">With mounted storage, only modifications within your `$Home` directory or `clouddrive` directory are persisted.</span></span>
* <span data-ttu-id="98d91-111">Filresurser kan monteras endast från din [tilldelade region](persisting-shell-storage.md#mount-a-new-clouddrive).</span><span class="sxs-lookup"><span data-stu-id="98d91-111">File shares can be mounted only from within your [assigned region](persisting-shell-storage.md#mount-a-new-clouddrive).</span></span>
* <span data-ttu-id="98d91-112">Azure Files stöder endast lokalt redundant lagring och konton för geo-redundant lagring.</span><span class="sxs-lookup"><span data-stu-id="98d91-112">Azure Files supports only locally redundant storage and geo-redundant storage accounts.</span></span>

## <a name="user-permissions"></a><span data-ttu-id="98d91-113">Användarbehörigheter</span><span class="sxs-lookup"><span data-stu-id="98d91-113">User permissions</span></span>
<span data-ttu-id="98d91-114">Behörigheterna anges som en vanlig användare utan åtkomst till sudo.</span><span class="sxs-lookup"><span data-stu-id="98d91-114">Permissions are set as regular users without sudo access.</span></span> <span data-ttu-id="98d91-115">En installation utanför din `$Home` katalog kommer inte att spara.</span><span class="sxs-lookup"><span data-stu-id="98d91-115">Any installation outside your `$Home` directory will not persist.</span></span>
<span data-ttu-id="98d91-116">Även om vissa kommandon i hello `clouddrive` katalogen som `git clone`, har inte rätt behörighet din `$Home` directory har behörighet.</span><span class="sxs-lookup"><span data-stu-id="98d91-116">Although certain commands within hello `clouddrive` directory, such as `git clone`, do not have proper permissions, your `$Home` directory does have permissions.</span></span>

## <a name="browser-support"></a><span data-ttu-id="98d91-117">Stöd för webbläsare</span><span class="sxs-lookup"><span data-stu-id="98d91-117">Browser support</span></span>
<span data-ttu-id="98d91-118">Molnet stödjer hello senaste versionerna av Microsoft Edge, Microsoft Internet Explorer, Google Chrome, Mozilla Firefox och Apple Safari.</span><span class="sxs-lookup"><span data-stu-id="98d91-118">Cloud Shell supports hello latest versions of Microsoft Edge, Microsoft Internet Explorer, Google Chrome, Mozilla Firefox, and Apple Safari.</span></span> <span data-ttu-id="98d91-119">Safari i privat läge stöds inte.</span><span class="sxs-lookup"><span data-stu-id="98d91-119">Safari in private mode is not supported.</span></span>

## <a name="copy-and-paste"></a><span data-ttu-id="98d91-120">Kopiera och klistra in</span><span class="sxs-lookup"><span data-stu-id="98d91-120">Copy and paste</span></span>
<span data-ttu-id="98d91-121">CTRL + C och Ctrl + V fungerar inte som att kopiera och klistra in genvägar i molnet Shell på Windows-datorer kan använda Ctrl + Insert och SKIFT + Insert toocopy och klistra in respektive.</span><span class="sxs-lookup"><span data-stu-id="98d91-121">Ctrl+C and Ctrl+V do not function as copy/paste shortcuts in Cloud Shell on Windows machines, use Ctrl+Insert and Shift+Insert toocopy and paste respectively.</span></span>

<span data-ttu-id="98d91-122">Högerklicka på Kopiera och klistra in alternativ är också tillgängliga, men genom att högerklicka på funktion är ämne toobrowser-specifika Urklipp åtkomst.</span><span class="sxs-lookup"><span data-stu-id="98d91-122">Right-click copy-and-paste options are also available, but right-click function is subject toobrowser-specific clipboard access.</span></span>

## <a name="editing-bashrc"></a><span data-ttu-id="98d91-123">Redigera .bashrc</span><span class="sxs-lookup"><span data-stu-id="98d91-123">Editing .bashrc</span></span>
<span data-ttu-id="98d91-124">Vara försiktig när du redigerar .bashrc, gör det kan orsaka oväntade fel i moln Shell.</span><span class="sxs-lookup"><span data-stu-id="98d91-124">Take caution when editing .bashrc, doing so can cause unexpected errors in Cloud Shell.</span></span>

## <a name="bashhistory"></a><span data-ttu-id="98d91-125">.bash_history</span><span class="sxs-lookup"><span data-stu-id="98d91-125">.bash_history</span></span>
<span data-ttu-id="98d91-126">Historik för bash kommandon kan vara inkonsekvent på grund av avbrott i molnet Shell session eller samtidiga sessioner.</span><span class="sxs-lookup"><span data-stu-id="98d91-126">Your history of bash commands may be inconsistent because of Cloud Shell session disruption or concurrent sessions.</span></span>

## <a name="usage-limits"></a><span data-ttu-id="98d91-127">Gränserna för Resursanvändning</span><span class="sxs-lookup"><span data-stu-id="98d91-127">Usage limits</span></span>
<span data-ttu-id="98d91-128">Moln-gränssnittet är avsedd för interaktiva användningsfall.</span><span class="sxs-lookup"><span data-stu-id="98d91-128">Cloud Shell is intended for interactive use cases.</span></span> <span data-ttu-id="98d91-129">Därför kan avslutas alla icke-interaktiv tidskrävande sessioner utan varning.</span><span class="sxs-lookup"><span data-stu-id="98d91-129">As a result, any long-running non-interactive sessions are ended without warning.</span></span>

## <a name="network-connectivity"></a><span data-ttu-id="98d91-130">Nätverksanslutning</span><span class="sxs-lookup"><span data-stu-id="98d91-130">Network connectivity</span></span>
<span data-ttu-id="98d91-131">En fördröjning i molnet Shell ämne toolocal Internetanslutning, molnet Shell fortsätter tooattempt toocarry ut instruktionerna som skickas.</span><span class="sxs-lookup"><span data-stu-id="98d91-131">Any latency in Cloud Shell is subject toolocal internet connectivity, Cloud Shell continues tooattempt toocarry out any instructions sent.</span></span>

## <a name="next-steps"></a><span data-ttu-id="98d91-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="98d91-132">Next steps</span></span>
[<span data-ttu-id="98d91-133">Molnet Shell Snabbstart</span><span class="sxs-lookup"><span data-stu-id="98d91-133">Cloud Shell Quickstart</span></span>](quickstart.md)

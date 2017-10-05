---
title: "Azure AD Connect Synchronization Service Manager-åtgärder | Microsoft Docs"
description: "Förstå fliken åtgärder i hanteraren för synkroniseringstjänsten för Azure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 97a26565-618f-4313-8711-5925eeb47cdc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a1475e4fcd11eb008badba49665f4af6029a1697
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="using-the-sync-service-manager-operations-tab"></a><span data-ttu-id="06f89-103">På fliken synkronisering Service Manager-åtgärder</span><span class="sxs-lookup"><span data-stu-id="06f89-103">Using the Sync Service Manager Operations tab</span></span>

![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/operations.png)

<span data-ttu-id="06f89-105">Fliken åtgärder visar resultaten från de senaste åtgärderna.</span><span class="sxs-lookup"><span data-stu-id="06f89-105">The operations tab shows the results from the most recent operations.</span></span> <span data-ttu-id="06f89-106">Den här fliken är för att förstå och felsöka problem.</span><span class="sxs-lookup"><span data-stu-id="06f89-106">This tab is key to understand and troubleshoot issues.</span></span>

## <a name="understand-the-information-visible-in-the-operations-tab"></a><span data-ttu-id="06f89-107">Förstå informationen som visas på fliken åtgärder</span><span class="sxs-lookup"><span data-stu-id="06f89-107">Understand the information visible in the operations tab</span></span>
<span data-ttu-id="06f89-108">Den övre delen visas alla körs i kronologisk ordning.</span><span class="sxs-lookup"><span data-stu-id="06f89-108">The top half shows all runs in chronological order.</span></span> <span data-ttu-id="06f89-109">Som standard åtgärderna logga behåller information om de senaste sju dagarna, men den här inställningen kan ändras med den [scheduler](active-directory-aadconnectsync-feature-scheduler.md).</span><span class="sxs-lookup"><span data-stu-id="06f89-109">By default, the operations log keeps information about the last seven days, but this setting can be changed with the [scheduler](active-directory-aadconnectsync-feature-scheduler.md).</span></span> <span data-ttu-id="06f89-110">Du vill söka efter alla kör som inte visar statusen lyckades.</span><span class="sxs-lookup"><span data-stu-id="06f89-110">You want to look for any run that does not show a success status.</span></span> <span data-ttu-id="06f89-111">Du kan ändra sortering genom att klicka på rubrikerna.</span><span class="sxs-lookup"><span data-stu-id="06f89-111">You can change the sorting by clicking the headers.</span></span>

<span data-ttu-id="06f89-112">Den **Status** kolumnen är den viktigaste informationen och visar de svåraste problemet för en körning.</span><span class="sxs-lookup"><span data-stu-id="06f89-112">The **Status** column is the most important information and shows the most severe problem for a run.</span></span> <span data-ttu-id="06f89-113">Här är en kort sammanfattning av de vanligaste status i prioritetsordning att undersöka (där * ange flera möjliga felsträngar).</span><span class="sxs-lookup"><span data-stu-id="06f89-113">Here is a quick summary of the most common statuses in order of priority to investigate (where * indicate several possible error strings).</span></span>

| <span data-ttu-id="06f89-114">Status</span><span class="sxs-lookup"><span data-stu-id="06f89-114">Status</span></span> | <span data-ttu-id="06f89-115">Kommentar</span><span class="sxs-lookup"><span data-stu-id="06f89-115">Comment</span></span> |
| --- | --- |
| <span data-ttu-id="06f89-116">Stoppad-*</span><span class="sxs-lookup"><span data-stu-id="06f89-116">stopped-*</span></span> |<span data-ttu-id="06f89-117">Kör kunde inte slutföras.</span><span class="sxs-lookup"><span data-stu-id="06f89-117">The run could not complete.</span></span> <span data-ttu-id="06f89-118">Till exempel om fjärrdatorn är igång och kan inte kontaktas.</span><span class="sxs-lookup"><span data-stu-id="06f89-118">For example, if the remote system is down and cannot be contacted.</span></span> |
| <span data-ttu-id="06f89-119">stoppats felgränsen</span><span class="sxs-lookup"><span data-stu-id="06f89-119">stopped-error-limit</span></span> |<span data-ttu-id="06f89-120">Det finns fler än 5 000 fel.</span><span class="sxs-lookup"><span data-stu-id="06f89-120">There are more than 5,000 errors.</span></span> <span data-ttu-id="06f89-121">Kör har automatiskt stoppats på grund av det stora antalet fel.</span><span class="sxs-lookup"><span data-stu-id="06f89-121">The run was automatically stopped due to the large number of errors.</span></span> |
| <span data-ttu-id="06f89-122">slutförda -\*-fel</span><span class="sxs-lookup"><span data-stu-id="06f89-122">completed-\*-errors</span></span> |<span data-ttu-id="06f89-123">Kör slutfördes, men det finns fel (färre än 5 000) som bör undersökas.</span><span class="sxs-lookup"><span data-stu-id="06f89-123">The run completed, but there are errors (fewer than 5,000) that should be investigated.</span></span> |
| <span data-ttu-id="06f89-124">slutförda -\*-varningar</span><span class="sxs-lookup"><span data-stu-id="06f89-124">completed-\*-warnings</span></span> |<span data-ttu-id="06f89-125">Kör slutfördes, men vissa data är inte i det förväntade tillståndet.</span><span class="sxs-lookup"><span data-stu-id="06f89-125">The run completed, but some data is not in the expected state.</span></span> <span data-ttu-id="06f89-126">Om du har fel sedan är det här meddelandet vanligtvis bara ett symtom.</span><span class="sxs-lookup"><span data-stu-id="06f89-126">If you have errors, then this message is usually only a symptom.</span></span> <span data-ttu-id="06f89-127">Du bör inte undersöka varningar förrän du har åtgärdat felen.</span><span class="sxs-lookup"><span data-stu-id="06f89-127">Until you have addressed errors, you should not investigate warnings.</span></span> |
| <span data-ttu-id="06f89-128">lyckades</span><span class="sxs-lookup"><span data-stu-id="06f89-128">success</span></span> |<span data-ttu-id="06f89-129">Inga problem.</span><span class="sxs-lookup"><span data-stu-id="06f89-129">No issues.</span></span> |

<span data-ttu-id="06f89-130">När du har valt en rad uppdateras längst ned om du vill visa detaljer för som kör.</span><span class="sxs-lookup"><span data-stu-id="06f89-130">When you select a row, the bottom updates to show the details of that run.</span></span> <span data-ttu-id="06f89-131">Du kanske har en lista som säger att längst till vänster i längst ned, **steg #**.</span><span class="sxs-lookup"><span data-stu-id="06f89-131">To the far left of the bottom, you might have a list saying **Step #**.</span></span> <span data-ttu-id="06f89-132">Den här listan visas bara om du har flera domäner i skogen där varje domän som representeras av ett steg.</span><span class="sxs-lookup"><span data-stu-id="06f89-132">This list only appears if you have multiple domains in your forest where each domain is represented by a step.</span></span> <span data-ttu-id="06f89-133">Domännamnet finns under rubriken **Partition**.</span><span class="sxs-lookup"><span data-stu-id="06f89-133">The domain name can be found under the heading **Partition**.</span></span> <span data-ttu-id="06f89-134">Under **Synkroniseringsstatistik**, du kan hitta mer information om antalet ändringar som har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="06f89-134">Under **Synchronization Statistics**, you can find more information about the number of changes that were processed.</span></span> <span data-ttu-id="06f89-135">Du kan klicka på länkarna för att hämta en lista över de ändrade objekt.</span><span class="sxs-lookup"><span data-stu-id="06f89-135">You can click the links to get a list of the changed objects.</span></span> <span data-ttu-id="06f89-136">Om du har objekt med fel felen visas **synkroniseringsfel**.</span><span class="sxs-lookup"><span data-stu-id="06f89-136">If you have objects with errors, those errors show up under **Synchronization Errors**.</span></span>

<span data-ttu-id="06f89-137">Mer information finns i [felsökning av ett objekt som inte synkroniseras](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md)</span><span class="sxs-lookup"><span data-stu-id="06f89-137">For more information, see [troubleshoot an object that is not synchronizing](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md)</span></span>

## <a name="next-steps"></a><span data-ttu-id="06f89-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="06f89-138">Next steps</span></span>
<span data-ttu-id="06f89-139">Lär dig mer om den [Azure AD Connect-synkronisering](active-directory-aadconnectsync-whatis.md) konfiguration.</span><span class="sxs-lookup"><span data-stu-id="06f89-139">Learn more about the [Azure AD Connect sync](active-directory-aadconnectsync-whatis.md) configuration.</span></span>

<span data-ttu-id="06f89-140">Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="06f89-140">Learn more about [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>

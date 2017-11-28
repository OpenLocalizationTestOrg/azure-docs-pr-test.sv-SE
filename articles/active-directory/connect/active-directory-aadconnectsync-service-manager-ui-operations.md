---
title: "Azure AD Connect Synchronization Service Manager-åtgärder | Microsoft Docs"
description: "Förstå hello Operations-fliken i hello Synchronization Service Manager för Azure AD Connect."
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
ms.openlocfilehash: decbc53613d456a71cd116c40c5e1fd761efd4af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-sync-service-manager-operations-tab"></a><span data-ttu-id="1d2ac-103">Med hjälp av hello fliken synkronisering Service Manager-åtgärder</span><span class="sxs-lookup"><span data-stu-id="1d2ac-103">Using hello Sync Service Manager Operations tab</span></span>

![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/operations.png)

<span data-ttu-id="1d2ac-105">hello visar operations hello resultaten från de senaste hello-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="1d2ac-105">hello operations tab shows hello results from hello most recent operations.</span></span> <span data-ttu-id="1d2ac-106">Den här fliken är viktiga toounderstand och felsöka problem.</span><span class="sxs-lookup"><span data-stu-id="1d2ac-106">This tab is key toounderstand and troubleshoot issues.</span></span>

## <a name="understand-hello-information-visible-in-hello-operations-tab"></a><span data-ttu-id="1d2ac-107">Förstå hello information visas i hello operations fliken</span><span class="sxs-lookup"><span data-stu-id="1d2ac-107">Understand hello information visible in hello operations tab</span></span>
<span data-ttu-id="1d2ac-108">hello övre delen visas alla körs i kronologisk ordning.</span><span class="sxs-lookup"><span data-stu-id="1d2ac-108">hello top half shows all runs in chronological order.</span></span> <span data-ttu-id="1d2ac-109">Som standard hello operations loggen sparas information om hello senaste sju dagarna, men den här inställningen kan ändras med hello [scheduler](active-directory-aadconnectsync-feature-scheduler.md).</span><span class="sxs-lookup"><span data-stu-id="1d2ac-109">By default, hello operations log keeps information about hello last seven days, but this setting can be changed with hello [scheduler](active-directory-aadconnectsync-feature-scheduler.md).</span></span> <span data-ttu-id="1d2ac-110">Vill du toolook för alla kör som inte visar statusen lyckades.</span><span class="sxs-lookup"><span data-stu-id="1d2ac-110">You want toolook for any run that does not show a success status.</span></span> <span data-ttu-id="1d2ac-111">Du kan ändra hello sortering genom att klicka på hello-huvuden.</span><span class="sxs-lookup"><span data-stu-id="1d2ac-111">You can change hello sorting by clicking hello headers.</span></span>

<span data-ttu-id="1d2ac-112">Hej **Status** kolumnen är hello viktigaste informationen och visar hello mest allvarliga problem för en körning.</span><span class="sxs-lookup"><span data-stu-id="1d2ac-112">hello **Status** column is hello most important information and shows hello most severe problem for a run.</span></span> <span data-ttu-id="1d2ac-113">Här är en kort sammanfattning av hello vanligaste status efter prioritet tooinvestigate (där * ange flera möjliga felsträngar).</span><span class="sxs-lookup"><span data-stu-id="1d2ac-113">Here is a quick summary of hello most common statuses in order of priority tooinvestigate (where * indicate several possible error strings).</span></span>

| <span data-ttu-id="1d2ac-114">Status</span><span class="sxs-lookup"><span data-stu-id="1d2ac-114">Status</span></span> | <span data-ttu-id="1d2ac-115">Kommentar</span><span class="sxs-lookup"><span data-stu-id="1d2ac-115">Comment</span></span> |
| --- | --- |
| <span data-ttu-id="1d2ac-116">Stoppad-*</span><span class="sxs-lookup"><span data-stu-id="1d2ac-116">stopped-*</span></span> |<span data-ttu-id="1d2ac-117">hello kör kunde inte slutföras.</span><span class="sxs-lookup"><span data-stu-id="1d2ac-117">hello run could not complete.</span></span> <span data-ttu-id="1d2ac-118">Till exempel om hello fjärrsystemet är inte tillgänglig och kan inte kontaktas.</span><span class="sxs-lookup"><span data-stu-id="1d2ac-118">For example, if hello remote system is down and cannot be contacted.</span></span> |
| <span data-ttu-id="1d2ac-119">stoppats felgränsen</span><span class="sxs-lookup"><span data-stu-id="1d2ac-119">stopped-error-limit</span></span> |<span data-ttu-id="1d2ac-120">Det finns fler än 5 000 fel.</span><span class="sxs-lookup"><span data-stu-id="1d2ac-120">There are more than 5,000 errors.</span></span> <span data-ttu-id="1d2ac-121">hello kör stoppades automatiskt på grund av toohello många fel.</span><span class="sxs-lookup"><span data-stu-id="1d2ac-121">hello run was automatically stopped due toohello large number of errors.</span></span> |
| <span data-ttu-id="1d2ac-122">slutförda -\*-fel</span><span class="sxs-lookup"><span data-stu-id="1d2ac-122">completed-\*-errors</span></span> |<span data-ttu-id="1d2ac-123">hello kör slutfördes, men det finns fel (färre än 5 000) som bör undersökas.</span><span class="sxs-lookup"><span data-stu-id="1d2ac-123">hello run completed, but there are errors (fewer than 5,000) that should be investigated.</span></span> |
| <span data-ttu-id="1d2ac-124">slutförda -\*-varningar</span><span class="sxs-lookup"><span data-stu-id="1d2ac-124">completed-\*-warnings</span></span> |<span data-ttu-id="1d2ac-125">hello kör slutfördes, men vissa data är inte i hello förväntat tillstånd.</span><span class="sxs-lookup"><span data-stu-id="1d2ac-125">hello run completed, but some data is not in hello expected state.</span></span> <span data-ttu-id="1d2ac-126">Om du har fel sedan är det här meddelandet vanligtvis bara ett symtom.</span><span class="sxs-lookup"><span data-stu-id="1d2ac-126">If you have errors, then this message is usually only a symptom.</span></span> <span data-ttu-id="1d2ac-127">Du bör inte undersöka varningar förrän du har åtgärdat felen.</span><span class="sxs-lookup"><span data-stu-id="1d2ac-127">Until you have addressed errors, you should not investigate warnings.</span></span> |
| <span data-ttu-id="1d2ac-128">lyckades</span><span class="sxs-lookup"><span data-stu-id="1d2ac-128">success</span></span> |<span data-ttu-id="1d2ac-129">Inga problem.</span><span class="sxs-lookup"><span data-stu-id="1d2ac-129">No issues.</span></span> |

<span data-ttu-id="1d2ac-130">När du har valt en rad uppdateras hello nedre tooshow hello information om som körs.</span><span class="sxs-lookup"><span data-stu-id="1d2ac-130">When you select a row, hello bottom updates tooshow hello details of that run.</span></span> <span data-ttu-id="1d2ac-131">toohello längst till vänster i hello längst ned, kanske du har en lista som säger **steg #**.</span><span class="sxs-lookup"><span data-stu-id="1d2ac-131">toohello far left of hello bottom, you might have a list saying **Step #**.</span></span> <span data-ttu-id="1d2ac-132">Den här listan visas bara om du har flera domäner i skogen där varje domän som representeras av ett steg.</span><span class="sxs-lookup"><span data-stu-id="1d2ac-132">This list only appears if you have multiple domains in your forest where each domain is represented by a step.</span></span> <span data-ttu-id="1d2ac-133">hello domännamn finns under rubriken hello **Partition**.</span><span class="sxs-lookup"><span data-stu-id="1d2ac-133">hello domain name can be found under hello heading **Partition**.</span></span> <span data-ttu-id="1d2ac-134">Under **Synkroniseringsstatistik**, du kan hitta mer information om hello antalet ändringar som har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="1d2ac-134">Under **Synchronization Statistics**, you can find more information about hello number of changes that were processed.</span></span> <span data-ttu-id="1d2ac-135">Du kan klicka på hello länkar tooget en lista över hello ändras objekt.</span><span class="sxs-lookup"><span data-stu-id="1d2ac-135">You can click hello links tooget a list of hello changed objects.</span></span> <span data-ttu-id="1d2ac-136">Om du har objekt med fel felen visas **synkroniseringsfel**.</span><span class="sxs-lookup"><span data-stu-id="1d2ac-136">If you have objects with errors, those errors show up under **Synchronization Errors**.</span></span>

<span data-ttu-id="1d2ac-137">Mer information finns i [felsökning av ett objekt som inte synkroniseras](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md)</span><span class="sxs-lookup"><span data-stu-id="1d2ac-137">For more information, see [troubleshoot an object that is not synchronizing](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md)</span></span>

## <a name="next-steps"></a><span data-ttu-id="1d2ac-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1d2ac-138">Next steps</span></span>
<span data-ttu-id="1d2ac-139">Mer information om hello [Azure AD Connect-synkronisering](active-directory-aadconnectsync-whatis.md) konfiguration.</span><span class="sxs-lookup"><span data-stu-id="1d2ac-139">Learn more about hello [Azure AD Connect sync](active-directory-aadconnectsync-whatis.md) configuration.</span></span>

<span data-ttu-id="1d2ac-140">Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="1d2ac-140">Learn more about [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>

---
title: 'Azure AD Connect: Funktioner i preview | Microsoft Docs'
description: "Det här avsnittet beskrivs i mer detalj som finns i förhandsgranskningen i Azure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: c75cd8cf-3eff-4619-bbca-66276757cc07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: bcfc710861b19d8f86f094ced0d1c691e0911f08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="more-details-about-features-in-preview"></a><span data-ttu-id="af3cc-103">Mer information om funktionerna i förhandsversionen</span><span class="sxs-lookup"><span data-stu-id="af3cc-103">More details about features in preview</span></span>
<span data-ttu-id="af3cc-104">Det här avsnittet beskrivs hur toouse funktioner för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="af3cc-104">This topic describes how toouse features currently in preview.</span></span>

## <a name="group-writeback"></a><span data-ttu-id="af3cc-105">Tillbakaskrivning av grupp</span><span class="sxs-lookup"><span data-stu-id="af3cc-105">Group writeback</span></span>
<span data-ttu-id="af3cc-106">hello-alternativet för tillbakaskrivning av grupp i valfria funktioner kan du toowriteback **Office 365-grupper** tooa skog med Exchange installerad.</span><span class="sxs-lookup"><span data-stu-id="af3cc-106">hello option for group writeback in optional features allows you toowriteback **Office 365 Groups** tooa forest with Exchange installed.</span></span> <span data-ttu-id="af3cc-107">Det här är en grupp som alltid lärt dig i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="af3cc-107">This is a group that is always mastered in hello cloud.</span></span> <span data-ttu-id="af3cc-108">Om du har Exchange lokalt kan sedan du skriva tillbaka dessa grupper tooon lokala så att användare med en lokal Exchange-postlåda kan skicka och ta emot e-post från dessa grupper.</span><span class="sxs-lookup"><span data-stu-id="af3cc-108">If you have Exchange on-premises, then you can write back these groups tooon-premises so users with an on-premises Exchange mailbox can send and receive emails from these groups.</span></span>

<span data-ttu-id="af3cc-109">Mer information om Office 365-grupper och hur toouse dem hittar [här](http://aka.ms/O365g).</span><span class="sxs-lookup"><span data-stu-id="af3cc-109">More information about Office 365 Groups and how toouse them can be found [here](http://aka.ms/O365g).</span></span>

<span data-ttu-id="af3cc-110">En grupp för Office 365 representeras som en distributionsgrupp i lokala AD DS.</span><span class="sxs-lookup"><span data-stu-id="af3cc-110">An Office 365 group is represented as a distribution group in on-premises AD DS.</span></span> <span data-ttu-id="af3cc-111">Lokal Exchange-server måste vara i Exchange 2013 cumulative update 8 (ut i mars 2015) eller Exchange 2016 toorecognize den här nya grupptypen.</span><span class="sxs-lookup"><span data-stu-id="af3cc-111">Your on-premises Exchange server must be on Exchange 2013 cumulative update 8 (released in March 2015) or Exchange 2016 toorecognize this new group type.</span></span>

<span data-ttu-id="af3cc-112">**Anteckningar hello förhandsversionen**</span><span class="sxs-lookup"><span data-stu-id="af3cc-112">**Notes during hello preview**</span></span>

* <span data-ttu-id="af3cc-113">hello-bok postattributet för närvarande inte har fyllts i i hello preview.</span><span class="sxs-lookup"><span data-stu-id="af3cc-113">hello address book attribute is currently not populated in hello preview.</span></span> <span data-ttu-id="af3cc-114">Utan det här attributet visas inte hello grupp i hello GAL.</span><span class="sxs-lookup"><span data-stu-id="af3cc-114">Without this attribute, hello group is not visible in hello GAL.</span></span> <span data-ttu-id="af3cc-115">Hej enklaste sättet toopopulate detta attribut är toouse hello Exchange PowerShell-cmdleten `update-recipient`.</span><span class="sxs-lookup"><span data-stu-id="af3cc-115">hello easiest way toopopulate this attribute is toouse hello Exchange PowerShell cmdlet `update-recipient`.</span></span>
* <span data-ttu-id="af3cc-116">Endast skogar med hello Exchange-schemat är ogiltigt mål för grupper.</span><span class="sxs-lookup"><span data-stu-id="af3cc-116">Only forests with hello Exchange schema are valid targets for groups.</span></span> <span data-ttu-id="af3cc-117">Om ingen Exchange upptäcktes sedan är tillbakaskrivning av grupp inte möjliga tooenable.</span><span class="sxs-lookup"><span data-stu-id="af3cc-117">If no Exchange was detected, then group writeback is not possible tooenable.</span></span>
* <span data-ttu-id="af3cc-118">Endast sammanhållna Exchange-organisation distributioner stöds för närvarande.</span><span class="sxs-lookup"><span data-stu-id="af3cc-118">Only single-forest Exchange organization deployments are currently supported.</span></span> <span data-ttu-id="af3cc-119">Om du har mer än ett Exchange organisation lokalt, måste en lokal GALSync lösning för dessa grupper tooappear i din andra skogar.</span><span class="sxs-lookup"><span data-stu-id="af3cc-119">If you have more than one Exchange organization on-premises, then you need an on-premises GALSync solution for these groups tooappear in your other forests.</span></span>
* <span data-ttu-id="af3cc-120">hello grupp tillbakaskrivning funktionen hanterar inte säkerhetsgrupper eller distributionsgrupper.</span><span class="sxs-lookup"><span data-stu-id="af3cc-120">hello Group writeback feature does not handle security groups or distribution groups.</span></span>

> [!NOTE]
> <span data-ttu-id="af3cc-121">Det krävs en prenumeration tooAzure AD Premium för tillbakaskrivning av grupp.</span><span class="sxs-lookup"><span data-stu-id="af3cc-121">A subscription tooAzure AD Premium is required for group writeback.</span></span>
> 
>

## <a name="user-writeback"></a><span data-ttu-id="af3cc-122">Tillbakaskrivning av användare</span><span class="sxs-lookup"><span data-stu-id="af3cc-122">User writeback</span></span>
> [!IMPORTANT]
> <span data-ttu-id="af3cc-123">hello användaren tillbakaskrivning förhandsgranskningsfunktion togs bort i hello augusti 2015 update tooAzure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="af3cc-123">hello user writeback preview feature was removed in hello August 2015 update tooAzure AD Connect.</span></span> <span data-ttu-id="af3cc-124">Om du har aktiverat det, bör du inaktivera den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="af3cc-124">If you have enabled it, then you should disable this feature.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="af3cc-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="af3cc-125">Next steps</span></span>
<span data-ttu-id="af3cc-126">Fortsätta din [anpassad installation av Azure AD Connect](active-directory-aadconnect-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="af3cc-126">Continue your [Custom installation of Azure AD Connect](active-directory-aadconnect-get-started-custom.md).</span></span>

<span data-ttu-id="af3cc-127">Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="af3cc-127">Learn more about [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>

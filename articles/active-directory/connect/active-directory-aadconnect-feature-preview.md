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
ms.openlocfilehash: cbf8f729d0ebfb271bb0d8702ac043442b42c262
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="more-details-about-features-in-preview"></a><span data-ttu-id="6e755-103">Mer information om funktionerna i förhandsversionen</span><span class="sxs-lookup"><span data-stu-id="6e755-103">More details about features in preview</span></span>
<span data-ttu-id="6e755-104">Det här avsnittet beskriver hur du använder funktioner för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="6e755-104">This topic describes how to use features currently in preview.</span></span>

## <a name="group-writeback"></a><span data-ttu-id="6e755-105">Tillbakaskrivning av grupp</span><span class="sxs-lookup"><span data-stu-id="6e755-105">Group writeback</span></span>
<span data-ttu-id="6e755-106">Alternativet för tillbakaskrivning av grupp i valfria funktioner kan du tillbakaskrivning **Office 365-grupper** till en skog med Exchange installerad.</span><span class="sxs-lookup"><span data-stu-id="6e755-106">The option for group writeback in optional features allows you to writeback **Office 365 Groups** to a forest with Exchange installed.</span></span> <span data-ttu-id="6e755-107">Det här är en grupp som alltid lärt dig i molnet.</span><span class="sxs-lookup"><span data-stu-id="6e755-107">This is a group that is always mastered in the cloud.</span></span> <span data-ttu-id="6e755-108">Om du har Exchange lokalt kan sedan du skriva tillbaka grupperna till lokalt så att användare med en lokal Exchange-postlåda kan skicka och ta emot e-post från dessa grupper.</span><span class="sxs-lookup"><span data-stu-id="6e755-108">If you have Exchange on-premises, then you can write back these groups to on-premises so users with an on-premises Exchange mailbox can send and receive emails from these groups.</span></span>

<span data-ttu-id="6e755-109">Mer information om Office 365-grupper och hur de används finns [här](http://aka.ms/O365g).</span><span class="sxs-lookup"><span data-stu-id="6e755-109">More information about Office 365 Groups and how to use them can be found [here](http://aka.ms/O365g).</span></span>

<span data-ttu-id="6e755-110">En grupp för Office 365 representeras som en distributionsgrupp i lokala AD DS.</span><span class="sxs-lookup"><span data-stu-id="6e755-110">An Office 365 group is represented as a distribution group in on-premises AD DS.</span></span> <span data-ttu-id="6e755-111">Lokal Exchange-server måste vara i Exchange 2013 cumulative update 8 (ut i mars 2015) eller Exchange 2016 att identifiera den här nya grupptypen.</span><span class="sxs-lookup"><span data-stu-id="6e755-111">Your on-premises Exchange server must be on Exchange 2013 cumulative update 8 (released in March 2015) or Exchange 2016 to recognize this new group type.</span></span>

<span data-ttu-id="6e755-112">**Anteckningar under förhandsgranskningen**</span><span class="sxs-lookup"><span data-stu-id="6e755-112">**Notes during the preview**</span></span>

* <span data-ttu-id="6e755-113">-Bok postattributet för närvarande inte har fyllts i i förhandsgranskningen.</span><span class="sxs-lookup"><span data-stu-id="6e755-113">The address book attribute is currently not populated in the preview.</span></span> <span data-ttu-id="6e755-114">Utan det här attributet visas inte gruppen i den globala Adresslistan.</span><span class="sxs-lookup"><span data-stu-id="6e755-114">Without this attribute, the group is not visible in the GAL.</span></span> <span data-ttu-id="6e755-115">Det enklaste sättet att fylla i det här attributet är att använda Exchange PowerShell-cmdleten `update-recipient`.</span><span class="sxs-lookup"><span data-stu-id="6e755-115">The easiest way to populate this attribute is to use the Exchange PowerShell cmdlet `update-recipient`.</span></span>
* <span data-ttu-id="6e755-116">Endast skogar med Exchange-schemat är ogiltigt mål för grupper.</span><span class="sxs-lookup"><span data-stu-id="6e755-116">Only forests with the Exchange schema are valid targets for groups.</span></span> <span data-ttu-id="6e755-117">Tillbakaskrivning av grupp är inte möjligt att aktivera om ingen Exchange upptäcktes.</span><span class="sxs-lookup"><span data-stu-id="6e755-117">If no Exchange was detected, then group writeback is not possible to enable.</span></span>
* <span data-ttu-id="6e755-118">Endast sammanhållna Exchange-organisation distributioner stöds för närvarande.</span><span class="sxs-lookup"><span data-stu-id="6e755-118">Only single-forest Exchange organization deployments are currently supported.</span></span> <span data-ttu-id="6e755-119">Om du har mer än ett Exchange organisation lokalt, måste en lokal GALSync lösning för dessa grupper visas i din andra skogar.</span><span class="sxs-lookup"><span data-stu-id="6e755-119">If you have more than one Exchange organization on-premises, then you need an on-premises GALSync solution for these groups to appear in your other forests.</span></span>
* <span data-ttu-id="6e755-120">Funktionen för tillbakaskrivning av grupp hanteras inte säkerhetsgrupper eller distributionsgrupper.</span><span class="sxs-lookup"><span data-stu-id="6e755-120">The Group writeback feature does not handle security groups or distribution groups.</span></span>

> [!NOTE]
> <span data-ttu-id="6e755-121">Det krävs en prenumeration på Azure AD Premium för tillbakaskrivning av grupp.</span><span class="sxs-lookup"><span data-stu-id="6e755-121">A subscription to Azure AD Premium is required for group writeback.</span></span>
> 
>

## <a name="user-writeback"></a><span data-ttu-id="6e755-122">Tillbakaskrivning av användare</span><span class="sxs-lookup"><span data-stu-id="6e755-122">User writeback</span></span>
> [!IMPORTANT]
> <span data-ttu-id="6e755-123">Förhandsgranskningsfunktionen för tillbakaskrivning av användare har tagits bort i augusti 2015-uppdateringen till Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="6e755-123">The user writeback preview feature was removed in the August 2015 update to Azure AD Connect.</span></span> <span data-ttu-id="6e755-124">Om du har aktiverat det, bör du inaktivera den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="6e755-124">If you have enabled it, then you should disable this feature.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="6e755-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6e755-125">Next steps</span></span>
<span data-ttu-id="6e755-126">Fortsätta din [anpassad installation av Azure AD Connect](active-directory-aadconnect-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="6e755-126">Continue your [Custom installation of Azure AD Connect](active-directory-aadconnect-get-started-custom.md).</span></span>

<span data-ttu-id="6e755-127">Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="6e755-127">Learn more about [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>

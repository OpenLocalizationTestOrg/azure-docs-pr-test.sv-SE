---
title: aaaAzure Rapportaviseringar i Active Directory
description: "Hur toouse hello Azure Active Directory reporting meddelanden för misstänkt tecken moduler."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ae6d4b0e-5931-4cb3-98bf-9be91b381c92
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/15/2017
ms.author: dhanyahk;markvi
ms.custom: oldportal
ms.reviewer: dhanyahk
ms.openlocfilehash: 3843c45eaf9d68e671943bfdbc7ab68933f38fbb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-reporting-notifications"></a><span data-ttu-id="8f553-103">Rapportaviseringar i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8f553-103">Azure Active Directory Reporting Notifications</span></span>
## <a name="what-reports-generate-email-notifications"></a><span data-ttu-id="8f553-104">Vilka rapporter Generera e-postaviseringar</span><span class="sxs-lookup"><span data-stu-id="8f553-104">What reports generate email notifications</span></span>
<span data-ttu-id="8f553-105">E-postmeddelanden för tillfället endast hello oregelbundna logga aktivitet rapporten utlösare.</span><span class="sxs-lookup"><span data-stu-id="8f553-105">At this time, only hello Irregular Sign in Activity report triggers email notifications.</span></span>

## <a name="what-is-an-irregular-sign-in"></a><span data-ttu-id="8f553-106">Vad är ett ”oregelbundna inloggning”?</span><span class="sxs-lookup"><span data-stu-id="8f553-106">What is an "Irregular Sign in"?</span></span>
<span data-ttu-id="8f553-107">Oregelbundna inloggningar är de som har identifierats av våra maskininlärningsalgoritmer hello baserat på ett ”omöjligt att resa” villkor som kombineras med ett avvikande inloggning plats och en enhet.</span><span class="sxs-lookup"><span data-stu-id="8f553-107">Irregular sign-ins are those that have been identified by our machine learning algorithms, on hello basis of an "impossible travel" condition combined with an anomalous sign-in location and device.</span></span> <span data-ttu-id="8f553-108">Detta kan tyda på att en hackare har försöker toosign in med det här kontot.</span><span class="sxs-lookup"><span data-stu-id="8f553-108">This may indicate that a hacker has been trying toosign in using this account.</span></span>

## <a name="who-receives-hello-email-notifications"></a><span data-ttu-id="8f553-109">Vem som får e-postmeddelanden som hello?</span><span class="sxs-lookup"><span data-stu-id="8f553-109">Who receives hello email notifications?</span></span>
<span data-ttu-id="8f553-110">hello e-postmeddelande skickas tooall globala administratörer som har tilldelats en Active Directory Premium-licens.</span><span class="sxs-lookup"><span data-stu-id="8f553-110">hello email is sent tooall global admins who have been assigned an Active Directory Premium license.</span></span> <span data-ttu-id="8f553-111">tooensure det levererats, vi skicka den samt toohello administratörer alternativ e-postadress.</span><span class="sxs-lookup"><span data-stu-id="8f553-111">tooensure it is delivered, we send it toohello admins Alternate Email Address as well.</span></span> <span data-ttu-id="8f553-112">Administratörer bör innehålla aad-alerts-noreply@mail.windowsazure.com i deras betrodda avsändare så att de inte missar hello e-post.</span><span class="sxs-lookup"><span data-stu-id="8f553-112">Admins should include aad-alerts-noreply@mail.windowsazure.com in their safe senders list so they don’t miss hello email.</span></span>

## <a name="how-often-are-these-emails-sent"></a><span data-ttu-id="8f553-113">Hur ofta uppdateras dessa e-postmeddelanden skickas?</span><span class="sxs-lookup"><span data-stu-id="8f553-113">How often are these emails sent?</span></span>
<span data-ttu-id="8f553-114">hello e-postmeddelande om 10 nya oregelbundna inloggning aktiviteter som inträffar i hello senaste 30 dagarna eller eftersom hello senaste e-postmeddelandet skickades, beroende på vilket som är mindre.</span><span class="sxs-lookup"><span data-stu-id="8f553-114">hello email is sent if 10 new irregular sign-in activities occur in hello last 30 days, or since hello last email was sent, whichever is less.</span></span>

## <a name="how-do-i-access-hello-report-mentioned-in-hello-email"></a><span data-ttu-id="8f553-115">Hur kommer jag åt hello-rapport som nämns i hello e-post?</span><span class="sxs-lookup"><span data-stu-id="8f553-115">How do I access hello report mentioned in hello email?</span></span>
<span data-ttu-id="8f553-116">När du klickar på hello länken kommer du att omdirigerade toohello rapportsidan inom hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8f553-116">When you click on hello link, you will be redirected toohello report page within hello Azure classic portal.</span></span> <span data-ttu-id="8f553-117">I ordning tooaccess hello rapport, måste du toobe båda:</span><span class="sxs-lookup"><span data-stu-id="8f553-117">In order tooaccess hello report, you need toobe both:</span></span>

* <span data-ttu-id="8f553-118">En administratör eller medadministratör för Azure-prenumerationen</span><span class="sxs-lookup"><span data-stu-id="8f553-118">An admin or co-admin of your Azure subscription</span></span>
* <span data-ttu-id="8f553-119">En global administratör i hello directory, och tilldelas en Active Directory Premium-licens.</span><span class="sxs-lookup"><span data-stu-id="8f553-119">A global administrator in hello directory, and assigned an Active Directory Premium license.</span></span> <span data-ttu-id="8f553-120">Mer information finns i [Azure Active Directory-versioner](active-directory-editions.md).</span><span class="sxs-lookup"><span data-stu-id="8f553-120">For more information, see [Azure Active Directory editions](active-directory-editions.md).</span></span>

## <a name="can-i-turn-off-these-emails"></a><span data-ttu-id="8f553-121">Kan jag inaktivera dessa e-postmeddelanden?</span><span class="sxs-lookup"><span data-stu-id="8f553-121">Can I turn off these emails?</span></span>
<span data-ttu-id="8f553-122">Ja, tooturn av meddelanden relaterade tooanomalous inloggningar inom hello klassiska Azure-portalen klickar du på **konfigurera**, och välj sedan **inaktiverad** under hello **meddelanden**avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8f553-122">Yes, tooturn off notifications related tooanomalous sign-ins within hello Azure classic portal, click **Configure**, and then select **Disabled** under hello **Notifications** section.</span></span>

## <a name="whats-next"></a><span data-ttu-id="8f553-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8f553-123">What's next</span></span>
* <span data-ttu-id="8f553-124">Är du nyfiken på vilka säkerhets-, gransknings- och aktivitet rapporter är tillgängliga?</span><span class="sxs-lookup"><span data-stu-id="8f553-124">Curious about what security, audit, and activity reports are available?</span></span> <span data-ttu-id="8f553-125">Checka ut [Azure AD-säkerhetsgrupp, granskning och aktivitetsrapporter](active-directory-view-access-usage-reports.md)</span><span class="sxs-lookup"><span data-stu-id="8f553-125">Check out [Azure AD Security, Audit, and Activity Reports](active-directory-view-access-usage-reports.md)</span></span>
* [<span data-ttu-id="8f553-126">Komma igång med Azure Active Directory Premium</span><span class="sxs-lookup"><span data-stu-id="8f553-126">Getting started with Azure Active Directory Premium</span></span>](active-directory-get-started-premium.md)
* [<span data-ttu-id="8f553-127">Lägga till företagsanpassning tooyour inloggnings- och åtkomstpanel-sidor</span><span class="sxs-lookup"><span data-stu-id="8f553-127">Add company branding tooyour Sign In and Access Panel pages</span></span>](active-directory-add-company-branding.md)


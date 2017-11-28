---
title: aaaNamed platser i Azure Active Directory | Microsoft Docs
description: "Genom att konfigurera med namnet platser kan du undvika med IP-adresser som ägs av organisationen generera falska positiva identifieringar för hello omöjligt att resa tooatypical platser riskerar händelsetyp."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 591e4b94b2ec9d45e20c01711e922f9972e047e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="named-locations-in-azure-active-directory"></a><span data-ttu-id="d2bc4-103">Sökvägarna i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d2bc4-103">Named locations in Azure Active Directory</span></span>

<span data-ttu-id="d2bc4-104">Du kan använda hello med namnet platser funktion i Azure Active Directory, för att sätta etiketter tillförlitliga IP-adressintervall i ditt företag.</span><span class="sxs-lookup"><span data-stu-id="d2bc4-104">With hello named locations feature of Azure Active Directory, you can label trusted IP address ranges in your organizations.</span></span> <span data-ttu-id="d2bc4-105">I din miljö kan du använda sökvägarna i hello kontexten hello upptäcka [riskerar händelser](active-directory-reporting-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="d2bc4-105">In your environment, you can use named locations in hello context of hello detection of [risk events](active-directory-reporting-risk-events.md).</span></span> <span data-ttu-id="d2bc4-106">hello funktionen minskar hello antalet rapporterade falska positiva identifieringar för hello *omöjligt att resa tooatypical platser* riskerar händelsetyp.</span><span class="sxs-lookup"><span data-stu-id="d2bc4-106">hello feature helps reduce hello number of reported false positives for hello *Impossible travel tooatypical locations* risk event type.</span></span> 

## <a name="configuration"></a><span data-ttu-id="d2bc4-107">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="d2bc4-107">Configuration</span></span>

<span data-ttu-id="d2bc4-108">tooconfigure en namngiven plats:</span><span class="sxs-lookup"><span data-stu-id="d2bc4-108">tooconfigure a named location:</span></span>

1. <span data-ttu-id="d2bc4-109">Logga in toohello [Azure-portalen](https://portal.azure.com) som global administratör.</span><span class="sxs-lookup"><span data-stu-id="d2bc4-109">Sign in toohello [Azure portal](https://portal.azure.com) as global administrator.</span></span>

2. <span data-ttu-id="d2bc4-110">Hello vänster klickar du på **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="d2bc4-110">In hello left pane, click **Azure Active Directory**.</span></span>

    ![hello Azure Active Directory-länken i hello till vänster](./media/active-directory-named-locations/01.png)

3. <span data-ttu-id="d2bc4-112">På hello **Azure Active Directory** bladet i hello **säkerhet** klickar du på **villkorlig åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="d2bc4-112">On hello **Azure Active Directory** blade, in hello **Security** section, click **Conditional access**.</span></span>

    ![hello kommandot för villkorlig åtkomst](./media/active-directory-named-locations/05.png)


4. <span data-ttu-id="d2bc4-114">På hello **villkorlig åtkomst** bladet i hello **hantera** klickar du på **med namnet platser**.</span><span class="sxs-lookup"><span data-stu-id="d2bc4-114">On hello **Conditional Access** blade, in hello **Manage** section, click **Named locations**.</span></span>

    ![hello namngivna platser kommando](./media/active-directory-named-locations/06.png)


5. <span data-ttu-id="d2bc4-116">På hello **med namnet platser** bladet, klickar du på **ny plats**.</span><span class="sxs-lookup"><span data-stu-id="d2bc4-116">On hello **Named locations** blade, click **New location**.</span></span>

    ![hello kommandot ny plats](./media/active-directory-named-locations/07.png)


6. <span data-ttu-id="d2bc4-118">På hello **ny** bladet hello följande:</span><span class="sxs-lookup"><span data-stu-id="d2bc4-118">On hello **New** blade, do hello following:</span></span>

    ![hello nytt blad](./media/active-directory-named-locations/08.png)

    <span data-ttu-id="d2bc4-120">a.</span><span class="sxs-lookup"><span data-stu-id="d2bc4-120">a.</span></span> <span data-ttu-id="d2bc4-121">I hello **namn** Skriv ett namn för din namngiven plats.</span><span class="sxs-lookup"><span data-stu-id="d2bc4-121">In hello **Name** box, type a name for your named location.</span></span>

    <span data-ttu-id="d2bc4-122">b.</span><span class="sxs-lookup"><span data-stu-id="d2bc4-122">b.</span></span> <span data-ttu-id="d2bc4-123">I hello **IP-adressintervall** skriver ett IP-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="d2bc4-123">In hello **IP ranges** box, type an IP range.</span></span> <span data-ttu-id="d2bc4-124">hello IP-intervallet måste toobe i hello *Classless Inter-Domain Routing* (CIDR)-format.</span><span class="sxs-lookup"><span data-stu-id="d2bc4-124">hello IP range needs toobe in hello *Classless Inter-Domain Routing* (CIDR) format.</span></span>  

    <span data-ttu-id="d2bc4-125">c.</span><span class="sxs-lookup"><span data-stu-id="d2bc4-125">c.</span></span> <span data-ttu-id="d2bc4-126">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d2bc4-126">Click **Create**.</span></span>



## <a name="what-you-should-know"></a><span data-ttu-id="d2bc4-127">Vad du bör känna till</span><span class="sxs-lookup"><span data-stu-id="d2bc4-127">What you should know</span></span>

<span data-ttu-id="d2bc4-128">**Massuppdateringar**: när du skapar eller uppdaterar sökvägarna för massuppdateringar, du kan överföra eller hämta en CSV-fil med hello IP-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="d2bc4-128">**Bulk updates**: When you create or update named locations, for bulk updates, you can upload or download a CSV file with hello IP ranges.</span></span> <span data-ttu-id="d2bc4-129">Överföra lägger till hello IP-adressintervall i hello toohello listan över filer i stället för att skriva över hello-listan.</span><span class="sxs-lookup"><span data-stu-id="d2bc4-129">An upload adds hello IP ranges in hello file toohello list instead of overwriting hello list.</span></span>

![hello ladda upp och hämta länkar](./media/active-directory-named-locations/09.png)


<span data-ttu-id="d2bc4-131">**Begränsningar**: du kan definiera högst 60 sökvägarna med en IP-adressintervall som tilldelats tooeach av dem.</span><span class="sxs-lookup"><span data-stu-id="d2bc4-131">**Limitations**: You can define a maximum of 60 named locations, with one IP range assigned tooeach of them.</span></span> <span data-ttu-id="d2bc4-132">Om du har en namngiven plats som har konfigurerats kan du definiera too500 IP-adressintervall för den.</span><span class="sxs-lookup"><span data-stu-id="d2bc4-132">If you have just one named location configured, you can define up too500 IP ranges for it.</span></span>


## <a name="next-steps"></a><span data-ttu-id="d2bc4-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d2bc4-133">Next steps</span></span>

<span data-ttu-id="d2bc4-134">toolearn mer om riskhändelser, se [Azure Active Directory riskhändelser](active-directory-reporting-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="d2bc4-134">toolearn more about risk events, see [Azure Active Directory risk events](active-directory-reporting-risk-events.md).</span></span>


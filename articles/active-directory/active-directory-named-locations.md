---
title: Med namnet platser i Azure Active Directory | Microsoft Docs
description: "Genom att konfigurera med namnet platser kan du undvika med IP-adresser som ägs av organisationen generera falska positiva identifieringar för Impossible reser till ovanliga platser risk händelsetyp."
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
ms.openlocfilehash: ff31ded1d9d60e47e0ae5f01119de78cd7f2df38
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="named-locations-in-azure-active-directory"></a><span data-ttu-id="55571-103">Sökvägarna i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="55571-103">Named locations in Azure Active Directory</span></span>

<span data-ttu-id="55571-104">Du kan använda funktionen sökvägarna i Azure Active Directory för att sätta etiketter tillförlitliga IP-adressintervall i ditt företag.</span><span class="sxs-lookup"><span data-stu-id="55571-104">With the named locations feature of Azure Active Directory, you can label trusted IP address ranges in your organizations.</span></span> <span data-ttu-id="55571-105">I din miljö kan du använda sökvägarna i kontexten för identifiering av [riskerar händelser](active-directory-reporting-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="55571-105">In your environment, you can use named locations in the context of the detection of [risk events](active-directory-reporting-risk-events.md).</span></span> <span data-ttu-id="55571-106">Funktionen hjälper till att minska antalet rapporterade falska positiva identifieringar för den *Impossible resa till ovanliga platser* riskerar händelsetyp.</span><span class="sxs-lookup"><span data-stu-id="55571-106">The feature helps reduce the number of reported false positives for the *Impossible travel to atypical locations* risk event type.</span></span> 

## <a name="configuration"></a><span data-ttu-id="55571-107">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="55571-107">Configuration</span></span>

<span data-ttu-id="55571-108">Så här konfigurerar du en namngiven plats:</span><span class="sxs-lookup"><span data-stu-id="55571-108">To configure a named location:</span></span>

1. <span data-ttu-id="55571-109">Logga in på den [Azure-portalen](https://portal.azure.com) som global administratör.</span><span class="sxs-lookup"><span data-stu-id="55571-109">Sign in to the [Azure portal](https://portal.azure.com) as global administrator.</span></span>

2. <span data-ttu-id="55571-110">I den vänstra rutan klickar du på **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="55571-110">In the left pane, click **Azure Active Directory**.</span></span>

    ![Azure Active Directory-länken i den vänstra rutan](./media/active-directory-named-locations/01.png)

3. <span data-ttu-id="55571-112">På den **Azure Active Directory** blad i den **säkerhet** klickar du på **villkorlig åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="55571-112">On the **Azure Active Directory** blade, in the **Security** section, click **Conditional access**.</span></span>

    ![Kommandot villkorlig åtkomst](./media/active-directory-named-locations/05.png)


4. <span data-ttu-id="55571-114">På den **villkorlig åtkomst** blad i den **hantera** klickar du på **med namnet platser**.</span><span class="sxs-lookup"><span data-stu-id="55571-114">On the **Conditional Access** blade, in the **Manage** section, click **Named locations**.</span></span>

    ![Kommandot namngivna platser](./media/active-directory-named-locations/06.png)


5. <span data-ttu-id="55571-116">På den **med namnet platser** bladet, klickar du på **ny plats**.</span><span class="sxs-lookup"><span data-stu-id="55571-116">On the **Named locations** blade, click **New location**.</span></span>

    ![Kommandot plats](./media/active-directory-named-locations/07.png)


6. <span data-ttu-id="55571-118">På den **ny** bladet gör du följande:</span><span class="sxs-lookup"><span data-stu-id="55571-118">On the **New** blade, do the following:</span></span>

    ![Det nya bladet](./media/active-directory-named-locations/08.png)

    <span data-ttu-id="55571-120">a.</span><span class="sxs-lookup"><span data-stu-id="55571-120">a.</span></span> <span data-ttu-id="55571-121">I den **namn** Skriv ett namn för din namngiven plats.</span><span class="sxs-lookup"><span data-stu-id="55571-121">In the **Name** box, type a name for your named location.</span></span>

    <span data-ttu-id="55571-122">b.</span><span class="sxs-lookup"><span data-stu-id="55571-122">b.</span></span> <span data-ttu-id="55571-123">I den **IP-adressintervall** skriver ett IP-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="55571-123">In the **IP ranges** box, type an IP range.</span></span> <span data-ttu-id="55571-124">IP-intervallet måste vara i den *Classless Inter-Domain Routing* (CIDR)-format.</span><span class="sxs-lookup"><span data-stu-id="55571-124">The IP range needs to be in the *Classless Inter-Domain Routing* (CIDR) format.</span></span>  

    <span data-ttu-id="55571-125">c.</span><span class="sxs-lookup"><span data-stu-id="55571-125">c.</span></span> <span data-ttu-id="55571-126">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="55571-126">Click **Create**.</span></span>



## <a name="what-you-should-know"></a><span data-ttu-id="55571-127">Vad du bör känna till</span><span class="sxs-lookup"><span data-stu-id="55571-127">What you should know</span></span>

<span data-ttu-id="55571-128">**Massuppdateringar**: när du skapar eller uppdaterar sökvägarna för massuppdateringar, kan du överföra eller hämta en CSV-fil med IP-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="55571-128">**Bulk updates**: When you create or update named locations, for bulk updates, you can upload or download a CSV file with the IP ranges.</span></span> <span data-ttu-id="55571-129">Överföra lägger till IP-adressintervall i filen i listan i stället för att skriva över listan.</span><span class="sxs-lookup"><span data-stu-id="55571-129">An upload adds the IP ranges in the file to the list instead of overwriting the list.</span></span>

![Länkar överföring och hämtning](./media/active-directory-named-locations/09.png)


<span data-ttu-id="55571-131">**Begränsningar**: du kan definiera högst 60 sökvägarna med en IP-adressintervall som är tilldelade till var och en av dem.</span><span class="sxs-lookup"><span data-stu-id="55571-131">**Limitations**: You can define a maximum of 60 named locations, with one IP range assigned to each of them.</span></span> <span data-ttu-id="55571-132">Om du har en namngiven plats som har konfigurerats kan definiera du upp till 500 IP-adressintervall för den.</span><span class="sxs-lookup"><span data-stu-id="55571-132">If you have just one named location configured, you can define up to 500 IP ranges for it.</span></span>


## <a name="next-steps"></a><span data-ttu-id="55571-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="55571-133">Next steps</span></span>

<span data-ttu-id="55571-134">Läs mer om riskhändelser i [Azure Active Directory riskhändelser](active-directory-reporting-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="55571-134">To learn more about risk events, see [Azure Active Directory risk events](active-directory-reporting-risk-events.md).</span></span>


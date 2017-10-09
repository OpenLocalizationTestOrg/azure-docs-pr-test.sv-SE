---
title: "aaaProvisioning appar med Omfångsfilter | Microsoft Docs"
description: "Lär dig hur toouse scope filtrerar tooprevent objekt i appar som stöder automatisk användaretablering från tillhandahålls faktiskt om ett objekt inte uppfyller dina affärsbehov."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: bcfbda74-e4d4-4859-83bc-06b104df3918
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: markvi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f0299390dc3fdb70aa9d271e835069a08827d635
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="attribute-based-application-provisioning-with-scoping-filters"></a><span data-ttu-id="c178a-103">Attributbaserad programmet etablering med Omfångsfilter</span><span class="sxs-lookup"><span data-stu-id="c178a-103">Attribute-based application provisioning with scoping filters</span></span>
<span data-ttu-id="c178a-104">hello syftet med det här avsnittet är tooexplain hur toouse scope filtrerar toodefine attributbaserade regler som bestämmer vilka användare som är etablerad toohello program.</span><span class="sxs-lookup"><span data-stu-id="c178a-104">hello objective of this section is tooexplain how toouse scoping filters toodefine attribute-based rules that determine which users are provisioned toohello application.</span></span>

## <a name="clauses-and-scope-groups"></a><span data-ttu-id="c178a-105">Satser och grupper</span><span class="sxs-lookup"><span data-stu-id="c178a-105">Clauses and Scope Groups</span></span>
![Omfångsfilter][1] 

<span data-ttu-id="c178a-107">Målgrupp filter definieras av en eller flera **omfång grupper**, varje av som innehåller en eller flera **satser**.</span><span class="sxs-lookup"><span data-stu-id="c178a-107">Scoping filters are defined by one or more **scope groups**, each of which hold one or more **clauses**.</span></span> <span data-ttu-id="c178a-108">toosee hello-satser för en viss scope-grupp, expandera den genom att klicka på hello pilen toohello till vänster i hello gruppnamn.</span><span class="sxs-lookup"><span data-stu-id="c178a-108">toosee hello clauses for a particular scope group, expand it by clicking hello arrow toohello left of hello group name.</span></span>

<span data-ttu-id="c178a-109">En **satsen** avgör vilka användare som får toopass via hello Omfångsfilter genom utvärdering av attribut för varje användare.</span><span class="sxs-lookup"><span data-stu-id="c178a-109">A **clause** determines which users are allowed toopass through hello scoping filter by evaluating each user’s attributes.</span></span> <span data-ttu-id="c178a-110">Till exempel kanske du en sats som kräver att en användare ”tillstånd” attributet lika New York, så att endast användare i New York tillhandahålls i hello program.</span><span class="sxs-lookup"><span data-stu-id="c178a-110">For example, you might have one clause that requires that a user’s ‘state’ attribute equal New York, so only New York users are provisioned into hello application.</span></span>

![Målgrupp gruppnamn][2] 

<span data-ttu-id="c178a-112">Varje **omfång grupp** börjar med en obligatorisk **satsen**, som visas i hello skärmbilden ovan.</span><span class="sxs-lookup"><span data-stu-id="c178a-112">Each **scope group** starts with one mandatory **clause**, as shown in hello screenshot above.</span></span> <span data-ttu-id="c178a-113">Den här satsen bara säger hello användaren först tilldelas toohello programmet innan det används av din målgrupp filter.</span><span class="sxs-lookup"><span data-stu-id="c178a-113">This clause simply states that hello user must first be assigned toohello application before it’s evaluated by your scoping filters.</span></span> <span data-ttu-id="c178a-114">Den här instruktionen kan inte tas bort eller ändras.</span><span class="sxs-lookup"><span data-stu-id="c178a-114">This clause cannot be deleted or modified.</span></span>

<span data-ttu-id="c178a-115">Du kan lägga till nya satser eller nya grupper genom att trycka på hello knapp.</span><span class="sxs-lookup"><span data-stu-id="c178a-115">You can add new clauses or new scope groups by pressing hello appropriate button.</span></span> <span data-ttu-id="c178a-116">Du kan namnge varje scope-grupp genom att redigera dess **omfång gruppnamn** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="c178a-116">You can give each scope group a name by editing its **Scope Group Name** property.</span></span>

## <a name="how-scoping-filters-are-evaluated"></a><span data-ttu-id="c178a-117">Hur scope filter utvärderas</span><span class="sxs-lookup"><span data-stu-id="c178a-117">How Scoping Filters are Evaluated</span></span>
<span data-ttu-id="c178a-118">Under etableringen testa vi varje tilldelad användare mot din målgrupp filter toodetermine om användaren ska ha åtkomst toohello program.</span><span class="sxs-lookup"><span data-stu-id="c178a-118">During provisioning, we test every assigned user against your scoping filters toodetermine if that user deserves access toohello application.</span></span> <span data-ttu-id="c178a-119">Du kan tänka dig varje instruktion som ett test som måste skickas för hello användaren tooavoid komma filtreras bort.</span><span class="sxs-lookup"><span data-stu-id="c178a-119">You can think of each clause as being a test that must be passed in order for hello user tooavoid getting filtered out.</span></span> 

<span data-ttu-id="c178a-120">Om du har flera grupper som definierats måste varje användare skicka minst en av dem tooaccess hello program.</span><span class="sxs-lookup"><span data-stu-id="c178a-120">If you have multiple scope groups defined, each user must pass at least one of them tooaccess hello application.</span></span> <span data-ttu-id="c178a-121">I varje scope-grupp dock måste hello användaren klara alla satsen toopass gruppen specifikt omfång.</span><span class="sxs-lookup"><span data-stu-id="c178a-121">Within each scope group, however, hello user must pass every clause toopass that specific scope group.</span></span> 

<span data-ttu-id="c178a-122">Med andra ord kan jämföras med grupper som villkor och du kan tänka dig hello-satser i dem som och villkor.</span><span class="sxs-lookup"><span data-stu-id="c178a-122">In other words, you can think of scope groups as being OR’d together, and you can think of hello clauses within them as being AND’d together.</span></span> <span data-ttu-id="c178a-123">Anta till exempel att hello Omfångsfilter nedan:</span><span class="sxs-lookup"><span data-stu-id="c178a-123">For example, consider hello scoping filter below:</span></span>

![Målgrupp gruppnamn][3]  

<span data-ttu-id="c178a-125">Enligt toothis Omfångsfilter användare måste uppfylla hello följande villkor, toobe etableras:</span><span class="sxs-lookup"><span data-stu-id="c178a-125">According toothis scoping filter, users must satisfy hello following criteria, toobe provisioned:</span></span>

1. <span data-ttu-id="c178a-126">De måste tilldelas toohello program.</span><span class="sxs-lookup"><span data-stu-id="c178a-126">They must be assigned toohello application.</span></span>
2. <span data-ttu-id="c178a-127">De måste arbeta på hello tekniker avdelning</span><span class="sxs-lookup"><span data-stu-id="c178a-127">They must work in hello Engineering department</span></span>
3. <span data-ttu-id="c178a-128">De måste vara fungerar i San Francisco eller Kanada.</span><span class="sxs-lookup"><span data-stu-id="c178a-128">They must be work in either San Francisco or Canada.</span></span>

## <a name="related-articles"></a><span data-ttu-id="c178a-129">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="c178a-129">Related Articles</span></span>
* [<span data-ttu-id="c178a-130">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c178a-130">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="c178a-131">Automatisera Användaretablering och avetablering tooSaaS program</span><span class="sxs-lookup"><span data-stu-id="c178a-131">Automate User Provisioning and Deprovisioning tooSaaS Applications</span></span>](active-directory-saas-app-provisioning.md)
* [<span data-ttu-id="c178a-132">Anpassa attributmappning för Användaretablering</span><span class="sxs-lookup"><span data-stu-id="c178a-132">Customizing Attribute Mappings for User Provisioning</span></span>](active-directory-saas-customizing-attribute-mappings.md)
* [<span data-ttu-id="c178a-133">Skriva uttryck för attributmappning</span><span class="sxs-lookup"><span data-stu-id="c178a-133">Writing Expressions for Attribute Mappings</span></span>](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [<span data-ttu-id="c178a-134">Kontot etablering meddelanden</span><span class="sxs-lookup"><span data-stu-id="c178a-134">Account Provisioning Notifications</span></span>](active-directory-saas-account-provisioning-notifications.md)
* [<span data-ttu-id="c178a-135">Använda SCIM tooenable Automatisk etablering av användare och grupper från Azure Active Directory tooapplications</span><span class="sxs-lookup"><span data-stu-id="c178a-135">Using SCIM tooenable automatic provisioning of users and groups from Azure Active Directory tooapplications</span></span>](active-directory-scim-provisioning.md)
* [<span data-ttu-id="c178a-136">Lista över självstudier om hur tooIntegrate SaaS-appar</span><span class="sxs-lookup"><span data-stu-id="c178a-136">List of Tutorials on How tooIntegrate SaaS Apps</span></span>](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-scoping-filters/ic782811.png
[2]: ./media/active-directory-saas-scoping-filters/ic782812.png
[3]: ./media/active-directory-saas-scoping-filters/ic782813.png

---
title: "Etablering appar med Omfångsfilter | Microsoft Docs"
description: "Lär dig hur du använder målgrupp filter för att förhindra att objekt i appar som stöder automatisk användaretablering från tillhandahålls faktiskt om ett objekt inte uppfyller dina affärsbehov."
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
ms.openlocfilehash: 109635052e2ded33831b050eb12d50745944091b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="attribute-based-application-provisioning-with-scoping-filters"></a><span data-ttu-id="7a78e-103">Attributbaserad programmet etablering med Omfångsfilter</span><span class="sxs-lookup"><span data-stu-id="7a78e-103">Attribute-based application provisioning with scoping filters</span></span>
<span data-ttu-id="7a78e-104">Syftet med det här avsnittet är som förklarar hur du använder målgrupp filter för att definiera attributbaserade regler som bestämmer vilka användare som har etablerats till programmet.</span><span class="sxs-lookup"><span data-stu-id="7a78e-104">The objective of this section is to explain how to use scoping filters to define attribute-based rules that determine which users are provisioned to the application.</span></span>

## <a name="clauses-and-scope-groups"></a><span data-ttu-id="7a78e-105">Satser och grupper</span><span class="sxs-lookup"><span data-stu-id="7a78e-105">Clauses and Scope Groups</span></span>
![Omfångsfilter][1] 

<span data-ttu-id="7a78e-107">Målgrupp filter definieras av en eller flera **omfång grupper**, varje av som innehåller en eller flera **satser**.</span><span class="sxs-lookup"><span data-stu-id="7a78e-107">Scoping filters are defined by one or more **scope groups**, each of which hold one or more **clauses**.</span></span> <span data-ttu-id="7a78e-108">Visa satser för en viss scope-grupp, expandera den genom att klicka på pilen till vänster om gruppnamnet.</span><span class="sxs-lookup"><span data-stu-id="7a78e-108">To see the clauses for a particular scope group, expand it by clicking the arrow to the left of the group name.</span></span>

<span data-ttu-id="7a78e-109">En **satsen** avgör vilka användare som får passera filtret målgrupp genom utvärdering av attribut för varje användare.</span><span class="sxs-lookup"><span data-stu-id="7a78e-109">A **clause** determines which users are allowed to pass through the scoping filter by evaluating each user’s attributes.</span></span> <span data-ttu-id="7a78e-110">Du kan till exempel ha en sats som kräver att en användares 'state'-attribut är lika med New York, så att endast användare i New York tillhandahålls av programmet.</span><span class="sxs-lookup"><span data-stu-id="7a78e-110">For example, you might have one clause that requires that a user’s ‘state’ attribute equal New York, so only New York users are provisioned into the application.</span></span>

![Målgrupp gruppnamn][2] 

<span data-ttu-id="7a78e-112">Varje **omfång grupp** börjar med en obligatorisk **satsen**som visas i skärmbilden ovan.</span><span class="sxs-lookup"><span data-stu-id="7a78e-112">Each **scope group** starts with one mandatory **clause**, as shown in the screenshot above.</span></span> <span data-ttu-id="7a78e-113">Den här satsen anger bara att användaren måste först tilldelas programmet innan det används av din målgrupp filter.</span><span class="sxs-lookup"><span data-stu-id="7a78e-113">This clause simply states that the user must first be assigned to the application before it’s evaluated by your scoping filters.</span></span> <span data-ttu-id="7a78e-114">Den här instruktionen kan inte tas bort eller ändras.</span><span class="sxs-lookup"><span data-stu-id="7a78e-114">This clause cannot be deleted or modified.</span></span>

<span data-ttu-id="7a78e-115">Du kan lägga till nya satser eller nya grupper genom att trycka på knappen.</span><span class="sxs-lookup"><span data-stu-id="7a78e-115">You can add new clauses or new scope groups by pressing the appropriate button.</span></span> <span data-ttu-id="7a78e-116">Du kan namnge varje scope-grupp genom att redigera dess **omfång gruppnamn** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="7a78e-116">You can give each scope group a name by editing its **Scope Group Name** property.</span></span>

## <a name="how-scoping-filters-are-evaluated"></a><span data-ttu-id="7a78e-117">Hur scope filter utvärderas</span><span class="sxs-lookup"><span data-stu-id="7a78e-117">How Scoping Filters are Evaluated</span></span>
<span data-ttu-id="7a78e-118">Under etableringen, kan vi Testa varje tilldelad användare mot din målgrupp filter för att avgöra om användaren ska ha åtkomst till programmet.</span><span class="sxs-lookup"><span data-stu-id="7a78e-118">During provisioning, we test every assigned user against your scoping filters to determine if that user deserves access to the application.</span></span> <span data-ttu-id="7a78e-119">Du kan tänka dig varje instruktion som ett test som måste skickas för användare att undvika komma filtreras bort.</span><span class="sxs-lookup"><span data-stu-id="7a78e-119">You can think of each clause as being a test that must be passed in order for the user to avoid getting filtered out.</span></span> 

<span data-ttu-id="7a78e-120">Om du har flera grupper som har definierats kan skicka varje användare minst en av dem åtkomst till programmet.</span><span class="sxs-lookup"><span data-stu-id="7a78e-120">If you have multiple scope groups defined, each user must pass at least one of them to access the application.</span></span> <span data-ttu-id="7a78e-121">I varje scope-grupp men måste användaren klara alla-satsen för att skicka gruppen specifikt omfång.</span><span class="sxs-lookup"><span data-stu-id="7a78e-121">Within each scope group, however, the user must pass every clause to pass that specific scope group.</span></span> 

<span data-ttu-id="7a78e-122">Med andra ord kan jämföras med grupper som villkor och du kan tänka dig satser i dem som och villkor.</span><span class="sxs-lookup"><span data-stu-id="7a78e-122">In other words, you can think of scope groups as being OR’d together, and you can think of the clauses within them as being AND’d together.</span></span> <span data-ttu-id="7a78e-123">Anta till exempel att filtret målgrupp nedan:</span><span class="sxs-lookup"><span data-stu-id="7a78e-123">For example, consider the scoping filter below:</span></span>

![Målgrupp gruppnamn][3]  

<span data-ttu-id="7a78e-125">Användarna måste uppfylla följande kriterier som ska etableras med enligt det här målgrupp filtret:</span><span class="sxs-lookup"><span data-stu-id="7a78e-125">According to this scoping filter, users must satisfy the following criteria, to be provisioned:</span></span>

1. <span data-ttu-id="7a78e-126">De måste tilldelas till programmet.</span><span class="sxs-lookup"><span data-stu-id="7a78e-126">They must be assigned to the application.</span></span>
2. <span data-ttu-id="7a78e-127">De måste arbeta i avdelningen tekniker</span><span class="sxs-lookup"><span data-stu-id="7a78e-127">They must work in the Engineering department</span></span>
3. <span data-ttu-id="7a78e-128">De måste vara fungerar i San Francisco eller Kanada.</span><span class="sxs-lookup"><span data-stu-id="7a78e-128">They must be work in either San Francisco or Canada.</span></span>

## <a name="related-articles"></a><span data-ttu-id="7a78e-129">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="7a78e-129">Related Articles</span></span>
* [<span data-ttu-id="7a78e-130">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7a78e-130">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="7a78e-131">Automatisera användaren etablering och avetablering för SaaS-program</span><span class="sxs-lookup"><span data-stu-id="7a78e-131">Automate User Provisioning and Deprovisioning to SaaS Applications</span></span>](active-directory-saas-app-provisioning.md)
* [<span data-ttu-id="7a78e-132">Anpassa attributmappning för Användaretablering</span><span class="sxs-lookup"><span data-stu-id="7a78e-132">Customizing Attribute Mappings for User Provisioning</span></span>](active-directory-saas-customizing-attribute-mappings.md)
* [<span data-ttu-id="7a78e-133">Skriva uttryck för attributmappning</span><span class="sxs-lookup"><span data-stu-id="7a78e-133">Writing Expressions for Attribute Mappings</span></span>](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [<span data-ttu-id="7a78e-134">Kontot etablering meddelanden</span><span class="sxs-lookup"><span data-stu-id="7a78e-134">Account Provisioning Notifications</span></span>](active-directory-saas-account-provisioning-notifications.md)
* [<span data-ttu-id="7a78e-135">Använda SCIM för att aktivera automatisk etablering av användare och grupper från Azure Active Directory till program</span><span class="sxs-lookup"><span data-stu-id="7a78e-135">Using SCIM to enable automatic provisioning of users and groups from Azure Active Directory to applications</span></span>](active-directory-scim-provisioning.md)
* [<span data-ttu-id="7a78e-136">Lista över självstudier om hur du integrerar SaaS-appar</span><span class="sxs-lookup"><span data-stu-id="7a78e-136">List of Tutorials on How to Integrate SaaS Apps</span></span>](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-scoping-filters/ic782811.png
[2]: ./media/active-directory-saas-scoping-filters/ic782812.png
[3]: ./media/active-directory-saas-scoping-filters/ic782813.png

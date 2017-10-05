---
title: "Azure Active Directory hybrid identity designöverväganden - ange krav för multifaktorautentisering"
description: "Med villkorlig åtkomstkontroll kontrollerar de särskilda villkor som du väljer när du autentiserar användaren och innan du tillåter åtkomst till programmet i Azure Active Directory. När dessa villkor är uppfyllda, autentiserade användaren och få tillgång till programmet."
documentationcenter: 
services: active-directory
author: femila
manager: billmath
editor: 
ms.assetid: 9c59fda9-47d0-4c7e-b3e7-3575c29beabe
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 5b3a8ce6e4203dfb3700f324e32687dd910118af
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="determine-multi-factor-authentication-requirements-for-your-hybrid-identity-solution"></a><span data-ttu-id="10b56-104">Ange krav för multifaktorautentisering för dina hybrididentitetslösning</span><span class="sxs-lookup"><span data-stu-id="10b56-104">Determine multi-factor authentication requirements for your hybrid identity solution</span></span>
<span data-ttu-id="10b56-105">I den här värld mobilitet, med användare som ansluter till data och program i molnet och från valfri enhet, blivit skydda den här informationen ytterst viktigt.</span><span class="sxs-lookup"><span data-stu-id="10b56-105">In this world of mobility, with users accessing data and applications in the cloud and from any device, securing this information has become paramount.</span></span>  <span data-ttu-id="10b56-106">Varje dag är det en ny rubrik om ett intrång.</span><span class="sxs-lookup"><span data-stu-id="10b56-106">Every day there is a new headline about a security breach.</span></span>  <span data-ttu-id="10b56-107">Även om det är inte säkert mot sådana överträdelser, ger multifaktorautentisering, ett ytterligare lager av säkerhet för att förhindra att dessa överträdelser.</span><span class="sxs-lookup"><span data-stu-id="10b56-107">Although, there is no guarantee against such breaches, multi-factor authentication, provides an additional layer of security to help prevent these breaches.</span></span>
<span data-ttu-id="10b56-108">Börja med att utvärdera organisationer kraven för multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="10b56-108">Start by evaluating the organizations requirements for multi-factor authentication.</span></span> <span data-ttu-id="10b56-109">Det vill säga vad organisationen försöker skydda.</span><span class="sxs-lookup"><span data-stu-id="10b56-109">That is, what is the organization trying to secure.</span></span>  <span data-ttu-id="10b56-110">Denna utvärdering är viktigt att definiera de tekniska kraven för att installera och aktivera organisationer användare för multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="10b56-110">This evaluation is important to define the technical requirements for setting up and enabling the organizations users for multi-factor authentication.</span></span>

> [!NOTE]
> <span data-ttu-id="10b56-111">Om du inte är bekant med MFA och syfte, rekommenderas du läsa artikeln [vad är Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md) före fortsätta läsa det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="10b56-111">If you are not familiar with MFA and what it does, it is strongly recommended that you read the article [What is Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md) prior to continue reading this section.</span></span>
> 
> 

<span data-ttu-id="10b56-112">Se till att svara på följande:</span><span class="sxs-lookup"><span data-stu-id="10b56-112">Make sure to answer the following:</span></span>

* <span data-ttu-id="10b56-113">Ditt företag försöker skydda Microsoft-appar?</span><span class="sxs-lookup"><span data-stu-id="10b56-113">Is your company trying to secure Microsoft apps?</span></span> 
* <span data-ttu-id="10b56-114">Hur dessa appar är publicerade?</span><span class="sxs-lookup"><span data-stu-id="10b56-114">How these apps are published?</span></span>
* <span data-ttu-id="10b56-115">Företaget kan ge fjärråtkomst så att anställda åtkomst till lokala appar?</span><span class="sxs-lookup"><span data-stu-id="10b56-115">Does your company provide remote access to allow employees to access on-premises apps?</span></span>

<span data-ttu-id="10b56-116">Om Ja, vilken typ av fjärråtkomst? Du måste utvärdera där användare som kommer åt dessa program kommer att finnas.</span><span class="sxs-lookup"><span data-stu-id="10b56-116">If yes, what type of remote access?You also need to evaluate where the users who are accessing these applications will be located.</span></span> <span data-ttu-id="10b56-117">Det här är ett annat viktigt steg för att definiera en strategi för rätt multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="10b56-117">This evaluation is another important step to define the proper multi-factor authentication strategy.</span></span> <span data-ttu-id="10b56-118">Se till att besvara följande frågor:</span><span class="sxs-lookup"><span data-stu-id="10b56-118">Make sure to answer the following questions:</span></span>

* <span data-ttu-id="10b56-119">Om användarna ska finnas?</span><span class="sxs-lookup"><span data-stu-id="10b56-119">Where are the users going to be located?</span></span>
* <span data-ttu-id="10b56-120">Kan de finnas var som helst?</span><span class="sxs-lookup"><span data-stu-id="10b56-120">Can they be located anywhere?</span></span>
* <span data-ttu-id="10b56-121">Vill företaget upprätta begränsningar enligt användarens plats?</span><span class="sxs-lookup"><span data-stu-id="10b56-121">Does your company want to establish restrictions according to the user’s location?</span></span>

<span data-ttu-id="10b56-122">När du förstår dessa krav är det viktigt att du även utvärdera användarens krav för multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="10b56-122">Once you understand these requirements, it is important to also evaluate the user’s requirements for multi-factor authentication.</span></span> <span data-ttu-id="10b56-123">Denna utvärdering är viktig eftersom den definierar kraven för lansera multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="10b56-123">This evaluation is important because it will define the requirements for rolling out multi-factor authentication.</span></span> <span data-ttu-id="10b56-124">Se till att besvara följande frågor:</span><span class="sxs-lookup"><span data-stu-id="10b56-124">Make sure to answer the following questions:</span></span>

* <span data-ttu-id="10b56-125">Är användare som är bekanta med Multi-Factor authentication?</span><span class="sxs-lookup"><span data-stu-id="10b56-125">Are the users familiar with multi-factor authentication?</span></span>
* <span data-ttu-id="10b56-126">Vissa använder ombedd att ange ytterligare autentisering?</span><span class="sxs-lookup"><span data-stu-id="10b56-126">Will some uses be required to provide additional authentication?</span></span>  
  * <span data-ttu-id="10b56-127">Om Ja, tid när kommer från externa nätverk eller vid åtkomst till specifika program eller andra villkor?</span><span class="sxs-lookup"><span data-stu-id="10b56-127">If yes, all the time, when coming from external networks, or accessing specific applications, or under other conditions?</span></span>
* <span data-ttu-id="10b56-128">Kommer användarna utbildning att konfigurera och implementera multifaktorautentisering?</span><span class="sxs-lookup"><span data-stu-id="10b56-128">Will the users require training on how to setup and implement multi-factor authentication?</span></span>
* <span data-ttu-id="10b56-129">Vilka är de scenarier som företaget vill aktivera Multi-Factor authentication för användarna?</span><span class="sxs-lookup"><span data-stu-id="10b56-129">What are the key scenarios that your company wants to enable multi-factor authentication for their users?</span></span>

<span data-ttu-id="10b56-130">Efter föregående frågor, kommer du att kunna förstå om multifaktorautentisering redan implementerats lokala.</span><span class="sxs-lookup"><span data-stu-id="10b56-130">After answering the previous questions, you will be able to understand if there are multi-factor authentication already implemented on-premises.</span></span> <span data-ttu-id="10b56-131">Denna utvärdering är viktigt att definiera de tekniska kraven för att installera och aktivera organisationer användare för multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="10b56-131">This evaluation is important to define the technical requirements for setting up and enabling the organizations users for multi-factor authentication.</span></span> <span data-ttu-id="10b56-132">Se till att besvara följande frågor:</span><span class="sxs-lookup"><span data-stu-id="10b56-132">Make sure to answer the following questions:</span></span>

* <span data-ttu-id="10b56-133">Behöver företaget skydda Privilegierade konton med MFA?</span><span class="sxs-lookup"><span data-stu-id="10b56-133">Does your company need to protect privileged accounts with MFA?</span></span>
* <span data-ttu-id="10b56-134">Behöver ditt företag att aktivera MFA för vissa program på grund av kompatibilitetsskäl?</span><span class="sxs-lookup"><span data-stu-id="10b56-134">Does your company need to enable MFA for certain application for compliance reasons?</span></span>
* <span data-ttu-id="10b56-135">Behöver ditt företag att aktivera MFA för alla berättigade användare av dessa program eller endast administratörer?</span><span class="sxs-lookup"><span data-stu-id="10b56-135">Does your company need to enable MFA for all eligible users of these application or only administrators?</span></span>
* <span data-ttu-id="10b56-136">Du behöver har alltid aktiverat MFA eller bara när användare är inloggade utanför företagets nätverk?</span><span class="sxs-lookup"><span data-stu-id="10b56-136">Do you need have MFA always enabled or only when the users are logged outside of your corporate network?</span></span>

## <a name="next-steps"></a><span data-ttu-id="10b56-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="10b56-137">Next steps</span></span>
[<span data-ttu-id="10b56-138">Definiera en strategi för införandet en hybrid identity</span><span class="sxs-lookup"><span data-stu-id="10b56-138">Define a hybrid identity adoption strategy</span></span>](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)

## <a name="see-also"></a><span data-ttu-id="10b56-139">Se även</span><span class="sxs-lookup"><span data-stu-id="10b56-139">See also</span></span>
[<span data-ttu-id="10b56-140">Översikt över design-överväganden</span><span class="sxs-lookup"><span data-stu-id="10b56-140">Design considerations overview</span></span>](active-directory-hybrid-identity-design-considerations-overview.md)


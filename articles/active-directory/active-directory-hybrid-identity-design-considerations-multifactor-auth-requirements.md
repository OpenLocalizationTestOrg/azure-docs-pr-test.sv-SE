---
title: "designöverväganden för aaaAzure Active Directory hybrid identity - ange krav för multifaktorautentisering"
description: "Med villkorlig åtkomstkontroll kontrollerar hello särskilda villkor som du väljer när du autentiserar användaren hello och innan åtkomst toohello program i Azure Active Directory. När dessa villkor är uppfyllda, hello användare autentiseras och tillåtet åtkomst toohello program."
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
ms.openlocfilehash: 49fa7b43772fb3a2d6664747477c60a34cddde2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="determine-multi-factor-authentication-requirements-for-your-hybrid-identity-solution"></a><span data-ttu-id="96ed7-104">Ange krav för multifaktorautentisering för dina hybrididentitetslösning</span><span class="sxs-lookup"><span data-stu-id="96ed7-104">Determine multi-factor authentication requirements for your hybrid identity solution</span></span>
<span data-ttu-id="96ed7-105">I den här värld mobilitet, med användare som ansluter till data och program i molnet hello och från valfri enhet, blivit skydda den här informationen ytterst viktigt.</span><span class="sxs-lookup"><span data-stu-id="96ed7-105">In this world of mobility, with users accessing data and applications in hello cloud and from any device, securing this information has become paramount.</span></span>  <span data-ttu-id="96ed7-106">Varje dag är det en ny rubrik om ett intrång.</span><span class="sxs-lookup"><span data-stu-id="96ed7-106">Every day there is a new headline about a security breach.</span></span>  <span data-ttu-id="96ed7-107">Även om det är inte säkert mot sådana överträdelser, multifaktorautentisering, ger ett ytterligare lager av säkerhet toohelp förhindra dessa överträdelser.</span><span class="sxs-lookup"><span data-stu-id="96ed7-107">Although, there is no guarantee against such breaches, multi-factor authentication, provides an additional layer of security toohelp prevent these breaches.</span></span>
<span data-ttu-id="96ed7-108">Börja med att utvärdera hello organisationer krav för multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="96ed7-108">Start by evaluating hello organizations requirements for multi-factor authentication.</span></span> <span data-ttu-id="96ed7-109">Vad är hello organisation försök toosecure.</span><span class="sxs-lookup"><span data-stu-id="96ed7-109">That is, what is hello organization trying toosecure.</span></span>  <span data-ttu-id="96ed7-110">Denna utvärdering är viktiga toodefine hello tekniska krav för att installera och aktivera hello organisationer användare för multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="96ed7-110">This evaluation is important toodefine hello technical requirements for setting up and enabling hello organizations users for multi-factor authentication.</span></span>

> [!NOTE]
> <span data-ttu-id="96ed7-111">Om du inte är bekant med MFA och syfte rekommenderas starkt att du läser hello artikel [vad är Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md) tidigare toocontinue läser det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="96ed7-111">If you are not familiar with MFA and what it does, it is strongly recommended that you read hello article [What is Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md) prior toocontinue reading this section.</span></span>
> 
> 

<span data-ttu-id="96ed7-112">Se till att tooanswer hello följande:</span><span class="sxs-lookup"><span data-stu-id="96ed7-112">Make sure tooanswer hello following:</span></span>

* <span data-ttu-id="96ed7-113">Försöker toosecure Microsoft-appar i företaget?</span><span class="sxs-lookup"><span data-stu-id="96ed7-113">Is your company trying toosecure Microsoft apps?</span></span> 
* <span data-ttu-id="96ed7-114">Hur dessa appar är publicerade?</span><span class="sxs-lookup"><span data-stu-id="96ed7-114">How these apps are published?</span></span>
* <span data-ttu-id="96ed7-115">Företaget kan ge fjärråtkomst tooallow anställda tooaccess lokala appar?</span><span class="sxs-lookup"><span data-stu-id="96ed7-115">Does your company provide remote access tooallow employees tooaccess on-premises apps?</span></span>

<span data-ttu-id="96ed7-116">Om Ja, vilken typ av fjärråtkomst? Du måste också tooevaluate där hello användare som kommer åt dessa program kommer att finnas.</span><span class="sxs-lookup"><span data-stu-id="96ed7-116">If yes, what type of remote access?You also need tooevaluate where hello users who are accessing these applications will be located.</span></span> <span data-ttu-id="96ed7-117">Denna utvärdering är ett annat viktigt steg toodefine hello rätt multifaktorautentisering strategi.</span><span class="sxs-lookup"><span data-stu-id="96ed7-117">This evaluation is another important step toodefine hello proper multi-factor authentication strategy.</span></span> <span data-ttu-id="96ed7-118">Se till att tooanswer hello följande frågor:</span><span class="sxs-lookup"><span data-stu-id="96ed7-118">Make sure tooanswer hello following questions:</span></span>

* <span data-ttu-id="96ed7-119">Var finns hello användare ska toobe?</span><span class="sxs-lookup"><span data-stu-id="96ed7-119">Where are hello users going toobe located?</span></span>
* <span data-ttu-id="96ed7-120">Kan de finnas var som helst?</span><span class="sxs-lookup"><span data-stu-id="96ed7-120">Can they be located anywhere?</span></span>
* <span data-ttu-id="96ed7-121">Vill ditt företag tooestablish begränsningar enligt toohello användarens plats?</span><span class="sxs-lookup"><span data-stu-id="96ed7-121">Does your company want tooestablish restrictions according toohello user’s location?</span></span>

<span data-ttu-id="96ed7-122">När du förstår dessa krav är det viktigt tooalso utvärdera hello användarens krav för multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="96ed7-122">Once you understand these requirements, it is important tooalso evaluate hello user’s requirements for multi-factor authentication.</span></span> <span data-ttu-id="96ed7-123">Denna utvärdering är viktig eftersom den definierar hello krav för lansera multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="96ed7-123">This evaluation is important because it will define hello requirements for rolling out multi-factor authentication.</span></span> <span data-ttu-id="96ed7-124">Se till att tooanswer hello följande frågor:</span><span class="sxs-lookup"><span data-stu-id="96ed7-124">Make sure tooanswer hello following questions:</span></span>

* <span data-ttu-id="96ed7-125">Känner hello användare med Multi-Factor authentication?</span><span class="sxs-lookup"><span data-stu-id="96ed7-125">Are hello users familiar with multi-factor authentication?</span></span>
* <span data-ttu-id="96ed7-126">Vissa använder kommer vara nödvändiga tooprovide ytterligare autentisering?</span><span class="sxs-lookup"><span data-stu-id="96ed7-126">Will some uses be required tooprovide additional authentication?</span></span>  
  * <span data-ttu-id="96ed7-127">Om Ja, alla hello gången när de kommer från externa nätverk eller vid åtkomst till specifika program eller andra villkor?</span><span class="sxs-lookup"><span data-stu-id="96ed7-127">If yes, all hello time, when coming from external networks, or accessing specific applications, or under other conditions?</span></span>
* <span data-ttu-id="96ed7-128">Hello användare kräver utbildning om hur toosetup och implementera multifaktorautentisering?</span><span class="sxs-lookup"><span data-stu-id="96ed7-128">Will hello users require training on how toosetup and implement multi-factor authentication?</span></span>
* <span data-ttu-id="96ed7-129">Vad är hello viktiga scenarier som företaget vill tooenable Multi-Factor authentication för användarna?</span><span class="sxs-lookup"><span data-stu-id="96ed7-129">What are hello key scenarios that your company wants tooenable multi-factor authentication for their users?</span></span>

<span data-ttu-id="96ed7-130">När besvara hello föregående frågor, kommer du att kunna toounderstand om multifaktorautentisering redan implementerats lokala.</span><span class="sxs-lookup"><span data-stu-id="96ed7-130">After answering hello previous questions, you will be able toounderstand if there are multi-factor authentication already implemented on-premises.</span></span> <span data-ttu-id="96ed7-131">Denna utvärdering är viktiga toodefine hello tekniska krav för att installera och aktivera hello organisationer användare för multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="96ed7-131">This evaluation is important toodefine hello technical requirements for setting up and enabling hello organizations users for multi-factor authentication.</span></span> <span data-ttu-id="96ed7-132">Se till att tooanswer hello följande frågor:</span><span class="sxs-lookup"><span data-stu-id="96ed7-132">Make sure tooanswer hello following questions:</span></span>

* <span data-ttu-id="96ed7-133">Behöver företaget tooprotect Privilegierade konton med MFA?</span><span class="sxs-lookup"><span data-stu-id="96ed7-133">Does your company need tooprotect privileged accounts with MFA?</span></span>
* <span data-ttu-id="96ed7-134">Behöver företaget tooenable MFA för vissa program på grund av kompatibilitetsskäl?</span><span class="sxs-lookup"><span data-stu-id="96ed7-134">Does your company need tooenable MFA for certain application for compliance reasons?</span></span>
* <span data-ttu-id="96ed7-135">Behöver företaget tooenable MFA för alla berättigade användare av dessa program eller endast administratörer?</span><span class="sxs-lookup"><span data-stu-id="96ed7-135">Does your company need tooenable MFA for all eligible users of these application or only administrators?</span></span>
* <span data-ttu-id="96ed7-136">Du behöver har alltid aktiverat MFA eller bara när hello användare är inloggade utanför företagets nätverk?</span><span class="sxs-lookup"><span data-stu-id="96ed7-136">Do you need have MFA always enabled or only when hello users are logged outside of your corporate network?</span></span>

## <a name="next-steps"></a><span data-ttu-id="96ed7-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="96ed7-137">Next steps</span></span>
[<span data-ttu-id="96ed7-138">Definiera en strategi för införandet en hybrid identity</span><span class="sxs-lookup"><span data-stu-id="96ed7-138">Define a hybrid identity adoption strategy</span></span>](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)

## <a name="see-also"></a><span data-ttu-id="96ed7-139">Se även</span><span class="sxs-lookup"><span data-stu-id="96ed7-139">See also</span></span>
[<span data-ttu-id="96ed7-140">Översikt över design-överväganden</span><span class="sxs-lookup"><span data-stu-id="96ed7-140">Design considerations overview</span></span>](active-directory-hybrid-identity-design-considerations-overview.md)


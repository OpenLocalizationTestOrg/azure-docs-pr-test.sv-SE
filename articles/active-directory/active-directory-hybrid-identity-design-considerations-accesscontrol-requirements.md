---
title: "designöverväganden för aaaAzure Active Directory hybrid identity - fastställa krav på åtkomstkontroll | Microsoft Docs"
description: "Omfattar hello pelare för identitets- och identifiera krav för resurser för användare i en hybridmiljö."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: e3b3b984-0d15-4654-93be-a396324b9f5e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: f0c22629f732a4c13ee7a24456651bec7637c387
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="determine-access-control-requirements-for-your-hybrid-identity-solution"></a><span data-ttu-id="c4d5a-103">Fastställa krav på åtkomstkontroll för din hybrididentitetslösning</span><span class="sxs-lookup"><span data-stu-id="c4d5a-103">Determine access control requirements for your hybrid identity solution</span></span>
<span data-ttu-id="c4d5a-104">Vid utformning av en organisation sina hybrididentitetslösning som de kan också använda den här möjligheten tooreview åtkomstkrav för hello resurser som de planerar toomake den tillgänglig för användare.</span><span class="sxs-lookup"><span data-stu-id="c4d5a-104">When an organization is designing their hybrid identity solution they can also use this opportunity tooreview access requirements for hello resources that they are planning toomake it available for users.</span></span> <span data-ttu-id="c4d5a-105">hello dataåtkomst mellan alla fyra pelare identitet, som är:</span><span class="sxs-lookup"><span data-stu-id="c4d5a-105">hello data access cross all four pillars of identity, which are:</span></span>

* <span data-ttu-id="c4d5a-106">Administration</span><span class="sxs-lookup"><span data-stu-id="c4d5a-106">Administration</span></span>
* <span data-ttu-id="c4d5a-107">Autentisering</span><span class="sxs-lookup"><span data-stu-id="c4d5a-107">Authentication</span></span>
* <span data-ttu-id="c4d5a-108">Auktorisering</span><span class="sxs-lookup"><span data-stu-id="c4d5a-108">Authorization</span></span>
* <span data-ttu-id="c4d5a-109">Granskning</span><span class="sxs-lookup"><span data-stu-id="c4d5a-109">Auditing</span></span>

<span data-ttu-id="c4d5a-110">hello avsnitten som följer beskriver autentisering och auktorisering mer detaljerat, administration och granskning ingår i hello hybrid identity livscykel.</span><span class="sxs-lookup"><span data-stu-id="c4d5a-110">hello sections that follows will cover authentication and authorization in more details, administration and auditing are part of hello hybrid identity lifecycle.</span></span> <span data-ttu-id="c4d5a-111">Läs [fastställa hanteringsuppgifter för hybrid identity](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) mer information om dessa funktioner.</span><span class="sxs-lookup"><span data-stu-id="c4d5a-111">Read [Determine hybrid identity management tasks](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) for more information about these capabilities.</span></span>

> [!NOTE]
> <span data-ttu-id="c4d5a-112">Läs [hello fyra pelare av Identity - Identity Management i hello ålder av Hybrid-IT](http://social.technet.microsoft.com/wiki/contents/articles/15530.the-four-pillars-of-identity-identity-management-in-the-age-of-hybrid-it.aspx) mer information om var och en av dessa pelare.</span><span class="sxs-lookup"><span data-stu-id="c4d5a-112">Read [hello Four Pillars of Identity - Identity Management in hello Age of Hybrid IT](http://social.technet.microsoft.com/wiki/contents/articles/15530.the-four-pillars-of-identity-identity-management-in-the-age-of-hybrid-it.aspx) for more information about each one of those pillars.</span></span>
> 
> 

## <a name="authentication-and-authorization"></a><span data-ttu-id="c4d5a-113">Autentisering och auktorisering</span><span class="sxs-lookup"><span data-stu-id="c4d5a-113">Authentication and authorization</span></span>
<span data-ttu-id="c4d5a-114">Det finns olika scenarier för autentisering och auktorisering, dessa scenarier har specifika krav som måste uppfyllas av hello hybrididentitetslösning att hello företag är pågående tooadopt.</span><span class="sxs-lookup"><span data-stu-id="c4d5a-114">There are different scenarios for authentication and authorization, these scenarios will have specific requirements that must be fulfilled by hello hybrid identity solution that hello company is going tooadopt.</span></span> <span data-ttu-id="c4d5a-115">Scenarier som involverar Business tooBusiness (B2B) kommunikation kan lägga till en extra utmaning för IT-administratörer eftersom de måste tooensure hello autentisering och auktorisering metod som används av hello organisation kan kommunicera med sina affärspartner.</span><span class="sxs-lookup"><span data-stu-id="c4d5a-115">Scenarios involving Business tooBusiness (B2B) communication can add an extra challenge for IT Admins since they will need tooensure that hello authentication and authorization method used by hello organization can communicate with their business partners.</span></span> <span data-ttu-id="c4d5a-116">Under hello designa process för krav på autentisering och auktorisering, kontrollerar du att hello följande frågor besvaras:</span><span class="sxs-lookup"><span data-stu-id="c4d5a-116">During hello designing process for authentication and authorization requirements, ensure that hello following questions are answered:</span></span>

* <span data-ttu-id="c4d5a-117">Kommer din organisation autentisera och auktorisera endast användare som finns i deras identitet hanteringssystemet?</span><span class="sxs-lookup"><span data-stu-id="c4d5a-117">Will your organization authenticate and authorize only users located at their identity management system?</span></span>
  * <span data-ttu-id="c4d5a-118">Finns det några planer för B2B-scenarier?</span><span class="sxs-lookup"><span data-stu-id="c4d5a-118">Are there any plans for B2B scenarios?</span></span>
  * <span data-ttu-id="c4d5a-119">Om Ja, du redan vet vilka protokoll (SAML OAuth, Kerberos, token eller certifikat) kommer att använda tooconnect både företag?</span><span class="sxs-lookup"><span data-stu-id="c4d5a-119">If yes, do you already know which protocols (SAML, OAuth, Kerberos, Tokens or Certificates) will be used tooconnect both businesses?</span></span>
* <span data-ttu-id="c4d5a-120">Stöder hello hybrididentitetslösning att du ska tooadopt dessa protokoll?</span><span class="sxs-lookup"><span data-stu-id="c4d5a-120">Does hello hybrid identity solution that you are going tooadopt support those protocols?</span></span>

<span data-ttu-id="c4d5a-121">En annan viktig punkt tooconsider är där hello autentisering databasen som ska användas av användare och partners kommer att finnas och hello administrativa modellen toobe används.</span><span class="sxs-lookup"><span data-stu-id="c4d5a-121">Another important point tooconsider is where hello authentication repository that will be used by users and partners will be located and hello administrative model toobe used.</span></span> <span data-ttu-id="c4d5a-122">Överväg följande två alternativ för kärnor hello:</span><span class="sxs-lookup"><span data-stu-id="c4d5a-122">Consider hello following two core options:</span></span>

* <span data-ttu-id="c4d5a-123">Centraliserad: i den här modellen hello användarens autentiseringsuppgifter, principer och administration kan vara lokalt centraliserad eller i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="c4d5a-123">Centralized: in this model hello user’s credentials, policies and administration can be centralized on-premises or in hello cloud.</span></span>
* <span data-ttu-id="c4d5a-124">Hybrid: i den här modellen hello användarens autentiseringsuppgifter, principer och administration ska finnas lokalt centraliserad och en replikerad i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="c4d5a-124">Hybrid: in this model hello user’s credentials, policies and administration will be centralized on-premises and a replicated in hello cloud.</span></span>

<span data-ttu-id="c4d5a-125">Vilken modell som företaget antar varierar bl.a tootheir affärsbehov vill du tooanswer hello följande frågor tooidentify där hello identity management-systemet finns och hello administrativa läge toouse:</span><span class="sxs-lookup"><span data-stu-id="c4d5a-125">Which model your organization will adopt will vary according tootheir business requirements, you want tooanswer hello following questions tooidentify where hello identity management system will reside and hello administrative mode toouse:</span></span>

* <span data-ttu-id="c4d5a-126">För närvarande har organisationen en Identitetshantering lokalt?</span><span class="sxs-lookup"><span data-stu-id="c4d5a-126">Does your organization currently have an identity management on-premises?</span></span>
  * <span data-ttu-id="c4d5a-127">Om Ja, de planerar tookeep den?</span><span class="sxs-lookup"><span data-stu-id="c4d5a-127">If yes, do they plan tookeep it?</span></span>
  * <span data-ttu-id="c4d5a-128">Finns det några förordning eller efterlevnad krav som din organisation måste följa de bestämmer där hello identity management-systemet ska finnas?</span><span class="sxs-lookup"><span data-stu-id="c4d5a-128">Are there any regulation or compliance requirements that your organization must follow that dictates where hello identity management system should reside?</span></span>
* <span data-ttu-id="c4d5a-129">Använder organisationen enkel inloggning för appar som finns lokalt eller i molnet hello?</span><span class="sxs-lookup"><span data-stu-id="c4d5a-129">Does your organization use single sign-on for apps located on-premises or in hello cloud?</span></span>
  * <span data-ttu-id="c4d5a-130">Om Ja, hello införandet av en modell för hybrid identity påverkar processen?</span><span class="sxs-lookup"><span data-stu-id="c4d5a-130">If yes, does hello adoption of a hybrid identity model affect this process?</span></span>

## <a name="access-control"></a><span data-ttu-id="c4d5a-131">Access Control</span><span class="sxs-lookup"><span data-stu-id="c4d5a-131">Access Control</span></span>
<span data-ttu-id="c4d5a-132">Autentisering och auktorisering är core element tooenable åtkomst toocorporate data via användarens verifiering, är det också viktigt toocontrol hello åtkomstnivå som användarna har och hello andelen administratörer har över hello resurser som de hanterar.</span><span class="sxs-lookup"><span data-stu-id="c4d5a-132">While authentication and authorization are core elements tooenable access toocorporate data through user’s validation, it is also important toocontrol hello level of access that these users will have and hello level of access administrators will have over hello resources that they are managing.</span></span> <span data-ttu-id="c4d5a-133">Din hybrididentitetslösning måste vara kan tooprovide detaljerade åtkomst tooresources delegering och rollbaserad åtkomstkontroll.</span><span class="sxs-lookup"><span data-stu-id="c4d5a-133">Your hybrid identity solution must be able tooprovide granular access tooresources, delegation and role base access control.</span></span> <span data-ttu-id="c4d5a-134">Se till att följande fråga hello besvaras om åtkomstkontroll:</span><span class="sxs-lookup"><span data-stu-id="c4d5a-134">Ensure that hello following question are answered regarding access control:</span></span>

* <span data-ttu-id="c4d5a-135">Ditt företag har fler än en användare med förhöjda privilegier toomanage systemet identitet?</span><span class="sxs-lookup"><span data-stu-id="c4d5a-135">Does your company have more than one user with elevated privilege toomanage your identity system?</span></span>
  * <span data-ttu-id="c4d5a-136">Om Ja, måste varje användare hello samma åtkomstnivå?</span><span class="sxs-lookup"><span data-stu-id="c4d5a-136">If yes, does each user need hello same access level?</span></span>
* <span data-ttu-id="c4d5a-137">Skulle företagets behov toodelegate åtkomst toousers toomanage specifika resurser?</span><span class="sxs-lookup"><span data-stu-id="c4d5a-137">Would your company need toodelegate access toousers toomanage specific resources?</span></span>
  * <span data-ttu-id="c4d5a-138">Om Ja, hur ofta detta sker?</span><span class="sxs-lookup"><span data-stu-id="c4d5a-138">If yes, how frequently this happens?</span></span>
* <span data-ttu-id="c4d5a-139">Behöver företaget toointegrate kontroll åtkomstfunktioner mellan lokala och moln resurser?</span><span class="sxs-lookup"><span data-stu-id="c4d5a-139">Would your company need toointegrate access control capabilities between on-premises and cloud resources?</span></span>
* <span data-ttu-id="c4d5a-140">Behöver företaget toolimit åtkomst tooresources enligt toosome villkor?</span><span class="sxs-lookup"><span data-stu-id="c4d5a-140">Would your company need toolimit access tooresources according toosome conditions?</span></span>
* <span data-ttu-id="c4d5a-141">Företaget skulle ha alla program som behöver en anpassad kontroll åtkomst toosome resurser?</span><span class="sxs-lookup"><span data-stu-id="c4d5a-141">Would your company have any application that needs custom control access toosome resources?</span></span>
  * <span data-ttu-id="c4d5a-142">Om Ja, var apparna finns (lokalt eller i molnet hello)?</span><span class="sxs-lookup"><span data-stu-id="c4d5a-142">If yes, where are those apps located (on-premises or in hello cloud)?</span></span>
  * <span data-ttu-id="c4d5a-143">Om Ja, där är de mål resurser (lokalt eller i molnet hello)?</span><span class="sxs-lookup"><span data-stu-id="c4d5a-143">If yes, where are those target resources located (on-premises or in hello cloud)?</span></span>

> [!NOTE]
> <span data-ttu-id="c4d5a-144">Se till att tootake anteckningar för varje svar och förstå hello anledningen hello svaret.</span><span class="sxs-lookup"><span data-stu-id="c4d5a-144">Make sure tootake notes of each answer and understand hello rationale behind hello answer.</span></span> <span data-ttu-id="c4d5a-145">[Definiera en strategi för Data Protection](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) kommer att överskrida hello tillgängliga alternativ och fördelar/nackdelar med varje alternativ.</span><span class="sxs-lookup"><span data-stu-id="c4d5a-145">[Define Data Protection Strategy](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) will go over hello options available and advantages/disadvantages of each option.</span></span>  <span data-ttu-id="c4d5a-146">Genom att besvara frågorna väljer du vilket alternativ som bäst passar dina affärsbehov.</span><span class="sxs-lookup"><span data-stu-id="c4d5a-146">By answering those questions you will select which option best suits your business needs.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="c4d5a-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c4d5a-147">Next steps</span></span>
[<span data-ttu-id="c4d5a-148">Fastställa kraven på incidentsvar</span><span class="sxs-lookup"><span data-stu-id="c4d5a-148">Determine incident response requirements</span></span>](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="see-also"></a><span data-ttu-id="c4d5a-149">Se även</span><span class="sxs-lookup"><span data-stu-id="c4d5a-149">See Also</span></span>
[<span data-ttu-id="c4d5a-150">Översikt över design-överväganden</span><span class="sxs-lookup"><span data-stu-id="c4d5a-150">Design considerations overview</span></span>](active-directory-hybrid-identity-design-considerations-overview.md)


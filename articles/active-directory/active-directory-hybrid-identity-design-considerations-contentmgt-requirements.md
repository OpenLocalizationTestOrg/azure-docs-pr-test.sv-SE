---
title: "designöverväganden för aaaAzure Active Directory hybrid identity - fastställa kraven för innehållshantering | Microsoft Docs"
description: "Ger inblick i hur toodetermine hello innehållshantering kraven i företaget. Vanligtvis kanske när en användare har sin egen enhet han också flera autentiseringsuppgifter som ska alternerande bl.a toohello program som han använder. Det är viktigt toodifferentiate vilka innehållet har skapats med hjälp av personliga autentiseringsuppgifter jämfört med hello som skapas med företagets autentiseringsuppgifter. Din identitetslösning ska vara kan toointeract med cloud services tooprovide en smidigt toohello slutanvändare vid säkerställa hans sekretess och öka hello skydd mot dataläckage."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: dd1ef776-db4d-4ab8-9761-2adaa5a4f004
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 607d366633c37b65ec5cf8ae5c64d73ca1cc96b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="determine-content-management-requirements-for-your-hybrid-identity-solution"></a><span data-ttu-id="50f85-106">Ange krav för innehållshantering för din hybrididentitetslösning</span><span class="sxs-lookup"><span data-stu-id="50f85-106">Determine content management requirements for your hybrid identity solution</span></span>
<span data-ttu-id="50f85-107">Så här fungerar hello innehållshantering kraven påverka för din verksamhet kan direkt ditt beslut på vilka hybrid identity lösning toouse.</span><span class="sxs-lookup"><span data-stu-id="50f85-107">Understanding hello content management requirements for your business may direct affect your decision on which hybrid identity solution toouse.</span></span> <span data-ttu-id="50f85-108">Med hello spridning av flera enheter och hello möjligheterna för användare toobring sina egna enheter ([BYOD](http://aka.ms/byodcg)), hello företag måste skydda egna data men den måste behålla användarens integritet intakta.</span><span class="sxs-lookup"><span data-stu-id="50f85-108">With hello proliferation of multiple devices and hello capability of users toobring their own devices ([BYOD](http://aka.ms/byodcg)), hello company must protect its own data but it also must keep user’s privacy intact.</span></span> <span data-ttu-id="50f85-109">Vanligtvis kanske när en användare har sin egen enhet han också flera autentiseringsuppgifter som ska alternerande bl.a toohello program som han använder.</span><span class="sxs-lookup"><span data-stu-id="50f85-109">Usually when a user has his own device he might have also multiple credentials that will be alternating according toohello application that he uses.</span></span> <span data-ttu-id="50f85-110">Det är viktigt toodifferentiate vilka innehållet har skapats med hjälp av personliga autentiseringsuppgifter jämfört med hello som skapas med företagets autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="50f85-110">It is important toodifferentiate what content was created using personal credentials versus hello ones created using corporate credentials.</span></span> <span data-ttu-id="50f85-111">Din identitetslösning ska vara kan toointeract med cloud services tooprovide en smidigt toohello slutanvändare vid säkerställa hans sekretess och öka hello skydd mot dataläckage.</span><span class="sxs-lookup"><span data-stu-id="50f85-111">Your identity solution should be able toointeract with cloud services tooprovide a seamless experience toohello end user while ensure his privacy and increase hello protection against data leakage.</span></span> 

<span data-ttu-id="50f85-112">Identity-lösningen kommer utnyttjas av olika tekniska kontroller i ordning tooprovide innehållshantering enligt hello bilden nedan:</span><span class="sxs-lookup"><span data-stu-id="50f85-112">Your identity solution will be leveraged by different technical controls in order tooprovide content management as shown in hello figure below:</span></span>

![](./media/hybrid-id-design-considerations/securitycontrols.png)

<span data-ttu-id="50f85-113">**Säkerhetsåtgärder som kommer kunna utnyttja systemet identity management**</span><span class="sxs-lookup"><span data-stu-id="50f85-113">**Security controls that will be leveraging your identity management system**</span></span>

<span data-ttu-id="50f85-114">I allmänhet använder innehållshantering krav din identitet hanteringssystemet hello följande områden:</span><span class="sxs-lookup"><span data-stu-id="50f85-114">In general, content management requirements will leverage your identity management system in hello following areas:</span></span>

* <span data-ttu-id="50f85-115">Sekretess: identifiera hello-användaren som äger en resurs och tillämpa hello lämpliga kontroller toomaintain integritet.</span><span class="sxs-lookup"><span data-stu-id="50f85-115">Privacy: identifying hello user that owns a resource and applying hello appropriate controls toomaintain integrity.</span></span>
* <span data-ttu-id="50f85-116">Dataklassificering: identifiera hello användaren eller gruppen och andelen tooan objektåtkomst enligt tooits klassificering.</span><span class="sxs-lookup"><span data-stu-id="50f85-116">Data Classification: identify hello user or group and level of access tooan object according tooits classification.</span></span> 
* <span data-ttu-id="50f85-117">Läckage av dataskydd: säkerhetsåtgärder som är ansvarig för att skydda tooavoid dataläckage behöver toointeract med hello identitet system toovalidate hello användarens identitet.</span><span class="sxs-lookup"><span data-stu-id="50f85-117">Data Leakage Protection: security controls responsible for protecting data tooavoid leakage will need toointeract with hello identity system toovalidate hello user’s identity.</span></span> <span data-ttu-id="50f85-118">Detta är även viktigt för spår revision.</span><span class="sxs-lookup"><span data-stu-id="50f85-118">This is also important for auditing trail purpose.</span></span>

> [!NOTE]
> <span data-ttu-id="50f85-119">Läs [dataklassificering moln beredskap för](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) mer information om metodtips och riktlinjer för dataklassificering.</span><span class="sxs-lookup"><span data-stu-id="50f85-119">Read [data classification for cloud readiness](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) for more information about best practices and guidelines for data classification.</span></span>
> 
> 

<span data-ttu-id="50f85-120">När du planerar din hybrididentitetslösning se till att hello följande besvaras frågor enligt tooyour organisations behov:</span><span class="sxs-lookup"><span data-stu-id="50f85-120">When planning your hybrid identity solution ensure that hello following questions are answered according tooyour organization’s requirements:</span></span>

* <span data-ttu-id="50f85-121">Har företaget säkerhetsåtgärder i plats tooenforce datasekretess?</span><span class="sxs-lookup"><span data-stu-id="50f85-121">Does your company have security controls in place tooenforce data privacy?</span></span>
  * <span data-ttu-id="50f85-122">Om Ja, kommer dessa säkerhetsåtgärder att kan toointegrate med hello hybrididentitetslösning att du är pågående tooadopt?</span><span class="sxs-lookup"><span data-stu-id="50f85-122">If yes, will those security controls be able toointegrate with hello hybrid identity solution that you are going tooadopt?</span></span>
* <span data-ttu-id="50f85-123">Använder företaget dataklassificering?</span><span class="sxs-lookup"><span data-stu-id="50f85-123">Does your company use data classification?</span></span>
  * <span data-ttu-id="50f85-124">Om Ja, är hello aktuella lösningen kan toointegrate med hello hybrididentitetslösning att du är pågående tooadopt?</span><span class="sxs-lookup"><span data-stu-id="50f85-124">If yes, is hello current solution able toointegrate with hello hybrid identity solution that you are going tooadopt?</span></span>
* <span data-ttu-id="50f85-125">Har ditt företag för närvarande någon lösning för dataläckage?</span><span class="sxs-lookup"><span data-stu-id="50f85-125">Does your company currently have any solution for data leakage?</span></span> 
  * <span data-ttu-id="50f85-126">Om Ja, är hello aktuella lösningen kan toointegrate med hello hybrididentitetslösning att du är pågående tooadopt?</span><span class="sxs-lookup"><span data-stu-id="50f85-126">If yes, is hello current solution able toointegrate with hello hybrid identity solution that you are going tooadopt?</span></span>
* <span data-ttu-id="50f85-127">Behöver företaget tooaudit åtkomst tooresources?</span><span class="sxs-lookup"><span data-stu-id="50f85-127">Does your company need tooaudit access tooresources?</span></span>
  * <span data-ttu-id="50f85-128">Om Ja, vilken typ av resurser?</span><span class="sxs-lookup"><span data-stu-id="50f85-128">If yes, what type of resources?</span></span>
  * <span data-ttu-id="50f85-129">Om Ja, hur mycket information som krävs?</span><span class="sxs-lookup"><span data-stu-id="50f85-129">If yes, what level of information is necessary?</span></span>
  * <span data-ttu-id="50f85-130">Om Ja, där hello granskningsloggen måste finnas?</span><span class="sxs-lookup"><span data-stu-id="50f85-130">If yes, where hello audit log must reside?</span></span> <span data-ttu-id="50f85-131">Lokalt eller i molnet hello?</span><span class="sxs-lookup"><span data-stu-id="50f85-131">On-premises or in hello cloud?</span></span>
* <span data-ttu-id="50f85-132">Behöver företaget tooencrypt alla e-postmeddelanden som innehåller känsliga data (personnummer, kreditkortsnummer o.s.v.)?</span><span class="sxs-lookup"><span data-stu-id="50f85-132">Does your company need tooencrypt any emails that contain sensitive data (SSNs, credit card numbers, etc)?</span></span>
* <span data-ttu-id="50f85-133">Behöver företaget tooencrypt alla dokument/innehållet delas med affärspartners?</span><span class="sxs-lookup"><span data-stu-id="50f85-133">Does your company need tooencrypt all documents/contents shared with external business partners?</span></span>
* <span data-ttu-id="50f85-134">Behöver företaget tooenforce företagsprinciper på vissa typer av e-postmeddelanden (gör något alla svar, vidarebefordra inte)?</span><span class="sxs-lookup"><span data-stu-id="50f85-134">Does your company need tooenforce corporate policies on certain kinds of emails (do no reply all, do not forward)?</span></span>

> [!NOTE]
> <span data-ttu-id="50f85-135">Se till att tootake anteckningar för varje svar och förstå hello anledningen hello svaret.</span><span class="sxs-lookup"><span data-stu-id="50f85-135">Make sure tootake notes of each answer and understand hello rationale behind hello answer.</span></span> <span data-ttu-id="50f85-136">[Definiera en strategi för Data Protection](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) kommer att överskrida hello tillgängliga alternativ och fördelar/nackdelar med varje alternativ.</span><span class="sxs-lookup"><span data-stu-id="50f85-136">[Define Data Protection Strategy](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) will go over hello options available and advantages/disadvantages of each option.</span></span>  <span data-ttu-id="50f85-137">Har besvarat frågorna väljer du vilket alternativ som bäst passar dina behöver.</span><span class="sxs-lookup"><span data-stu-id="50f85-137">By having answered those questions you will select which option best suits your business needs.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="50f85-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="50f85-138">Next steps</span></span>
[<span data-ttu-id="50f85-139">Fastställa krav på åtkomstkontroll</span><span class="sxs-lookup"><span data-stu-id="50f85-139">Determine access control requirements</span></span>](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)

## <a name="see-also"></a><span data-ttu-id="50f85-140">Se även</span><span class="sxs-lookup"><span data-stu-id="50f85-140">See Also</span></span>
[<span data-ttu-id="50f85-141">Översikt över design-överväganden</span><span class="sxs-lookup"><span data-stu-id="50f85-141">Design considerations overview</span></span>](active-directory-hybrid-identity-design-considerations-overview.md)


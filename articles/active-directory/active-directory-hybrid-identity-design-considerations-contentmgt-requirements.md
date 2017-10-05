---
title: "Azure Active Directory hybrid identity designöverväganden - fastställa kraven för innehållshantering | Microsoft Docs"
description: "Ger kunskaper om hur du fastställer kraven för innehållshantering i ditt företag. Vanligtvis kanske när en användare har sin egen enhet han också flera autentiseringsuppgifter som ska alternerande enligt det program som han använder. Det är viktigt att skilja mellan vilka innehållet har skapats med hjälp av personliga autentiseringsuppgifter jämfört med de som skapas med företagets autentiseringsuppgifter. Identitetslösning bör kunna interagera med cloud services och ger en sömlös upplevelse för slutanvändaren när säkerställa sina sekretess och ökar skyddet mot dataläckage."
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
ms.openlocfilehash: 840de1e1fcba74285788d51d8f544375f0affa77
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="determine-content-management-requirements-for-your-hybrid-identity-solution"></a><span data-ttu-id="a1e97-106">Ange krav för innehållshantering för din hybrididentitetslösning</span><span class="sxs-lookup"><span data-stu-id="a1e97-106">Determine content management requirements for your hybrid identity solution</span></span>
<span data-ttu-id="a1e97-107">Förstå kraven för innehållshantering för ditt företag kan direkt påverka ditt beslut om vilken hybrididentitetslösning som ska användas.</span><span class="sxs-lookup"><span data-stu-id="a1e97-107">Understanding the content management requirements for your business may direct affect your decision on which hybrid identity solution to use.</span></span> <span data-ttu-id="a1e97-108">Med den ökande mängden av flera enheter och göra att användare kan ha sina egna enheter ([BYOD](http://aka.ms/byodcg)), måste företaget skydda egna data men den måste behålla användarens integritet intakta.</span><span class="sxs-lookup"><span data-stu-id="a1e97-108">With the proliferation of multiple devices and the capability of users to bring their own devices ([BYOD](http://aka.ms/byodcg)), the company must protect its own data but it also must keep user’s privacy intact.</span></span> <span data-ttu-id="a1e97-109">Vanligtvis kanske när en användare har sin egen enhet han också flera autentiseringsuppgifter som ska alternerande enligt det program som han använder.</span><span class="sxs-lookup"><span data-stu-id="a1e97-109">Usually when a user has his own device he might have also multiple credentials that will be alternating according to the application that he uses.</span></span> <span data-ttu-id="a1e97-110">Det är viktigt att skilja mellan vilka innehållet har skapats med hjälp av personliga autentiseringsuppgifter jämfört med de som skapas med företagets autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="a1e97-110">It is important to differentiate what content was created using personal credentials versus the ones created using corporate credentials.</span></span> <span data-ttu-id="a1e97-111">Identitetslösning bör kunna interagera med cloud services och ger en sömlös upplevelse för slutanvändaren när säkerställa sina sekretess och ökar skyddet mot dataläckage.</span><span class="sxs-lookup"><span data-stu-id="a1e97-111">Your identity solution should be able to interact with cloud services to provide a seamless experience to the end user while ensure his privacy and increase the protection against data leakage.</span></span> 

<span data-ttu-id="a1e97-112">Identity-lösningen kommer utnyttjas av olika tekniska kontroller för att ge innehållshantering som visas i bilden nedan:</span><span class="sxs-lookup"><span data-stu-id="a1e97-112">Your identity solution will be leveraged by different technical controls in order to provide content management as shown in the figure below:</span></span>

![](./media/hybrid-id-design-considerations/securitycontrols.png)

<span data-ttu-id="a1e97-113">**Säkerhetsåtgärder som kommer kunna utnyttja systemet identity management**</span><span class="sxs-lookup"><span data-stu-id="a1e97-113">**Security controls that will be leveraging your identity management system**</span></span>

<span data-ttu-id="a1e97-114">I allmänhet använder innehållshantering krav din identitet hanteringssystemet inom följande områden:</span><span class="sxs-lookup"><span data-stu-id="a1e97-114">In general, content management requirements will leverage your identity management system in the following areas:</span></span>

* <span data-ttu-id="a1e97-115">Sekretess: identifiera användaren som äger en resurs och tillämpa lämpliga kontroller för att bibehålla dataintegritet.</span><span class="sxs-lookup"><span data-stu-id="a1e97-115">Privacy: identifying the user that owns a resource and applying the appropriate controls to maintain integrity.</span></span>
* <span data-ttu-id="a1e97-116">Dataklassificering: identifiera användaren eller gruppen och åtkomst till ett objekt enligt sin klassificering.</span><span class="sxs-lookup"><span data-stu-id="a1e97-116">Data Classification: identify the user or group and level of access to an object according to its classification.</span></span> 
* <span data-ttu-id="a1e97-117">Läckage av dataskydd: säkerhetsåtgärder som är ansvarig för att skydda data för att undvika läckage behöver interagera med identitetssystem för att verifiera användarens identitet.</span><span class="sxs-lookup"><span data-stu-id="a1e97-117">Data Leakage Protection: security controls responsible for protecting data to avoid leakage will need to interact with the identity system to validate the user’s identity.</span></span> <span data-ttu-id="a1e97-118">Detta är även viktigt för spår revision.</span><span class="sxs-lookup"><span data-stu-id="a1e97-118">This is also important for auditing trail purpose.</span></span>

> [!NOTE]
> <span data-ttu-id="a1e97-119">Läs [dataklassificering moln beredskap för](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) mer information om metodtips och riktlinjer för dataklassificering.</span><span class="sxs-lookup"><span data-stu-id="a1e97-119">Read [data classification for cloud readiness](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) for more information about best practices and guidelines for data classification.</span></span>
> 
> 

<span data-ttu-id="a1e97-120">När planera din hybrididentitetslösning för att säkerställa att följande frågor besvaras enligt organisationens behov:</span><span class="sxs-lookup"><span data-stu-id="a1e97-120">When planning your hybrid identity solution ensure that the following questions are answered according to your organization’s requirements:</span></span>

* <span data-ttu-id="a1e97-121">Har företaget säkerhetsåtgärder för att genomdriva datasekretess?</span><span class="sxs-lookup"><span data-stu-id="a1e97-121">Does your company have security controls in place to enforce data privacy?</span></span>
  * <span data-ttu-id="a1e97-122">Om Ja, kommer dessa säkerhetsåtgärder att kunna integreras med hybrididentitetslösning som du kommer att anta?</span><span class="sxs-lookup"><span data-stu-id="a1e97-122">If yes, will those security controls be able to integrate with the hybrid identity solution that you are going to adopt?</span></span>
* <span data-ttu-id="a1e97-123">Använder företaget dataklassificering?</span><span class="sxs-lookup"><span data-stu-id="a1e97-123">Does your company use data classification?</span></span>
  * <span data-ttu-id="a1e97-124">Om Ja, kan den aktuella lösningen integreras med hybrididentitetslösning som du kommer att anta?</span><span class="sxs-lookup"><span data-stu-id="a1e97-124">If yes, is the current solution able to integrate with the hybrid identity solution that you are going to adopt?</span></span>
* <span data-ttu-id="a1e97-125">Har ditt företag för närvarande någon lösning för dataläckage?</span><span class="sxs-lookup"><span data-stu-id="a1e97-125">Does your company currently have any solution for data leakage?</span></span> 
  * <span data-ttu-id="a1e97-126">Om Ja, kan den aktuella lösningen integreras med hybrididentitetslösning som du kommer att anta?</span><span class="sxs-lookup"><span data-stu-id="a1e97-126">If yes, is the current solution able to integrate with the hybrid identity solution that you are going to adopt?</span></span>
* <span data-ttu-id="a1e97-127">Behöver ditt företag kunna granska åtkomsten till resurser?</span><span class="sxs-lookup"><span data-stu-id="a1e97-127">Does your company need to audit access to resources?</span></span>
  * <span data-ttu-id="a1e97-128">Om Ja, vilken typ av resurser?</span><span class="sxs-lookup"><span data-stu-id="a1e97-128">If yes, what type of resources?</span></span>
  * <span data-ttu-id="a1e97-129">Om Ja, hur mycket information som krävs?</span><span class="sxs-lookup"><span data-stu-id="a1e97-129">If yes, what level of information is necessary?</span></span>
  * <span data-ttu-id="a1e97-130">Om Ja, där granskningsloggen måste finnas?</span><span class="sxs-lookup"><span data-stu-id="a1e97-130">If yes, where the audit log must reside?</span></span> <span data-ttu-id="a1e97-131">Lokalt eller i molnet?</span><span class="sxs-lookup"><span data-stu-id="a1e97-131">On-premises or in the cloud?</span></span>
* <span data-ttu-id="a1e97-132">Behöver ditt företag att kryptera alla e-postmeddelanden som innehåller känsliga data (personnummer, kreditkortsnummer o.s.v.)?</span><span class="sxs-lookup"><span data-stu-id="a1e97-132">Does your company need to encrypt any emails that contain sensitive data (SSNs, credit card numbers, etc)?</span></span>
* <span data-ttu-id="a1e97-133">Behöver ditt företag att kryptera alla dokument/innehållet delas med affärspartners?</span><span class="sxs-lookup"><span data-stu-id="a1e97-133">Does your company need to encrypt all documents/contents shared with external business partners?</span></span>
* <span data-ttu-id="a1e97-134">Behöver företaget att genomdriva företagsprinciper på vissa typer av e-postmeddelanden (gör något alla svar, vidarebefordra inte)?</span><span class="sxs-lookup"><span data-stu-id="a1e97-134">Does your company need to enforce corporate policies on certain kinds of emails (do no reply all, do not forward)?</span></span>

> [!NOTE]
> <span data-ttu-id="a1e97-135">Se till att varje svar och försök förstå anledningen till svaret.</span><span class="sxs-lookup"><span data-stu-id="a1e97-135">Make sure to take notes of each answer and understand the rationale behind the answer.</span></span> <span data-ttu-id="a1e97-136">[Definiera en strategi för Data Protection](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) kommer att överskrida det tillgängliga alternativ och fördelar/nackdelar med varje alternativ.</span><span class="sxs-lookup"><span data-stu-id="a1e97-136">[Define Data Protection Strategy](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) will go over the options available and advantages/disadvantages of each option.</span></span>  <span data-ttu-id="a1e97-137">Har besvarat frågorna väljer du vilket alternativ som bäst passar dina behöver.</span><span class="sxs-lookup"><span data-stu-id="a1e97-137">By having answered those questions you will select which option best suits your business needs.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="a1e97-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a1e97-138">Next steps</span></span>
[<span data-ttu-id="a1e97-139">Fastställa krav på åtkomstkontroll</span><span class="sxs-lookup"><span data-stu-id="a1e97-139">Determine access control requirements</span></span>](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)

## <a name="see-also"></a><span data-ttu-id="a1e97-140">Se även</span><span class="sxs-lookup"><span data-stu-id="a1e97-140">See Also</span></span>
[<span data-ttu-id="a1e97-141">Översikt över design-överväganden</span><span class="sxs-lookup"><span data-stu-id="a1e97-141">Design considerations overview</span></span>](active-directory-hybrid-identity-design-considerations-overview.md)


---
title: "aaaLearn om tvåstegsverifiering i Azure MFA | Microsoft Docs"
description: "Vad är Azure Multi-Factor Authentication, varför ska jag använda MFA, mer information om hello Multi-Factor Authentication-klienten och hello olika metoder och versioner som är tillgängliga. "
keywords: "Introduktion tooMFA mfa översikt vad är mfa"
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: c40d7a34-1274-4496-96b0-784850c06e9b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/03/2017
ms.author: kgremban
ms.openlocfilehash: a91b8d6941d2b6ce72a789a97c57e10e594e7ada
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-multi-factor-authentication"></a><span data-ttu-id="7778d-104">Vad är Azure Multi-Factor Authentication?</span><span class="sxs-lookup"><span data-stu-id="7778d-104">What is Azure Multi-Factor Authentication?</span></span>
<span data-ttu-id="7778d-105">Tvåstegsverifiering är en autentiseringsmetod som kräver mer än en verifieringsmetod och lägger till ett kritiskt andra säkerhetslager säkerhet toouser inloggningar och transaktioner.</span><span class="sxs-lookup"><span data-stu-id="7778d-105">Two-step verification is a method of authentication that requires more than one verification method and adds a critical second layer of security toouser sign-ins and transactions.</span></span> <span data-ttu-id="7778d-106">Det fungerar genom att två eller flera av följande verifieringsmetoderna hello:</span><span class="sxs-lookup"><span data-stu-id="7778d-106">It works by requiring any two or more of hello following verification methods:</span></span>

* <span data-ttu-id="7778d-107">Något du vet (normalt ett lösenord)</span><span class="sxs-lookup"><span data-stu-id="7778d-107">Something you know (typically a password)</span></span>
* <span data-ttu-id="7778d-108">Något du har (betrodda enheter som inte enkelt dubbleras, t.ex. en telefon)</span><span class="sxs-lookup"><span data-stu-id="7778d-108">Something you have (a trusted device that is not easily duplicated, like a phone)</span></span>
* <span data-ttu-id="7778d-109">Något du är (biometrik)</span><span class="sxs-lookup"><span data-stu-id="7778d-109">Something you are (biometrics)</span></span>

<span data-ttu-id="7778d-110"><center>![Användarnamn och lösenord](./media/multi-factor-authentication/pword.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![certifikat](./media/multi-factor-authentication/phone.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![Smart Phone](./media/multi-factor-authentication/hware.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![smartkort](./media/multi-factor-authentication/smart.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![Virtuellt smartkort](./media/multi-factor-authentication/vsmart.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![användarnamn och Lösenord](./media/multi-factor-authentication/cert.png)</center></span><span class="sxs-lookup"><span data-stu-id="7778d-110"><center>![Username and Password](./media/multi-factor-authentication/pword.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Certificates](./media/multi-factor-authentication/phone.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Smart Phone](./media/multi-factor-authentication/hware.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Smart Card](./media/multi-factor-authentication/smart.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Virtual Smart Card](./media/multi-factor-authentication/vsmart.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Username and Password](./media/multi-factor-authentication/cert.png)</center></span></span>

<span data-ttu-id="7778d-111">Azure Multi-Factor Authentication (MFA) är Microsofts verifieringslösning i två steg.</span><span class="sxs-lookup"><span data-stu-id="7778d-111">Azure Multi-Factor Authentication (MFA) is Microsoft's two-step verification solution.</span></span> <span data-ttu-id="7778d-112">Azure MFA hjälper dig att skydda åtkomst toodata och program och uppfyller efterfrågan från användarna för en process för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7778d-112">Azure MFA helps safeguard access toodata and applications while meeting user demand for a simple sign-in process.</span></span> <span data-ttu-id="7778d-113">Den ger stark autentisering via en mängd verifieringsmetoder, inklusive telefonsamtal, textmeddelande eller verifiering av mobilappar.</span><span class="sxs-lookup"><span data-stu-id="7778d-113">It delivers strong authentication via a range of verification methods, including phone call, text message, or mobile app verification.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/WA-MFA-Overview/player]
>
>

## <a name="why-use-azure-multi-factor-authentication"></a><span data-ttu-id="7778d-114">Varför använda Azure Multi-Factor Authentication?</span><span class="sxs-lookup"><span data-stu-id="7778d-114">Why use Azure Multi-Factor Authentication?</span></span>
<span data-ttu-id="7778d-115">I större utsträckning än någonsin, ansluts allt personer.</span><span class="sxs-lookup"><span data-stu-id="7778d-115">Today, more than ever, people are increasingly connected.</span></span> <span data-ttu-id="7778d-116">Med smarta telefoner, surfplattor, bärbara datorer och datorer kan ha personer flera olika alternativ för hur de ska tooconnect och upprätthålla anslutningen när som helst.</span><span class="sxs-lookup"><span data-stu-id="7778d-116">With smart phones, tablets, laptops, and PCs, people have several different options on how they are going tooconnect and stay connected at any time.</span></span> <span data-ttu-id="7778d-117">Användare kan komma åt sina konton och program från valfri plats, vilket innebär att de kan få mer arbete gjort och hantera sina kunder bättre.</span><span class="sxs-lookup"><span data-stu-id="7778d-117">People can access their accounts and applications from anywhere, which means that they can get more work done and serve their customers better.</span></span>

<span data-ttu-id="7778d-118">Azure Multi-Factor Authentication är ett enkelt toouse skalbara och tillförlitliga som ger en annan metod för autentisering så att användarna alltid är skyddad.</span><span class="sxs-lookup"><span data-stu-id="7778d-118">Azure Multi-Factor Authentication is an easy toouse, scalable, and reliable solution that provides a second method of authentication so your users are always protected.</span></span>

| ![Enkelt tooUse](./media/multi-factor-authentication/simple.png) | ![Skalbar](./media/multi-factor-authentication/scalable.png) | ![Alltid skyddad](./media/multi-factor-authentication/protected.png) | ![Tillförlitlig](./media/multi-factor-authentication/reliable.png) |
|:---:|:---:|:---:|:---:|
| <span data-ttu-id="7778d-123">**Enkelt toouse**</span><span class="sxs-lookup"><span data-stu-id="7778d-123">**Easy toouse**</span></span> |<span data-ttu-id="7778d-124">**Skalbar**</span><span class="sxs-lookup"><span data-stu-id="7778d-124">**Scalable**</span></span> |<span data-ttu-id="7778d-125">**Alltid skyddad**</span><span class="sxs-lookup"><span data-stu-id="7778d-125">**Always Protected**</span></span> |<span data-ttu-id="7778d-126">**Tillförlitlig**</span><span class="sxs-lookup"><span data-stu-id="7778d-126">**Reliable**</span></span> |

* <span data-ttu-id="7778d-127">**Enkelt tooUse** -Azure Multi-Factor Authentication är enkla tooset upp och användning.</span><span class="sxs-lookup"><span data-stu-id="7778d-127">**Easy tooUse** - Azure Multi-Factor Authentication is simple tooset up and use.</span></span> <span data-ttu-id="7778d-128">hello tillåter extra skydd som medföljer Azure Multi-Factor Authentication att användare toomanage sina egna enheter.</span><span class="sxs-lookup"><span data-stu-id="7778d-128">hello extra protection that comes with Azure Multi-Factor Authentication allows users toomanage their own devices.</span></span> <span data-ttu-id="7778d-129">Bästa av alla i många fall det kan ställas in med ett par enkla klick.</span><span class="sxs-lookup"><span data-stu-id="7778d-129">Best of all, in many instances it can be set up with just a few simple clicks.</span></span>
* <span data-ttu-id="7778d-130">**Skalbar** -Azure Multi-Factor Authentication använder hello kraften i Molnets för hello och kan integreras med din lokala AD och anpassade appar.</span><span class="sxs-lookup"><span data-stu-id="7778d-130">**Scalable** - Azure Multi-Factor Authentication uses hello power of hello cloud and integrates with your on-premises AD and custom apps.</span></span> <span data-ttu-id="7778d-131">Det här skyddet utökas även tooyour stora volymer, verksamhetskritiska scenarier.</span><span class="sxs-lookup"><span data-stu-id="7778d-131">This protection is even extended tooyour high-volume, mission-critical scenarios.</span></span>
* <span data-ttu-id="7778d-132">**Alltid skyddad** -Azure Multi-Factor Authentication ger stark autentisering med hjälp av hello högsta branschstandarder.</span><span class="sxs-lookup"><span data-stu-id="7778d-132">**Always Protected** - Azure Multi-Factor Authentication provides strong authentication using hello highest industry standards.</span></span>
* <span data-ttu-id="7778d-133">**Tillförlitliga** -vi garanterar 99,9% tillgänglighet för Azure Multi-Factor Authentication.</span><span class="sxs-lookup"><span data-stu-id="7778d-133">**Reliable** - We guarantee 99.9% availability of Azure Multi-Factor Authentication.</span></span> <span data-ttu-id="7778d-134">hello tjänsten räknas som otillgänglig när det inte går tooreceive eller process verifiering begäranden för hello tvåstegsverifiering.</span><span class="sxs-lookup"><span data-stu-id="7778d-134">hello service is considered unavailable when it is unable tooreceive or process verification requests for hello two-step verification.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Windows-Azure-Multi-Factor-Authentication/player]


## <a name="next-steps"></a><span data-ttu-id="7778d-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7778d-135">Next steps</span></span>

- <span data-ttu-id="7778d-136">Lär dig mer om [hur Azure Multi-Factor Authentication fungerar](multi-factor-authentication-how-it-works.md)</span><span class="sxs-lookup"><span data-stu-id="7778d-136">Learn about [how Azure Multi-Factor Authentication works](multi-factor-authentication-how-it-works.md)</span></span>

- <span data-ttu-id="7778d-137">Läs mer om olika hello [versioner och av förbrukningsmetoder för Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md)</span><span class="sxs-lookup"><span data-stu-id="7778d-137">Read about hello different [versions and consumption methods for Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md)</span></span>

---
title: "Lär dig mer om tvåstegsverifiering i Azure MFA | Microsoft Docs"
description: "Vad är Azure Multi-Factor Authentication, varför ska jag använda MFA, mer information om Multifaktorautentisering klienten och olika metoder och versioner som är tillgängliga. "
keywords: "Introduktion till MFA, mfa översikt vad är mfa"
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
ms.openlocfilehash: 7334ab5b278c3339fdbc2e363fd5c609604d3e14
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="what-is-azure-multi-factor-authentication"></a><span data-ttu-id="68cb0-104">Vad är Azure Multi-Factor Authentication?</span><span class="sxs-lookup"><span data-stu-id="68cb0-104">What is Azure Multi-Factor Authentication?</span></span>
<span data-ttu-id="68cb0-105">Tvåstegsverifiering är en autentiseringsmetod som kräver mer än en verifieringsmetod och lägger till ett kritiskt andra säkerhetslager till användarinloggningar och transaktioner.</span><span class="sxs-lookup"><span data-stu-id="68cb0-105">Two-step verification is a method of authentication that requires more than one verification method and adds a critical second layer of security to user sign-ins and transactions.</span></span> <span data-ttu-id="68cb0-106">Det fungerar genom att två eller flera av följande verifieringsmetoder för:</span><span class="sxs-lookup"><span data-stu-id="68cb0-106">It works by requiring any two or more of the following verification methods:</span></span>

* <span data-ttu-id="68cb0-107">Något du vet (normalt ett lösenord)</span><span class="sxs-lookup"><span data-stu-id="68cb0-107">Something you know (typically a password)</span></span>
* <span data-ttu-id="68cb0-108">Något du har (betrodda enheter som inte enkelt dubbleras, t.ex. en telefon)</span><span class="sxs-lookup"><span data-stu-id="68cb0-108">Something you have (a trusted device that is not easily duplicated, like a phone)</span></span>
* <span data-ttu-id="68cb0-109">Något du är (biometrik)</span><span class="sxs-lookup"><span data-stu-id="68cb0-109">Something you are (biometrics)</span></span>

<span data-ttu-id="68cb0-110"><center>![Användarnamn och lösenord](./media/multi-factor-authentication/pword.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![certifikat](./media/multi-factor-authentication/phone.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![Smart Phone](./media/multi-factor-authentication/hware.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![smartkort](./media/multi-factor-authentication/smart.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![Virtuellt smartkort](./media/multi-factor-authentication/vsmart.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![användarnamn och Lösenord](./media/multi-factor-authentication/cert.png)</center></span><span class="sxs-lookup"><span data-stu-id="68cb0-110"><center>![Username and Password](./media/multi-factor-authentication/pword.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Certificates](./media/multi-factor-authentication/phone.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Smart Phone](./media/multi-factor-authentication/hware.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Smart Card](./media/multi-factor-authentication/smart.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Virtual Smart Card](./media/multi-factor-authentication/vsmart.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Username and Password](./media/multi-factor-authentication/cert.png)</center></span></span>

<span data-ttu-id="68cb0-111">Azure Multi-Factor Authentication (MFA) är Microsofts verifieringslösning i två steg.</span><span class="sxs-lookup"><span data-stu-id="68cb0-111">Azure Multi-Factor Authentication (MFA) is Microsoft's two-step verification solution.</span></span> <span data-ttu-id="68cb0-112">Azures MFA bidrar till att skydda åtkomsten till data och program och tillgodoser samtidigt användarens önskemål om en enkel inloggningsprocess.</span><span class="sxs-lookup"><span data-stu-id="68cb0-112">Azure MFA helps safeguard access to data and applications while meeting user demand for a simple sign-in process.</span></span> <span data-ttu-id="68cb0-113">Den ger stark autentisering via en mängd verifieringsmetoder, inklusive telefonsamtal, textmeddelande eller verifiering av mobilappar.</span><span class="sxs-lookup"><span data-stu-id="68cb0-113">It delivers strong authentication via a range of verification methods, including phone call, text message, or mobile app verification.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/WA-MFA-Overview/player]
>
>

## <a name="why-use-azure-multi-factor-authentication"></a><span data-ttu-id="68cb0-114">Varför använda Azure Multi-Factor Authentication?</span><span class="sxs-lookup"><span data-stu-id="68cb0-114">Why use Azure Multi-Factor Authentication?</span></span>
<span data-ttu-id="68cb0-115">I större utsträckning än någonsin, ansluts allt personer.</span><span class="sxs-lookup"><span data-stu-id="68cb0-115">Today, more than ever, people are increasingly connected.</span></span> <span data-ttu-id="68cb0-116">Med smarta telefoner, surfplattor, bärbara datorer och datorer kan ha personer flera olika alternativ för hur de ska kunna ansluta och upprätthålla anslutningen när som helst.</span><span class="sxs-lookup"><span data-stu-id="68cb0-116">With smart phones, tablets, laptops, and PCs, people have several different options on how they are going to connect and stay connected at any time.</span></span> <span data-ttu-id="68cb0-117">Användare kan komma åt sina konton och program från valfri plats, vilket innebär att de kan få mer arbete gjort och hantera sina kunder bättre.</span><span class="sxs-lookup"><span data-stu-id="68cb0-117">People can access their accounts and applications from anywhere, which means that they can get more work done and serve their customers better.</span></span>

<span data-ttu-id="68cb0-118">Azure Multi-Factor Authentication är en lättanvänd, skalbara och tillförlitliga lösning som innehåller en annan metod för autentisering så att användarna alltid är skyddad.</span><span class="sxs-lookup"><span data-stu-id="68cb0-118">Azure Multi-Factor Authentication is an easy to use, scalable, and reliable solution that provides a second method of authentication so your users are always protected.</span></span>

| ![Lätt att använda](./media/multi-factor-authentication/simple.png) | ![Skalbar](./media/multi-factor-authentication/scalable.png) | ![Alltid skyddad](./media/multi-factor-authentication/protected.png) | ![Tillförlitlig](./media/multi-factor-authentication/reliable.png) |
|:---:|:---:|:---:|:---:|
| <span data-ttu-id="68cb0-123">**Lätt att använda**</span><span class="sxs-lookup"><span data-stu-id="68cb0-123">**Easy to use**</span></span> |<span data-ttu-id="68cb0-124">**Skalbar**</span><span class="sxs-lookup"><span data-stu-id="68cb0-124">**Scalable**</span></span> |<span data-ttu-id="68cb0-125">**Alltid skyddad**</span><span class="sxs-lookup"><span data-stu-id="68cb0-125">**Always Protected**</span></span> |<span data-ttu-id="68cb0-126">**Tillförlitlig**</span><span class="sxs-lookup"><span data-stu-id="68cb0-126">**Reliable**</span></span> |

* <span data-ttu-id="68cb0-127">**Lättanvända** -Azure Multi-Factor Authentication är enkel att konfigurera och använda.</span><span class="sxs-lookup"><span data-stu-id="68cb0-127">**Easy to Use** - Azure Multi-Factor Authentication is simple to set up and use.</span></span> <span data-ttu-id="68cb0-128">Det extra skydd som medföljer Azure Multi-Factor Authentication tillåter användare att hantera sina egna enheter.</span><span class="sxs-lookup"><span data-stu-id="68cb0-128">The extra protection that comes with Azure Multi-Factor Authentication allows users to manage their own devices.</span></span> <span data-ttu-id="68cb0-129">Bästa av alla i många fall det kan ställas in med ett par enkla klick.</span><span class="sxs-lookup"><span data-stu-id="68cb0-129">Best of all, in many instances it can be set up with just a few simple clicks.</span></span>
* <span data-ttu-id="68cb0-130">**Skalbar** -Azure Multi-Factor Authentication använder kraften i molnet och kan integreras med din lokala AD och anpassade appar.</span><span class="sxs-lookup"><span data-stu-id="68cb0-130">**Scalable** - Azure Multi-Factor Authentication uses the power of the cloud and integrates with your on-premises AD and custom apps.</span></span> <span data-ttu-id="68cb0-131">Det här skyddet utökas även stora volymer, verksamhetskritiska scenariot.</span><span class="sxs-lookup"><span data-stu-id="68cb0-131">This protection is even extended to your high-volume, mission-critical scenarios.</span></span>
* <span data-ttu-id="68cb0-132">**Alltid skyddad** -Azure Multi-Factor Authentication ger stark autentisering med högsta branschstandarder.</span><span class="sxs-lookup"><span data-stu-id="68cb0-132">**Always Protected** - Azure Multi-Factor Authentication provides strong authentication using the highest industry standards.</span></span>
* <span data-ttu-id="68cb0-133">**Tillförlitliga** -vi garanterar 99,9% tillgänglighet för Azure Multi-Factor Authentication.</span><span class="sxs-lookup"><span data-stu-id="68cb0-133">**Reliable** - We guarantee 99.9% availability of Azure Multi-Factor Authentication.</span></span> <span data-ttu-id="68cb0-134">Tjänsten anses inte tillgängligt när det inte går att ta emot eller bearbetar begäranden för verifiering för tvåstegsverifiering.</span><span class="sxs-lookup"><span data-stu-id="68cb0-134">The service is considered unavailable when it is unable to receive or process verification requests for the two-step verification.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Windows-Azure-Multi-Factor-Authentication/player]


## <a name="next-steps"></a><span data-ttu-id="68cb0-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="68cb0-135">Next steps</span></span>

- <span data-ttu-id="68cb0-136">Lär dig mer om [hur Azure Multi-Factor Authentication fungerar](multi-factor-authentication-how-it-works.md)</span><span class="sxs-lookup"><span data-stu-id="68cb0-136">Learn about [how Azure Multi-Factor Authentication works](multi-factor-authentication-how-it-works.md)</span></span>

- <span data-ttu-id="68cb0-137">Läs mer om de olika [versioner och av förbrukningsmetoder för Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md)</span><span class="sxs-lookup"><span data-stu-id="68cb0-137">Read about the different [versions and consumption methods for Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md)</span></span>

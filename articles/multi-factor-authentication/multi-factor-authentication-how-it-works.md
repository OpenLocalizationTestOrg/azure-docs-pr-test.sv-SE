---
title: aaaAzure Multi-Factor Authentication - hur det fungerar
description: "Azure Multi-Factor Authentication hjälper till att skydda åtkomst toodata och program och uppfyller efterfrågan från användarna för en process för enkel inloggning. Det ger ökad säkerhet genom att kräva andra formen av autentisering och ger stark autentisering via en mängd alternativ för enkel verifiering."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d14db902-9afe-4fca-b3a5-4bd54b3d8ec5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: 82f234fb86f145c42e8e56b8bdd2d61720c9ff2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-azure-multi-factor-authentication-works"></a><span data-ttu-id="11432-104">Hur Azure Multi-Factor Authentication fungerar</span><span class="sxs-lookup"><span data-stu-id="11432-104">How Azure Multi-Factor Authentication works</span></span>
<span data-ttu-id="11432-105">hello säkerheten för tvåstegsverifiering ligger i dess överlappande tillvägagångssättet.</span><span class="sxs-lookup"><span data-stu-id="11432-105">hello security of two-step verification lies in its layered approach.</span></span> <span data-ttu-id="11432-106">Att kompromettera flera autentiseringsfaktorer presenterar en betydande utmaning för angripare.</span><span class="sxs-lookup"><span data-stu-id="11432-106">Compromising multiple authentication factors presents a significant challenge for attackers.</span></span> <span data-ttu-id="11432-107">Även om en angripare lyckas toolearn hello användarens lösenord, är det oanvändbar utan också tillgång hello betrodd enhet.</span><span class="sxs-lookup"><span data-stu-id="11432-107">Even if an attacker manages toolearn hello user's password, it is useless without also having possession of hello trusted device.</span></span> 

![Proofup](./media/multi-factor-authentication-how-it-works/howitworks.png)

<span data-ttu-id="11432-109">Azure Multi-Factor Authentication hjälper till att skydda åtkomst toodata och program och uppfyller efterfrågan från användarna för en process för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="11432-109">Azure Multi-Factor Authentication helps safeguard access toodata and applications while meeting user demand for a simple sign-in process.</span></span>  <span data-ttu-id="11432-110">Det ger ökad säkerhet genom att kräva andra formen av autentisering och ger stark autentisering via en mängd alternativ för enkel verifiering.</span><span class="sxs-lookup"><span data-stu-id="11432-110">It provides additional security by requiring a second form of authentication and delivers strong authentication via a range of easy verification options.</span></span>


## <a name="methods-available-for-two-step-verification"></a><span data-ttu-id="11432-111">Metoderna för tvåstegsverifiering</span><span class="sxs-lookup"><span data-stu-id="11432-111">Methods available for two-step verification</span></span>
<span data-ttu-id="11432-112">När en användare loggar in, skickas en ytterligare verifiering toohello användare.</span><span class="sxs-lookup"><span data-stu-id="11432-112">When a user signs in, an additional verification is sent toohello user.</span></span>  <span data-ttu-id="11432-113">hello nedan följer en lista över metoder som kan användas för andra verifieringen.</span><span class="sxs-lookup"><span data-stu-id="11432-113">hello following are a list of methods that can be used for this second verification.</span></span>

| <span data-ttu-id="11432-114">Verifieringsmetod</span><span class="sxs-lookup"><span data-stu-id="11432-114">Verification Method</span></span> | <span data-ttu-id="11432-115">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="11432-115">Description</span></span> |
| --- | --- |
| <span data-ttu-id="11432-116">Telefonsamtal</span><span class="sxs-lookup"><span data-stu-id="11432-116">Phone call</span></span> |<span data-ttu-id="11432-117">Ett anrop placeras tooa användarens registrerade telefon.</span><span class="sxs-lookup"><span data-stu-id="11432-117">A call is placed tooa user’s registered phone.</span></span> <span data-ttu-id="11432-118">hello användaren anger en PIN-kod om det behövs och sedan trycker hello #-knappen.</span><span class="sxs-lookup"><span data-stu-id="11432-118">hello user enters a PIN if necessary then presses hello # key.</span></span> |
| <span data-ttu-id="11432-119">Textmeddelande</span><span class="sxs-lookup"><span data-stu-id="11432-119">Text message</span></span> |<span data-ttu-id="11432-120">Ett textmeddelande skickas tooa användarens mobiltelefon med en sexsiffrig kod.</span><span class="sxs-lookup"><span data-stu-id="11432-120">A text message is sent tooa user’s mobile phone with a six-digit code.</span></span> <span data-ttu-id="11432-121">hello användaren anger den här koden på hello-inloggningssida.</span><span class="sxs-lookup"><span data-stu-id="11432-121">hello user enters this code on hello sign-in page.</span></span> |
| <span data-ttu-id="11432-122">Avisering från mobilappen</span><span class="sxs-lookup"><span data-stu-id="11432-122">Mobile app notification</span></span> |<span data-ttu-id="11432-123">En begäran om identitetsverifiering skickas tooa användarens Smartphone.</span><span class="sxs-lookup"><span data-stu-id="11432-123">A verification request is sent tooa user’s smart phone.</span></span> <span data-ttu-id="11432-124">hello användaren anger en PIN-kod vid behov och sedan väljer **Kontrollera** på hello mobila appar.</span><span class="sxs-lookup"><span data-stu-id="11432-124">hello user enters a PIN if necessary then selects **Verify** on hello mobile app.</span></span> |
| <span data-ttu-id="11432-125">Verifieringskod för mobila appar</span><span class="sxs-lookup"><span data-stu-id="11432-125">Mobile app verification code</span></span> |<span data-ttu-id="11432-126">Hej mobilappen som körs på en användares Smartphone, visar en Verifieringskod som ändrar var 30: e sekund.</span><span class="sxs-lookup"><span data-stu-id="11432-126">hello mobile app, which is running on a user’s smart phone, displays a verification code that changes every 30 seconds.</span></span> <span data-ttu-id="11432-127">hello användare söker efter hello senaste kod och registrerar den på hello-inloggningssida.</span><span class="sxs-lookup"><span data-stu-id="11432-127">hello user finds hello most recent code and enters it on hello sign-in page.</span></span> |
| <span data-ttu-id="11432-128">OATH-token från tredje part</span><span class="sxs-lookup"><span data-stu-id="11432-128">Third-party OATH tokens</span></span> | <span data-ttu-id="11432-129">Azure Multi-Factor Authentication-servern kan vara konfigurerade tooaccept tredjepartsverifiering metoder.</span><span class="sxs-lookup"><span data-stu-id="11432-129">Azure Multi-Factor Authentication Server can be configured tooaccept third-party verification methods.</span></span> |

<span data-ttu-id="11432-130">Azure Multi-Factor Authentication ger valbar verifieringsmetoderna för moln- och server.</span><span class="sxs-lookup"><span data-stu-id="11432-130">Azure Multi-Factor Authentication provides selectable verification methods for both cloud and server.</span></span> <span data-ttu-id="11432-131">Du kan välja vilka metoder som är tillgängliga för användarna: telefonsamtal, text, avisering via app eller app-koder.</span><span class="sxs-lookup"><span data-stu-id="11432-131">You can choose which methods are available for your users: phone call, text, app notification, or app codes.</span></span> <span data-ttu-id="11432-132">Mer information finns i [valbar verifieringsmetoderna](multi-factor-authentication-whats-next.md#selectable-verification-methods).</span><span class="sxs-lookup"><span data-stu-id="11432-132">For more information, see [selectable verification methods](multi-factor-authentication-whats-next.md#selectable-verification-methods).</span></span>

## <a name="next-steps"></a><span data-ttu-id="11432-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="11432-133">Next steps</span></span>

- <span data-ttu-id="11432-134">Läs mer om olika hello [versioner och av förbrukningsmetoder för Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md)</span><span class="sxs-lookup"><span data-stu-id="11432-134">Read about hello different [versions and consumption methods for Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md)</span></span>

- <span data-ttu-id="11432-135">Välj om toodeploy Azure MFA [i hello molnet eller lokalt](multi-factor-authentication-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="11432-135">Choose whether toodeploy Azure MFA [in hello cloud or on-premises](multi-factor-authentication-get-started.md)</span></span>

- <span data-ttu-id="11432-136">Läs svar på [vanliga frågor och svar](multi-factor-authentication-faq.md)</span><span class="sxs-lookup"><span data-stu-id="11432-136">Read answers for [Frequently asked questions](multi-factor-authentication-faq.md)</span></span>
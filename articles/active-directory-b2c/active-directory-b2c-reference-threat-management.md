---
title: 'Azure Active Directory B2C: Hot management | Microsoft Docs'
description: "Läs mer om identifiering och minskning tekniker för denial of service-attacker och lösenord attacker i Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: vigunase
manager: Ajith Alexander
editor: 
ms.assetid: 6df79878-65cb-4dfc-98bb-2b328055bc2e
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2016
ms.author: 
ms.openlocfilehash: 60bc0cc392b332cc4e9741ddb97dfa58e68ed420
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-threat-management"></a><span data-ttu-id="abb5f-103">Azure Active Directory B2C: Threat management</span><span class="sxs-lookup"><span data-stu-id="abb5f-103">Azure Active Directory B2C: Threat management</span></span>

<span data-ttu-id="abb5f-104">Threat management innehåller planering för skydd mot attacker mot ditt system och nätverk.</span><span class="sxs-lookup"><span data-stu-id="abb5f-104">Threat management includes planning for protection from attacks against your system and networks.</span></span> <span data-ttu-id="abb5f-105">Denial of service-attacker kan göra resurser tillgängliga toointended användare.</span><span class="sxs-lookup"><span data-stu-id="abb5f-105">Denial-of-service attacks might make resources unavailable toointended users.</span></span> <span data-ttu-id="abb5f-106">Lösenord attacker lead toounauthorized åtkomst tooresources.</span><span class="sxs-lookup"><span data-stu-id="abb5f-106">Password attacks lead toounauthorized access tooresources.</span></span> <span data-ttu-id="abb5f-107">Azure Active Directory B2C (Azure AD B2C) har inbyggda funktioner som kan hjälpa dig att skydda dina data mot dessa hot på flera olika sätt.</span><span class="sxs-lookup"><span data-stu-id="abb5f-107">Azure Active Directory B2C (Azure AD B2C) has built-in features that can help you protect your data against these threats in multiple ways.</span></span>

## <a name="denial-of-service-attacks"></a><span data-ttu-id="abb5f-108">Denial of service-attacker</span><span class="sxs-lookup"><span data-stu-id="abb5f-108">Denial-of-service attacks</span></span>

<span data-ttu-id="abb5f-109">Azure AD B2C använder tekniker för identifiering och lösning som SYN cookies och hastighet och anslutning gränser tooprotect underliggande resurser mot denial of service-attacker.</span><span class="sxs-lookup"><span data-stu-id="abb5f-109">Azure AD B2C uses detection and mitigation techniques like SYN cookies, and rate and connection limits tooprotect underlying resources against denial-of-service attacks.</span></span>

## <a name="password-attacks"></a><span data-ttu-id="abb5f-110">Lösenord attacker</span><span class="sxs-lookup"><span data-stu-id="abb5f-110">Password attacks</span></span>

<span data-ttu-id="abb5f-111">Azure AD B2C har också minskning tekniker för lösenord attacker.</span><span class="sxs-lookup"><span data-stu-id="abb5f-111">Azure AD B2C also has mitigation techniques in place for password attacks.</span></span> <span data-ttu-id="abb5f-112">Lösning innehåller lösenord brute force-attacker och lösenord för ordlisteattacker.</span><span class="sxs-lookup"><span data-stu-id="abb5f-112">Mitigation includes brute-force password attacks and dictionary password attacks.</span></span> <span data-ttu-id="abb5f-113">Lösenord som anges av användarna är obligatoriska toobe rimligen komplexa.</span><span class="sxs-lookup"><span data-stu-id="abb5f-113">Passwords that are set by users are required toobe reasonably complex.</span></span> <span data-ttu-id="abb5f-114">Med hjälp av olika signaler analyserar Azure AD B2C hello integriteten för begäranden.</span><span class="sxs-lookup"><span data-stu-id="abb5f-114">By using various signals, Azure AD B2C analyzes hello integrity of requests.</span></span> <span data-ttu-id="abb5f-115">Azure AD B2C är utformad toointelligently skilja avsedda användarna från hackare och botnät.</span><span class="sxs-lookup"><span data-stu-id="abb5f-115">Azure AD B2C is designed toointelligently differentiate intended users from hackers and botnets.</span></span> <span data-ttu-id="abb5f-116">Azure AD B2C ger en strategi för avancerade toolock konton baserat på hello-lösenord som angetts i hello sannolikheten för en attack.</span><span class="sxs-lookup"><span data-stu-id="abb5f-116">Azure AD B2C provides a sophisticated strategy toolock accounts based on hello passwords entered, in hello likelihood of an attack.</span></span>

<span data-ttu-id="abb5f-117">Mer information finns i hello [Microsoft Trust Center](https://www.microsoft.com/trustcenter/security/threatmanagement).</span><span class="sxs-lookup"><span data-stu-id="abb5f-117">For more information, visit hello [Microsoft Trust Center](https://www.microsoft.com/trustcenter/security/threatmanagement).</span></span>

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
ms.openlocfilehash: 9472cb01eb713e297053727b1a314293574bb657
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-b2c-threat-management"></a><span data-ttu-id="abfdf-103">Azure Active Directory B2C: Threat management</span><span class="sxs-lookup"><span data-stu-id="abfdf-103">Azure Active Directory B2C: Threat management</span></span>

<span data-ttu-id="abfdf-104">Threat management innehåller planering för skydd mot attacker mot ditt system och nätverk.</span><span class="sxs-lookup"><span data-stu-id="abfdf-104">Threat management includes planning for protection from attacks against your system and networks.</span></span> <span data-ttu-id="abfdf-105">Denial of service-attacker kan göra resurser tillgängliga för avsedda användarna.</span><span class="sxs-lookup"><span data-stu-id="abfdf-105">Denial-of-service attacks might make resources unavailable to intended users.</span></span> <span data-ttu-id="abfdf-106">Lösenord attacker leda till obehörig åtkomst till resurser.</span><span class="sxs-lookup"><span data-stu-id="abfdf-106">Password attacks lead to unauthorized access to resources.</span></span> <span data-ttu-id="abfdf-107">Azure Active Directory B2C (Azure AD B2C) har inbyggda funktioner som kan hjälpa dig att skydda dina data mot dessa hot på flera olika sätt.</span><span class="sxs-lookup"><span data-stu-id="abfdf-107">Azure Active Directory B2C (Azure AD B2C) has built-in features that can help you protect your data against these threats in multiple ways.</span></span>

## <a name="denial-of-service-attacks"></a><span data-ttu-id="abfdf-108">Denial of service-attacker</span><span class="sxs-lookup"><span data-stu-id="abfdf-108">Denial-of-service attacks</span></span>

<span data-ttu-id="abfdf-109">Azure AD B2C använder identifiering och minskning tekniker som SYN cookies och hastighet och anslutningen gränserna för att skydda underliggande resurserna mot DOS-attacker.</span><span class="sxs-lookup"><span data-stu-id="abfdf-109">Azure AD B2C uses detection and mitigation techniques like SYN cookies, and rate and connection limits to protect underlying resources against denial-of-service attacks.</span></span>

## <a name="password-attacks"></a><span data-ttu-id="abfdf-110">Lösenord attacker</span><span class="sxs-lookup"><span data-stu-id="abfdf-110">Password attacks</span></span>

<span data-ttu-id="abfdf-111">Azure AD B2C har också minskning tekniker för lösenord attacker.</span><span class="sxs-lookup"><span data-stu-id="abfdf-111">Azure AD B2C also has mitigation techniques in place for password attacks.</span></span> <span data-ttu-id="abfdf-112">Lösning innehåller lösenord brute force-attacker och lösenord för ordlisteattacker.</span><span class="sxs-lookup"><span data-stu-id="abfdf-112">Mitigation includes brute-force password attacks and dictionary password attacks.</span></span> <span data-ttu-id="abfdf-113">Lösenord som anges av användare krävs för att vara rimligen komplex.</span><span class="sxs-lookup"><span data-stu-id="abfdf-113">Passwords that are set by users are required to be reasonably complex.</span></span> <span data-ttu-id="abfdf-114">Med hjälp av olika signaler analyserar Azure AD B2C integriteten för begäranden.</span><span class="sxs-lookup"><span data-stu-id="abfdf-114">By using various signals, Azure AD B2C analyzes the integrity of requests.</span></span> <span data-ttu-id="abfdf-115">Azure AD B2C är utformat för att särskilja Intelligent avsedda användarna mot hackare och botnät.</span><span class="sxs-lookup"><span data-stu-id="abfdf-115">Azure AD B2C is designed to intelligently differentiate intended users from hackers and botnets.</span></span> <span data-ttu-id="abfdf-116">Azure AD B2C innehåller en avancerad strategi för att låsa ett konto baserat på de lösenord som angetts i sannolikheten för en attack.</span><span class="sxs-lookup"><span data-stu-id="abfdf-116">Azure AD B2C provides a sophisticated strategy to lock accounts based on the passwords entered, in the likelihood of an attack.</span></span>

<span data-ttu-id="abfdf-117">Mer information finns i [Microsoft Trust Center](https://www.microsoft.com/trustcenter/security/threatmanagement).</span><span class="sxs-lookup"><span data-stu-id="abfdf-117">For more information, visit the [Microsoft Trust Center](https://www.microsoft.com/trustcenter/security/threatmanagement).</span></span>

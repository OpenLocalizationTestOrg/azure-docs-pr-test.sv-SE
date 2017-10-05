---
title: "Azure Active Directory PoC Playbook beståndsdelar | Microsoft Docs"
description: "Utforska och snabbt implementera scenarier för identitets- och åtkomsthantering"
services: active-directory
keywords: Azure active directory, playbook, konceptbevis, PoC
documentationcenter: 
author: dstefanMSFT
manager: asuthar
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/12/2017
ms.author: dstefan
ms.openlocfilehash: d2a0fe280f143d390f5e4ba40e0ebe92d8a4a837
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-proof-of-concept-playbook-ingredients"></a><span data-ttu-id="13947-104">Azure Active Directory bevis på koncept Playbook komponenter</span><span class="sxs-lookup"><span data-stu-id="13947-104">Azure Active Directory Proof of Concept Playbook Ingredients</span></span> 

## <a name="theme"></a><span data-ttu-id="13947-105">Tema</span><span class="sxs-lookup"><span data-stu-id="13947-105">Theme</span></span>
<span data-ttu-id="13947-106">Azure AD innehåller identitets- och lösningar i flera områden i företaget.</span><span class="sxs-lookup"><span data-stu-id="13947-106">Azure AD provides identity and access solutions across multiple areas in the enterprise.</span></span> <span data-ttu-id="13947-107">Vi klassificera scenarier inom följande områden:</span><span class="sxs-lookup"><span data-stu-id="13947-107">We classify the scenarios in the following areas:</span></span> 

* [<span data-ttu-id="13947-108">Många appar, en identitet</span><span class="sxs-lookup"><span data-stu-id="13947-108">Lots of apps, one identity</span></span>](active-directory-playbook-implementation.md#theme---lots-of-apps-one-identity) 
* [<span data-ttu-id="13947-109">Öka säkerheten</span><span class="sxs-lookup"><span data-stu-id="13947-109">Increase your security</span></span>](active-directory-playbook-implementation.md#theme---increase-your-security) 
* [<span data-ttu-id="13947-110">Skala med självbetjäning</span><span class="sxs-lookup"><span data-stu-id="13947-110">Scale with Self Service</span></span>](active-directory-playbook-implementation.md#theme---scale-with-self-service) 

<span data-ttu-id="13947-111">Definiera ett tema för att ram PoC hjälper dig att fokusera på arbetet som får respons med verksamhetsmål som ofta är av intresse för ett konceptbevis utlösare i första hand.</span><span class="sxs-lookup"><span data-stu-id="13947-111">Defining a theme to frame the PoC helps to focus the efforts that resonates with business goals, which oftentimes are the triggers of the interest in a proof of concept in the first place.</span></span> 

## <a name="environment"></a><span data-ttu-id="13947-112">Miljö</span><span class="sxs-lookup"><span data-stu-id="13947-112">Environment</span></span>

<span data-ttu-id="13947-113">Det är viktigt att fastställa information om miljön där du levererar konceptbeviset.</span><span class="sxs-lookup"><span data-stu-id="13947-113">It is important to determine the details of the environment where you will deliver the PoC.</span></span> <span data-ttu-id="13947-114">Det bästa du kan bygga vidare på den när konceptbeviset har slutförts.</span><span class="sxs-lookup"><span data-stu-id="13947-114">Ideally you can build upon it after the PoC is completed.</span></span> <span data-ttu-id="13947-115">Målmiljön är avgörande bör du hitta rätt balans mellan att göra det som verkliga som möjligt och arbetet med begränsningar eller ytterligare överväganden.</span><span class="sxs-lookup"><span data-stu-id="13947-115">The target environment is crucial and you should find the right balance between making it as real as possible and the overhead of constraints or extra considerations.</span></span> <span data-ttu-id="13947-116">Miljöer för POC är:</span><span class="sxs-lookup"><span data-stu-id="13947-116">The typical environments for PoCs are:</span></span>
* <span data-ttu-id="13947-117">**Produktion:** scenarier kommer att genomföras i live-miljön och redan har distribuerats Microsoft Cloud-tjänsterna (produktion AD, Office 365, Azure AD-klient/SSO lösning).</span><span class="sxs-lookup"><span data-stu-id="13947-117">**Production:** The scenarios will be implemented in your live environment and already deployed Microsoft Cloud services (production AD, Office 365, Azure AD tenant/SSO solution).</span></span> 
* <span data-ttu-id="13947-118">**Användaren godkänner testa (UAT) / utvecklingsmiljö:** du har test-infrastruktur (parallell AD och eventuellt Azure AD-klient/SSO-lösning) med testdata som liknar produktion.</span><span class="sxs-lookup"><span data-stu-id="13947-118">**User Acceptance Test (UAT)/Dev environment:** You have test infrastructure (parallel AD and potentially Azure AD tenant/SSO solution) with test data that resembles production.</span></span> <span data-ttu-id="13947-119">Normalt delas testmiljön mellan flera projekt i företaget.</span><span class="sxs-lookup"><span data-stu-id="13947-119">Typically, the test environment is shared across multiple projects in the enterprise.</span></span>

<span data-ttu-id="13947-120">De flesta scenarier i den här handboken är additiva till sin natur.</span><span class="sxs-lookup"><span data-stu-id="13947-120">Most scenarios in this guide are additive in nature.</span></span> <span data-ttu-id="13947-121">De kan därför distribueras i produktion klienten utan att påverka användare utanför konceptbeviset.</span><span class="sxs-lookup"><span data-stu-id="13947-121">As a result, they can be deployed in the production tenant without affecting users outside the PoC.</span></span> <span data-ttu-id="13947-122">I det här dokumentet ringer vi ut vilka scenarier som skulle ha klient hela effekt.</span><span class="sxs-lookup"><span data-stu-id="13947-122">Throughout this document, we will be calling out which scenarios would have tenant-wide effect.</span></span> <span data-ttu-id="13947-123">I sådana fall kanske du vill överväga en icke-produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="13947-123">In those cases, you might want to consider a non-production environment.</span></span> 


## <a name="target-users"></a><span data-ttu-id="13947-124">Målgruppsanvändare</span><span class="sxs-lookup"><span data-stu-id="13947-124">Target Users</span></span>

<span data-ttu-id="13947-125">Det är viktigt att fastställa vilken uppsättning användare som ska ha scenarier, särskilt när miljön är produktions- och mål.</span><span class="sxs-lookup"><span data-stu-id="13947-125">It is important to determine the target set of users that will exercise the scenarios, especially when the environment is production or test.</span></span> <span data-ttu-id="13947-126">Mål-användare för PoC är:</span><span class="sxs-lookup"><span data-stu-id="13947-126">The categories of target users for PoC are:</span></span>
* <span data-ttu-id="13947-127">**Pilotanvändarna:** verkliga användare i miljön som kommer att använda lösningen med det konto de använder för sina dagliga jobbfunktioner</span><span class="sxs-lookup"><span data-stu-id="13947-127">**Pilot Users:** Real users in the environment that will be using the solution with the account they use for their day to day job functions</span></span>
* <span data-ttu-id="13947-128">**Testanvändare:** testa konton som har skapats i miljön</span><span class="sxs-lookup"><span data-stu-id="13947-128">**Test Users:** Test accounts created in the environment</span></span> 

<span data-ttu-id="13947-129">De flesta scenarier i den här guiden kan utnyttjas av pilotanvändare.</span><span class="sxs-lookup"><span data-stu-id="13947-129">Most scenarios in this guide can be exercised by pilot users.</span></span> <span data-ttu-id="13947-130">I det här dokumentet kommer vi ringer ut mål användaren överväganden om det behövs.</span><span class="sxs-lookup"><span data-stu-id="13947-130">Throughout this document, we will be calling out target user considerations if needed.</span></span>


[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]
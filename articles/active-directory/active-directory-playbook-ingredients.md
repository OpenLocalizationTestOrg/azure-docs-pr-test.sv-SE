---
title: "aaaAzure Active Directory PoC Playbook beståndsdelar | Microsoft Docs"
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
ms.openlocfilehash: 0a7f5cd659b9d62ac86e3c27e5727294d481f4a2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-proof-of-concept-playbook-ingredients"></a><span data-ttu-id="b8632-104">Azure Active Directory bevis på koncept Playbook komponenter</span><span class="sxs-lookup"><span data-stu-id="b8632-104">Azure Active Directory Proof of Concept Playbook Ingredients</span></span> 

## <a name="theme"></a><span data-ttu-id="b8632-105">Tema</span><span class="sxs-lookup"><span data-stu-id="b8632-105">Theme</span></span>
<span data-ttu-id="b8632-106">Azure AD innehåller identitets- och lösningar i flera områden i hello enterprise.</span><span class="sxs-lookup"><span data-stu-id="b8632-106">Azure AD provides identity and access solutions across multiple areas in hello enterprise.</span></span> <span data-ttu-id="b8632-107">Vi klassificera hello scenarier i hello följande områden:</span><span class="sxs-lookup"><span data-stu-id="b8632-107">We classify hello scenarios in hello following areas:</span></span> 

* [<span data-ttu-id="b8632-108">Många appar, en identitet</span><span class="sxs-lookup"><span data-stu-id="b8632-108">Lots of apps, one identity</span></span>](active-directory-playbook-implementation.md#theme---lots-of-apps-one-identity) 
* [<span data-ttu-id="b8632-109">Öka säkerheten</span><span class="sxs-lookup"><span data-stu-id="b8632-109">Increase your security</span></span>](active-directory-playbook-implementation.md#theme---increase-your-security) 
* [<span data-ttu-id="b8632-110">Skala med självbetjäning</span><span class="sxs-lookup"><span data-stu-id="b8632-110">Scale with Self Service</span></span>](active-directory-playbook-implementation.md#theme---scale-with-self-service) 

<span data-ttu-id="b8632-111">Definiera ett tema tooframe hello PoC hjälper toofocus hello arbete som får respons med verksamhetsmål som ofta är hello utlösare hello intresse för ett konceptbevis hello första plats.</span><span class="sxs-lookup"><span data-stu-id="b8632-111">Defining a theme tooframe hello PoC helps toofocus hello efforts that resonates with business goals, which oftentimes are hello triggers of hello interest in a proof of concept in hello first place.</span></span> 

## <a name="environment"></a><span data-ttu-id="b8632-112">Miljö</span><span class="sxs-lookup"><span data-stu-id="b8632-112">Environment</span></span>

<span data-ttu-id="b8632-113">Det är viktigt toodetermine hello information om hello miljö där du levererar hello PoC.</span><span class="sxs-lookup"><span data-stu-id="b8632-113">It is important toodetermine hello details of hello environment where you will deliver hello PoC.</span></span> <span data-ttu-id="b8632-114">Det bästa du kan bygga vidare på den efter hello PoC har slutförts.</span><span class="sxs-lookup"><span data-stu-id="b8632-114">Ideally you can build upon it after hello PoC is completed.</span></span> <span data-ttu-id="b8632-115">hello målmiljön är avgörande bör du hitta hello rätt balans mellan att göra det som verkliga som möjligt och extra hello begränsningar eller ytterligare överväganden.</span><span class="sxs-lookup"><span data-stu-id="b8632-115">hello target environment is crucial and you should find hello right balance between making it as real as possible and hello overhead of constraints or extra considerations.</span></span> <span data-ttu-id="b8632-116">hello miljöer för POC är:</span><span class="sxs-lookup"><span data-stu-id="b8632-116">hello typical environments for PoCs are:</span></span>
* <span data-ttu-id="b8632-117">**Produktion:** hello scenarier kommer att genomföras i live-miljön och redan har distribuerats Microsoft Cloud-tjänsterna (produktion AD, Office 365, Azure AD-klient/SSO lösning).</span><span class="sxs-lookup"><span data-stu-id="b8632-117">**Production:** hello scenarios will be implemented in your live environment and already deployed Microsoft Cloud services (production AD, Office 365, Azure AD tenant/SSO solution).</span></span> 
* <span data-ttu-id="b8632-118">**Användaren godkänner testa (UAT) / utvecklingsmiljö:** du har test-infrastruktur (parallell AD och eventuellt Azure AD-klient/SSO-lösning) med testdata som liknar produktion.</span><span class="sxs-lookup"><span data-stu-id="b8632-118">**User Acceptance Test (UAT)/Dev environment:** You have test infrastructure (parallel AD and potentially Azure AD tenant/SSO solution) with test data that resembles production.</span></span> <span data-ttu-id="b8632-119">Normalt delas hello testmiljö mellan flera projekt i hello företag.</span><span class="sxs-lookup"><span data-stu-id="b8632-119">Typically, hello test environment is shared across multiple projects in hello enterprise.</span></span>

<span data-ttu-id="b8632-120">De flesta scenarier i den här handboken är additiva till sin natur.</span><span class="sxs-lookup"><span data-stu-id="b8632-120">Most scenarios in this guide are additive in nature.</span></span> <span data-ttu-id="b8632-121">De kan därför distribueras i hello produktion klienten utan att påverka användare utanför hello PoC.</span><span class="sxs-lookup"><span data-stu-id="b8632-121">As a result, they can be deployed in hello production tenant without affecting users outside hello PoC.</span></span> <span data-ttu-id="b8632-122">I det här dokumentet ringer vi ut vilka scenarier som skulle ha klient hela effekt.</span><span class="sxs-lookup"><span data-stu-id="b8632-122">Throughout this document, we will be calling out which scenarios would have tenant-wide effect.</span></span> <span data-ttu-id="b8632-123">I sådana fall kanske du vill tooconsider en icke-produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="b8632-123">In those cases, you might want tooconsider a non-production environment.</span></span> 


## <a name="target-users"></a><span data-ttu-id="b8632-124">Målgruppsanvändare</span><span class="sxs-lookup"><span data-stu-id="b8632-124">Target Users</span></span>

<span data-ttu-id="b8632-125">Det är viktigt toodetermine hello måluppsättningen med användare som kommer utöva hello scenarier, särskilt när hello-miljö är produktions- och.</span><span class="sxs-lookup"><span data-stu-id="b8632-125">It is important toodetermine hello target set of users that will exercise hello scenarios, especially when hello environment is production or test.</span></span> <span data-ttu-id="b8632-126">hello användarkategorier mål för PoC är:</span><span class="sxs-lookup"><span data-stu-id="b8632-126">hello categories of target users for PoC are:</span></span>
* <span data-ttu-id="b8632-127">**Pilotanvändarna:** verkliga användare i hello-miljö som kommer att använda hello lösning med hello konto de använder för sina dag tooday jobbfunktioner</span><span class="sxs-lookup"><span data-stu-id="b8632-127">**Pilot Users:** Real users in hello environment that will be using hello solution with hello account they use for their day tooday job functions</span></span>
* <span data-ttu-id="b8632-128">**Testanvändare:** testa konton som har skapats i hello-miljö</span><span class="sxs-lookup"><span data-stu-id="b8632-128">**Test Users:** Test accounts created in hello environment</span></span> 

<span data-ttu-id="b8632-129">De flesta scenarier i den här guiden kan utnyttjas av pilotanvändare.</span><span class="sxs-lookup"><span data-stu-id="b8632-129">Most scenarios in this guide can be exercised by pilot users.</span></span> <span data-ttu-id="b8632-130">I det här dokumentet kommer vi ringer ut mål användaren överväganden om det behövs.</span><span class="sxs-lookup"><span data-stu-id="b8632-130">Throughout this document, we will be calling out target user considerations if needed.</span></span>


[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]
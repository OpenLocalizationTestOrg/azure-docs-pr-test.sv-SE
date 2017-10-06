---
title: "Azure Active Directory B2C: Kommentarer för utvecklare om hur du använder anpassade principer | Microsoft Docs"
description: "Information för utvecklare på Konfigurera och underhålla Azure AD B2C med anpassade principer"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 05/05/2017
ms.author: joroja
ms.openlocfilehash: 979b8a264eb819ee4a208b9171a53a5ffbf062c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="release-notes-for-azure-active-directory-b2c-custom-policy-public-preview"></a><span data-ttu-id="47e0e-103">Viktig information för en förhandsversion av Azure Active Directory B2C anpassad princip</span><span class="sxs-lookup"><span data-stu-id="47e0e-103">Release notes for Azure Active Directory B2C custom policy public preview</span></span>
<span data-ttu-id="47e0e-104">hello anpassad princip funktioner är nu tillgänglig för utvärdering under förhandsversion för alla Azure Active Directory B2C (Azure AD B2C) kunder.</span><span class="sxs-lookup"><span data-stu-id="47e0e-104">hello custom policy feature set is now available for evaluation under public preview for all Azure Active Directory B2C (Azure AD B2C) customers.</span></span> <span data-ttu-id="47e0e-105">Den här funktionsuppsättningen är inriktad på avancerade identitet utvecklare skapar hello mest komplexa identitetslösningar.</span><span class="sxs-lookup"><span data-stu-id="47e0e-105">This feature set is targeted at advanced identity developers building hello most complex identity solutions.</span></span>  

<span data-ttu-id="47e0e-106">Idag kräver den här funktionsuppsättningen utvecklare tooconfigure hello identitet upplevelse Framework direkt via redigering av XML-filen.</span><span class="sxs-lookup"><span data-stu-id="47e0e-106">Today, this feature set requires developers tooconfigure hello Identity Experience Framework directly via XML file editing.</span></span> <span data-ttu-id="47e0e-107">Den här metoden i konfigurationen är kraftfulla och komplexa.</span><span class="sxs-lookup"><span data-stu-id="47e0e-107">This method of configuration is powerful and complex.</span></span> <span data-ttu-id="47e0e-108">Avancerade identitet utvecklare som använder hello bör identitet upplevelse Framework planera tooinvest lite tid att slutföra genomgångar och läsa referensdokument.</span><span class="sxs-lookup"><span data-stu-id="47e0e-108">Advanced identity developers using hello Identity Experience Framework should plan tooinvest some time completing walk-throughs and reading reference documents.</span></span> 

## <a name="features-included-in-this-public-preview"></a><span data-ttu-id="47e0e-109">Funktioner som ingår i denna förhandsversion</span><span class="sxs-lookup"><span data-stu-id="47e0e-109">Features included in this public preview</span></span>
<span data-ttu-id="47e0e-110">Med hello nya funktioner som introduceras i hello förhandsversion, kan utvecklare utföra hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="47e0e-110">With hello new features introduced in hello public preview, developers can perform hello following tasks:</span></span><br>

* <span data-ttu-id="47e0e-111">Författare och överföra anpassad autentisering användaren transporter med hjälp av anpassade principer.</span><span class="sxs-lookup"><span data-stu-id="47e0e-111">Author and upload custom authentication user journeys by using custom policies.</span></span> 
   * <span data-ttu-id="47e0e-112">Beskriv användaren resor stegvisa som utbyten mellan Anspråksproviders.</span><span class="sxs-lookup"><span data-stu-id="47e0e-112">Describe user journeys step-by-step as exchanges between claims providers.</span></span> 
   * <span data-ttu-id="47e0e-113">Definiera villkorliga grenar i resor för användaren.</span><span class="sxs-lookup"><span data-stu-id="47e0e-113">Define conditional branching in user journeys.</span></span> 
* <span data-ttu-id="47e0e-114">Integrera REST API-aktiverade tjänster i din användare resor för anpassad autentisering.</span><span class="sxs-lookup"><span data-stu-id="47e0e-114">Integrate REST API-enabled services in your custom authentication user journeys.</span></span>  
* <span data-ttu-id="47e0e-115">Lägg till federation med identitetsleverantörer som är kompatibla med hello OpenIDConnect som standard.</span><span class="sxs-lookup"><span data-stu-id="47e0e-115">Add federation with identity providers that are compliant with hello OpenIDConnect standard.</span></span> <br>
* <span data-ttu-id="47e0e-116">Lägg till federation med identitetsleverantörer som följer toohello SAML 2.0-protokollet.</span><span class="sxs-lookup"><span data-stu-id="47e0e-116">Add federation with identity providers that adhere toohello SAML 2.0 protocol.</span></span> 

## <a name="terms-of-hello-public-preview"></a><span data-ttu-id="47e0e-117">Villkor för hello förhandsversion</span><span class="sxs-lookup"><span data-stu-id="47e0e-117">Terms of hello public preview</span></span>

* <span data-ttu-id="47e0e-118">Vi rekommenderar att du toouse hello nya funktioner i utvärderingssyfte.</span><span class="sxs-lookup"><span data-stu-id="47e0e-118">We encourage you toouse hello new features for evaluation purposes only.</span></span><br>
* <span data-ttu-id="47e0e-119">Nya funktioner är inte avsedd att användas i en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="47e0e-119">The new features are not intended for use in a production environment.</span></span><br>
* <span data-ttu-id="47e0e-120">Servicenivåavtal (SLA) gäller inte toohello nya funktioner.</span><span class="sxs-lookup"><span data-stu-id="47e0e-120">Service level agreements (SLAs) do not apply toohello new features.</span></span> <br>
* <span data-ttu-id="47e0e-121">Supportärenden kan registreras via vanlig supportkanaler.</span><span class="sxs-lookup"><span data-stu-id="47e0e-121">Support requests can be filed through regular support channels.</span></span> <br>
* <span data-ttu-id="47e0e-122">Det finns inga framtida datum för allmän tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="47e0e-122">There is no promised date for general availability.</span></span><br>
* <span data-ttu-id="47e0e-123">Vår gottfinnande och av någon anledning, kan Microsoft flagga och avvisa eller begränsa scenarier och användaren resor som överskrider hello omfattning hello Azure AD B2C produkten auktoriserad tooserve som plattform för hantering (CIAM) av identitets- och en kund.</span><span class="sxs-lookup"><span data-stu-id="47e0e-123">At our discretion, and for any reason, Microsoft can flag and reject or restrict scenarios and user journeys that exceed hello scope of hello Azure AD B2C product charter tooserve as a customer identity and access management (CIAM) platform.</span></span>

## <a name="responsibilities-of-custom-policy-feature-set-developers"></a><span data-ttu-id="47e0e-124">Ansvaret för anpassad princip funktionsuppsättningen utvecklare</span><span class="sxs-lookup"><span data-stu-id="47e0e-124">Responsibilities of custom policy feature-set developers</span></span>
<span data-ttu-id="47e0e-125">Manuell konfiguration ger lägre behörighet toohello underliggande plattform på Azure AD B2C och resulterar i en unik helt anpassningsbar förtroende ram hello skapas.</span><span class="sxs-lookup"><span data-stu-id="47e0e-125">Manual policy configuration grants lower-level access toohello underlying platform of Azure AD B2C and results in hello creation of a unique, fully customizable trust framework.</span></span> <span data-ttu-id="47e0e-126">Möjliga varianter av anpassade identitetsleverantörer, Betrodda relationer integration med externa tjänster och stegvisa arbetsflöden Placera större krav på hello avancerade utvecklare som använder dem.</span><span class="sxs-lookup"><span data-stu-id="47e0e-126">The possible permutations of custom identity providers, trust relationships, integrations with external services, and step-by-step workflows place greater demands on hello advanced developers consuming them.</span></span>

<span data-ttu-id="47e0e-127">toofully förmånen från hello förhandsversion, vi rekommenderar att utvecklare förbrukar hello anpassad princip funktionsuppsättningen följer toohello följande riktlinjer:</span><span class="sxs-lookup"><span data-stu-id="47e0e-127">toofully benefit from hello public preview, we suggest that developers consuming hello custom policy feature set adhere toohello following guidelines:</span></span>
* <span data-ttu-id="47e0e-128">Bekanta dig med nyckel/hemligheter hantering och hello configuration språk hello identitet upplevelse motorn.</span><span class="sxs-lookup"><span data-stu-id="47e0e-128">Become familiar with hello configuration language of hello Identity Experience Engine and key/secrets management.</span></span>
* <span data-ttu-id="47e0e-129">Bli ägare till scenarier och anpassad integrering.</span><span class="sxs-lookup"><span data-stu-id="47e0e-129">Take ownership of scenarios and custom integrations.</span></span>
* <span data-ttu-id="47e0e-130">Utföra metodisk scenariotestning.</span><span class="sxs-lookup"><span data-stu-id="47e0e-130">Perform methodical scenario testing.</span></span>
* <span data-ttu-id="47e0e-131">Följ programvaruutveckling och mellanlagring metodtips med minst en utvecklings- och testmiljö och en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="47e0e-131">Follow software development and staging best practices with a minimum of one development and testing environment and one production environment.</span></span>
* <span data-ttu-id="47e0e-132">Håll dig informerad om förändringar från hello identitetsleverantörer och tjänster som du integrerar med.</span><span class="sxs-lookup"><span data-stu-id="47e0e-132">Stay informed about new developments from hello identity providers and services you integrate with.</span></span> <span data-ttu-id="47e0e-133">Till exempel hålla koll på ändringar i hemligheter och planerade och oplanerade ändringar toohello service.</span><span class="sxs-lookup"><span data-stu-id="47e0e-133">For example, keep track of changes in secrets and of scheduled and unscheduled changes toohello service.</span></span>
* <span data-ttu-id="47e0e-134">Konfigurera active övervakning och övervaka hello svarstider för produktionsmiljöer.</span><span class="sxs-lookup"><span data-stu-id="47e0e-134">Set up active monitoring, and monitor hello responsiveness of production environments.</span></span>
* <span data-ttu-id="47e0e-135">Behåll det aktuella kontakta e-postadresser och håll responsiv toohello Microsoft live-plats-teamet e-postmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="47e0e-135">Keep contact email addresses current, and stay responsive toohello Microsoft live-site team emails.</span></span>
* <span data-ttu-id="47e0e-136">Vidta lämpliga åtgärder när uppmanade toodo hello genom Microsoft live-plats-teamet.</span><span class="sxs-lookup"><span data-stu-id="47e0e-136">Take timely action when advised toodo so by hello Microsoft live-site team.</span></span> 


>[!NOTE]
><span data-ttu-id="47e0e-137">Dessa funktioner kan slutligen ingå i Azure AD inbyggda principer, vilket gör dem lättare att använda tooall-utvecklare.</span><span class="sxs-lookup"><span data-stu-id="47e0e-137">These features might eventually be included in Azure AD built-in policies, making them more accessible tooall developers.</span></span>

## <a name="next-steps"></a><span data-ttu-id="47e0e-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="47e0e-138">Next steps</span></span>
<span data-ttu-id="47e0e-139">[Kom igång med anpassade principer](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="47e0e-139">[Get started with custom policies](active-directory-b2c-get-started-custom.md).</span></span>

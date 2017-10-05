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
ms.openlocfilehash: a5f222e5b11e05286152a9f1cc55d2c3fc27a9dc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="release-notes-for-azure-active-directory-b2c-custom-policy-public-preview"></a><span data-ttu-id="24bf3-103">Viktig information för en förhandsversion av Azure Active Directory B2C anpassad princip</span><span class="sxs-lookup"><span data-stu-id="24bf3-103">Release notes for Azure Active Directory B2C custom policy public preview</span></span>
<span data-ttu-id="24bf3-104">Funktionsuppsättningen anpassad princip är nu tillgänglig för utvärdering under förhandsversion för alla Azure Active Directory B2C (Azure AD B2C) kunder.</span><span class="sxs-lookup"><span data-stu-id="24bf3-104">The custom policy feature set is now available for evaluation under public preview for all Azure Active Directory B2C (Azure AD B2C) customers.</span></span> <span data-ttu-id="24bf3-105">Den här funktionsuppsättningen är inriktad på avancerade identitet utvecklare som bygger mest komplexa identitetslösningar.</span><span class="sxs-lookup"><span data-stu-id="24bf3-105">This feature set is targeted at advanced identity developers building the most complex identity solutions.</span></span>  

<span data-ttu-id="24bf3-106">Idag kräver den här funktionsuppsättningen konfigurera identitet upplevelse Framework direkt via redigering av XML-filen.</span><span class="sxs-lookup"><span data-stu-id="24bf3-106">Today, this feature set requires developers to configure the Identity Experience Framework directly via XML file editing.</span></span> <span data-ttu-id="24bf3-107">Den här metoden i konfigurationen är kraftfulla och komplexa.</span><span class="sxs-lookup"><span data-stu-id="24bf3-107">This method of configuration is powerful and complex.</span></span> <span data-ttu-id="24bf3-108">Avancerade identitet utvecklare som använder identitet upplevelse Framework bör planera att investera lite tid att slutföra genomgångar och läsa referensdokument.</span><span class="sxs-lookup"><span data-stu-id="24bf3-108">Advanced identity developers using the Identity Experience Framework should plan to invest some time completing walk-throughs and reading reference documents.</span></span> 

## <a name="features-included-in-this-public-preview"></a><span data-ttu-id="24bf3-109">Funktioner som ingår i denna förhandsversion</span><span class="sxs-lookup"><span data-stu-id="24bf3-109">Features included in this public preview</span></span>
<span data-ttu-id="24bf3-110">Med de nya funktionerna som introducerades i public preview kan utvecklare göra följande:</span><span class="sxs-lookup"><span data-stu-id="24bf3-110">With the new features introduced in the public preview, developers can perform the following tasks:</span></span><br>

* <span data-ttu-id="24bf3-111">Författare och överföra anpassad autentisering användaren transporter med hjälp av anpassade principer.</span><span class="sxs-lookup"><span data-stu-id="24bf3-111">Author and upload custom authentication user journeys by using custom policies.</span></span> 
   * <span data-ttu-id="24bf3-112">Beskriv användaren resor stegvisa som utbyten mellan Anspråksproviders.</span><span class="sxs-lookup"><span data-stu-id="24bf3-112">Describe user journeys step-by-step as exchanges between claims providers.</span></span> 
   * <span data-ttu-id="24bf3-113">Definiera villkorliga grenar i resor för användaren.</span><span class="sxs-lookup"><span data-stu-id="24bf3-113">Define conditional branching in user journeys.</span></span> 
* <span data-ttu-id="24bf3-114">Integrera REST API-aktiverade tjänster i din användare resor för anpassad autentisering.</span><span class="sxs-lookup"><span data-stu-id="24bf3-114">Integrate REST API-enabled services in your custom authentication user journeys.</span></span>  
* <span data-ttu-id="24bf3-115">Lägg till federation med identitetsleverantörer som är kompatibla med OpenIDConnect standard.</span><span class="sxs-lookup"><span data-stu-id="24bf3-115">Add federation with identity providers that are compliant with the OpenIDConnect standard.</span></span> <br>
* <span data-ttu-id="24bf3-116">Lägg till federation med identitetsleverantörer som följer SAML 2.0-protokollet.</span><span class="sxs-lookup"><span data-stu-id="24bf3-116">Add federation with identity providers that adhere to the SAML 2.0 protocol.</span></span> 

## <a name="terms-of-the-public-preview"></a><span data-ttu-id="24bf3-117">Villkoren i public preview</span><span class="sxs-lookup"><span data-stu-id="24bf3-117">Terms of the public preview</span></span>

* <span data-ttu-id="24bf3-118">Vi rekommenderar att du kan använda de nya funktionerna i utvärderingssyfte.</span><span class="sxs-lookup"><span data-stu-id="24bf3-118">We encourage you to use the new features for evaluation purposes only.</span></span><br>
* <span data-ttu-id="24bf3-119">Nya funktioner är inte avsedd att användas i en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="24bf3-119">The new features are not intended for use in a production environment.</span></span><br>
* <span data-ttu-id="24bf3-120">Servicenivåavtal (SLA) gäller inte för de nya funktionerna.</span><span class="sxs-lookup"><span data-stu-id="24bf3-120">Service level agreements (SLAs) do not apply to the new features.</span></span> <br>
* <span data-ttu-id="24bf3-121">Supportärenden kan registreras via vanlig supportkanaler.</span><span class="sxs-lookup"><span data-stu-id="24bf3-121">Support requests can be filed through regular support channels.</span></span> <br>
* <span data-ttu-id="24bf3-122">Det finns inga framtida datum för allmän tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="24bf3-122">There is no promised date for general availability.</span></span><br>
* <span data-ttu-id="24bf3-123">Vår gottfinnande och av någon anledning, kan Microsoft flagga och avvisa eller begränsa scenarier och användaren resor som överskrider omfånget för Azure AD B2C produkten auktoriserad som fungerar som en kund identitets- och (CIAM) plattform.</span><span class="sxs-lookup"><span data-stu-id="24bf3-123">At our discretion, and for any reason, Microsoft can flag and reject or restrict scenarios and user journeys that exceed the scope of the Azure AD B2C product charter to serve as a customer identity and access management (CIAM) platform.</span></span>

## <a name="responsibilities-of-custom-policy-feature-set-developers"></a><span data-ttu-id="24bf3-124">Ansvaret för anpassad princip funktionsuppsättningen utvecklare</span><span class="sxs-lookup"><span data-stu-id="24bf3-124">Responsibilities of custom policy feature-set developers</span></span>
<span data-ttu-id="24bf3-125">Manuell konfiguration ger lågnivå-åtkomst till den underliggande plattformen för Azure AD B2C och resulterar i att skapa ett unikt, helt anpassningsbar förtroende ramverk.</span><span class="sxs-lookup"><span data-stu-id="24bf3-125">Manual policy configuration grants lower-level access to the underlying platform of Azure AD B2C and results in the creation of a unique, fully customizable trust framework.</span></span> <span data-ttu-id="24bf3-126">Möjliga varianter av anpassade identitetsleverantörer, Betrodda relationer integration med externa tjänster och stegvisa arbetsflöden Placera större krav på avancerade utvecklare som använder dem.</span><span class="sxs-lookup"><span data-stu-id="24bf3-126">The possible permutations of custom identity providers, trust relationships, integrations with external services, and step-by-step workflows place greater demands on the advanced developers consuming them.</span></span>

<span data-ttu-id="24bf3-127">Om du vill utnyttja alla fördelar som förhandsversion, föreslår vi att utvecklare förbrukar funktionsuppsättningen anpassad princip följer riktlinjerna nedan:</span><span class="sxs-lookup"><span data-stu-id="24bf3-127">To fully benefit from the public preview, we suggest that developers consuming the custom policy feature set adhere to the following guidelines:</span></span>
* <span data-ttu-id="24bf3-128">Bekanta dig med Identity Experience-motorn och nyckel/hemligheter management configuration språk.</span><span class="sxs-lookup"><span data-stu-id="24bf3-128">Become familiar with the configuration language of the Identity Experience Engine and key/secrets management.</span></span>
* <span data-ttu-id="24bf3-129">Bli ägare till scenarier och anpassad integrering.</span><span class="sxs-lookup"><span data-stu-id="24bf3-129">Take ownership of scenarios and custom integrations.</span></span>
* <span data-ttu-id="24bf3-130">Utföra metodisk scenariotestning.</span><span class="sxs-lookup"><span data-stu-id="24bf3-130">Perform methodical scenario testing.</span></span>
* <span data-ttu-id="24bf3-131">Följ programvaruutveckling och mellanlagring metodtips med minst en utvecklings- och testmiljö och en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="24bf3-131">Follow software development and staging best practices with a minimum of one development and testing environment and one production environment.</span></span>
* <span data-ttu-id="24bf3-132">Håll dig informerad om förändringar från leverantörer och tjänster som du integrerar med.</span><span class="sxs-lookup"><span data-stu-id="24bf3-132">Stay informed about new developments from the identity providers and services you integrate with.</span></span> <span data-ttu-id="24bf3-133">Till exempel hålla koll på ändringar i hemligheter och planerade och oplanerade ändringar av tjänsten.</span><span class="sxs-lookup"><span data-stu-id="24bf3-133">For example, keep track of changes in secrets and of scheduled and unscheduled changes to the service.</span></span>
* <span data-ttu-id="24bf3-134">Konfigurera active övervakning och övervaka svarstiderna hos produktionsmiljöer.</span><span class="sxs-lookup"><span data-stu-id="24bf3-134">Set up active monitoring, and monitor the responsiveness of production environments.</span></span>
* <span data-ttu-id="24bf3-135">Behåll det aktuella kontakta e-postadresser och håll kan svara på e-postmeddelanden Microsoft live-plats-teamet.</span><span class="sxs-lookup"><span data-stu-id="24bf3-135">Keep contact email addresses current, and stay responsive to the Microsoft live-site team emails.</span></span>
* <span data-ttu-id="24bf3-136">Vidta lämpliga åtgärder när du gör detta genom att Microsoft live-plats-teamet.</span><span class="sxs-lookup"><span data-stu-id="24bf3-136">Take timely action when advised to do so by the Microsoft live-site team.</span></span> 


>[!NOTE]
><span data-ttu-id="24bf3-137">Dessa funktioner kan slutligen ingå i Azure AD inbyggda principer, vilket gör dem mer tillgänglig för alla utvecklare.</span><span class="sxs-lookup"><span data-stu-id="24bf3-137">These features might eventually be included in Azure AD built-in policies, making them more accessible to all developers.</span></span>

## <a name="next-steps"></a><span data-ttu-id="24bf3-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="24bf3-138">Next steps</span></span>
<span data-ttu-id="24bf3-139">[Kom igång med anpassade principer](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="24bf3-139">[Get started with custom policies](active-directory-b2c-get-started-custom.md).</span></span>

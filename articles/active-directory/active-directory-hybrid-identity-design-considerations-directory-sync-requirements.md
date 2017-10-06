---
title: "designöverväganden för aaaAzure Active Directory hybrid identity - fastställa kraven för directory-synkronisering | Microsoft Docs"
description: "Identifiera vilka krav som behövs för att synkronisera alla hello användare mellan on = lokala och molnbaserade för hello enterprise."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: 593eaa71-17eb-4c16-8c98-43cc62987e65
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 6646e3792c65f37c3d62eecdb6c6f3bd257f04f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="determine-directory-synchronization-requirements"></a><span data-ttu-id="26f90-103">Ange krav för directory-synkronisering</span><span class="sxs-lookup"><span data-stu-id="26f90-103">Determine directory synchronization requirements</span></span>
<span data-ttu-id="26f90-104">Synkronisering är att tillhandahålla användare en identitet i hello molnet baserat på deras lokala identitet.</span><span class="sxs-lookup"><span data-stu-id="26f90-104">Synchronization is all about providing users an identity in hello cloud based on their on-premises identity.</span></span> <span data-ttu-id="26f90-105">Oavsett om de ska använda synkroniserade kontot för autentisering eller federerad autentisering, måste hello användarna toohave en identitet i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="26f90-105">Whether or not they will use synchronized account for authentication or federated authentication, hello users will still need toohave an identity in hello cloud.</span></span>  <span data-ttu-id="26f90-106">Den här identiteten måste toobe underhålls och uppdateras regelbundet.</span><span class="sxs-lookup"><span data-stu-id="26f90-106">This identity will need toobe maintained and updated periodically.</span></span>  <span data-ttu-id="26f90-107">hello uppdateringar kan ta många formulär från titel ändringar toopassword ändras.</span><span class="sxs-lookup"><span data-stu-id="26f90-107">hello updates can take many forms, from title changes toopassword changes.</span></span>  

<span data-ttu-id="26f90-108">Starta genom att utvärdera hello organisationer krav på lokal identitet lösningen och användare.</span><span class="sxs-lookup"><span data-stu-id="26f90-108">Start by evaluating hello organizations on-premises identity solution and user requirements.</span></span> <span data-ttu-id="26f90-109">Denna utvärdering är viktiga toodefine hello tekniska krav för hur användaridentiteter skapas och underhålls i molnet hello.</span><span class="sxs-lookup"><span data-stu-id="26f90-109">This evaluation is important toodefine hello technical requirements for how user identities will be created and maintained in hello cloud.</span></span>  <span data-ttu-id="26f90-110">Active Directory är lokala för en majoriteten av organisationer och hello på lokal katalog som användare kommer att synkroniseras från, men i vissa fall detta kommer inte att hello fallet.</span><span class="sxs-lookup"><span data-stu-id="26f90-110">For a majority of organizations, Active Directory is on-premises and will be hello on-premises directory that users will by synchronized from, however in some cases this will not be hello case.</span></span>  

<span data-ttu-id="26f90-111">Se till att tooanswer hello följande frågor:</span><span class="sxs-lookup"><span data-stu-id="26f90-111">Make sure tooanswer hello following questions:</span></span>

* <span data-ttu-id="26f90-112">Har du en AD-skog, flera eller ingen?</span><span class="sxs-lookup"><span data-stu-id="26f90-112">Do you have one AD forest, multiple, or none?</span></span>
  
  * <span data-ttu-id="26f90-113">Hur många Azure AD-kataloger kommer du att synkroniseras till?</span><span class="sxs-lookup"><span data-stu-id="26f90-113">How many Azure AD directories will you be synchronizing to?</span></span>
    
    1. <span data-ttu-id="26f90-114">Använder du filtrering?</span><span class="sxs-lookup"><span data-stu-id="26f90-114">Are you using filtering?</span></span>
    2. <span data-ttu-id="26f90-115">Har du flera Azure AD Connect-servrar som är planerat?</span><span class="sxs-lookup"><span data-stu-id="26f90-115">Do you have multiple Azure AD Connect servers planned?</span></span>
* <span data-ttu-id="26f90-116">Har du en synkronisering verktyget lokalt?</span><span class="sxs-lookup"><span data-stu-id="26f90-116">Do you currently have a synchronization tool on-premises?</span></span>
  
  * <span data-ttu-id="26f90-117">Om Ja, har du dina användare om användarna har en virtuell katalog/integrering av identiteter?</span><span class="sxs-lookup"><span data-stu-id="26f90-117">If yes, does your users if users have a virtual directory/integration of identities?</span></span>
* <span data-ttu-id="26f90-118">Har du alla andra directory lokalt som du vill toosynchronize (t.ex. LDAP-katalogen, HR-databasen, osv)?</span><span class="sxs-lookup"><span data-stu-id="26f90-118">Do you have any other directory on-premises that you want toosynchronize (e.g. LDAP Directory, HR database, etc)?</span></span>
  * <span data-ttu-id="26f90-119">Ska du göra några GALSync toobe?</span><span class="sxs-lookup"><span data-stu-id="26f90-119">Are you going toobe doing any GALSync?</span></span>
  * <span data-ttu-id="26f90-120">Vad är hello aktuell status för UPN-namn i din organisation?</span><span class="sxs-lookup"><span data-stu-id="26f90-120">What is hello current state of UPNs in your organization?</span></span> 
  * <span data-ttu-id="26f90-121">Har du en annan katalog som användare autentiseras mot?</span><span class="sxs-lookup"><span data-stu-id="26f90-121">Do you have a different directory that users authenticate against?</span></span>
  * <span data-ttu-id="26f90-122">Använder företaget Microsoft Exchange?</span><span class="sxs-lookup"><span data-stu-id="26f90-122">Does your company use Microsoft Exchange?</span></span>
    * <span data-ttu-id="26f90-123">Kommer de att en exchange-hybridinstallation?</span><span class="sxs-lookup"><span data-stu-id="26f90-123">Do they plan of having a hybrid exchange deployment?</span></span>

<span data-ttu-id="26f90-124">Nu när du har en uppfattning om synkroniseringskrav för måste toodetermine vilket verktyg som hello rätt en toomeet dessa krav.</span><span class="sxs-lookup"><span data-stu-id="26f90-124">Now that you have an idea about your synchronization requirements, you need toodetermine which tool is hello correct one toomeet these requirements.</span></span>  <span data-ttu-id="26f90-125">Microsoft erbjuder flera verktyg tooaccomplish directory-integrering och synkronisering.</span><span class="sxs-lookup"><span data-stu-id="26f90-125">Microsoft provides several tools tooaccomplish directory integration and synchronization.</span></span>  <span data-ttu-id="26f90-126">Se hello [Hybrididentitet directory integration verktyg jämförelsetabell](active-directory-hybrid-identity-design-considerations-tools-comparison.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="26f90-126">See hello [Hybrid Identity directory integration tools comparison table](active-directory-hybrid-identity-design-considerations-tools-comparison.md) for more information.</span></span> 

<span data-ttu-id="26f90-127">Nu när du har synkroniseringskrav och hello-verktyg som gör det för ditt företag måste tooevaluate hello program som använder dessa katalogtjänster.</span><span class="sxs-lookup"><span data-stu-id="26f90-127">Now that you have your synchronization requirements and hello tool that will accomplish this for your company, you need tooevaluate hello applications that use these directory services.</span></span> <span data-ttu-id="26f90-128">Denna utvärdering är viktigt toodefine hello tekniska krav toointegrate dessa program toohello moln.</span><span class="sxs-lookup"><span data-stu-id="26f90-128">This evaluation is important toodefine hello technical requirements toointegrate these applications toohello cloud.</span></span> <span data-ttu-id="26f90-129">Se till att tooanswer hello följande frågor:</span><span class="sxs-lookup"><span data-stu-id="26f90-129">Make sure tooanswer hello following questions:</span></span>

* <span data-ttu-id="26f90-130">Dessa program kommer att flyttas toohello molnet och använda hello-katalog?</span><span class="sxs-lookup"><span data-stu-id="26f90-130">Will these applications be moved toohello cloud and use hello directory?</span></span>
* <span data-ttu-id="26f90-131">Finns det särskilda attribut som måste synkroniseras toobe toohello molnet så att programmen kan använda dem har?</span><span class="sxs-lookup"><span data-stu-id="26f90-131">Are there special attributes that need toobe synchronized toohello cloud so these applications can use them successfully?</span></span>
* <span data-ttu-id="26f90-132">Behöver programmen toobe igen skrivs tootake utnyttja molnet auth?</span><span class="sxs-lookup"><span data-stu-id="26f90-132">Will these applications need toobe re-written tootake advantage of cloud auth?</span></span>
* <span data-ttu-id="26f90-133">Dessa program fortsätter toolive lokalt medan användarna komma åt dem med hjälp av molnidentitet hello?</span><span class="sxs-lookup"><span data-stu-id="26f90-133">Will these applications continue toolive on-premises while users access them using hello cloud identity?</span></span>

<span data-ttu-id="26f90-134">Du måste också toodetermine hello säkerhet kraven och begränsningarna katalogsynkronisering.</span><span class="sxs-lookup"><span data-stu-id="26f90-134">You also need toodetermine hello security requirements and constraints directory synchronization.</span></span> <span data-ttu-id="26f90-135">Denna utvärdering är viktiga tooget en lista över hello kraven som krävs i ordning toocreate och underhålla användaridentiteter i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="26f90-135">This evaluation is important tooget a list of hello requirements that will be needed in order toocreate and maintain user’s identities in hello cloud.</span></span> <span data-ttu-id="26f90-136">Se till att tooanswer hello följande frågor:</span><span class="sxs-lookup"><span data-stu-id="26f90-136">Make sure tooanswer hello following questions:</span></span>

* <span data-ttu-id="26f90-137">Där hello synkroniseringsserver placeras?</span><span class="sxs-lookup"><span data-stu-id="26f90-137">Where will hello synchronization server be located?</span></span>
* <span data-ttu-id="26f90-138">Kommer det att vara ansluten till en domän?</span><span class="sxs-lookup"><span data-stu-id="26f90-138">Will it be domain joined?</span></span>
* <span data-ttu-id="26f90-139">Hello-servern finns på ett begränsat nätverk bakom en brandvägg, till exempel en DMZ?</span><span class="sxs-lookup"><span data-stu-id="26f90-139">Will hello server be located on a restricted network behind a firewall, such as a DMZ?</span></span>
  * <span data-ttu-id="26f90-140">Kommer du att kunna tooopen hello krävs brandväggen portar toosupport synkronisering?</span><span class="sxs-lookup"><span data-stu-id="26f90-140">Will you be able tooopen hello required firewall ports toosupport synchronization?</span></span>
* <span data-ttu-id="26f90-141">Har du en återställningsplan för hello synkroniseringsserver?</span><span class="sxs-lookup"><span data-stu-id="26f90-141">Do you have a disaster recovery plan for hello synchronization server?</span></span>
* <span data-ttu-id="26f90-142">Har du ett konto med hello rätt behörigheter för alla skogar som du vill toosynch med?</span><span class="sxs-lookup"><span data-stu-id="26f90-142">Do you have an account with hello correct permissions for all forests you want toosynch with?</span></span>
  * <span data-ttu-id="26f90-143">Om företaget inte vet hello svar för den här frågan, granska hello ”behörigheter för Lösenordssynkronisering” i avsnittet hello artikel [installera hello Azure Active Directory Sync Service](https://msdn.microsoft.com/library/azure/dn757602.aspx#BKMK_CreateAnADAccountForTheSyncService) och ta reda på om du redan har en konto med dessa behörigheter eller om du behöver toocreate en.</span><span class="sxs-lookup"><span data-stu-id="26f90-143">If your company doesn’t know hello answer for this question, review hello section “Permissions for password synchronization” in hello article [Install hello Azure Active Directory Sync Service](https://msdn.microsoft.com/library/azure/dn757602.aspx#BKMK_CreateAnADAccountForTheSyncService) and determine if you already have an account with these permissions or if you need toocreate one.</span></span>
* <span data-ttu-id="26f90-144">Om du har inte mutli-skog sync hello synkronisering kan tooget tooeach-serverskogen?</span><span class="sxs-lookup"><span data-stu-id="26f90-144">If you have mutli-forest sync is hello sync server able tooget tooeach forest?</span></span>

> [!NOTE]
> <span data-ttu-id="26f90-145">Se till att tootake anteckningar för varje svar och förstå hello anledningen hello svaret.</span><span class="sxs-lookup"><span data-stu-id="26f90-145">Make sure tootake notes of each answer and understand hello rationale behind hello answer.</span></span> <span data-ttu-id="26f90-146">[Fastställa kraven på incidentsvar](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) kommer att överskrida hello-alternativ som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="26f90-146">[Determine incident response requirements](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) will go over hello options available.</span></span> <span data-ttu-id="26f90-147">Har besvarat frågorna väljer du vilket alternativ som bäst passar dina behöver.</span><span class="sxs-lookup"><span data-stu-id="26f90-147">By having answered those questions you will select which option best suits your business needs.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="26f90-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="26f90-148">Next steps</span></span>
[<span data-ttu-id="26f90-149">Ange krav för multifaktorautentisering</span><span class="sxs-lookup"><span data-stu-id="26f90-149">Determine multi-factor authentication requirements</span></span>](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="see-also"></a><span data-ttu-id="26f90-150">Se även</span><span class="sxs-lookup"><span data-stu-id="26f90-150">See also</span></span>
[<span data-ttu-id="26f90-151">Översikt över design-överväganden</span><span class="sxs-lookup"><span data-stu-id="26f90-151">Design considerations overview</span></span>](active-directory-hybrid-identity-design-considerations-overview.md)


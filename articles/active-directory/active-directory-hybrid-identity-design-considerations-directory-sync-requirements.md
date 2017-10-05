---
title: "Azure Active Directory hybrid identity designöverväganden - fastställa kraven för directory-synkronisering | Microsoft Docs"
description: "Identifiera vilka krav som behövs för att synkronisera alla användare mellan on = lokala och molnbaserade för företaget."
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
ms.openlocfilehash: 5ef87e606f055359ca325befd6048353ce57ca2b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="determine-directory-synchronization-requirements"></a><span data-ttu-id="a06d3-103">Ange krav för directory-synkronisering</span><span class="sxs-lookup"><span data-stu-id="a06d3-103">Determine directory synchronization requirements</span></span>
<span data-ttu-id="a06d3-104">Synkronisering är att tillhandahålla användare en identitet i molnet baserat på deras lokala identitet.</span><span class="sxs-lookup"><span data-stu-id="a06d3-104">Synchronization is all about providing users an identity in the cloud based on their on-premises identity.</span></span> <span data-ttu-id="a06d3-105">Oavsett om de ska använda synkroniserade kontot för autentisering eller federerad autentisering, behöver användarna fortfarande ha en identitet i molnet.</span><span class="sxs-lookup"><span data-stu-id="a06d3-105">Whether or not they will use synchronized account for authentication or federated authentication, the users will still need to have an identity in the cloud.</span></span>  <span data-ttu-id="a06d3-106">Den här identiteten behöver underhålls och uppdateras regelbundet.</span><span class="sxs-lookup"><span data-stu-id="a06d3-106">This identity will need to be maintained and updated periodically.</span></span>  <span data-ttu-id="a06d3-107">Uppdateringarna kan ta många formulär från titel ändras till ändring av lösenord.</span><span class="sxs-lookup"><span data-stu-id="a06d3-107">The updates can take many forms, from title changes to password changes.</span></span>  

<span data-ttu-id="a06d3-108">Börja med att utvärdera organisationer lokalt identitet lösningen och användaren kraven.</span><span class="sxs-lookup"><span data-stu-id="a06d3-108">Start by evaluating the organizations on-premises identity solution and user requirements.</span></span> <span data-ttu-id="a06d3-109">Denna utvärdering är viktigt att definiera de tekniska kraven för hur användaridentiteter skapas och underhålls i molnet.</span><span class="sxs-lookup"><span data-stu-id="a06d3-109">This evaluation is important to define the technical requirements for how user identities will be created and maintained in the cloud.</span></span>  <span data-ttu-id="a06d3-110">Active Directory är lokala för en majoriteten av organisationer och lokala katalogen som användare kommer att synkroniseras från, men i vissa fall det inte är fallet.</span><span class="sxs-lookup"><span data-stu-id="a06d3-110">For a majority of organizations, Active Directory is on-premises and will be the on-premises directory that users will by synchronized from, however in some cases this will not be the case.</span></span>  

<span data-ttu-id="a06d3-111">Se till att besvara följande frågor:</span><span class="sxs-lookup"><span data-stu-id="a06d3-111">Make sure to answer the following questions:</span></span>

* <span data-ttu-id="a06d3-112">Har du en AD-skog, flera eller ingen?</span><span class="sxs-lookup"><span data-stu-id="a06d3-112">Do you have one AD forest, multiple, or none?</span></span>
  
  * <span data-ttu-id="a06d3-113">Hur många Azure AD-kataloger kommer du att synkroniseras till?</span><span class="sxs-lookup"><span data-stu-id="a06d3-113">How many Azure AD directories will you be synchronizing to?</span></span>
    
    1. <span data-ttu-id="a06d3-114">Använder du filtrering?</span><span class="sxs-lookup"><span data-stu-id="a06d3-114">Are you using filtering?</span></span>
    2. <span data-ttu-id="a06d3-115">Har du flera Azure AD Connect-servrar som är planerat?</span><span class="sxs-lookup"><span data-stu-id="a06d3-115">Do you have multiple Azure AD Connect servers planned?</span></span>
* <span data-ttu-id="a06d3-116">Har du en synkronisering verktyget lokalt?</span><span class="sxs-lookup"><span data-stu-id="a06d3-116">Do you currently have a synchronization tool on-premises?</span></span>
  
  * <span data-ttu-id="a06d3-117">Om Ja, har du dina användare om användarna har en virtuell katalog/integrering av identiteter?</span><span class="sxs-lookup"><span data-stu-id="a06d3-117">If yes, does your users if users have a virtual directory/integration of identities?</span></span>
* <span data-ttu-id="a06d3-118">Har du alla andra directory lokalt som du vill synkronisera (t.ex. LDAP-katalogen, HR-databasen, osv)?</span><span class="sxs-lookup"><span data-stu-id="a06d3-118">Do you have any other directory on-premises that you want to synchronize (e.g. LDAP Directory, HR database, etc)?</span></span>
  * <span data-ttu-id="a06d3-119">Ska du göra alla GALSync</span><span class="sxs-lookup"><span data-stu-id="a06d3-119">Are you going to be doing any GALSync?</span></span>
  * <span data-ttu-id="a06d3-120">Vad är det aktuella tillståndet för UPN-namn i din organisation?</span><span class="sxs-lookup"><span data-stu-id="a06d3-120">What is the current state of UPNs in your organization?</span></span> 
  * <span data-ttu-id="a06d3-121">Har du en annan katalog som användare autentiseras mot?</span><span class="sxs-lookup"><span data-stu-id="a06d3-121">Do you have a different directory that users authenticate against?</span></span>
  * <span data-ttu-id="a06d3-122">Använder företaget Microsoft Exchange?</span><span class="sxs-lookup"><span data-stu-id="a06d3-122">Does your company use Microsoft Exchange?</span></span>
    * <span data-ttu-id="a06d3-123">Kommer de att en exchange-hybridinstallation?</span><span class="sxs-lookup"><span data-stu-id="a06d3-123">Do they plan of having a hybrid exchange deployment?</span></span>

<span data-ttu-id="a06d3-124">Nu när du har en uppfattning om synkroniseringskrav för måste fastställa vilket verktyg som är korrekt så att de uppfyller dessa krav.</span><span class="sxs-lookup"><span data-stu-id="a06d3-124">Now that you have an idea about your synchronization requirements, you need to determine which tool is the correct one to meet these requirements.</span></span>  <span data-ttu-id="a06d3-125">Microsoft erbjuder flera verktyg för att åstadkomma katalogintegrering och synkronisering.</span><span class="sxs-lookup"><span data-stu-id="a06d3-125">Microsoft provides several tools to accomplish directory integration and synchronization.</span></span>  <span data-ttu-id="a06d3-126">Finns det [Hybrididentitet directory integration verktyg jämförelsetabell](active-directory-hybrid-identity-design-considerations-tools-comparison.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="a06d3-126">See the [Hybrid Identity directory integration tools comparison table](active-directory-hybrid-identity-design-considerations-tools-comparison.md) for more information.</span></span> 

<span data-ttu-id="a06d3-127">Nu när du har synkroniseringskrav för och verktyg som gör det för ditt företag, som du behöver utvärdera de program som använder dessa katalogtjänster.</span><span class="sxs-lookup"><span data-stu-id="a06d3-127">Now that you have your synchronization requirements and the tool that will accomplish this for your company, you need to evaluate the applications that use these directory services.</span></span> <span data-ttu-id="a06d3-128">Denna utvärdering är viktigt att definiera de tekniska kraven för att integrera dessa program till molnet.</span><span class="sxs-lookup"><span data-stu-id="a06d3-128">This evaluation is important to define the technical requirements to integrate these applications to the cloud.</span></span> <span data-ttu-id="a06d3-129">Se till att besvara följande frågor:</span><span class="sxs-lookup"><span data-stu-id="a06d3-129">Make sure to answer the following questions:</span></span>

* <span data-ttu-id="a06d3-130">Dessa program flyttas till molnet och använda katalogen?</span><span class="sxs-lookup"><span data-stu-id="a06d3-130">Will these applications be moved to the cloud and use the directory?</span></span>
* <span data-ttu-id="a06d3-131">Finns det särskilda attribut som måste synkroniseras till molnet så att programmen kan använda dem korrekt?</span><span class="sxs-lookup"><span data-stu-id="a06d3-131">Are there special attributes that need to be synchronized to the cloud so these applications can use them successfully?</span></span>
* <span data-ttu-id="a06d3-132">Dessa program måste skrivas igen för att utnyttja fördelarna med molnet auth?</span><span class="sxs-lookup"><span data-stu-id="a06d3-132">Will these applications need to be re-written to take advantage of cloud auth?</span></span>
* <span data-ttu-id="a06d3-133">Fortsätter programmen live lokalt medan användare komma åt dem med hjälp av molnidentiteten?</span><span class="sxs-lookup"><span data-stu-id="a06d3-133">Will these applications continue to live on-premises while users access them using the cloud identity?</span></span>

<span data-ttu-id="a06d3-134">Du måste också bestämma säkerhet kraven och begränsningarna katalogsynkronisering.</span><span class="sxs-lookup"><span data-stu-id="a06d3-134">You also need to determine the security requirements and constraints directory synchronization.</span></span> <span data-ttu-id="a06d3-135">Denna utvärdering är viktigt att hämta en lista över de krav som behövs för att skapa och underhålla användaridentiteter i molnet.</span><span class="sxs-lookup"><span data-stu-id="a06d3-135">This evaluation is important to get a list of the requirements that will be needed in order to create and maintain user’s identities in the cloud.</span></span> <span data-ttu-id="a06d3-136">Se till att besvara följande frågor:</span><span class="sxs-lookup"><span data-stu-id="a06d3-136">Make sure to answer the following questions:</span></span>

* <span data-ttu-id="a06d3-137">Där kommer att hitta synkroniseringsservern?</span><span class="sxs-lookup"><span data-stu-id="a06d3-137">Where will the synchronization server be located?</span></span>
* <span data-ttu-id="a06d3-138">Kommer det att vara ansluten till en domän?</span><span class="sxs-lookup"><span data-stu-id="a06d3-138">Will it be domain joined?</span></span>
* <span data-ttu-id="a06d3-139">Servern finns på ett begränsat nätverk bakom en brandvägg, till exempel en DMZ?</span><span class="sxs-lookup"><span data-stu-id="a06d3-139">Will the server be located on a restricted network behind a firewall, such as a DMZ?</span></span>
  * <span data-ttu-id="a06d3-140">Kommer du att kunna öppna de nödvändiga Brandväggsportarna att stödja synkronisering?</span><span class="sxs-lookup"><span data-stu-id="a06d3-140">Will you be able to open the required firewall ports to support synchronization?</span></span>
* <span data-ttu-id="a06d3-141">Har du en återställningsplan för av synkroniseringsservern?</span><span class="sxs-lookup"><span data-stu-id="a06d3-141">Do you have a disaster recovery plan for the synchronization server?</span></span>
* <span data-ttu-id="a06d3-142">Har du ett konto med rätt behörigheter för alla skogar som du vill synkronisera med?</span><span class="sxs-lookup"><span data-stu-id="a06d3-142">Do you have an account with the correct permissions for all forests you want to synch with?</span></span>
  * <span data-ttu-id="a06d3-143">Om företaget inte känner till svaret på den här frågan, läser du avsnittet ”behörigheter för Lösenordssynkronisering” i artikeln [installera Azure Active Directory Sync Service](https://msdn.microsoft.com/library/azure/dn757602.aspx#BKMK_CreateAnADAccountForTheSyncService) och ta reda på om du redan har ett konto med dessa behörigheter eller om du behöver skapa en.</span><span class="sxs-lookup"><span data-stu-id="a06d3-143">If your company doesn’t know the answer for this question, review the section “Permissions for password synchronization” in the article [Install the Azure Active Directory Sync Service](https://msdn.microsoft.com/library/azure/dn757602.aspx#BKMK_CreateAnADAccountForTheSyncService) and determine if you already have an account with these permissions or if you need to create one.</span></span>
* <span data-ttu-id="a06d3-144">Om du har mutli-skog synkronisering kan synkroniseringsservern för varje skog?</span><span class="sxs-lookup"><span data-stu-id="a06d3-144">If you have mutli-forest sync is the sync server able to get to each forest?</span></span>

> [!NOTE]
> <span data-ttu-id="a06d3-145">Se till att varje svar och försök förstå anledningen till svaret.</span><span class="sxs-lookup"><span data-stu-id="a06d3-145">Make sure to take notes of each answer and understand the rationale behind the answer.</span></span> <span data-ttu-id="a06d3-146">[Fastställa kraven på incidentsvar](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) kommer att överskrida de tillgängliga alternativen.</span><span class="sxs-lookup"><span data-stu-id="a06d3-146">[Determine incident response requirements](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) will go over the options available.</span></span> <span data-ttu-id="a06d3-147">Har besvarat frågorna väljer du vilket alternativ som bäst passar dina behöver.</span><span class="sxs-lookup"><span data-stu-id="a06d3-147">By having answered those questions you will select which option best suits your business needs.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="a06d3-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a06d3-148">Next steps</span></span>
[<span data-ttu-id="a06d3-149">Ange krav för multifaktorautentisering</span><span class="sxs-lookup"><span data-stu-id="a06d3-149">Determine multi-factor authentication requirements</span></span>](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="see-also"></a><span data-ttu-id="a06d3-150">Se även</span><span class="sxs-lookup"><span data-stu-id="a06d3-150">See also</span></span>
[<span data-ttu-id="a06d3-151">Översikt över design-överväganden</span><span class="sxs-lookup"><span data-stu-id="a06d3-151">Design considerations overview</span></span>](active-directory-hybrid-identity-design-considerations-overview.md)


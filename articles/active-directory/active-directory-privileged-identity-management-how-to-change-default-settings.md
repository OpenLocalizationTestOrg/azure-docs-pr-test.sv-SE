---
title: "aaaHow toomanage produktaktivering rollinställningar | Microsoft Docs"
description: "Lär dig hur toochange hello standardinställningarna för privilegierade identiteter med hello Azure Active Directory Privileged Identity Management – tillägget."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: f6cbcb6a-8a89-4077-afd8-06c94a64f4aa
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 453bb6f8f8e0fd2598cb073ef86c1dd55458dc72
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-role-activation-settings-in-azure-ad-privileged-identity-management"></a><span data-ttu-id="e2409-103">Hur toomanage produktaktivering rollinställningar i Azure AD Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="e2409-103">How toomanage role activation settings in Azure AD Privileged Identity Management</span></span>
<span data-ttu-id="e2409-104">En administratör av Privilegierade roller kan anpassa Azure AD Privileged Identity Management (PIM) i organisationen, inklusive ändra hello upplevelse för användare som aktivera en berättigad rolltilldelning.</span><span class="sxs-lookup"><span data-stu-id="e2409-104">A privileged role administrator can customize Azure AD Privileged Identity Management (PIM) in their organization, including changing hello experience for a user who is activating an eligible role assignment.</span></span>

## <a name="manage-hello-role-activation-settings"></a><span data-ttu-id="e2409-105">Hantera hello produktaktivering rollinställningar</span><span class="sxs-lookup"><span data-stu-id="e2409-105">Manage hello role activation settings</span></span>
1. <span data-ttu-id="e2409-106">Gå toohello [Azure-portalen](https://portal.azure.com) och välj hello **Azure AD Privileged Identity Management** app hello instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="e2409-106">Go toohello [Azure portal](https://portal.azure.com) and select hello **Azure AD Privileged Identity Management** app from hello dashboard.</span></span>
2. <span data-ttu-id="e2409-107">Välj **hantera Privilegierade roller** > **inställningar** > **Privilegierade roller**.</span><span class="sxs-lookup"><span data-stu-id="e2409-107">Select **Manage privileged roles** > **Settings** > **Privileged Roles**.</span></span>
3. <span data-ttu-id="e2409-108">Välj hello roll vars inställningar du vill toomanage.</span><span class="sxs-lookup"><span data-stu-id="e2409-108">Choose hello role whose settings you want toomanage.</span></span>

<span data-ttu-id="e2409-109">Det finns ett antal inställningar som du kan konfigurera hello på sidan Inställningar för varje roll.</span><span class="sxs-lookup"><span data-stu-id="e2409-109">On hello settings page for each role, there are a number of settings you can configure.</span></span> <span data-ttu-id="e2409-110">Dessa inställningar påverkar endast användare som är berättigade administratörer, inte permanenta administratörer.</span><span class="sxs-lookup"><span data-stu-id="e2409-110">These settings only affect users who are eligible admins, not permanent admins.</span></span>

<span data-ttu-id="e2409-111">**Aktiveringar**: hello tiden, i timmar, som en roll förblir aktiv innan den upphör.</span><span class="sxs-lookup"><span data-stu-id="e2409-111">**Activations**: hello time, in hours, that a role stays active before it expires.</span></span> <span data-ttu-id="e2409-112">Detta kan vara mellan 1 och 72 timmar.</span><span class="sxs-lookup"><span data-stu-id="e2409-112">This can be between 1 and 72 hours.</span></span>

<span data-ttu-id="e2409-113">**Meddelanden**: du kan välja huruvida hello skickas e-postmeddelanden tooadmins bekräftar att de har aktiverat en roll.</span><span class="sxs-lookup"><span data-stu-id="e2409-113">**Notifications**: You can choose whether or not hello system sends emails tooadmins confirming that they have activated a role.</span></span> <span data-ttu-id="e2409-114">Detta kan vara användbart för att upptäcka otillåten eller otillåtna aktiveringar.</span><span class="sxs-lookup"><span data-stu-id="e2409-114">This can be useful for detecting unauthorized or illegitimate activations.</span></span>

<span data-ttu-id="e2409-115">**Incident/begäran biljett**: du kan välja huruvida toorequire berättigade administratörer tooinclude en biljett number när de aktiverar rollen.</span><span class="sxs-lookup"><span data-stu-id="e2409-115">**Incident/Request Ticket**: You can choose whether or not toorequire eligible admins tooinclude a ticket number when they activate their role.</span></span> <span data-ttu-id="e2409-116">Detta kan vara användbart när du utför roll åtkomst granskningar.</span><span class="sxs-lookup"><span data-stu-id="e2409-116">This can be useful when you perform role access audits.</span></span>

<span data-ttu-id="e2409-117">**Multifaktorautentisering**: du kan välja huruvida toorequire användare tooverify sin identitet med MFA innan de kan aktivera deras roller.</span><span class="sxs-lookup"><span data-stu-id="e2409-117">**Multi-Factor Authentication**: You can choose whether or not toorequire users tooverify their identity with MFA before they can activate their roles.</span></span> <span data-ttu-id="e2409-118">De har bara tooverify detta en gång per session inte varje gång de aktivera en roll.</span><span class="sxs-lookup"><span data-stu-id="e2409-118">They only have tooverify this once per session, not every time they activate a role.</span></span> <span data-ttu-id="e2409-119">Det finns två tips tookeep i åtanke när du aktiverar MFA:</span><span class="sxs-lookup"><span data-stu-id="e2409-119">There are two tips tookeep in mind when you enable MFA:</span></span>

* <span data-ttu-id="e2409-120">Användare som har Microsoft-konton för sina e-postadresser (vanligtvis @outlook.com, men inte alltid) kan inte registrera dig för Azure MFA.</span><span class="sxs-lookup"><span data-stu-id="e2409-120">Users who have Microsoft accounts for their email addresses (typically @outlook.com, but not always) cannot register for Azure MFA.</span></span> <span data-ttu-id="e2409-121">Om du vill tooassign roller toousers med Microsoft-konton, bör du göra dem permanenta administratörer eller inaktivera MFA för rollen.</span><span class="sxs-lookup"><span data-stu-id="e2409-121">If you want tooassign roles toousers with Microsoft accounts, you should either make them permanent admins or disable MFA for that role.</span></span>
* <span data-ttu-id="e2409-122">Du kan inte inaktivera MFA för mycket Privilegierade roller för Azure AD och Office 365.</span><span class="sxs-lookup"><span data-stu-id="e2409-122">You cannot disable MFA for highly privileged roles for Azure AD and Office365.</span></span> <span data-ttu-id="e2409-123">Det här är en säkerhetsfunktion eftersom dessa roller ska skyddas noggrant:</span><span class="sxs-lookup"><span data-stu-id="e2409-123">This is a safety feature because these roles should be carefully protected:</span></span>  
  
  * <span data-ttu-id="e2409-124">Programadministratör</span><span class="sxs-lookup"><span data-stu-id="e2409-124">Application administrator</span></span>
  * <span data-ttu-id="e2409-125">Programmet proxyserverns administratör</span><span class="sxs-lookup"><span data-stu-id="e2409-125">Application Proxy server administrator</span></span>
  * <span data-ttu-id="e2409-126">Faktureringsadministratör</span><span class="sxs-lookup"><span data-stu-id="e2409-126">Billing administrator</span></span>  
  * <span data-ttu-id="e2409-127">Kompatibilitet administratör</span><span class="sxs-lookup"><span data-stu-id="e2409-127">Compliance administrator</span></span>  
  * <span data-ttu-id="e2409-128">CRM-tjänstadministratör</span><span class="sxs-lookup"><span data-stu-id="e2409-128">CRM service administrator</span></span>
  * <span data-ttu-id="e2409-129">Kunden LockBox åtkomst godkännare</span><span class="sxs-lookup"><span data-stu-id="e2409-129">Customer LockBox access approver</span></span>
  * <span data-ttu-id="e2409-130">Directory-skrivare</span><span class="sxs-lookup"><span data-stu-id="e2409-130">Directory writer</span></span>  
  * <span data-ttu-id="e2409-131">Exchange-administratören</span><span class="sxs-lookup"><span data-stu-id="e2409-131">Exchange administrator</span></span>  
  * <span data-ttu-id="e2409-132">Global administratör</span><span class="sxs-lookup"><span data-stu-id="e2409-132">Global administrator</span></span>
  * <span data-ttu-id="e2409-133">Intune-tjänstadministratören</span><span class="sxs-lookup"><span data-stu-id="e2409-133">Intune service administrator</span></span>
  * <span data-ttu-id="e2409-134">Administratör för postlåda</span><span class="sxs-lookup"><span data-stu-id="e2409-134">Mailbox administrator</span></span>  
  * <span data-ttu-id="e2409-135">Support för partner tier1</span><span class="sxs-lookup"><span data-stu-id="e2409-135">Partner tier1 support</span></span>  
  * <span data-ttu-id="e2409-136">Support för partner tier2</span><span class="sxs-lookup"><span data-stu-id="e2409-136">Partner tier2 support</span></span>  
  * <span data-ttu-id="e2409-137">Administratör av Privilegierade roller</span><span class="sxs-lookup"><span data-stu-id="e2409-137">Privileged role administrator</span></span>   
  * <span data-ttu-id="e2409-138">Säkerhetsadministratör</span><span class="sxs-lookup"><span data-stu-id="e2409-138">Security administrator</span></span>  
  * <span data-ttu-id="e2409-139">SharePoint-administratör</span><span class="sxs-lookup"><span data-stu-id="e2409-139">SharePoint administrator</span></span>  
  * <span data-ttu-id="e2409-140">Skype for Business-administratör</span><span class="sxs-lookup"><span data-stu-id="e2409-140">Skype for Business administrator</span></span>  
  * <span data-ttu-id="e2409-141">Kontoadministratör för användaren</span><span class="sxs-lookup"><span data-stu-id="e2409-141">User account administrator</span></span>  

<span data-ttu-id="e2409-142">Läs mer om hur du använder MFA med PIM [hur tooRequire MFA](active-directory-privileged-identity-management-how-to-require-mfa.md).</span><span class="sxs-lookup"><span data-stu-id="e2409-142">For more information about using MFA with PIM see [How tooRequire MFA](active-directory-privileged-identity-management-how-to-require-mfa.md).</span></span>

<!--PLACEHOLDER: Need an explanation of what hello temporary Global Administrator setting is for.-->

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="e2409-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e2409-143">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]


---
title: "Så här hanterar du produktaktivering rollinställningar | Microsoft Docs"
description: "Lär dig hur du ändrar standardinställningarna för privilegierade identiteter med Azure Active Directory Privileged Identity Management-tillägget."
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
ms.openlocfilehash: 23605e89cd1846d2e06e48cb5d3e0191cb9e9b4a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-manage-role-activation-settings-in-azure-ad-privileged-identity-management"></a><span data-ttu-id="3bfc2-103">Så här hanterar du inställningar för aktivering av rollen i Azure AD Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="3bfc2-103">How to manage role activation settings in Azure AD Privileged Identity Management</span></span>
<span data-ttu-id="3bfc2-104">En administratör av Privilegierade roller kan anpassa Azure AD Privileged Identity Management (PIM) i organisationen, inklusive ändra upplevelsen för en användare som aktivera en berättigad rolltilldelning.</span><span class="sxs-lookup"><span data-stu-id="3bfc2-104">A privileged role administrator can customize Azure AD Privileged Identity Management (PIM) in their organization, including changing the experience for a user who is activating an eligible role assignment.</span></span>

## <a name="manage-the-role-activation-settings"></a><span data-ttu-id="3bfc2-105">Hantera produktaktivering rollinställningar</span><span class="sxs-lookup"><span data-stu-id="3bfc2-105">Manage the role activation settings</span></span>
1. <span data-ttu-id="3bfc2-106">Gå till den [Azure-portalen](https://portal.azure.com) och välj den **Azure AD Privileged Identity Management** app från instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="3bfc2-106">Go to the [Azure portal](https://portal.azure.com) and select the **Azure AD Privileged Identity Management** app from the dashboard.</span></span>
2. <span data-ttu-id="3bfc2-107">Välj **hantera Privilegierade roller** > **inställningar** > **Privilegierade roller**.</span><span class="sxs-lookup"><span data-stu-id="3bfc2-107">Select **Manage privileged roles** > **Settings** > **Privileged Roles**.</span></span>
3. <span data-ttu-id="3bfc2-108">Välj rollen vars inställningar du vill hantera.</span><span class="sxs-lookup"><span data-stu-id="3bfc2-108">Choose the role whose settings you want to manage.</span></span>

<span data-ttu-id="3bfc2-109">Det finns ett antal inställningar som du kan konfigurera på inställningssidan för varje roll.</span><span class="sxs-lookup"><span data-stu-id="3bfc2-109">On the settings page for each role, there are a number of settings you can configure.</span></span> <span data-ttu-id="3bfc2-110">Dessa inställningar påverkar endast användare som är berättigade administratörer, inte permanenta administratörer.</span><span class="sxs-lookup"><span data-stu-id="3bfc2-110">These settings only affect users who are eligible admins, not permanent admins.</span></span>

<span data-ttu-id="3bfc2-111">**Aktiveringar**: tid, i timmar, som en roll förblir aktiv innan den upphör.</span><span class="sxs-lookup"><span data-stu-id="3bfc2-111">**Activations**: The time, in hours, that a role stays active before it expires.</span></span> <span data-ttu-id="3bfc2-112">Detta kan vara mellan 1 och 72 timmar.</span><span class="sxs-lookup"><span data-stu-id="3bfc2-112">This can be between 1 and 72 hours.</span></span>

<span data-ttu-id="3bfc2-113">**Meddelanden**: du kan välja huruvida systemet skickar e-post till administratörer bekräftar att de har aktiverat en roll.</span><span class="sxs-lookup"><span data-stu-id="3bfc2-113">**Notifications**: You can choose whether or not the system sends emails to admins confirming that they have activated a role.</span></span> <span data-ttu-id="3bfc2-114">Detta kan vara användbart för att upptäcka otillåten eller otillåtna aktiveringar.</span><span class="sxs-lookup"><span data-stu-id="3bfc2-114">This can be useful for detecting unauthorized or illegitimate activations.</span></span>

<span data-ttu-id="3bfc2-115">**Incident/begäran biljett**: du kan välja om du vill kräver berättigade administratörer att inkludera en Biljettnummer när de aktiverar rollen.</span><span class="sxs-lookup"><span data-stu-id="3bfc2-115">**Incident/Request Ticket**: You can choose whether or not to require eligible admins to include a ticket number when they activate their role.</span></span> <span data-ttu-id="3bfc2-116">Detta kan vara användbart när du utför roll åtkomst granskningar.</span><span class="sxs-lookup"><span data-stu-id="3bfc2-116">This can be useful when you perform role access audits.</span></span>

<span data-ttu-id="3bfc2-117">**Multifaktorautentisering**: du kan välja om du vill att användare ska verifiera sin identitet med MFA innan de kan aktivera sina roller eller inte.</span><span class="sxs-lookup"><span data-stu-id="3bfc2-117">**Multi-Factor Authentication**: You can choose whether or not to require users to verify their identity with MFA before they can activate their roles.</span></span> <span data-ttu-id="3bfc2-118">De behöver bara kontrollera detta en gång per session inte varje gång de aktivera en roll.</span><span class="sxs-lookup"><span data-stu-id="3bfc2-118">They only have to verify this once per session, not every time they activate a role.</span></span> <span data-ttu-id="3bfc2-119">Det finns två tips att tänka på när du aktiverar MFA:</span><span class="sxs-lookup"><span data-stu-id="3bfc2-119">There are two tips to keep in mind when you enable MFA:</span></span>

* <span data-ttu-id="3bfc2-120">Användare som har Microsoft-konton för sina e-postadresser (vanligtvis @outlook.com, men inte alltid) kan inte registrera dig för Azure MFA.</span><span class="sxs-lookup"><span data-stu-id="3bfc2-120">Users who have Microsoft accounts for their email addresses (typically @outlook.com, but not always) cannot register for Azure MFA.</span></span> <span data-ttu-id="3bfc2-121">Om du vill tilldela roller till användare med Microsoft-konton, bör du göra dem permanenta administratörer eller inaktivera MFA för rollen.</span><span class="sxs-lookup"><span data-stu-id="3bfc2-121">If you want to assign roles to users with Microsoft accounts, you should either make them permanent admins or disable MFA for that role.</span></span>
* <span data-ttu-id="3bfc2-122">Du kan inte inaktivera MFA för mycket Privilegierade roller för Azure AD och Office 365.</span><span class="sxs-lookup"><span data-stu-id="3bfc2-122">You cannot disable MFA for highly privileged roles for Azure AD and Office365.</span></span> <span data-ttu-id="3bfc2-123">Det här är en säkerhetsfunktion eftersom dessa roller ska skyddas noggrant:</span><span class="sxs-lookup"><span data-stu-id="3bfc2-123">This is a safety feature because these roles should be carefully protected:</span></span>  
  
  * <span data-ttu-id="3bfc2-124">Programadministratör</span><span class="sxs-lookup"><span data-stu-id="3bfc2-124">Application administrator</span></span>
  * <span data-ttu-id="3bfc2-125">Programmet proxyserverns administratör</span><span class="sxs-lookup"><span data-stu-id="3bfc2-125">Application Proxy server administrator</span></span>
  * <span data-ttu-id="3bfc2-126">Faktureringsadministratör</span><span class="sxs-lookup"><span data-stu-id="3bfc2-126">Billing administrator</span></span>  
  * <span data-ttu-id="3bfc2-127">Kompatibilitet administratör</span><span class="sxs-lookup"><span data-stu-id="3bfc2-127">Compliance administrator</span></span>  
  * <span data-ttu-id="3bfc2-128">CRM-tjänstadministratör</span><span class="sxs-lookup"><span data-stu-id="3bfc2-128">CRM service administrator</span></span>
  * <span data-ttu-id="3bfc2-129">Kunden LockBox åtkomst godkännare</span><span class="sxs-lookup"><span data-stu-id="3bfc2-129">Customer LockBox access approver</span></span>
  * <span data-ttu-id="3bfc2-130">Directory-skrivare</span><span class="sxs-lookup"><span data-stu-id="3bfc2-130">Directory writer</span></span>  
  * <span data-ttu-id="3bfc2-131">Exchange-administratören</span><span class="sxs-lookup"><span data-stu-id="3bfc2-131">Exchange administrator</span></span>  
  * <span data-ttu-id="3bfc2-132">Global administratör</span><span class="sxs-lookup"><span data-stu-id="3bfc2-132">Global administrator</span></span>
  * <span data-ttu-id="3bfc2-133">Intune-tjänstadministratören</span><span class="sxs-lookup"><span data-stu-id="3bfc2-133">Intune service administrator</span></span>
  * <span data-ttu-id="3bfc2-134">Administratör för postlåda</span><span class="sxs-lookup"><span data-stu-id="3bfc2-134">Mailbox administrator</span></span>  
  * <span data-ttu-id="3bfc2-135">Support för partner tier1</span><span class="sxs-lookup"><span data-stu-id="3bfc2-135">Partner tier1 support</span></span>  
  * <span data-ttu-id="3bfc2-136">Support för partner tier2</span><span class="sxs-lookup"><span data-stu-id="3bfc2-136">Partner tier2 support</span></span>  
  * <span data-ttu-id="3bfc2-137">Administratör av Privilegierade roller</span><span class="sxs-lookup"><span data-stu-id="3bfc2-137">Privileged role administrator</span></span>   
  * <span data-ttu-id="3bfc2-138">Säkerhetsadministratör</span><span class="sxs-lookup"><span data-stu-id="3bfc2-138">Security administrator</span></span>  
  * <span data-ttu-id="3bfc2-139">SharePoint-administratör</span><span class="sxs-lookup"><span data-stu-id="3bfc2-139">SharePoint administrator</span></span>  
  * <span data-ttu-id="3bfc2-140">Skype for Business-administratör</span><span class="sxs-lookup"><span data-stu-id="3bfc2-140">Skype for Business administrator</span></span>  
  * <span data-ttu-id="3bfc2-141">Kontoadministratör för användaren</span><span class="sxs-lookup"><span data-stu-id="3bfc2-141">User account administrator</span></span>  

<span data-ttu-id="3bfc2-142">Läs mer om hur du använder MFA med PIM [så kräver MFA](active-directory-privileged-identity-management-how-to-require-mfa.md).</span><span class="sxs-lookup"><span data-stu-id="3bfc2-142">For more information about using MFA with PIM see [How to Require MFA](active-directory-privileged-identity-management-how-to-require-mfa.md).</span></span>

<!--PLACEHOLDER: Need an explanation of what the temporary Global Administrator setting is for.-->

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="3bfc2-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3bfc2-143">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]


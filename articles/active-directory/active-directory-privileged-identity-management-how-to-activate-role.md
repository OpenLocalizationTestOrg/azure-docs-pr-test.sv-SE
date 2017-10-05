---
title: "Så här aktiverar eller inaktiverar du en roll | Microsoft Docs"
description: "Lär dig mer om att aktivera roller för privilegierade identiteter med Azure Privileged Identity Management-programmet."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 1ce9e2e7-452b-4f66-9588-0d9cd2539e45
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/14/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: a143fd78eae52fda0cbadb7e74fd8209f24629a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-activate-or-deactivate-roles-in-azure-ad-privileged-identity-management"></a><span data-ttu-id="e92a6-103">Så här aktiverar eller inaktiverar roller i Azure AD Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="e92a6-103">How to activate or deactivate roles in Azure AD Privileged Identity Management</span></span>
<span data-ttu-id="e92a6-104">Azure Active Directory (AD) Privileged Identity Management förenklar hur företag hantera privilegierad åtkomst till resurser i Azure AD och andra Microsoft online services som Office 365 eller Microsoft Intune.</span><span class="sxs-lookup"><span data-stu-id="e92a6-104">Azure Active Directory (AD) Privileged Identity Management simplifies how enterprises manage privileged access to resources in Azure AD and other Microsoft online services like Office 365 or Microsoft Intune.</span></span>  

<span data-ttu-id="e92a6-105">Om du har gjorts tillgängliga för en administrativ roll som innebär att du kan aktivera rollen när du behöver utföra Privilegierade åtgärder.</span><span class="sxs-lookup"><span data-stu-id="e92a6-105">If you have been made eligible for an administrative role, that means you can activate that role when you need to perform privileged actions.</span></span> <span data-ttu-id="e92a6-106">Till exempel om du hanterar ibland Office 365-funktioner kan din organisation Privilegierade roller administratörer kan inte göra du permanent Global administratör, eftersom rollen påverkar andra tjänster för.</span><span class="sxs-lookup"><span data-stu-id="e92a6-106">For example, if you occasionally manage Office 365 features, your organization's privileged role administrators may not make you a permanent Global Administrator, since that role impacts other services, too.</span></span> <span data-ttu-id="e92a6-107">I stället gör de du berättigad till Azure AD-roller såsom Exchange Online-administratör.</span><span class="sxs-lookup"><span data-stu-id="e92a6-107">Instead, they make you eligible for Azure AD roles such as Exchange Online Administrator.</span></span> <span data-ttu-id="e92a6-108">Du kan begära för att aktivera rollen när du behöver ha dess behörighet, och du har admin-kontroll för en förinställd tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="e92a6-108">You can request to activate that role when you need its privileges, and then you'll have admin control for a predetermined time period.</span></span>

<span data-ttu-id="e92a6-109">Den här artikeln är administratörer som behöver aktivera deras roll i Azure AD Privileged Identity Management (PIM).</span><span class="sxs-lookup"><span data-stu-id="e92a6-109">This article is for admins who need to activate their role in Azure AD Privileged Identity Management (PIM).</span></span> <span data-ttu-id="e92a6-110">Den vägleder dig genom stegen för att aktivera en roll om du behöver behörigheter och inaktivera rollen när du är klar.</span><span class="sxs-lookup"><span data-stu-id="e92a6-110">It walks you through the steps to activate a role when you need the permissions, and deactivate the role when you're done.</span></span> <span data-ttu-id="e92a6-111">Privilegierade rollen administratörer kan dessutom kräva godkännande för att aktivera en roll (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="e92a6-111">In addition, privileged role administrators can require approval to activate a role (Preview).</span></span> <span data-ttu-id="e92a6-112">Lär dig mer om [PIM godkännandearbetsflöden](./privileged-identity-management/azure-ad-pim-approval-workflow.md) här.</span><span class="sxs-lookup"><span data-stu-id="e92a6-112">Learn more about [PIM Approval Workflows](./privileged-identity-management/azure-ad-pim-approval-workflow.md) here.</span></span>

## <a name="add-the-privileged-identity-management-application"></a><span data-ttu-id="e92a6-113">Lägga till programmet Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="e92a6-113">Add the Privileged Identity Management application</span></span>
<span data-ttu-id="e92a6-114">Använda Azure AD Privileged Identity Management-programmet i den [Azure-portalen](https://portal.azure.com/) begära rollaktivering, även om du ska köras i en annan portal eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e92a6-114">Use the Azure AD Privileged Identity Management application in the [Azure portal](https://portal.azure.com/) to request a role activation, even if you're going to operate in another portal or PowerShell.</span></span> <span data-ttu-id="e92a6-115">Om du inte har programmet Azure AD Privileged Identity Management på Azure-portalen, följer du dessa steg för att komma igång.</span><span class="sxs-lookup"><span data-stu-id="e92a6-115">If you don't have the Azure AD Privileged Identity Management application on your Azure portal, follow these steps to get started.</span></span>

1. <span data-ttu-id="e92a6-116">Logga in på [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="e92a6-116">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="e92a6-117">Välj ditt användarnamn i det övre högra hörnet i Azure-portalen och välj den katalog där du kommer du att driva.</span><span class="sxs-lookup"><span data-stu-id="e92a6-117">Select your username in the upper right-hand corner of the Azure portal, and select the directory where you will you be operating.</span></span>
3. <span data-ttu-id="e92a6-118">Välj **Fler tjänster** och använd textrutan Filter för att söka efter **Azure AD Privileged Identity Management**.</span><span class="sxs-lookup"><span data-stu-id="e92a6-118">Select **More services** and use the Filter textbox to search for **Azure AD Privileged Identity Management**.</span></span>
4. <span data-ttu-id="e92a6-119">Markera **Fäst på instrumentpanelen** och klicka sedan på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="e92a6-119">Check **Pin to dashboard** and then click **Create**.</span></span> <span data-ttu-id="e92a6-120">Privileged Identity Management-programmet öppnas.</span><span class="sxs-lookup"><span data-stu-id="e92a6-120">The Privileged Identity Management application opens.</span></span>

## <a name="activate-a-role"></a><span data-ttu-id="e92a6-121">Aktivera en roll</span><span class="sxs-lookup"><span data-stu-id="e92a6-121">Activate a role</span></span>
<span data-ttu-id="e92a6-122">När du behöver ta på en roll kan du begära aktivering genom att välja den **Mina roller** navigeringsalternativet i Azure AD Privileged Identity Management-programmet vänstra navigeringsfönstret kolumn.</span><span class="sxs-lookup"><span data-stu-id="e92a6-122">When you need to take on a role, you can request activation by selecting the **My Roles** navigation option in the Azure AD Privileged Identity Management application's left navigation column.</span></span>

1. <span data-ttu-id="e92a6-123">Logga in på den [Azure-portalen](https://portal.azure.com/) och välj panelen Azure AD Privileged Identity Management.</span><span class="sxs-lookup"><span data-stu-id="e92a6-123">Sign in to the [Azure portal](https://portal.azure.com/) and select the Azure AD Privileged Identity Management tile.</span></span>
2. <span data-ttu-id="e92a6-124">Välj **min roller**.</span><span class="sxs-lookup"><span data-stu-id="e92a6-124">Select **My Roles**.</span></span> <span data-ttu-id="e92a6-125">En lista över tilldelade berättigade rollerna visas i grupperingen överst på sidan.</span><span class="sxs-lookup"><span data-stu-id="e92a6-125">A list of your assigned eligible roles appear in the grouping at the top of the page.</span></span>
3. <span data-ttu-id="e92a6-126">Välj en roll som ska aktiveras.</span><span class="sxs-lookup"><span data-stu-id="e92a6-126">Select a role to activate.</span></span>
4. <span data-ttu-id="e92a6-127">Välj **aktivera**.</span><span class="sxs-lookup"><span data-stu-id="e92a6-127">Select **Activate**.</span></span> <span data-ttu-id="e92a6-128">Den **begär rollaktivering** bladet visas.</span><span class="sxs-lookup"><span data-stu-id="e92a6-128">The **Request role activation** blade appears.</span></span>
5. <span data-ttu-id="e92a6-129">Vissa roller kräver Multi-Factor Authentication (MFA) innan du kan aktivera rollen.</span><span class="sxs-lookup"><span data-stu-id="e92a6-129">Some roles require Multi-Factor Authentication (MFA) before you can activate the role.</span></span> <span data-ttu-id="e92a6-130">Du behöver bara autentisera en gång per session.</span><span class="sxs-lookup"><span data-stu-id="e92a6-130">You only have to authenticate once per session.</span></span>
   
    ![Kontrollera med MFA innan rollaktivering – skärmbild][2]
6. <span data-ttu-id="e92a6-132">Ange orsaken till aktiveringsbegäran i textfältet.</span><span class="sxs-lookup"><span data-stu-id="e92a6-132">Enter the reason for the activation request in the text field.</span></span>  <span data-ttu-id="e92a6-133">Vissa roller kräver att du kan ange en Biljettnummer problem.</span><span class="sxs-lookup"><span data-stu-id="e92a6-133">Some roles require you to supply a trouble ticket number.</span></span>
7. <span data-ttu-id="e92a6-134">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="e92a6-134">Select **OK**.</span></span>  <span data-ttu-id="e92a6-135">Om rollen inte kräver godkännande har nu aktiverats och rollen visas i listan över aktiva roller (direkt under listan med tillgängliga rolltilldelningar).</span><span class="sxs-lookup"><span data-stu-id="e92a6-135">If the role does not require approval, it is now activated, and the role appears in the list of active roles (directly below the list of eligible role assignments).</span></span> <span data-ttu-id="e92a6-136">Om den [rollen kräver godkännande](./privileged-identity-management/azure-ad-pim-approval-workflow.md) ett popup-meddelande visas om du vill aktivera, kortfattat i det övre högra hörnet i webbläsaren om begäran väntar på godkännande.</span><span class="sxs-lookup"><span data-stu-id="e92a6-136">If the [role requires approval](./privileged-identity-management/azure-ad-pim-approval-workflow.md) to activate, a toast notification will briefly appear in the upper right-hand corner of your browser informing you the request is pending approval.</span></span>

    ![Begära väntande meddelanden – skärmbild][3]

## <a name="deactivate-a-role"></a><span data-ttu-id="e92a6-138">Inaktivera en roll</span><span class="sxs-lookup"><span data-stu-id="e92a6-138">Deactivate a role</span></span>
<span data-ttu-id="e92a6-139">När en roll har aktiverats kan inaktiveras automatiskt när tidsgränsen (berättigade varaktighet) har uppnåtts.</span><span class="sxs-lookup"><span data-stu-id="e92a6-139">Once a role has been activated, it automatically deactivates when its time limit (eligible duration) is reached.</span></span>

<span data-ttu-id="e92a6-140">Om du har slutfört dina administratörsuppgifter tidigt, kan du inaktivera en roll manuellt i Azure AD Privileged Identity Management-programmet.</span><span class="sxs-lookup"><span data-stu-id="e92a6-140">If you complete your admin tasks early, you can also deactivate a role manually in the Azure AD Privileged Identity Management application.</span></span>  <span data-ttu-id="e92a6-141">Välj **Mina roller**, väljer du vilken roll som du är klar med från den **Active rolltilldelningar** gruppering och välj **inaktivera**.</span><span class="sxs-lookup"><span data-stu-id="e92a6-141">Select **My Roles**, choose the role you're done using from the **Active role assignments** grouping, and select **Deactivate**.</span></span>  

## <a name="cancel-a-pending-request"></a><span data-ttu-id="e92a6-142">Avbryta en väntande begäran</span><span class="sxs-lookup"><span data-stu-id="e92a6-142">Cancel a pending request</span></span>
<span data-ttu-id="e92a6-143">Om du inte behöver aktivering av en roll som kräver godkännande, kan du avbryta en väntande begäran när som helst.</span><span class="sxs-lookup"><span data-stu-id="e92a6-143">In the event you do not require activation of a role that requires approval, you may cancel a pending request at any time.</span></span> <span data-ttu-id="e92a6-144">Bara väljer den **Mina roller** navigeringsalternativet i Azure AD Privileged Identity Management-programmet vänstra navigeringsfönstret kolumn.</span><span class="sxs-lookup"><span data-stu-id="e92a6-144">Simply select the **My Roles** navigation option in the Azure AD Privileged Identity Management application's left navigation column.</span></span>

1. <span data-ttu-id="e92a6-145">Logga in på den [Azure-portalen](https://portal.azure.com/) och välj panelen Azure AD Privileged Identity Management.</span><span class="sxs-lookup"><span data-stu-id="e92a6-145">Sign in to the [Azure portal](https://portal.azure.com/) and select the Azure AD Privileged Identity Management tile.</span></span>
2. <span data-ttu-id="e92a6-146">Välj **min roller**.</span><span class="sxs-lookup"><span data-stu-id="e92a6-146">Select **My Roles**.</span></span> <span data-ttu-id="e92a6-147">En lista över tilldelade berättigade rollerna visas i grupperingen överst på sidan.</span><span class="sxs-lookup"><span data-stu-id="e92a6-147">A list of your assigned eligible roles appear in the grouping at the top of the page.</span></span>
3. <span data-ttu-id="e92a6-148">Välj en roll.</span><span class="sxs-lookup"><span data-stu-id="e92a6-148">Select a role.</span></span>
4. <span data-ttu-id="e92a6-149">Välj den **aktivering väntar på godkännande** banderoll i bladet rollen aktivering information.</span><span class="sxs-lookup"><span data-stu-id="e92a6-149">Select the **Activation is pending approval** banner on the role activation details blade.</span></span>
5. <span data-ttu-id="e92a6-150">Välj **Avbryt** överst i den **godkännande** bladet.</span><span class="sxs-lookup"><span data-stu-id="e92a6-150">Select **Cancel** at the top of the **Pending approval** blade.</span></span>

   ![Avbryta väntande begäran skärmbild][4]

## <a name="next-steps"></a><span data-ttu-id="e92a6-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e92a6-152">Next steps</span></span>
<span data-ttu-id="e92a6-153">Om du vill veta mer om Azure AD Privileged Identity Management har följande länkar mer information.</span><span class="sxs-lookup"><span data-stu-id="e92a6-153">If you're interested in learning more about Azure AD Privileged Identity Management, the following links have more information.</span></span>

[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-how-to-activate-role/PIM_activation_MFA.png
[3]: ./media/active-directory-privileged-identity-management-how-to-activate-role/PIM_Request_Pending_Toast2.png
[4]: ./media/active-directory-privileged-identity-management-how-to-activate-role/PIM_Request_Pending_Banner_Cancel.png

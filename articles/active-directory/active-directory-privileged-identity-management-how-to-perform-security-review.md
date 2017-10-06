---
title: "aaaHow tooperform en åtkomst-granskning | Microsoft Docs"
description: "Lär dig hur tooperform en översyn med hello Azure Privileged Identity Management-program."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 49ee2feb-7d2e-4acf-82c1-40ff23062862
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 301a5e9f97b68fedfbf4954e0bd7dadb7f0fc510
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooperform-an-access-review-in-azure-ad-privileged-identity-management"></a><span data-ttu-id="e09ed-103">Hur tooperform åtkomst finns i Azure AD Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="e09ed-103">How tooperform an access review in Azure AD Privileged Identity Management</span></span>
<span data-ttu-id="e09ed-104">Azure Active Directory (AD) Privileged Identity Management förenklar hur företag hanterar tooresources privilegierad åtkomst i Azure AD och andra Microsoft online services som Office 365 eller Microsoft Intune.</span><span class="sxs-lookup"><span data-stu-id="e09ed-104">Azure Active Directory (AD) Privileged Identity Management simplifies how enterprises manage privileged access tooresources in Azure AD and other Microsoft online services like Office 365 or Microsoft Intune.</span></span>  

<span data-ttu-id="e09ed-105">Om du har tilldelats tooan administrativ roll din organisations administratör av Privilegierade roller kan be dig tooregularly bekräfta att du fortfarande behöver rollen för jobbet.</span><span class="sxs-lookup"><span data-stu-id="e09ed-105">If you are assigned tooan administrative role, your organization's privileged role administrator may ask you tooregularly confirm that you still need that role for your job.</span></span> <span data-ttu-id="e09ed-106">Du kan få ett e-postmeddelande som innehåller en länk eller gå raka toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e09ed-106">You might get an email that includes a link, or you can go straight toohello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="e09ed-107">Hello åtgärderna i den här artikeln tooperform själva granska din tilldelade roller.</span><span class="sxs-lookup"><span data-stu-id="e09ed-107">Follow hello steps in this article tooperform a self-review of your assigned roles.</span></span>

<span data-ttu-id="e09ed-108">Om du är en administratör av Privilegierade roller som är intresserade av åtkomst granskningar kan få mer information på [hur toostart åtkomsten granska](active-directory-privileged-identity-management-how-to-start-security-review.md).</span><span class="sxs-lookup"><span data-stu-id="e09ed-108">If you're a privileged role administrator interested in access reviews, get more details at [How toostart an access review](active-directory-privileged-identity-management-how-to-start-security-review.md).</span></span>

## <a name="add-hello-privileged-identity-management-application"></a><span data-ttu-id="e09ed-109">Lägg till hello Privileged Identity Management-program</span><span class="sxs-lookup"><span data-stu-id="e09ed-109">Add hello Privileged Identity Management application</span></span>
<span data-ttu-id="e09ed-110">Du kan använda hello Azure AD Privileged Identity Management (PIM) programmet i hello [Azure-portalen](https://portal.azure.com/) tooperform din granskning.</span><span class="sxs-lookup"><span data-stu-id="e09ed-110">You can use hello Azure AD Privileged Identity Management (PIM) application in hello [Azure portal](https://portal.azure.com/) tooperform your review.</span></span>  <span data-ttu-id="e09ed-111">Om du inte har programmet för hello Azure AD Privileged Identity Management på portalen, följer du dessa steg tooget igång.</span><span class="sxs-lookup"><span data-stu-id="e09ed-111">If you don't have hello Azure AD Privileged Identity Management application on your portal, follow these steps tooget started.</span></span>

1. <span data-ttu-id="e09ed-112">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="e09ed-112">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="e09ed-113">Välj ditt användarnamn i hello övre högra hörnet av hello Azure-portalen och väljer hello directory där du kommer du att driva.</span><span class="sxs-lookup"><span data-stu-id="e09ed-113">Select your username in hello upper right-hand corner of hello Azure portal, and select hello directory where you will you be operating.</span></span>
3. <span data-ttu-id="e09ed-114">Välj **fler tjänster** och använda hello Filter textruta toosearch för **Azure AD Privileged Identity Management**.</span><span class="sxs-lookup"><span data-stu-id="e09ed-114">Select **More services** and use hello Filter textbox toosearch for **Azure AD Privileged Identity Management**.</span></span>
4. <span data-ttu-id="e09ed-115">Kontrollera **PIN-kod toodashboard** och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="e09ed-115">Check **Pin toodashboard** and then click **Create**.</span></span> <span data-ttu-id="e09ed-116">hello programmet Privileged Identity Management öppnas.</span><span class="sxs-lookup"><span data-stu-id="e09ed-116">hello Privileged Identity Management application will open.</span></span>

## <a name="approve-or-deny-access"></a><span data-ttu-id="e09ed-117">Godkänna eller neka åtkomst</span><span class="sxs-lookup"><span data-stu-id="e09ed-117">Approve or deny access</span></span>
<span data-ttu-id="e09ed-118">När du godkänna eller neka åtkomst, du precis uppmanar hello granskare om du fortfarande använda den här rollen eller inte.</span><span class="sxs-lookup"><span data-stu-id="e09ed-118">When you approve or deny access, you're just telling hello reviewer whether you still use this role or not.</span></span> <span data-ttu-id="e09ed-119">Välj **Godkänn** om du vill toostay hello roll eller **neka** om du inte behöver hello åtkomst längre.</span><span class="sxs-lookup"><span data-stu-id="e09ed-119">Choose **Approve** if you want toostay in hello role, or **Deny** if you don't need hello access anymore.</span></span> <span data-ttu-id="e09ed-120">Statusen ändras inte direkt, tills hello granskare gäller hello resultat.</span><span class="sxs-lookup"><span data-stu-id="e09ed-120">Your status won't change right away, until hello reviewer applies hello results.</span></span>
<span data-ttu-id="e09ed-121">Följ dessa steg toofind och slutför hello åtkomst granskning:</span><span class="sxs-lookup"><span data-stu-id="e09ed-121">Follow these steps toofind and complete hello access review:</span></span>

1. <span data-ttu-id="e09ed-122">Välj i hello PIM-program, **Granska privilegierad åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="e09ed-122">In hello PIM application, select **Review privileged access**.</span></span> <span data-ttu-id="e09ed-123">Om du har alla väntande åtkomst granskningar kan visas de i hello Azure AD åtkomst granskar bladet.</span><span class="sxs-lookup"><span data-stu-id="e09ed-123">If you have any pending access reviews, they appear in hello Azure AD Access reviews blade.</span></span>
2. <span data-ttu-id="e09ed-124">Välj hello granska som du vill toocomplete.</span><span class="sxs-lookup"><span data-stu-id="e09ed-124">Select hello review you want toocomplete.</span></span>
3. <span data-ttu-id="e09ed-125">Om du har skapat hello, granska visas som hello bara användare i hello granskning.</span><span class="sxs-lookup"><span data-stu-id="e09ed-125">Unless you created hello review, you appear as hello only user in hello review.</span></span> <span data-ttu-id="e09ed-126">Välj hello markerat nästa tooyour namn.</span><span class="sxs-lookup"><span data-stu-id="e09ed-126">Select hello check mark next tooyour name.</span></span>
4. <span data-ttu-id="e09ed-127">Välj antingen **godkänna** eller **neka**.</span><span class="sxs-lookup"><span data-stu-id="e09ed-127">Choose either **Approve** or **Deny**.</span></span> <span data-ttu-id="e09ed-128">Du kan behöva tooinclude en orsak till ditt beslut i hello **motivera** textruta.</span><span class="sxs-lookup"><span data-stu-id="e09ed-128">You may need tooinclude a reason for your decision in hello **Provide a reason** text box.</span></span>  
5. <span data-ttu-id="e09ed-129">Stäng hello **granska Azure AD-roller** bladet.</span><span class="sxs-lookup"><span data-stu-id="e09ed-129">Close hello **Review Azure AD roles** blade.</span></span>

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="e09ed-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e09ed-130">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png

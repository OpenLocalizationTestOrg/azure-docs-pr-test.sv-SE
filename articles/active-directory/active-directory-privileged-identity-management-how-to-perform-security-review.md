---
title: "Hur du utför en åtkomst-granskning | Microsoft Docs"
description: "Lär dig hur du utför en granskning med Azure Privileged Identity Management-programmet."
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
ms.openlocfilehash: a98ed60221eeba1d9c92df846aeae2deafb8ae60
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-perform-an-access-review-in-azure-ad-privileged-identity-management"></a><span data-ttu-id="3d28d-103">Hur du utför en åtkomst-granskning i Azure AD Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="3d28d-103">How to perform an access review in Azure AD Privileged Identity Management</span></span>
<span data-ttu-id="3d28d-104">Azure Active Directory (AD) Privileged Identity Management förenklar hur företag hantera privilegierad åtkomst till resurser i Azure AD och andra Microsoft online services som Office 365 eller Microsoft Intune.</span><span class="sxs-lookup"><span data-stu-id="3d28d-104">Azure Active Directory (AD) Privileged Identity Management simplifies how enterprises manage privileged access to resources in Azure AD and other Microsoft online services like Office 365 or Microsoft Intune.</span></span>  

<span data-ttu-id="3d28d-105">Om du har tilldelats en administrativ roll din organisations administratör av Privilegierade roller kan bli ombedd att regelbundet kontrollera att du fortfarande behöver rollen för jobbet.</span><span class="sxs-lookup"><span data-stu-id="3d28d-105">If you are assigned to an administrative role, your organization's privileged role administrator may ask you to regularly confirm that you still need that role for your job.</span></span> <span data-ttu-id="3d28d-106">Du kan få ett e-postmeddelande som innehåller en länk eller du kan gå direkt till den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3d28d-106">You might get an email that includes a link, or you can go straight to the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="3d28d-107">Följ stegen i den här artikeln för att utföra automatisk granska din tilldelade roller.</span><span class="sxs-lookup"><span data-stu-id="3d28d-107">Follow the steps in this article to perform a self-review of your assigned roles.</span></span>

<span data-ttu-id="3d28d-108">Om du är en administratör av Privilegierade roller som är intresserade av åtkomst granskningar kan få mer information på [hur du startar en åtkomst-granskning](active-directory-privileged-identity-management-how-to-start-security-review.md).</span><span class="sxs-lookup"><span data-stu-id="3d28d-108">If you're a privileged role administrator interested in access reviews, get more details at [How to start an access review](active-directory-privileged-identity-management-how-to-start-security-review.md).</span></span>

## <a name="add-the-privileged-identity-management-application"></a><span data-ttu-id="3d28d-109">Lägga till programmet Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="3d28d-109">Add the Privileged Identity Management application</span></span>
<span data-ttu-id="3d28d-110">Du kan använda Azure AD Privileged Identity Management (PIM)-program i den [Azure-portalen](https://portal.azure.com/) att utföra din granskning.</span><span class="sxs-lookup"><span data-stu-id="3d28d-110">You can use the Azure AD Privileged Identity Management (PIM) application in the [Azure portal](https://portal.azure.com/) to perform your review.</span></span>  <span data-ttu-id="3d28d-111">Om du inte har programmet Azure AD Privileged Identity Management på portalen, följer du dessa steg för att komma igång.</span><span class="sxs-lookup"><span data-stu-id="3d28d-111">If you don't have the Azure AD Privileged Identity Management application on your portal, follow these steps to get started.</span></span>

1. <span data-ttu-id="3d28d-112">Logga in på [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="3d28d-112">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="3d28d-113">Välj ditt användarnamn i det övre högra hörnet i Azure-portalen och välj den katalog där du kommer du att driva.</span><span class="sxs-lookup"><span data-stu-id="3d28d-113">Select your username in the upper right-hand corner of the Azure portal, and select the directory where you will you be operating.</span></span>
3. <span data-ttu-id="3d28d-114">Välj **Fler tjänster** och använd textrutan Filter för att söka efter **Azure AD Privileged Identity Management**.</span><span class="sxs-lookup"><span data-stu-id="3d28d-114">Select **More services** and use the Filter textbox to search for **Azure AD Privileged Identity Management**.</span></span>
4. <span data-ttu-id="3d28d-115">Markera **Fäst på instrumentpanelen** och klicka sedan på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="3d28d-115">Check **Pin to dashboard** and then click **Create**.</span></span> <span data-ttu-id="3d28d-116">Programmet Privileged Identity Management öppnas.</span><span class="sxs-lookup"><span data-stu-id="3d28d-116">The Privileged Identity Management application will open.</span></span>

## <a name="approve-or-deny-access"></a><span data-ttu-id="3d28d-117">Godkänna eller neka åtkomst</span><span class="sxs-lookup"><span data-stu-id="3d28d-117">Approve or deny access</span></span>
<span data-ttu-id="3d28d-118">När du godkänna eller neka åtkomst, du precis uppmanar granskaren om du fortfarande använda den här rollen eller inte.</span><span class="sxs-lookup"><span data-stu-id="3d28d-118">When you approve or deny access, you're just telling the reviewer whether you still use this role or not.</span></span> <span data-ttu-id="3d28d-119">Välj **Godkänn** om du vill hålla rollen eller **neka** om du inte längre behöver åtkomst.</span><span class="sxs-lookup"><span data-stu-id="3d28d-119">Choose **Approve** if you want to stay in the role, or **Deny** if you don't need the access anymore.</span></span> <span data-ttu-id="3d28d-120">Statusen ändras inte direkt, tills granskaren gäller resultaten.</span><span class="sxs-lookup"><span data-stu-id="3d28d-120">Your status won't change right away, until the reviewer applies the results.</span></span>
<span data-ttu-id="3d28d-121">Följ dessa steg för att hitta och Slutför granskningen åtkomst:</span><span class="sxs-lookup"><span data-stu-id="3d28d-121">Follow these steps to find and complete the access review:</span></span>

1. <span data-ttu-id="3d28d-122">Markera i PIM-programmet **Granska privilegierad åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="3d28d-122">In the PIM application, select **Review privileged access**.</span></span> <span data-ttu-id="3d28d-123">Om du har alla väntande åtkomst granskningar som de visas i bladet granskningar Azure AD-åtkomst.</span><span class="sxs-lookup"><span data-stu-id="3d28d-123">If you have any pending access reviews, they appear in the Azure AD Access reviews blade.</span></span>
2. <span data-ttu-id="3d28d-124">Välj granska som du vill slutföra.</span><span class="sxs-lookup"><span data-stu-id="3d28d-124">Select the review you want to complete.</span></span>
3. <span data-ttu-id="3d28d-125">Om du har skapat granskningen visas som den enda användaren i granskningen.</span><span class="sxs-lookup"><span data-stu-id="3d28d-125">Unless you created the review, you appear as the only user in the review.</span></span> <span data-ttu-id="3d28d-126">Markera kryssrutan bredvid ditt namn.</span><span class="sxs-lookup"><span data-stu-id="3d28d-126">Select the check mark next to your name.</span></span>
4. <span data-ttu-id="3d28d-127">Välj antingen **godkänna** eller **neka**.</span><span class="sxs-lookup"><span data-stu-id="3d28d-127">Choose either **Approve** or **Deny**.</span></span> <span data-ttu-id="3d28d-128">Du kan behöva ta en orsak till ditt beslut i den **motivera** textruta.</span><span class="sxs-lookup"><span data-stu-id="3d28d-128">You may need to include a reason for your decision in the **Provide a reason** text box.</span></span>  
5. <span data-ttu-id="3d28d-129">Stäng den **granska Azure AD-roller** bladet.</span><span class="sxs-lookup"><span data-stu-id="3d28d-129">Close the **Review Azure AD roles** blade.</span></span>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="3d28d-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3d28d-130">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png

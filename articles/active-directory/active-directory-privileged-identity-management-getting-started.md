---
title: "Komma igång med Azure AD Privileged Identity Management | Microsoft Docs"
description: "Lär dig hur du hanterar privilegierade identiteter med programmet Azure Active Directory Privileged Identity Management på Azure-portalen."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 2299db7d-bee7-40d0-b3c6-8d628ac61071
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/27/2017
ms.author: billmath
ms.custom: pim ; H1Hack27Feb2017
ms.openlocfilehash: 17cdff033cc3dbb199d11c3b8ac1acbc92499877
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="start-using-azure-ad-privileged-identity-management"></a><span data-ttu-id="291d8-103">Börja använda Azure AD Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="291d8-103">Start using Azure AD Privileged Identity Management</span></span>
<span data-ttu-id="291d8-104">Med Azure Active Directory (AD) Privileged Identity Management kan du hantera, kontrollera och övervaka åtkomst inom din organisation.</span><span class="sxs-lookup"><span data-stu-id="291d8-104">With Azure Active Directory (AD) Privileged Identity Management, you can manage, control, and monitor access within your organization.</span></span> <span data-ttu-id="291d8-105">Det här omfånget inkluderar åtkomst till resurser i Azure AD och andra Microsoft online-tjänster som Office 365 eller Microsoft Intune.</span><span class="sxs-lookup"><span data-stu-id="291d8-105">This scope includes access to resources in Azure AD and other Microsoft online services like Office 365 or Microsoft Intune.</span></span>

<span data-ttu-id="291d8-106">Den här artikeln visar hur du lägger till appen Azure AD PIM på instrumentpanelen på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="291d8-106">This article tells you how to add the Azure AD PIM app to your Azure portal dashboard.</span></span>

## <a name="add-the-privileged-identity-management-application"></a><span data-ttu-id="291d8-107">Lägga till programmet Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="291d8-107">Add the Privileged Identity Management application</span></span>
<span data-ttu-id="291d8-108">Innan du använder Azure AD Privileged Identity Management måste du lägga till programmet på instrumentpanelen på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="291d8-108">Before you use Azure AD Privileged Identity Management, you need to add the application to your Azure portal dashboard.</span></span>

1. <span data-ttu-id="291d8-109">Logga in på [Azure-portalen](https://portal.azure.com/) som global administratör för din katalog.</span><span class="sxs-lookup"><span data-stu-id="291d8-109">Sign in to the [Azure portal](https://portal.azure.com/) as a global administrator of your directory.</span></span>
2. <span data-ttu-id="291d8-110">Om din organisation har mer än en katalog väljer du ditt användarnamn längst upp till höger på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="291d8-110">If your organization has more than one directory, select your username in the upper right-hand corner of the Azure portal.</span></span> <span data-ttu-id="291d8-111">Välj den katalog där du vill använda PIM.</span><span class="sxs-lookup"><span data-stu-id="291d8-111">Select the directory where you want to use PIM.</span></span>
3. <span data-ttu-id="291d8-112">Välj **Fler tjänster** och använd textrutan Filter för att söka efter **Azure AD Privileged Identity Management**.</span><span class="sxs-lookup"><span data-stu-id="291d8-112">Select **More services** and use the Filter textbox to search for **Azure AD Privileged Identity Management**.</span></span>
4. <span data-ttu-id="291d8-113">Markera **Fäst på instrumentpanelen** och klicka sedan på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="291d8-113">Check **Pin to dashboard** and then click **Create**.</span></span> <span data-ttu-id="291d8-114">Privileged Identity Management-programmet öppnas.</span><span class="sxs-lookup"><span data-stu-id="291d8-114">The Privileged Identity Management application opens.</span></span>

<span data-ttu-id="291d8-115">Om du är den första personen som använder Azure AD Privileged Identity Management i din katalog så tilldelas du automatiskt rollerna **Säkerhetsadministratör** och **Privilegierad rolladministratör** i katalogen.</span><span class="sxs-lookup"><span data-stu-id="291d8-115">If you're the first person to use Azure AD Privileged Identity Management in your directory, you are automatically assigned the **Security administrator** and **Privileged role administrator** roles in the directory.</span></span> <span data-ttu-id="291d8-116">Endast privilegierade rolladministratörer kan hantera rolltilldelningar för användare.</span><span class="sxs-lookup"><span data-stu-id="291d8-116">Only privileged role administrators can manage role assignments of users.</span></span> <span data-ttu-id="291d8-117">Dessutom kan du välja att köra [Säkerhetsguiden.](active-directory-privileged-identity-management-security-wizard.md)</span><span class="sxs-lookup"><span data-stu-id="291d8-117">In addition, you may choose to run the [security wizard.](active-directory-privileged-identity-management-security-wizard.md)</span></span> <span data-ttu-id="291d8-118">som från grunden lär dig hur du identifierar och tilldelar.</span><span class="sxs-lookup"><span data-stu-id="291d8-118">that walks you through the initial discovery and assignment experience.</span></span>

## <a name="navigate-to-your-tasks"></a><span data-ttu-id="291d8-119">Gå till dina uppgifter</span><span class="sxs-lookup"><span data-stu-id="291d8-119">Navigate to your tasks</span></span>
<span data-ttu-id="291d8-120">När Azure AD Privileged Identity Management har konfigurerats ser du alltid navigeringsbladet när du öppnar programmet.</span><span class="sxs-lookup"><span data-stu-id="291d8-120">Once Azure AD Privileged Identity Management is set up, you see the navigation blade whenever you open the application.</span></span> <span data-ttu-id="291d8-121">Använd det här bladet för att utföra dina identitetshanteringsaktiviteter.</span><span class="sxs-lookup"><span data-stu-id="291d8-121">Use this blade to accomplish your identity management tasks.</span></span>

![Toppnivåaktiviteter för PIM - skärmbild](./media/active-directory-privileged-identity-management-getting-started/PIM_Tasks_New.png)

* <span data-ttu-id="291d8-123">**Mina roller** tar dig till en lista över roller som har tilldelats dig.</span><span class="sxs-lookup"><span data-stu-id="291d8-123">**My Roles** takes you to a list of roles that are assigned to you.</span></span> <span data-ttu-id="291d8-124">Du aktiverar alla roller som du är berättigad till i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="291d8-124">This section is where you activate any roles that you are eligible for.</span></span>
* <span data-ttu-id="291d8-125">**Godkänna förfrågningar (förhandsgranskning)** visar en lista över väntande aktiveringsförfrågningar från användare i din katalog.</span><span class="sxs-lookup"><span data-stu-id="291d8-125">**Approve Requests (Preview)** displays a list of pending activation requests from users in your directory.</span></span> [<span data-ttu-id="291d8-126">Läs mer.</span><span class="sxs-lookup"><span data-stu-id="291d8-126">Learn more.</span></span>](./privileged-identity-management/azure-ad-pim-approval-workflow.md)
* <span data-ttu-id="291d8-127">**Väntande förfrågningar (förhandsgranskning)** visar aktuella förfrågningar om aktivering.</span><span class="sxs-lookup"><span data-stu-id="291d8-127">**Pending Requests (Preview)** displays any current requests to have made to activate.</span></span>
* <span data-ttu-id="291d8-128">**Granska åtkomst** tar dig till alla väntande åtkomstgranskningar som du måste slutföra, oavsett om du granskar åtkomst åt dig själv eller någon annan.</span><span class="sxs-lookup"><span data-stu-id="291d8-128">**Review Access** takes you to any pending access reviews that you need to complete, whether you're reviewing access for yourself or someone else.</span></span>
* <span data-ttu-id="291d8-129">**Azure AD-katalogroller** finns under avsnittet ”Hantera” och är instrumentpanelen för privilegierade rolladministratörer. Där kan du hantera rolltilldelningar, ändra rollaktiveringsinställningar, starta åtkomstgranskningar och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="291d8-129">**Azure AD Directory Roles** located under the 'Manage' section is the dashboard for privileged role admins to manage role assignments, change role activation settings, start access reviews, and more.</span></span> <span data-ttu-id="291d8-130">Alternativen i den här instrumentpanelen är inaktiverade för alla som inte är en privilegierad rolladministratör.</span><span class="sxs-lookup"><span data-stu-id="291d8-130">The options in this dashboard are disabled for anyone who isn't a privileged role administrator.</span></span>

## <a name="next-steps"></a><span data-ttu-id="291d8-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="291d8-131">Next steps</span></span>
<span data-ttu-id="291d8-132">[Azure AD Privileged Identity Management-översikt](active-directory-privileged-identity-management-configure.md) innehåller mer information om hur du kan hantera administrativ åtkomst i din organisation.</span><span class="sxs-lookup"><span data-stu-id="291d8-132">The [Azure AD Privileged Identity Management overview](active-directory-privileged-identity-management-configure.md) includes more details on how you can manage administrative access in your organization.</span></span>

[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png

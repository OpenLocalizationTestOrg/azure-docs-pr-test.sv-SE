---
title: "Tilldela licenser för Azure MFA | Microsoft Docs"
description: "Lär dig hur du tilldelar användarlicenser för Microsoft Azure Multi-Factor Authentication."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 514ef423-8ee6-4987-8a4e-80d5dc394cf9
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/13/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ROBOTS: NOINDEX
ms.openlocfilehash: 45522bf526c4aeab1d6ccc8891a55a0436ff9320
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="assigning-an-azure-mfa-azure-ad-premium-or-enterprise-mobility-license-to-users"></a><span data-ttu-id="28fc3-103">Tilldela en Azure MFA-, Azure AD Premium- eller Enterprise Mobility-licens till användare</span><span class="sxs-lookup"><span data-stu-id="28fc3-103">Assigning an Azure MFA, Azure AD Premium, or Enterprise Mobility license to users</span></span>
<span data-ttu-id="28fc3-104">Om du har köpt Azure MFA-, Azure AD Premium- eller Enterprise Mobility Suite-licenser behöver du inte skapa någon Multi-Factor Authentication-provider.</span><span class="sxs-lookup"><span data-stu-id="28fc3-104">If you have purchased Azure MFA, Azure AD Premium, or Enterprise Mobility Suite licenses, you do not need to create a Multi-Factor Auth provider.</span></span> <span data-ttu-id="28fc3-105">När du tilldelar licenser till dina användare kan du börja aktivera dem för MFA.</span><span class="sxs-lookup"><span data-stu-id="28fc3-105">Once you assign the licenses to your users, you can begin enabling them for MFA.</span></span>

## <a name="to-assign-a-license"></a><span data-ttu-id="28fc3-106">Tilldela en licens</span><span class="sxs-lookup"><span data-stu-id="28fc3-106">To assign a license</span></span>
1. <span data-ttu-id="28fc3-107">Logga in på [den klassiska Azure-portalen](https://manage.windowsazure.com) som administratör.</span><span class="sxs-lookup"><span data-stu-id="28fc3-107">Sign in to the [Azure classic portal](https://manage.windowsazure.com) as an administrator.</span></span>
2. <span data-ttu-id="28fc3-108">Välj **Active Directory** till vänster.</span><span class="sxs-lookup"><span data-stu-id="28fc3-108">On the left, select **Active Directory**.</span></span>
3. <span data-ttu-id="28fc3-109">På Active Directory-sidan dubbelklickar du på katalogen som innehåller de användare som du vill aktivera.</span><span class="sxs-lookup"><span data-stu-id="28fc3-109">On the Active Directory page, double-click the directory that has the users you wish to enable.</span></span>
4. <span data-ttu-id="28fc3-110">Överst på katalogsidan väljer du **Licenser**.</span><span class="sxs-lookup"><span data-stu-id="28fc3-110">At the top of the directory page, select **Licenses**.</span></span>
   <span data-ttu-id="28fc3-111">![Tilldela licenser](./media/multi-factor-authentication-get-started-assign-licenses/assign1.png)</span><span class="sxs-lookup"><span data-stu-id="28fc3-111">![Assign Licenses](./media/multi-factor-authentication-get-started-assign-licenses/assign1.png)</span></span>
5. <span data-ttu-id="28fc3-112">På sidan Licenser väljer du **Azure Multi-Factor Authentication**, **Active Directory Premium** eller **Enterprise Mobility Suite**.</span><span class="sxs-lookup"><span data-stu-id="28fc3-112">On the Licenses page, select **Azure Multi-Factor Authentication**, **Active Directory Premium**, or **Enterprise Mobility Suite**.</span></span>  <span data-ttu-id="28fc3-113">Om du bara har en så har den valts automatiskt.</span><span class="sxs-lookup"><span data-stu-id="28fc3-113">If you only have one, then it should be selected automatically.</span></span>
6. <span data-ttu-id="28fc3-114">Klicka på **Tilldela** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="28fc3-114">At the bottom of the page, click **Assign**.</span></span>
   <span data-ttu-id="28fc3-115">![Tilldela licenser](./media/multi-factor-authentication-get-started-assign-licenses/assign3.png)</span><span class="sxs-lookup"><span data-stu-id="28fc3-115">![Assign Licenses](./media/multi-factor-authentication-get-started-assign-licenses/assign3.png)</span></span>
7. <span data-ttu-id="28fc3-116">I rutan som visas klickar du bredvid de användare eller grupper som du vill tilldela licenser.</span><span class="sxs-lookup"><span data-stu-id="28fc3-116">In the box that comes up, click next to the users or groups you want to assign licenses to.</span></span>  <span data-ttu-id="28fc3-117">En grön bockmarkering bör visas.</span><span class="sxs-lookup"><span data-stu-id="28fc3-117">You should see a green check mark appear.</span></span>
8. <span data-ttu-id="28fc3-118">Spara ändringarna genom att klicka på bockmarkeringen.</span><span class="sxs-lookup"><span data-stu-id="28fc3-118">Click the check mark icon to save the changes.</span></span>
   <span data-ttu-id="28fc3-119">![Tilldela licenser](./media/multi-factor-authentication-get-started-assign-licenses/assign4.png)</span><span class="sxs-lookup"><span data-stu-id="28fc3-119">![Assign Licenses](./media/multi-factor-authentication-get-started-assign-licenses/assign4.png)</span></span>
9. <span data-ttu-id="28fc3-120">Du bör se ett meddelande som anger hur många licenser som har tilldelats och hur många som kanske har misslyckats.</span><span class="sxs-lookup"><span data-stu-id="28fc3-120">You should see a message saying how many licenses were assigned and how many may have failed.</span></span>  <span data-ttu-id="28fc3-121">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="28fc3-121">Click **Ok**.</span></span>
   <span data-ttu-id="28fc3-122">![Tilldela licenser](./media/multi-factor-authentication-get-started-assign-licenses/assign5.png)</span><span class="sxs-lookup"><span data-stu-id="28fc3-122">![Assign Licenses](./media/multi-factor-authentication-get-started-assign-licenses/assign5.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="28fc3-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="28fc3-123">Next steps</span></span>

- <span data-ttu-id="28fc3-124">Mer information finns i [Vad är Microsoft Azure Active Directory-licensiering?](../active-directory/active-directory-licensing-what-is.md) (på engelska)</span><span class="sxs-lookup"><span data-stu-id="28fc3-124">For more information, see [What is Microsoft Azure Active Directory licensing?](../active-directory/active-directory-licensing-what-is.md)</span></span>

---
title: "Ta bort en användare eller grupp från en enterprise-app i Azure Active Directory | Microsoft Docs"
description: "Hur du tar bort tilldelningen åtkomst av en användare eller grupp från en enterprise-app i Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 7b2d365b-ae92-477f-9702-353cc6acc5ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: 02f122acfb53c2107e2b0af66c6195aa127a2c77
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory"></a><span data-ttu-id="eb5bb-103">Ta bort en användare eller grupp från en enterprise-app i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="eb5bb-103">Remove a user or group assignment from an enterprise app in Azure Active Directory</span></span>
<span data-ttu-id="eb5bb-104">Det är enkelt att ta bort en användare eller grupp tilldelas åtkomst till en enterprise-program i Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="eb5bb-104">It's easy to remove a user or a group from being assigned access to one of your enterprise applications in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="eb5bb-105">Du måste ha behörighet att hantera enterprise-appen och du måste vara global administratör för katalogen.</span><span class="sxs-lookup"><span data-stu-id="eb5bb-105">You must have the appropriate permissions to manage the enterprise app, and you must be global admin for the directory.</span></span>

## <a name="how-do-i-remove-a-user-or-group-assignment"></a><span data-ttu-id="eb5bb-106">Hur tar jag bort en användare eller grupptilldelning?</span><span class="sxs-lookup"><span data-stu-id="eb5bb-106">How do I remove a user or group assignment?</span></span>
1. <span data-ttu-id="eb5bb-107">Logga in på den [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för katalogen.</span><span class="sxs-lookup"><span data-stu-id="eb5bb-107">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="eb5bb-108">Välj **fler tjänster**, ange **Azure Active Directory** i textrutan och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="eb5bb-108">Select **More services**, enter **Azure Active Directory** in the text box, and then select **Enter**.</span></span>
3. <span data-ttu-id="eb5bb-109">På den **Azure Active Directory - *directoryname***  bladet (det vill säga Azure AD bladet för den katalog som du hanterar), Välj **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="eb5bb-109">On the **Azure Active Directory - *directoryname*** blade (that is, the Azure AD blade for the directory you are managing), select **Enterprise applications**.</span></span>

    ![Öppna företagsappar](./media/active-directory-coreapps-remove-assignment-user-azure-portal/open-enterprise-apps.png)
4. <span data-ttu-id="eb5bb-111">På den **företagsprogram** bladet väljer **alla program**.</span><span class="sxs-lookup"><span data-stu-id="eb5bb-111">On the **Enterprise applications** blade, select **All applications**.</span></span> <span data-ttu-id="eb5bb-112">Visas en lista över appar som du kan hantera.</span><span class="sxs-lookup"><span data-stu-id="eb5bb-112">You'll see a list of the apps you can manage.</span></span>
5. <span data-ttu-id="eb5bb-113">På den **företagsprogram - alla program** bladet väljer du en app.</span><span class="sxs-lookup"><span data-stu-id="eb5bb-113">On the **Enterprise applications - All applications** blade, select an app.</span></span>
6. <span data-ttu-id="eb5bb-114">På den ***appname*** bladet (det vill säga bladet med namnet på den valda appen i namnet), Välj **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="eb5bb-114">On the ***appname*** blade (that is, the blade with the name of the selected app in the title), select **Users & Groups**.</span></span>

    ![Att välja användare eller grupper](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-app-users.png)
7. <span data-ttu-id="eb5bb-116">På den ***appname*** **-användaren & grupptilldelning** bladet Välj en av flera användare eller grupper och välj sedan den **ta bort** kommando.</span><span class="sxs-lookup"><span data-stu-id="eb5bb-116">On the ***appname*** **- User & Group Assignment** blade, select one of more users or groups and then select the **Remove** command.</span></span> <span data-ttu-id="eb5bb-117">Bekräfta beslutet i Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="eb5bb-117">Confirm your decision at the prompt.</span></span>

    ![Att välja kommandot Ta bort](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-users.png)

## <a name="next-steps"></a><span data-ttu-id="eb5bb-119">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="eb5bb-119">Next steps</span></span>
* [<span data-ttu-id="eb5bb-120">Visa alla mina grupper</span><span class="sxs-lookup"><span data-stu-id="eb5bb-120">See all of my groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="eb5bb-121">Tilldela en användare eller grupp till en enterprise-app</span><span class="sxs-lookup"><span data-stu-id="eb5bb-121">Assign a user or group to an enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)
* [<span data-ttu-id="eb5bb-122">Inaktivera användarinloggningar för en enterprise-app</span><span class="sxs-lookup"><span data-stu-id="eb5bb-122">Disable user sign-ins for an enterprise app</span></span>](active-directory-coreapps-disable-app-azure-portal.md)
* [<span data-ttu-id="eb5bb-123">Ändra namnet eller logotypen av en enterprise-app</span><span class="sxs-lookup"><span data-stu-id="eb5bb-123">Change the name or logo of an enterprise app</span></span>](active-directory-coreapps-change-app-logo-user-azure-portal.md)

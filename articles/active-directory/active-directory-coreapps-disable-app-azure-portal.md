---
title: "Inaktivera användarinloggningar för en enterprise-app i Azure Active Directory | Microsoft Docs"
description: "Så här inaktiverar du en enterprise-programmet så att inga användare kan logga in till den i Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: a27562f9-18dc-42e8-9fee-5419566f8fd7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: 5d27046370eada0c371c94fb573fa1bcf536f7cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory"></a><span data-ttu-id="9047e-103">Inaktivera användarinloggningar för en enterprise-app i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9047e-103">Disable user sign-ins for an enterprise app in Azure Active Directory</span></span>
<span data-ttu-id="9047e-104">Det är enkelt att inaktivera ett företagsprogram så att inga användare kan logga in till den i Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="9047e-104">It's easy to disable an enterprise application so that no users may sign in to it in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="9047e-105">Du måste ha behörighet att hantera enterprise-appen och du måste vara global administratör för katalogen.</span><span class="sxs-lookup"><span data-stu-id="9047e-105">You must have the appropriate permissions to manage the enterprise app, and you must be global admin for the directory.</span></span>

## <a name="how-do-i-disable-user-sign-ins"></a><span data-ttu-id="9047e-106">Hur inaktiverar användarinloggningar?</span><span class="sxs-lookup"><span data-stu-id="9047e-106">How do I disable user sign-ins?</span></span>
1. <span data-ttu-id="9047e-107">Logga in på den [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för katalogen.</span><span class="sxs-lookup"><span data-stu-id="9047e-107">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="9047e-108">Välj **fler tjänster**, ange **Azure Active Directory** i textrutan och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="9047e-108">Select **More services**, enter **Azure Active Directory** in the text box, and then select **Enter**.</span></span>
3. <span data-ttu-id="9047e-109">På den **Azure Active Directory** -  ***directoryname*** bladet (det vill säga Azure AD bladet för den katalog som du hanterar), Välj **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="9047e-109">On the **Azure Active Directory** -  ***directoryname*** blade (that is, the Azure AD blade for the directory you are managing), select **Enterprise applications**.</span></span>

    ![Öppna företagsappar](./media/active-directory-coreapps-disable-app-azure-portal/open-enterprise-apps.png)
4. <span data-ttu-id="9047e-111">På den **företagsprogram** bladet väljer **alla program**.</span><span class="sxs-lookup"><span data-stu-id="9047e-111">On the **Enterprise applications** blade, select **All applications**.</span></span> <span data-ttu-id="9047e-112">Du kan se en lista över appar som du kan hantera.</span><span class="sxs-lookup"><span data-stu-id="9047e-112">You see a list of the apps you can manage.</span></span>
5. <span data-ttu-id="9047e-113">På den **företagsprogram - alla program** bladet väljer du en app.</span><span class="sxs-lookup"><span data-stu-id="9047e-113">On the **Enterprise applications - All applications** blade, select an app.</span></span>
6. <span data-ttu-id="9047e-114">På den ***appname*** bladet (det vill säga bladet med namnet på den valda appen i namnet), Välj **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="9047e-114">On the ***appname*** blade (that is, the blade with the name of the selected app in the title), select **Properties**.</span></span>

    ![Att välja kommandot alla program](./media/active-directory-coreapps-disable-app-azure-portal/select-app.png)
7. <span data-ttu-id="9047e-116">På den ***appname*** - **egenskaper** bladet väljer **nr** för **aktiverats för användare att logga in?**.</span><span class="sxs-lookup"><span data-stu-id="9047e-116">On the ***appname*** - **Properties** blade, select **No** for **Enabled for users to sign-in?**.</span></span>
8. <span data-ttu-id="9047e-117">Välj den **spara** kommando.</span><span class="sxs-lookup"><span data-stu-id="9047e-117">Select the **Save** command.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9047e-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9047e-118">Next steps</span></span>
* [<span data-ttu-id="9047e-119">Se alla grupper</span><span class="sxs-lookup"><span data-stu-id="9047e-119">See all my groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="9047e-120">Tilldela en användare eller grupp till en enterprise-app</span><span class="sxs-lookup"><span data-stu-id="9047e-120">Assign a user or group to an enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)
* [<span data-ttu-id="9047e-121">Ta bort en användare eller grupp från en enterprise-app</span><span class="sxs-lookup"><span data-stu-id="9047e-121">Remove a user or group assignment from an enterprise app</span></span>](active-directory-coreapps-remove-assignment-azure-portal.md)
* [<span data-ttu-id="9047e-122">Ändra namnet eller logotypen av en enterprise-app</span><span class="sxs-lookup"><span data-stu-id="9047e-122">Change the name or logo of an enterprise app</span></span>](active-directory-coreapps-change-app-logo-user-azure-portal.md)

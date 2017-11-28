---
title: "aaaDisable användarinloggningar för en enterprise-app i Azure Active Directory | Microsoft Docs"
description: "Hur toodisable ett företagsprogram så att inga användare kan logga in tooit i Azure Active Directory"
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
ms.openlocfilehash: 4c560b59359d433b0852a7606cc2cc0204866234
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory"></a><span data-ttu-id="94a44-103">Inaktivera användarinloggningar för en enterprise-app i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="94a44-103">Disable user sign-ins for an enterprise app in Azure Active Directory</span></span>
<span data-ttu-id="94a44-104">Det är enkelt toodisable ett företagsprogram så att inga användare kan logga in tooit i Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="94a44-104">It's easy toodisable an enterprise application so that no users may sign in tooit in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="94a44-105">Du måste ha en hello behörighet toomanage hello och du måste vara global administratör för hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="94a44-105">You must have hello appropriate permissions toomanage hello enterprise app, and you must be global admin for hello directory.</span></span>

## <a name="how-do-i-disable-user-sign-ins"></a><span data-ttu-id="94a44-106">Hur inaktiverar användarinloggningar?</span><span class="sxs-lookup"><span data-stu-id="94a44-106">How do I disable user sign-ins?</span></span>
1. <span data-ttu-id="94a44-107">Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="94a44-107">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="94a44-108">Välj **fler tjänster**, ange **Azure Active Directory** i hello textruta och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="94a44-108">Select **More services**, enter **Azure Active Directory** in hello text box, and then select **Enter**.</span></span>
3. <span data-ttu-id="94a44-109">På hello **Azure Active Directory** -  ***directoryname*** bladet (det vill säga hello Azure AD bladet för hello-katalog som du hanterar) Välj **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="94a44-109">On hello **Azure Active Directory** -  ***directoryname*** blade (that is, hello Azure AD blade for hello directory you are managing), select **Enterprise applications**.</span></span>

    ![Öppna företagsappar](./media/active-directory-coreapps-disable-app-azure-portal/open-enterprise-apps.png)
4. <span data-ttu-id="94a44-111">På hello **företagsprogram** bladet väljer **alla program**.</span><span class="sxs-lookup"><span data-stu-id="94a44-111">On hello **Enterprise applications** blade, select **All applications**.</span></span> <span data-ttu-id="94a44-112">Du kan se en lista över hello-appar som du kan hantera.</span><span class="sxs-lookup"><span data-stu-id="94a44-112">You see a list of hello apps you can manage.</span></span>
5. <span data-ttu-id="94a44-113">På hello **företagsprogram - alla program** bladet väljer du en app.</span><span class="sxs-lookup"><span data-stu-id="94a44-113">On hello **Enterprise applications - All applications** blade, select an app.</span></span>
6. <span data-ttu-id="94a44-114">På hello ***appname*** bladet (det vill säga hello bladet med hello namn för valda hello-appen i hello rubrik) Välj **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="94a44-114">On hello ***appname*** blade (that is, hello blade with hello name of hello selected app in hello title), select **Properties**.</span></span>

    ![Att välja hello kommandot för alla program](./media/active-directory-coreapps-disable-app-azure-portal/select-app.png)
7. <span data-ttu-id="94a44-116">På hello ***appname*** - **egenskaper** bladet väljer **nr** för **aktiverats för användare i toosign?**.</span><span class="sxs-lookup"><span data-stu-id="94a44-116">On hello ***appname*** - **Properties** blade, select **No** for **Enabled for users toosign-in?**.</span></span>
8. <span data-ttu-id="94a44-117">Välj hello **spara** kommando.</span><span class="sxs-lookup"><span data-stu-id="94a44-117">Select hello **Save** command.</span></span>

## <a name="next-steps"></a><span data-ttu-id="94a44-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="94a44-118">Next steps</span></span>
* [<span data-ttu-id="94a44-119">Se alla grupper</span><span class="sxs-lookup"><span data-stu-id="94a44-119">See all my groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="94a44-120">Tilldela en användare eller grupp tooan enterprise app</span><span class="sxs-lookup"><span data-stu-id="94a44-120">Assign a user or group tooan enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)
* [<span data-ttu-id="94a44-121">Ta bort en användare eller grupp från en enterprise-app</span><span class="sxs-lookup"><span data-stu-id="94a44-121">Remove a user or group assignment from an enterprise app</span></span>](active-directory-coreapps-remove-assignment-azure-portal.md)
* [<span data-ttu-id="94a44-122">Ändra hello namn eller logotyp av en enterprise-app</span><span class="sxs-lookup"><span data-stu-id="94a44-122">Change hello name or logo of an enterprise app</span></span>](active-directory-coreapps-change-app-logo-user-azure-portal.md)

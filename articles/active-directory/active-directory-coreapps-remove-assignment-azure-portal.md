---
title: "aaaRemove en tilldelning av användare eller grupp från en enterprise-app i Azure Active Directory | Microsoft Docs"
description: "Hur tooremove hello åt tilldelning av en användare eller grupp från en enterprise-app i Azure Active Directory"
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
ms.openlocfilehash: c067ecf59b4dedfe8f848357ca8bd545bdc610eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory"></a><span data-ttu-id="ebe4d-103">Ta bort en användare eller grupp från en enterprise-app i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ebe4d-103">Remove a user or group assignment from an enterprise app in Azure Active Directory</span></span>
<span data-ttu-id="ebe4d-104">Det är enkelt tooremove en användare eller en grupp som tilldelas åtkomst tooone enterprise program i Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="ebe4d-104">It's easy tooremove a user or a group from being assigned access tooone of your enterprise applications in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="ebe4d-105">Du måste ha en hello behörighet toomanage hello och du måste vara global administratör för hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="ebe4d-105">You must have hello appropriate permissions toomanage hello enterprise app, and you must be global admin for hello directory.</span></span>

## <a name="how-do-i-remove-a-user-or-group-assignment"></a><span data-ttu-id="ebe4d-106">Hur tar jag bort en användare eller grupptilldelning?</span><span class="sxs-lookup"><span data-stu-id="ebe4d-106">How do I remove a user or group assignment?</span></span>
1. <span data-ttu-id="ebe4d-107">Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="ebe4d-107">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="ebe4d-108">Välj **fler tjänster**, ange **Azure Active Directory** i hello textruta och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="ebe4d-108">Select **More services**, enter **Azure Active Directory** in hello text box, and then select **Enter**.</span></span>
3. <span data-ttu-id="ebe4d-109">På hello **Azure Active Directory - *directoryname***  bladet (det vill säga hello Azure AD bladet för hello-katalog som du hanterar) Välj **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="ebe4d-109">On hello **Azure Active Directory - *directoryname*** blade (that is, hello Azure AD blade for hello directory you are managing), select **Enterprise applications**.</span></span>

    ![Öppna företagsappar](./media/active-directory-coreapps-remove-assignment-user-azure-portal/open-enterprise-apps.png)
4. <span data-ttu-id="ebe4d-111">På hello **företagsprogram** bladet väljer **alla program**.</span><span class="sxs-lookup"><span data-stu-id="ebe4d-111">On hello **Enterprise applications** blade, select **All applications**.</span></span> <span data-ttu-id="ebe4d-112">Visas en lista över hello-appar som du kan hantera.</span><span class="sxs-lookup"><span data-stu-id="ebe4d-112">You'll see a list of hello apps you can manage.</span></span>
5. <span data-ttu-id="ebe4d-113">På hello **företagsprogram - alla program** bladet väljer du en app.</span><span class="sxs-lookup"><span data-stu-id="ebe4d-113">On hello **Enterprise applications - All applications** blade, select an app.</span></span>
6. <span data-ttu-id="ebe4d-114">På hello ***appname*** bladet (det vill säga hello bladet med hello namn för valda hello-appen i hello rubrik) Välj **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="ebe4d-114">On hello ***appname*** blade (that is, hello blade with hello name of hello selected app in hello title), select **Users & Groups**.</span></span>

    ![Att välja användare eller grupper](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-app-users.png)
7. <span data-ttu-id="ebe4d-116">På hello ***appname*** **-användaren & grupptilldelning** bladet Välj en av flera användare eller grupper och välj sedan hello **ta bort** kommando.</span><span class="sxs-lookup"><span data-stu-id="ebe4d-116">On hello ***appname*** **- User & Group Assignment** blade, select one of more users or groups and then select hello **Remove** command.</span></span> <span data-ttu-id="ebe4d-117">Bekräfta beslutet hello i Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="ebe4d-117">Confirm your decision at hello prompt.</span></span>

    ![Välja hello ta borttagningskommando](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-users.png)

## <a name="next-steps"></a><span data-ttu-id="ebe4d-119">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ebe4d-119">Next steps</span></span>
* [<span data-ttu-id="ebe4d-120">Visa alla mina grupper</span><span class="sxs-lookup"><span data-stu-id="ebe4d-120">See all of my groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="ebe4d-121">Tilldela en användare eller grupp tooan enterprise app</span><span class="sxs-lookup"><span data-stu-id="ebe4d-121">Assign a user or group tooan enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)
* [<span data-ttu-id="ebe4d-122">Inaktivera användarinloggningar för en enterprise-app</span><span class="sxs-lookup"><span data-stu-id="ebe4d-122">Disable user sign-ins for an enterprise app</span></span>](active-directory-coreapps-disable-app-azure-portal.md)
* [<span data-ttu-id="ebe4d-123">Ändra hello namn eller logotyp av en enterprise-app</span><span class="sxs-lookup"><span data-stu-id="ebe4d-123">Change hello name or logo of an enterprise app</span></span>](active-directory-coreapps-change-app-logo-user-azure-portal.md)

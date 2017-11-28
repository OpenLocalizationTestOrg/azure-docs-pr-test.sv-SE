---
title: "aaaAssign en användare eller grupp tooan enterprise-app i Azure Active Directory | Microsoft Docs"
description: "Hur tooselect tooassign en enterprise-appen en användare eller grupp tooit i Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 5817ad48-d916-492b-a8d0-2ade8c50a224
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: 86c11f19892b9c947a5331677c17759178ed2806
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="assign-a-user-or-group-tooan-enterprise-app-in-azure-active-directory"></a><span data-ttu-id="3e9a6-103">Tilldela en användare eller grupp tooan enterprise-app i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3e9a6-103">Assign a user or group tooan enterprise app in Azure Active Directory</span></span>
<span data-ttu-id="3e9a6-104">Det är enkelt tooassign en användare eller en grupp tooyour företagsprogram i Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="3e9a6-104">It's easy tooassign a user or a group tooyour enterprise applications in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="3e9a6-105">Du måste ha en hello behörighet toomanage hello och du måste vara global administratör för hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="3e9a6-105">You must have hello appropriate permissions toomanage hello enterprise app, and you must be global admin for hello directory.</span></span>

## <a name="how-do-i-assign-user-access-tooan-enterprise-app"></a><span data-ttu-id="3e9a6-106">Hur tilldelar användaren åtkomst tooan enterprise app?</span><span class="sxs-lookup"><span data-stu-id="3e9a6-106">How do I assign user access tooan enterprise app?</span></span>
1. <span data-ttu-id="3e9a6-107">Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="3e9a6-107">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="3e9a6-108">Välj **fler tjänster**, ange Azure Active Directory i hello och väljer sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="3e9a6-108">Select **More services**, enter Azure Active Directory in hello text box, and then select **Enter**.</span></span>
3. <span data-ttu-id="3e9a6-109">På hello **Azure Active Directory - *directoryname***  bladet (det vill säga hello Azure AD bladet för hello-katalog som du hanterar) Välj **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="3e9a6-109">On hello **Azure Active Directory - *directoryname*** blade (that is, hello Azure AD blade for hello directory you are managing), select **Enterprise applications**.</span></span>

    ![Öppna företagsappar](./media/active-directory-coreapps-assign-user-azure-portal/open-enterprise-apps.png)
4. <span data-ttu-id="3e9a6-111">På hello **företagsprogram** bladet väljer **alla program**.</span><span class="sxs-lookup"><span data-stu-id="3e9a6-111">On hello **Enterprise applications** blade, select **All applications**.</span></span> <span data-ttu-id="3e9a6-112">Visas en lista över hello-appar som du kan hantera.</span><span class="sxs-lookup"><span data-stu-id="3e9a6-112">You'll see a list of hello apps you can manage.</span></span>
5. <span data-ttu-id="3e9a6-113">På hello **företagsprogram - alla program** bladet väljer du en app.</span><span class="sxs-lookup"><span data-stu-id="3e9a6-113">On hello **Enterprise applications - All applications** blade, select an app.</span></span>
6. <span data-ttu-id="3e9a6-114">På hello ***appname*** bladet (det vill säga hello bladet med hello namn för valda hello-appen i hello rubrik) Välj **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="3e9a6-114">On hello ***appname*** blade (that is, hello blade with hello name of hello selected app in hello title), select **Users & Groups**.</span></span>

    ![Att välja hello kommandot för alla program](./media/active-directory-coreapps-assign-user-azure-portal/select-app-users.png)
7. <span data-ttu-id="3e9a6-116">På hello ***appname*** **-användaren & grupptilldelning** bladet, Välj hello **Lägg till** kommando.</span><span class="sxs-lookup"><span data-stu-id="3e9a6-116">On hello ***appname*** **- User & Group Assignment** blade, select hello **Add** command.</span></span>
8. <span data-ttu-id="3e9a6-117">På hello **Lägg uppdrag** bladet väljer **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="3e9a6-117">On hello **Add Assignment** blade, select **Users and groups**.</span></span>

    ![Tilldela en användare eller grupp toohello app](./media/active-directory-coreapps-assign-user-azure-portal/assign-users.png)
9. <span data-ttu-id="3e9a6-119">På hello **användare och grupper** bladet, Välj en eller flera användare eller grupper från hello och sedan väljer hello **Välj** knappen längst ned hello hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="3e9a6-119">On hello **Users and groups** blade, select one or more users or groups from hello list and then select hello **Select** button at hello bottom of hello blade.</span></span>
10. <span data-ttu-id="3e9a6-120">På hello **Lägg uppdrag** bladet väljer **rollen**.</span><span class="sxs-lookup"><span data-stu-id="3e9a6-120">On hello **Add Assignment** blade, select **Role**.</span></span> <span data-ttu-id="3e9a6-121">Klicka sedan på hello **Välj roll** bladet, Välj en roll tooapply toohello markerade användare eller grupper och välj sedan hello **OK** knappen längst ned hello hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="3e9a6-121">Then, on hello **Select Role** blade, select a role tooapply toohello selected users or groups, and then select hello **OK** button at hello bottom of hello blade.</span></span>
11. <span data-ttu-id="3e9a6-122">På hello **Lägg uppdrag** bladet, Välj hello **tilldela** knappen längst ned hello hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="3e9a6-122">On hello **Add Assignment** blade, select hello **Assign** button at hello bottom of hello blade.</span></span> <span data-ttu-id="3e9a6-123">hello tilldelad användare eller grupper har hello behörigheter som definieras av hello valda rollen för den här enterprise-appen.</span><span class="sxs-lookup"><span data-stu-id="3e9a6-123">hello assigned users or groups will have hello permissions defined by hello selected role for this enterprise app.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3e9a6-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3e9a6-124">Next steps</span></span>
* [<span data-ttu-id="3e9a6-125">Visa alla mina grupper</span><span class="sxs-lookup"><span data-stu-id="3e9a6-125">See all of my groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="3e9a6-126">Ta bort en användare eller grupp från en enterprise-app</span><span class="sxs-lookup"><span data-stu-id="3e9a6-126">Remove a user or group assignment from an enterprise app</span></span>](active-directory-coreapps-remove-assignment-azure-portal.md)
* [<span data-ttu-id="3e9a6-127">Inaktivera användarinloggningar för en enterprise-app</span><span class="sxs-lookup"><span data-stu-id="3e9a6-127">Disable user sign-ins for an enterprise app</span></span>](active-directory-coreapps-disable-app-azure-portal.md)
* [<span data-ttu-id="3e9a6-128">Ändra hello namn eller logotyp av en enterprise-app</span><span class="sxs-lookup"><span data-stu-id="3e9a6-128">Change hello name or logo of an enterprise app</span></span>](active-directory-coreapps-change-app-logo-user-azure-portal.md)

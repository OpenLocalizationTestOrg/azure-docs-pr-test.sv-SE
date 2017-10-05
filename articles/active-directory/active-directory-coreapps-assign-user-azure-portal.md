---
title: "Tilldela en användare eller grupp till en enterprise-app i Azure Active Directory | Microsoft Docs"
description: "Hur du väljer en enterprise-app för att tilldela en användare eller grupp till den i Azure Active Directory"
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
ms.openlocfilehash: ee784704ada9238b5cd048f99aaa4cb192ec7d57
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="assign-a-user-or-group-to-an-enterprise-app-in-azure-active-directory"></a><span data-ttu-id="7119c-103">Tilldela en användare eller grupp till en enterprise-app i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7119c-103">Assign a user or group to an enterprise app in Azure Active Directory</span></span>
<span data-ttu-id="7119c-104">Det är lätt att tilldela en användare eller grupp i enterprise-programmen i Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="7119c-104">It's easy to assign a user or a group to your enterprise applications in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="7119c-105">Du måste ha behörighet att hantera enterprise-appen och du måste vara global administratör för katalogen.</span><span class="sxs-lookup"><span data-stu-id="7119c-105">You must have the appropriate permissions to manage the enterprise app, and you must be global admin for the directory.</span></span>

## <a name="how-do-i-assign-user-access-to-an-enterprise-app"></a><span data-ttu-id="7119c-106">Hur tilldelar åtkomst till en enterprise-app?</span><span class="sxs-lookup"><span data-stu-id="7119c-106">How do I assign user access to an enterprise app?</span></span>
1. <span data-ttu-id="7119c-107">Logga in på den [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för katalogen.</span><span class="sxs-lookup"><span data-stu-id="7119c-107">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="7119c-108">Välj **fler tjänster**, ange Azure Active Directory i textrutan och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="7119c-108">Select **More services**, enter Azure Active Directory in the text box, and then select **Enter**.</span></span>
3. <span data-ttu-id="7119c-109">På den **Azure Active Directory - *directoryname***  bladet (det vill säga Azure AD bladet för den katalog som du hanterar), Välj **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="7119c-109">On the **Azure Active Directory - *directoryname*** blade (that is, the Azure AD blade for the directory you are managing), select **Enterprise applications**.</span></span>

    ![Öppna företagsappar](./media/active-directory-coreapps-assign-user-azure-portal/open-enterprise-apps.png)
4. <span data-ttu-id="7119c-111">På den **företagsprogram** bladet väljer **alla program**.</span><span class="sxs-lookup"><span data-stu-id="7119c-111">On the **Enterprise applications** blade, select **All applications**.</span></span> <span data-ttu-id="7119c-112">Visas en lista över appar som du kan hantera.</span><span class="sxs-lookup"><span data-stu-id="7119c-112">You'll see a list of the apps you can manage.</span></span>
5. <span data-ttu-id="7119c-113">På den **företagsprogram - alla program** bladet väljer du en app.</span><span class="sxs-lookup"><span data-stu-id="7119c-113">On the **Enterprise applications - All applications** blade, select an app.</span></span>
6. <span data-ttu-id="7119c-114">På den ***appname*** bladet (det vill säga bladet med namnet på den valda appen i namnet), Välj **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="7119c-114">On the ***appname*** blade (that is, the blade with the name of the selected app in the title), select **Users & Groups**.</span></span>

    ![Att välja kommandot alla program](./media/active-directory-coreapps-assign-user-azure-portal/select-app-users.png)
7. <span data-ttu-id="7119c-116">På den ***appname*** **-användaren & grupptilldelning** bladet väljer den **Lägg till** kommando.</span><span class="sxs-lookup"><span data-stu-id="7119c-116">On the ***appname*** **- User & Group Assignment** blade, select the **Add** command.</span></span>
8. <span data-ttu-id="7119c-117">På den **Lägg uppdrag** bladet väljer **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="7119c-117">On the **Add Assignment** blade, select **Users and groups**.</span></span>

    ![Tilldela en användare eller grupp i appen](./media/active-directory-coreapps-assign-user-azure-portal/assign-users.png)
9. <span data-ttu-id="7119c-119">På den **användare och grupper** bladet Välj en eller flera användare eller grupper i listan och välj sedan den **Välj** längst ned på bladet.</span><span class="sxs-lookup"><span data-stu-id="7119c-119">On the **Users and groups** blade, select one or more users or groups from the list and then select the **Select** button at the bottom of the blade.</span></span>
10. <span data-ttu-id="7119c-120">På den **Lägg uppdrag** bladet väljer **rollen**.</span><span class="sxs-lookup"><span data-stu-id="7119c-120">On the **Add Assignment** blade, select **Role**.</span></span> <span data-ttu-id="7119c-121">Klicka sedan på den **Välj roll** bladet Välj en roll ska tillämpas på de valda användare eller grupper och välj sedan den **OK** längst ned på bladet.</span><span class="sxs-lookup"><span data-stu-id="7119c-121">Then, on the **Select Role** blade, select a role to apply to the selected users or groups, and then select the **OK** button at the bottom of the blade.</span></span>
11. <span data-ttu-id="7119c-122">På den **Lägg uppdrag** bladet väljer den **tilldela** längst ned på bladet.</span><span class="sxs-lookup"><span data-stu-id="7119c-122">On the **Add Assignment** blade, select the **Assign** button at the bottom of the blade.</span></span> <span data-ttu-id="7119c-123">Tilldelade användare eller grupper har de behörigheter som definieras av den valda rollen för den här enterprise-appen.</span><span class="sxs-lookup"><span data-stu-id="7119c-123">The assigned users or groups will have the permissions defined by the selected role for this enterprise app.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7119c-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7119c-124">Next steps</span></span>
* [<span data-ttu-id="7119c-125">Visa alla mina grupper</span><span class="sxs-lookup"><span data-stu-id="7119c-125">See all of my groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="7119c-126">Ta bort en användare eller grupp från en enterprise-app</span><span class="sxs-lookup"><span data-stu-id="7119c-126">Remove a user or group assignment from an enterprise app</span></span>](active-directory-coreapps-remove-assignment-azure-portal.md)
* [<span data-ttu-id="7119c-127">Inaktivera användarinloggningar för en enterprise-app</span><span class="sxs-lookup"><span data-stu-id="7119c-127">Disable user sign-ins for an enterprise app</span></span>](active-directory-coreapps-disable-app-azure-portal.md)
* [<span data-ttu-id="7119c-128">Ändra namnet eller logotypen av en enterprise-app</span><span class="sxs-lookup"><span data-stu-id="7119c-128">Change the name or logo of an enterprise app</span></span>](active-directory-coreapps-change-app-logo-user-azure-portal.md)

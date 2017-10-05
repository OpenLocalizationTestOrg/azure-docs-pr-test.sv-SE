---
title: "Ändra namnet eller logotypen av en enterprise-app i Azure Active Directory | Microsoft Docs"
description: "Så här ändrar du namnet eller logotypen för en anpassad enterprise-app i Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: d01303ce-e6cb-4f3b-a4d6-ec29dfd68146
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: 3e44e876dcbac704a9809ae5b3957bf94be21c48
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="change-the-name-or-logo-of-an-enterprise-app-in-azure-active-directory"></a><span data-ttu-id="50692-103">Ändra namn på eller av en enterprise-app i Azure Active Directory-logotypen</span><span class="sxs-lookup"><span data-stu-id="50692-103">Change the name or logo of an enterprise app in Azure Active Directory</span></span>
<span data-ttu-id="50692-104">Det är enkelt att ändra namnet eller logotyp för en anpassad företagsprogram i Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="50692-104">It's easy to change the name or logo for a custom enterprise application in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="50692-105">Du måste ha behörighet att göra dessa ändringar, och du måste vara skapare av anpassad app.</span><span class="sxs-lookup"><span data-stu-id="50692-105">You must have the appropriate permissions to make these changes, and you must be the creator of the custom app.</span></span>

## <a name="how-do-i-change-an-enterprise-apps-name-or-logo"></a><span data-ttu-id="50692-106">Hur ändrar jag enterprise appens namn eller logotyp?</span><span class="sxs-lookup"><span data-stu-id="50692-106">How do I change an enterprise app's name or logo?</span></span>
1. <span data-ttu-id="50692-107">Logga in på den [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för katalogen.</span><span class="sxs-lookup"><span data-stu-id="50692-107">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="50692-108">Välj **fler tjänster**, ange **Azure Active Directory** i textrutan och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="50692-108">Select **More services**, enter **Azure Active Directory** in the text box, and then select **Enter**.</span></span>
3. <span data-ttu-id="50692-109">På den **Azure Active Directory - *directoryname***  bladet (det vill säga Azure AD bladet för den katalog som du hanterar), Välj **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="50692-109">On the **Azure Active Directory - *directoryname*** blade (that is, the Azure AD blade for the directory you are managing), select **Enterprise applications**.</span></span>

    ![Öppna företagsappar](./media/active-directory-coreapps-change-app-logo-azure-portal/open-enterprise-apps.png)
4. <span data-ttu-id="50692-111">På den **företagsprogram** bladet väljer **alla program**.</span><span class="sxs-lookup"><span data-stu-id="50692-111">On the **Enterprise applications** blade, select **All applications**.</span></span> <span data-ttu-id="50692-112">Visas en lista över appar som du kan hantera.</span><span class="sxs-lookup"><span data-stu-id="50692-112">You'll see a list of the apps you can manage.</span></span>
5. <span data-ttu-id="50692-113">På den **företagsprogram - alla program** bladet väljer du en app.</span><span class="sxs-lookup"><span data-stu-id="50692-113">On the **Enterprise applications - All applications** blade, select an app.</span></span>
6. <span data-ttu-id="50692-114">På den ***appname*** bladet (det vill säga bladet med namnet på den valda appen i namnet), Välj **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="50692-114">On the ***appname*** blade (that is, the blade with the name of the selected app in the title), select **Properties**.</span></span>

    ![Att välja kommandot Egenskaper](./media/active-directory-coreapps-change-app-logo-azure-portal/select-app.png)
7. <span data-ttu-id="50692-116">På den ***appname*** **-egenskaper** bladet, bläddra efter en fil som ska användas som en logotyp som är nya eller redigera namnet på appen, eller båda.</span><span class="sxs-lookup"><span data-stu-id="50692-116">On the ***appname*** **- Properties** blade, browse for a file to use as a new logo, or edit the app name, or both.</span></span>

    ![Ändra kommandot app logotyp eller nameproperties](./media/active-directory-coreapps-change-app-logo-azure-portal/change-logo.png)
8. <span data-ttu-id="50692-118">Välj den **spara** kommando.</span><span class="sxs-lookup"><span data-stu-id="50692-118">Select the **Save** command.</span></span>

## <a name="next-steps"></a><span data-ttu-id="50692-119">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="50692-119">Next steps</span></span>
* [<span data-ttu-id="50692-120">Visa alla mina grupper</span><span class="sxs-lookup"><span data-stu-id="50692-120">See all of my groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="50692-121">Tilldela en användare eller grupp till en enterprise-app</span><span class="sxs-lookup"><span data-stu-id="50692-121">Assign a user or group to an enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)
* [<span data-ttu-id="50692-122">Ta bort en användare eller grupp från en enterprise-app</span><span class="sxs-lookup"><span data-stu-id="50692-122">Remove a user or group assignment from an enterprise app</span></span>](active-directory-coreapps-remove-assignment-azure-portal.md)
* [<span data-ttu-id="50692-123">Inaktivera användarinloggningar för en enterprise-app</span><span class="sxs-lookup"><span data-stu-id="50692-123">Disable user sign-ins for an enterprise app</span></span>](active-directory-coreapps-disable-app-azure-portal.md)

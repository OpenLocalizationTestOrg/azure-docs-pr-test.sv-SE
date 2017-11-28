---
title: aaaChange hello namn eller logotyp av en enterprise-app i Azure Active Directory | Microsoft Docs
description: "Hur toochange hello namn eller logotyp för en anpassad enterprise-app i Azure Active Directory"
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
ms.openlocfilehash: b660e1f0f6c7ffd626e17e2e3399e7169f6e8bc8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-name-or-logo-of-an-enterprise-app-in-azure-active-directory"></a><span data-ttu-id="a5a63-103">Ändra hello namn eller en enterprise-app i Azure Active Directory-logotyp</span><span class="sxs-lookup"><span data-stu-id="a5a63-103">Change hello name or logo of an enterprise app in Azure Active Directory</span></span>
<span data-ttu-id="a5a63-104">Det är enkelt toochange hello namn eller logotyp för en anpassad företagsprogram i Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="a5a63-104">It's easy toochange hello name or logo for a custom enterprise application in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="a5a63-105">Du måste ha hello behörighet toomake ändringarna och du måste vara hello skapare av hello anpassad app.</span><span class="sxs-lookup"><span data-stu-id="a5a63-105">You must have hello appropriate permissions toomake these changes, and you must be hello creator of hello custom app.</span></span>

## <a name="how-do-i-change-an-enterprise-apps-name-or-logo"></a><span data-ttu-id="a5a63-106">Hur ändrar jag enterprise appens namn eller logotyp?</span><span class="sxs-lookup"><span data-stu-id="a5a63-106">How do I change an enterprise app's name or logo?</span></span>
1. <span data-ttu-id="a5a63-107">Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="a5a63-107">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="a5a63-108">Välj **fler tjänster**, ange **Azure Active Directory** i hello textruta och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a5a63-108">Select **More services**, enter **Azure Active Directory** in hello text box, and then select **Enter**.</span></span>
3. <span data-ttu-id="a5a63-109">På hello **Azure Active Directory - *directoryname***  bladet (det vill säga hello Azure AD bladet för hello-katalog som du hanterar) Välj **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="a5a63-109">On hello **Azure Active Directory - *directoryname*** blade (that is, hello Azure AD blade for hello directory you are managing), select **Enterprise applications**.</span></span>

    ![Öppna företagsappar](./media/active-directory-coreapps-change-app-logo-azure-portal/open-enterprise-apps.png)
4. <span data-ttu-id="a5a63-111">På hello **företagsprogram** bladet väljer **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a5a63-111">On hello **Enterprise applications** blade, select **All applications**.</span></span> <span data-ttu-id="a5a63-112">Visas en lista över hello-appar som du kan hantera.</span><span class="sxs-lookup"><span data-stu-id="a5a63-112">You'll see a list of hello apps you can manage.</span></span>
5. <span data-ttu-id="a5a63-113">På hello **företagsprogram - alla program** bladet väljer du en app.</span><span class="sxs-lookup"><span data-stu-id="a5a63-113">On hello **Enterprise applications - All applications** blade, select an app.</span></span>
6. <span data-ttu-id="a5a63-114">På hello ***appname*** bladet (det vill säga hello bladet med hello namn för valda hello-appen i hello rubrik) Välj **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="a5a63-114">On hello ***appname*** blade (that is, hello blade with hello name of hello selected app in hello title), select **Properties**.</span></span>

    ![Kommandot för att välja hello-egenskaper](./media/active-directory-coreapps-change-app-logo-azure-portal/select-app.png)
7. <span data-ttu-id="a5a63-116">På hello ***appname*** **-egenskaper** bladet Bläddra efter en fil toouse som en ny logotyp eller redigera hello appens namn eller båda.</span><span class="sxs-lookup"><span data-stu-id="a5a63-116">On hello ***appname*** **- Properties** blade, browse for a file toouse as a new logo, or edit hello app name, or both.</span></span>

    ![Ändra hello app logotyp eller nameproperties kommando](./media/active-directory-coreapps-change-app-logo-azure-portal/change-logo.png)
8. <span data-ttu-id="a5a63-118">Välj hello **spara** kommando.</span><span class="sxs-lookup"><span data-stu-id="a5a63-118">Select hello **Save** command.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a5a63-119">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a5a63-119">Next steps</span></span>
* [<span data-ttu-id="a5a63-120">Visa alla mina grupper</span><span class="sxs-lookup"><span data-stu-id="a5a63-120">See all of my groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="a5a63-121">Tilldela en användare eller grupp tooan enterprise app</span><span class="sxs-lookup"><span data-stu-id="a5a63-121">Assign a user or group tooan enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)
* [<span data-ttu-id="a5a63-122">Ta bort en användare eller grupp från en enterprise-app</span><span class="sxs-lookup"><span data-stu-id="a5a63-122">Remove a user or group assignment from an enterprise app</span></span>](active-directory-coreapps-remove-assignment-azure-portal.md)
* [<span data-ttu-id="a5a63-123">Inaktivera användarinloggningar för en enterprise-app</span><span class="sxs-lookup"><span data-stu-id="a5a63-123">Disable user sign-ins for an enterprise app</span></span>](active-directory-coreapps-disable-app-azure-portal.md)

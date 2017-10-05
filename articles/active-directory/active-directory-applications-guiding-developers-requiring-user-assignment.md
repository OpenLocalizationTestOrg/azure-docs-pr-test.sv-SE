---
title: "Kräver Användartilldelning - Azure AD | Microsoft Docs"
description: "Så här kräver Användartilldelning för Azure-program."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: 30b78cba-1e0f-472f-8314-f2250a9b91c3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: kgremban
robots: noindex
ms.openlocfilehash: 079b806c041a4a21e9350342867aee581c57bf45
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-ad-and-applications-require-user-assignment"></a><span data-ttu-id="cb625-103">Azure AD och program: kräver Användartilldelning</span><span class="sxs-lookup"><span data-stu-id="cb625-103">Azure AD and applications: Require user assignment</span></span>
## <a name="requiring-user-assignment"></a><span data-ttu-id="cb625-104">Kräver Användartilldelning</span><span class="sxs-lookup"><span data-stu-id="cb625-104">Requiring User Assignment</span></span>
1. <span data-ttu-id="cb625-105">Logga in på Azure-portalen med ett administratörskonto.</span><span class="sxs-lookup"><span data-stu-id="cb625-105">Log in to the Azure portal with an administrator account.</span></span>
2. <span data-ttu-id="cb625-106">Klicka på den **alla objekt** objekt på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="cb625-106">Click on the **All Items** item in the main menu.</span></span>
3. <span data-ttu-id="cb625-107">Välj det bibliotek som du använder för programmet.</span><span class="sxs-lookup"><span data-stu-id="cb625-107">Choose the directory you are using for the application.</span></span>
4. <span data-ttu-id="cb625-108">Klicka på den **program** fliken.</span><span class="sxs-lookup"><span data-stu-id="cb625-108">Click on the **APPLICATIONS** tab.</span></span>
5. <span data-ttu-id="cb625-109">Markera programmet i listan över program som är associerade med den här katalogen.</span><span class="sxs-lookup"><span data-stu-id="cb625-109">Select the application from the list of applications associated with this directory.</span></span>
6. <span data-ttu-id="cb625-110">Klicka på den **konfigurera** fliken.</span><span class="sxs-lookup"><span data-stu-id="cb625-110">Click the **CONFIGURE** tab.</span></span>
7. <span data-ttu-id="cb625-111">Ändra den **användaren tilldelning krävs för att Access-appen** växla till Ja.</span><span class="sxs-lookup"><span data-stu-id="cb625-111">Change the **User Assignment Required to Access App** toggle to Yes.</span></span>
8. <span data-ttu-id="cb625-112">Klicka på den **spara** längst ned på skärmen.</span><span class="sxs-lookup"><span data-stu-id="cb625-112">Click the **Save** button at the bottom of the screen.</span></span>

<span data-ttu-id="cb625-113">Nu behöver du tilldela användare och/eller grupper till programmet.</span><span class="sxs-lookup"><span data-stu-id="cb625-113">You will now have to assign users and/or groups to the application.</span></span> <span data-ttu-id="cb625-114">Se [tilldela användare till ett program](active-directory-applications-guiding-developers-assigning-users.md) och [tilldela grupper till ett program](active-directory-applications-guiding-developers-assigning-groups.md).</span><span class="sxs-lookup"><span data-stu-id="cb625-114">See [Assigning users to an application](active-directory-applications-guiding-developers-assigning-users.md) and [Assigning groups to an application](active-directory-applications-guiding-developers-assigning-groups.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="cb625-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cb625-115">Next Steps</span></span>
[!INCLUDE [active-directory-applications-guiding-developers-for-lob-applications-toc.md](../../includes/active-directory-applications-guiding-developers-for-lob-applications-toc.md)]

---
title: "Azure AD och program: tilldela användare till ett program | Microsoft Docs"
description: "Hur du implementerar Användartilldelning för Azure-program."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: 97ce69c1-4034-4e38-bd82-8caf984f6b98
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: kgremban
robots: noindex
ms.openlocfilehash: 29d63bd5781dc7ef9e84840dd4b1b70222cf6892
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-ad-and-applications-assigning-users-to-an-application"></a><span data-ttu-id="87e1a-103">Azure AD och program: tilldela användare till ett program</span><span class="sxs-lookup"><span data-stu-id="87e1a-103">Azure AD and Applications: Assigning Users to an Application</span></span>
<span data-ttu-id="87e1a-104">Innan du kan tilldela användare och grupper till ett program, måste du begära Användartilldelning.</span><span class="sxs-lookup"><span data-stu-id="87e1a-104">Before you can assign users and groups to an application, you must require user assignment.</span></span>  <span data-ttu-id="87e1a-105">Information om hur du kräver Användartilldelning finns i [kräver Användartilldelning](active-directory-applications-guiding-developers-requiring-user-assignment.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="87e1a-105">To learn how to require user assignment please see the [Requiring User Assignment](active-directory-applications-guiding-developers-requiring-user-assignment.md) article.</span></span>

## <a name="assigning-users-to-an-application"></a><span data-ttu-id="87e1a-106">Tilldela användare till ett program</span><span class="sxs-lookup"><span data-stu-id="87e1a-106">Assigning Users to an Application</span></span>
1. <span data-ttu-id="87e1a-107">Logga in på Azure-portalen med ett administratörskonto.</span><span class="sxs-lookup"><span data-stu-id="87e1a-107">Log in to the Azure portal with an administrator account.</span></span>
2. <span data-ttu-id="87e1a-108">Klicka på den **alla objekt** objekt på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="87e1a-108">Click on the **All Items** item in the main menu.</span></span>
3. <span data-ttu-id="87e1a-109">Välj det bibliotek som du använder för programmet.</span><span class="sxs-lookup"><span data-stu-id="87e1a-109">Choose the directory you are using for the application.</span></span>
4. <span data-ttu-id="87e1a-110">Klicka på den **program** fliken.</span><span class="sxs-lookup"><span data-stu-id="87e1a-110">Click on the **APPLICATIONS** tab.</span></span>
5. <span data-ttu-id="87e1a-111">Markera programmet i listan över program som är associerade med den här katalogen.</span><span class="sxs-lookup"><span data-stu-id="87e1a-111">Select the application from the list of applications associated with this directory.</span></span>
6. <span data-ttu-id="87e1a-112">Klicka på den **användare och grupper** fliken.</span><span class="sxs-lookup"><span data-stu-id="87e1a-112">Click the **USERS AND GROUPS** tab.</span></span>
7. <span data-ttu-id="87e1a-113">Markera de användare som du vill tilldela till programmet.</span><span class="sxs-lookup"><span data-stu-id="87e1a-113">Select the users you want to assign to the application.</span></span>
8. <span data-ttu-id="87e1a-114">Klicka på **TILLDELA**.</span><span class="sxs-lookup"><span data-stu-id="87e1a-114">Click **ASSIGN**.</span></span>
9. <span data-ttu-id="87e1a-115">Klicka på **Ja** när du tillfrågas.</span><span class="sxs-lookup"><span data-stu-id="87e1a-115">Click **yes** when prompted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="87e1a-116">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="87e1a-116">Next Steps</span></span>
[!INCLUDE [guiding-developers-for-lob-applications-toc.md](../../includes/active-directory-applications-guiding-developers-for-lob-applications-toc.md)]


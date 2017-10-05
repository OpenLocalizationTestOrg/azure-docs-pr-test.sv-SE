---
title: Tilldela till Azure AD-appar | Microsoft Docs
description: "Hur du implementerar grupptilldelning för Azure-program."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: 29b5ba89-a1c7-4f1f-a294-248a40106617
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: kgremban
ms.custom: H1Hack27Feb2017
robots: noindex
ms.openlocfilehash: e0b0b87a454db96747f024e81882fe83d62fdbe2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="assign-azure-active-directory-groups-to-an-application"></a><span data-ttu-id="376cd-103">Tilldela ett program med Azure Active Directory-grupper</span><span class="sxs-lookup"><span data-stu-id="376cd-103">Assign Azure Active Directory groups to an application</span></span>
<span data-ttu-id="376cd-104">Innan du kan tilldela användare och grupper till ett program, måste du begära Användartilldelning.</span><span class="sxs-lookup"><span data-stu-id="376cd-104">Before you can assign users and groups to an application, you must require user assignment.</span></span> <span data-ttu-id="376cd-105">Information om hur du kan kräva Användartilldelning finns i [kräver Användartilldelning](active-directory-applications-guiding-developers-requiring-user-assignment.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="376cd-105">To learn how to require user assignment, see the [Requiring User Assignment](active-directory-applications-guiding-developers-requiring-user-assignment.md) article.</span></span>

<span data-ttu-id="376cd-106">Den här artikeln förutsätter att du redan har skapat grupper i active directory som du använder för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="376cd-106">This article assumes that you have already created groups in the active directory you are using for this application.</span></span>

## <a name="assigning-groups-to-an-application"></a><span data-ttu-id="376cd-107">Tilldela grupper till ett program</span><span class="sxs-lookup"><span data-stu-id="376cd-107">Assigning Groups to an Application</span></span>
1. <span data-ttu-id="376cd-108">Logga in på Azure-portalen med ett administratörskonto.</span><span class="sxs-lookup"><span data-stu-id="376cd-108">Log in to the Azure portal with an administrator account.</span></span>
2. <span data-ttu-id="376cd-109">Klicka på den **alla objekt** objekt på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="376cd-109">Click on the **All Items** item in the main menu.</span></span>
3. <span data-ttu-id="376cd-110">Välj det bibliotek som du använder för programmet.</span><span class="sxs-lookup"><span data-stu-id="376cd-110">Choose the directory you are using for the application.</span></span>
4. <span data-ttu-id="376cd-111">Klicka på den **program** fliken.</span><span class="sxs-lookup"><span data-stu-id="376cd-111">Click on the **APPLICATIONS** tab.</span></span>
5. <span data-ttu-id="376cd-112">Markera programmet i listan över program som är associerade med den här katalogen.</span><span class="sxs-lookup"><span data-stu-id="376cd-112">Select the application from the list of applications associated with this directory.</span></span>
6. <span data-ttu-id="376cd-113">Klicka på den **användare och grupper** fliken.</span><span class="sxs-lookup"><span data-stu-id="376cd-113">Click the **USERS AND GROUPS** tab.</span></span>
7. <span data-ttu-id="376cd-114">Filtrera listan över grupper i active directory med hjälp av den **grupper** listrutan.</span><span class="sxs-lookup"><span data-stu-id="376cd-114">Filter the list of groups in your active directory by using the **Groups** dropdown list.</span></span>
8. <span data-ttu-id="376cd-115">Välj en grupp.</span><span class="sxs-lookup"><span data-stu-id="376cd-115">Select the group.</span></span>
9. <span data-ttu-id="376cd-116">Klicka på **TILLDELA**.</span><span class="sxs-lookup"><span data-stu-id="376cd-116">Click **ASSIGN**.</span></span>
10. <span data-ttu-id="376cd-117">Klicka på **Ja** när du tillfrågas.</span><span class="sxs-lookup"><span data-stu-id="376cd-117">Click **yes** when prompted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="376cd-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="376cd-118">Next Steps</span></span>
[!INCLUDE [active-directory-applications-guiding-developers-for-lob-applications-toc.md](../../includes/active-directory-applications-guiding-developers-for-lob-applications-toc.md)]

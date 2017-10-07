---
title: aaaAssign grupper tooAzure AD-appar | Microsoft Docs
description: "Hur tooimplement gruppera tilldelning för Azure-program."
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
ms.openlocfilehash: 086619df09c13bf259afc3128d45ed804b99e519
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="assign-azure-active-directory-groups-tooan-application"></a><span data-ttu-id="152c6-103">Tilldela Azure Active Directory-grupper tooan program</span><span class="sxs-lookup"><span data-stu-id="152c6-103">Assign Azure Active Directory groups tooan application</span></span>
<span data-ttu-id="152c6-104">Innan du kan tilldela användare och grupper tooan program, måste du begära Användartilldelning.</span><span class="sxs-lookup"><span data-stu-id="152c6-104">Before you can assign users and groups tooan application, you must require user assignment.</span></span> <span data-ttu-id="152c6-105">toolearn hur toorequire Användartilldelning finns hello [kräver Användartilldelning](active-directory-applications-guiding-developers-requiring-user-assignment.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="152c6-105">toolearn how toorequire user assignment, see hello [Requiring User Assignment](active-directory-applications-guiding-developers-requiring-user-assignment.md) article.</span></span>

<span data-ttu-id="152c6-106">Den här artikeln förutsätter att du redan har skapat grupper i hello active directory som du använder för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="152c6-106">This article assumes that you have already created groups in hello active directory you are using for this application.</span></span>

## <a name="assigning-groups-tooan-application"></a><span data-ttu-id="152c6-107">Tilldela grupper tooan program</span><span class="sxs-lookup"><span data-stu-id="152c6-107">Assigning Groups tooan Application</span></span>
1. <span data-ttu-id="152c6-108">Logga in toohello Azure-portalen med ett administratörskonto.</span><span class="sxs-lookup"><span data-stu-id="152c6-108">Log in toohello Azure portal with an administrator account.</span></span>
2. <span data-ttu-id="152c6-109">Klicka på hello **alla objekt** objekt i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="152c6-109">Click on hello **All Items** item in hello main menu.</span></span>
3. <span data-ttu-id="152c6-110">Välja hello-katalog som du använder för hello program.</span><span class="sxs-lookup"><span data-stu-id="152c6-110">Choose hello directory you are using for hello application.</span></span>
4. <span data-ttu-id="152c6-111">Klicka på hello **program** fliken.</span><span class="sxs-lookup"><span data-stu-id="152c6-111">Click on hello **APPLICATIONS** tab.</span></span>
5. <span data-ttu-id="152c6-112">Välj hello programmet hello listan över program som är associerade med den här katalogen.</span><span class="sxs-lookup"><span data-stu-id="152c6-112">Select hello application from hello list of applications associated with this directory.</span></span>
6. <span data-ttu-id="152c6-113">Klicka på hello **användare och grupper** fliken.</span><span class="sxs-lookup"><span data-stu-id="152c6-113">Click hello **USERS AND GROUPS** tab.</span></span>
7. <span data-ttu-id="152c6-114">Filter hello listan över grupper i active directory med hjälp av hello **grupper** listrutan.</span><span class="sxs-lookup"><span data-stu-id="152c6-114">Filter hello list of groups in your active directory by using hello **Groups** dropdown list.</span></span>
8. <span data-ttu-id="152c6-115">Välj hello grupp.</span><span class="sxs-lookup"><span data-stu-id="152c6-115">Select hello group.</span></span>
9. <span data-ttu-id="152c6-116">Klicka på **TILLDELA**.</span><span class="sxs-lookup"><span data-stu-id="152c6-116">Click **ASSIGN**.</span></span>
10. <span data-ttu-id="152c6-117">Klicka på **Ja** när du tillfrågas.</span><span class="sxs-lookup"><span data-stu-id="152c6-117">Click **yes** when prompted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="152c6-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="152c6-118">Next Steps</span></span>
[!INCLUDE [active-directory-applications-guiding-developers-for-lob-applications-toc.md](../../includes/active-directory-applications-guiding-developers-for-lob-applications-toc.md)]

---
title: "aaaManage server-administratörer i Azure Analysis Services | Microsoft Docs"
description: "Lär dig hur toomanage server-administratörer för en Analysis Services-server i Azure."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: e04387e48e9b9483c382ee5cc9fd65f8331fb2a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-server-administrators"></a><span data-ttu-id="57288-103">Hantera server-administratörer</span><span class="sxs-lookup"><span data-stu-id="57288-103">Manage server administrators</span></span>
<span data-ttu-id="57288-104">Server-administratörer måste vara en giltig användare eller grupp i hello Azure Active Directory (Azure AD) för hello klient i vilka hello-servern finns.</span><span class="sxs-lookup"><span data-stu-id="57288-104">Server administrators must be a valid user or group in hello Azure Active Directory (Azure AD) for hello tenant in which hello server resides.</span></span> <span data-ttu-id="57288-105">Du kan använda **Analysis Services-administratörer** i hello kontrollen bladet för din server i Azure-portalen eller serveregenskaper i SSMS toomanage serveradministratörer.</span><span class="sxs-lookup"><span data-stu-id="57288-105">You can use **Analysis Services Admins** in hello control blade for your server in Azure portal, or Server Properties in SSMS toomanage server administrators.</span></span> 

## <a name="tooadd-server-administrators-by-using-azure-portal"></a><span data-ttu-id="57288-106">tooadd serveradministratörer med hjälp av Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="57288-106">tooadd server administrators by using Azure portal</span></span>
1. <span data-ttu-id="57288-107">I hello kontrollen bladet för din server, klickar du på **Analysis Services-administratörer**.</span><span class="sxs-lookup"><span data-stu-id="57288-107">In hello control blade for your server, click **Analysis Services Admins**.</span></span>
2. <span data-ttu-id="57288-108">I hello  **\<servername >-Analysis Services-administratörer** bladet, klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="57288-108">In hello **\<servername> - Analysis Services Admins** blade, click **Add**.</span></span>
3. <span data-ttu-id="57288-109">I hello **lägga till serveradministratörer** bladet välj användarkonton från din Azure AD eller bjuda in externa användare efter e-postadress.</span><span class="sxs-lookup"><span data-stu-id="57288-109">In hello **Add Server Administrators** blade, select user accounts from your Azure AD or invite external users by email address.</span></span>

    ![Server-administratörer i Azure-portalen](./media/analysis-services-server-admins/aas-manage-users-admins.png)

## <a name="tooadd-server-administrators-by-using-ssms"></a><span data-ttu-id="57288-111">tooadd serveradministratörer med hjälp av SSMS</span><span class="sxs-lookup"><span data-stu-id="57288-111">tooadd server administrators by using SSMS</span></span>
1. <span data-ttu-id="57288-112">Högerklicka på hello server > **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="57288-112">Right-click hello server > **Properties**.</span></span>
2. <span data-ttu-id="57288-113">I **Analysis Server-egenskaper**, klickar du på **säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="57288-113">In **Analysis Server Properties**, click **Security**.</span></span>
3. <span data-ttu-id="57288-114">Klicka på **Lägg till**, och sedan ange hello e-postadress för en användare eller grupp i din Azure AD.</span><span class="sxs-lookup"><span data-stu-id="57288-114">Click **Add**, and then enter hello email address for a user or group in your Azure AD.</span></span>
   
    ![Lägg till server-administratörer i SSMS](./media/analysis-services-server-admins/aas-manage-users-ssms.png)

## <a name="next-steps"></a><span data-ttu-id="57288-116">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="57288-116">Next steps</span></span> 
[<span data-ttu-id="57288-117">Autentisering och användarbehörigheter</span><span class="sxs-lookup"><span data-stu-id="57288-117">Authentication and user permissions</span></span>](analysis-services-manage-users.md)  
[<span data-ttu-id="57288-118">Hantera databasroller och användare</span><span class="sxs-lookup"><span data-stu-id="57288-118">Manage database roles and users</span></span>](analysis-services-database-users.md)  
[<span data-ttu-id="57288-119">Rollbaserad åtkomstkontroll</span><span class="sxs-lookup"><span data-stu-id="57288-119">Role-Based Access Control</span></span>](../active-directory/role-based-access-control-what-is.md)  


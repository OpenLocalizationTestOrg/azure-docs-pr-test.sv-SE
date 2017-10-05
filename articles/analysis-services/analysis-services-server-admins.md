---
title: "Hantera server-administratörer i Azure Analysis Services | Microsoft Docs"
description: "Lär dig hur du hanterar server-administratörer för en Analysis Services-server i Azure."
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
ms.openlocfilehash: a1b58125dafdf73f245b6a8cd0f4917513b22ea9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="manage-server-administrators"></a><span data-ttu-id="31b12-103">Hantera server-administratörer</span><span class="sxs-lookup"><span data-stu-id="31b12-103">Manage server administrators</span></span>
<span data-ttu-id="31b12-104">Server-administratörer måste vara en giltig användare eller grupp i Azure Active Directory (Azure AD) för klienten som servern finns.</span><span class="sxs-lookup"><span data-stu-id="31b12-104">Server administrators must be a valid user or group in the Azure Active Directory (Azure AD) for the tenant in which the server resides.</span></span> <span data-ttu-id="31b12-105">Du kan använda **Analysis Services-administratörer** i bladet kontroll för din server i Azure-portalen eller serveregenskaper i SSMS att hantera serveradministratörer.</span><span class="sxs-lookup"><span data-stu-id="31b12-105">You can use **Analysis Services Admins** in the control blade for your server in Azure portal, or Server Properties in SSMS to manage server administrators.</span></span> 

## <a name="to-add-server-administrators-by-using-azure-portal"></a><span data-ttu-id="31b12-106">Lägg till server-administratörer med Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="31b12-106">To add server administrators by using Azure portal</span></span>
1. <span data-ttu-id="31b12-107">I bladet kontroll för servern klickar du på **Analysis Services-administratörer**.</span><span class="sxs-lookup"><span data-stu-id="31b12-107">In the control blade for your server, click **Analysis Services Admins**.</span></span>
2. <span data-ttu-id="31b12-108">I den  **\<servername >-Analysis Services-administratörer** bladet, klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="31b12-108">In the **\<servername> - Analysis Services Admins** blade, click **Add**.</span></span>
3. <span data-ttu-id="31b12-109">I den **lägga till serveradministratörer** bladet välj användarkonton från din Azure AD eller bjuda in externa användare efter e-postadress.</span><span class="sxs-lookup"><span data-stu-id="31b12-109">In the **Add Server Administrators** blade, select user accounts from your Azure AD or invite external users by email address.</span></span>

    ![Server-administratörer i Azure-portalen](./media/analysis-services-server-admins/aas-manage-users-admins.png)

## <a name="to-add-server-administrators-by-using-ssms"></a><span data-ttu-id="31b12-111">Lägg till server-administratörer med SSMS</span><span class="sxs-lookup"><span data-stu-id="31b12-111">To add server administrators by using SSMS</span></span>
1. <span data-ttu-id="31b12-112">Högerklicka på servern > **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="31b12-112">Right-click the server > **Properties**.</span></span>
2. <span data-ttu-id="31b12-113">I **Analysis Server-egenskaper**, klickar du på **säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="31b12-113">In **Analysis Server Properties**, click **Security**.</span></span>
3. <span data-ttu-id="31b12-114">Klicka på **Lägg till**, och sedan ange den e-postadressen för en användare eller grupp i din Azure AD.</span><span class="sxs-lookup"><span data-stu-id="31b12-114">Click **Add**, and then enter the email address for a user or group in your Azure AD.</span></span>
   
    ![Lägg till server-administratörer i SSMS](./media/analysis-services-server-admins/aas-manage-users-ssms.png)

## <a name="next-steps"></a><span data-ttu-id="31b12-116">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="31b12-116">Next steps</span></span> 
[<span data-ttu-id="31b12-117">Autentisering och användarbehörigheter</span><span class="sxs-lookup"><span data-stu-id="31b12-117">Authentication and user permissions</span></span>](analysis-services-manage-users.md)  
[<span data-ttu-id="31b12-118">Hantera databasroller och användare</span><span class="sxs-lookup"><span data-stu-id="31b12-118">Manage database roles and users</span></span>](analysis-services-database-users.md)  
[<span data-ttu-id="31b12-119">Rollbaserad åtkomstkontroll</span><span class="sxs-lookup"><span data-stu-id="31b12-119">Role-Based Access Control</span></span>](../active-directory/role-based-access-control-what-is.md)  


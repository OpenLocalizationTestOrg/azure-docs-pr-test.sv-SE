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
# <a name="manage-server-administrators"></a>Hantera server-administratörer
Server-administratörer måste vara en giltig användare eller grupp i hello Azure Active Directory (Azure AD) för hello klient i vilka hello-servern finns. Du kan använda **Analysis Services-administratörer** i hello kontrollen bladet för din server i Azure-portalen eller serveregenskaper i SSMS toomanage serveradministratörer. 

## <a name="tooadd-server-administrators-by-using-azure-portal"></a>tooadd serveradministratörer med hjälp av Azure-portalen
1. I hello kontrollen bladet för din server, klickar du på **Analysis Services-administratörer**.
2. I hello  **\<servername >-Analysis Services-administratörer** bladet, klickar du på **Lägg till**.
3. I hello **lägga till serveradministratörer** bladet välj användarkonton från din Azure AD eller bjuda in externa användare efter e-postadress.

    ![Server-administratörer i Azure-portalen](./media/analysis-services-server-admins/aas-manage-users-admins.png)

## <a name="tooadd-server-administrators-by-using-ssms"></a>tooadd serveradministratörer med hjälp av SSMS
1. Högerklicka på hello server > **egenskaper**.
2. I **Analysis Server-egenskaper**, klickar du på **säkerhet**.
3. Klicka på **Lägg till**, och sedan ange hello e-postadress för en användare eller grupp i din Azure AD.
   
    ![Lägg till server-administratörer i SSMS](./media/analysis-services-server-admins/aas-manage-users-ssms.png)

## <a name="next-steps"></a>Nästa steg 
[Autentisering och användarbehörigheter](analysis-services-manage-users.md)  
[Hantera databasroller och användare](analysis-services-database-users.md)  
[Rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-what-is.md)  


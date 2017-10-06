---
title: aaaCreate Analysis Services-server i Azure | Microsoft Docs
description: "Lär dig hur toocreate Analysis Services-server-instansen i Azure."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 7f560216-8a9a-4d06-852e-48cf24deab19
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: 3668f659039f79f3dd71498d1066e8682bf33228
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-analysis-services-server-in-azure-portal"></a>Skapa en Azure Analysis Services-server i Azure-portalen
Den här artikeln vägleder dig genom att skapa en resurs för Analysis Services-server i din Azure-prenumeration.

## <a name="before-you-begin"></a>Innan du börjar
toocomplete Snabbstart, behöver du:

* **Azure-prenumeration**: Besök [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/offers/ms-azr-0044p/) toocreate ett konto.
* **Azure Active Directory**: din prenumeration måste vara kopplad till en Azure Active Directory-klient. Och du behöver toobe tooAzure inloggad med ett konto i den Azure Active Directory. Microsoft-konton stöds inte. Det finns fler toolearn [autentisering och användarbehörigheter](analysis-services-manage-users.md).
* **Resursgruppen**: använda en resursgrupp som du redan har eller [skapa en ny](../azure-resource-manager/resource-group-overview.md).

> [!NOTE]
> Att skapa en server kan resultera i en ny fakturerbar tjänst. Det finns fler toolearn [priser för Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/).
> 
> 

## <a name="toocreate-a-server-in-azure-portal"></a>toocreate en server i Azure-portalen
1. Logga in toohello [Azure-portalen](https://portal.azure.com).  
2. Klicka på **+ ny** > **Data + analys** > **Analysis Services**.
3. I hello **Analysis Services** bladet, Fyll i hello krävs fält och tryck sedan på **skapa**.
   
    ![Skapa server](./media/analysis-services-create-server/aas-create-server-blade.png)
   
   * **Servernamnet**: Skriv ett unikt namn som används tooreference hello-server.
   * **Prenumerationen**: Välj hello-prenumeration som den här servern växlar till.
   * **Resursgruppen**: dessa behållare är utformad toohelp du hantera en samling Azure-resurser. Det finns fler toolearn [resursgrupper](../azure-resource-manager/resource-group-overview.md).
   * **Plats**: den här Azure datacenter plats värdar hello server. Välj en plats närmast största användarbasen.
   * **Prisnivån**: Välj en prisnivå. Tabellmodeller in too400 GB stöds. Det finns fler toolearn [priser för Azure Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/).
4. Klicka på **Skapa**.

Skapa tar vanligtvis under en minut; ofta bara några sekunder. Om du har valt **lägga till tooPortal**, navigera tooyour portal toosee den nya servern. Eller navigera för**fler tjänster** > **Analysis Services** toosee om din server är klar.

 ![Instrumentpanel](./media/analysis-services-create-server/aas-create-server-dashboard.png)


## <a name="next-steps"></a>Nästa steg
När du har skapat din server, kan du [distribuera en modell](analysis-services-deploy.md) tooit med SSDT eller med SSMS.

Om en modell som du distribuerar tooyour server ansluter tooon lokala datakällor, behöver du tooinstall en [lokala datagateway](analysis-services-gateway.md) på en dator i nätverket.


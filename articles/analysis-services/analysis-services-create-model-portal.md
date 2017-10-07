---
title: "aaaCreate en tabellmodell genom att använda hello Azure Analysis Services Web designer | Microsoft Docs"
description: "Lär dig hur toocreate en tabellmodell med hjälp av Azure Analysis Services hello Webbdesigner i Azure-portalen."
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
ms.date: 08/21/2017
ms.author: owend
ms.openlocfilehash: a37b326b76c84fc3a4300827bc1c8706b0584701
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-model-in-azure-portal"></a>Skapa en modell i Azure-portalen

hello Azure Analysis Services web designer (förhandsversion)-funktionen i Azure-portalen innehåller ett snabbt och enkelt sätt toocreate och redigera tabellmodeller och fråga modellen data direkt i webbläsaren. 

Kom ihåg, hello Webbdesigner är **preview**. När nya funktioner läggs alla hello tiden, i förhandsvisningen är funktionerna begränsade. För mer avancerade modellen utveckling och testning är det bästa toouse Visual Studio (SSDT) och SQL Server Management Studio (SSMS).

## <a name="prerequisites"></a>Krav

- En Azure Analysis Services-server på hello Standard eller utvecklare nivå. Nya modeller som skapats med hjälp av hello Webbdesigner är DirectQuery, stöds endast av dessa nivåer.
- Azure SQL Database, Azure SQL Data Warehouse eller Power BI Desktop (.pbix)-filen som en datakälla. Nya modeller som skapats från Power BI Desktop filer stöd Azure SQL Database, Azure SQL Data Warehouse, Oracle och Teradata-datakällor.
- Ett SQL Server-konto och lösenord för att ansluta tooAzure SQL-databas eller Azure SQL Data Warehouse-datakällor.

## <a name="toocreate-a-new-tabular-model"></a>toocreate en ny tabellmodell

1. I serverns **översikt** bladet > **Webbdesigner**, klickar du på **öppna**.

    ![Skapa en modell i Azure-portalen](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. I **Webbdesigner** > **modeller**, klickar du på **+ Lägg till**.

    ![Skapa en modell i Azure-portalen](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. I **ny modell**, Skriv ett namn och välj sedan en datakälla.

    ![Dialogrutan ny modell i Azure-portalen](./media/analysis-services-create-model-portal/aas-create-portal-new-model.png)

4. I **Anslut**, ange hello anslutningsegenskaper. Användarnamn och lösenord måste vara en SQL Server-konto.

     ![Ansluta dialogrutan i Azure-portalen](./media/analysis-services-create-model-portal/aas-create-portal-connect.png)

5. I **tabeller och vyer**, Välj hello tabeller tooinclude i din modell och klicka sedan på **skapa**. Är att skapa relationer automatiskt mellan tabeller med ett nyckelpar.

     ![Välj tabeller och vyer](./media/analysis-services-create-model-portal/aas-create-portal-tables.png)

Din nya modell visas i webbläsaren. Här kan göra du följande:   

- Fråga modelldata genom att dra fält toohello Frågedesigner och lägga till filter.
- Skapa nya åtgärder i tabeller.
- Redigera modellmetadata med hello json-redigerare.
- Öppna hello modellen i Visual Studio (SSDT), Power BI Desktop eller Excel.

![Välj tabeller och vyer](./media/analysis-services-create-model-portal/aas-create-portal-query.png)

> [!NOTE]
> När du redigerar modellmetadata eller skapa nya åtgärder i webbläsaren, sparar du dessa ändringar tooyour modellen i Azure. Om du arbetar också på din modell i SSDT, Power BI Desktop eller Excel, kan du få synkroniserad din modell.


## <a name="next-steps"></a>Nästa steg 
[Hantera databasroller och användare](analysis-services-database-users.md)  
[Anslut till Excel](analysis-services-connect-excel.md)  



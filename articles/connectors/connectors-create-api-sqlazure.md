---
title: aaaAdd hello Azure SQL Database connector i dina Logic Apps | Microsoft Docs
description: "Översikt över Azure SQL Database-anslutningen med REST API-parametrar"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: d8a319d0-e4df-40cf-88f0-29a6158c898c
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: a9ca0f446d05dc00f310a908eee8d50e41fcd82b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-azure-sql-database-connector"></a>Kom igång med hello Azure SQL Database-koppling
Använd hello Azure SQL Database-anslutningen för att skapa arbetsflöden för din organisation som hanterar data i tabellerna. 

Med SQL-databas måste du:

* Skapa ditt arbetsflöde genom att lägga till en ny databas för kunden tooa kunder eller uppdatera en ordning i en order-databas.
* Använd åtgärder tooget en rad med data, infoga en ny rad och även ta bort. Till exempel när en post har skapats i Dynamics CRM Online (en utlösare), sedan infoga en rad i en Azure SQL Database (en åtgärd). 

Det här avsnittet visar hur toouse hello SQL Database-kopplingen i en logikapp och även visar hello åtgärder.

toolearn mer om Logic Apps finns [vad är logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) och [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-tooazure-sql-database"></a>Ansluta tooAzure SQL-databas
Innan din logikapp får åtkomst till alla tjänster måste du först skapa en *anslutning* toohello service. En anslutning kan du ansluta en logikapp och en annan tjänst. Till exempel tooconnect tooSQL databasen du först skapa en SQL-databas *anslutning*. toocreate en anslutning, ange hello autentiseringsuppgifter som du vanligtvis använder tooaccess hello-tjänsten som du ansluter till. Ange så SQL autentiseringsuppgifter toocreate hello databasanslutningen i SQL-databas. 

#### <a name="create-hello-connection"></a>Skapa hello-anslutning
> [!INCLUDE [Create hello connection tooSQL Azure](../../includes/connectors-create-api-sqlazure.md)]
> 
> 

## <a name="use-a-trigger"></a>Använda en utlösare
Den här anslutningen har inte utlösare. Använda andra utlösare toostart hello logikapp, till exempel en upprepning utlösare, en HTTP-Webhook-utlösare, utlösare som är tillgängliga med övriga kopplingar och mycket mer. [Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md) innehåller ett exempel.

## <a name="use-an-action"></a>Använda en åtgärd
En åtgärd är en åtgärd som utförs av hello arbetsflöde som definierats i en logikapp. [Mer information om åtgärder](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

1. Välj hello plustecken. Du ser flera alternativ: **lägga till en åtgärd**, **Lägg till ett villkor**, eller en av hello **mer** alternativ.
   
    ![](./media/connectors-create-api-sqlazure/add-action.png)
2. Välj **lägga till en åtgärd**.
3. Skriv ”sql” tooget en lista över alla tillgängliga åtgärder för hello hello i textrutan.
   
    ![](./media/connectors-create-api-sqlazure/sql-1.png) 
4. I vårt exempel väljer **SQL Server - Get-raden**. Om det finns redan en anslutning, och välj sedan hello **tabellnamn** från hello nedrullningsbara listan, och ange hello **rad-ID** du vill tooreturn.
   
    ![](./media/connectors-create-api-sqlazure/sample-table.png)
   
    Om du uppmanas att ange anslutningsinformation för hello ange hello information toocreate hello anslutning. [Skapa hello anslutning](connectors-create-api-sqlazure.md#create-the-connection) i det här avsnittet beskriver dessa egenskaper. 
   
   > [!NOTE]
   > I det här exemplet returnerar vi en rad från en tabell. toosee hello data i den här raden Lägg till en annan åtgärd som skapar en fil med hello fält från hello tabell. Till exempel lägga till en åtgärd som OneDrive som använder hello FirstName och LastName fält toocreate en ny fil i hello cloud storage-konto. 
   > 
   > 
5. **Spara** dina ändringar (övre vänstra hörnet av hello verktygsfältet). Din logikapp sparas och aktiveras automatiskt.

## <a name="connector-specific-details"></a>Connector-specifik information

Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/sql/). 

## <a name="next-steps"></a>Nästa steg
[Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md). Utforska hello andra tillgängliga kopplingar i Logic Apps på vår [API: er listan](apis-list.md).


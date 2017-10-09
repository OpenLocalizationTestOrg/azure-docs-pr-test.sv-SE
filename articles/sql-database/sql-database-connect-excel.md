---
title: aaaConnect Excel tooSQL databasen | Microsoft Docs
description: "Lär dig hur tooconnect Microsoft Excel tooAzure SQL-databas i hello molnet. Importera data till Excel för rapportering och dataundersökning."
services: sql-database
keywords: ansluta excel toosql, importera data tooexcel
documentationcenter: 
author: joseidz
manager: jhubbard
editor: 
ms.assetid: 906924bc-2707-48d3-bac6-397976a0409d
ms.service: sql-database
ms.custom: develop apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2017
ms.author: jhubbard
ms.openlocfilehash: 0048849432023145bd1009d45b6d9b64a9c7ac3c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-excel-tooan-azure-sql-database-and-create-a-report"></a>Ansluta Excel tooan Azure SQL-databas och skapa en rapport

Ansluta Excel tooa SQL-databas i molnet hello och importera data och skapa tabeller och diagram baserat på värdena i hello-databasen. I den här kursen ska du ställa in hello anslutningen mellan Excel och en databastabell, spara hello-filen som lagrar data och hello anslutningsinformationen för Excel och skapa ett pivotdiagram från hello databasvärden.

Du behöver en SQL-databas i Azure innan du börjar. Om du inte har någon [skapa din första SQL-databas](sql-database-get-started-portal.md) tooget en databas med exempeldata på några minuter. I den här artikeln, ska du importera exempeldata till Excel från artikeln, men du kan följa liknande steg med dina egna data.

Du kommer också behöva en kopia av Excel. Den här artikeln använder [Microsoft Excel 2016](https://products.office.com/).

## <a name="connect-excel-tooa-sql-database-and-create-an-odc-file"></a>Anslut Excel tooa SQL-databas och skapa en odc-fil
1. tooconnect Excel tooSQL databas, öppnar du Excel och sedan skapa en ny arbetsbok eller öppna en befintlig Excel-arbetsbok.
2. I hello menyraden överst hello på hello-sidan klickar du på **Data**, klickar du på **från andra källor**, och klicka sedan på **från SQL Server**.
   
   ![Välj datakälla: Anslut Excel tooSQL-databas.](./media/sql-database-connect-excel/excel_data_source.png)
   
   Hej Dataanslutningsguiden öppnas.
3. I hello **ansluta tooDatabase Server** dialogrutan typen hello SQL Database **servernamn** du vill att tooconnect tooin hello formuläret <*servername* > **. database.windows.net**. Till exempel **adworkserver.database.windows.net**.
4. Under **inloggningsuppgifter**, klickar du på **Använd hello följande användarnamn och lösenord**, typen hello **användarnamn** och **lösenord** du ställt in för Hej SQL Database-server när du skapade den och klicka sedan på **nästa**.
   
   ![Skriv hello server servernamn och inloggningsuppgifter](./media/sql-database-connect-excel/connect-to-server.png)
   
   > [!TIP]
   > Beroende på din nätverksmiljö, kanske du inte kan tooconnect eller hello anslutning kan gå förlorade om hello SQL Database-server som inte tillåter trafik från klientens IP-adress. Gå toohello [Azure-portalen](https://portal.azure.com/), klicka på SQL-servrar, servern, klicka på brandvägg under inställningar och Lägg till klientens IP-adress. Se [hur tooconfigure brandväggsinställningar](sql-database-configure-firewall-settings.md) mer information.
   > 
   > 
5. I hello **Välj databas och tabell** dialogrutan, Välj hello databas du vill toowork med hello listan och klicka på hello tabeller eller vyer som du vill toowork med (vi valde **vGetAllCategories**), och sedan Klicka på **nästa**.
   
    ![Välj en databas och en tabell.](./media/sql-database-connect-excel/select-database-and-table.png)
   
    Hej **Spara dataanslutningsfil och slutför** öppnas, där du kan ange information om hello filen Office-databasanslutningar (*.odc) som används av Excel. Du kan lämna hello standardvärden eller anpassa dina val.
6. Du kan lämna hello standard, men Observera hello **filnamn** särskilt. En **beskrivning**, **eget namn**, och **sökord** hjälper dig och andra användare att komma ihåg vad du ansluter tooand hitta hello-anslutning. Klicka på **alltid försöker toouse informationen filen toorefresh** om du vill att anslutningsinformation lagras i hello odc-filen så att den kan uppdatera när du ansluter tooit och klicka sedan på **Slutför**.
   
    ![Spara en odc-fil](./media/sql-database-connect-excel/save-odc-file.png)
   
    Hej **dataimport** dialogrutan visas.

## <a name="import-hello-data-into-excel-and-create-a-pivot-chart"></a>Importera hello data till Excel och skapa ett pivotdiagram
Nu när du har skapat hello anslutning och skapade hello med data och anslutningsinformation, läser du tooimport hello data.

1. I hello **importera Data** dialogrutan hello-alternativ som du vill använda för att presentera dina data i hello kalkylblad och klicka sedan på **OK**. Vi valde **PivotChart**. Du kan också välja toocreate en **nytt kalkylblad** eller för**lägga till den här data tooa datamodellen**. Mer information om datamodeller finns i [Skapa en datamodell i Excel](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B). Klicka på **egenskaper** tooexplore information om hello odc-filen som du skapade i hello föregående steg och toochoose alternativ för datauppdatering hello.
   
    ![Välja hello dataformat i Excel](./media/sql-database-connect-excel/import-data.png)
   
    hello kalkylbladet har nu en tom pivottabell och diagram.
2. Under **PivotTable-Fields**, Välj alla hello kryssrutorna för hello fält som du vill tooview.
   
    ![Konfigurera databasrapporten.](./media/sql-database-connect-excel/power-pivot-results.png)

> [!TIP]
> Om du vill tooconnect andra Excel-arbetsböcker och kalkylblad toohello databasen, klickar du på **Data**, klickar du på **anslutningar**, klickar du på **Lägg till**, Välj hello-anslutning som du skapade hello listan och klicka sedan på **öppna**.
> ![Öppna en anslutning från en annan arbetsbok](./media/sql-database-connect-excel/open-from-another-workbook.png)
> 
> 

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[ansluta tooSQL databasen med SQL Server Management Studio](sql-database-connect-query-ssms.md) för avancerade frågor och analys.
* Lär dig mer om hello fördelarna med [elastiska pooler](sql-database-elastic-pool.md).
* Lär dig hur för[skapa ett webbprogram som ansluter tooSQL databasen på hello backend-](../app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).


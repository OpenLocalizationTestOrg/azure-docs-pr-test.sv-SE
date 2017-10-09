---
title: "Självstudie: Skapa en pipeline med Copy Wizard | Microsoft Docs"
description: "I kursen får skapa du ett Azure Data Factory-pipelinen med en Kopieringsaktiviteten med hello guiden Kopiera stöds av Data Factory"
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: b87afb8e-53b7-4e1b-905b-0343dd096198
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 567b89e7a54c245c134cd0674690e6f3499b46d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-data-factory-copy-wizard"></a>Självstudie: Skapa en pipeline med en kopieringsaktivitet med hjälp av Guiden Data Factory-kopia
> [!div class="op_single_selector"]
> * [Översikt och förutsättningar](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Guiden Kopiera](data-factory-copy-data-wizard-tutorial.md)
> * [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Azure Resource Manager-mall](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [.NET-API](data-factory-copy-activity-tutorial-using-dotnet-api.md)

De här självstudierna visar hur toouse hello **guiden Kopiera** toocopy data från Azure blob storage tooan Azure SQL-databas. 

hello Azure Data Factory **guiden Kopiera** kan du tooquickly skapar en pipeline för data som kopierar data från ett datalager för stöds källa data store tooa stöds mål. Därför rekommenderar vi att du använder guiden hello som ett första steg toocreate en exempel-pipeline för ditt scenario för flytt av data. En lista över datakällor som stöds som källor och mottagare finns i [datalager som stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats).  

Den här kursen visar hur toocreate ett Azure data factory, starta hello guiden Kopiera, gå igenom ett antal steg tooprovide information om ditt scenario för införandet/flytt av data. När du slutför stegen i guiden hello hello guiden skapar automatiskt en pipeline med en Kopieringsaktiviteten toocopy data från Azure blob storage tooan Azure SQL-databas. Se artikeln [Dataförflyttningsaktiviteter](data-factory-data-movement-activities.md) för information om kopieringsaktiviteten.

## <a name="prerequisites"></a>Krav
Slutföra förutsättningar som anges i hello [kursen översikt](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) artikel innan du utför den här kursen.

## <a name="create-data-factory"></a>Skapa en datafabrik
I det här steget kan du använda hello Azure portal toocreate ett Azure data factory med namnet **ADFTutorialDataFactory**.

1. Logga in för[Azure-portalen](https://portal.azure.com).
2. Klicka på **+ ny** hello övre vänstra hörnet och klicka på **Data + analys**, och klicka på **Data Factory**. 
   
   ![Nytt->DataFactory](./media/data-factory-copy-data-wizard-tutorial/new-data-factory-menu.png)
2. I hello **nya data factory** bladet:
   
   1. Ange **ADFTutorialDataFactory** för hello **namn**.
       hello namn i hello Azure data factory måste vara globalt unika. Om felmeddelandet hello: `Data factory name “ADFTutorialDataFactory” is not available`, ändra hello namn i hello data factory (till exempel yournameADFTutorialDataFactoryYYYYMMDD) och försök att skapa igen. Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för namnregler för Data Factory-artefakter.  
      
       ![Datafabriksnamnet är inte tillgängligt](./media/data-factory-copy-data-wizard-tutorial/getstarted-data-factory-not-available.png)    
   2. Välj din Azure-**prenumeration**.
   3. För resursgruppen, gör du något av följande hello: 
      
      - Välj **använda befintliga** tooselect en befintlig resursgrupp.
      - Välj **Skapa nytt** tooenter ett namn för en resursgrupp.
          
        Några av hello stegen i den här självstudiekursen förutsätts att du använder hello namn: **ADFTutorialResourceGroup** för hello resursgrupp. toolearn om resursgrupper finns [resursnamnet grupper toomanage resurserna i Azure](../azure-resource-manager/resource-group-overview.md).
   4. Välj en **plats** för hello data factory.
   5. Välj **PIN-kod toodashboard** kryssrutan längst hello hello-bladet.  
   6. Klicka på **Skapa**.
      
       ![Bladet Ny datafabrik](media/data-factory-copy-data-wizard-tutorial/new-data-factory-blade.png)            
3. När hello har skapats visas hello **Data Factory** bladet som visas i följande bild hello:
   
   ![Datafabrikens startsida](./media/data-factory-copy-data-wizard-tutorial/getstarted-data-factory-home-page.png)

## <a name="launch-copy-wizard"></a>Använda guiden Kopiera
1. På hello Data Factory-bladet, klickar du på **kopiera data [FÖRHANDSGRANSKNING]** toolaunch hello **guiden Kopiera**. 
   
   > [!NOTE]
   > Om du ser att hello webbläsare har ”auktorisera...”, inaktivera/avmarkera **blockerar cookies från tredje part och platsdata** inställningen i hello webbläsarinställningar (eller) Håll den är aktiverad och skapa ett undantag för  **login.microsoftonline.com** och försök sedan starta hello guiden igen.
2. I hello **egenskaper** sidan:
   
   1. Ange **CopyFromBlobToAzureSql** som **aktivitetsnamn**
   2. Ange en **beskrivning** (valfritt).
   3. Ändra hello **startdatum tid** och hello **sluttiden datum** så att hello slutdatum är inställt tootoday starta datum toofive dagar tidigare.  
   4. Klicka på **Nästa**.  
      
      ![Verktyget Kopiera – sidan Egenskaper](./media/data-factory-copy-data-wizard-tutorial/copy-tool-properties-page.png) 
3. På hello **källa datalagret** klickar du på **Azure Blob Storage** panelen. Du kan använda det här datalagret för sidan toospecify hello källa för hello kopiera aktivitet. 
   
    ![Verktyget Kopiera – sidan Källans datalager](./media/data-factory-copy-data-wizard-tutorial/copy-tool-source-data-store-page.png)
4. På hello **ange hello Azure Blob storage-konto** sidan:
   
   1. Ange **AzureStorageLinkedService** som **Namn på länkad tjänst**.
   2. Kontrollera att alternativet **Från Azure-prenumerationer** har valts för **Metod för kontoval**.
   3. Välj din Azure-**prenumeration**.  
   4. Välj en **Azure storage-konto** från hello lista över Azure storage-konton tillgängliga i hello valda prenumerationen. Du kan också välja tooenter lagringsutrymmet kontoinställningar manuellt genom att välja **ange manuellt** alternativ för hello **konto urvalsmetod**, och klicka sedan på **nästa**. 
      
      ![Kopiera verktyget - Ange hello Azure Blob storage-konto](./media/data-factory-copy-data-wizard-tutorial/copy-tool-specify-azure-blob-storage-account.png)
5. På **Välj hello inkommande fil eller mapp** sidan:
   
   1. Dubbelklicka på **adftutorial** (mapp).
   2. Välj **emp.txt** och klicka på **Välj**
      
      ![Kopiera verktyget – Välj hello inkommande fil eller mapp](./media/data-factory-copy-data-wizard-tutorial/copy-tool-choose-input-file-or-folder.png)
6. På hello **Välj hello inkommande fil eller mapp** klickar du på **nästa**. Välj inte **Binär kopia**. 
   
    ![Kopiera verktyget – Välj hello inkommande fil eller mapp](./media/data-factory-copy-data-wizard-tutorial/chose-input-file-folder.png) 
7. På hello **filen formatinställningar** kan du se hello avgränsare och hello schemat som är detekterade hello guiden med hello fil-parsning. Du kan även ange hello avgränsare manuellt för hello kopiera guiden toostop auto-identifiering eller toooverride. Klicka på **nästa** när du granskar hello avgränsare och förhandsgranska data. 
   
    ![Verktyget Kopiera – Filformatinställningar](./media/data-factory-copy-data-wizard-tutorial/copy-tool-file-format-settings.png)  
8. Hello mål data lagra sidan, Välj **Azure SQL Database**, och klicka på **nästa**.
   
    ![Verktyget kopiera - Välj målarkiv](./media/data-factory-copy-data-wizard-tutorial/choose-destination-store.png)
9. På **ange hello Azure SQL database** sidan:
   
   1. Ange **AzureSqlLinkedService** för hello **anslutningsnamn** fältet.
   2. Kontrollera att alternativet **Från Azure-prenumerationer** har valts för **Metod för server/databasval**.
   3. Välj din Azure-**prenumeration**.  
   4. Välj **Servernamn** och **Databas**.
   5. Ange **Användarnamn** och **Lösenord**.
   6. Klicka på **Nästa**.  
      
      ![Verktyget Kopiera - Ange Azure SQL-databas](./media/data-factory-copy-data-wizard-tutorial/specify-azure-sql-database.png)
10. På hello **tabell mappning** väljer **tomma** för hello **mål** hello nedrullningsbara listan och klicka **NEDPIL** (valfritt) toosee hello schemat och toopreview hello data.
    
     ![Verktyget Kopiera – Tabellmappning](./media/data-factory-copy-data-wizard-tutorial/copy-tool-table-mapping-page.png) 
11. På hello **schemamappning** klickar du på **nästa**.
    
    ![Verktyget Kopiera - schemamappning](./media/data-factory-copy-data-wizard-tutorial/schema-mapping-page.png)
12. På hello **prestandainställningar** klickar du på **nästa**. 
    
    ![Verktyget Kopiera - prestandainställningar](./media/data-factory-copy-data-wizard-tutorial/performance-settings.png)
13. Granska informationen i hello **sammanfattning** och klicka på **Slutför**. hello skapas två länkade tjänster, två datamängder (indata och utdata) och en pipeline i hello data factory (från där startas hello guiden Kopiera). 
    
    ![Verktyget Kopiera - prestandainställningar](./media/data-factory-copy-data-wizard-tutorial/summary-page.png)

## <a name="launch-monitor-and-manage-application"></a>Starta övervakning och hantera program
1. På hello **distribution** klickar du på länken hello: `Click here toomonitor copy pipeline`.
   
   ![Verktyget Kopiera – Distributionen är klar](./media/data-factory-copy-data-wizard-tutorial/copy-tool-deployment-succeeded.png)  
2. hello-program startas i en separat flik i webbläsaren.   
   
   ![Övervakningsapp](./media/data-factory-copy-data-wizard-tutorial/monitoring-app.png)   
3. toosee hello senaste statusen för varje timme segment klickar du på **uppdatera** knapp i hello **aktivitet WINDOWS** lista längst ned hello. Du kan se fem aktivitet windows i fem dagar mellan start- och sluttider för hello pipelinen. hello listan uppdateras inte automatiskt, så du kanske behöver tooclick uppdatera några gånger innan du ser alla hello aktivitet windows hello klar att skriva. 
4. Välj en aktivitetsfönstret hello-listan. Se hello information om det i hello **aktivitet fönstret Explorer** på hello rätt.

    ![Information om aktivitetsfönstret](media/data-factory-copy-data-wizard-tutorial/activity-window-details.png)    

    Lägg märke till att hello datum 11, 12, 13, 14 och 15 i grön färg, vilket innebär att det redan har producerats hello dagliga utdata segment för dessa datum. Du också se färgkodning på hello pipeline och hello utdatamängden i hello diagramvyn. I hello föregående steg, se att två segment har redan skapats ett segment håller på att behandlas och hello andra två som väntar på toobe bearbetas (baserat på syntax i hello färger). 

    Mer information om hur du använder det här programmet finns i artikeln [Övervaka och hantera pipeline med övervakningsappen](data-factory-monitor-manage-app.md).

## <a name="next-steps"></a>Nästa steg
I den här kursen används Azure blob storage som ett datalager för källa och en Azure SQL-databas som ett dataarkiv som mål i en kopieringsåtgärd. hello innehåller följande tabell en lista över datakällor som stöds som källor och mål av hello kopieringsaktiviteten: 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

Mer information om fält och egenskaper som visas i guiden för hello kopiera för ett datalager klickar du på hello länk för hello-datalager i hello tabell. 

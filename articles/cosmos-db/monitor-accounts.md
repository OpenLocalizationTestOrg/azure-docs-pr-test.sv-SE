---
title: "aaaMonitor Azure Cosmos DB begäranden och lagring | Microsoft Docs"
description: "Lär dig hur toomonitor Azure Cosmos-DB-kontot för prestandavärden, till exempel begäranden och fel, och användningsstatistik, till exempel användningen av lagringsutrymme."
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 4c6a2e6f-6e78-48e3-8dc6-f4498b235a9e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: mimig
ms.openlocfilehash: aea029d10717236a573a080dab9d06d87f97f318
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-azure-cosmos-db-requests-usage-and-storage"></a>Övervaka Azure Cosmos DB begäranden, användning och lagring
Du kan övervaka dina Azure DB som Cosmos-konton i hello [Azure-portalen](https://portal.azure.com/). Båda prestandavärden, till exempel begäranden och fel och användningsstatistik, till exempel lagringsförbrukning, är tillgängliga för varje Azure DB som Cosmos-konto.

Mått kan granskas på hello nytt mått blad-kontoblad hello eller i Azure-Monitor.

## <a name="view-performance-metrics-on-hello-metrics-blade"></a>Visa prestandamått på hello mått blad
1. I hello [Azure-portalen](https://portal.azure.com/), klickar du på **fler tjänster**, rulla för**databaser**, klickar du på **Azure Cosmos DB**, och klicka sedan på hello namnet på hello Azure DB Cosmos-konto som du vill att tooview prestandamått.
2. Hello resurs menyn under **övervakning**, klickar du på **mått**.

hello mått blad öppnas och du kan välja hello samlingen tooreview. Du kan granska mått för tillgänglighet, begäranden, genomflöde och lagring och jämföra dem toohello Cosmos-SLA Azure DB.

## <a name="view-performance-metrics-by-using-azure-monitoring"></a>Visa prestandamått med hjälp av Azure-övervakning
1. I hello [Azure-portalen](https://portal.azure.com/), klickar du på **övervakaren** på hello Jumpbar.
2. Hello resurs menyn klickar du på **mått**.
3. I hello **Monitor - mått** i hello fönstret **esurs grupp** nedrullningsbara menyn, Välj hello resursgruppen som associeras med hello Azure DB som Cosmos-konto som du vill att toomonitor. 
4. I hello **resurs** nedrullningsbara menyn, Välj hello databasen konto toomonitor.
5. I hello lista över **tillgängliga mått**, Välj hello mått toodisplay. Använd hello CTRL toomulti-och väljer. 

    Dina visas på i hello **Rita** fönster. 

## <a name="view-performance-metrics-on-hello-account-blade"></a>Visa prestandamått på hello-kontoblad
1. I hello [Azure-portalen](https://portal.azure.com/), klickar du på **fler tjänster**, rulla för**databaser**, klickar du på **Azure Cosmos DB**, och klicka sedan på hello namnet på hello Azure DB Cosmos-konto som du vill att tooview prestandamått.
2. Hej **övervakning** lins visar hello följande paneler som standard:
   
   * Totalt antal begäranden för hello aktuell dag.
   * Lagringsutrymme som används.
   
   Om din tabell visar **inga tillgängliga data** och du tror att det finns data i databasen, se hello [felsökning](#troubleshooting) avsnitt.
   
   ![Skärmbild som visar hello övervakning lins som visar hello begäranden och hello lagringskvoten](./media/monitor-accounts/documentdb-total-requests-and-usage.png)
3. Klicka på hello **begäranden** eller **kvot** panelen öppnar en detaljerad **mått** bladet.
4. Hej **mått** bladet visar information om hello mått som du har valt.  Vid hello är överkant hello bladet ett diagram över begäranden i diagrammet varje timme och nedan som tabell som visar aggregering värden för begränsad och Totalt antal begäranden.  hello mått bladet visar även hello listan över aviseringar som har definierats, filtrerad toohello mått som visas på hello aktuella mått bladet (det här sättet om du har ett antal aviseringar, du bara se hello relevanta som presenteras här).   
   
   ![Skärmbild av bladet för hello mått som innehåller begränsas begäranden](./media/monitor-accounts/documentdb-metric-blade.png)

## <a name="customize-performance-metric-views-in-hello-portal"></a>Anpassa mått prestandavyer hello-portalen
1. toocustomize hello mått som visas i ett diagram, klickar du på hello diagram tooopen i hello **mått** bladet och klicka sedan på **redigera diagram**.  
   ![Skärmbild som visar hello mått bladet kontroller med Redigera diagram markerat](./media/monitor-accounts/madocdb3.png)
2. På hello **redigera diagram** bladet finns alternativ toomodify hello mått som visas i hello diagram, samt deras tidsintervall.  
   ![Skärmbild av bladet för hello redigera diagram](./media/monitor-accounts/madocdb4.png)
3. toochange hello mått visas hello del bara markera eller avmarkera hello tillgängliga prestandamått och klicka på **OK** längst hello hello-bladet.  
4. toochange hello tidsintervallet, Välj ett annat område (till exempel **anpassad**), och klicka sedan på **OK** längst hello hello-bladet.  
   
   ![Skärmbild som visar hello tidsintervall tillhör hello redigera diagram bladet visar hur tooenter ett anpassat tidsintervall](./media/monitor-accounts/madocdb5.png)

## <a name="create-side-by-side-charts-in-hello-portal"></a>Skapa sida-vid-sida-diagram i hello-portalen
hello Azure-portalen kan du toocreate sida-vid-sida mått diagram.  

1. Först, högerklicka på hello diagram toocopy och välj **anpassa**.
   
   ![Skärmbild som visar hello förfrågningarna diagram med hello anpassa markerat](./media/monitor-accounts/madocdb6.png)
2. Klicka på **klona** hello menyn toocopy hello del och klicka sedan på **klar anpassa**.
   
   ![Skärmen tog på hello förfrågningarna diagram med hello klona och klar anpassa alternativ markerat](./media/monitor-accounts/madocdb7.png)  

Du kan nu hantera den här delen som andra mått sidan, anpassa hello mått och ett tidsintervall visas hello del.  På så sätt ser två olika mått diagram sida-vid-sida på hello samtidigt.  
    ![Skärmbild som visar hello förfrågningarna diagram och hello nya förfrågningarna tidigare timme diagram](./media/monitor-accounts/madocdb8.png)  

## <a name="set-up-alerts-in-hello-portal"></a>Konfigurera aviseringar i hello portal
1. I hello [Azure-portalen](https://portal.azure.com/), klickar du på **fler tjänster**, klickar du på **Azure Cosmos DB**, och klicka sedan på hello namnet på hello Azure DB som Cosmos-konto som du vill att toosetup prestandavarningar för mått.
2. Hello resurs menyn klickar du på **Varningsregler** tooopen hello Varningsregler bladet.  
   ![Skärmbild som visar hello avisering regler del markerad](./media/monitor-accounts/madocdb10.5.png)
3. I hello **Varna regler** bladet, klickar du på **Lägg till avisering**.  
   ![Skärmbild av hello Varningsregler bladet med Lägg till avisering hello-knappen markerad](./media/monitor-accounts/madocdb11.png)
4. I hello **lägga till en varningsregel** bladet, ange:
   
   * hello namnet hello varningsregel du ställer in.
   * En beskrivning av hello nya varningsregel.
   * hello mått för hello varningsregel.
   * hello villkoret tröskelvärde och period som bestämmer när hello avisering aktiveras. Till exempel ett serverfel större antal än 5 över hello senaste 15 minuterna.
   * Om hello tjänstadministratören och coadministrators via e-post när hello avisering utlöses.
   * Ytterligare e-postadresser för aviseringar.  
     ![Skärmbild som visar hello lägga till en varningsregel-bladet](./media/monitor-accounts/madocdb12.png)

## <a name="monitor-azure-cosmos-db-programatically"></a>Övervaka Azure Cosmos DB genom att programmera
Hej konto nivån mätvärden som är tillgängliga i hello portal, till exempel kontot lagring användnings- och Totalt antal begäranden är inte tillgängliga via hello DocumentDB APIs. Dock kan du hämta data om programvaruanvändning på hello samlingsnivå genom att använda hello DocumentDB APIs. nivån tooretrieve samlingsdata, hello följande:

* toouse hello REST API [utföra GET på hello samlingen](https://msdn.microsoft.com/library/mt489073.aspx). information om hello kvoten och användning för hello samling returneras hello x-ms-resurs-quota och x-ms--Resursanvändning huvuden i svar hello.
* toouse hello .NET SDK använder hello [DocumentClient.ReadDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.readdocumentcollectionasync.aspx) metod som returnerar en [ResourceResponse](https://msdn.microsoft.com/library/dn799209.aspx) som innehåller ett antal egenskaper som  **CollectionSizeUsage**, **DatabaseUsage**, **DocumentUsage**, med mera.

tooaccess ytterligare statistik, använda hello [SDK för Azure-Monitor](https://www.nuget.org/packages/Microsoft.Azure.Insights). Tillgängliga mått definitioner kan hämtas genom att anropa:

    https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metricDefinitions?api-version=2015-04-08

Frågor tooretrieve enskilda mått Använd hello följande format:

    https://management.azure.com/subscriptions/{SubecriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metrics?api-version=2015-04-08&$filter=%28name.value%20eq%20%27Total%20Requests%27%29%20and%20timeGrain%20eq%20duration%27PT5M%27%20and%20startTime%20eq%202016-06-03T03%3A26%3A00.0000000Z%20and%20endTime%20eq%202016-06-10T03%3A26%3A00.0000000Z

Mer information finns i [hämtar resurs mätvärden via hello Azure övervakaren REST API](https://blogs.msdn.microsoft.com/cloud_solution_architect/2016/02/23/retrieving-resource-metrics-via-the-azure-insights-api/). Observera att ”Azure Inights” bytte ”Azure-Monitor”.  Det här blogginlägget refererar toohello äldre namn.

## <a name="troubleshooting"></a>Felsökning
Om övervakningen av rutor visa hello **inga tillgängliga data** meddelandet och nyligen gjorts begäranden eller lagt data toohello databasen kan du redigera hello panelen tooreflect hello senare användning.

### <a name="edit-a-tile-toorefresh-current-data"></a>Redigera en toorefresh aktuella paneldata
1. toocustomize hello mått som visas i en viss del klickar du på hello diagram tooopen hello **mått** bladet och klicka sedan på **redigera diagram**.  
   ![Skärmbild som visar hello mått bladet kontroller med Redigera diagram markerat](./media/monitor-accounts/madocdb3.png)
2. På hello **redigera diagram** bladet i hello **tidsintervall** klickar du på **efter timme**, och klicka sedan på **OK**.  
   ![Skärmbild som visar hello redigera diagram bladet med senaste timmen som valts](./media/monitor-accounts/documentdb-no-available-data-past-hour.png)
3. Din panel Uppdatera nu visar aktuella data och användning.  
   ![Skärmbild som visar hello uppdateras Totalt antal begäranden efter timme sida vid sida](./media/monitor-accounts/documentdb-no-available-data-fixed.png)

## <a name="next-steps"></a>Nästa steg
toolearn mer om Azure Cosmos DB kapacitetsplanering finns hello [Azure Cosmos DB kapacitet planner Kalkylatorn](https://www.documentdb.com/capacityplanner).


---
title: "Övervaka Azure Cosmos DB begäranden och storage | Microsoft Docs"
description: "Lär dig hur du övervakar Azure Cosmos DB kontot för prestandavärden, till exempel begäranden och fel, och användningsstatistik, till exempel användningen av lagringsutrymme."
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
ms.openlocfilehash: 0ca652d31d6c50124f87916b4486d8279075f106
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-azure-cosmos-db-requests-usage-and-storage"></a>Övervaka Azure Cosmos DB begäranden, användning och lagring
Du kan övervaka dina Azure DB som Cosmos-konton i den [Azure-portalen](https://portal.azure.com/). Båda prestandavärden, till exempel begäranden och fel och användningsstatistik, till exempel lagringsförbrukning, är tillgängliga för varje Azure DB som Cosmos-konto.

Mått kan ses i bladet konto bladet ny mått eller i Azure-Monitor.

## <a name="view-performance-metrics-on-the-metrics-blade"></a>Visa prestandamått i bladet mått
1. I den [Azure-portalen](https://portal.azure.com/), klickar du på **fler tjänster**, bläddra till **databaser**, klickar du på **Azure Cosmos DB**, och klicka sedan på namnet på Azure Cosmos DB-konto som du vill visa prestandamått.
2. I menyn resurs under **övervakning**, klickar du på **mått**.

Bladet mått öppnas och du kan välja en samling ska granska. Du kan granska mått för tillgänglighet, begäranden, genomflöde och lagring och jämför dem med Azure Cosmos DB serviceavtal.

## <a name="view-performance-metrics-by-using-azure-monitoring"></a>Visa prestandamått med hjälp av Azure-övervakning
1. I den [Azure-portalen](https://portal.azure.com/), klickar du på **övervakaren** på Jumpbar.
2. Resurs-menyn klickar du på **mått**.
3. I den **Monitor - mått** fönster i den **esurs grupp** listrutan väljer du resursgruppen som är associerade med Azure DB som Cosmos-konto som du vill övervaka. 
4. I den **resurs** nedrullningsbara menyn och väljer databasen konto för att övervaka.
5. I listan över **tillgängliga mått**, välja mått att visa. Använd knappen CTRL du kan markera. 

    Dina visas i den **Rita** fönster. 

## <a name="view-performance-metrics-on-the-account-blade"></a>Visa prestandamått i bladet konto
1. I den [Azure-portalen](https://portal.azure.com/), klickar du på **fler tjänster**, bläddra till **databaser**, klickar du på **Azure Cosmos DB**, och klicka sedan på namnet på Azure Cosmos DB-konto som du vill visa prestandamått.
2. Den **övervakning** lins visar följande rubriker som standard:
   
   * Totalt antal begäranden för den aktuella dagen.
   * Lagringsutrymme som används.
   
   Om din tabell visar **inga tillgängliga data** och du tror att det finns data i din databas, finns det [felsökning](#troubleshooting) avsnitt.
   
   ![Skärmbild av övervakning linsen som visar begäranden och lagringsanvändningen](./media/monitor-accounts/documentdb-total-requests-and-usage.png)
3. Klicka på den **begäranden** eller **kvot** panelen öppnar en detaljerad **mått** bladet.
4. Den **mått** bladet visar information om mått som du har valt.  Längst upp på bladet är ett diagram över begäranden i diagrammet varje timme och nedan som är tabell som visar aggregering värden för begränsad och Totalt antal begäranden.  Bladet mått visar också en lista över aviseringar som har definierats, filtreras till statistik som visas i bladet aktuella mått (det här sättet om du har ett antal aviseringar, du bara se de relevanta som presenteras här).   
   
   ![Skärmbild av bladet mått som innehåller begränsas begäranden](./media/monitor-accounts/documentdb-metric-blade.png)

## <a name="customize-performance-metric-views-in-the-portal"></a>Anpassa mått prestandavyer i portalen
1. Om du vill anpassa de mätvärden som visas i ett diagram, klickar du på diagrammet för att öppna den i den **mått** bladet och klicka sedan på **redigera diagram**.  
   ![Skärmbild av bladet mått-kontroller med Redigera diagram markerat](./media/monitor-accounts/madocdb3.png)
2. På den **redigera diagram** bladet finns alternativ för att ändra mått som visas i diagrammet, samt deras tidsintervall.  
   ![Skärmbild av bladet redigera diagram](./media/monitor-accounts/madocdb4.png)
3. Om du vill ändra måtten visas del bara markera eller avmarkera tillgängliga prestandamåtten och klicka på **OK** längst ned på bladet.  
4. Välj ett annat område för att ändra tidsintervallet (till exempel **anpassad**), och klicka sedan på **OK** längst ned på bladet.  
   
   ![Skärmbild som visar det tidsintervall som en del av bladet redigera diagram som visar hur du anger ett anpassat tidsintervall](./media/monitor-accounts/madocdb5.png)

## <a name="create-side-by-side-charts-in-the-portal"></a>Skapa sida-vid-sida-diagram i portalen
Azure-portalen kan du skapa mått sida-vid-sida-diagram.  

1. Först, högerklickar du på diagrammet som du vill kopiera och välj **anpassa**.
   
   ![Skärmbild som visar totalt antal begäranden diagrammet med alternativet Anpassa markerat](./media/monitor-accounts/madocdb6.png)
2. Klicka på **klona** på menyn för att kopiera delen och klicka sedan på **klar anpassa**.
   
   ![Skärmen som förfrågningarna diagrammets klonen och klar anpassa alternativ markerat](./media/monitor-accounts/madocdb7.png)  

Du kan nu hantera den här delen som mått del, anpassa de mått och ett tidsintervall visas del.  På så sätt ser du två olika mått diagram sida-vid-sida på samma gång.  
    ![Skärmbild som visar totalt antal begäranden diagrammet och nya förfrågningarna tidigare timme diagram](./media/monitor-accounts/madocdb8.png)  

## <a name="set-up-alerts-in-the-portal"></a>Ställa in aviseringar på portalen
1. I den [Azure-portalen](https://portal.azure.com/), klickar du på **fler tjänster**, klickar du på **Azure Cosmos DB**, och klicka sedan på namnet på Azure DB som Cosmos-konto som du vill att installationsprogrammet prestanda mått aviseringar.
2. Resurs-menyn klickar du på **Varningsregler** att öppna bladet Varningsregler.  
   ![Skärmbild som visar aviseringen regler del markerad](./media/monitor-accounts/madocdb10.5.png)
3. I den **Varna regler** bladet, klickar du på **Lägg till avisering**.  
   ![Skärmbild av bladet Varningsregler med knappen Lägg till avisering markerad](./media/monitor-accounts/madocdb11.png)
4. I den **lägga till en varningsregel** bladet, ange:
   
   * Namnet på regeln som du ställer in.
   * En beskrivning av den nya regeln.
   * Mått för regeln.
   * Villkor, tröskelvärde och period som bestämmer när aviseringen aktiveras. Till exempel ett serverfel större antal än 5 under de senaste 15 minuterna.
   * Om tjänstadministratören och coadministrators via e-post när aviseringen utlöses.
   * Ytterligare e-postadresser för aviseringar.  
     ![Skärmbild av guiden Lägg till en varningsregel-bladet](./media/monitor-accounts/madocdb12.png)

## <a name="monitor-azure-cosmos-db-programatically"></a>Övervaka Azure Cosmos DB genom att programmera
De konto nivån tillgängliga mått i portalen, till exempel kontot lagring användnings- och Totalt antal begäranden är inte tillgängliga via DocumentDB APIs. Du kan dock hämta data om programvaruanvändning på samlingsnivå med hjälp av DocumentDB APIs. Om du vill hämta data om nivån, gör du följande:

* Använda REST-API för [utföra GET på samlingen](https://msdn.microsoft.com/library/mt489073.aspx). Informationen om kvot- och användningsdata för samlingen returneras i x-ms-resurs-quota- och x-ms--Resursanvändning i svaret.
* Använd .NET SDK i [DocumentClient.ReadDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.readdocumentcollectionasync.aspx) metod som returnerar en [ResourceResponse](https://msdn.microsoft.com/library/dn799209.aspx) som innehåller ett antal egenskaper som  **CollectionSizeUsage**, **DatabaseUsage**, **DocumentUsage**, med mera.

Om du vill komma åt ytterligare mått kan använda den [SDK för Azure-Monitor](https://www.nuget.org/packages/Microsoft.Azure.Insights). Tillgängliga mått definitioner kan hämtas genom att anropa:

    https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metricDefinitions?api-version=2015-04-08

Frågor för att hämta enskilda mått använder du följande format:

    https://management.azure.com/subscriptions/{SubecriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metrics?api-version=2015-04-08&$filter=%28name.value%20eq%20%27Total%20Requests%27%29%20and%20timeGrain%20eq%20duration%27PT5M%27%20and%20startTime%20eq%202016-06-03T03%3A26%3A00.0000000Z%20and%20endTime%20eq%202016-06-10T03%3A26%3A00.0000000Z

Mer information finns i [hämtar resurs mätvärden via REST API för Azure-Monitor](https://blogs.msdn.microsoft.com/cloud_solution_architect/2016/02/23/retrieving-resource-metrics-via-the-azure-insights-api/). Observera att ”Azure Inights” bytte ”Azure-Monitor”.  Det här blogginlägget refererar till äldre namn.

## <a name="troubleshooting"></a>Felsökning
Om övervakningen av rutor visas den **inga tillgängliga data** meddelandet och nyligen gjorts begäranden eller lagt till data i databasen kan du redigera panelen för att återspegla den nya användningen.

### <a name="edit-a-tile-to-refresh-current-data"></a>Redigera en panel för att uppdatera aktuella data
1. Om du vill anpassa de mätvärden som visas i en viss del klickar du på diagrammet för att öppna den **mått** bladet och klicka sedan på **redigera diagram**.  
   ![Skärmbild av bladet mått-kontroller med Redigera diagram markerat](./media/monitor-accounts/madocdb3.png)
2. På den **redigera diagram** blad i den **tidsintervall** klickar du på **efter timme**, och klicka sedan på **OK**.  
   ![Skärmbild av bladet redigera diagram med senaste timmen som valts](./media/monitor-accounts/documentdb-no-available-data-past-hour.png)
3. Din panel Uppdatera nu visar aktuella data och användning.  
   ![Skärmbild av den uppdaterade Totalt antal begäranden efter timme sida vid sida](./media/monitor-accounts/documentdb-no-available-data-fixed.png)

## <a name="next-steps"></a>Nästa steg
Läs mer om Azure Cosmos DB kapacitetsplanering i den [Azure Cosmos DB kapacitet planner Kalkylatorn](https://www.documentdb.com/capacityplanner).


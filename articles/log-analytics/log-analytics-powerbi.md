---
title: aaaExport logganalys data tooPower BI | Microsoft Docs
description: "Powerbi är en molnbaserad business analytics-tjänst från Microsoft som innehåller omfattande visualiseringar och rapporter för analys av olika datauppsättningar.  Logganalys exportera kontinuerligt data från hello OMS-databasen till Power BI så att du kan utnyttja dess visualiseringar och verktyg för analys.  Den här artikeln beskriver hur tooconfigure frågor i logganalys som exporterar tooPower BI automatiskt med jämna mellanrum."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 83edc411-6886-4de1-aadd-33982147b9c3
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/24/2017
ms.author: bwren
ms.openlocfilehash: 4822f99677e5d1080c72e95cda410da81615bac5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="export-log-analytics-data-toopower-bi"></a>Exportera logganalys data tooPower BI

>[!NOTE]
> Om ditt arbetsområde har uppgraderade toohello [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), och sedan på den här processen för att exportera logganalys data tooPower BI fungerar inte längre.  Alla befintliga scheman som du skapade innan du uppgraderar inaktiveras. 
>
> Efter uppgraderingen hello Azure Log Analytics använder samma plattform som Application Insights och du använder samma process tooexport logganalys frågor tooPower BI som hello [hello processen tooexport Application Insights frågar tooPower BI](../application-insights/app-insights-export-power-bi.md#export-analytics-queries).  Du kan antingen exportera hello frågan med hello Analytics konsolen enligt beskrivningen i artikeln eller så kan du välja hello **Power BI** knappen hello överst i hello-skärmen i hello loggen Sök-portalen.



[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) är en molnbaserad business analytics-tjänst från Microsoft som innehåller omfattande visualiseringar och rapporter för analys av olika datauppsättningar.  Logganalys exportera automatiskt data från hello OMS-databasen till Power BI så att du kan utnyttja dess visualiseringar och verktyg för analys.

När du konfigurerar Power BI med logganalys skapa log-frågor som exporterar deras resultat toocorresponding datauppsättningar i Power BI.  hello frågan och exportera fortsätter tooautomatically körs enligt ett schema som du definierar tookeep hello datamängden in toodate med hello senaste data som samlas in av logganalys.

![Log Analytics tooPower BI](media/log-analytics-powerbi/overview.png)

## <a name="power-bi-schedules"></a>Power BI-scheman
En *Power BI schema* innehåller en logg sökning som exporterar en uppsättning data från hello OMS databasen tooa motsvarande dataset i Power BI och ett schema som definierar hur ofta den här sökningen körs tookeep hello dataset aktuella.

hello fält i datamängden hello matchar hello egenskaper för hello-poster som returneras av hello loggen sökning.  Om hello sökningen poster med olika typer och sedan hello datauppsättningen tas alla med hello egenskaper från varje hello posttyper.  

> [!NOTE]
> Det är en bästa praxis toouse en logg sökfråga som returnerar rådata som skillnad från tooperforming konsolidering med kommandon som [mått](log-analytics-search-reference.md#measure).  Du kan utföra någon aggregering och beräkningar i Power BI från hello rådata.
>
>

## <a name="connecting-oms-workspace-toopower-bi"></a>Ansluta OMS-arbetsytan tooPower BI
Innan du kan exportera från logganalys tooPower BI måste du ansluta din OMS arbetsytan tooyour Power BI-konto med hjälp av hello nedan.  

1. I hello OMS-konsolen klickar du på hello **inställningar** panelen.
2. Välj **konton**.
3. I hello **arbetsyteinformation** Klicka på avsnittet **ansluta tooPower BI-konto**.
4. Ange hello autentiseringsuppgifter för Power BI-konto.

## <a name="create-a-power-bi-schedule"></a>Skapa ett schema för Power BI
Skapa ett Power BI-schema för varje dataset med hello nedan.

1. I hello OMS-konsolen klickar du på hello **loggen Sök** panelen.
2. Ange en ny fråga eller välj en sparad sökning som returnerar hello data som du vill tooexport för**Power BI**.  
3. Klicka på hello **Power BI** knappen hello överst i hello sidan tooopen hello **Power BI** dialogrutan.
4. Ange hello information i hello följande tabell och klicka på **spara**.

| Egenskap | Beskrivning |
|:--- |:--- |
| Namn |Namnet tooidentify hello schema när du visar hello listan över Power BI-scheman. |
| Sparad sökning |hello loggen Sök toorun.  Du kan välja hello aktuell fråga, eller så kan du välja en befintlig sparad sökning från hello listrutan. |
| Schema |Hur ofta toorun hello sparade söka och exportera toohello Power BI dataset.  hello-värdet måste vara mellan 15 minuter och 24 timmar. |
| DataSet-namnet |hello namnet på hello dataset i Power BI.  Den kommer att skapas om det inte finns och uppdateras om den finns. |

## <a name="viewing-and-removing-power-bi-schedules"></a>Visa och ta bort scheman för Power BI
Visa hello en lista över befintliga Power BI-scheman med hello nedan.

1. I hello OMS-konsolen klickar du på hello **inställningar** panelen.
2. Välj **Power BI**.

Dessutom toohello information om hello schemalägga, hello antalet gånger som hello schema har körts i hello gångna veckan och hello status för senaste synkronisering för hello visas.  Om fel uppstod i hello synkronisering klickar du på hello länken toorun en logg söka efter poster med information om hello-fel.

Du kan ta bort ett schema genom att klicka på hello **X** i hello **ta bort kolumnen**.  Du kan inaktivera ett schema genom att välja **av**.  toomodify ett schema som du måste ta bort den och återskapa den med nya inställningar för hello.

![Power BI-scheman](media/log-analytics-powerbi/schedules.png)

## <a name="sample-walkthrough"></a>Exempel genomgång
hello följande avsnitt beskriver hur ett exempel på att skapa ett schema för Power BI och använda dess dataset toocreate en enkel rapport.  I det här exemplet alla prestandadata för en uppsättning datorer är exporterade tooPower BI och sedan ett linjediagram skapas toodisplay processorbelastning.

### <a name="create-log-search"></a>Skapa loggen sökning
Vi börjar med att skapa en logg sökning efter hello data som vi vill toosend toohello dataset.  I det här exemplet ska vi använda en fråga som returnerar alla prestandadata för datorer med ett namn som börjar med *srv*.  

![Power BI-scheman](media/log-analytics-powerbi/walkthrough-query.png)

### <a name="create-power-bi-search"></a>Skapa Power BI-sökning
Vi klickar på hello **Power BI** knappen tooopen hello Power BI dialogrutan och ange hello krävs information.  Vi vill att den här sökningen toorun en gång i timmen och skapa en datamängd som kallas *Contoso Perf*.  Eftersom det redan har hello Sök öppna som skapar hello data som vi vill vi behåller hello standardvärdet *Använd aktuell sökfråga* för **sparad sökning**.

![Power BI-sökningen](media/log-analytics-powerbi/walkthrough-schedule.png)

### <a name="verify-power-bi-search"></a>Kontrollera Power BI-sökningen
tooverify att vi har skapat hello schema korrekt, vi visa hello lista över Power BI sökningar under hello **inställningar** panelen i hello OMS-instrumentpanelen.  Vi Vänta några minuter och uppdatera den här vyn förrän den rapporterar att hello synkronisering har körts.

![Power BI-sökningen](media/log-analytics-powerbi/walkthrough-schedules.png)

### <a name="verify-hello-dataset-in-power-bi"></a>Verifiera hello datauppsättningen i Power BI
Vi logga in på vår konto på [powerbi.microsoft.com](http://powerbi.microsoft.com/) och rulla för**datauppsättningar** längst hello hello till vänster.  Vi kan se att hello *Contoso Perf* datauppsättningen visas som anger att vår export har kunnat köras.

![Power BI dataset](media/log-analytics-powerbi/walkthrough-datasets.png)

### <a name="create-report-based-on-dataset"></a>Skapa rapport baserad på dataset
Vi väljer hello **Contoso Perf** dataset och klicka sedan på **resultat** i hello **fält** rutan på hello rätt tooview hello fält som ingår i denna dataset.  toocreate en rad diagram som visar processorbelastning för varje dator, vi utföra hello följande åtgärder.

1. Välj hello rad diagram visualiseringen.
2. Dra **ObjectName** för**rapporten nivån filter** och kontrollera **Processor**.
3. Dra **CounterName** för**rapporten nivån filter** och kontrollera **% processortid**.
4. Dra **CounterValue** för**värden**.
5. Dra **datorn** för**förklaring**.
6. Dra **TimeGenerated** för**axel**.

Vi kan se det resulterande linjediagrammet hello visas med hello data från vår datauppsättning.

![Power BI-linjediagram](media/log-analytics-powerbi/walkthrough-linegraph.png)

### <a name="save-hello-report"></a>Spara hello-rapport
Vi spara hello rapporten genom att klicka på hello knappen hello överst i hello-skärmen Spara och validera att det visas nu i hello rapporter i hello till vänster.

![Power BI-rapporter](media/log-analytics-powerbi/walkthrough-report.png)

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [logga sökningar](log-analytics-log-searches.md) toobuild frågor som kan exporteras tooPower BI.
* Lär dig mer om [Power BI](http://powerbi.microsoft.com) toobuild visualiseringar utifrån logganalys exporter.

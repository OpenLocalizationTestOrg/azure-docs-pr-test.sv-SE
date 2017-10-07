---
title: aaaCollect anpassade loggar i OMS Log Analytics | Microsoft Docs
description: "Logganalys kan samla in händelser från textfiler på Windows- och Linux-datorer.  Den här artikeln beskriver hur toodefine en ny anpassad logg och information om hello poster skapas i hello OMS-databasen."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: aca7f6bb-6f53-4fd4-a45c-93f12ead4ae1
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/15/2017
ms.author: bwren
ms.openlocfilehash: a75d058c371fecf7c43690a797d4e650c1570317
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="custom-logs-in-log-analytics"></a>Anpassade loggar i logganalys
datakälla för hello anpassade loggar i logganalys kan toocollect händelser från textfiler på Windows- och Linux-datorer. Många program loggar information tootext filer i stället för standardtjänster loggning, till exempel Windows-händelseloggen eller Syslog.  När samlas in, analyserar du varje post i loggen för hello i tooindividual fält med hello [anpassade fält](log-analytics-custom-fields.md) funktion i logganalys.

![Anpassade Logginsamling](media/log-analytics-data-sources-custom-logs/overview.png)

hello loggen filer toobe samlas in måste matcha hello följande villkor.

- hello logg måste antingen ha en enda post per rad eller använda en tidsstämpel som matchar en av följande hello formaterar hello början av varje post.

    ÅÅÅÅ-MM-DD: MM: SS<br>M/D/ÅÅÅÅ HH: MM: SS TID <br>MON DD, YYYY: mm: ss

- hello loggfil får inte tillåta cirkulär uppdateringar där hello filen skrivs över med nya poster.
- hello loggfilen måste använda ASCII- eller UTF-8-kodning.  Andra format, till exempel UTF-16 stöds inte.

>[!NOTE]
>Om det finns dubblettvärden i hello loggfil, logganalys samlar in dem.  Hello sökresultat kommer dock att inkonsekvent där hello filterresultaten visa fler händelser än hello resultatantal.  Kommer att vara viktigt att du validera hello loggen toodetermine om hello-program som skapar den orsakar problemet och åtgärda om möjligt innan du skapar hello anpassad logg samling definition.  
>
  
## <a name="defining-a-custom-log"></a>Definiera en anpassad logg
Använd hello följa proceduren toodefine en anpassad loggfil.  Rulla toohello slutet av artikeln en genomgång av ett prov för att lägga till en anpassad logg.

### <a name="step-1-open-hello-custom-log-wizard"></a>Steg 1. Öppna hello anpassad logg guide
hello logg guide körs i hello OMS-portalen och kan du toodefine toocollect för en ny anpassad logg.

1. I hello OMS-portalen, går för**inställningar**.
2. Klicka på **Data** och sedan **anpassade loggar**.
3. Som standard är alla konfigurationsändringar automatiskt pushas tooall agenter.  Linux-agenter skickas en konfigurationsfil toohello Fluentd datainsamlaren.  Om du inte vill toomodify den här filen manuellt på varje Linux-agent, avmarkerar du kryssrutan hello *tillämpa konfigurationen toomy Linux-datorerna nedan*.
4. Klicka på **Lägg till +** tooopen hello guiden för anpassad logg.

### <a name="step-2-upload-and-parse-a-sample-log"></a>Steg 2. Överföra och parsa en exempellogg
Börja med att ladda upp ett exempel på hello anpassad logg.  hello guiden parsa och visa hello poster i den här filen när du toovalidate.  Log Analytics använder hello avgränsare som du anger tooidentify varje post.

**Ny rad** hello standard avgränsare och används för loggfiler som har en post per rad.  Om hello rad som börjar med ett datum och tid i något av formaten hello sedan kan du ange en **tidsstämpel** avgränsare som har stöd för transaktioner som sträcker sig över flera rader.

Om en avgränsare för en tidsstämpel används fylls hello TimeGenerated egenskap för varje post i OMS med hello datum/tid som angetts för posten i hello loggfilen.  Om en Radavgränsare för en ny används fylls TimeGenerated med datum och tid att logganalys samlas in hello-post.

> [!NOTE]
> Logganalys behandlar för närvarande hello datum/tid som samlas in från en logg med en tidsstämpel avgränsare som UTC.  Detta kommer snart att ändrade toouse hello tidszonen på hello agent.
>
>

1. Klicka på **Bläddra** och bläddra tooa exempelfilen.  Observera att detta kan knappen betecknas **Välj fil** i vissa webbläsare.
2. Klicka på **Nästa**.
3. hello anpassad logg guiden kommer att överföra hello fil- och hello poster som identifieras.
4. Ändra hello avgränsare som används tooidentify en ny post och välj hello avgränsare som bäst identifierar hello poster i loggfilen.
5. Klicka på **Nästa**.

### <a name="step-3-add-log-collection-paths"></a>Steg 3. Lägg till loggsamling
Du måste definiera en eller flera sökvägar på hello agent där den kan hitta hello anpassad logg.  Du kan antingen ange en specifik sökväg och ett namn för hello loggfilen eller du kan ange en sökväg med jokertecken för hello namn.  Detta stöder program som skapar en ny fil varje dag eller när en fil når en viss storlek.  Du kan också ange flera sökvägar för en enda loggfil.

Till exempel kan ett program skapa en loggfil skapas varje dag med hello datumet som ingår i hello namn som log20100316.txt. Ett mönster för sådana loggen kan vara *loggen\*.txt* som skulle ha använts tooany loggfilen hello tillämpning naming schemat.

hello följande tabell innehåller exempel på giltiga toospecify olika loggfilerna.

| Beskrivning | Sökväg |
|:--- |:--- |
| Alla filer i *C:\Logs* med filnamnstillägget .txt på Windows-agenten |C:\Logs\\\*.txt |
| Alla filer i *C:\Logs* med ett namn som börjar med loggen och filnamnstillägget .txt på Windows-agenten |C:\Logs\log\*.txt |
| Alla filer i */var/log/audit* med filnamnstillägget .txt på Linux-agent |/var/log/audit/*.txt |
| Alla filer i */var/log/audit* med ett namn som börjar med loggen och filnamnstillägget .txt på Linux-agent |/var/log/audit/log\*.txt |

1. Välj Windows eller Linux toospecify vilka sökvägsformat du lägger till.
2. Anger hello sökväg och klickar på hello  **+**  knappen.
3. Upprepa hello processen för eventuella ytterligare sökvägar.

### <a name="step-4-provide-a-name-and-description-for-hello-log"></a>Steg 4. Ange ett namn och beskrivning för hello logg
hello ska namn som du anger användas för hello loggtyp enligt beskrivningen ovan.  Den slutar alltid med _CL toodistinguish den som en anpassad logg.

1. Ange ett namn för hello logg.  Hej  **\_CL** suffix anges automatiskt.
2. Lägg till en valfri **beskrivning**.
3. Klicka på **nästa** toosave hello anpassad loggdefinition.

### <a name="step-5-validate-that-hello-custom-logs-are-being-collected"></a>Steg 5. Verifiera att hello anpassade loggar samlas in
Det kan ta upp tooan timme för hello inledande data från en ny anpassad logg tooappear i logganalys.  Den börjar samla in poster från hello loggar finns i hello sökväg du angav från hello tidpunkten som du definierade hello anpassad logg.  Det kommer inte att ha hello-poster som du har överfört under skapande av hello anpassad logg, men den samlar in redan befintliga poster i hello loggfiler som påträffas.

När logganalys startar har samlats in från hello anpassad logg är dess poster tillgängliga med en logg-sökning.  Använd hello namn du gav hello anpassad logg som hello **typen** i frågan.

> [!NOTE]
> Om hello \data egenskap saknas från hello sökning, kanske du behöver tooclose och öppna webbläsaren.
>
>

### <a name="step-6-parse-hello-custom-log-entries"></a>Steg 6. Parsa hello anpassade loggposter
hello hela loggpost kommer att lagras i en enda egenskap som kallas **\data**.  Du kommer förmodligen vill tooseparate hello olika delar av informationen i varje post i enskilda egenskaper lagras i hello-post.  Du gör detta med hjälp av hello [anpassade fält](log-analytics-custom-fields.md) funktion i logganalys.

Detaljerade anvisningar för att parsa hello anpassade loggpost finns inte här.  Se toohello [anpassade fält](log-analytics-custom-fields.md) dokumentationen för den här informationen.

## <a name="disabling-a-custom-log"></a>Inaktivera en anpassad logg
Du kan inte ta bort en anpassad loggdefinition när den har skapats, men du kan inaktivera det genom att ta bort alla sökvägar samling.

1. I hello OMS-portalen, går för**inställningar**.
2. Klicka på **Data** och sedan **anpassade loggar**.
3. Klicka på **information** nästa toohello anpassad logg definition toodisable.
4. Ta bort alla hello samling sökvägar för hello anpassad loggdefinition.

## <a name="data-collection"></a>Datainsamling
Logganalys samlar in nya poster från varje anpassad logg ungefär var 5: e minut.  hello agenten att registrera sin plats i varje loggfil som samlas in från.  Om hello agenten tas offline under en tidsperiod, sedan logganalys samlar in poster från där den senast slutade, även om dessa poster har skapats i hello-agenten var offline.

hello hela innehållet i hello loggpost skrivs tooa enda egenskap som kallas **\data**.  Du kan parsa detta till flera egenskaper som kan analyseras och genomsöks separat genom att definiera [anpassade fält](log-analytics-custom-fields.md) när du har skapat hello anpassad logg.

## <a name="custom-log-record-properties"></a>Egenskaper för anpassad logg-post
Anpassade loggposter har en typ med hello loggnamn som du anger och hello egenskaper i hello i den följande tabellen.

| Egenskap | Beskrivning |
|:--- |:--- |
| TimeGenerated |Datum och tid då hello post samlades in av logganalys.  Om hello loggen använder en avgränsare för en tidsbaserad är hello tid som samlas in från hello-post. |
| SourceSystem |Agenten hello posttyp samlats in från. <br> Ansluta OpsManager – Windows-agenten, antingen direkt eller System Center Operations Manager <br> Linux – alla Linux-agenter |
| \Data |Fullständig text hello samlas in transaktionen. |
| ManagementGroupName |Namnet på hanteringsgruppen för hello för System Center Operations hantera agenter.  För andra agenter är AOI -\<arbetsyte-ID\> |

## <a name="log-searches-with-custom-log-records"></a>Loggen sökningar med anpassade loggposter
Poster från anpassade loggar lagras i hello OMS databasen precis som poster från andra datakällor.  De har en typ som matchar hello-namn som du anger när du definierar hello loggen, så du kan använda hello typegenskapen i din sökning tooretrieve poster som samlats in från en viss loggning.

hello innehåller följande tabell olika exempel på loggen sökningar som hämtar poster från anpassade loggar.

| Fråga | Beskrivning |
|:--- |:--- |
| Typ = MyApp_CL |Alla händelser från en anpassad logga namngiven MyApp_CL. |
| Typ = MyApp_CL Severity_CF = fel |Alla händelser från en anpassad logga namngivna MyApp_CL med värdet *fel* i ett anpassat fält med namnet *Severity_CF*. |

>[!NOTE]
> Om ditt arbetsområde har uppgraderade toohello [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), hello senare frågorna skulle ändra toohello följande.

> | Fråga | Beskrivning |
|:--- |:--- |
| MyApp_CL |Alla händelser från en anpassad logga namngiven MyApp_CL. |
| MyApp_CL &#124; där Severity_CF == ”error” |Alla händelser från en anpassad logga namngivna MyApp_CL med värdet *fel* i ett anpassat fält med namnet *Severity_CF*. |


## <a name="sample-walkthrough-of-adding-a-custom-log"></a>Exempel genomgången för att lägga till en anpassad logg
hello följande avsnitt beskriver hur ett exempel på hur du skapar en anpassad logg.  Hej exempellogg som samlas in har en enda post på varje rad som börjar med ett datum och tid och sedan kommaavgränsad fält för koden, status och meddelandet.  Flera exempel poster visas nedan.

    2016-03-10 01:34:36 207,Success,Client 05a26a97-272a-4bc9-8f64-269d154b0e39 connected
    2016-03-10 01:33:33 208,Warning,Client ec53d95c-1c88-41ae-8174-92104212de5d disconnected
    2016-03-10 01:35:44 209,Success,Transaction 10d65890-b003-48f8-9cfc-9c74b51189c8 succeeded
    2016-03-10 01:38:22 302,Error,Application could not connect toodatabase
    2016-03-10 01:31:34 303,Error,Application lost connection toodatabase

### <a name="upload-and-parse-a-sample-log"></a>Överföra och parsa en exempellogg
Vi tillhandahåller någon hello loggfiler och kan se hello händelser som kommer samla in.  I det här fallet är ny rad tillräcklig avgränsare.  Om en enda post i loggen för hello kan sträcka sig över flera rader men, skulle en avgränsare för en tidsstämpel måste toobe används.

![Överföra och parsa en exempellogg](media/log-analytics-data-sources-custom-logs/delimiter.png)

### <a name="add-log-collection-paths"></a>Lägg till loggsamling
hello-loggfiler finns i *C:\MyApp\Logs*.  En ny fil skapas varje dag med ett namn som innehåller hello datum i hello mönster *appYYYYMMDD.log*.  Ett tillräckligt mönster för den här loggfilen skulle vara *C:\MyApp\Logs\\\*.log*.

![Loggsökvägen för samlingen](media/log-analytics-data-sources-custom-logs/collection-path.png)

### <a name="provide-a-name-and-description-for-hello-log"></a>Ange ett namn och beskrivning för hello logg
Vi använder ett namn på *MyApp_CL* och skriver i en **beskrivning**.

![Loggnamn](media/log-analytics-data-sources-custom-logs/log-name.png)

### <a name="validate-that-hello-custom-logs-are-being-collected"></a>Verifiera att hello anpassade loggar samlas in
Vi använder en fråga med *typ = MyApp_CL* tooreturn alla poster från hello insamlade loggen.

![Loggen fråga utan anpassade fält](media/log-analytics-data-sources-custom-logs/query-01.png)

### <a name="parse-hello-custom-log-entries"></a>Parsa hello anpassade loggposter
Vi använder anpassade fält toodefine hello *EventTime*, *kod*, *Status*, och *meddelandet* fält och vi ser hello skillnad i hello poster som returneras av hello frågan.

![Loggen frågan med anpassade fält](media/log-analytics-data-sources-custom-logs/query-02.png)

## <a name="next-steps"></a>Nästa steg
* Använd [anpassade fält](log-analytics-custom-fields.md) tooparse hello poster i hello anpassad logg i tooindividual fält.
* Lär dig mer om [logga sökningar](log-analytics-log-searches.md) tooanalyze hello data som samlas in från datakällor och lösningar.

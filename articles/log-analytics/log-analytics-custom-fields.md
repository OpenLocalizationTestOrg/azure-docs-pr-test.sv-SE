---
title: "aaaCustom fält i Log Analytics | Microsoft Docs"
description: "hello anpassade fält för Log Analytics kan du toocreate egna sökbara fält från OMS-data som lägger till toohello egenskaper för en post som samlats in.  Den här artikeln beskriver hello processen toocreate ett anpassat fält och ger en detaljerad genomgång en exempelhändelse."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 31572b51-6b57-4945-8208-ecfc3b5304fc
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/18/2016
ms.author: bwren
ms.openlocfilehash: 1aa0e497d5b1d7898b0da6a5ef40f568e63bc589
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="custom-fields-in-log-analytics"></a>Anpassade fält i logganalys
Hej **anpassade fält** funktionen för Log Analytics gör att du tooextend befintliga poster i hello OMS-databas genom att lägga till egna sökbara fält.  Anpassade fält fylls automatiskt i från data som hämtats från andra egenskaper i hello samma post.

![Översikt över anpassade fält](media/log-analytics-custom-fields/overview.png)

Till exempel har hello exempelpost nedan användbara data begravd i hello händelsebeskrivningen.  Extrahera data i separata egenskaper gör den tillgänglig för exempelvis sortera och filtrera.

![Logga sökknappen](media/log-analytics-custom-fields/sample-extract.png)

> [!NOTE]
> I hello Förhandsgranska är du begränsad too100 anpassade fält i arbetsytan.  Den här gränsen ska expanderas när den här funktionen når allmän tillgänglighet.
> 
> 

## <a name="creating-a-custom-field"></a>Skapa ett anpassat fält
När du skapar ett anpassat fält måste logganalys veta vilka data toouse toopopulate dess värde.  Den använder en teknik från Microsoft Research kallas FlashExtract tooquickly identifiera dessa data.  I stället för att kräva att du tooprovide explicit instruktioner logganalys lär sig hello data vill du tooextract från de exempel som du anger.

hello följande avsnitt innehåller hello procedur för att skapa ett anpassat fält.  Hello är längst ned på den här artikeln en genomgång av en exempel-extrahering.

> [!NOTE]
> hello anpassat fält fylls som innehåller matchande hello angivna villkor läggs toohello OMS-databasen, så visas endast på poster som samlas in när hello anpassat fält har skapats.  hello anpassat fält läggs inte toorecords som redan finns i datalagret hello när den skapas.
> 
> 

### <a name="step-1--identify-records-that-will-have-hello-custom-field"></a>Steg 1 – identifiera poster som ska ha hello anpassat fält
hello första steget är tooidentify hello poster som får hello anpassat fält.  Du börjar med en [standard loggen Sök](log-analytics-log-searches.md) och välj sedan en post tooact som hello-modell som logganalys lära dig från.  När du anger att du ska tooextract data i ett anpassat fält hello **fältet Extraheringsguiden** öppnas där du validerar och förfina hello kriterier.

1. Gå för**loggen Sök** och använda en [fråga tooretrieve hello poster](log-analytics-log-searches.md) som har hello anpassat fält.
2. Markera en post att Log Analytics använder tooact som en modell för att extrahera data toopopulate hello anpassat fält.  Du identifierar hello data som du vill tooextract från den här posten och Log Analytics använder denna information toodetermine hello logik toopopulate hello anpassat fält för alla liknande poster.
3. Klicka på hello knappen toohello till vänster i en textegenskap av hello och välja **extrahera fält från**.
4. Hej **fältet Extraheringsguiden öppnas**, och hello-post som du har valt visas i hello **Main exempel** kolumn.  hello anpassat fält definieras posterna mot hello samma värden i hello egenskaper som är markerade.  
5. Om hello markeringen är inte exakt vad du vill kan du välja ytterligare fält toonarrow hello villkor.  I ordning toochange hello fältvärden för hello villkor, måste du avbryta och väljer en annan post matchar hello villkoren som du vill.

### <a name="step-2---perform-initial-extract"></a>Steg 2 – utför inledande extrahera.
När du har hittat hello-poster som ska ha hello anpassat fält, kan du identifiera hello data som du vill tooextract.  Log Analytics använder denna information tooidentify liknande mönster i liknande poster.  I hello steg efter detta du ska kunna toovalidate hello resultat och ange ytterligare information för Log Analytics toouse i sin analys.

1. Markera hello text i hello exempelpost som du vill toopopulate hello anpassat fält.  Sedan visas med en dialogrutan rutan tooprovide ett namn för hello fältet och tooperform hello inledande extrahera.  Hej tecken  **\_CF** läggs automatiskt.
2. Klicka på **extrahera** tooperform en analys av insamlade poster.  
3. Hej **sammanfattning** och **sökresultat** avsnitt visas hello resultaten av hello extrahera så kan du granska dess noggrannhet.  **Sammanfattning** visar hello kriterierna tooidentify poster och ett antal för varje hello datavärden identifieras.  **Sökresultat** ger en detaljerad lista över poster som matchar hello villkor.

### <a name="step-3--verify-accuracy-of-hello-extract-and-create-custom-field"></a>Steg 3 – verifiera noggrannhet hello extrahera och skapa anpassade fält
När du har utfört hello inledande extrahera visas logganalys resultaten baserat på data som redan samlats in.  Om hello resultatet ser ut korrekt kan du skapa hello anpassat fält med inget ytterligare arbete.  Om inte, du kan förfina hello resultat så att Log Analytics kan förbättra sin logik.

1. Om alla värden i hello inledande extrahera inte stämmer klickar hello **redigera** ikonen nästa tooan felaktig post och välj **ändra den här markeringen** i ordning toomodify hello markering.
2. hello-posten är kopierade toohello **ytterligare exempel** avsnitt under hello **Main exempel**.  Du kan justera hello Markera här toohelp logganalys förstå hello markeringen skulle ha gjort.
3. Klicka på **extrahera** toouse som den här nya information tooevaluate alla hello befintliga poster.  hello resultat kan ändras efter poster än hello något du just har ändrat baserat på den här nya intelligence.
4. Fortsätt tooadd korrigeringar tills alla poster i hello extrahera korrekt identifiera hello toopopulate hello nya anpassade fält.
5. Klicka på **spara extrahera** när du är nöjd med hello resultat.  hello anpassat fält definieras nu, men det kommer inte att lägga till tooany poster ännu.
6. Vänta tills nya poster som matchar hello ange kriterier toobe samlas in och kör sedan hello log-sökningen igen. Nya poster ska ha hello anpassat fält.
7. Använd hello anpassat fält som andra poster egenskapen.  Du kan använda det tooaggregate och gruppera data och även använda tooproduce nya insikter.

## <a name="viewing-custom-fields"></a>Visa anpassade fält
Du kan visa en lista över alla anpassade fält i din hanteringsgrupp från hello **inställningar** panelen hello OMS-instrumentpanelen.  Välj **Data** och sedan **anpassade fält** en lista över alla anpassade fält i arbetsytan.  

![Anpassade fält](media/log-analytics-custom-fields/list.png)

## <a name="removing-a-custom-field"></a>Ta bort ett anpassat fält
Det finns två sätt tooremove ett anpassat fält.  hello är först hello **ta bort** alternativ för varje fält när du visar hello lista enligt beskrivningen ovan.  hello är andra metoden tooretrieve en hello posten och klicka på knappen-toohello till vänster i hello-fältet.  hello menyn har ett anpassat fält för alternativet tooremove hello.

## <a name="sample-walkthrough"></a>Exempel genomgång
hello följande avsnitt beskriver hur en komplett exempel på hur du skapar anpassade fält.  Det här exemplet extraherar hello-tjänstnamnet i Windows-händelser som indikerar att en tjänst som ändrar tillstånd.  Detta är beroende av händelser som skapas av Service Control Manager i hello systemloggen på Windows-datorer.  Om du vill toofollow det här exemplet måste du vara [att samla in händelser med Information om hello systemloggen](log-analytics-data-sources-windows-events.md).

Vi ange hello följande fråga tooreturn alla händelser från Service Control Manager som har händelse-ID 7036 som är hello händelse som anger att en tjänst startar eller stoppar.

![Fråga](media/log-analytics-custom-fields/query.png)

Vi kan sedan välja en post med händelse-ID 7036.

![Posten i datakällan](media/log-analytics-custom-fields/source-record.png)

Vi vill hello tjänstnamn som visas i hello **RenderedDescription** egenskapen och välj hello knappen Nästa toothis.

![Extrahera fält](media/log-analytics-custom-fields/extract-fields.png)

Hej **fältet Extraheringsguiden** öppnas och hello **EventLog** och **EventID** fält har markerats i hello **Main exempel** kolumn.  Detta anger det Anpassa fältet hello definieras händelser från hello systemloggen med händelse-ID 7036.  Detta är tillräckliga så vi inte behöver tooselect andra fält.

![Main-exempel](media/log-analytics-custom-fields/main-example.png)

Vi fokusera hello namnet på hello tjänst i hello **RenderedDescription** egenskapen och Använd **Service** tooidentify hello tjänstnamn.  hello anpassat fält som ska anropas **Service_CF**.

![Fältet Rubrik](media/log-analytics-custom-fields/field-title.png)

Vi kan se att hello namn identifieras korrekt för vissa poster men inte för andra.   Hej **sökresultat** visas som en del av hello namn för hello **WMI-prestanda adaptern** inte valts.  Hej **sammanfattning** visar fyra poster med **DPRMA** service felaktigt ingår en extra word och två poster identifieras **moduler Installer** i stället för **Windows moduler Installer**.  

![Sökresultat](media/log-analytics-custom-fields/search-results-01.png)

Vi börjar med hello **WMI-prestanda adaptern** post.  Vi klickar på redigeringsikonen och sedan **ändra den här markeringen**.  

![Ändra markeringen](media/log-analytics-custom-fields/modify-highlight.png)

Vi öka hello markeringen tooinclude hello word **WMI** och kör sedan hello extrahera.  

![Ytterligare exempel](media/log-analytics-custom-fields/additional-example-01.png)

Kan vi se att hello poster för **WMI-prestanda adaptern** har korrigerats och logganalys också att toocorrect hello informationsposter för **Windows modulen Installer**.  Vi kan se i hello **sammanfattning** om avsnittet som **DPMRA** är fortfarande inte identifieras korrekt.

![Sökresultat](media/log-analytics-custom-fields/search-results-02.png)

Vi rulla tooa post med hello DPMRA-tjänsten och använder samma process toocorrect registrera hello.

![Ytterligare exempel](media/log-analytics-custom-fields/additional-example-02.png)

 När vi kör hello extrahering, kan vi se att alla våra resultat nu är korrekt.

![Sökresultat](media/log-analytics-custom-fields/search-results-03.png)

Vi kan se det **Service_CF** har skapats, men har ännu inte lagts tooany poster.

![Inledande antal](media/log-analytics-custom-fields/initial-count.png)

Efter en stund har passerat så nya händelser som samlas in, kan vi se som hello **Service_CF** fältet läggs nu toorecords som matchar våra villkor.

![Slutresultatet](media/log-analytics-custom-fields/final-results.png)

Vi kan nu använda hello anpassat fält som någon annan post-egenskap.  tooillustrate kan vi skapa en fråga som grupperar efter nya hello **Service_CF** fältet tooinspect tjänster som är mest aktiva hello.

![Grupp av frågan](media/log-analytics-custom-fields/query-group.png)

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [logga sökningar](log-analytics-log-searches.md) toobuild frågor med anpassade fält för villkoret.
* Övervakaren [anpassade loggfiler](log-analytics-data-sources-custom-logs.md) som du tolkas med anpassade fält.


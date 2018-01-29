---
title: "Analysera media med hjälp av Azure portal | Microsoft Docs"
description: "Det här avsnittet beskrivs hur du bearbetar media med Media Analytics-medieprocessorer (MP) med Azure-portalen."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 18213fc1-74f5-4074-a32b-02846fe90601
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: juliako
ms.openlocfilehash: 22032aef0cc8b7b015503043028873e70c21ee85
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="analyze-your-media-using-the-azure-portal"></a>Analysera dina medier med Azure Portal
> [!NOTE]
> Du behöver ett Azure-konto för att slutföra den här självstudien. Mer information finns i [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

## <a name="overview"></a>Översikt
Azure Media Services Analytics är en samling tal- och visionskomponenter komponenter (på företagsnivå, kompatibilitet, säkerhet och globalt omfattande) som gör det lättare för organisationer och företag att härleda insikter som går att åtgärda från sina videofiler. Mer detaljerad översikt över Azure Media Services Analytics finns [detta](media-services-analytics-overview.md) avsnittet. 

Det här avsnittet beskrivs hur du bearbetar media med Media Analytics-medieprocessorer (MP) med Azure-portalen. Media Analytics MP producerar MP4-filer eller JSON-filer. Om en medieprocessor har producerat en MP4-fil kan du hämta filen progressivt. Om en medieprocessor har producerat en JSON-fil kan du hämta filen från Azure Blob Storage. 

## <a name="choose-an-asset-that-you-want-to-analyze"></a>Välj en tillgång som du vill analysera
1. Välj ditt Azure Media Services-konto i [Azure-portalen](https://portal.azure.com/).
2. I fönstret **Inställningar** väljer du **Tillgångar**.  
   .
    ![Analysera videor](./media/media-services-portal-analyze/media-services-portal-analyze001.png)
3. Välj den tillgång som du vill analysera och tryck på den **analysera** knappen.
   
    ![Analysera videor](./media/media-services-portal-analyze/media-services-portal-analyze002.png)
4. I den **processen media tillgång med Media Analytics** fönstret Välj processorn. 
   
    Resten av artikeln förklarar varför och hur du använder varje processor. 
5. Tryck på **skapa** att starta ett jobb.

## <a name="azure-media-indexer"></a>Azure Media Indexer
Den **Azure Media Indexer** medieprocessor kan du göra mediefiler och innehåll sökbara samt generera stängd textning spår. Detta avsnitt ger information om alternativ som du kan ange för denna MP.

![Analysera videor](./media/media-services-portal-analyze/media-services-portal-analyze003.png)

### <a name="language"></a>Språk
Naturligt språk ska kunna identifieras i fil. Till exempel engelska och spanska. 

### <a name="captions"></a>Etiketter
Du kan välja en etikett-format som kommer att genereras från ditt innehåll. En indexering jobb kan generera textning filer i följande format:  

* **SAMISKA**
* **TTML**
* **WebVTT**

Stängd beskrivning (CC) filer i dessa format kan användas för att göra det tillgängligt för personer med funktionshinder hörsel ljud- och bildfiler.

### <a name="aib-file"></a>AIB fil
Välj det här alternativet om du vill generera ljud Index Blob-fil för användning med anpassade SQL Server IFilter. Mer information finns i [detta](https://azure.microsoft.com/blog/using-aib-files-with-azure-media-indexer-and-sql-server/) blogg.

### <a name="keywords"></a>Nyckelord
Välj det här alternativet om du vill generera ett nyckelord XML-fil. Den här filen innehåller nyckelord som extraheras från tal-innehåll med frekvens och offset-information.

### <a name="job-name"></a>Jobbnamn
Ett eget namn som gör att du kan identifiera jobbet. [Detta](media-services-portal-check-job-progress.md) artikeln beskriver hur du kan övervaka förloppet för ett jobb. 

### <a name="output-file"></a>Utdatafil
Ett eget namn som gör att du kan identifiera vilket innehåll som utdata. 

## <a name="azure-media-hyperlapse"></a>Azure Media Hyperlapse
Azure Media Hyperlapse är en HP som skapar smooth tid upphörde att gälla videor från första person eller åtgärd kamera innehåll.  Mer information finns i [detta](media-services-hyperlapse-content.md) avsnitt. Detta avsnitt ger information om alternativ som du kan ange för denna MP.

![Analysera videor](./media/media-services-portal-analyze/media-services-portal-analyze004.png)

### <a name="speed"></a>Hastighet
Ange den hastighet som kan påskynda inkommande videon. Utdata är ett stabilt och tid slut återgivning av video indata.

### <a name="job-name"></a>Jobbnamn
Ett eget namn som gör att du kan identifiera jobbet. [Detta](media-services-portal-check-job-progress.md) artikeln beskriver hur du kan övervaka förloppet för ett jobb. 

### <a name="output-file"></a>Utdatafil
Ett eget namn som gör att du kan identifiera vilket innehåll som utdata. 

## <a name="azure-media-face-detector"></a>Azure Media Face Detector
Den **Azure Media ansikte detektor** medieprocessor (HP) kan du räkna, spåra förflyttningar och även mäta målgruppen deltagande och reaktion via ansikte uttryck. Den här tjänsten innehåller två funktioner: 

* **Ansikts-identifiering**
  
    Identifiering av framsidan hittar och spårar mänsklig ytor inom en video. Flera ytor kan identifieras och spåras senare när de flyttas runt, med tid och plats metadata som returneras i en JSON-fil. Under spårning, görs ett försök att ge en konsekvent ID till samma sida när personen flyttar på skärmen, även om de hindras eller lämna en kort ramen.
  
  > [!NOTE]
  > Den här tjänster inte att utföra ansiktsigenkänning. En person som lämnar ramen eller blir hindras för länge får ett nytt-ID när de kommer tillbaka.
  > 
  > 
* **Känsloigenkänning**
  
    Känsloigenkänning är en valfri komponent i ansikte identifiering Medieprocessor som returnerar analysis på flera känslomässiga attribut från ytor upptäckt, inklusive lycka, sadness, fruktan, resulterande och mycket mer. 

![Analysera videor](./media/media-services-portal-analyze/media-services-portal-analyze005.png)

### <a name="detection-mode"></a>Identifieringsläget
Något av följande lägen kan användas av processorn:

* ansikts-identifiering
* per ansikte känsloigenkänning
* sammanställd känsloigenkänning

### <a name="job-name"></a>Jobbnamn
Ett eget namn som gör att du kan identifiera jobbet. [Detta](media-services-portal-check-job-progress.md) artikeln beskriver hur du kan övervaka förloppet för ett jobb. 

### <a name="output-file"></a>Utdatafil
Ett eget namn som gör att du kan identifiera vilket innehåll som utdata. 

## <a name="azure-media-motion-detector"></a>Azure Media Motion Detector
Den **Azure Media rörelsedetektor** medieprocessor (HP) kan du effektivt identifiera avsnitt av intresse i ett annat långa och primärdomänkontrollant video. Rörelseidentifiering kan användas på statisk övervakningskameror för att identifiera avsnitt av videon där rörelse inträffar. Den genererar en JSON-fil som innehåller en metadata med tidsstämplar och den omgivande region där händelsen inträffade.

Riktad mot säkerhet video feeds kan den här tekniken kategorisera rörelse i relevanta händelser och falska positiva identifieringar som skuggor och andra ändringar. På så sätt kan du skapa säkerhetsaviseringar från kameran feeds utan som skräppost med oändlig irrelevanta händelser när kunna extrahera stund intressanta från extremt långa övervakning videor.

![Analysera videor](./media/media-services-portal-analyze/media-services-portal-analyze006.png)

## <a name="azure-media-video-thumbnails"></a>Azure Media Video Thumbnails
Med hjälp av den här processorn kan du skapa sammanfattningar av långa videor automatiskt genom att intressanta kodavsnitt från källvideo. Detta är användbart när du vill ge en snabb överblick över vad som händer i en lång video. Mer detaljerad information och exempel finns [Använd Azure Media Video-miniatyrer för att skapa en Videosammanfattning](media-services-video-summarization.md)

![Analysera videor](./media/media-services-portal-analyze/media-services-portal-analyze008.png)

### <a name="job-name"></a>Jobbnamn
Ett eget namn som gör att du kan identifiera jobbet. [Detta](media-services-portal-check-job-progress.md) artikeln beskriver hur du kan övervaka förloppet för ett jobb. 

### <a name="output-file"></a>Utdatafil
Ett eget namn som gör att du kan identifiera vilket innehåll som utdata. 

## <a name="next-steps"></a>Nästa steg
Visa Media Services utbildningsvägar.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


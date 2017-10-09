---
title: "aaaAnalyze media med hjälp av hello Azure-portalen | Microsoft Docs"
description: "Det här avsnittet beskrivs hur tooprocess media med Media Analytics-medieprocessorer (MP) med hjälp av hello Azure-portalen."
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
ms.openlocfilehash: d549c0315cd58c3771420379316b4f2ad3c2cea6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-your-media-using-hello-azure-portal"></a>Analysera dina media med hello Azure-portalen
> [!NOTE]
> toocomplete den här självstudiekursen kommer du behöver ett Azure-konto. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

## <a name="overview"></a>Översikt
Azure Media Services Analytics är en samling tal- och visionskomponenter komponenter (på företagsnivå, kompatibilitet, säkerhet och globalt omfattande) som gör det lättare för organisationer och företag tooderive tillämplig insikter från sina videofiler. Mer detaljerad översikt över Azure Media Services Analytics finns [detta](media-services-analytics-overview.md) avsnittet. 

Det här avsnittet beskrivs hur tooprocess media med Media Analytics-medieprocessorer (MP) med hjälp av hello Azure-portalen. Media Analytics MP producerar MP4-filer eller JSON-filer. Om en medieprocessor har producerat en MP4-fil kan hämta du progressivt hello-filen. Om en medieprocessor har producerat en JSON-fil kan du ladda ned hello filen från hello Azure blob storage. 

## <a name="choose-an-asset-that-you-want-tooanalyze"></a>Välj en tillgång som du vill tooanalyze
1. I hello [Azure-portalen](https://portal.azure.com/), Välj Azure Media Services-konto.
2. I hello **inställningar** väljer **tillgångar**.  
   .
    ![Analysera videor](./media/media-services-portal-analyze/media-services-portal-analyze001.png)
3. Välj hello tillgång som du vill att tooanalyze och tryck på hello **analysera** knappen.
   
    ![Analysera videor](./media/media-services-portal-analyze/media-services-portal-analyze002.png)
4. I hello **processen media tillgång med Media Analytics** fönster, Välj hello processor. 
   
    hello resten av hello artikel som förklarar varför och hur toouse varje processor. 
5. Tryck på **skapa** toohello starta ett jobb.

## <a name="azure-media-indexer"></a>Azure Media Indexer
Hej **Azure Media Indexer** medieprocessor kan du toomake mediefiler och innehåll sökbara, samt generera stängd textning spår. Detta avsnitt ger information om alternativ som du kan ange för denna MP.

![Analysera videor](./media/media-services-portal-analyze/media-services-portal-analyze003.png)

### <a name="language"></a>Språk
hello naturligt språk toobe identifieras i hello fil. Till exempel engelska och spanska. 

### <a name="captions"></a>Etiketter
Du kan välja en etikett-format som kommer att genereras från ditt innehåll. En indexering jobb kan generera textning filer i hello följande format:  

* **SAMISKA**
* **TTML**
* **WebVTT**

Stängd beskrivning (CC) filer i dessa format kan vara används toomake ljud och video filer tillgänglig toopeople med hörsel funktionshinder.

### <a name="aib-file"></a>AIB fil
Välj det här alternativet om du skulle t.ex toogenerate hello ljud Index Blob-fil för användning med hello anpassade IFilter för SQL Server. Mer information finns i [detta](https://azure.microsoft.com/blog/using-aib-files-with-azure-media-indexer-and-sql-server/) blogg.

### <a name="keywords"></a>Nyckelord
Välj det här alternativet om du vill att toogenerate en nyckelord XML-fil. Den här filen innehåller nyckelord som extraheras från hello tal innehåll med frekvens och offset-information.

### <a name="job-name"></a>Jobbnamn
Ett eget namn som gör att du kan identifiera hello jobb. [Detta](media-services-portal-check-job-progress.md) artikeln beskriver hur du övervakar hello förloppet för ett jobb. 

### <a name="output-file"></a>Utdatafil
Ett eget namn som gör att du kan identifiera hello utdata innehåll. 

## <a name="azure-media-hyperlapse"></a>Azure Media Hyperlapse
Azure Media Hyperlapse är en HP som skapar smooth tid upphörde att gälla videor från första person eller åtgärd kamera innehåll.  Mer information finns i [detta](media-services-hyperlapse-content.md) avsnitt. Detta avsnitt ger information om alternativ som du kan ange för denna MP.

![Analysera videor](./media/media-services-portal-analyze/media-services-portal-analyze004.png)

### <a name="speed"></a>Hastighet
Ange hello hastighet med vilken toospeed in hello indatavideo. hello-utdata är ett stabilt och tid slut återgivning av hello indatavideo.

### <a name="job-name"></a>Jobbnamn
Ett eget namn som gör att du kan identifiera hello jobb. [Detta](media-services-portal-check-job-progress.md) artikeln beskriver hur du övervakar hello förloppet för ett jobb. 

### <a name="output-file"></a>Utdatafil
Ett eget namn som gör att du kan identifiera hello utdata innehåll. 

## <a name="azure-media-face-detector"></a>Azure Media Face Detector
Hej **Azure Media ansikte detektor** medieprocessor (HP) kan du toocount, spåra förflyttningar och även mätaren målgruppen deltagande och reaktion via ansikte uttryck. Den här tjänsten innehåller två funktioner: 

* **Ansikts-identifiering**
  
    Identifiering av framsidan hittar och spårar mänsklig ytor inom en video. Flera ytor kan identifieras och spåras senare när de flyttas runt, med hello tid och plats metadata som returneras i en JSON-fil. Under spårning försöker toogive en konsekvent ID toohello samma står inför när hello person flyttar på skärmen, även om de hindras eller lämna en kort hello ram.
  
  > [!NOTE]
  > Den här tjänster inte att utföra ansiktsigenkänning. En person som lämnar hello ram eller blir hindras för länge får ett nytt-ID när de kommer tillbaka.
  > 
  > 
* **Känsloigenkänning**
  
    Känsloigenkänning är en valfri komponent i hello ansikte identifiering Medieprocessor som returnerar analysis på flera känslomässiga attribut från hello ytor upptäckt, inklusive lycka, sadness, fruktan, resulterande och mycket mer. 

![Analysera videor](./media/media-services-portal-analyze/media-services-portal-analyze005.png)

### <a name="detection-mode"></a>Identifieringsläget
Något av följande lägen hello kan användas av hello processor:

* ansikts-identifiering
* per ansikte känsloigenkänning
* sammanställd känsloigenkänning

### <a name="job-name"></a>Jobbnamn
Ett eget namn som gör att du kan identifiera hello jobb. [Detta](media-services-portal-check-job-progress.md) artikeln beskriver hur du övervakar hello förloppet för ett jobb. 

### <a name="output-file"></a>Utdatafil
Ett eget namn som gör att du kan identifiera hello utdata innehåll. 

## <a name="azure-media-motion-detector"></a>Azure Media Motion Detector
Hej **Azure Media rörelsedetektor** media-processor (HP) aktiverar du tooefficiently identifierar avsnitt av intresse i ett annat långa och primärdomänkontrollant video. Rörelseidentifiering kan användas på statisk kamera tagning tooidentify avsnitt av hello video där rörelse inträffar. Den genererar en JSON-fil som innehåller en metadata med tidsstämplar och hello avgränsar region där hello händelsen inträffade.

Den här tekniken är riktade mot säkerhet video feeds kan toocategorize rörelse in relevanta händelser och falska positiva identifieringar som skuggor och andra ändringar. Detta ger dig toogenerate säkerhetsaviseringar från kameran feeds utan som skräppost med oändlig irrelevanta händelser inte kan tooextract stund intressanta från extremt långa övervakning videor.

![Analysera videor](./media/media-services-portal-analyze/media-services-portal-analyze006.png)

## <a name="azure-media-video-thumbnails"></a>Azure Media Video Thumbnails
Med hjälp av den här processorn kan du skapa sammanfattningar av långa videor automatiskt genom att intressanta kodavsnitt från hello källa video. Detta är användbart när du vill tooprovide en snabb överblick över vilka tooexpect i en lång video. Mer detaljerad information och exempel finns [Använd Azure Media Video-miniatyrer tooCreate en Videosammanfattning](media-services-video-summarization.md)

![Analysera videor](./media/media-services-portal-analyze/media-services-portal-analyze008.png)

### <a name="job-name"></a>Jobbnamn
Ett eget namn som gör att du kan identifiera hello jobb. [Detta](media-services-portal-check-job-progress.md) artikeln beskriver hur du övervakar hello förloppet för ett jobb. 

### <a name="output-file"></a>Utdatafil
Ett eget namn som gör att du kan identifiera hello utdata innehåll. 

## <a name="next-steps"></a>Nästa steg
Visa Media Services utbildningsvägar.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


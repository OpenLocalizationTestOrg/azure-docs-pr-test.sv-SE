---
title: "aaaMedia Analytics på hello Media Services plattform | Microsoft Docs"
description: "Översikt över förhandsversion av Media Analytics, en samling tal- och vision tjänster på företagsnivå, kompatibilitet, säkerhet och globalt omfattande"
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: c56e3781-8510-4f7f-b5ff-a218c1bb6f4c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2017
ms.author: milanga;juliako;johndeu
ms.openlocfilehash: 7545f0532d7618164ebe65e2f4232c5f63453cfd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="media-analytics-on-hello-media-services-platform"></a>Media Analytics på hello Media Services-plattformen
## <a name="overview"></a>Översikt
Flera organisationer använder video som hello prioriterade medelhög tootrain sina anställda, engagera sina kunder och IT-funktioner på dokumentet. Molnet innehåller en sätt toostore strömma och komma åt dessa stora mediefiler. Men eftersom ett bibliotek med videoinnehåll växer måste ett lika effektivt sätt att extrahera insikter från hello innehåll. 

tooaddress detta växande behov Azure Media Services erbjuder Azure Media Analytics. Media Analytics är en samling tal-och visionskomponenter som gör det enklare för organisationer och företag tooderive tillämplig insikter från sina videofiler. Inbyggda med hjälp av hello kärnkomponenter Media Services-plattformen kan Media Analytics hantera media bearbetning i stor skala på dagen.

Med Media Analytics sätta utvecklare snabbt avancerade video funktionerna i program. Det ger företagsmiljöer hello full skala, kompatibilitet, säkerhet och globalt omfattande krävs för stora organisationer.

hello följande diagram visar Media Analytics och andra viktiga delar av hello Media Services-plattformen. 

![VoD-arbetsflöde](./media/media-services-analytics-overview/media-services-analytics-overview01.png)

Media Analytics-medieprocessorer producerar MP4-filer eller JSON-filer. Om en medieprocessor producerar MP4-fil, kan du progressivt hämta hello-fil. Om en medieprocessor producerar en JSON-fil, kan du ladda ned hello filen från Azure Blob storage. 

## <a name="media-analytics-services"></a>Media Analytics-tjänster

### <a name="indexer"></a>Indexerare
Med Azure Media Indexer ska göra du innehåll sökbara och generera-textning spår. Toohello jämfört med tidigare version, Azure Media Indexer 2 Preview har stöd för snabbare indexering och bredare språk. Språk som stöds är engelska, spanska, franska, tyska, italienska, kinesiska, portugisiska och arabiska. Mer detaljerad information och exempel finns [bearbeta videor med Azure Media Indexer 2](media-services-process-content-with-indexer2.md).
### <a name="hyperlapse"></a>Videostabilisering
Microsoft Hyperlapse kombinerar videostabiliserings och tidsfördröjning kapaciteten toocreate snabbt och använda videor från långa-innehåll. Förutom att skapa tidsfördröjning video kan använda du Videostabilisera toocreate stabil videor från shaky videor som hämtats via mobiltelefoner och kameror. Mer detaljerad information och exempel finns [Videostabilisera mediefiler med Azure Media Hyperlapse](media-services-hyperlapse-content.md).
### <a name="motion-detector"></a>Rörelsedetektor
Du kan använda rörelsedetektor toodetect rörelse i en video med stilla bakgrund. Detta gör det möjligt toocheck för falska positiva identifieringar på rörelse händelser som identifieras av övervakningskameror. Mer detaljerad information och exempel finns [Rörelseidentifiering för Azure Media Analytics](media-services-motion-detection.md).
### <a name="face-detector"></a>Ansiktsigenkänning
Med hjälp av framsidan detektor, kan du identifiera personer ytor och deras emotikoner, inklusive lycka, sadness och oväntat. Detta har flera användbara branschspecifika program, beskrivs senare, inklusive aggregering och analys av reaktion på personer som deltar i en händelse. Mer detaljerad information och exempel finns [Ansikts- och känslo identifiering för Azure Media Analytics](media-services-face-and-emotion-detection.md).
### <a name="video-summarization"></a>Sammanfattning av video
Sammanfattning av video kan hjälpa dig skapa sammanfattningar av långa videor automatiskt genom att intressanta kodavsnitt från hello källa video. Den här möjligheten är användbart när du vill tooprovide en snabb överblick över vilka tooexpect i en lång video. Mer detaljerad information och exempel finns [sammanfattning av video Använd Azure Media Video-miniatyrer toocreate](media-services-video-summarization.md).
### <a name="optical-character-recognition"></a>Optisk teckenigenkänning
Du kan konvertera textinnehåll i videofiler till redigerbar, sökbara digitala text med Azure Media OCR (OCR). Sedan kan du automatisera hello extrahering av beskrivande metadata från hello video signal medieinnehåll.
### <a name="scalable-face-redaction"></a>Skalbar ansikte bortredigering
Azure Media Redactor är en Media Analytics medieprocessor som erbjuder skalbara ansikte bortredigering i hello molnet. Du kan ändra din video tooblur ytor valda personer med hjälp av framsidan bortredigering. Du kanske vill toouse hello ansikte bortredigering tjänst i Nyheter media eller allmän säkerhet ingår. Några minuter med material som innehåller flera ytor kan ta timmar tooredact manuellt, men med den här tjänsten ansikte bortredigering tar bara några få enkla steg. Mer information finns i hello [redigera bort personerna bakom Azure Media Analytics](media-services-face-redaction.md) artikel.

## <a name="common-scenarios"></a>Vanliga scenarier
Media Analytics hjälper organisationer och företag glean nya insikter från video och mer hantera effektivt stora volymer av videoinnehåll. Här följer några scenarier:

* **Anropa Datacenter**. Även om hello ankomsten av sociala medier underlätta kunden call Center fortfarande en stor del av kundservice transaktioner. Kodning i den här ljuddata är en stor mängd kundinformation som kan vara analyseras tooachieve högre nöjda. Med hjälp av Media indexeraren organisationer extrahera text och skapa sökindex och instrumentpaneler. De kan sedan extrahera intelligence runt vanligt klagomål, källor klagomål och andra relevanta data.
* **Användargenererade innehåll avbrottsmoderering**. Många organisationer har offentliga portaler som accepterar användargenererade media, till exempel videoklipp och bilder från nyheter media uttag toopolice avdelningar. hello mängd innehåll kan ökar på grund av toounexpected händelser. I dessa scenarier är det svårt tooconduct effektiva manuell granskning av innehållet för lämplighet. Kunder kan förlita dig på hello innehåll avbrottsmoderering service toofocus på innehåll som är lämpligt.
* **Övervakning**. Med hello kommer tillväxt används av IP-kameror en växande inventering av övervakning video. Manuell granskning övervakning video är intensiv och felbenägna toohuman fel. Media Analytics tillhandahåller tjänster som rörelseidentifiering, står inför identifiering och Videostabilisera toomake hello processen för granskning, hantera och skapa derivat enklare.

## <a name="media-analytics-media-processors"></a>Media Analytics-medieprocessorer
Detta avsnitt visar hello Media Analytics-medieprocessorer och visar hur toouse .NET eller REST tooget objektet en media-processor (HP).

### <a name="mp-names"></a>HP-namn
* Azure Media Indexer 2 Preview
* Azure Media Indexer
* Azure Media Hyperlapse
* Azure Media Face Detector
* Azure Media Motion Detector
* Azure Media Video Thumbnails
* Azure Media OCR

### <a name="net"></a>.NET
hello följande tar en av hello angetts MP namn och returnerar ett HP-objekt.

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors
            .Where(p => p.Name == mediaProcessorName)
            .ToList()
            .OrderBy(p => new Version(p.Version))
            .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }


### <a name="rest"></a>REST
Begäran:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Azure%20Media%20OCR' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.12
    Host: media.windows.net

Svar:

    . . .

    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:074c3899-d9fb-448f-9ae1-4ebcbe633056",
             "Description":"Azure Media OCR",
             "Name":"Azure Media OCR",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

## <a name="demos"></a>Demonstrationer
Se [Azure Media Analytics demonstrationer](http://azuremedialabs.azurewebsites.net/demos/Analytics.html).

## <a name="next-steps"></a>Nästa steg
Granska sökvägarna för Media Services-utbildning.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>Relaterade artiklar
Se [Media Services Analytics meddelande](https://azure.microsoft.com/blog/introducing-azure-media-analytics/).

<!-- Images -->

[overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png

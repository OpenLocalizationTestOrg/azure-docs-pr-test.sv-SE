---
title: "aaaHow tooencode en Azure-tillgång med Media Encoder Standard | Microsoft Docs"
description: "Lär dig hur toouse Media Encoder Standard tooencode media innehåll på Azure Media Services. Kodexempel använda REST API."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 2a7273c6-8a22-4f82-9bfe-4509ff32d4a4
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: b766bafded7ee98eda3e6ef149c31d5d8fe406fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooencode-an-asset-by-using-media-encoder-standard"></a>Hur tooencode en tillgång med Media Encoder Standard
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-encode-with-media-encoder-standard.md)
> * [REST](media-services-rest-encode-asset.md)
> * [Portal](media-services-portal-encode.md)
>
>

## <a name="overview"></a>Översikt
toodeliver digital video via hello Internet, måste du komprimera hello media. Digitala videofiler är stort och kan vara för stort toodeliver via hello Internet eller för dina kunders enheter toodisplay korrekt. Kodning är hello process för att komprimera video och ljud så att kunderna kan visa dina media.

Kodning jobb är en hello vanligaste bearbetningen i Azure Media Services. Du skapar kodning jobb tooconvert media-filer från en kodning tooanother. När du kodar kan du använda hello Media Services inbyggda-kodare (Media Encoder Standard). Du kan också använda en kodare som tillhandahålls av Media Services partner. Kodare från tredje part är tillgängliga via hello Azure Marketplace. Du kan ange hello information om kodning uppgifter med hjälp av förinställda strängar som definierats för din kodare eller med hjälp av förinställda konfigurationsfiler. toosee hello typer av förinställda som är tillgängliga, se [aktivitet förinställningar för Media Encoder Standard](http://msdn.microsoft.com/library/mt269960).

Varje jobb kan ha en eller flera aktiviteter beroende på hello typ av bearbetning som du vill tooaccomplish. Via hello REST-API, kan du skapa jobb och deras relaterade uppgifter i ett av två sätt:

* Aktiviteter kan vara definierade infogade via hello uppgifter navigeringsegenskap på jobbet entiteter.
* Använd OData-batch-bearbetning.

Vi rekommenderar att du alltid koda källfilerna till en MP4-uppsättningen med anpassad bithastighet och sedan konvertera hello set toohello önskat format med hjälp av [dynamisk paketering](media-services-dynamic-packaging-overview.md).

Om din utdatatillgången är lagringskrypterad, måste du konfigurera hello tillgångsleveransprincip. Mer information finns i [konfigurera tillgångsleveransprincip](media-services-rest-configure-asset-delivery-policy.md).

## <a name="considerations"></a>Överväganden

Vid åtkomst till entiteter i Media Services måste du ange specifika namn på huvudfält och värden i HTTP-begäranden. Mer information finns i [installationsprogrammet för Media Services REST API-utveckling](media-services-rest-how-to-use.md).

Innan du börjar refererar till media processorer, kontrollera att du har hello rätt media processor-ID. Mer information finns i [hämta medieprocessorer](media-services-rest-get-media-processor.md).

## <a name="connect-toomedia-services"></a>Ansluta tooMedia tjänster

Mer information om hur tooconnect toohello AMS API, se [åtkomst hello Azure Media Services API med Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md). 

>[!NOTE]
>När du har anslutit toohttps://media.windows.net, får du en 301 omdirigering att ange en annan Media Services-URI. Du måste göra följande anrop toohello ny URI.

## <a name="create-a-job-with-a-single-encoding-task"></a>Skapa ett jobb med en enda kodning aktivitet
> [!NOTE]
> När du arbetar med hello Media Services REST API gäller hello följande:
>
> Vid åtkomst till entiteter i Media Services måste du ange specifika namn på huvudfält och värden i HTTP-begäranden. Mer information finns i [installationsprogrammet för Media Services REST API-utveckling](media-services-rest-how-to-use.md).
>
> När du har anslutit toohttps://media.windows.net, får du en 301 omdirigering att ange en annan Media Services-URI. Du måste göra följande anrop toohello ny URI. Mer information om hur tooconnect toohello AMS API, se [åtkomst hello Azure Media Services API med Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).
>
> När med JSON och ange toouse hello **__metadata** nyckelord i hello-begäran (till exempel tooreferences ett länkat kvar), måste du ange hello **acceptera** huvud för[utförlig JSON-format ](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/): Acceptera: application/json; odata = utförlig.
>
>

hello som följande exempel visar hur toocreate och efter ett jobb med en uppgift ange tooencode en video på en viss upplösning och kvalitet. När du kodar med Media Encoder Standard kan du använda aktiviteten configuration förinställningar anges [här](http://msdn.microsoft.com/library/mt269960).

Begäran:

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    Host: media.windows.net

    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aaab7f15b-3136-4ddf-9962-e9ecb28fb9d2')"}}],  "Tasks" : [{"Configuration" : "Adaptive Streaming", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",  "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"}]}

Svar:

    HTTP/1.1 201 Created

    . . .

### <a name="set-hello-output-assets-name"></a>Ange namnet på hello utdata tillgångsinformation
hello som följande exempel visar hur tooset hello assetName attribut:

    { "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"}

## <a name="considerations"></a>Överväganden
* TaskBody egenskaper måste använda literal XML toodefine hello antal indata eller utdata tillgångar som används av hello-aktivitet. hello avsnittet innehåller hello XML Schema Definition för hello XML.
* I hello TaskBody definition, varje inre värde för <inputAsset> och <outputAsset> måste anges som JobInputAsset(value) eller JobOutputAsset(value).
* En aktivitet kan innehålla flera utdata tillgångar. En JobOutputAsset(x) kan bara användas en gång som utdata för en aktivitet i ett jobb.
* Du kan ange JobInputAsset eller JobOutputAsset som inkommande tillgång för en aktivitet.
* Aktiviteter måste inte utgör en cykel.
* Parametern för hello-värde som du skickar tooJobInputAsset eller JobOutputAsset representerar hello indexvärdet för en tillgång. hello faktiska tillgångar har definierats i hello InputMediaAssets och OutputMediaAssets navigeringsegenskaper på hello jobbdefinitionen entitet.
* Eftersom Media Services bygger på OData v3 hello enskilda tillgångar i hello InputMediaAssets och OutputMediaAssets navigering egenskapsuppsättningar refereras via en ”__metadata: uri” namn / värde-par.
* InputMediaAssets mappar tooone eller flera resurser som du skapade i Media Services. OutputMediaAssets skapas med hello system. De refererar inte till en befintlig tillgång.
* OutputMediaAssets kan namnges genom att använda hello assetName attributet. Om det här attributet är inte tillgängligt och sedan hello hello OutputMediaAsset heter det inre textvärdet hello hello <outputAsset> elementet är med suffixet hello jobbnamn värde eller hello jobb-Id-värde (i hello fall där hello namnegenskapen har inte definierats). Till exempel om du anger ett värde för assetName Sample för ”Sample” och sedan hello OutputMediaAsset Name-egenskapen anges för ””. Om du inte ange ett värde för assetName men ställts in hello jobbnamn skulle för ”NewJob” och sedan hello OutputMediaAsset namn dock vara ”JobOutputAsset (värde) _NewJob”.

## <a name="create-a-job-with-chained-tasks"></a>Skapa ett jobb med länkad uppgifter
I många Programscenarier vill utvecklare toocreate en serie bearbetningsåtgärder. Du kan skapa en serie länkad aktiviteter i Media Services. Varje aktivitet utför steg för behandling och kan använda olika media processorer. Hej sammankedjade uppgifter kan lämnar in en tillgång från en aktivitet tooanother, utför en linjär aktivitetssekvensen på hello tillgången. Hello-uppgifter som utförs i ett jobb är dock inte obligatoriskt toobe i en sekvens. När du skapar en länkad aktivitet hello sammankedjade **ITask** objekt som skapas i en enda **IJob** objekt.

> [!NOTE]
> Det finns en gräns på 30 uppgifter per jobb. Om du behöver toochain mer än 30 uppgifter kan du skapa mer än ett jobb toocontain hello uppgifter.
>
>

    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://testrest.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A910ffdc1-2e25-4b17-8a42-61ffd4b8914c')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          },
          {  
             "Configuration":"H264 Smooth Streaming 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-16\"?><taskBody><inputAsset>JobOutputAsset(0)</inputAsset><outputAsset>JobOutputAsset(1)</outputAsset></taskBody>"
          }
       ]
    }


### <a name="considerations"></a>Överväganden
tooenable länkning för uppgiften:

* Ett jobb måste ha minst två aktiviteter.
* Det måste finnas minst en aktivitet vars indata är hello utdata från en annan aktivitet i hello-jobbet.

## <a name="use-odata-batch-processing"></a>Använd OData-batch-bearbetning
hello som följande exempel visar hur toouse OData batch bearbetning toocreate ett jobb och uppgifter. Mer information om batchbearbetning finns [Open Data Protocol (OData) gruppbearbetning](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).

    POST https://media.windows.net/api/$batch HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: multipart/mixed; boundary=batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Accept: multipart/mixed
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    Host: media.windows.net


    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Content-Type: multipart/mixed; boundary=changeset_122fb0a4-cd80-4958-820f-346309967e4d

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary

    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-ID: 1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {"Name" : "NewTestJob", "InputMediaAssets@odata.bind":["https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A2a22445d-1500-80c6-4b34-f1e5190d33c6')"]}

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary

    POST https://media.windows.net/api/$1/Tasks HTTP/1.1
    Content-ID: 2
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {  
       "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
       "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
       "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"Custom output name\">JobOutputAsset(0)</outputAsset></taskBody>"
    }

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d--
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e--



## <a name="create-a-job-by-using-a-jobtemplate"></a>Skapa ett jobb med hjälp av en jobbmall
När du bearbetar flera resurser med hjälp av en gemensam uppsättning aktiviteter, Använd en jobbmall toospecify hello standarduppgiften förinställningar eller tooset hello ordningen för aktiviteter.

hello som följande exempel visar hur toocreate en jobbmall med en TaskTemplate är infogade datadefinitioner. Hej TaskTemplate använder hello Media Encoder Standard som hello MediaProcessor tooencode hello resursfil. Dock kan andra MediaProcessors också användas.

    POST https://media.windows.net/API/JobTemplates HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net


    {"Name" : "NewJobTemplate25", "JobTemplateBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><jobTemplate><taskBody taskTemplateId=\"nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789\"><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody></jobTemplate>", "TaskTemplates" : [{"Id" : "nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789", "Configuration" : "H264 Smooth Streaming 720p", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56", "Name" : "SampleTaskTemplate2", "NumberofInputAssets" : 1, "NumberofOutputAssets" : 1}] }


> [!NOTE]
> Till skillnad från andra Media Services-enheter måste du definiera en ny GUID-identifierare för varje TaskTemplate och placera det i hello taskTemplateId och Id-egenskap i frågans brödtext. hello innehåll identifieringsschemat måste följa hello schemat som beskrivs i identifiera Azure Media Services entiteter. JobTemplates kan inte uppdateras. I stället måste du skapa en ny med ändringarna uppdaterade.
>
>

Om det lyckas, returneras hello efter svar:

    HTTP/1.1 201 Created

    . . .


följande exempel visar hur hello toocreate ett jobb som refererar till en jobbmall-Id:

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net


    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A3f1fe4a2-68f5-4190-9557-cd45beccef92')"}}], "TemplateId" : "nb:jtid:UUID:15e6e5e6-ac85-084e-9dc2-db3645fbf0aa"}


Om det lyckas, returneras hello efter svar:

    HTTP/1.1 201 Created

    . . .



## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Nästa steg
Nu när du vet hur toocreate ett jobb tooencode en tillgång, se [hur toocheck jobb pågår med Media Services](media-services-rest-check-job-progress.md).

## <a name="see-also"></a>Se även
[Hämta Media-processorer](media-services-rest-get-media-processor.md)

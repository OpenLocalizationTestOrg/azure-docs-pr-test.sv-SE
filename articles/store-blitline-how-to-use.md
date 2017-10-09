---
title: "aaaHow toouse Blitline för bearbetning av bilden – Azure funktion guide"
description: "Lär dig hur toouse hello Blitline tjänsten tooprocess bilderna i ett Azure-program."
services: 
documentationcenter: .net
author: blitline-dev
manager: jason@blitline.com
editor: jason@blitline.com
ms.assetid: 6c711248-0e52-4895-ba9e-8395628de924
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2014
ms.author: support@blitline.com
ms.openlocfilehash: 328fd177e25f45f29f8ad8e142d02b46017a858e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blitline-with-azure-and-azure-storage"></a>Hur toouse Blitline med Azure och Azure Storage
Den här guiden förklarar hur tooaccess Blitline tjänster och hur toosubmit jobb tooBlitline.

## <a name="what-is-blitline"></a>Vad är Blitline?
Blitline är en molnbaserad avbildning bearbetning av tjänst som tillhandahåller enterprise nivån avbildningen bearbetning på en bråkdel av hello pris att det skulle kosta toobuild den själv.

hello faktum är att avbildningen bearbetningen har gjorts upprepade gånger, vanligtvis återskapas från grunden hello varje webbplats. Vi förstår detta eftersom vi har byggt dem miljoner gånger för. En dag som vi valt kanske är det dags göra vi bara det för alla. Vi vet hur toodo den toodo den snabb och effektiv och spara alla fungerar i hello tiden.

Mer information finns i [http://www.blitline.com](http://www.blitline.com).

## <a name="what-blitline-is-not"></a>Vad Blitline är inte...
tooclarify vad Blitline är användbart för, är det ofta enklare tooidentify vad Blitline inte utför innan du går vidare.

* Blitline har inte HTML widgetar tooupload bilder. Du måste ha tillgängliga avbildningarna offentligt eller med begränsade behörigheter som är tillgängliga för Blitline tooreach.
* Blitline utför inte live avbildningen bearbetning, till exempel Aviary.com
* Blitline accepteras inte av bilder, kan inte vara utgivarinitierad bilder-tooBlitline direkt. Du måste push-installera dem tooAzure lagring eller andra platser som har stöd för Blitline och berätta var toogo få dem Blitline.
* Blitline är massivt parallell och utför inte någon synkron bearbetning. Vilket innebär att du måste ge oss en postback_url och vi kan meddela dig när vi är klar bearbetning.

## <a name="create-a-blitline-account"></a>Skapa ett Blitline-konto
[!INCLUDE [blitline-signup](../includes/blitline-signup.md)]

## <a name="how-toocreate-a-blitline-job"></a>Hur toocreate ett Blitline-jobb
Blitline använder JSON toodefine hello åtgärder som du vill tootake på en bild. Den här JSON består av några enkla fält.

hello enklaste exemplet är följande:

        json : '{
       "application_id": "MY_APP_ID",
       "src" : "http://cdn.blitline.com/filters/boys.jpeg",
       "functions" : [ {
           "name": "resize_to_fit",
           "params" : { "width": 240, "height": 140 },
           "save" : { "image_identifier" : "external_sample_1" }
       } ]
    }'

Här är JSON som tar en avbildning ”src” ”... boys.jpeg” och sedan ändra storlek på det bild too240x140.

hello program-ID är något som du hittar i din **ANSLUTNINGSINFORMATION** eller **hantera** fliken på Azure. Det är din hemliga identifierare som gör toorun jobb på Blitline.

Hej ”spara”-parametern identifierar information om där du vill tooput hello avbildningen när vi har bearbetat den. Vi har inte definierats någon i det här fallet är trivial. Om ingen plats har definierats Blitline lagrar det lokalt (och tillfälligt) på en plats som unikt molnet. Du kommer att kunna tooget som platsen från hello JSON som returneras av Blitline när du gör hello Blitline. Hej ”bild” identifierare krävs och returneras tooyou när tooidentify sparade den här viss bild.

Du hittar mer information om hello *funktioner* stöder vi här: <http://www.blitline.com/docs/functions>

Du kan också hitta dokumentation om hello alternativ här: <http://www.blitline.com/docs/api>

När du har din JSON är allt du behöver toodo **POST** den för`http://api.blitline.com/job`

Får du JSON tillbaka som ser ut ungefär så här:

    {
     "results":
         {"images":
            [{
              "image_identifier":"external_sample_1",
              "s3_url":"https://s3.amazonaws.com/dev.blitline/2011110722/YOUR_APP_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg"
            }],
          "job_id":"4eb8c9f72a50ee2a9900002f"
         }
    }


Du ser att Blitline har tagit emot din begäran, den har placerats i en kö för bearbetning och när den har slutförts hello avbildningen ska vara tillgängligt vid: **https://s3.amazonaws.com/dev.blitline/2011110722/YOUR\_APP\_ID /CK3f0xBF_2bV6wf7gEZE8w.jpg**

## <a name="how-toosave-an-image-tooyour-azure-storage-account"></a>Hur toosave en bild tooyour Azure Storage-konto
Om du har ett Azure Storage-konto kan ha du enkelt Blitline push hello bearbetas bilder till din Azure-behållaren. Genom att lägga till en ”azure_destination” definiera hello plats och behörigheter för Blitline toopush till.

Här är ett exempel:

    job : '{
      "application_id": "YOUR_APP_ID",
      "src" : "http://www.google.com/logos/2011/houdini11-hp.jpg",
         "functions" : [{
         "name": "blur",
         "save" : {
             "image_identifier" : "YOUR_IMAGE_IDENTIFIER",
             "azure_destination" : {
                 "account_name" : "YOUR_AZURE_CONTAINER_NAME",
                 "shared_access_signature" : "SAS_THAT_GIVES_BLITLINE_PERMISSION_TO_WRITE_THIS_OBJECT_TO_CONTAINER",
               }
           }
         }]
       }'


Genom att fylla i hello CAPITALIZED värden med dina egna, kan du skicka den här JSON-toohttp://api.blitline.com/job och hello ”src” bild bearbetas med ett filter för oskärpa och pushas tooyou Azure mål.

### <a name="please-note"></a>Obs!
hello SAS måste innehålla hello hela SAS-url, inklusive hello filnamnet hello målfilen.

Exempel:

    http://blitline.blob.core.windows.net/sample/image.jpg?sr=b&sv=2012-02-12&st=2013-04-12T03%3A18%3A30Z&se=2013-04-12T04%3A18%3A30Z&sp=w&sig=Bte2hkkbwTT2sqlkkKLop2asByrE0sIfeesOwj7jNA5o%3D


Du kan också läsa hello senaste utgåvan av Blitline's Azure Storage docs [här](http://www.blitline.com/docs/azure_storage).

## <a name="next-steps"></a>Nästa steg
Besök blitline.com tooread om våra andra funktioner:

* Blitline API-slutpunkt Docs <http://www.blitline.com/docs/api>
* Blitline API-funktioner <http://www.blitline.com/docs/functions>
* Blitline API exempel <http://www.blitline.com/docs/examples>
* Tredje Part Nuget biblioteket <http://nuget.org/packages/Blitline.Net>


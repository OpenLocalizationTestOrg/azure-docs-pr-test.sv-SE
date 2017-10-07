---
title: 'Snabbstartsguide: Azure Machine Learning rekommendationer API (version 1) | Microsoft Docs'
description: Azure Machine Learning rekommendationer - Snabbstartsguide
services: machine-learning
documentationcenter: 
author: LuisCabrer
manager: jhubbard
editor: cgronlun
ms.assetid: 5bce1a4a-1ad6-473f-812b-84f800fdc09a
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: luisca
ROBOTS: NOINDEX
redirect_url: machine-learning-datamarket-deprecation
redirect_document_id: True
ms.openlocfilehash: d8f98e85f723a1104cb169b26d797934140979f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="quick-start-guide-for-hello-machine-learning-recommendations-api-version-1"></a>Snabbstartsguide för hello Machine Learning rekommendationer API (version 1)

> [!NOTE]
> Du bör börja använda hello [kognitiva rekommendationer API-tjänsten](https://www.microsoft.com/cognitive-services/recommendations-api) i stället för den här versionen. hello kognitiva Rekommendationstjänsten ersätter den här tjänsten och alla nya funktioner för hello det kommer fram. Den har nya funktioner som batchbearbetning support, bättre API Explorer, en tydligare API underlag, mer konsekvent signup/fakturerings experience och så vidare.
>
> Lär dig mer om [migrera toohello nya kognitiva tjänsten](http://aka.ms/recomigrate).
> 
> 

Det här dokumentet beskriver hur tooonboard tjänst- eller toouse Microsoft Azure Machine Learning rekommendationer. Du hittar mer information på hello rekommendationer API i hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/MachineLearningAPIs/Recommendations-2).

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="general-overview"></a>Allmän översikt
toouse Azure Machine Learning rekommendationer, behöver du tootake hello följande steg:

* Skapa en modell - en modell är en behållare för dina användningsdata och katalogdata hello rekommendation modellen.
* Importera data catalog - en katalog innehåller information om metadata på hello-objekt. 
* Importera data om programvaruanvändning - användningsdata kan överföras i ett av två sätt (eller båda):
  * Genom att överföra en fil som innehåller hello användningsdata.
  * Genom att skicka data förvärv händelser.
    Vanligtvis överför en fil för användning i ordning toobe kan toocreate en första rekommendationen model (bootstrap) och använda den förrän hello system samlar in tillräckligt med data med hjälp av hello förvärv dataformat.
* Skapa en rekommendation modell – det här är en asynkron åtgärd i vilka hello rekommendation system tar alla hello användningsdata och skapar en rekommendation modell. Den här åtgärden kan ta flera minuter eller flera timmar beroende på hello storleken på hello data och hello bygger konfigurationsparametrar. När utlöser hello build kan får du ett build-ID. Använd detta toocheck när hello build-processen har avslutats innan du startar tooconsume rekommendationer.
* Använda rekommendationer - Get rekommendationer för ett specifikt objekt eller en lista med objekt.

Alla hello stegen ovan görs via hello Azure Machine Learning rekommendationer API.  Du kan hämta ett exempelprogram som implementerar var och en av de här stegen från hello [samt gallery.](http://1drv.ms/1xeO2F3)

## <a name="limitations"></a>Begränsningar
* hello maximala antalet modeller per prenumeration är 10.
* hello maximalt antal objekt som kan innehålla en katalog är 100 000.
* hello maxantalet återställningspunkter för användning som hålls är ~ 5,000,000. hello äldsta tas bort om nya överföras eller rapporteras.
* hello maximala storleken på data som kan skickas i POST (till exempel importera katalogdata import användningsdata) är 200MB.
* hello antalet transaktioner per sekund för en rekommendation modell-version som inte är aktiv är ~ 2TPS. En rekommendation modell-version som är aktiv kan innehålla upp too20TPS.

## <a name="integration"></a>Integrering
### <a name="authentication"></a>Autentisering
Microsoft Azure Marketplace stöder antingen hello grundläggande eller OAuth-autentiseringsmetoden. Du lätt kan hitta hello nycklar genom att gå toohello nycklar i hello marketplace under [dina kontoinställningar](https://datamarket.azure.com/account/keys). 

#### <a name="basic-authentication"></a>Grundläggande autentisering
Lägg till hello Authorization-huvud:

    Authorization: Basic <creds>

    Where <creds> = ConvertToBase64("AccountKey:" + yourAccountKey);  

Konvertera tooBase64 (C#)

    var bytes = Encoding.UTF8.GetBytes("AccountKey:" + yourAccountKey);
    var creds = Convert.ToBase64String(bytes);

Konvertera tooBase64 (JavaScript)

    var creds = window.btoa("AccountKey" + ":" + yourAccountKey);




### <a name="service-uri"></a>URI för tjänsten
hello service rot URI för hello Azure Machine Learning rekommendationer API: er är [här.](https://api.datamarket.azure.com/amla/recommendations/v2/)

fullständig hello-tjänstens URI uttrycks med hjälp av element i hello OData-specifikationen.

### <a name="api-version"></a>API-version
Varje API-anrop har, i slutet av hello frågeparameter kallas apiVersion som ska anges för ”1.0”.

### <a name="ids-are-case-sensitive"></a>ID: N är skiftlägeskänsliga
ID: N som returneras av någon av hello API: er, är skiftlägeskänsliga och kan användas som sådana när de skickas som parametrar i efterföljande API-anrop. Till exempel är modell-ID: N och katalog-ID: N skiftlägeskänsliga.

### <a name="create-a-model"></a>Skapa en modell
Skapa en begäran om ”skapa modellen”:

| HTTP-metoden | URI: N |
|:--- |:--- |
| POST |`<rootURI>/CreateModel?modelName=%27<model_name>%27&apiVersion=%271.0%27`<br><br>Exempel:<br>`<rootURI>/CreateModel?modelName=%27MyFirstModel%27&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelName/ |Endast bokstäver (A-Z, a-z), siffror (0-9), bindestreck (-) och understreck (_) är tillåtna.<br>Maxlängd: 20 |
| apiVersion |1.0 |
|  | |
| Begärandetext |INGEN |

**Svaret**:

HTTP-statuskod: 200

* `feed/entry/content/properties/id`-Innehåller hello modell-ID.
  Observera att hello modell-ID är skiftlägeskänsliga.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Create A New Model</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T06:35:21Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">CreateANewModelEntity2</title>
    <updated>2014-10-05T06:35:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">a658c626-2baa-43a7-ac98-f6ee26120a12</d:Id>
        <d:Name m:type="Edm.String">MyFirstModel</d:Name>
        <d:Date m:type="Edm.String">10/5/2014 6:35:19 AM</d:Date>
        <d:Status m:type="Edm.String">Created</d:Status>
        <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
        <d:BuildId m:type="Edm.String">-1</d:BuildId>
        <d:Mpr m:type="Edm.String">0</d:Mpr>
        <d:UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:UserName>
        <d:Description m:type="Edm.String"></d:Description>
      </m:properties>
    </content>
      </entry>
    </feed>


### <a name="import-catalog-data"></a>Importera data catalog
Om du överför flera katalog filer toohello samma modell med flera anrop infogas vi bara hello nya katalogobjekt. Befintliga objekt förblir med hello ursprungliga värden.

| HTTP-metoden | URI: N |
|:--- |:--- |
| POST |`<rootURI>/ImportCatalogFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Exempel:<br>`<rootURI>/ImportCatalogFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |Unik identifierare för hello modellen (skiftlägeskänsligt) |
| filnamn |Textrepresentation identifierare för hello katalog.<br>Endast bokstäver (A-Z, a-z), siffror (0-9), bindestreck (-) och understreck (_) är tillåtna.<br>Maxlängd: 50 |
| apiVersion |1.0 |
|  | |
| Begärandetext |Katalogen data. Format:<br>`<Item Id>,<Item Name>,<Item Category>[,<description>]`<br><br><table><tr><th>Namn</th><th>Obligatorisk</th><th>Typ</th><th>Beskrivning</th></tr><tr><td>Objekt-Id</td><td>Ja</td><td>Längden på alfanumeriska, max 50</td><td>Unik identifierare för ett objekt</td></tr><tr><td>Objektnamnet</td><td>Ja</td><td>Längden på alfanumeriska, max 255</td><td>Objektnamnet</td></tr><tr><td>Objektet kategori</td><td>Ja</td><td>Längden på alfanumeriska, max 255</td><td>Kategori toowhich det här objektet tillhör (till exempel matlagning Books Drama...)</td></tr><tr><td>Beskrivning</td><td>Nej</td><td>Alfanumeriska, max längd 4000</td><td>Beskrivning av det här objektet</td></tr></table><br>Maximal filstorlek är 200MB.<br><br>Exempel:<br><code>2406e770-769c-4189-89de-1c9283f93a96,Clara Callan,Book<br>21bf8088-b6c0-4509-870c-e1c7ac78304a,hello Forgetting Room: A Fiction (Byzantium Book),Book<br>3bb5cb44-d143-4bdd-a55c-443964bf4b23,Spadework,Book<br>552a1940-21e4-4399-82bb-594b46d7ed54,Restraint of Beasts,Book</code> |

**Svaret**:

HTTP-statuskod: 200

* `Feed\entry\content\properties\LineCount`-Antal rader som accepteras.
* `Feed\entry\content\properties\ErrorCount`-Antal rader som inte infogades på grund av tooan fel.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
          <subtitle type="text">Import catalog file</subtitle>
          <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T06:58:04Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">ImportCatalogFileEntity2</title>
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">4</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
      </m:properties>
    </content>
      </entry>
    </feed>


### <a name="import-usage-data"></a>Importera data om programvaruanvändning
#### <a name="uploading-a-file"></a>Överför en fil
Det här avsnittet visas hur tooupload användningsdata med hjälp av en fil. Du kan anropa denna API flera gånger med användningsdata. Alla användningsdata sparas för alla samtal.

| HTTP-metoden | URI: N |
|:--- |:--- |
| POST |`<rootURI>/ImportUsageFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Exempel:<br>`<rootURI>/ImportUsageFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27ImplicitMatrix10_Guid_small.txt%27&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |Unik identifierare för hello modellen (skiftlägeskänsligt) |
| filnamn |Textrepresentation identifierare för hello katalog.<br>Endast bokstäver (A-Z, a-z), siffror (0-9), bindestreck (-) och understreck (_) är tillåtna.<br>Maxlängd: 50 |
| apiVersion |1.0 |
|  | |
| Begärandetext |Användningsdata. Format:<br>`<User Id>,<Item Id>[,<Time>,<Event>]`<br><br><table><tr><th>Namn</th><th>Obligatorisk</th><th>Typ</th><th>Beskrivning</th></tr><tr><td>Användar-Id</td><td>Ja</td><td>Alfanumeriskt</td><td>Unik identifierare för en användare</td></tr><tr><td>Objekt-Id</td><td>Ja</td><td>Längden på alfanumeriska, max 50</td><td>Unik identifierare för ett objekt</td></tr><tr><td>Tid</td><td>Nej</td><td>Datum i format: ÅÅÅÅ/MM/ddTHH (till exempel 2013/06/20T10:00:00)</td><td>Tid för data</td></tr><tr><td>Händelse</td><td>Nej, om tillhandahålls måste också sätta datum</td><td>En av hello följande:<br>• Klicka på<br>• RecommendationClick<br>• AddShopCart<br>• RemoveShopCart<br>• Köp</td><td></td></tr></table><br>Maximal filstorlek är 200MB.<br><br>Exempel:<br><pre>149452,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>6360,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>50321,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>71285,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>224450,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>236645,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>107951,1b3d95e2-84e4-414c-bb38-be9cf461c347</pre> |

**Svaret**:

HTTP-statuskod: 200

* `Feed\entry\content\properties\LineCount`-Antal rader som accepteras.
* `Feed\entry\content\properties\ErrorCount`-Antal rader som inte infogades på grund av tooan fel.
* `Feed\entry\content\properties\FileId`-Filen identifierare.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Add bulk usage data (usage file)</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T07:21:44Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">AddBulkUsageDataUsageFileEntity2</title>
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">33</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
        <d:FileId m:type="Edm.String">fead7c1c-db01-46c0-872f-65bcda36025d</d:FileId>
      </m:properties>
    </content>
      </entry>
    </feed>


#### <a name="using-data-acquisition"></a>Med hjälp av datainsamling
Det här avsnittet visar hur toosend händelser i real time tooAzure Machine Learning rekommendationer, vanligen från din webbplats.

| HTTP-metoden | URI: N |
|:--- |:--- |
| POST |`<rootURI>/AddUsageEvent?apiVersion=%271.0%27-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| apiVersion |1.0 |
|  | |
| Begärandetexten |Händelsen inmatning för varje händelse som du vill toosend. Du bör skicka för hello samma användare eller webbläsare session hello samma ID i hello sessions-ID-fältet. (Se exemplet för händelsen nedan.) |

* Händelse 'Klickar du på ”exempel:
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>Click</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>
* Händelse 'RecommendationClick' exempel:
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
          <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
          <SessionId>11112222</SessionId>
          <EventData>
        <EventData>
          <Name>RecommendationClick</Name>
          <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
          </EventData>
        </Event>
* Händelse 'AddShopCart' exempel:
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
          <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
          <SessionId>11112222</SessionId>
          <EventData>
        <EventData>
          <Name>AddShopCart</Name>
          <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
          </EventData>
        </Event>
* Händelse 'RemoveShopCart' exempel:
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
          <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
          <SessionId>11112222</SessionId>
          <EventData>
        <EventData>
          <Name>RemoveShopCart</Name>
          <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
          </EventData>
        </Event>
* Händelse 'Inköp' exempel:
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
            <Name>Purchase</Name> 
            <PurchaseItems>
            <PurchaseItems>
                <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
                <Count>3</Count>
            </PurchaseItems>
        </PurchaseItems>
        </EventData>
        </EventData>
        </Event>
* Exempel skickar 2 händelser, klickar du på' och 'AddShopCart':
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
          <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
          <SessionId>11112222</SessionId>
          <EventData>
        <EventData>
          <Name>Click</Name>
          <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
          <ItemName>itemName</ItemName>
          <ItemDescription>item description</ItemDescription>
          <ItemCategory>category</ItemCategory>
        </EventData>
        <EventData>
          <Name>AddShopCart</Name>
          <ItemId>552A1940-21E4-4399-82BB-594B46D7ED54</ItemId>
        </EventData>
          </EventData>
        </Event>

**Svaret**: HTTP-statuskod: 200

### <a name="build-a-recommendation-model"></a>Skapa en rekommendation modell
| HTTP-metoden | URI: N |
|:--- |:--- |
| POST |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&apiVersion=%271.0%27`<br><br>Exempel:<br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |Unik identifierare för hello modellen (skiftlägeskänsligt) |
| userDescription |Textrepresentation identifierare för hello katalog. Observera att om du använder lagringsutrymmen du koda den med % 20 i stället. Finns i exemplet ovan.<br>Maxlängd: 50 |
| apiVersion |1.0 |
|  | |
| Begärandetext |INGEN |

**Svaret**:

HTTP-statuskod: 200

Detta är en asynkron API. Du får ett build-ID som en respons. tooknow när hello build har slutförts ska du anropa hello ”hämta bygger Status för en modell” API och leta upp det skapa ID hello svar. Observera att en version kan ta från toohours minuter beroende på hello storleken på hello data.

Du kan inte använda rekommendationer till hello skapa avslutas.

Giltig build-status:

* Skapa – modellen skapades.
* I kö – modellen build utlöstes och den är i kö.
* Skapa – modellen skapas.
* Lyckade – Build slutförts utan fel.
* Fel – Build avslutades med ett fel.
* Annullerat – avbröts skapa.
* Om du avbryter – Build avbryts.

Observera att hello build ID finns under hello följande sökväg:`Feed\entry\content\properties\Id`

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Build a Model with RequestBody</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T08:56:34Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">1000272</d:Id>
        <d:UserName m:type="Edm.String"></d:UserName>
        <d:ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:ModelId>
        <d:ModelName m:type="Edm.String">docTest</d:ModelName>
        <d:Type m:type="Edm.String">Recommendation</d:Type>
        <d:CreationTime m:type="Edm.String">2014-10-05T08:56:31.893</d:CreationTime>
        <d:Progress_BuildId m:type="Edm.String">1000272</d:Progress_BuildId>
        <d:Progress_ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:Progress_ModelId>
        <d:Progress_UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:Progress_UserName>
        <d:Progress_IsExecutionStarted m:type="Edm.String">false</d:Progress_IsExecutionStarted>
        <d:Progress_IsExecutionEnded m:type="Edm.String">false</d:Progress_IsExecutionEnded>
        <d:Progress_Percent m:type="Edm.String">0</d:Progress_Percent>
        <d:Progress_StartTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_StartTime>
        <d:Progress_EndTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_EndTime>
        <d:Progress_UpdateDateUTC m:type="Edm.String"></d:Progress_UpdateDateUTC>
        <d:Status m:type="Edm.String">Queued</d:Status>
        <d:Key1 m:type="Edm.String">UseFeaturesInModel</d:Key1>
        <d:Value1 m:type="Edm.String">False</d:Value1>
      </m:properties>
    </content>
      </entry>
    </feed>

### <a name="get-build-status-of-a-model"></a>Hämta build-status för en modell
| HTTP-metoden | URI: N |
|:--- |:--- |
| HÄMTA |`<rootURI>/GetModelBuildsStatus?modelId=%27<modelId>%27&onlyLastBuild=<bool>&apiVersion=%271.0%27`<br><br>Exempel:<br>`<rootURI>/GetModelBuildsStatus?modelId=%279559872f-7a53-4076-a3c7-19d9385c1265%27&onlyLastBuild=true&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |Unik identifierare för hello modellen (skiftlägeskänsligt) |
| onlyLastBuild |Anger om alla hello tooreturn skapa historik över hello modell eller endast hello status hello den senaste versionen. |
| apiVersion |1.0 |

**Svaret**:

HTTP-statuskod: 200

hello svaret innehåller en post per version. Varje post innehåller hello följande data:

* `feed/entry/content/properties/UserName`– Namnet på hello användare.
* `feed/entry/content/properties/ModelName`– Namnet på hello modell.
* `feed/entry/content/properties/ModelId`– Modell Unik identifierare.
* `feed/entry/content/properties/IsDeployed`– Om hello build distribueras (kallas även aktiva build).
* `feed/entry/content/properties/BuildId`– Skapa Unik identifierare.
* `feed/entry/content/properties/BuildType`-Typen av hello build.
* `feed/entry/content/properties/Status`– Skapa status. Kan vara något av följande hello: fel, skapa, i kö, Canceling, Avbruten, lyckades
* `feed/entry/content/properties/StatusMessage`– Detaljerad statusmeddelande (gäller endast toospecific tillstånd).
* `feed/entry/content/properties/Progress`– Skapa förlopp (%).
* `feed/entry/content/properties/StartTime`– Skapa starttid.
* `feed/entry/content/properties/EndTime`– Skapa sluttid.
* `feed/entry/content/properties/ExecutionTime`– Skapa varaktighet.
* `feed/entry/content/properties/ProgressStep`– Information om hello aktuell etapp som en build pågår.

Giltig build-status:

* Skapad – skapades skapa posten.
* I kö – skapa begäran utlöstes och den är i kö.
* Skapa – Build pågår.
* Lyckade – Build slutförts utan fel.
* Fel – Build avslutades med ett fel.
* Annullerat – avbröts skapa.
* Om du avbryter – Build avbryts.

Giltiga värden för build-typ:

* Rangordning - skapa rang. (För rang skapa information, se toohello ”Machine Learning rekommendation API-dokumentation” dokumentet.)
* Rekommendation - rekommendation build.
* Fbt - köpt ofta tillsammans build.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get builds status of a model</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T17:51:10Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetBuildsStatusEntity</title>
    <updated>2014-11-05T17:51:10Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:UserName m:type="Edm.String">b-434e-b2c9-84935664ff20@dm.com</d:UserName>
        <d:ModelName m:type="Edm.String">ModelName</d:ModelName>
        <d:ModelId m:type="Edm.String">1d20c34f-dca1-4eac-8e5d-f299e4e4ad66</d:ModelId>
        <d:IsDeployed m:type="Edm.String">true</d:IsDeployed>
        <d:BuildId m:type="Edm.String">1000272</d:BuildId>
        <d:BuildType m:type="Edm.String">Recommendation</d:BuildType>
        <d:Status m:type="Edm.String">Success</d:Status>
        <d:StatusMessage m:type="Edm.String"></d:StatusMessage>
        <d:Progress m:type="Edm.String">0</d:Progress>
        <d:StartTime m:type="Edm.String">2014-11-02T13:43:51</d:StartTime>
        <d:EndTime m:type="Edm.String">2014-11-02T13:45:10</d:EndTime>
        <d:ExecutionTime m:type="Edm.String">00:01:19</d:ExecutionTime>
        <d:IsExecutionStarted m:type="Edm.String">false</d:IsExecutionStarted>
        <d:ProgressStep m:type="Edm.String"></d:ProgressStep>
      </m:properties>
     </content>
    </entry>
    </feed>


### <a name="get-recommendations"></a>Få rekommendationer
| HTTP-metoden | URI: N |
|:--- |:--- |
| HÄMTA |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Exempel:<br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |Unik identifierare för hello modellen (skiftlägeskänsligt) |
| ItemId |Kommaavgränsad lista över hello objekt toorecommend för.<br>Maxlängd: 1024 |
| numberOfResults |Antalet nödvändiga resultat |
| includeMetatadata |Framtida användning alltid false |
| apiVersion |1.0 |

**Svar:**

HTTP-statuskod: 200

hello svaret innehåller en post per rekommenderade objekt. Varje post innehåller hello följande data:

* `Feed\entry\content\properties\Id`-Rekommenderade objekt-ID.
* `Feed\entry\content\properties\Name`-Namnet på hello-objektet.
* `Feed\entry\content\properties\Rating`-Klassificering av hello rekommendation. högre nummer innebär högre tillförlitlighet.
* `Feed\entry\content\properties\Reasoning`-Rekommendation motivationen (till exempel rekommendation förklaringar).

OData-XML

Hej exempelsvar nedan innehåller 10 rekommenderade objekt:

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Get Recommendation</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T12:28:48Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">159</d:Id>
        <d:Name m:type="Edm.String">159</d:Name>
        <d:Rating m:type="Edm.Double">0.543343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '159'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">52</d:Id>
        <d:Name m:type="Edm.String">52</d:Name>
        <d:Rating m:type="Edm.Double">0.539588900748721</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '52'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">35</d:Id>
        <d:Name m:type="Edm.String">35</d:Name>
        <d:Rating m:type="Edm.Double">0.53842946443853</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '35'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">124</d:Id>
        <d:Name m:type="Edm.String">124</d:Name>
        <d:Rating m:type="Edm.Double">0.536712832792886</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '124'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">120</d:Id>
        <d:Name m:type="Edm.String">120</d:Name>
        <d:Rating m:type="Edm.Double">0.533673023762878</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '120'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">96</d:Id>
        <d:Name m:type="Edm.String">96</d:Name>
        <d:Rating m:type="Edm.Double">0.532544826370521</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '96'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">69</d:Id>
        <d:Name m:type="Edm.String">69</d:Name>
        <d:Rating m:type="Edm.Double">0.531678607847759</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '69'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">172</d:Id>
        <d:Name m:type="Edm.String">172</d:Name>
        <d:Rating m:type="Edm.Double">0.530957821375951</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '172'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">155</d:Id>
        <d:Name m:type="Edm.String">155</d:Name>
        <d:Rating m:type="Edm.Double">0.529093541481333</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '155'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">32</d:Id>
        <d:Name m:type="Edm.String">32</d:Name>
        <d:Rating m:type="Edm.Double">0.528917978168322</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '32'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
    </feed>

### <a name="update-model"></a>Uppdatera modellen
Du kan uppdatera hello beskrivning eller hello active build-ID.
*Aktiva build-ID* -varje build för varje modell har ett build-ID. hello active build-ID är hello första lyckade version av varje ny modell. När du har en aktiv build-ID och du göra ytterligare versioner för hello samma modell som du behöver tooexplicitly ange den som hello standard skapa ID om du vill. När du använder rekommendationer om du inte anger hello build-ID som du vill toouse hello-standard som ska användas automatiskt.

Den här mekanismen gör att du - när du har en rekommendation modell i produktionen, nya modeller för toobuild och testa dem innan du höjer upp dem tooproduction.

| HTTP-metoden | URI: N |
|:--- |:--- |
| PLACERA |`<rootURI>/UpdateModel?id=%27<modelId>%27&apiVersion=%271.0%27`<br><br>Exempel:<br>`<rootURI>/UpdateModel?id=%279559872f-7a53-4076-a3c7-19d9385c1265%27&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| id |Unik identifierare för hello modellen (skiftlägeskänsligt) |
| apiVersion |1.0 |
|  | |
| Begärandetext |`<ModelUpdateParams xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`<br>`   <Description>New Description</Description>`<br>`          <ActiveBuildId>-1</ActiveBuildId>`<br>`</ModelUpdateParams>`<br><br>Observera att hello XML taggar beskrivning och ActiveBuildId är valfria. Om du inte vill att tooset beskrivning eller ActiveBuildId bort hela hello-taggen. |

**Svaret**:

HTTP-statuskod: 200

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Update an Existing Model</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T10:27:17Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'" />
    </feed>

## <a name="legal"></a>Juridisk information
Detta dokument tillhandahålls ”som-är”. Information och åsikter som uttrycks i detta dokument, inklusive Webbadresser och andra webbplatsreferenser, kan ändras utan föregående meddelande. Vissa exempel som beskrivs häri tillhandahålls enbart som illustration och är fiktiva. Ingen verklig associering eller koppling är avsedd eller underförstådd. Det här dokumentet ger dig inga juridiska rättigheter tooany immateriell egendom i någon Microsoft-produkt. Du får kopiera och använda det här dokumentet som intern referens. © 2014 Microsoft. Med ensamrätt. 


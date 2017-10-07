---
title: aaaMachine Learning rekommendationer API-dokumentationen | Microsoft Docs
description: "Azure Machine Learning rekommendationer API-dokumentationen för en rekommendationer motor som är tillgängliga i hello Microsoft Azure Marketplace."
services: machine-learning
documentationcenter: 
author: LuisCabrer
manager: jhubbard
editor: cgronlun
ms.assetid: 32c3ab2f-fdd7-48cc-b501-ad55c79b87dc
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: LuisCa
ROBOTS: NOINDEX
redirect_url: machine-learning-datamarket-deprecation
redirect_document_id: True
ms.openlocfilehash: d1cec228bf23870c05c8ab8df2779b0c3c65b06d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-machine-learning-recommendations-api-documentation"></a>Azure Machine Learning-rekommendationer – API-dokumentation
> [!NOTE]
> Du bör börja använda hello kognitiva rekommendationer API-tjänsten i stället för den här versionen. hello kognitiva Rekommendationstjänsten ersätter den här tjänsten och alla nya funktioner för hello det kommer fram. Den har nya funktioner som batchbearbetning support, bättre API Explorer, en tydligare API underlag, mer konsekvent signup/fakturerings experience och så vidare.
> Lär dig mer om [migrera toohello ny kognitiva tjänst](http://aka.ms/recomigrate)
> 
> 

Det här dokumentet visar Microsoft Azure Machine Learning rekommendationer API.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="1-general-overview"></a>1. Allmän översikt
Det här dokumentet är en API-referens. Du bör börja med ”Azure Machine Learning rekommendation--Snabbstart” hello-dokument.

hello Azure Machine Learning rekommendationer API kan delas in i hello följande logiska grupper:

* <ins>Begränsningar</ins> -rekommendationer API-begränsningar.
* <ins>Allmän Information</ins> -Information om autentisering, service URI och versionshantering.
* <ins>Modellen Basic</ins> -API: er som gör att du toodo hello grundläggande åtgärder på modellen (t.ex. Skapa, uppdatera och ta bort en modell).
* <ins>Modellen Avancerat</ins> -API: er som gör att du tooget avancerade datainsikter om hello modellen.
* <ins>Modellen affärsregler</ins> -API: er som gör att du toomanage affärsregler på hello modellen rekommendation resultaten.
* <ins>Katalogen</ins> -API: er som gör att du toodo grundläggande åtgärder på en modell-katalog. En katalog innehåller information om metadata på hello objekt hello användningsdata.
* <ins>Funktionen</ins> -API: er som aktiverar tooget insikter om objektet till hello katalog och hur toouse denna information toobuild bättre rekommendationer.
* <ins>Användningsdata</ins> -API: er som gör att du toodo grundläggande åtgärder på hello modellen användningsdata. Användningsdata i hello grundformat består av rader som innehåller par &#60; userId &#62; &#60; itemId &#62;.
* <ins>Skapa</ins> - API: er som aktiverar tootrigger en modell bygga och skapa grundläggande åtgärder som är relaterade toothis. Du kan utlösa en version av modellen när du har värdefulla användningsdata.
* <ins>Rekommendation</ins> -API: er som gör att du tooconsume rekommendationer när du slutar hello version av en modell.
* <ins>Användardata</ins> -API: er som gör att du toofetch information om användning av hello användardata.
* <ins>Meddelanden</ins> -API: er som gör att du tooreceive meddelanden om problem relaterade tooyour API-åtgärder. (Till exempel du rapporterar användningsdata via för datainsamling och de flesta av hello händelser bearbetning av inte importeras. Ett felmeddelande aktiveras.)

## <a name="2-limitations"></a>2. Begränsningar
* hello maximala antalet modeller per prenumeration är 10.
* hello maximalt antal versioner per modell är 20.
* hello maximalt antal objekt som kan innehålla en katalog är 100 000.
* hello maxantalet återställningspunkter för användning som hålls är ~ 5,000,000. hello äldsta tas bort om nya överföras eller rapporteras.
* hello maximala storleken på data som kan skickas i POST (t.ex. Importera katalogdata import användningsdata) är 200MB.
* hello maximalt antal objekt som kan bli tillfrågad om när du hämtar rekommendationer är 150.

## <a name="3-apis---general-information"></a>3. API - allmän information
### <a name="31-authentication"></a>3.1. Autentisering
Följ hello Microsoft Azure Marketplace riktlinjer om autentisering. hello marketplace stöder antingen hello grundläggande eller OAuth autentiseringsmetoden.

### <a name="32-service-uri"></a>3.2. URI för tjänsten
hello service rot URI för hello Azure Machine Learning rekommendationer API: er är [här.](https://api.datamarket.azure.com/amla/recommendations/v3/)

fullständig hello-tjänstens URI uttrycks med hjälp av element i hello OData-specifikationen.  

### <a name="33-api-version"></a>3.3. API-version
Varje API-anrop har ska i slutet av hello frågeparameter kallas apiVersion som ska anges too1.0.

### <a name="34-ids-are-case-sensitive"></a>3.4. ID: N är skiftlägeskänsliga
ID: N som returneras av någon av hello API: er, är skiftlägeskänsliga och ska användas som sådana när de skickas som parametrar i efterföljande API-anrop. Till exempel är modell-ID: N och katalog-ID: N skiftlägeskänsliga.

## <a name="4-recommendations-quality-and-cold-items"></a>4. Rekommendationer kvalitet och kalla objekt
### <a name="41-recommendation-quality"></a>4.1. Rekommendation kvalitet
Skapa en rekommendation modell är vanligtvis tillräckligt med tooallow hello system tooprovide rekommendationer. Dock rekommendation kvalitet varierar beroende på hello användning bearbetas och hello täckning av hello-katalogen. Till exempel om du har många cold artiklar (artiklar utan betydande syntax) hello systemet har problem med att tillhandahålla en rekommendation för ett sådant objekt eller med ett sådant objekt som en rekommenderad. I ordning tooovercome hello cold objektet problem kan hello system hello användning av metadata för hello objekt tooenhance hello rekommendationer. Dessa metadata är refererad tooas funktioner. Vanliga funktioner är en bok författaren eller en film aktören. Funktioner som tillhandahålls via hello katalog i hello form av nyckel/värde-strängar. Hello fullständig formatering av hello katalogfil finns toohello [importera katalogen avsnittet](#81-import-catalog-data). 

### <a name="42-rank-build"></a>4.2. Rank build
Funktioner kan förbättra hello rekommendation modell, men toodo kräver hello med meningsfull funktioner. För detta ändamål som en ny version introducerades - skapa en rang. Den här versionen ska rangordnas hello utnyttjar funktioner. En användbar funktion är en funktion med rangordningen 2 och uppåt.
Efter att förstå vilka hello funktioner som är meningsfullt, Utlös en rekommendation version med hello lista (eller underordnad lista) med meningsfull funktioner. Det är möjligt toouse dessa funktioner för hello förbättring av både varm objekt och kalla. I ordning toouse dem för varmt objekt hello `UseFeatureInModel` build-parametern ska ställas in. I ordning toouse funktioner för cold artiklar hello `AllowColdItemPlacement` build-parametern måste vara aktiverad.
Obs: Det är inte möjligt tooenable `AllowColdItemPlacement` utan att aktivera `UseFeatureInModel`.

### <a name="43-recommendation-reasoning"></a>4.3. Rekommendation motivationen
Rekommendation motivationen är en annan del av användning av funktioner. Faktiskt kan hello Azure Machine Learning rekommendationer motorn använda funktioner tooprovide rekommendation förklaringar (kallas även skäl till), rekommenderas inledande toomore förtroende i hello objekt från hello rekommendation konsumenten.
tooenable skäl, hello `AllowFeatureCorrelation` och `ReasoningFeatureList` parametrarna vara installationsprogrammet tidigare toorequesting en rekommendation version.

## <a name="5-model-basic"></a>5. Modellen Basic
### <a name="51-create-model"></a>5.1. Skapa modell
Skapar en ”skapa modellen” begäran.

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
  **Obs**: modell-ID är skiftlägeskänsliga.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Create A New Model</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T06:35:21Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">CreateANewModelEntity2</title>
    <updated>2014-10-05T06:35:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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

### <a name="52-get-model"></a>5.2. Hämta modellen
Skapar en ”get-”-modellbegäran.

| HTTP-metoden | URI: N |
|:--- |:--- |
| HÄMTA |`<rootURI>/GetModel?id=%27<model_id>%27&apiVersion=%271.0%27`<br>Exempel:<br>`<rootURI>/GetModel?id=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| id |Unik identifierare för hello modellen (skiftlägeskänslig) |
| apiVersion |1.0 |
|  | |
| Begärandetext |INGEN |

**Svaret**:

HTTP-statuskod: 200

hello modelldata kan hittas under hello följande element:

* `feed/entry/content/properties/Id`-Modell unikt ID.
* `feed/entry/content/properties/Name`-Namn.
* `feed/entry/content/properties/Date`-Skapandedatum för modell.
* `feed/entry/content/properties/Status`-Status för modellen. En av hello följande:
  * -Modellen har skapats och innehåller inte katalog- och användningsdata.
  * ReadyForBuild - modellen skapas och innehåller katalogen och användning.
* `feed/entry/content/properties/HasActiveBuild`-Anger om hello modellen har har skapats.
* `feed/entry/content/properties/BuildId`-Modell active build-ID.
* `feed/entry/content/properties/Mpr`-Modellen medelvärde percentil rangordning (MPR - mer information finns i ModelInsight).
* `feed/entry/content/properties/UserName`-Modell interna användarnamn.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Get A List Of All Models</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-28T14:35:51Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetAListOfAllModelsEntity</title>
    <updated>2014-10-28T14:35:51Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">68b232f2-1037-45f7-8f49-af822695ee63</d:Id>
        <d:Name m:type="Edm.String">vah-11111</d:Name>
        <d:Date m:type="Edm.String">10/28/2014 2:16:07 PM</d:Date>
        <d:Status m:type="Edm.String">ReadyForBuild</d:Status>
        <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
        <d:BuildId m:type="Edm.String">-1</d:BuildId>
        <d:Mpr m:type="Edm.String">0</d:Mpr>
        <d:UsageFileNames m:type="Edm.String">ImplicitMatrix10_Guid_small.txt, ImplicitMatrix11_Guid_small.txt</d:UsageFileNames>
        <d:CatalogId m:type="Edm.String">626babdb-cab6-43a6-82ef-4fb86345700c</d:CatalogId>
        <d:UserName m:type="Edm.String">89dc4722-03ba-4f90-8821-b1db388408b5@dm.com</d:UserName>
        <d:Description m:type="Edm.String">short description</d:Description>
        <d:CatalogFileName m:type="Edm.String">catalog10_small.txt</d:CatalogFileName>
      </m:properties>
    </content>
      </entry>
    </feed>

### <a name="53----get-all-models"></a>5.3.    Hämta alla modeller
Hämtar alla modeller för hello aktuella användaren.

| HTTP-metoden | URI: N |
|:--- |:--- |
| HÄMTA |`<rootURI>/GetAllModels?apiVersion=%271.0%27`<br>Exempel:<br>`<rootURI>/GetAllModels?apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| apiVersion |1.0 |
|  | |
| Begärandetext |INGEN |

**Svaret**:

HTTP-statuskod: 200

* `feed/entry/content/properties/Id`-Modell unikt ID.
* `feed/entry/content/properties/Name`-Namn.
* `feed/entry/content/properties/Date`-Skapandedatum för modell.
* `feed/entry/content/properties/Status`-Status för modellen. En av hello följande:
  * -Modellen har skapats och innehåller inte katalog- och användningsdata.
  * ReadyForBuild - modellen skapas och innehåller katalogen och användning.
* `feed/entry/content/properties/HasActiveBuild`-Anger om hello modellen har har skapats.
* `feed/entry/content/properties/BuildId`-Modell active build-ID.
* `feed/entry/content/properties/Mpr`-Modellen MPR (se ModelInsight för mer information).
* `feed/entry/content/properties/UserName`-Modell interna användarnamn.
* `feed/entry/content/properties/UsageFileNames`-Lista över modellen användningsfiler avgränsade med kommatecken.
* `feed/entry/content/properties/CatalogId`-Modell katalog-ID.
* `feed/entry/content/properties/Description`-Beskrivning av modellen.
* `feed/entry/content/properties/CatalogFileName`-Namnet på modellfilen katalogen.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get A List Of All Models</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-28T14:35:51Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetAListOfAllModelsEntity</title>
    <updated>2014-10-28T14:35:51Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Id m:type="Edm.String">68b232f2-1037-45f7-8f49-af822695ee63</d:Id>
                    <d:Name m:type="Edm.String">vah-11111</d:Name>
                    <d:Date m:type="Edm.String">10/28/2014 2:16:07 PM</d:Date>
                    <d:Status m:type="Edm.String">ReadyForBuild</d:Status>
                    <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
                    <d:BuildId m:type="Edm.String">-1</d:BuildId>
                    <d:Mpr m:type="Edm.String">0</d:Mpr>
                    <d:UsageFileNames m:type="Edm.String">ImplicitMatrix10_Guid_small.txt, ImplicitMatrix11_Guid_small.txt</d:UsageFileNames>
                    <d:CatalogId m:type="Edm.String">626babdb-cab6-43a6-82ef-4fb86345700c</d:CatalogId>
                    <d:UserName m:type="Edm.String">89dc4722-03ba-4f90-8821-b1db388408b5@dm.com</d:UserName>
                    <d:Description m:type="Edm.String">short description</d:Description>
                    <d:CatalogFileName m:type="Edm.String">catalog10_small.txt</d:CatalogFileName>
                </m:properties>
            </content>
        </entry>
    </feed>

### <a name="54----update-model"></a>5.4.    Uppdatera modellen
Du kan uppdatera hello beskrivning eller hello active build-ID.<br>
<ins>Aktiva build-ID</ins> -varje build för varje modell har ett build-ID. hello active build-ID är hello första lyckade version av varje ny modell. När du har en aktiv build-ID och du göra ytterligare versioner för hello samma modell som du behöver tooexplicitly ange den som hello standard skapa ID om du vill. När du använder rekommendationer om du inte anger hello build-ID som du vill toouse hello-standard som ska användas automatiskt.<br>
Den här mekanismen gör att du - när du har en rekommendation modell i produktionen, nya modeller för toobuild och testa dem innan du höjer upp dem tooproduction.

| HTTP-metoden | URI: N |
|:--- |:--- |
| PLACERA |`<rootURI>/UpdateModel?id=%27<modelId>%27&apiVersion=%271.0%27`<br>Exempel:<br>`<rootURI>/UpdateModel?id=%279559872f-7a53-4076-a3c7-19d9385c1265%27&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| id |Unik identifierare för hello modellen (skiftlägeskänslig) |
| apiVersion |1.0 |
|  | |
| Begärandetext |`<ModelUpdateParams xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`<br>`<Description>New Description</Description>`<br>`<ActiveBuildId>-1</ActiveBuildId>`<br>` </ModelUpdateParams>`<br><br>Observera att hello XML taggar beskrivning och ActiveBuildId är valfria. Om du inte vill att tooset beskrivning eller ActiveBuildId bort hela hello-taggen. |

**Svaret**:

HTTP-statuskod: 200

### <a name="55----delete-model"></a>5.5.    Ta bort modellen
Tar bort en befintlig modell efter-ID.

| HTTP-metoden | URI: N |
|:--- |:--- |
| TA BORT |`<rootURI>/DeleteModel?id=%27<model_id>%27&apiVersion=%271.0%27`<br>Exempel:<br>`<rootURI>/DeleteModel?id=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| id |Unik identifierare för hello modellen (skiftlägeskänslig) |
| apiVersion |1.0 |
|  | |
| Begärandetext |INGEN |

**Svaret**:

HTTP-statuskod: 200

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Delete Model by Id</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-28T10:39:33Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">DeleteModelByIdEntity</title>
    <updated>2014-10-28T10:39:33Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:string m:type="Edm.String"></d:string>
      </m:properties>
    </content>
      </entry>
    </feed>

## <a name="6-model-advanced"></a>6. Avancerade modellen
### <a name="61----model-data-insight"></a>6.1.    Modellen Data Insight
Returnerar statistiska data på hello användningsdata som den här modellen har skapats med.

Endast tillgängligt för rekommendation build.

| HTTP-metoden | URI: N |
|:--- |:--- |
| HÄMTA |`<rootURI>/GetDataInsight?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br>Exempel:<br>`<rootURI>/GetDataInsight?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |Unik identifierare för hello modellen |
| apiVersion |1.0 |
|  | |
| Begärandetext |INGEN |

**Svaret**:

HTTP-statuskod: 200

hello data returneras som en uppsättning egenskaper.

* `feed/entry/id/content/properties/key`-Innehåller hello egenskapsnamn.
* `feed/entry/id/content/properties/value`-Innehåller hello egenskapsvärde.

hello tabellen nedan visar hello-värde som representerar varje nyckel.

| Nyckel | Beskrivning |
|:--- |:--- |
| AvgItemLength |Genomsnittligt antal specifika användare per artikel. |
| AvgUserLength |Genomsnittligt antal distinkta objekt per användare. |
| DensificationNumberOfItems |Antal objekt efter katalogrensning objekt som kan utformas. |
| DensificationNumberOfUsers |Antal användning poäng efter katalogrensning användare och objekt som kan utformas. |
| DensificationNumberOfRecords |Antal användning poäng efter katalogrensning användare och objekt som kan utformas. |
| MaxItemLength |Antal specifika användare för hello populäraste objektet. |
| MaxUserLength |Maximalt antal distinkta objekt för en användare. |
| MinItemLength |Maximalt antal specifika användare för ett objekt. |
| MinUserLength |Minimalt antal distinkta objekt för en användare. |
| RawNumberOfItems |Antal objekt i hello användningsfiler. |
| RawNumberOfUsers |Antal användning poäng innan alla rensning. |
| RawNumberOfRecords |Antal användning poäng innan alla rensning. |
| SamplingNumberOfItems |Saknas |
| SamplingNumberOfRecords |Saknas |
| SamplingNumberOfUsers |Saknas |

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get data insight statistics</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">AvgItemLength</d:Key>
        <d:Value m:type="Edm.String">18.936</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">AvgUserLength</d:Key>
        <d:Value m:type="Edm.String">51.879</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">DensificationNumberOfItems</d:Key>
        <d:Value m:type="Edm.String">2,000</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">DensificationNumberOfRecords</d:Key>
        <d:Value m:type="Edm.String">37,872</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
    <content type="application/xml">
        <m:properties>
            <d:Key m:type="Edm.String">DensificationNumberOfUsers</d:Key>
            <d:Value m:type="Edm.String">730</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MaxItemLength</d:Key>
                <d:Value m:type="Edm.String">4,704</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MaxUserLength</d:Key>
                <d:Value m:type="Edm.String">190</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MinItemLength</d:Key>
                <d:Value m:type="Edm.String">8</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MinUserLength</d:Key>
                <d:Value m:type="Edm.String">20</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">RawNumberOfItems</d:Key>
                <d:Value m:type="Edm.String">2,000</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">RawNumberOfRecords</d:Key>
                <d:Value m:type="Edm.String">72,022</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">RawNumberOfUsers</d:Key>
                <d:Value m:type="Edm.String">9,044</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">SampelingNumberOfItems</d:Key>
                <d:Value m:type="Edm.String">2,000</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=13&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=13&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">SampelingNumberOfRecords</d:Key>
                <d:Value m:type="Edm.String">72,022</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=14&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=14&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">SampelingNumberOfUsers</d:Key>
                <d:Value m:type="Edm.String">9,044</d:Value>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="62----model-insight"></a>6.2.    Modellen Insight
Returnerar modell insight hello active build eller (om det angetts) på en viss version.

Endast tillgängligt för rekommendation build.

| HTTP-metoden | URI: N |
|:--- |:--- |
| HÄMTA |Med active build-ID:<br>`<rootURI>/GetModelInsight?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Exempel:<br>`<rootURI>/GetModelInsight?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`<br><br>Med specifika build-ID:<br>`<rootURI>/GetModelInsight?modelId=%27<model_id>%27&buildId=%27<build_id>%27&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |Unik identifierare för hello modellen |
| buildId |Valfritt - nummer som identifierar en lyckad version. |
| apiVersion |1.0 |
|  | |
| Begärandetext |INGEN |

**Svaret**:

HTTP-statuskod: 200

hello data returneras som en uppsättning egenskaper.

* `feed/entry/id/content/properties/key`
* `feed/entry/id/content/properties/value`

hello tabellen nedan visar hello-värde som representerar varje nyckel.

| Nyckel | Beskrivning |
|:--- |:--- |
| CatalogCoverage |Vilken del av hello katalog kan utformas med användningsmönster. hello resten av hello objekt behöver innehållsbaserad funktioner. |
| MPR |Medelvärde percentil rangordning hello modellen. Lägre är bättre. |
| NumberOfDimensions |Antalet dimensioner som används av hello matrisen factorization algoritmen. |

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get model insight statistics</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-27T14:27:11Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">GetModelInsightStatisticsEntity</title>
        <updated>2014-10-27T14:27:11Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">CatalogCoverage</d:Key>
                <d:Value m:type="Edm.String">1.000</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetModelInsightStatisticsEntity</title>
        <updated>2014-10-27T14:27:11Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">Mpr</d:Key>
                <d:Value m:type="Edm.String">0.367</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">GetModelInsightStatisticsEntity</title>
        <updated>2014-10-27T14:27:11Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">NumberOfDimensions</d:Key>
                <d:Value m:type="Edm.String">20</d:Value>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="63----get-model-sample"></a>6.3.    Hämta modell-exempel
Hämtar ett exempel på hello rekommendation modellen.

| HTTP-metoden | URI: N |
|:--- |:--- |
| HÄMTA |`<rootURI>/GetModelSample?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br>Exempel:<br>`<rootURI>/GetModelSample?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`<br><br>Med specifika build-ID:<br>`<rootURI>/GetModelSample?modelId=%27<model_id>%27&buildId=%27<build_id>%27&apiVersion=%271.0%27`<br>Exempel:<br>`<rootURI>/GetModelSample?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&buildId=%271500068%27&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |Unik identifierare för hello modellen |
| apiVersion |1.0 |
|  | |
| Begärandetext |INGEN |

**Svaret**:

HTTP-statuskod: 200

OData-XML

Svar returneras i raw-format:

<pre>
Level 1
---------------
655fc955-a5a3-4a26-9723-3090859cb27b, Prey: A Novel
    655fc955-a5a3-4a26-9723-3090859cb27b, Prey: A Novel Rating: 0.5215
    3f471802-f84f-44a0-99c8-6d2e7418eec1, Black Hawk Down: A Story of Modern War Rating: 0.5151
    07b10e28-9e7c-4032-90b7-10acab7f2460, Cryptonomicon Rating: 0.5148
    6afc18e4-8c2a-43d1-9021-57543d6b11d8, Imajica Rating: 0.5146
    e4cc5e69-3567-43ab-b00f-f0d8d0506870, Hit List Rating: 0.514
56b61441-0eed-46cc-a8f6-112775b81892, Life and Death in Shanghai
    56b61441-0eed-46cc-a8f6-112775b81892, Life and Death in Shanghai Rating: 0.5218
    53156702-cc0c-443d-b718-6fb74b2491d3, Son of \ Rating: 0.5212
    fb8cf7a6-8719-46ee-97d4-92f931d77a3a, Smoke and Mirrors: Short Fictions and Illusions Rating: 0.5188
    8f5fe006-79e4-4679-816b-950989d1db4b, A Place I've Never Been (Contemporary American Fiction) Rating: 0.5156
    d8db4583-cc0f-49ce-bc95-b7fa3491623f, Happiness: A Novel Rating: 0.5156
50471eec-9aeb-4900-84d7-21567ab18546, If hello Buddha Dated: A Handbook for Finding Love on a Spiritual Path
    cfe922a1-7ca0-4f8d-ad9d-b7cc87bfe0ef, Divine Secrets of hello Ya-Ya Sisterhood: A Novel Rating: 0.5266
    ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, hello Poisonwood Bible: A Novel Rating: 0.5252
    973f8cbd-0846-4f6b-9d28-4dd0d7dc3a19, Pigs in Heaven Rating: 0.5244
    e2cbf7ad-0636-4117-8b30-298da6df7077, Animal Dreams Rating: 0.5227
    6c818fd3-5a09-417d-9ab4-7ffe090f0fef, Confessions of an Ugly Stepsister: A Novel Rating: 0.5222
5e97148f-defb-4d74-af2d-80f4763bf531, hello Deep End of hello Ocean (Oprah's Book Club)
    5e97148f-defb-4d74-af2d-80f4763bf531, hello Deep End of hello Ocean (Oprah's Book Club) Rating: 0.537
    5dcbac37-2946-4f2a-a0b3-bbe710f9409a, Up Island: A Novel Rating: 0.5277
    bc5b69db-733b-4346-adde-3927544258f7, Downtown Rating: 0.5275
    31fe5c63-3e5a-48d0-802b-d3b0f989a634, Have a Nice Day: A Tale of Blood and Sweatsocks Rating: 0.5252
    0adf981a-b65b-4c11-b36b-78aca2f948a2, hello Perfect Storm: A True Story of Men Against hello Sea Rating: 0.5238
68f97068-ae1a-4163-9e94-396b800b743d, Modoc: hello True Story of hello Greatest Elephant That Ever Lived
    68f97068-ae1a-4163-9e94-396b800b743d, Modoc: hello True Story of hello Greatest Elephant That Ever Lived Rating: 0.5379
    6724862e-e4e7-4022-9614-1468d8b902ff, Little House on hello Prairie Rating: 0.5345
    cdedb837-1620-496d-94c4-6ccfed888320, Little House in hello Big Woods Rating: 0.5325
    382164ba-406b-4187-b726-d7a54b9d790d, hello Tao of Pooh Rating: 0.5309
    6a068d6a-bb74-4ba3-b3f2-a956c4f9d1b5, On hello Banks of Plum Creek Rating: 0.5285
37ef8e74-e348-44e5-aabc-1d7f9efcb25b, Men Are from Mars Women Are from Venus: A Practical Guide for Improving Communication and Getting What You Want in Your Relationships
    37ef8e74-e348-44e5-aabc-1d7f9efcb25b, Men Are from Mars, Women Are from Venus: A Practical Guide for Improving Communication and Getting What You Want in Your Relationships Rating: 0.5397
    f2be16d4-5faf-4d32-ab83-7ba74d29261e, Politically Correct Bedtime Stories: Modern Tales for Our Life and Times Rating: 0.5207
    ef732c5c-334b-4d6b-ab82-7255eb7286d0, Honor Among Thieves Rating: 0.5195
    0b209b8c-7cdd-47fd-b940-05c7ff7c60fc, hello Giving Tree Rating: 0.5194
    883b360f-8b42-407f-b977-2f44ad840877, Scary Stories tooTell in hello Dark: Collected from American Folklore (Scary Stories) Rating: 0.5184
ff51b67e-fa8e-4c5e-8f4d-02a928de735d, Men at Work: hello Craft of Baseball
    d008dae9-c73a-40a1-9a9b-96d5cf546f36, hello Gulag Archipelago 1918-1956: An Experiment in Literary Investigation I-II Rating: 0.5416
    ff51b67e-fa8e-4c5e-8f4d-02a928de735d, Men at Work: hello Craft of Baseball Rating: 0.5403
    49dec30e-0adb-411a-b186-48eaabf6f8bc, Fatherland Rating: 0.5394
    cc7964fd-d30f-478e-a425-93ddbdf094ed, Magic hello Gathering: Arena Vol. 1 Rating: 0.5379
    8a1e9f36-97af-4614-bed9-24e3940a05f3, More Sniglets: Any Word That Doesn't Appear in hello Dictionary but Should Rating: 0.5377
12a6d988-be21-4a09-8143-9d5f4261ba16, A Dream of Eagles
    07b10e28-9e7c-4032-90b7-10acab7f2460, Cryptonomicon Rating: 0.5417
    e4cc5e69-3567-43ab-b00f-f0d8d0506870, Hit List Rating: 0.5416
    1f1a34c4-9781-49f5-a3cc-acec3ae3c71d, hello Family Rating: 0.5371
    56daeffe-7d48-43cd-8ef8-7dffd0c103d3, Kilo Class Rating: 0.5366
    b2fe511e-5cb9-4a56-b823-2801e63e6a96, Legal Tender Rating: 0.5366
df87525b-e435-4bd6-8701-4e60ad344e28, Finding Fish
    56d33036-dfda-46b9-8e2a-76cb03921bb0, hello X-Files: Ground Zero Rating: 0.5417
    0780cde8-6529-4e1d-b6c6-082c1b80e596, Twelve Red Herrings Rating: 0.5416
    df87525b-e435-4bd6-8701-4e60ad344e28, Finding Fish Rating: 0.5408
    400fe331-2c35-490c-adbc-b28b4b73d56c, Shall We Tell hello President? Rating: 0.5383
    f86ad7d0-5c03-42b3-aebf-13d44aec8b30, Shades of Grace Rating: 0.5358
de1f62a4-89e6-44d2-aaee-992a4bf093f1, hello Map That Changed hello World: William Smith and hello Birth of Modern Geology
    de1f62a4-89e6-44d2-aaee-992a4bf093f1, hello Map That Changed hello World: William Smith and hello Birth of Modern Geology Rating: 0.5422
    b303538f-e2c6-4a2c-b425-8d21e684fc3e, My Uncle Oswald Rating: 0.5385
    34b84627-48af-4a4c-96c4-b26fb3863f56, Midnight In hello Garden of Good and Evil Rating: 0.5379
    306cbaa7-b1a8-4142-9d55-e11b5018a7a8, hello Street Lawyer Rating: 0.5376
    e53b4baa-8c09-45c4-95c0-b6a26b98770b, Miss Smillas Feeling for Snow Rating: 0.5367

Level 2
---------------
352aaea1-6b12-454d-a3d5-46379d9e4eb2, hello Sinister Pig (Hillerman Tony)
    352aaea1-6b12-454d-a3d5-46379d9e4eb2, hello Sinister Pig (Hillerman Tony) Rating: 0.5425
    74c49398-bc10-4af5-a658-a996a1201254, Children of hello Storm (Peters Elizabeth) Rating: 0.5387
    9ba80080-196e-43fd-8025-391d963f77e7, hello Floating Girl Rating: 0.5372
    e68f81d5-7745-4cc7-b943-fedb8fcc2ced, Killer Smile (Scottoline Lisa) Rating: 0.5353
    b2fe511e-5cb9-4a56-b823-2801e63e6a96, Legal Tender Rating: 0.5332
c65c3995-abf7-4c7b-bb3c-8eb5aa9be7a5, Lake Wobegon days
    0adf981a-b65b-4c11-b36b-78aca2f948a2, hello Perfect Storm: A True Story of Men Against hello Sea Rating: 0.5433
    c65c3995-abf7-4c7b-bb3c-8eb5aa9be7a5, Lake Wobegon days Rating: 0.543
    a00ae6ad-4a7f-4211-9836-75ce8834eb11, Sniglets (Snig'lit: Any Word That Doesn't Appear in hello Dictionary But Should) Rating: 0.5327
    6f6e192e-0d64-49ca-9b63-f09413ea1ee6, Politically Correct Holiday Stories: For an Enlightened Yuletide Season Rating: 0.5307
    798051a8-147d-4d46-b0dc-e836325029e6, AGE OF INNOCENCE (MOVIE TIE-IN) Rating: 0.5301
73f3e25a-e996-4162-9ed8-ff3d34075650, O Pioneers! (Penguin Twentieth-Century Classics)
    cba8163f-6536-436b-8130-47b4a43c827f, Trust No One (hello Official Guide toohello X-Files Vol. 2) Rating: 0.5434
    5708e4cb-2492-49c0-94a8-cc413eec5d89, Small Gods (Discworld Novels (Paperback)) Rating: 0.5406
    73f3e25a-e996-4162-9ed8-ff3d34075650, O Pioneers! (Penguin Twentieth-Century Classics) Rating: 0.5403
    d885b0bd-ae4b-452d-bdf2-faa90197dbc9, hello Color of Magic Rating: 0.539
    b133a9c4-4784-4db3-b100-d0d6dffb94d2, hello Truth Is Out There (hello Official Guide toohello X-Files Vol. 1) Rating: 0.5367
271700a5-854a-4d5a-8409-6b57a5ee4de4, Fluke: Or I Know Why hello Winged Whale Sings
    271700a5-854a-4d5a-8409-6b57a5ee4de4, Fluke: Or I Know Why hello Winged Whale Sings Rating: 0.5445
    2de1c354-90ff-47c5-a0db-1bad7d88ef94, hello Salaryman's Wife (Children of Violence Series) Rating: 0.5329
    d279416e-19c0-43f8-9ec9-a585947879ca, Zen Attitude Rating: 0.5316
    c8f854d7-3de3-4b23-8217-f4f851670fd4, Revenge of hello Cootie Girls: A Robin Hudson Mystery (Robin Hudson Mysteries (Paperback)) Rating: 0.5305
    8ef4751c-7074-409e-a3ac-d49b222fc864, Where hello Wild Things Are Rating: 0.5289
9ad1b620-0a7b-4543-8673-66d4c3bcb2f1, Their Eyes Were Watching God
    9ad1b620-0a7b-4543-8673-66d4c3bcb2f1, Their Eyes Were Watching God Rating: 0.5446
    da45c4d5-aba1-413b-a9bd-50df98b1e1d2, hello Bean Trees Rating: 0.5389
    65ecbdd1-131c-40c3-a3d6-d86ca281377a, hello God of Small Things Rating: 0.5387
    c78743bf-7947-4a0c-8db7-8a3bfe69ba70, hello Stone Diaries Rating: 0.5355
    973f8cbd-0846-4f6b-9d28-4dd0d7dc3a19, Pigs in Heaven Rating: 0.5344
5f17d90a-2604-4fe8-8977-1a280b9098b1, One for hello Money (Stephanie Plum Novels (Paperback))
    5f17d90a-2604-4fe8-8977-1a280b9098b1, One for hello Money (Stephanie Plum Novels (Paperback)) Rating: 0.5446
    57169b2b-9a8a-486b-9aac-1ed98ce57168, Final Appeal Rating: 0.5332
    efcb1bc4-7278-4a8f-b491-befde02070d6, Moment of Truth Rating: 0.5329
    1efa91a2-993b-4c43-9f5c-3454fc12612d, Burn Factor Rating: 0.5309
    24c59962-458a-4ec8-b95d-d694e861919c, At Home in Mitford (hello Mitford Years) Rating: 0.5303
4fd48c46-1a20-4c57-bc7f-a02ef123dc52, As Nature Made Him: hello Boy Who Was Raised As a Girl
    4fd48c46-1a20-4c57-bc7f-a02ef123dc52, As Nature Made Him: hello Boy Who Was Raised As a Girl Rating: 0.5449
    cd5f2c03-20cb-43be-a1fb-3b4233e63222, Pigs in Heaven Rating: 0.5329
    19985fdb-d07a-4a25-ae4a-97b9cb61e5d1, Love in hello Time of Cholera (Penguin Great Books of hello 20th Century) Rating: 0.5267
    15689d09-c711-4844-84d8-130a90237b26, Bel Canto Rating: 0.5245
    ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, hello Poisonwood Bible: A Novel Rating: 0.5235
98df28ec-41e7-4fca-b77f-8b0d3109085d, Star Trek Memories
    f874b5a3-5d40-4436-94ff-0fa1c090ddf5, hello Sun Also Rises (A Scribner classic) Rating: 0.5451
    98df28ec-41e7-4fca-b77f-8b0d3109085d, Star Trek Memories Rating: 0.5442
    0ce0014a-9a48-4013-a08a-7f2c11877930, H.M.S. Unseen Rating: 0.5421
    15316ca6-1e38-425f-893d-691944a47000, More Scary Stories tooTell In hello Dark Rating: 0.5409
    329d5682-3dc3-4206-8aa2-eef4b1032258, Letters from hello Earth Rating: 0.54
5b9445d5-c072-419c-8d49-6f669bb1b0a9, Daughter of Fortune: A Novel (Oprah's Book Club (Hardcover))
    5b9445d5-c072-419c-8d49-6f669bb1b0a9, Daughter of Fortune: A Novel (Oprah's Book Club (Hardcover)) Rating: 0.5462
    ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, hello Poisonwood Bible: A Novel Rating: 0.5372
    604eb3bd-6026-4f51-bffd-9fb54f180400, Family Pictures: A Novel Rating: 0.5341
    8d06d01d-31cd-4678-b6b1-140a67987ce9, Songs in Ordinary Time (Oprah's Book Club (Paperback)) Rating: 0.5334
    da45c4d5-aba1-413b-a9bd-50df98b1e1d2, hello Bean Trees Rating: 0.5319
d5358189-d70f-4e35-8add-34b83b4942b3, Pigs in Heaven
    d5358189-d70f-4e35-8add-34b83b4942b3, Pigs in Heaven Rating: 0.5491
    ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, hello Poisonwood Bible: A Novel Rating: 0.5401
    c78743bf-7947-4a0c-8db7-8a3bfe69ba70, hello Stone Diaries Rating: 0.5393
    8d06d01d-31cd-4678-b6b1-140a67987ce9, Songs in Ordinary Time (Oprah's Book Club (Paperback)) Rating: 0.5382
    973f8cbd-0846-4f6b-9d28-4dd0d7dc3a19, Pigs in Heaven Rating: 0.5367

</pre>


## <a name="7-model-business-rules"></a>7. Affärsregler för modellen
Dessa är hello typer av regler som stöds:

* <strong>BlockList</strong> -BlockList kan du tooprovide en lista med objekt som du inte vill tooreturn i hello rekommendation resultat. 
* <strong>FeatureBlockList</strong> -funktionen BlockList aktiverar du tooblock objekt baserat på hello värden dess funktioner.

*Skicka inte fler än 1000 objekt i en enda blocklist regel eller samtalet kan timeout. Om du behöver tooblock fler än 1000 objekt kan göra du flera blocklist samtal.*

* <strong>Upsale</strong> -Upsale kan du tooenforce objekt tooreturn i hello rekommendation resultat.
* <strong>Godkända</strong> -lista för tillåten aktiverar du tooonly föreslår rekommendationerna från en lista med objekt.
* <strong>FeatureWhiteList</strong> -funktionen vitlista kan du tooonly rekommenderar objekt som innehåller värden för specifika funktioner.
* <strong>PerSeedBlockList</strong> - Per Seed Blockeringslista aktiverar tooprovide per objektet en lista med objekt som inte kan returneras som resultat av rekommendation.

### <a name="71----get-model-rules"></a>7.1.    Hämta modellen regler
| HTTP-metoden | URI: N |
|:--- |:--- |
| HÄMTA |`<rootURI>/GetModelRules?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br>Exempel:<br>`<rootURI>/GetModelRules?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |Unik identifierare för hello modellen |
| apiVersion |1.0 |
|  | |
| Begärandetext |INGEN |

**Svaret**:

HTTP-statuskod: 200

* `feed/entry/content/properties/Id`-Unik identifierare för den här regeln.
* `feed/entry/content/properties/Type`-Typ av hello regel.
* `feed/entry/content/properties/Parameter`-Regel parameter.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get a list of rules for a model</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T12:58:57Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">GetAListOfRulesForAModelEntity</title>
        <updated>2014-11-05T12:58:57Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">1000043</d:Id>
                <d:Type m:type="Edm.String">BlockList</d:Type>
                <d:Parameters m:type="Edm.String">{"ItemsToExclude":["1000"]}</d:Parameters>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetAListOfRulesForAModelEntity</title>
        <updated>2014-11-05T12:58:57Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">1000044</d:Id>
                <d:Type m:type="Edm.String">BlockList</d:Type>
                <d:Parameters m:type="Edm.String">{"ItemsToExclude":["1001"]}</d:Parameters>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="72----add-rule"></a>7.2.    Lägg till regel
| HTTP-metoden | URI: N |
|:--- |:--- |
| POST |`<rootURI>/AddRule?apiVersion=%271.0%27` |
| HUVUDET |`"Content-Type", "text/xml"` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| apiVersion |1.0 |
|  | |
| Begärandetext | |

<ins>När det är att tillhandahålla objekt-ID: N för affärsregler, att att toouse Hej externa hello objekt-Id (hello samma Id som du använde i hello katalogfil)</ins><br>
<ins>tooadd BlockList regel:</ins><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>BlockList</Type><Value>{"ItemsToExclude":["2406E770-769C-4189-89DE-1C9283F93A96","3906E110-769C-4189-89DE-1C9283F98888"]}</Value></ApiFilter>`<br><br><ins>
<ins>tooadd FeatureBlockList regel:</ins><br>
<br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>FeatureBlockList</Type><Value>{"Name":"Movie_category","Values":["Adult","Drama"]}</Value></ApiFilter>`<br><br><ins>tooadd en Upsale regel:</ins><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>Upsale</Type><Value>{"ItemsToUpsale":["2406E770-769C-4189-89DE-1C9283F93A96"],"NumberOfItemsToUpsale":5}</Value></ApiFilter>`<br><br>
<ins>tooadd en godkänd lista regel:</ins><br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>WhiteList</Type><Value>{"ItemsToInclude":["2406E770-769C-4189-89DE-1C9283F93A96","1116E770-769C-4189-89DE-1C9283F88888"]}</Value></ApiFilter>`<br><br><ins>
<ins>tooadd FeatureWhiteList regel:</ins><br>
<br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>FeatureWhiteList</Type><Value>{"Name":"Movie_rating","Values":["PG13"]}</Value></ApiFilter>`<br><br><ins>tooadd PerSeedBlockList regel:</ins><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>PerSeedBlockList</Type><Value>{"SeedItems":["9949"],"ItemsToExclude":["9862","8158","8244"]}</Value></ApiFilter>`|

**Svaret**:

HTTP-statuskod: 200

hello API returnerar hello nyligen skapade regeln med information om den. hello regler egenskapen kan hämtas från hello följande sökvägar:

* `feed/entry/content/properties/Id`-Unik identifierare för den här regeln.
* `feed/entry/content/properties/Type`-Typ av regel som hello: BlockList eller Upsale.
* `feed/entry/content/properties/Parameter`-Regel parameter.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/AddRule" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Add A Rule</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T11:13:28Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">AddARuleEntity</title>
        <updated>2014-11-05T11:13:28Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">1000041</d:Id>
                <d:Type m:type="Edm.String">BlockList</d:Type>
                <d:Parameters m:type="Edm.String">{"ItemsToExclude":["1002"]}</d:Parameters>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="73----delete-rule"></a>7.3.    Ta bort regeln
| HTTP-metoden | URI: N |
|:--- |:--- |
| TA BORT |`<rootURI>/DeleteRule?modelId=%27<model_id>%27&filterId=%27<filter_Id>%27&apiVersion=%271.0%27`<br><br>Exempel:<br>`DeleteRule?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&filterId=%271000011%27&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |Unik identifierare för hello modellen |
| filterId |Unik identifierare för hello filter |
| apiVersion |1.0 |
|  | |
| Begärandetext |INGEN |

**Svaret**:

HTTP-statuskod: 200

### <a name="74----delete-all-rules"></a>7.4.    Ta bort alla regler
| HTTP-metoden | URI: N |
|:--- |:--- |
| TA BORT |`<rootURI>/DeleteAllRules?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Exempel:<br>`DeleteAllRules?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |Unik identifierare för hello modellen |
| apiVersion |1.0 |
|  | |
| Begärandetext |INGEN |

**Svaret**:

HTTP-statuskod: 200

## <a name="8-catalog"></a>8. Katalogen
### <a name="81----import-catalog-data"></a>8.1.    Importera katalogdata
Om du överför flera katalog filer toohello samma modell med flera anrop infogas vi bara hello nya katalogobjekt. Befintliga objekt förblir med hello ursprungliga värden. Du kan inte uppdatera katalogdata med den här metoden.

hello katalogdata bör följa hello följande format:

* Utan funktioner-`<Item Id>,<Item Name>,<Item Category>[,<Description>]`
* Med funktioner-`<Item Id>,<Item Name>,<Item Category>,[<Description>],<Features list>`

Obs: hello maximal filstorlek är 200MB.

** Formatera information **

| Namn | Obligatorisk | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| Objekt-Id |Ja |[A-z], [a-z], [0-9] [_] &#40; Understreck &#41; [-] &#40; Dash &#41;<br> Maxlängd: 50 |Unik identifierare för ett objekt. |
| Objektnamnet |Ja |Alfanumeriska tecken<br> Maxlängd: 255 |Objektnamnet. |
| Objektet kategori |Ja |Alfanumeriska tecken <br> Maxlängd: 255 |Kategori toowhich artikeln tillhör (t.ex. matlagning böcker, Drama...); kan vara tom. |
| Beskrivning |Nej, såvida inte-funktioner finns (men kan vara tom) |Alfanumeriska tecken <br> Maxlängd: 4000 |Beskrivning av det här objektet. |
| Listan över funktioner |Nej |Alfanumeriska tecken <br> Maxlängd: 4000; Högsta antal funktioner: 20 |Kommaavgränsad lista över funktionens namn = värde för funktionen som kan använda tooenhance modellen rekommendation. Se [avancerade alternativ](#2-advanced-topics) avsnitt. |

| HTTP-metoden | URI: N |
|:--- |:--- |
| POST |`<rootURI>/ImportCatalogFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Exempel:<br>`<rootURI>/ImportCatalogFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27` |
| HUVUDET |`"Content-Type", "text/xml"` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |Unik identifierare för hello modellen |
| filnamn |Textrepresentation identifierare för hello katalog.<br>Endast bokstäver (A-Z, a-z), siffror (0-9), bindestreck (-) och understreck (_) är tillåtna.<br>Maxlängd: 50 |
| apiVersion |1.0 |
|  | |
| Begärandetext |Exempel (med funktioner):<br/>2406e770-769c-4189-89de-1c9283f93a96, Clara Callan boken hello beskrivning, redigera = Richard Wright utgivare = Harper Flamingo Kanada år = 2001<br>21bf8088-b6c0-4509-870c-e1c7ac78304a, hello Forgetting rummet: A fiktion (Byzantium bok), adressbok, redigera = Nick Bantock utgivare = Harpercollins, år = 1997<br>3bb5cb44-d143-4bdd-a55c-443964bf4b23, Spadework, adressbok, redigera = Timothy Findley utgivare = HarperFlamingo Kanada år = 2001<br>552a1940-21e4-4399-82bb-594b46d7ed54, begränsning av natur eller fauna, Book hello beskrivning, redigera = Magnus Mills, utgivare = arkad Publishing år = 1998</pre> |

**Svaret**:

HTTP-statuskod: 200

hello API returnerar en rapport över hello import.

* `feed\entry\content\properties\LineCount`-Antal rader som accepteras.
* `feed\entry\content\properties\ErrorCount`-Antal rader som inte infogades på grund av tooan fel.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Import catalog file</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'" />
    <entry>
       <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">ImportCatalogFileEntity2</title>
        <updated>2014-10-05T06:58:04Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:LineCount m:type="Edm.String">4</d:LineCount>
                <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="82----get-catalog"></a>8.2.    Hämta katalogen
Hämtar alla katalogobjekt.
hello katalog kommer att hämta en sida i taget. Om du vill tooget objekt vid ett visst index, kan du använda hello $skip odata-parameter. Till exempel om du vill tooget objekt som startar vid position 100, lägger du till hello parametern $skip = 100 toohello begäran.

| HTTP-metoden | URI: N |
|:--- |:--- |
| HÄMTA |`<rootURI>/GetCatalog?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Exempel:<br>`GetCatalog?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |Unik identifierare för hello modellen |
| apiVersion |1.0 |
|  | |
| Begärandetext |INGEN |

**Svaret**:

HTTP-statuskod: 200

hello svaret innehåller en post per katalogobjekt. Varje post innehåller hello följande data:

* `feed/entry/content/properties/ExternalId`-Katalog-ID för extern, hello en som tillhandahålls av hello kunden.
* `feed/entry/content/properties/InternalId`-Katalogen objektet internt ID, hello en Azure Machine Learning rekommendationer har genererat.
* `feed/entry/content/properties/Name`-Objektet katalognamnet.
* `feed/entry/content/properties/Category`-Objektet kategorier.
* `feed/entry/content/properties/Description`-Katalogen beskrivning.
* `feed/entry/content/properties/Metadata`-Katalogen objektmetadata.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get All Catalog Items</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">552A1940-21E4-4399-82BB-594B46D7ED54</d:ExternalId>
                <d:InternalId m:type="Edm.String">060db26a-e6a6-464e-bb52-436d2da82a50</d:InternalId>
                <d:Name m:type="Edm.String">Restraint of Beasts</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">2406E770-769C-4189-89DE-1C9283F93A96</d:ExternalId>
                <d:InternalId m:type="Edm.String">209d7bfe-2eb9-4455-92a3-7c867a41a74a</d:InternalId>
                <d:Name m:type="Edm.String">Clara Callan</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">3BB5CB44-D143-4BDD-A55C-443964BF4B23</d:ExternalId>
                <d:InternalId m:type="Edm.String">913ff79b-059b-4d4d-8042-6fa4cc1391dd</d:InternalId>
                <d:Name m:type="Edm.String">Spadework</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">21BF8088-B6C0-4509-870C-E1C7AC78304A</d:ExternalId>
                <d:InternalId m:type="Edm.String">ea65e4fa-768c-40b4-92c3-69d3e8178691</d:InternalId>
                <d:Name m:type="Edm.String">hello Forgetting Room: A Fiction (Byzantium Book)</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="83----get-catalog-items-by-token"></a>8.3.    Hämta katalogobjekt av Token
| HTTP-metoden | URI: N |
|:--- |:--- |
| HÄMTA |`<rootURI>/GetCatalogItemsByToken?modelId=%27<modelId>%27&token=%27<token>%27&apiVersion=%271.0%27`<br><br>Exempel:<br>`GetCatalogItemsByToken?modelId=%270dbb55fa-7f11-418d-8537-8ff2d9d1d9c6%27&token=%27Cla%27&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |Unik identifierare för hello modellen |
| Token |Token för hello katalog objektets namn. Ska innehålla minst 3 tecken. |
| apiVersion |1.0 |
|  | |
| Begärandetext |INGEN |

**Svaret**:

HTTP-statuskod: 200

hello svaret innehåller en post per katalogobjekt. Varje post innehåller hello följande data:

* `feed/entry/content/properties/InternalId`-Katalogen objektet internt ID, hello en Azure Machine Learning rekommendationer har genererat.
* `feed/entry/content/properties/Name`-Objektet katalognamnet.
* `feed/entry/content/properties/Rating`-(för framtida användning)
* `feed/entry/content/properties/Reasoning`-(för framtida användning)
* `feed/entry/content/properties/Metadata`-(för framtida användning)
* `feed/entry/content/properties/FormattedRating`-(för framtida användning)

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get Catalog items that contain a token</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-29T11:48:19Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">CatalogItemsThatContainATokenEntity</title>
            <updated>2014-10-29T11:48:19Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                  <m:properties>
                    <d:Id m:type="Edm.String">2406E770-769C-4189-89DE-1C9283F93A96</d:Id>
                    <d:Name m:type="Edm.String">Clara Callan</d:Name>
                    <d:Rating m:type="Edm.Double">0</d:Rating>
                    <d:Reasoning m:type="Edm.String"></d:Reasoning>
                    <d:Metadata m:type="Edm.String"></d:Metadata>
                    <d:FormattedRating m:type="Edm.Double" m:null="true"></d:FormattedRating>
                  </m:properties>
            </content>
        </entry>
    </feed>

## <a name="9-usage-data"></a>9. Användningsdata
### <a name="91----import-usage-data"></a>9.1.    Importera Data om programvaruanvändning
#### <a name="911-uploading-file"></a>9.1.1. Ladda upp fil
Det här avsnittet visas hur tooupload användningsdata med hjälp av en fil. Du kan anropa denna API flera gånger med användningsdata. Alla användningsdata sparas för alla samtal.

| HTTP-metoden | URI: N |
|:--- |:--- |
| POST |`<rootURI>/ImportUsageFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Exempel:<br>`<rootURI>/ImportUsageFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27ImplicitMatrix10_Guid_small.txt%27&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |Unik identifierare för hello modellen |
| filnamn |Textrepresentation identifierare för hello katalog.<br>Endast bokstäver (A-Z, a-z), siffror (0-9), bindestreck (-) och understreck (_) är tillåtna.<br>Maxlängd: 50 |
| apiVersion |1.0 |
|  | |
| Begärandetext |Användningsdata. Format:<br>`<User Id>,<Item Id>[,<Time>,<Event>]`<br><br><table><tr><th>Namn</th><th>Obligatorisk</th><th>Typ</th><th>Beskrivning</th></tr><tr><td>Användar-Id</td><td>Ja</td><td>[A-z], [a-z], [0-9] [_] &#40; Understreck &#41; [-] &#40; Dash &#41;<br> Maxlängd: 255 </td><td>Unik identifierare för en användare.</td></tr><tr><td>Objekt-Id</td><td>Ja</td><td>[A-z], [a-z], [0-9] [& #95.] &#40; Understreck &#41; [-] &#40; Dash &#41;<br> Maxlängd: 50</td><td>Unik identifierare för ett objekt.</td></tr><tr><td>Tid</td><td>Nej</td><td>Datum i format: ÅÅÅÅ/MM/ddTHH (t.ex. 2013/06/20T10:00:00)</td><td>Tid för data.</td></tr><tr><td>Händelse</td><td>Nej. Om anges måste också sätta datum</td><td>En av hello följande:<br>• Klicka på<br>• RecommendationClick<br>• AddShopCart<br>• RemoveShopCart<br>• Köp</td><td></td></tr></table><br>Maximal filstorlek: 200MB<br><br>Exempel:<br><pre>149452,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>6360,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>50321,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>71285,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>224450,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>236645,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>107951,1b3d95e2-84e4-414c-bb38-be9cf461c347</pre> |

**Svaret**:

HTTP-statuskod: 200

* `Feed\entry\content\properties\LineCount`-Antal rader som accepteras.
* `Feed\entry\content\properties\ErrorCount`-Antal rader som inte infogades på grund av tooan fel.
* `Feed\entry\content\properties\FileId`-Filen identifierare.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Add bulk usage data (usage file)</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T07:21:44Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">AddBulkUsageDataUsageFileEntity2</title>
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">33</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
        <d:FileId m:type="Edm.String">fead7c1c-db01-46c0-872f-65bcda36025d</d:FileId>
      </m:properties>
    </content>
      </entry>
    </feed>


#### <a name="912-using-data-acquisition"></a>9.1.2. Med hjälp av datainsamling
Det här avsnittet visar hur toosend händelser i real time tooAzure Machine Learning rekommendationer, vanligen från din webbplats.

| HTTP-metoden | URI: N |
|:--- |:--- |
| POST |`<rootURI>/AddUsageEvent?apiVersion=%271.0%27` |
| HUVUDET |`"Content-Type", "text/xml"` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| apiVersion |1.0 |
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
                    <PurchaseItem>
                        <ItemId>ABBF8081-C5C0-4F09-9701-E1C7AC78304A</ItemId>
                        <Count>1</Count>
                    </PurchaseItem>
                    <PurchaseItem>
                        <ItemId>21BF8088-B6C0-4509-870C-11C0AC7F304B</ItemId>
                        <Count>3</Count>
                    </PurchaseItem>
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

### <a name="92----list-model-usage-files"></a>9.2.    Lista modellen Användningsfiler
Hämtar metadata för alla filer för användning av modellen.
hello hämta användning filer kommer att en sida i taget. Varje containes 100 objekt. Om du vill tooget objekt vid ett visst index, kan du använda hello $skip odata-parameter. Till exempel om du vill tooget objekt som startar vid position 100, lägger du till hello parametern $skip = 100 toohello begäran.

| HTTP-metoden | URI: N |
|:--- |:--- |
| HÄMTA |`<rootURI>/ListModelUsageFiles?forModelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Exempel:<br>`<rootURI>/ListModelUsageFiles?forModelId=%270dbb55fa-7f11-418d-8537-8ff2d9d1d9c6%27&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| forModelId |Unik identifierare för hello modellen |
| apiVersion |1.0 |
|  | |
| Begärandetext |INGEN |

**Svaret**:

HTTP-statuskod: 200

hello svaret innehåller en post per fil för användning. Varje post innehåller hello följande data:

* `feed\entry\content\properties\Id`-Användning fil-ID.
* `feed\entry\content\properties\Length`-Användning fillängden i MB.
* `feed\entry\content\properties\DateModified`-Datum när hello användning filen skapades.
* `feed\entry\content\properties\UseInModel`– Om hello användning filen används i hello modellen.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get a list of model's usage files info</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-30T09:40:25Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetAListOfModelsUsageFilesInfoEntity</title>
            <updated>2014-10-30T09:40:25Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Id m:type="Edm.String">4c067b42-e975-4cb2-8c98-a6ab80ed6d63</d:Id>
                    <d:Length m:type="Edm.Double">0</d:Length>
                    <d:DateModified m:type="Edm.String">10/30/2014 9:19:57 AM</d:DateModified>
                    <d:UseInModel m:type="Edm.String">true</d:UseInModel>
                </m:properties>
            </content>
        </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetAListOfModelsUsageFilesInfoEntity</title>
        <updated>2014-10-30T09:40:25Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">3126d816-4e80-4248-8339-1ebbdb9d544d</d:Id>
                <d:Length m:type="Edm.Double">0.001</d:Length>
                <d:DateModified m:type="Edm.String">10/30/2014 9:21:44 AM</d:DateModified>
                <d:UseInModel m:type="Edm.String">true</d:UseInModel>
              </m:properties>
        </content>
    </entry>
</feed>

### <a name="93----get-usage-statistics"></a>9.3.    Hämta användningsstatistik
Hämtar användningsstatistik.

| HTTP-metoden | URI: N |
|:--- |:--- |
| HÄMTA |`<rootURI>/GetUsageStatistics?modelId=%27<modelId>%27& startDate=%27<date>%27&endDate=%27<date>%27&eventTypes=%27<types>%27&apiVersion=%271.0%27`<br><br>Exempel:<br>`<rootURI>/GetUsageStatistics?modelId=%271d20c34f-dca1-4eac-8e5d-f299e4e4ad66%27&startDate=%272014%2F10%2F17T00%3A00%3A00%27&endDate=%272014%2F11%2F16T00%3A00%3A00%27&eventTypes=%271%2C2%2C3%2C4%2C5%27&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |Unik identifierare för hello modellen |
| Startdatum |Startdatum. Format: ÅÅÅÅ/MM/ddTHH |
| Slutdatum |Slutdatumet. Format: ÅÅÅÅ/MM/ddTHH |
| eventTypes |Kommaavgränsad sträng för händelsen typer eller null tooget alla händelser |
| apiVersion |1.0 |
|  | |
| Begärandetext |INGEN |

**Svaret**:

HTTP-statuskod: 200

En samling nyckel/värde-element. Var och en innehåller hello summan av händelser för en specifik händelsetyp grupperas per timme.

* `feed\entry[i]\content\properties\Key`-Innehåller hello tid (grupperade per timme) och hello händelsetyp.
* `feed\entry[i]\content\properties\Value`-Totalt antal.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get usage statistics</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-18T11:39:16Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/9/2014 8:00:00 AM;Click</d:Event>
                <d:Value m:type="Edm.String">5</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/9/2014 8:00:00 AM;RecommendationClick</d:Event>
                <d:Value m:type="Edm.String">10</d:Value>
              </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/1/2014 8:00:00 AM;RemoveShopCart</d:Event>
                <d:Value m:type="Edm.String">10</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/5/2014 8:00:00 AM;RemoveShopCart</d:Event>
                <d:Value m:type="Edm.String">10</d:Value>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="94----get-usage-file-sample"></a>9.4.    Hämta exempel användning
Hämtar hello första 2KB på filinnehåll för användning.

| HTTP-metoden | URI: N |
|:--- |:--- |
| HÄMTA |`<rootURI>/GetUsageFileSample?modelId=%27<modelId>%27& fileId=%27<fileId>%27&apiVersion=%271.0%27`<br><br>Exempel:<br>`<rootURI>/GetUsageFileSample?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&fileId=%274c067b42-e975-4cb2-8c98-a6ab80ed6d63%27&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |Unik identifierare för hello modellen |
| fileId |Unik identifierare för hello modellfil för användning |
| apiVersion |1.0 |
|  | |
| Begärandetext |INGEN |

**Svaret**:

HTTP-statuskod: 200

Svar returneras i raw-format:

<pre>
85526,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
210926,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
116866,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
177458,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
274004,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
123883,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
37712,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
152249,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
250948,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
235588,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
158254,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
271195,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
141157,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
171118,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
225087,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
</pre>


### <a name="95----get-model-usage-file"></a>9.5.    Hämta modellfil för användning
Hämtar hello fullständiga innehållet på hello användning av filen.

| HTTP-metoden | URI: N |
|:--- |:--- |
| HÄMTA |`<rootURI>/GetModelUsageFile?mid=%27<modelId>%27& fid=%27<fileId>%27&download=%27<download_value>%27&apiVersion=%271.0%27`<br><br>Exempel:<br>`<rootURI>/GetModelUsageFile?mid=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&fid=%273126d816-4e80-4248-8339-1ebbdb9d544d%27&download=%271%27&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| Mid |Unik identifierare för hello modellen |
| FID |Unik identifierare för hello modellfil för användning |
| Ladda ned |1 |
| apiVersion |1.0 |
|  | |
| Begärandetext |INGEN |

**Svaret**:

HTTP-statuskod: 200

Svar returneras i raw-format:

<pre>
85526,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
210926,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
116866,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
177458,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
274004,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
123883,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
37712,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
152249,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
250948,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
235588,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
158254,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
271195,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
141157,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
171118,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
225087,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
244881,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
50547,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
213090,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
260655,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
72214,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
189334,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
36326,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
189336,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
189334,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
260655,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
162100,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
54946,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
260965,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
102758,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
112602,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
163925,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
262998,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
144717,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
</pre>

### <a name="96----delete-usage-file"></a>9.6.    Ta bort filen användning
Tar bort hello angivna användning modellfil.

| HTTP-metoden | URI: N |
|:--- |:--- |
| TA BORT |`<rootURI>/DeleteUsageFile?modelId=%27<modelId>%27&fileId=%27<fileId>%27&apiVersion=%271.0%27`<br><br>Exempel:<br>`<rootURI>/DeleteUsageFile?modelId=%270f86d698-d0f4-4406-a684-d13d22c47a73%27&fileId=%27f2e0b09d-be5c-46b2-9ac2-c7f622e5e1a5%27&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |Unik identifierare för hello modellen |
| fileId |Unik identifierare för hello filen toobe bort |
| apiVersion |1.0 |
|  | |
| Begärandetext |INGEN |

**Svaret**:

HTTP-statuskod: 200

### <a name="97----delete-all-usage-files"></a>9.7.    Ta bort alla Användningsfiler
Tar bort alla filer för användning av modellen.

| HTTP-metoden | URI: N |
|:--- |:--- |
| TA BORT |`<rootURI>/DeleteAllUsageFiles?modelId=%27<modelId>%27&apiVersion=%271.0%27`<br><br>Exempel:<br>`<rootURI>/DeleteAllUsageFiles?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |Unik identifierare för hello modellen |
| apiVersion |1.0 |
|  | |
| Begärandetext |INGEN |

**Svaret**:

HTTP-statuskod: 200

## <a name="10-features"></a>10. Funktioner
Det här avsnittet visar hur tooretrieve funktion information, till exempel hello importeras funktioner och deras värden, deras rangordning och när den här rang allokerades. Funktioner importeras som en del av hello katalogdata och sedan deras rangordning associeras när en rank build görs.
Funktionen RANG kan ändra bl.a toohello mönster för användningsdata och typen av objekt. Men för konsekvent användning/artiklar hello rang bör endast små variationer.
hello rangordningen för funktioner som är ett icke-negativt tal. hello nummer 0 innebär att funktionen hello inte rangordnas (händer om du anropar den här API tidigare toohello slutförande av hello första rank build). hello datum då hello rang har attributet kallas hello poäng dokumentens.

### <a name="101-get-features-info-for-last-rank-build"></a>10.1. Hämta information om funktioner (för senaste Rank Build)
Hämtar hello Funktionsinformation, inklusive rangordning för hello senaste lyckade rank-version.

| HTTP-metoden | URI: N |
|:--- |:--- |
| HÄMTA |`<rootURI>/GetModelFeatures?modelId=%27<modelId>%27&samplingSize=%27<samplingSize>%27&apiVersion=%271.0%27`<br><br>Exempel:<br>`<rootURI>/GetModelFeatures?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&samplingSize=10%27&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |Unik identifierare för hello modellen |
| samplingSize |Antal värden tooinclude för varje funktion enligt toohello data som finns i hello-katalogen. <br/>Möjliga värden:<br> -1 - alla prover. <br>0 - ingen provtagning. <br>N - returnera N prover för varje funktionsnamnet. |
| apiVersion |1.0 |
|  | |
| Begärandetext |INGEN |

**Svaret**:

HTTP-statuskod: 200

hello svaret innehåller en lista över funktionen info transaktioner. Varje post innehåller:

* `feed/entry/content/m:properties/d:Name`-Funktionsnamnet.
* `feed/entry/content/m:properties/d:RankUpdateDate`-Datumet på vilka hello rang var allokerade toothis funktion, kallas även poäng dokumentens funktion. Ett tidigare datum ('0001-01-01T00:00:00 ”) innebär att inga rank build utfördes.
* `feed/entry/content/m:properties/d:Rank`-Funktionen RANG (flytande). En en rangordning 2.0 och uppåt anses vara en bra funktion.
* `feed/entry/content/m:properties/d:SampleValues`– Kommaavgränsad lista med värden upp toohello provtagning storlek som har begärts.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get hello features of a model</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2015-01-08T13:15:02Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">ModelFeaturesEntity</title>
        <updated>2015-01-08T13:15:02Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Name m:type="Edm.String">Author</d:Name>
                <d:RankUpdateDate m:type="Edm.String">0001-01-01T00:00:00</d:RankUpdateDate>
                <d:Rank m:type="Edm.String">0</d:Rank>
                <d:SampleValues m:type="Edm.String">A. A. Attanasio, A. A. Milne, A. Bates, A. C. Bhaktivedanta Swami Prabhupada et al., A. C. Crispin, A. C. Doyle, A. C. H. Smith, A. E. Parker, A. J. Holt, A. J. Matthews</d:SampleValues>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">ModelFeaturesEntity</title>
        <updated>2015-01-08T13:15:02Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Name m:type="Edm.String">Publisher</d:Name>
                <d:RankUpdateDate m:type="Edm.String">0001-01-01T00:00:00</d:RankUpdateDate>
                <d:Rank m:type="Edm.String">0</d:Rank>
                <d:SampleValues m:type="Edm.String">A. Mondadori, Abacus, Abacus Press, Abacus Uk, Abstract Studio, Acacia Press, Academy Chicago Publishers, Ace Books, ACE Charter, Actar</d:SampleValues>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">ModelFeaturesEntity</title>
        <updated>2015-01-08T13:15:02Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1"/>
        <contenttype="application/xml">
        <m:properties>
            <d:Name m:type="Edm.String">Year</d:Name>
            <d:RankUpdateDate m:type="Edm.String">0001-01-01T00:00:00</d:RankUpdateDate>
            <d:Rank m:type="Edm.String">0</d:Rank>
            <d:SampleValues m:type="Edm.String">0, 1920, 1926, 1927, 1929, 1930, 1932, 1942, 1943, 1946</d:SampleValues>
        </m:properties>
        </content>
    </entry>
</feed>

### <a name="102-get-features-info-for-specific-rank-build"></a>10.2. Hämta information om funktioner (om en specifik Rank Build)
Hämtar hello Funktionsinformation, inklusive hello rangordning för en specifik rank version.

| HTTP-metoden | URI: N |
|:--- |:--- |
| HÄMTA |`<rootURI>/GetModelFeatures?modelId=%27<modelId>%27&samplingSize=%27<samplingSize>%27&rankBuildId=<rankBuildId>&apiVersion=%271.0%27`<br><br>Exempel:<br>`<rootURI>/GetModelFeatures?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&samplingSize=10%27&rankBuildId=1000551&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |Unik identifierare för hello modellen |
| samplingSize |Antal värden tooinclude för varje funktion enligt toohello data som finns i hello-katalogen.<br/> Möjliga värden:<br> -1 - alla prover. <br>0 - ingen provtagning. <br>N - returnera N prover för varje funktionsnamnet. |
| rankBuildId |Unik identifierare för hello rank build eller -1 för hello senaste rank version |
| apiVersion |1.0 |
|  | |
| Begärandetext |INGEN |

**Svaret**:

HTTP-statuskod: 200

hello svaret innehåller en lista över funktionen info transaktioner. Varje post innehåller:

* `feed/entry/content/m:properties/d:Name`-Funktionsnamnet.
* `feed/entry/content/m:properties/d:RankUpdateDate`-Datumet på vilka hello rang var allokerade toothis funktion, kallas även poäng dokumentens funktion. Ett tidigare datum ('0001-01-01T00:00:00 ”) innebär att inga rank build utfördes.
* `feed/entry/content/m:properties/d:Rank`-Funktionen RANG (flytande). En en rangordning 2.0 och uppåt anses vara en bra funktion.
* `feed/entry/content/m:properties/d:SampleValues`– Kommaavgränsad lista med värden upp toohello provtagning storlek som har begärts.

OData

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get hello features of a model</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2015-01-08T13:54:22Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">ModelFeaturesEntity</title>
            <updated>2015-01-08T13:54:22Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Name m:type="Edm.String">Author</d:Name>
                    <d:RankUpdateDate m:type="Edm.String">2015-01-08T13:52:14.673</d:RankUpdateDate>
                    <d:Rank m:type="Edm.String">3.38867426</d:Rank>
                    <d:SampleValues m:type="Edm.String">A. A. Attanasio, A. A. Milne, A. Bates, A. C. Bhaktivedanta Swami Prabhupada et al., A. C. Crispin, A. C. Doyle, A. C. H. Smith, A. E. Parker, A. J. Holt, A. J. Matthews</d:SampleValues>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
            <title type="text">ModelFeaturesEntity</title>
            <updated>2015-01-08T13:54:22Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Name m:type="Edm.String">Publisher</d:Name>
                    <d:RankUpdateDate m:type="Edm.String">2015-01-08T13:52:14.673</d:RankUpdateDate>
                    <d:Rank m:type="Edm.String">1.67839336</d:Rank>
                    <d:SampleValues m:type="Edm.String">A. Mondadori, Abacus, Abacus Press, Abacus Uk, Abstract Studio, Acacia Press, Academy Chicago Publishers, Ace Books, ACE Charter, Actar</d:SampleValues>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
            <title type="text">ModelFeaturesEntity</title>
            <updated>2015-01-08T13:54:22Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Name m:type="Edm.String">Year</d:Name>
                    <d:RankUpdateDate m:type="Edm.String">2015-01-08T13:52:14.673</d:RankUpdateDate>
                    <d:Rank m:type="Edm.String">1.12352145</d:Rank>
                    <d:SampleValues m:type="Edm.String">0, 1920, 1926, 1927, 1929, 1930, 1932, 1942, 1943, 1946</d:SampleValues>
                </m:properties>
            </content>
        </entry>
    </feed>


## <a name="11-build"></a>11. Utveckla
  Det här avsnittet beskrivs hello olika API: er relaterade toobuilds. Det finns 3 typer av versioner: en rekommendation version, en rank version och en FBT (ofta köpt tillsammans) version.

hello rekommendation build syftet är toogenerate en rekommendation modell används för förutsägelser. Förutsägelser (för den här typen av build) förekommer i två varianter:

* I2I – kallas även Objektet tooItem rekommendationer - ett objekt eller en lista med objekt kommer det här alternativet förutsäga en lista med objekt som är sannolikt toobe högt intresse.
* U2I – kallas även Rekommendationerna som användaren tooItem - ges ett användar-id (och eventuellt en lista med objekt) det här alternativet kommer förutsäga en lista med objekt som är sannolikt toobe högt intresse för hello angivna användare (och dess ytterligare val av objekt). Hej U2I rekommendationer baserat på hello historik över objekt som var av intresse för hello användaren driftstid toohello hello modellen har skapats.

Rank build är en teknisk version som du kan använda toolearn om hello användbarhet funktioner. Du bör vanligtvis vidta hello följa stegen i ordning tooget hello bästa resultatet för en rekommendation modell som omfattar funktioner:

* Utlösa en rank version (om inte hello poäng funktioner är stabil) och vänta tills du får hello funktionen poäng.
* Hämta hello rang funktioner genom att anropa hello [visa funktioner Info](#101-get-features-info-for-last-rank-build) API.
* Konfigurera en rekommendation version med hello följande parametrar:
  * `useFeatureInModel`-Ange tooTrue.
  * `ModelingFeatureList`-Ange tooa kommaavgränsad lista över funktioner med ett resultat på 2.0 eller flera enligt (toohello rangordningar som du hämtade i hello föregående steg).
  * `AllowColdItemPlacement`-Ange tooTrue.
  * Om du vill ange `EnableFeatureCorrelation` tooTrue och `ReasoningFeatureList` toohello lista över funktioner som du vill toouse förklaringar (vanligtvis hello samma lista över funktioner som används i modellering eller en underordnad lista).
* Utlösaren hello rekommendation build med hello konfigurerade parametrar.

Obs: Om du inte konfigurerar några parametrar (t.ex. anropa hello rekommendation build utan parametrar) eller du inte uttryckligen inaktiverar hello användning av funktioner (t.ex. `UseFeatureInModel` ange tooFalse), hello system ställer in hello funktionen-relaterade parametrar toohello beskrivs värdena ovan om det finns en rank version.

Det finns ingen begränsning om hur du kör en rank version och en rekommendation skapa samtidigt för hello samma modell. Du kan dock köra två versioner av samma typ i samma modell parallellt hello hello.

Bygga en FBT (ofta köpt tillsammans) ännu en annan rekommendationer algoritm kallas ibland ”konservativ” rekommenderare som är bra för kataloger som inte är likartade egenskaper (homogena: böcker, filmer, vissa mat sätt; heterogen: dator och enheter, domäner, hög olika).

Obs: om hello-användningsfiler som du överfört innehåller hello Fritextfält ”typ” för FBT modellering endast ”köp” händelser används. Om det finns inga händelsetyp alla händelser betraktas som köp.

#### <a name="111-build-parameters"></a>11,1 skapa parametrar
Varje build-typ kan konfigureras via en uppsättning parametrar (visas nedan). Om du inte konfigurerar hello parametrar hello system automatiskt att attributet toohello parametervärden enligt toohello informationen hello tidpunkt du utlöser en version.

##### <a name="1111-usage-condenser"></a>11.1.1. Användning kylaren
Användare eller objekt med några användning kan innehålla flera brus än information. hello system försöker toopredict hello minimalt antal punkter användning per användare/artikel toobe användas i en modell. Det här antalet blir inom hello-intervall som definieras av hello ItemCutoffLowerBound och ItemCutoffUpperBound parametrar för artiklar och hello-intervall som definieras av hello UserCutOffLowerBound och UserCutoffUpperBound parametrar för användare. hello kylaren effekt på objekt eller användare kan minimeras genom att ange minst en av hello motsvarande gränser toozero.

##### <a name="1112-rank-build-parameters"></a>11.1.2. Rangordning skapa parametrar
hello tabellen nedan visar hello build parametrar för en rank version.

| Nyckel | Beskrivning | Typ | Giltigt värde |
|:--- |:--- |:--- |:--- |
| NumberOfModelIterations |hello antal upprepningar hello modellen presterar avspeglas av hello övergripande compute tid och hello modellen noggrannhet. hello högre hello nummer, hello bättre noggrannhet visas, men hello beräkna tid tar det längre tid. |Integer |10-50 |
| NumberOfModelDimensions |hello antalet dimensioner relaterar toohello antal 'funktioner' hello modellen försöker toofind i dina data. Öka hello antal dimensioner kan bättre finjustering av hello resultat i mindre kluster. För många dimensioner kommer dock förhindra att hello modellen från att hitta visar sambandet mellan objekt. |Integer |10-40 |
| ItemCutOffLowerBound |Definierar hello objektet nedre gräns för hello kylaren. Se användning kylaren ovan. |Integer |2 eller högre (0 inaktivera kylaren) |
| ItemCutOffUpperBound |Definierar hello objektet övre gränsen för hello kylaren. Se användning kylaren ovan. |Integer |2 eller högre (0 inaktivera kylaren) |
| UserCutOffLowerBound |Definierar hello användaren nedre gräns för hello kylaren. Se användning kylaren ovan. |Integer |2 eller högre (0 inaktivera kylaren) |
| UserCutOffUpperBound |Definierar hello användaren övre gränsen för hello kylaren. Se användning kylaren ovan. |Integer |2 eller högre (0 inaktivera kylaren) |

##### <a name="1113-recommendation-build-parameters"></a>11.1.3. Rekommendation build-parametrar
hello tabellen nedan visar hello build parametrar för rekommendation version.

| Nyckel | Beskrivning | Typ | Giltigt värde |
|:--- |:--- |:--- |:--- |
| NumberOfModelIterations |hello antal upprepningar hello modellen presterar avspeglas av hello övergripande compute tid och hello modellen noggrannhet. hello högre hello nummer, hello bättre noggrannhet visas, men hello beräkna tid tar det längre tid. |Integer |10-50 |
| NumberOfModelDimensions |hello antalet dimensioner relaterar toohello antal 'funktioner' hello modellen försöker toofind i dina data. Öka hello antal dimensioner kan bättre finjustering av hello resultat i mindre kluster. För många dimensioner kommer dock förhindra att hello modellen från att hitta visar sambandet mellan objekt. |Integer |10-40 |
| ItemCutOffLowerBound |Definierar hello objektet nedre gräns för hello kylaren. Se användning kylaren ovan. |Integer |2 eller högre (0 inaktivera kylaren) |
| ItemCutOffUpperBound |Definierar hello objektet övre gränsen för hello kylaren. Se användning kylaren ovan. |Integer |2 eller högre (0 inaktivera kylaren) |
| UserCutOffLowerBound |Definierar hello användaren nedre gräns för hello kylaren. Se användning kylaren ovan. |Integer |2 eller högre (0 inaktivera kylaren) |
| UserCutOffUpperBound |Definierar hello användaren övre gränsen för hello kylaren. Se användning kylaren ovan. |Integer |2 eller högre (0 inaktivera kylaren) |
| Beskrivning |Skapa beskrivning. |Sträng |All text maximalt 512 tecken |
| EnableModelingInsights |Tillåter toocompute mått på hello rekommendation modellen. |Booleskt värde |SANT/FALSKT |
| UseFeaturesInModel |Anger om funktioner som kan användas i ordning tooenhance hello rekommendation modellen. |Booleskt värde |SANT/FALSKT |
| ModelingFeatureList |Kommaavgränsad lista över funktionen namn toobe används i hello rekommendation build i ordning tooenhance hello rekommendation. |Sträng |Funktionsnamn in too512 tecken |
| AllowColdItemPlacement |Anger om hello rekommendation ska också push-installera cold objekt via funktionen likhet. |Booleskt värde |SANT/FALSKT |
| EnableFeatureCorrelation |Anger om funktioner som kan användas i motivationen. |Booleskt värde |SANT/FALSKT |
| ReasoningFeatureList |Kommaavgränsad lista över funktionen namn toobe används för skäl meningar (t.ex. rekommendation förklaringar). |Sträng |Funktionsnamn in too512 tecken |
| EnableU2I |Tillåt hello personliga rekommendation kallas även U2I (användare tooitem rekommendationer). |Booleskt värde |SANT/FALSKT (standard SANT) |

##### <a name="1114-fbt-build-parameters"></a>11.1.4. FBT build-parametrar
hello tabellen nedan visar hello build parametrar för rekommendation version.

| Nyckel | Beskrivning | Typ | Giltigt värde (standard) |
|:--- |:--- |:--- |:--- |
| FbtSupportThreshold |Hur konservativ hello modellen. Antalet samtidigt förekomster av objekt toobe för förutsägelsemodellering. |Integer |3-50 (6) |
| FbtMaxItemSetSize |Gränser hello antal objekt i en ofta. |Integer |2-3 (2) |
| FbtMinimalScore |Minimal poäng som ofta set bör ha i ordning toobe ingår i hello returnerade resultat. hello högre hello bättre. |dubbla |0 och senare (0) |
| FbtSimilarityFunction |Definierar hello likhet funktionen toobe används av hello build. Lift prioriterar serendipity samtidigt förekomsten prioriterar förutsägbarhet och Jaccard är en bra kompromiss mellan hello två. |Sträng |cooccurrence lift, jaccard (lift) |

### <a name="112-trigger-a-recommendation-build"></a>11.2. Utlös en rekommendation version
  Som standard utlöser detta API en rekommendation modellen version. tootrigger rangen skapa (i ordning tooscore funktioner), hello build API variant med build typparameter som ska användas.

| HTTP-metoden | URI: N |
|:--- |:--- |
| POST |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&apiVersion=%271.0%27`<br><br>Exempel:<br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&apiVersion=%271.0%27` |
| HUVUDET |`"Content-Type", "text/xml"`(Om skickar brödtext i begäran) |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |Unik identifierare för hello modellen |
| userDescription |Textrepresentation identifierare för hello katalog. Observera att om du använder lagringsutrymmen du koda den med % 20 i stället. Finns i exemplet ovan.<br>Maxlängd: 50 |
| apiVersion |1.0 |
|  | |
| Begärandetext |Om den lämnas tom kommer hello build att köras med hello-standardparametrar build.<br><br>Om du vill tooset hello build parametrar, skicka hello parametrar som XML till hello meddelandetext som i följande exempel hello. (Hello ”Build parametrar” avsnittet finns en förklaring av hello parametrar.)`<NumberOfModelIterations>40</NumberOfModelIterations><NumberOfModelDimensions>20</NumberOfModelDimensions><MinItemAppearance>5</MinItemAppearance><MinUserAppearance>5</MinUserAppearance><EnableModelingInsights>true</EnableModelingInsights><UseFeaturesInModel>false</UseFeaturesInModel><ModelingFeatureList>feature_name_1,feature_name_2,...</ModelingFeatureList><AllowColdItemPlacement>false</AllowColdItemPlacement><EnableFeatureCorrelation>false</EnableFeatureCorrelation><ReasoningFeatureList>feature_name_a,feature_name_b,...</ReasoningFeatureList></BuildParametersList>` |

**Svaret**:

HTTP-statuskod: 200

Detta är en asynkron API. Du får ett build-ID som en respons. tooknow när hello build har slutförts ska du anropa hello ”hämta bygger Status för en modell” API och leta upp det skapa ID hello svar. Observera att en version kan ta från toohours minuter beroende på hello storleken på hello data.

Du kan inte använda rekommendationer till hello skapa avslutas.

Giltig build-status:

* Skapa - Build begäran skapades.
* I kö - Build-begäran har skickats och den är i kö.
* Skapa - Build pågår.
* Slutfördes - Build slutförts utan fel.
* Fel - Build avslutades med ett fel.
* Avbröts - avbröts Build.
* Avbryter - en begäran om avbrytning för hello version skickades.

Observera att hello build ID finns under hello följande sökväg:`Feed\entry\content\properties\Id`

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Build a Model with RequestBody</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T08:56:34Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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

### <a name="113-trigger-build-recommendation-rank-or-fbt"></a>11.3. Utlösaren Build (rekommendation, rangordning eller FBT)
| HTTP-metoden | URI: N |
|:--- |:--- |
| POST |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&buildType=%27<buildType>%27&apiVersion=%271.0%27`<br><br>Exempel:<br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&buildType=%27Ranking%27&apiVersion=%271.0%27` |
| HUVUDET |`"Content-Type", "text/xml"`(Om skickar brödtext i begäran) |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |Unik identifierare för hello modellen |
| userDescription |Textrepresentation identifierare för hello katalog. Observera att om du använder lagringsutrymmen du koda den med % 20 i stället. Finns i exemplet ovan.<br>Maxlängd: 50 |
| buildType |Typ av hello build tooinvoke: <br/> -Rekommendation om du för rekommendation version <br> -Rangordning för rank version <br/> -Fbt om du för FBT version |
| apiVersion |1.0 |
|  | |
| Begärandetext |Om den lämnas tom kommer hello build att köras med hello-standardparametrar build.<br><br>Om du vill tooset build-parametrar måste skickas som XML till hello meddelandetext som i följande exempel hello. (Hello ”Build parametrar” avsnittet finns en förklaring och en fullständig lista över hello parametrar.)`<BuildParametersList><NumberOfModelIterations>40</NumberOfModelIterations><NumberOfModelDimensions>20</NumberOfModelDimensions><MinItemAppearance>5</MinItemAppearance><MinUserAppearance>5</MinUserAppearance></BuildParametersList>` |

**Svaret**:

HTTP-statuskod: 200

Detta är en asynkron API. Du får ett build-ID som en respons. tooknow när hello build har slutförts ska du anropa hello ”hämta bygger Status för en modell” API och leta upp det skapa ID hello svar. Observera att en version kan ta från toohours minuter beroende på hello storleken på hello data.

Du kan inte använda rekommendationer till hello skapa avslutas.

Giltig build-status:

* Skapa - modellen har skapats.
* I kö - modellen build utlöstes och den är i kö.
* Skapa - modellen skapas.
* Slutfördes - Build slutförts utan fel.
* Fel - Build avslutades med ett fel.
* Avbröts - avbröts Build.
* Avbryter - Build avbryts.

Observera att hello build ID finns under hello följande sökväg:`Feed\entry\content\properties\Id`

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Build a Model with RequestBody</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T08:56:34Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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




### <a name="114-get-builds-status-of-a-model"></a>11.4. Hämta Status för versioner av en modell
Hämtar versioner och deras status för en angiven modell.

| HTTP-metoden | URI: N |
|:--- |:--- |
| HÄMTA |`<rootURI>/GetModelBuildsStatus?modelId=%27<modelId>%27&onlyLastBuild=<bool>&apiVersion=%271.0%27`<br><br>Exempel:<br>`<rootURI>/GetModelBuildsStatus?modelId=%279559872f-7a53-4076-a3c7-19d9385c1265%27&onlyLastBuild=true&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |Unik identifierare för hello modellen |
| onlyLastBuild |Anger om tooreturn alla hello skapa historik över hello modell eller endast hello status hello senaste version |
| apiVersion |1.0 |

**Svaret**:

HTTP-statuskod: 200

hello svaret innehåller en post per version. Varje post innehåller hello följande data:

* `feed/entry/content/properties/UserName`-Namnet på hello användare.
* `feed/entry/content/properties/ModelName`-Namnet på hello modell.
* `feed/entry/content/properties/ModelId`-Modellen Unik identifierare.
* `feed/entry/content/properties/IsDeployed`– Om hello anmärkningar distribueras (kallas även aktiva build).
* `feed/entry/content/properties/BuildId`-Skapa Unik identifierare.
* `feed/entry/content/properties/BuildType`-Typen av hello build.
* `feed/entry/content/properties/Status`-Skapa status. Kan vara något av följande hello: fel, skapa, i kö, Cancelling, avbrott, lyckas.
* `feed/entry/content/properties/StatusMessage`-Det detaljerade statusmeddelanden (gäller endast toospecific tillstånd).
* `feed/entry/content/properties/Progress`-Skapa förlopp (%).
* `feed/entry/content/properties/StartTime`-Skapa starttid.
* `feed/entry/content/properties/EndTime`-Skapa sluttid.
* `feed/entry/content/properties/ExecutionTime`-Skapa varaktighet.
* `feed/entry/content/properties/ProgressStep`-Information om hello aktuella etappen en pågår.

Giltig build-status:

* Skapa - skapades Build posten.
* I kö - Build begäran utlöstes och den är i kö.
* Skapa - Build pågår.
* Slutfördes - Build slutförts utan fel.
* Fel - Build avslutades med ett fel.
* Avbröts - avbröts Build.
* Avbryter - Build avbryts.

Giltiga värden för build-typ:

* Rangordning - skapa rang.
* Rekommendation - rekommendation build.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get builds status of a model</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-11-05T17:51:10Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetBuildsStatusEntity</title>
            <updated>2014-11-05T17:51:10Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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


### <a name="115-get-builds-status"></a>11.5. Hämta Status för versioner
Hämtar skapa status för alla modeller för en användare.

| HTTP-metoden | URI: N |
|:--- |:--- |
| HÄMTA |`<rootURI>/GetUserBuildsStatus?onlyLastBuilds=<bool>&apiVersion=%271.0%27`<br><br>Exempel:<br>`<rootURI>/GetUserBuildsStatus?onlyLastBuilds=true&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| onlyLastBuild |Anger om alla hello tooreturn skapa historik över hello modell eller endast hello status hello den senaste versionen. |
| apiVersion |1.0 |

**Svaret**:

HTTP-statuskod: 200

hello svaret innehåller en post per version. Varje post innehåller hello följande data:

* `feed/entry/content/properties/UserName`-Namnet på hello användare.
* `feed/entry/content/properties/ModelName`-Namnet på hello modell.
* `feed/entry/content/properties/ModelId`-Modellen Unik identifierare.
* `feed/entry/content/properties/IsDeployed`– Om hello anmärkningar distribueras.
* `feed/entry/content/properties/BuildId`-Skapa Unik identifierare.
* `feed/entry/content/properties/BuildType`-Typen av hello build.
* `feed/entry/content/properties/Status`-Skapa status. Kan vara något av följande hello: fel, skapa, i kö, avbrott, Cancelling, lyckas.
* `feed/entry/content/properties/StatusMessage`-Det detaljerade statusmeddelanden (gäller endast toospecific tillstånd).
* `feed/entry/content/properties/Progress`-Skapa förlopp (%).
* `feed/entry/content/properties/StartTime`-Skapa starttid.
* `feed/entry/content/properties/EndTime`-Skapa sluttid.
* `feed/entry/content/properties/ExecutionTime`-Skapa varaktighet.
* `feed/entry/content/properties/ProgressStep`-Information om hello aktuella etappen en pågår.

Giltig build-status:

* Skapa - skapades Build posten.
* I kö - Build begäran utlöstes och den är i kö.
* Skapa - Build pågår.
* Slutfördes - Build slutförts utan fel.
* Fel - Build avslutades med ett fel.
* Avbröts - avbröts Build.
* Avbryter - Build avbryts.

Giltiga värden för build-typ:

* Rangordning - skapa rang.
* Rekommendation - rekommendation build.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get builds status of a user</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-11-05T18:41:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetBuildsStatusEntity</title>
            <updated>2014-11-05T18:41:21Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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


### <a name="116-delete-build"></a>11.6. Ta bort Build
Tar bort en version.

OBS! <br>Du kan inte ta bort en aktiv version. hello modellen ska vara uppdaterade tooa olika active build innan du tar bort den.<br>Du kan inte ta bort en pågående version. Avbryt först hello build genom att anropa <strong>Avbryt skapa</strong>.

| HTTP-metoden | URI: N |
|:--- |:--- |
| TA BORT |`<rootURI>/DeleteBuild?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br>Exempel:<br>`<rootURI>/DeleteBuild?buildId=%271500068%27&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| buildId |Unik identifierare för hello build. |
| apiVersion |1.0 |

**Svar:**

HTTP-statuskod: 200

### <a name="117-cancel-build"></a>11.7. Avbryt Build
Avbryter en version som har skapa status.

| HTTP-metoden | URI: N |
|:--- |:--- |
| PLACERA |`<rootURI>/CancelBuild?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br>Exempel:<br>`<rootURI>/CancelBuild?buildId=%271500076%27&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| buildId |Unik identifierare för hello build. |
| apiVersion |1.0 |

**Svar:**

HTTP-statuskod: 200

### <a name="118-get-build-parameters"></a>11.8. Hämta Build-parametrar
Hämtar skapa parametrar.

| HTTP-metoden | URI: N |
|:--- |:--- |
| HÄMTA |`<rootURI>/GetBuildParameters?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br>Exempel:<br>`<rootURI>/GetBuildParameters?buildId=%271000653%27&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| buildId |Unik identifierare för hello build. |
| apiVersion |1.0 |

**Svar:**

HTTP-statuskod: 200

Detta API returnerar en mängd med nyckel/värde-element. Varje element representerar en parameter och dess värde:

* `feed/entry/content/properties/Key`-Skapa parameternamnet.
* `feed/entry/content/properties/Value`-Skapa parametervärdet.

hello tabellen nedan visar hello-värde som representerar varje nyckel.

| Nyckel | Beskrivning | Typ | Giltigt värde |
|:--- |:--- |:--- |:--- |
| NumberOfModelIterations |hello antal upprepningar hello modellen presterar avspeglas av hello övergripande compute tid och hello modellen noggrannhet. hello högre hello nummer, hello bättre noggrannhet visas, men hello beräkna tid tar det längre tid. |Integer |10-50 |
| NumberOfModelDimensions |hello antalet dimensioner relaterar toohello antal 'funktioner' hello modellen försöker toofind i dina data. Öka hello antal dimensioner kan bättre finjustering av hello resultat i mindre kluster. För många dimensioner kommer dock förhindra att hello modellen från att hitta visar sambandet mellan objekt. |Integer |10-40 |
| ItemCutOffLowerBound |Definierar hello objektet nedre gräns för hello kylaren. Se användning kylaren ovan. |Integer |2 eller högre (0 inaktivera kylaren) |
| ItemCutOffUpperBound |Definierar hello objektet övre gränsen för hello kylaren. Se användning kylaren ovan. |Integer |2 eller högre (0 inaktivera kylaren) |
| UserCutOffLowerBound |Definierar hello användaren nedre gräns för hello kylaren. Se användning kylaren ovan. |Integer |2 eller högre (0 inaktivera kylaren) |
| UserCutOffUpperBound |Definierar hello användaren övre gränsen för hello kylaren. Se användning kylaren ovan. |Integer |2 eller högre (0 inaktivera kylaren) |
| Beskrivning |Skapa beskrivning. |Sträng |All text maximalt 512 tecken |
| EnableModelingInsights |Tillåter toocompute mått på hello rekommendation modellen. |Booleskt värde |SANT/FALSKT |
| UseFeaturesInModel |Anger om funktioner som kan användas i ordning tooenhance hello rekommendation modellen. |Booleskt värde |SANT/FALSKT |
| ModelingFeatureList |Kommaavgränsad lista över funktionen namn toobe används i hello rekommendation build i ordning tooenhance hello rekommendation. |Sträng |Funktionsnamn in too512 tecken |
| AllowColdItemPlacement |Anger om hello rekommendation ska också push-installera cold objekt via funktionen likhet. |Booleskt värde |SANT/FALSKT |
| EnableFeatureCorrelation |Anger om funktioner som kan användas i motivationen. |Booleskt värde |SANT/FALSKT |
| ReasoningFeatureList |Kommaavgränsad lista över funktionen namn toobe används för skäl meningar (t.ex. rekommendation förklaringar). |Sträng |Funktionsnamn in too512 tecken |

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get build parameters</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2015-01-08T13:50:57Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">UseFeaturesInModel</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">AllowColdItemPlacement</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">EnableFeatureCorrelation</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">EnableModelingInsights</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">NumberOfModelIterations</d:Key>
                    <d:Value m:type="Edm.String">10</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
            <titletype="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">NumberOfModelDimensions</d:Key>
                    <d:Value m:type="Edm.String">10</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <linkrel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ItemCutOffLowerBound</d:Key>
                    <d:Value m:type="Edm.String">2</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ItemCutOffUpperBound</d:Key>
                    <d:Value m:type="Edm.String">2147483647</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">UserCutOffLowerBound</d:Key>
                    <d:Value m:type="Edm.String">2</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">UserCutOffUpperBound</d:Key>
                    <d:Value m:type="Edm.String">2147483647</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ModelingFeatureList</d:Key>
                    <d:Value m:type="Edm.String"/>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ReasoningFeatureList</d:Key>
                    <d:Value m:type="Edm.String"/>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">Description</d:Key>
                    <d:Value m:type="Edm.String">rankBuild</d:Value>
                </m:properties>
            </content>
        </entry>
    </feed>

## <a name="12-recommendation"></a>12. Rekommendation
### <a name="121-get-item-recommendations-for-active-build"></a>12.1. Få rekommendationer för objektet (för aktiva build)
Få rekommendationer för hello active build av typen ”Recommendation” eller ”Fbt” baserat på en lista med objekt frö (indata).

| HTTP-metoden | URI: N |
|:--- |:--- |
| HÄMTA |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Exempel:<br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |Unik identifierare för hello modellen |
| ItemId |Kommaavgränsad lista över hello objekt toorecommend för. <br>Ange om hello active build är av FBT skickar du bara ett objekt. <br>Maxlängd: 1024 |
| numberOfResults |Antalet nödvändiga resultat <br> Max: 150 |
| includeMetatadata |Framtida användning alltid false |
| apiVersion |1.0 |

**Svar:**

HTTP-statuskod: 200

hello svaret innehåller en post per rekommenderade objekt. Varje post innehåller hello följande data:

* `Feed\entry\content\properties\Id`-Rekommenderade objekt-ID.
* `Feed\entry\content\properties\Name`-Namnet på hello-objektet.
* `Feed\entry\content\properties\Rating`-Klassificering av hello rekommendation. högre nummer innebär högre tillförlitlighet.
* `Feed\entry\content\properties\Reasoning`-Rekommendation skäl till (t.ex. rekommendation förklaringar).

hello exempel svaret nedan innehåller 10 rekommenderade objekt.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Get Recommendation</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T12:28:48Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
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

### <a name="122-get-item-recommendations-of-a-specific-build"></a>12.2. Få rekommendationer för objektet (för en specifik version)
Få rekommendationer för en specifik version av typen ”Recommendation” eller ”Fbt”.

| HTTP-metoden | URI: N |
|:--- |:--- |
| HÄMTA |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br>Exempel:<br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&buildId=1234&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |Unik identifierare för hello modellen |
| ItemId |Kommaavgränsad lista över hello objekt toorecommend för. <br>Ange om hello active build är av FBT skickar du bara ett objekt. <br>Maxlängd: 1024 |
| numberOfResults |Antalet nödvändiga resultat <br> Max: 150 |
| includeMetatadata |Framtida användning alltid false |
| buildId |hello skapa id toouse för denna rekommendation begäran |
| apiVersion |1.0 |

**Svar:**

HTTP-statuskod: 200

hello svaret innehåller en post per rekommenderade objekt. Varje post innehåller hello följande data:

* `Feed\entry\content\properties\Id`-Rekommenderade objekt-ID.
* `Feed\entry\content\properties\Name`-Namnet på hello-objektet.
* `Feed\entry\content\properties\Rating`-Klassificering av hello rekommendation. högre nummer innebär högre tillförlitlighet.
* `Feed\entry\content\properties\Reasoning`-Rekommendation skäl till (t.ex. rekommendation förklaringar).

Se ett exempel svar i 12.1

### <a name="123-get-fbt-recommendations-for-active-build"></a>12.3. Hämta FBT rekommendationer (för aktiva build)
Få rekommendationer av hello active version av typen ”Fbt” baserat på ett startvärde (indata)-objekt.

| HTTP-metoden | URI: N |
|:--- |:--- |
| HÄMTA |`<rootURI>/ItemFbtRecommend?modelId=%27<modelId>%27&itemId=%27<itemId>%27&numberOfResults=<int>&minimalScore=<double>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Exempel:<br>`<rootURI>/ItemFbtRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemId=%271003%27&numberOfResults=10&minimalScore=<double>&includeMetadata=false&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |Unik identifierare för hello modellen |
| itemId |Objektet toorecommend för. <br>Maxlängd: 1024 |
| numberOfResults |Antalet nödvändiga resultat <br>Max: 150 |
| minimalScore |Minimal poäng som ofta set bör ha i ordning toobe ingår i hello returnerade resultat |
| includeMetatadata |Framtida användning alltid false |
| apiVersion |1.0 |

**Svar:**

HTTP-statuskod: 200

hello svaret innehåller en post per rekommenderade objektet uppsättningen (en uppsättning objekt som vanligtvis köpas tillsammans med hello seed-indata objekt). Varje post innehåller hello följande data:

* `Feed\entry\content\properties\Id1`-Rekommenderade objekt-ID.
* `Feed\entry\content\properties\Name1`-Namnet på hello-objektet.
* `Feed\entry\content\properties\Id2`2 rekommenderade artikel-ID (valfritt).
* `Feed\entry\content\properties\Name2`-Namnet på hello 2 objektet (valfritt).
* `Feed\entry\content\properties\Rating`-Klassificering av hello rekommendation. högre nummer innebär högre tillförlitlighet.
* `Feed\entry\content\properties\Reasoning`-Rekommendation skäl till (t.ex. rekommendation förklaringar).

Hej exempelsvar nedan innehåller 3 rekommenderade objektet uppsättningar.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Get Recommendation</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemId='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T12:28:48Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetFbtRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id1 m:type="Edm.String">159</d:Id1>
        <d:Name1 m:type="Edm.String">159</d:Name1>
        <d:Id2 m:type="Edm.String"></d:Id2>
        <d:Name2 m:type="Edm.String"></d:Name2>
        <d:Rating m:type="Edm.Double">0.543343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who bought '1003' also bought '159'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetFbtRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id1 m:type="Edm.String">52</d:Id1>
        <d:Name1 m:type="Edm.String">52</d:Name1>
        <d:Id2 m:type="Edm.String"></d:Id2>
        <d:Name2 m:type="Edm.String"></d:Name2>
        <d:Rating m:type="Edm.Double">0.533343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who bought '1003' also bought '52'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetFbtRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id1 m:type="Edm.String">35</d:Id1>
        <d:Name1 m:type="Edm.String">35</d:Name1>
        <d:Id2 m:type="Edm.String">102</d:Id2>
        <d:Name2 m:type="Edm.String">102</d:Name2>
        <d:Rating m:type="Edm.Double">0.523343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who bought '1003' also bought '35' and '102'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
    </feed>

### <a name="124-get-fbt-recommendations-of-a-specific-build"></a>12.4. Få rekommendationer FBT (för en specifik version)
Få rekommendationer för en specifik version av typen ”Fbt”.

| HTTP-metoden | URI: N |
|:--- |:--- |
| HÄMTA |`<rootURI>/ItemFbtRecommend?modelId=%27<modelId>%27&itemId=%27<itemId>%27&numberOfResults=<int>&minimalScore=<double>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br>Exempel:<br>`<rootURI>/ItemFbtRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemId=%271003%27&numberOfResults=10&minimalScore=0.1&includeMetadata=false&buildId=1234&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |Unik identifierare för hello modellen |
| itemId |Objektet toorecommend för. <br>Maxlängd: 1024 |
| numberOfResults |Antalet nödvändiga resultat <br>Max: 150 |
| minimalScore |Minimal poäng som ofta set bör ha i ordning toobe ingår i hello returnerade resultat |
| includeMetatadata |Framtida användning alltid false |
| buildId |hello skapa id toouse för denna rekommendation begäran |
| apiVersion |1.0 |

**Svar:**

HTTP-statuskod: 200

hello svaret innehåller en post per rekommenderade objektet uppsättningen (en uppsättning objekt som vanligtvis köpas tillsammans med hello seed-indata objekt). Varje post innehåller hello följande data:

* `Feed\entry\content\properties\Id1`-Rekommenderade objekt-ID.
* `Feed\entry\content\properties\Name1`-Namnet på hello-objektet.
* `Feed\entry\content\properties\Id2`2 rekommenderade artikel-ID (valfritt).
* `Feed\entry\content\properties\Name2`-Namnet på hello 2 objektet (valfritt).
* `Feed\entry\content\properties\Rating`-Klassificering av hello rekommendation. högre nummer innebär högre tillförlitlighet.
* `Feed\entry\content\properties\Reasoning`-Rekommendation skäl till (t.ex. rekommendation förklaringar).

Se ett exempel svar i 12.3

### <a name="125-get-user-recommendations-for-active-build"></a>12.5. Få rekommendationer för användare (för aktiva build)
Få rekommendationer för användare av en version av typen ”Recommendation” markerats som aktiv build.

hello API returneras en lista över förväntade objekt enligt toohello användningshistorik för hello användare.

Information: 

1. Det finns ingen användare rekommendation för FBT version.
2. Om hello active skapa är FBT som den här metoden ska returneras ett fel.

| HTTP-metoden | URI: N |
|:--- |:--- |
| HÄMTA |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Exempel:<br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |Unik identifierare för hello modellen |
| Användar-ID |Unik identifierare för hello användare |
| numberOfResults |Antalet nödvändiga resultat |
| includeMetatadata |Framtida användning alltid false |
| apiVersion |1.0 |

**Svar:**

HTTP-statuskod: 200

hello svaret innehåller en post per rekommenderade objekt. Varje post innehåller hello följande data:

* `Feed\entry\content\properties\Id`-Rekommenderade objekt-ID.
* `Feed\entry\content\properties\Name`-Namnet på hello-objektet.
* `Feed\entry\content\properties\Rating`-Klassificering av hello rekommendation. högre nummer innebär högre tillförlitlighet.
* `Feed\entry\content\properties\Reasoning`-Rekommendation skäl till (t.ex. rekommendation förklaringar).

Se ett exempel svar i 12.1

### <a name="126-get-user-recommendations-with-item-list-for-active-build"></a>12.6. Få rekommendationer för användare med listan över objekt (för aktiva build)
Få rekommendationer för användare av en version av typen ”Recommendation” markerats som aktiv build med en ytterligare lista med objekt

hello API returneras en lista över förväntade objekt enligt toohello användningshistorik för hello användaren och hello ytterligare angivna objekt.

Information: 

1. Det finns ingen användare rekommendation för FBT version.
2. Om hello active skapa är FBT som den här metoden ska returneras ett fel.

| HTTP-metoden | URI: N |
|:--- |:--- |
| HÄMTA |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>&itemsIds=%27<itemsIds>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Exempel:<br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&itemsIds=%271003%2C1000%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |Unik identifierare för hello modellen |
| Användar-ID |Unik identifierare för hello användare |
| itemsIds |Kommaavgränsad lista över hello objekt toorecommend för. Maxlängd: 1024 |
| numberOfResults |Antalet nödvändiga resultat |
| includeMetatadata |Framtida användning alltid false |
| apiVersion |1.0 |

**Svar:**

HTTP-statuskod: 200

hello svaret innehåller en post per rekommenderade objekt. Varje post innehåller hello följande data:

* `Feed\entry\content\properties\Id`-Rekommenderade objekt-ID.
* `Feed\entry\content\properties\Name`-Namnet på hello-objektet.
* `Feed\entry\content\properties\Rating`-Klassificering av hello rekommendation. högre nummer innebär högre tillförlitlighet.
* `Feed\entry\content\properties\Reasoning`-Rekommendation skäl till (t.ex. rekommendation förklaringar).

Se ett exempel svar i 12.1

### <a name="127-get-user-recommendations--of-a-specific-build"></a>12.7. Få rekommendationer för användare (för en specifik version)
Få rekommendationer för användare av en viss version av typen ”Recommendation”.

hello API returneras en lista över förväntade objekt enligt toohello användningshistorik för hello användare (används i hello specifika build).

Obs: Det finns ingen användare rekommendation för FBT version.

| HTTP-metoden | URI: N |
|:--- |:--- |
| HÄMTA |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br>Exempel:<br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&numberOfResults=10&includeMetadata=false&buildId=50012&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |Unik identifierare för hello modellen |
| Användar-ID |Unik identifierare för hello användare |
| numberOfResults |Antalet nödvändiga resultat |
| includeMetatadata |Framtida användning alltid false |
| buildId |hello skapa id toouse för denna rekommendation begäran |
| apiVersion |1.0 |

**Svar:**

HTTP-statuskod: 200

hello svaret innehåller en post per rekommenderade objekt. Varje post innehåller hello följande data:

* `Feed\entry\content\properties\Id`-Rekommenderade objekt-ID.
* `Feed\entry\content\properties\Name`-Namnet på hello-objektet.
* `Feed\entry\content\properties\Rating`-Klassificering av hello rekommendation. högre nummer innebär högre tillförlitlighet.
* `Feed\entry\content\properties\Reasoning`-Rekommendation skäl till (t.ex. rekommendation förklaringar).

Se ett exempel svar i 12.1

### <a name="128-get-user-recommendations-with-item-list-of-a-specific-build"></a>12.8. Få rekommendationer för användare med listan över objekt (för en specifik version)
Få rekommendationer för användare av en viss version av typen ”Recommendation” och hello lista över ytterligare objekt.

hello API returneras en lista över förväntade objekt enligt toohello användningshistorik för hello användare och hello ytterligare lista med objekt.

Obs: Tdet är ingen användare rekommendation för FBT version.

| HTTP-metoden | URI: N |
|:--- |:--- |
| HÄMTA |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&itemsIds=%27<itemsIds>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br>Exempel:<br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&itemsIds=%271003%27&numberOfResults=10&includeMetadata=false&buildId=50012&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |Unik identifierare för hello modellen |
| Användar-ID |Unik identifierare för hello användare |
| ItemId |Kommaavgränsad lista över hello objekt toorecommend för. Maxlängd: 1024 |
| numberOfResults |Antalet nödvändiga resultat |
| includeMetatadata |Framtida användning alltid false |
| buildId |hello skapa id toouse för denna rekommendation begäran |
| apiVersion |1.0 |

**Svar:**

HTTP-statuskod: 200

hello svaret innehåller en post per rekommenderade objekt. Varje post innehåller hello följande data:

* `Feed\entry\content\properties\Id`-Rekommenderade objekt-ID.
* `Feed\entry\content\properties\Name`-Namnet på hello-objektet.
* `Feed\entry\content\properties\Rating`-Klassificering av hello rekommendation. högre nummer innebär högre tillförlitlighet.
* `Feed\entry\content\properties\Reasoning`-Rekommendation skäl till (t.ex. rekommendation förklaringar).

Se ett exempel svar i 12.1

## <a name="13-user-usage-history"></a>13. Historik för användning av användaren
När du väl har skapat en rekommendation modell kommer hello att tooretrieve hello användarhistorik (objekt associerade tooa särskilda användare) används för hello version.
Detta API Tillåt tooretrieve hello användarens historik

Obs: hello användarhistorik är endast tillgänglig för rekommendation versioner.

### <a name="131-retrieve-user-history"></a>13,1 hämta användarens historik
Hämta hello lista över objekt som används i hello active skapa eller skapa för hello angivna användar-id i hello som angetts.

| HTTP-metoden | URI: N |
|:--- |:--- |
| HÄMTA |Hämta hello användarhistorik för hello active version.<br/>`<rootURI>/GetUserHistory?modelId=%27<model_id>%27&userId=%27<userId>%27&apiVersion=%271.0%27`<br/><br/>Hämta hello användarhistorik för hello angivna build`<rootURI>/GetUserHistory?modelId=%27<model_id>%27&userId=%27<userId>%27&buildId=<int>&apiVersion=%271.0%27`<br/><br/>Exempel:`<rootURI>/GetUserHistory?modelId=%2727967136e8-f868-4258-9331-10d567f87fae%27&&userId=%27u_1013%27&apiVersion=%271.0%277` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |hello Unik identifierare för hello modellen. |
| Användar-ID |hello Unik identifierare för hello användare. |
| buildId |valfri parameter, Tillåt tooindicate från vilken build hello användarhistorik ska hämta |
| apiVersion |1.0 |

**Svar:**

HTTP-statuskod: 200

hello svaret innehåller en post per rekommenderade objekt. Varje post innehåller hello följande data:

* `Feed\entry\content\properties\Id`-Rekommenderade objekt-ID.
* `Feed\entry\content\properties\Name`-Namnet på hello-objektet.
* `Feed\entry\content\properties\Rating`-SAKNAS.
* `Feed\entry\content\properties\Reasoning`-SAKNAS.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get User History</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2015-05-26T15:32:47Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">CatalogItemsThatContainATokenEntity</title>
        <updated>2015-05-26T15:32:47Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">2406E770-769C-4189-89DE-1C9283F93A96</d:Id>
                <d:Name m:type="Edm.String">Clara Callan</d:Name>
                <d:Rating m:type="Edm.Double">0</d:Rating>
                <d:Reasoning m:type="Edm.String"/>
                <d:Metadata m:type="Edm.String"/>
                <d:FormattedRating m:type="Edm.Double" m:null="true"/>
            </m:properties>
        </content>
    </entry>
</feed>

## <a name="14-notifications"></a>14. Meddelanden
Azure Machine Learning rekommendationer skapar meddelanden när permanenta fel uppstår i hello system. Det finns 3 typer av aviseringar:

1. Build-fel - det här meddelandet utlöses för varje build-fel.
2. Datainsamling bearbetningsfel - denna avisering utlöses när vi har fler än 100 fel i hello senaste 5 minuterna i hello bearbetningen av Användningshändelser per modell.
3. Rekommendation förbrukning fel - det här meddelandet utlöses när vi har fler än 100 fel i hello senaste 5 minuterna i hello bearbetningen av rekommendation förfrågningar per modell.

### <a name="141-get-notifications"></a>14.1. Få meddelanden
Hämtar alla hello-meddelanden för alla modeller eller för en modell.

| HTTP-metoden | URI: N |
|:--- |:--- |
| HÄMTA |`<rootURI>/GetNotifications?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Hämta alla meddelanden för alla modeller:<br>`<rootURI>/GetNotifications?apiVersion=%271.0%27`<br><br>Exempel för att hämta meddelanden för en viss modell:<br>`<rootURI>/GetNotifications?modelId=%27967136e8-f868-4258-9331-10d567f87fae%27&apiVersion=%271.0%277` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |Valfri parameter. Om detta utelämnas används visas alla meddelanden för alla modeller. <br>Giltigt värde: Unik identifierare för hello modellen. |
| apiVersion |1.0 |
|  | |
| Begärandetext |INGEN |

**Svar:**

HTTP-statuskod: 200

OData-XML

    hello response includes one entry per notification. Each entry has hello following data:
        * feed\entry\content\properties\UserName - Internal user name identification.
        * feed\entry\content\properties\ModelId - Model ID.
        * feed\entry\content\properties\Message - Notification message.
        * feed\entry\content\properties\DateCreated - Date that this notification was created in UTC format.
        * feed\entry\content\properties\NotificationType - Notification types. Values are BuildFailure, RecommendationFailure, and DataAquisitionFailure.

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get a list of Notifications for a user</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-11-04T13:24:23Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'" />
        <entry>
                <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetAListOfNotificationsForAUserEntity</title>
            <updated>2014-11-04T13:24:23Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:UserName m:type="Edm.String">515506bc-3693-4dce-a5e2-81cb3e8efb56@dm.com</d:UserName>
                    <d:ModelId m:type="Edm.String">967136e8-f868-4258-9331-10d567f87fae</d:ModelId>
                    <d:Message m:type="Edm.String">Build failed for user</d:Message>
                    <d:DateCreated m:type="Edm.String">2014-11-04T13:23:14.383</d:DateCreated>
                    <d:NotificationType m:type="Edm.String">BuildFailure</d:NotificationType>
                </m:properties>
            </content>
        </entry>
    </feed>

### <a name="142-delete-model-notifications"></a>14.2. Ta bort modellen meddelanden
Tar bort alla skrivskyddade meddelanden för en modell.

| HTTP-metoden | URI: N |
|:--- |:--- |
| TA BORT |`<rootURI>/DeleteModelNotifications?modelId=%<model_id>%27&apiVersion=%271.0%27`<br><br>Exempel:<br>`<rootURI>/DeleteModelNotifications?modelId=%27967136e8-f868-4258-9331-10d567f87fae%27&apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| %{ModelID/ |Unik identifierare för hello modellen |
| apiVersion |1.0 |
|  | |
| Begärandetext |INGEN |

**Svaret**:

HTTP-statuskod: 200

### <a name="143-delete-user-notifications"></a>14.3. Ta bort användarmeddelanden
Tar bort alla meddelanden för alla modeller.

| HTTP-metoden | URI: N |
|:--- |:--- |
| TA BORT |`<rootURI>/DeleteUserNotifications?apiVersion=%271.0%27` |

| Parameternamn | Giltiga värden |
|:--- |:--- |
| apiVersion |1.0 |
|  | |
| Begärandetext |INGEN |

**Svaret**:

HTTP-statuskod: 200

## <a name="15-legal"></a>15. Juridisk information
Detta dokument tillhandahålls ”som-är”. Information och åsikter som uttrycks i detta dokument, inklusive Webbadresser och andra webbplatsreferenser, kan ändras utan föregående meddelande.<br><br>
Vissa exempel som beskrivs häri tillhandahålls enbart som illustration och är fiktiva. Ingen verklig associering eller koppling är avsedd eller underförstådd.<br><br>
Det här dokumentet ger dig inga juridiska rättigheter tooany immateriell egendom i någon Microsoft-produkt. Du får kopiera och använda det här dokumentet som intern referens.<br><br>
© 2015 Microsoft. Med ensamrätt.


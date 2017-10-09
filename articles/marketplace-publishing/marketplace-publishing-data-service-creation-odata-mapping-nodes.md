---
title: "aaaGuide toocreating en datatjänst för hello Marketplace | Microsoft Docs"
description: "Detaljerade anvisningar hur toocreate, certifiera och distribuera en Data-tjänst för att köpa på hello Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 107baab2-5828-4158-abdf-59a580c80d37
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: e3d88412389d43d104662dc4434363b6ad9475f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-hello-nodes-schema-for-mapping-an-existing-web-service-tooodata-through-csdl"></a>Förstå hello noder schemat för mappning av en befintlig web service tooOData via CSDL
> [!IMPORTANT]
> **Just nu vi inte längre onboarding några nya Data Service-utgivare. Nya dataservices kommer inte godkännas för lista.** Om du har ett SaaS-affärsprogram som toopublish på AppSource hittar du mer information [här](https://appsource.microsoft.com/partners). Om du har en IaaS-program eller utvecklare tjänst du skulle t.ex toopublish på Azure Marketplace hittar du mer information [här](https://azure.microsoft.com/marketplace/programs/certified/).
>
>

Det här dokumentet hjälper dig att klargöra hello nod strukturen för mappning av en OData-protokollet tooCSDL. Det är viktigt toonote att hello nod struktur är korrekt formaterad XML. Så gäller rot, överordnade och underordnade schema när du utformar din OData-mappning.

## <a name="ignored-elements"></a>Ignoreras element
hello följande är hello hög nivå CSDL element (XML-noder) som inte kommer toobe som används av hello Azure Marketplace backend under hello import av metadata för hello web service. De kan vara närvarande men kommer att ignoreras.

| Element | Omfång |
| --- | --- |
| Med Element |hello-nod, sub noder och alla attribut |
| Documentation-elementet |hello-nod, sub noder och alla attribut |
| ComplexType |hello-nod, sub noder och alla attribut |
| Kopplingen Element |hello-nod, sub noder och alla attribut |
| En Extended-egenskap |hello-nod, sub noder och alla attribut |
| EntityContainer |Endast hello följande attribut ignoreras: *utökar* och *AssociationSet* |
| Schemat |Endast hello följande attribut ignoreras: *Namespace* |
| FunctionImport |Endast hello följande attribut ignoreras: *läge* (standardvärdet för ln förutsätts) |
| EntityType |Endast hello följande sub noder ignoreras: *nyckeln* och *PropertyRef* |

hello nedan beskrivs hello ändringar (lägga till och ignoreras element) toohello olika CSDL XML-noder i detalj.

## <a name="functionimport-node"></a>FunctionImport nod
En FunctionImport-nod representerar en URL (startpunkt) som Exponerar en service toohello slutanvändaren. hello noden kan som beskriver hur hello URL riktar, vilka parametrar som är tillgängliga toohello slutanvändare och hur dessa parametrar har angetts.

Information om den här noden finns på [här][MSDNFunctionImportLink](https://msdn.microsoft.com/library/cc716710.aspx)

hello följande är ytterligare attribut som hello (eller tillägg tooattributes) som exponeras av hello FunctionImport nod:

**d:BaseUri** -hello URI-mall för hello REST-resurs som är exponerade tooMarketplace. Marketplace använder hello mallen tooconstruct frågor mot hello REST-webbtjänst. hello URI-mall innehåller platshållare för hello parametrar i hello form av {parameterName}, där parameterName är hello hello parameterns namn. T.ex. apiVersion = {apiVersion}.
Parametrar tillåts tooappear som URI-parametrar eller som en del av hello URI-sökväg. Hello gäller hello utseende i hello sökväg är de alltid obligatoriska (kan inte markeras som kan ha värdet null). *Exempel:*`d:BaseUri="http://api.MyWeb.com/Site/{url}/v1/visits?start={start}&amp;end={end}&amp;ApiKey=3fadcaa&amp;Format=XML"`

**Namnet** -hello namnet på hello importeras funktion.  Det går inte att vara hello samma som andra definierade namn i hello CSDL.  T.ex. Name = ”GetModelUsageFile”

**EntitySet** *(valfritt)* - om hello funktionen returnerar en mängd entitetstyper, hello värdet för hello **EntitySet** måste vara hello entitet set toowhich hello samling tillhör. Annars hello **EntitySet** attributet får inte användas. *Exempel:*`EntitySet="GetUsageStatisticsEntitySet"`

**ReturnType** *(valfritt)* -anger hello typ av element som returneras av hello URI.  Använd inte det här attributet om hello funktionen inte returnerar ett värde. hello följande finns hello stöds typer:

* **Samling (<Entity type name>)**: Anger en uppsättning definierade entitetstyper. hello namn finns i hello namnattributet för hello EntityType nod. Ett exempel är en samling (WXC. HourlyResult).
* **Rådata (<mime type>)**: Anger en rå dokumentet/blob som returneras toohello användare. Ett exempel är Raw(image/jpeg) andra exempel:

  * ReturnType="Raw(text/plain)”
  * ReturnType = ”samling (sage. DeleteAllUsageFilesEntity) ”*

**d:paging** – anger hur sidindelning hanteras av hello REST-resurs. Hej parametern värden används i typografiska braches, t.ex. sidan = {$page} & itemsperpage = {$size} hello tillgängliga alternativ är:

* **Ingen:** inga växling är tillgänglig
* **Hoppa över:** sidindelning uttrycks via en logisk ”hoppa över” och ”ta” (överst). Hoppa över hwila M element och ta sedan returnerar hello nästa N-element. Värdet för parametern: $skip
* **Vidta:** vidta returnerar hello nästa N element. Värdet för parametern: $take
* **PageSize:** sidindelning anges med en logisk sida och storlek (objekt per sida). Sidan representerar hello sidan som returneras. Värdet för parametern: $page
* **Storlek:** storlek representerar hello antalet objekt som returneras för varje sida. Värdet för parametern: $size

**d:AllowedHttpMethods** *(valfritt)* – anger vilka verb hanteras av hello REST-resurs. Dessutom begränsar godkända verb toohello anges värdet.  Standard = POST.  *Exempel:* `d:AllowedHttpMethods="GET"` hello alternativen är:

* **GET:** oftast används tooreturn data
* **POST:** oftast används tooinsert nya data
* **PLACERA:** oftast används tooupdate data
* **Ta bort:** används toodelete data

Ytterligare underordnade noder (som inte omfattas av hello CSDL dokumentation) i hello FunctionImport nod är:

**d:RequestBody** *(valfritt)* -hello begäran meddelandetexten är används tooindicate som hello begäran förväntar sig en brödtext toobe skickas. Parametrar kan anges i begärandetexten för hello. De uttrycks inom klammerparenteser, t.ex. {parameterName}. Dessa parametrar mappas från hello användarindata till hello brödtext som överförs toohello innehållsleverantören service. Hej requestBody element har ett attribut med namnet httpMethod. hello attributet tillåter två värden:

* **POST:** används om hello-begäran är en HTTP POST
* **GET:** används om hello-begäran är en HTTP GET

    Exempel:

        `<d:RequestBody d:httpMethod="POST">
        <![CDATA[
        <req1:Request xmlns:r1="http://schemas.mysite.com//generic/requests/1" Version="1.0">
        <req1:Query>{Query}</req1:Query><req1:AppId>D453474</req1:AppId>
        <req:DestinationSchemas><req1:Schema>Generic.RequestResponse[1.0]</req1:Schema>
        </req1:DestinationSchemas>
        </req1: Request>
        ]]>
        </d:RequestBody>`

**d:Namespaces** och **d:Namespace** -beskriver hello namnområden som definierats i hello XML som returneras av funktionsimporten hello (URI slutpunkt) för den här noden. hello XML som returneras av hello serverdelstjänst kan innehålla valfritt antal namnområden toodifferentiate hello innehåll som returneras. **Alla dessa namnområden om används i d:Map eller d:Match XPath-frågor måste toobe visas.** Hej d:Namespaces noden innehåller en uppsättning/lista över d:Namespace noder. Var och en av dem innehåller ett namnområde som används i hello backend svaret. hello följande är hello-attributet för hello d:Namespace nod:

* **d:prefix:** hello prefix för hello namnområdet som visas i hello XML-resultat från hello-tjänsten, t.ex. f:FirstName, f:LastName, där f är hello prefix.
* **d:URI:** hello fullständig hello namnområdet som används i hello resultatet dokument-URI. Hello värdet representerar det hello-prefixet är löst tooat runtime.

**d:ErrorHandling** *(valfritt)* -den här noden innehåller villkor för felhantering. Hello villkor verifieras mot hello resultatet som returneras av hello innehållsleverantören service. Om ett villkor matchar hello föreslagna HTTP-felkod returneras ett felmeddelande toohello slutanvändaren.

**d:ErrorHandling** *(valfritt)* och **d:Condition** *(valfritt)* -ett villkor nod innehåller ett villkor som är markerad i hello resultatet som returneras av hello innehållsleverantören-tjänsten. hello följande är hello **krävs** attribut:

* **d:match:** ett XPath-uttryck som kontrollerar om en viss nod/värde finns i hello innehållsleverantören utdata från XML. hello XPath körs mot hello utdata och ska returnera true om hello villkoret är en match eller false annars.
* **d:HttpStatusCode:** hello HTTP-statuskod som ska returneras av Marketplace i hello case hello villkor matchar. Marketplace signalizes fel toohello användare via HTTP-statuskoder. En lista över HTTP-statuskoder finns på http://en.wikipedia.org/wiki/HTTP_status_code
* **d:errorMessage:** hello felmeddelande returnerade – med hello HTTP-statuskoden – toohello slutanvändaren. Detta bör vara ett eget felmeddelande som inte innehåller några hemligheter.

**d:Title** *(valfritt)* -tillåter som beskriver hello titeln på hello-funktionen. hello-värdet för hello kommer från

* hello valfria kartan attribut (en xpath) som anger om toofind hello rubrik hello svar returneras från hello tjänstbegäran.
* - Eller - hello titel anges som värde för hello-nod.

**d:Rights** *(valfritt)* -hello rättigheter (t.ex. copyright) som är associerade med hello-funktionen. hello-värde för hello rättigheter kommer från:

* hello valfria kartan attribut (en xpath) som anger om toofind hello rättigheter hello svar returneras från hello tjänstbegäran.
* - Eller - hello rättigheter som anges som värde för hello-nod.

**d:Description** *(valfritt)* – en kort beskrivning för hello-funktionen. hello värdet hello beskrivning kommer från

* hello valfria kartan attribut (en xpath) som anger om toofind hello beskrivning hello svar returneras från hello tjänstbegäran.
* - Eller – hello beskrivning som angetts som värde för hello-nod.

**d:EmitSelfLink** - *finns ovanför exempel ”FunctionImport för sidindelning om du via returnerade data”*

**d:EncodeParameterValue** -valfritt tillägg tooOData

**d:QueryResourceCost** -valfritt tillägg tooOData

**d:Map** -valfritt tillägg tooOData

**d:headers** -valfritt tillägg tooOData

**d:headers** -valfritt tillägg tooOData

**d:value** -valfritt tillägg tooOData

**d:HttpStatusCode** -valfritt tillägg tooOData

**d:errorMessage** -valfritt tillägg tooOData

## <a name="parameter-node"></a>Parametern nod
Den här noden representerar en parameter som visas som en del av URI-mall för hello / begärandetext som har angetts i hello FunctionImport nod.

En användbar information dokumentsidan om hello ”parameterelement” nod påträffades vid [här](http://msdn.microsoft.com/library/ee473431.aspx) (Använd hello **andra Version** listrutan tooselect en annan version om nödvändigt tooview hello dokumentationen). *Exempel:*`<Parameter Name="Query" Nullable="false" Mode="In" Type="String" d:Description="Query" d:SampleValues="Rudy Duck" d:EncodeParameterValue="true" MaxLength="255" FixedLength="false" Unicode="false" annotation:StoreGeneratedPattern="Identity"/>`

| Parameterattributet | Krävs | Värde |
| --- | --- | --- |
| Namn |Ja |hello namnet på hello-parameter. Skiftlägeskänsliga!  Gemener/versaler hello BaseUri. **Exempel:**`<Property Name="IsDormant" Type="Byte" />` |
| Typ |Ja |hello parametertyp. hello-värdet måste vara ett **EDMSimpleType** eller en komplex typ som är inom hello omfång för hello modellen. Mer information finns i ”6 stöds Parameter/egenskap types”.  (Skiftlägeskänslig! Första tecken är versaler, resten är gemen.)  Också se [konceptuella modellen typer (CSDL)][MSDNParameterLink](http://msdn.microsoft.com/library/bb399548.aspx). **Exempel:**`<Property Name="LimitedPartnershipID " Type="Int32" />` |
| Läge |Nej |**I**, Out eller InOut beroende på om hello-parametern är ett indata eller utdata i/o-parametern. (Endast ”i” är tillgängliga i Azure Marketplace.) **Exempel:**`<Parameter Name="StudentID" Mode="In" Type="Int32" />` |
| maxLength |Nej |hello tillåtna längden för hello-parametern. **Exempel:**`<Property Name="URI" Type="String" MaxLength="100" FixedLength="false" Unicode="false" />` |
| Precision |Nej |hello precision för hello-parametern. **Exempel:**`<Property Name="PreviousDate" Type="DateTime" Precision="0" />` |
| Skala |Nej |hello skala för hello-parametern. **Exempel:**`<Property Name="SICCode" Type="Decimal" Precision="10" Scale="0" />` |

hello följande är hello-attribut som har lagts till toohello CSDL-specifikationen:

| Parameterattributet | Beskrivning |
| --- | --- |
| **d:Regex** *(valfritt)* |Regex-uttrycket används toovalidate hello indatavärdet för hello-parametern. Om hello indatavärdet inte matchar hello instruktionen hello värdet avvisas. Detta gör att toospecify också en uppsättning värden, t.ex. ^ [0-9] +? $ tooonly Tillåt siffror. **Exempel:** ' < parameternamn = ”name” läge = ”i” Type = ”sträng” d: kan ha värdet null = ”false” d:Regex = ”^ [a-öA-Ö] * $” d:Description = ”ett namn som inte får innehålla blanksteg eller icke-engelska tecken icke-alfanumeriska” d:SampleValues = ”George |
| **d:enum** *(valfritt)* |En pipe avgränsade lista med värden som är giltiga för hello-parametern. hello typ av värden som hello måste toomatch hello definierats typ av hello-parametern. Exempel: ' engelska |
| **d: kan ha värdet null** *(valfritt)* |Kan du definiera om en parameter kan vara null. hello standardvärdet är: SANT. Dock får inte parametrar som visas som en del av sökvägen hello i hello URI-mall vara null. När hello är attributet toofalse för dessa parametrar – åsidosätts hello indata från användaren. **Exempel:**`<Parameter Name="BikeType" Type="String" Mode="In" Nullable="false"/>` |
| **d:SampleValue** *(valfritt)* |Ett exempel värdet toodisplay som en anteckning toohello klienten i hello Användargränssnittet.  Det är möjligt tooadd flera värden med hjälp av en pipe avgränsade listan, dvs. ' en |

## <a name="entitytype-node"></a>EntityType-nod
Den här noden representerar hello-typer som returneras från Marketplace toohello slutanvändaren. Den innehåller också hello mappning från hello-utdata som returneras av hello innehållsleverantören-toohello värden som returneras toohello slutanvändaren.

Information om den här noden finns på [här](http://msdn.microsoft.com/library/bb399206.aspx) (Använd hello **andra Version** listrutan tooselect en annan version om nödvändigt tooview hello dokumentation.)

| Attributets namn | Krävs | Värde |
| --- | --- | --- |
| Namn |Ja |hello namnet på hello entitetstypen. **Exempel:**`<EntityType Name="ListOfAllEntities" d:Map="//EntityModel">` |
| BaseType |Nej |hello namnet på en annan entitetstypen som hello bastypen för hello entitetstypen som definieras. **Exempel:**`<EntityType Name="PhoneRecord" BaseType="dqs:RequestRecord">` |

hello följande är hello-attribut som har lagts till toohello CSDL-specifikationen:

**d:Map** -ett XPath-uttryck som körs mot hello service utdata. hello här antas att hello service utdata innehåller en uppsättning element som upprepas, som en ATOM-feed där det finns en uppsättning post noder som upprepas. Var och en av dessa upprepande noder innehåller en post. hello XPath är angivna toopoint hello enskilda upprepande noden i hello innehållsleverantören service resultat som innehåller hello värden för en enskild post. Exempel utdata från hello-tjänsten:

        `<foo>
          <bar> … content … </bar>
          <bar> … content … </bar>
          <bar> … content … </bar>
        </foo>`

hello XPath-uttryck är /foo/bar eftersom varje hello fältet nod är hello upprepande nod i hello utdata och den innehåller hello själva innehållet som returneras toohello slutanvändaren.

**Nyckeln** – det här attributet ignoreras av Marketplace. REST-baserat webbtjänsterna, exponera i allmänhet inte en primärnyckel.

## <a name="property-node"></a>Egenskapsnod
Den här noden innehåller en egenskap av hello-post.

Information om den här noden finns på [http://msdn.microsoft.com/library/bb399546.aspx](http://msdn.microsoft.com/library/bb399546.aspx) (Använd hello **andra Version** listrutan tooselect en annan version om nödvändigt tooview hello dokumentation.) *Exempel:*`<EntityType Name="MetaDataEntityType" d:Map="/MyXMLPath">
        <Property Name="Name"     Type="String" Nullable="true" d:Map="./Service/Name" d:IsPrimaryKey="true" DefaultValue=”Joe Doh” MaxLength="25" FixedLength="true" />
        ...
        </EntityType>`

| AttributeName | Krävs | Värde |
| --- | --- | --- |
| Namn |Ja |hello namnet på hello-egenskap. |
| Typ |Ja |hello typ av hello egenskapsvärde. värdet för hello egenskapstypen måste vara en **EDMSimpleType** eller en komplex typ (identifieras med ett fullständigt kvalificerat namn) som ligger inom omfånget för hello modellen. Mer information finns i konceptuella modellen typer (CSDL). |
| Kan ha värdet null |Nej |**SANT** (hello standardvärdet) eller **FALSKT** beroende på om hello-egenskapen kan ha värdet null. Obs: Hello version av CSDL visas med hello [http://schemas.microsoft.com/ado/2006/04/edm](http://schemas.microsoft.com/ado/2006/04/edm) namnområde, en komplex typ-egenskap måste ha Nullable = ”False”. |
| Standardvärde |Nej |hello standardvärdet för hello-egenskap. |
| maxLength |Nej |hello maximal längd på hello egenskapsvärde. |
| Multisträng med fast |Nej |**SANT** eller **FALSKT** beroende på om hello egenskapsvärdet lagras som en sträng med längden fiexed. |
| Precision |Nej |Refererar toohello maximalt antal siffror tooretain i hello numeriskt värde. |
| Skala |Nej |Maximalt antal decimaler tooretain i hello numeriskt värde. |
| Unicode |Nej |**SANT** eller **FALSKT** beroende på om hello egenskapens värde vara lagras som en Unicode-sträng. |
| Sortering |Nej |En sträng som anger hello sortera sekvens toobe används i hello-datakälla. |
| ConcurrencyMode |Nej |**Ingen** (hello standardvärdet) eller **fast**. Om hello värde har angetts för**fast**, hello egenskapens värde som ska användas i Optimistisk samtidighet kontroller. |

hello följande är ytterligare hello-attribut som har lagts till toohello CSDL-specifikationen:

**d:Map** -XPath-uttryck som körs mot hello service utdata och extraherar en egenskap av hello utdata. hello XPath som angetts är relativ toohello upprepad nod som har valts i hello EntityType nod XPath. Det är också möjligt toospecify som ett absolut XPath tooallow inklusive en statisk resurs i varje hello utdata noder, som till exempel en upphovsrätt som endast finns i hello ursprungliga service utdata, men ska finnas i varje hello rader i hello OData utdata. Exempel från hello-tjänsten:

        `<foo>
          <bar>
           <baz0>… value …</baz0>
           <baz1>… value …</baz1>
           <baz2>… value …</baz2>
          </bar>
        </foo>`

här hello XPath-uttryck är ./bar/baz0 tooget hello baz0 noden från hello innehållsleverantören-tjänsten.

**d:CharMaxLength** -för strängtyp, kan du ange hello maxlängden. Se DataService CSDL-exempel

**d:IsPrimaryKey** -anger om hello kolumnen är hello primärnyckel i hello tabellen/vyn. Se DataService CSDL exempel.

**d:isExposed** -anger om hello tabellschemat exponeras (vanligtvis true). Se DataService CSDL-exempel

**d:IsView** *(valfritt)* – SANT om detta baseras på en vy i stället för en tabell.  Se DataService CSDL-exempel

**d:Tableschema** -Se DataService CSDL-exempel

**d:ColumnName** -hello namn för hello kolumnen i hello tabellen/vyn.  Se DataService CSDL-exempel

**d:IsReturned** -är hello booleskt värde som anger om hello Service exponerar värdet toohello klienten.  Se DataService CSDL-exempel

**d:IsQueryable** -är hello booleskt värde som anger om hello kolumnen kan användas i en databasfråga.   Se DataService CSDL-exempel

**d:OrdinalPosition** -är hello kolumnen numeriska position utseende, x, i hello tabell eller vy för hello, där x är från 1 toohello antalet kolumner i tabellen hello.  Se DataService CSDL-exempel

**d:DatabaseDataType** -hello datatyp för kolumnen hello i hello databas, d.v.s. SQL-datatypen. Se DataService CSDL-exempel

## <a name="supported-parametersproperty-types"></a>Typer stöds parametrar-egenskap
hello följande är hello stöds typer för parametrar och egenskaper. (Skiftlägeskänslig)

| Primitiva typer | Beskrivning |
| --- | --- |
| Null |Representerar hello avsaknaden av ett värde |
| Booleskt värde |Representerar hello matematiska begreppet binary-valued logik |
| Mottagna byte |Värdet 8-bitars heltal utan tecken |
| Datum och tid |Representerar datum och tid med värden mellan 12:00:00 midnatt 1 januari, 1753 e. kr. via 11:59:59 P.M, December 9999 e. kr. |
| Decimal |Representerar numeriska värden med fast precision och skala. Den här typen kan beskriva ett numeriskt värde mellan negativa 10 ^ 255 + 1 toopositive 10 ^ 255 -1 |
| dubbla |Representerar ett tal med 15 decimaler precision som kan representera värden med ungefärliga intervall på + 2, 23E-308 via + 1, 79E flyttalsvärden +308. **Använd Decimal på grund av tooExel export problemet** |
| Enskild |Representerar en flytande nummer med 7 siffror precision som kan representera värden med ungefärliga intervall på + 1, 18E-38 via + 3, 40E + 38 |
| GUID |Representerar ett värde för unik identifierare för 16 byte (128-bitars) |
| Int16 |Representerar ett signerat 16-bitars heltal |
| Int32 |Representerar ett signerat 32-bitars heltal |
| Int64 |Representerar ett signerat 64-bitars heltal |
| Sträng |Representerar fixed - eller variabel längd teckendata |

## <a name="see-also"></a>Se även
* Om du vill förstå hello övergripande processen för OData-mappning och ändamål, den här artikeln [mappning för OData-tjänsten](marketplace-publishing-data-service-creation-odata-mapping.md) tooreview definitioner, strukturer och instruktioner.
* Om du vill granska exempel den här artikeln [mappa Data Service OData-exempel](marketplace-publishing-data-service-creation-odata-mapping-examples.md) toosee exempelkoden och förstå kodsyntax och kontext.
* tooreturn toohello föreskrivs sökväg för att publicera en datatjänst toohello Azure Marketplace som den här artikeln [Data Service publiceringsguide](marketplace-publishing-data-service-creation.md).

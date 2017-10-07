---
title: "aaaGuide toocreating en datatjänst för hello Marketplace | Microsoft Docs"
description: "Detaljerade anvisningar hur toocreate, certifiera och distribuera en Data-tjänst för att köpa på hello Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 3a632825-db5b-49ec-98bd-887138798bc4
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: deb2e52dd03f5beb2ad6a927bd2d03e47d20b691
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="mapping-an-existing-web-service-tooodata-through-csdl"></a>Mappning av en befintlig web service tooOData via CSDL
> [!IMPORTANT]
> **Just nu vi inte längre onboarding några nya Data Service-utgivare. Nya dataservices kommer inte godkännas för lista.** Om du har ett SaaS-affärsprogram som toopublish på AppSource hittar du mer information [här](https://appsource.microsoft.com/partners). Om du har en IaaS-program eller utvecklare tjänst du skulle t.ex toopublish på Azure Marketplace hittar du mer information [här](https://azure.microsoft.com/marketplace/programs/certified/).
> 
> 

Den här artikeln ger en översikt över om hur toouse CSDL-toomap en befintlig service tooan kompatibel OData-tjänst. Den förklarar hur toocreate hello mappning dokumentet (CSDL) som omvandlar hello inkommande begäran från hello klient via en samtalet och hello utdata (data) tillbaka toohello klienten via en kompatibel OData-feed. Microsoft Azure Marketplace visar services toohello slutanvändare med hjälp av hello OData-protokollet. Tjänster som exponeras av innehållsleverantörer (dataägare) visas i flera olika former REST, SOAP, t.ex.

## <a name="what-is-a-csdl-and-its-structure"></a>Vad är en CSDL och dess struktur?
En CSDL (konceptuellt Schema Definition Language) är en specifikation som definierar hur toodescribe webbtjänst eller databas tjänsten gemensamma XML ordflöde toohello Azure Marketplace.

Enkel översikt över hello **flödet för begäran:**

  `Client -> Azure Marketplace -> Content Provider’s Web Service (Get, Post, Delete, Put)`

Hej **dataflöde** i hello motsatt riktning:

  `Client <- Azure Marketplace <- Content Provider’s WebService`

**Bild 1** diagram en klient skulle hur hämta data från en innehållsleverantör (din tjänst) genom att gå via hello Azure Marketplace.  Hej CSDL används av hello mappning-transformering komponenten toohandle hello begäran och data som skickas mellan hello innehållsleverantören tjänster och hello begär klienten.

*Bild 1: Detaljerad flödar från begär klienten toocontent provider via Azure Marketplace*

  ![Rita](media/marketplace-publishing-data-service-creation-odata-mapping/figure-1.png)

Bakgrundsinformation om Atom och Atom Pub hello OData-protokollet vid vilka hello Azure Marketplace tillägg build granska: [http://msdn.microsoft.com/library/ff478141.aspx](http://msdn.microsoft.com/library/ff478141.aspx)

Utdrag ovan länk: *”hello hello Open Data protocol (som anges nedan tooas OData) syftar tooprovide en REST-baserat protokoll för CRUD-format åtgärder (skapa, läsa, uppdatera och ta bort) mot resurser visas som data services. En ”datatjänst” är en slutpunkt har angett namn / värde-par där det finns data exponeras från en eller flera ”samlingar” varje med noll eller flera ”poster”, som består av. OData har publicerats av Microsoft under OASIS (organisation för hello framflyttning för strukturerade Information Standards) standarder så att vem som helst som vill toocan skapa servrar, klienter eller verktyg utan royalty eller begränsningar ”.*

### <a name="three-critical-pieces-that-have-toobe-defined-by-hello-csdl-are"></a>Tre viktiga uppgifter som har toobe som definieras av hello CSDL är:
* Hej **endpoint** hello Web adress (URI) av hello Service av hello-leverantör
* Hej **dataparametrar** som skickas som indata toohello tjänstleverantör hello definitioner av hello parametrar skickas toohello innehållsleverantören service ned toohello datatyp.
* **Schemat** hello data som returneras toohello begär Service hello schemat för hello som levereras av hello innehållsleverantören tjänsten, inklusive behållare, samlingar-tabeller, variabler eller kolumner och datatyper.

hello följande diagram visar en översikt över hello flödet, från där hello-klienten anger hello OData-uttryck (anropet toohello Innehållsleverantörens webbtjänst) toogetting hello/Resultatdata tillbaka.

  ![Rita](media/marketplace-publishing-data-service-creation-odata-mapping/figure-2.png)

### <a name="steps"></a>Så här:
1. Klienten skickar begäran via tjänsten anropet slutfördes med indataparametrar som definierats i XML-toohello Azure Marketplace
2. CSDL är används toovalidate hello anropet till tjänsten.
   * hello skickas formaterad Service anropa sedan toohello innehåll Providers Service av hello Azure Marketplace
3. hello Webservice körs och preforms hello åtgärden hello Http-Verb (d.v.s. får) hello data returneras tooAzure Marketplace där hello begärde data (eventuella) är visar i XML-Format toohello klienten med hjälp av hello mappning som definierats i hello CSDL.
4. hello klienten skickas hello data (om sådan finns) i XML- eller JSON-format

## <a name="definitions"></a>Definitioner
### <a name="odata-atom-pub"></a>OData-ATOM pub
Ange ett tillägg toohello ATOM pub där varje post representerar en rad med ett resultat. hello innehåll en del av hello transaktionen är förbättrad toocontain hello värden för hello rad – som nyckel/värde-par. Mer information finns här: [https://www.odata.org/documentation/odata-version-3-0/atom-format/](https://www.odata.org/documentation/odata-version-3-0/atom-format/)

### <a name="csdl---conceptual-schema-definition-language"></a>CSDL - Conceptual Schema Definition Language
Här kan du definiera funktioner (SPROCs) och de entiteter som är tillgängliga via en databas. Mer information finns här: [http://msdn.microsoft.com/library/bb399292.aspx](http://msdn.microsoft.com/library/bb399292.aspx)  

> [!TIP]
> Klicka på hello **andra versioner** listrutan och väljer en version om du inte ser hello artikel.
> 
> 

### <a name="edm---entry-data-model"></a>EDM - post-datamodell
* Översikt: [http://msdn.microsoft.com/library/vstudio/ee382825 (v=vs.100).aspx][OverviewLink]

[OverviewLink]:http://msdn.microsoft.com/library/vstudio/ee382825(v=vs.100).aspx
* Förhandsversion: [http://msdn.microsoft.com/library/aa697428 (v=vs.80).aspx][PreviewLink]

[PreviewLink]:http://msdn.microsoft.com/library/aa697428(v=vs.80).aspx
* Datatyper: [http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx][DataTypesLink]

[DataTypesLink]:http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx

hello följande visar hello detaljerad vänstra tooRight flödar från där hello-klienten anger hello OData-uttryck (anropet toohello Innehållsleverantörens webbtjänst) toogetting hello/Resultatdata tillbaka:

  ![Rita](media/marketplace-publishing-data-service-creation-odata-mapping/figure-3.png)

## <a name="csdl-basics"></a>CSDL-grunderna
En CSDL (konceptuellt Schema Definition Language) är en specifikation som definierar hur toodescribe webbtjänst eller databas tjänsten gemensamma XML ordflöde toohello Azure Marketplace. CSDL beskriver hello kritiska delar som **gör det möjligt hello överföring av data från hello datakällan toohello Azure Marketplace.** Här beskrivs hello viktigaste delarna:

* Gränssnittsinformation som beskriver alla offentligt tillgängliga funktioner (FunctionImport nod)
* Information om datatyp för alla requests(input) meddelandet och meddelandet responses(outputs) (EntityType-EntityContainer/EntitySet noder)
* Bindningsinformation om hello transport protocol toobe används (sidhuvud nod)
* Information om för att hitta hello anges service (BaseURI attribut)

I kort sagt kan representerar hello CSDL ett kontrakt och språk-plattformsoberoende mellan hello service begärande och hello-leverantör. Med hello CSDL kan en klient Leta upp en webbtjänst för service-databasen och anropa en av dess offentligt tillgängliga funktioner.

### <a name="relating-a-csdl-tooa-database-or-a-collection"></a>Om en CSDL-tooa databas eller en samling
**Hej CSDL-specifikationen**

CSDL är XML-grammatiken för att beskriva en webbtjänst. hello-specifikationen själva är uppdelad i 4 viktiga element: EntitySet, FunctionImport; Namnområdet och EntityType.

toomake detta enklare toounderstand abstraction kan relatera en CSDL tooa tabell.

Kom ihåg;

  CSDL representerar en plattform och språkoberoende avtal mellan hello **service begärande** och hello **tjänstleverantör**. Med hjälp av CSDL, en **klienten** kan hitta en **webbtjänsten tjänstdatabasen/** och anropa en av dess offentligt tillgängliga **funktioner.**

För en datatjänst hello kan fyra delar av en CSDL betraktas som en databas, tabeller, kolumner och Store-procedur.

Om dessa på följande sätt för en datatjänst:

* EntityContainer ~ = databas
* EntitySet ~ = tabell
* EntityType ~ = kolumner
* FunctionImport ~ = lagrad procedur

**HTTP-verb tillåts**

* Hämta – returnerar värden från hello db (returnerar en mängd)
* POST – används toopass data tooand valfria returvärden från hello db (skapa en ny post i hello samling, return-id eller-URI)
* Ta bort – tar bort data från hello DB (tar bort en samling)
* PLACERA – uppdatera data i en DB (ersätta en samling eller skapa ett)

## <a name="metadatamapping-document"></a>Metadata-mappning dokumentet
hello metadata-mappning dokumentet är används toomap en innehållsleverantör befintliga webbtjänster så att den kan visas som en OData-webbtjänst hello Azure Marketplace-systemet. Den är baserad på CSDL och implementerar några tillägg tooCSDL tooaccommodate hello behov REST baserat webbtjänsterna som exponeras via Azure Marketplace. hello-tillägg finns i hello [http://schemas.microsoft.com/dallas/2010/04](http://schemas.microsoft.com/dallas/2010/04) namnområde.

Ett exempel på hello CSDL visas nedan: (kopiera och klistra in hello nedan exempel CSDL till en XML-redigerare och ändra toomatch din tjänst.  Klistra in i CSDL mappning DataService fliken när du skapar din tjänst i hello [Azure Marketplace Publishing Portal](https://publish.windowsazure.com)).

**Villkor:** hänför sig hello CSDL villkor toohello [Publiceringsportal](https://publish.windowsazure.com) Användargränssnittet (PPUI) villkor.

* Erbjuder ”Title” i hello PPUI relaterar tooMyWebOffer
* Mitt företag i hello PPUI gäller för**Publisher visningsnamn** i hello [Microsoft Developer Center](http://dev.windows.com/registration?accountprogram=azure) UI
* Din API relaterad tooa webb- eller Data-tjänsten (en Plan i hello PPUI)

**Hierarki:** ett företag (innehållsleverantören) äger erbjudanden som har plan eller de planer, nämligen service(s) vilken rad upp med ett API.

### <a name="webservice-csdl-example"></a>Webbtjänsten CSDL-exempel
Ansluter tooa tjänst som visar en web application slutpunkt (t.ex. en C#-program)

        <?xml version="1.0" encoding="utf-8"?>
        <!-- hello namespace attribute below is used by our system toogenerate C#. You can change “MyCompany.MyOffer” toosomething that makes sense for you, but change “MyOffer” consistently throughout hello document. -->
        <Schema Namespace="MyCompany.MyWebOffer" Alias="MyOffer" xmlns="http://schemas.microsoft.com/ado/2009/08/edm" xmlns:d="http://schemas.microsoft.com/dallas/2010/04" >
        <!-- EntityContainer groups all hello web service calls together into a single offering. Every web service call has a FunctionImport definition. -->
          <EntityContainer Name="MyOffer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
        @Name is used as reference by FunctionImport @EntitySet. And is used in hello customer facing UI as name of hello Service.
        @EntityType is used toopoint at hello type definition near hello bottom of this file. -->
            <EntitySet Name="MyEntities" EntityType="MyOffer.MyEntityType" />
        <!-- Add a FunctionImport for every service method. Multiple FunctionImports can share a single return type (EntityType). -->
        <!-- ReturnType is either Raw() for a stream or Collection() for an Atom feed. Ex. of Raw: ReturnType=”Raw(text/plain)” -->
        <!—EntitySet is hello entityset defined above, and is needed if ReturnType is not Raw -->
        <!-- BaseURI attribute defines hello service call, replace & with hello encode value (&amp;).
        In hello input name value pairs {param} represents passed in value.
        Or hello value can be hard coded as with AccountKey. -->
        <!-- AllowedHttpMethods optional (default = “GET”), allows hello CSDL toospecifically specify hello verb of hello service, “Get”, “Post”, “Put”, or “Delete”. -->
        <!-- EncodeParameterValues, True encodes hello parameter values, false does not. -->
        <!-- BaseURI is translated into an URITemplate which defines how hello web service call is exposed toomarketplace customers.
        Ex. https://api.datamarket.azure.com/mycompany/MyOfferPlan?name={name}
        BaseURI is XML encoded, hello {...} point toohello parameters defined below.
        Marketplace will read hello parameters from this URITemplate and fill hello values into hello corresponding parameters of hello BaseUri or RequestBody (below) during calls tooyour service.  
        It is okay for @d:BaseUri tooinclude information only for Marketplace consumption, it will not be exposed tooend users. i.e. hello hardcoded AccountKey in hello above BaseURI does not show up in hello client facing URITemplate. -->
            <FunctionImport Name="MyWebServiceMethod"
                            EntitySet="MyEntities"
                            ReturnType="Collection(MyOffer.MyEntityType)"
        d:AllowedHttpMethods="GET"
        d:EncodeParameterValues="true"
        d:BaseUri="http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <!-- Definition of hello RequestBody is only required for HTTP POST requests and is optional for HTTP GET requests. -->
        <d:RequestBody d:httpMethod="POST">
                <!-- Use {} for placeholders tooinsert parameters. -->
                <!-- This example uses SOAP formatting, but any POST body can be used. -->
            <!-- This example shows how toopass userid and password via hello header -->
                <![CDATA[<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:MyOffer="http://services.organization.net/MyServicePath">
                  <soapenv:Header/>
                  <soapenv:Body>
                    <MyOffer:ws_MyWebServiceMethod>
                      <myWebServiceMethodRequest>
                        <!--This information is not exposed tooend users. -->
                        <UserId>userid</UserId>
                        <Password>password</Password>
                        <!-- {name} is replaced with hello value read from @d:UriTemplate above -->
                        <Name>{name}</Name>
                        <!-- Parameters can be used more than once and are not limited tooappearing as hello value of an element -->
                        <CustomField Name="{name}" />
                        <MyField>Static content</MyField>
                      </myWebServiceMethodRequest>
                    </MyOffer:ws_MyWebServiceMethod>
                  </soapenv:Body>
                </soapenv:Envelope>      
              ]]>
        </d:RequestBody>
        <!-- Title, Rights and Description are optional and used toospecify values tooinsert into hello ATOM feed returned toohello end user.  You can specify hello element toocontain a fixed message by providing a value for hello element (this is hello default value).  @d:Map is an XPath expression that points into hello response returned by your service and is optional.  -->
        <d:Title d:Map="/MyResponse/Title">Default title.</d:Title>
        <d:Rights>© My copyright. This is a fixed response. It is okay tooalso add a d:Map attribute toooverride this text.</d:Rights>
        <d:Description d:Map="/MyResponse/Description"></d:Description>
        <d:Namespaces>
        <d:Namespace d:Prefix="p"  d:Uri="http://schemas.organization.net/2010/04/myNamespace" />
        <d:Namespace d:Prefix="p2" d:Uri="http://schemas.organization.net/2010/04/MyNamespace2" />
        </d:Namespaces>
        <!-- Parameters of hello web service call:
        @Name should match exactly (case sensitive) hello {…} placeholders in hello @d:BaseUri, @d:UriTemplate, and d:RequestBody, i.e. “name” parameter in above BaseURI.
        @Mode is always "In", compatibility with CSDL
        @Type is hello EDM.SimpleType of hello parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx
        @d:Nullable indicates whether hello parameter is required.
        @d:Regex - optional, attribute toodescribe hello string, limiting unwanted input at hello entry of hello system
        @d:Description - optional, is used by Service Explorer as help information
        @d:SampleValues - optional, is used by Service Explorer as help information. Multiple Sample values are separated by '|', e.g. "804735132|234534224|23409823234"
        @d:Enum - optional for string type. Contains an enumeration of possible values separated by a '|', e.g. d:enum="english|metric|raw". Will be converted in a dropdown list in hello Service Explorer.
        -->
        <Parameter name="name" Mode="In" Type="String" d:Nullable="false" d:Regex="^[a-zA-Z]*$" d:Description="A name that cannot contain any spaces or non-alpha non-English characters"
        d:Enum="George|John|Thomas|James"
        d:SampleValues="George"/>
        <Parameter Name=" AccountKey" Mode="In" Type="String" d:Nullable="false" />

        <!-- d:ErrorHandling is an optional element. Use it define standardized errors by evaluating hello service response. -->
        <d:ErrorHandling>
        <!-- Any number of d:Condition elements are allowed, they are evaluated in hello order listed.
        @d:Match is an Xpath query on hello service response, it should return true or false where true indicates an error.
        @d:httpStatusCode is hello error code tooreturn if an response matches hello error.
        @d:errorMessage is hello user friendly message tooreturn when an error occurs.
        -->
        <d:Condition d:Match="/Result/ErrorMessage[text()='Invalid token']" d:HttpStatusCode="403" d:ErrorMessage="User cannot connect toohello service." />
        </d:ErrorHandling>
           </FunctionImport>

            <!-- hello EntityContainer defines hello output data schema -->
        </EntityContainer>
        <!-- hello EntityType @d:Map defines hello repeating node (an XPath query) in hello response (output data schema). -->
        <!-- If these nodes are outside a namespace, add hello prefix in hello xpath. -->
        <!--
        @Name - define your user readable name, will become an XML element in hello ATOM feed, so comply with hello XML element naming restrictions (no spaces or other illegal characters).
        @Type is hello EDM.SimpleType of hello parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx.
        @d:Map uses an Xpath query toopoint at hello location tooextract hello content from your services response.
        hello "." is relative toohello repeating node in hello EntityType @d:Map Xpath expression.
        -->
            <EntityType Name="MyEntityType" d:Map="/MyResponse/MyEntities">
        <Property Name="ID"    d:IsPrimaryKey="True" Type="Int32"    Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="Amount"    Type="Double"    Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="City"    Type="String"    Nullable="false" d:Map="./City"/>
        <Property Name="State"    Type="String"    Nullable="false" d:Map="./State"/>
        <Property Name="Zip"    Type="Int32"    Nullable="false" d:Map="./Zip"/>
        <Property Name="Updated"    Type="DateTime"    Nullable="false" d:Map="./Updated"/>
        <Property Name="AdditionalInfo" Type="String" Nullable="true"
        d:Map="./Info/More[1]"/>
            </EntityType>
        </Schema>

> [!TIP]
> Visa fler CSDL Web Service-exempel i hello artikel [exempel för att matcha en befintlig web service tooOData via CSDLs](marketplace-publishing-data-service-creation-odata-mapping-examples.md)
> 
> 

### <a name="dataservice-csdl-example"></a>DataService CSDL-exempel
Ansluter tooa tjänst som Exponerar en databastabell eller vy som en slutpunkt som exemplet nedan visar två API: er för databas baserat API CSDL (kan använda vyer i stället för tabeller).

        <?xml version="1.0"?>
        <!-- hello namespace attribute below is used by our system toogenerate C#. You can change “MyCompany.MyOffer” toosomething that makes sense for you, but change “MyOffer” consistently throughout hello document. -->
        <Schema Namespace="MyCompany.MyDataOffer" Alias="MyOffer" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ado/2009/08/edm">
        <!-- EntityContainer groups all hello data service calls together into a single offering. Every web service call has a FunctionImport definition. -->
        <EntityContainer Name="MyOfferContainer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
            Think of hello EntitySet as a Service
        @Name is used in hello customer facing UI as name of hello Service.
        @EntityType is used toopoint at hello type definition (returned set of table columns). -->
        <EntitySet Name="CompanyInfoEntitySet" EntityType="MyOffer.CompanyInfo" />
        <EntitySet Name="ProductInfoEntitySet" EntityType="MyOffer.ProductInfo" />
        </EntityContainer>
        <!-- EntityType defines result (output); hello table (or view) and columns toobe returned by hello data service.)
            Map is hello schema.tabel or schema.view
            dals.TableName is hello table Name
            Name is hello name identifier for hello EntityType and hello Name of hello service exposed toohello client via hello UI.
            dals:IsExposed determines if hello table schema is exposed (generally true).
            dals:IsView (optional) true if this is based on a view rather than a table
            dals:TableSchema is hello schema name of hello table/view
        -->
        <EntityType
        Map="[dbo].[CompanyInfo]"
        dals:TableName="CompanyInfo"
        Name="CompanyInfo"
        dals:IsExposed="true"
        dals:IsView="false"
        dals:TableSchema="dbo"
        xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <!-- Property defines hello column properties and hello output of hello service.
            dals:ColumnName is hello name of hello column in hello table /view.
            Type is hello emd.SimpleType
            Nullable determines if NULL is a valid output value
            dals.CharMaxLenght is hello maximum length of hello output value
            Name is hello name of hello Property and is exposed toohello client facing UI
            dals:IsReturned is hello Boolean that determines if hello Service exposes this value toohello client.
            IsQueryable is hello Boolean that determines if hello column can be used in a database query
            (For data Services: tooimprove Performance make sure that columns marked ISQueryable=”true” are in an index.)
            dals:OrdinalPosition is hello numerical position x in hello table or hello View, where x is from 1 toohello number of columns in hello table.
            dals:DatabaseDataType is hello data type of hello column in hello database, i.e. SQL data type dals:IsPrimaryKey indicates if hello column is hello Primary key in hello table/view.  (hello columns marked ISPrimaryKey are used in hello Order by clause when returning data.)
        -->
        <Property dals:ColumnName="data" Type="String" Nullable="true" dals:CharMaxLength="-1" Name="data" dals:IsReturned="true" dals:IsQueryable="false" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="ticker" Type="String" Nullable="true" dals:CharMaxLength="10" Name="ticker" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        <EntityType Map="[dbo].[ProductInfo]" dals:TableName="ProductInfo" Name="ProductInfo" dals:IsExposed="true" dals:IsView="false" dals:TableSchema="dbo" xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <Property dals:ColumnName="companyid" Type="Int32" Nullable="true" Name="companyid" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="product" Type="String" Nullable="true" dals:CharMaxLength="50" Name="product" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        </Schema>

## <a name="see-also"></a>Se även
* Om du är intresserad av utbildning och förstå hello specifika noder och deras parametrar kan den här artikeln [mappa Data Service OData-noder](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) definitioner och förklaringar, exempel och Använd case kontext.
* Om du vill granska exempel den här artikeln [mappa Data Service OData-exempel](marketplace-publishing-data-service-creation-odata-mapping-examples.md) toosee exempelkoden och förstå kodsyntax och kontext.
* tooreturn toohello föreskrivs sökväg för att publicera en datatjänst toohello Azure Marketplace som den här artikeln [Data Service publiceringsguide](marketplace-publishing-data-service-creation.md).


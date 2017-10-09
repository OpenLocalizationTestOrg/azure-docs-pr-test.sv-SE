---
title: "aaaUsing signaturer för delad åtkomst (SAS) i Azure Storage | Microsoft Docs"
description: "Lär dig toouse delad åtkomst signaturer (SAS) toodelegate åtkomst tooAzure lagringsresurser, inklusive blobbar, köer, tabeller och filer."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 46fd99d7-36b3-4283-81e3-f214b29f1152
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/18/2017
ms.author: marsma
ms.openlocfilehash: 5b75a3c25bcfb9f1ceb81f31dc2d42376bd105cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-shared-access-signatures-sas"></a>Använda signaturer för delad åtkomst (SAS)

En signatur för delad åtkomst (SAS) ger dig ett sätt toogrant begränsad åtkomst tooobjects i ditt lagringskonto tooother klienter utan att utsätta din kontonyckel. I den här artikeln vi ger en översikt över hello SAS-modellen, metodtips för SAS och titta på några exempel.

För ytterligare kodexempel med hjälp av SAS utöver de som presenteras här, se [komma igång med Azure Blob Storage i .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/) och andra hello-exempel [Azure kodexempel](https://azure.microsoft.com/documentation/samples/?service=storage) bibliotek. Du kan hämta hello exempelprogram och köra dem eller bläddra hello koden på GitHub.

## <a name="what-is-a-shared-access-signature"></a>Vad är en signatur för delad åtkomst?
En signatur för delad åtkomst ger delegerad åtkomst tooresources i ditt lagringskonto. Du kan ge klienter åtkomst till tooresources i ditt lagringskonto, utan att dela dina nycklar med en SAS. Detta är hello nyckel använder signaturer för delad åtkomst i dina program – en SAS är ett säkert sätt tooshare dina lagringsresurser utan att kompromissa med dina nycklar för kontot.

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

En SAS ger detaljerad kontroll över hello typ av åtkomst du bevilja tooclients som har hello SAS, inklusive:

* hello intervall över vilka hello SAS är giltiga, inklusive hello starttid och hello förfallotiden.
* hello behörigheterna av hello SAS. Till exempel kan en SAS för en blob bevilja läsbehörighet och skriva behörigheter toothat blob, men inte att ta bort behörigheter.
* En valfri IP-adress eller intervall av IP-adresser som Azure Storage godkänner hello SAS. Du kan till exempel ange ett intervall IP-adresser som tillhör tooyour organisation.
* hello protokoll som du ska ta emot hello SAS Azure Storage. Du kan använda den här valfria parametern toorestrict åtkomst tooclients med hjälp av HTTPS.

## <a name="when-should-you-use-a-shared-access-signature"></a>När bör du använda en signatur för delad åtkomst?
Du kan använda en SAS när du vill tooprovide åtkomst tooresources i storage-konto tooany klienten inte har åtkomstnycklarna för ditt lagringskonto. Ditt lagringskonto innehåller både en primär och sekundär snabbtangent, som båda bevilja administratörsbehörighet tooyour konto och alla resurser i den. Ditt konto toohello möjligheten att skadlig eller bristande öppnas exponering av någon av dessa nycklar. Signaturer för delad åtkomst är en säker alternativ som tillåter klienter tooread, skriva och ta bort data i ditt lagringskonto enligt toohello behörigheter som du uttryckligen har beviljat utan behovet av att någon kontonyckel.

Ett vanligt scenario där en SAS är användbar är en tjänst där användare läsa och skriva sina egna data tooyour storage-konto. Det finns två vanliga designmönster i ett scenario där ett lagringskonto lagrar användardata:

1. Klienter ladda upp och hämta data via en frontend-proxytjänst som utför autentisering. Den här tjänsten frontend proxy har hello fördelen att det tillåter validering av affärsregler, men för stora mängder data eller omfattande transaktioner, skapa en tjänst som kan skalas toomatch begäran kan vara dyra eller svårt.

  ![Scenariot diagram: frontend-proxytjänst](./media/storage-dotnet-shared-access-signature-part-1/sas-storage-fe-proxy-service.png)   

1. En förenklad tjänst autentiserar hello klienten efter behov och genererar en SAS. När hello klienten tar emot hello SAS kan de komma åt kontot lagringsresurser direkt med hello behörigheter har definierats av hello SAS och för hello-intervall som tillåts av hello SAS. hello SAS minskar hello behovet för omdirigering av alla data via hello frontend proxy-tjänst.

  ![Scenariot diagram: SAS-providertjänst](./media/storage-dotnet-shared-access-signature-part-1/sas-storage-provider-service.png)   

Många verkliga tjänster kan använda en kombination av dessa två sätt. Vissa data kan till exempel bearbetas och godkänts via hello frontend proxy, medan andra data är sparade och/eller läsa med hjälp av SAS.

Dessutom behöver du toouse ett SAS tooauthenticate hello källobjekt i en kopieringsåtgärd i vissa scenarier:

* När du kopierar en blob tooanother blob som finns i ett annat lagringskonto, måste du använda en SAS tooauthenticate hello käll-blob. Du kan eventuellt använda en SAS tooauthenticate hello mål-blob samt.
* När du kopierar en fil tooanother som finns i ett annat lagringskonto, måste du använda en SAS tooauthenticate hello källfil. Du kan eventuellt använda en SAS tooauthenticate hello målfil samt.
* När du kopierar en tooa blob-fil eller en fil tooa blob, måste du använda en SAS tooauthenticate hello källobjektet, även om hello käll- och objekt som finns inom hello samma lagringskonto.

## <a name="types-of-shared-access-signatures"></a>Typer av signaturer för delad åtkomst
Du kan skapa två typer av signaturer för delad åtkomst:

* **Tjänst-SAS.** hello service SAS delegater åtkomst tooa resursen i en av hello lagringstjänster: hello Blob, kön, tabell eller fil-tjänsten. Se [hur du skapar en tjänst-SAS](https://msdn.microsoft.com/library/dn140255.aspx) och [Service SAS exempel](https://msdn.microsoft.com/library/dn140256.aspx) för detaljerad information om hur du skapar hello service SAS-token.
* **Konto-SAS.** hello konto SAS delegater komma åt tooresources i en eller flera hello lagringstjänster. Alla hello-åtgärder som är tillgängliga via en tjänst-SAS finns också tillgängliga via en konto-SAS. Dessutom med hello konto SAS, du kan delegera åtkomst toooperations som gäller tooa beroende tjänst, t.ex **Get/Set-tjänstegenskaper** och **få statistik för tjänsten**. Du kan också delegera åtkomst tooread-, Skriv- och delete-åtgärder på blob-behållare, tabeller, köer och filresurser som inte tillåts med en tjänst-SAS. Se [hur du skapar ett konto-SAS](https://msdn.microsoft.com/library/mt584140.aspx) för detaljerad information om hur du skapar hello konto SAS-token.

## <a name="how-a-shared-access-signature-works"></a>Så här fungerar en signatur för delad åtkomst
En signatur för delad åtkomst är en signerad URI pekar tooone eller mer lagringsresurser som innehåller en token som innehåller en särskild uppsättning Frågeparametrar. hello token anger hur hello resurser kan användas av hello-klienten. En hello frågeparametrar Hej signatur, är konstrueras utifrån hello SAS parametrar och signerad med hello kontonyckel. Den här signaturen används av Azure Storage tooauthenticate hello SAS.

Här är ett exempel på en SAS-URI, som visar hello resurs-URI och hello SAS-token:

![Komponenter för en SAS-URI](./media/storage-dotnet-shared-access-signature-part-1/sas-storage-uri.png)   

hello SAS-token är en sträng som du skapar på hello *klienten* sida (se hello [SAS exempel](#sas-examples) avsnittet kodexempel). En SAS-token som du skapar med hello storage-klientbibliotek, till exempel spåras inte av Azure Storage på något sätt. Du kan skapa ett obegränsat antal SAS-token på hello på klientsidan.

När en klient innehåller SAS-URI-tooAzure lagring som en del av en begäran, kontrollerar hello-tjänsten hello SAS parametrar och signatur tooverify om den är giltig för att autentisera hello-begäran. Om hello tjänsten kontrollerar hello signaturen är giltig, autentisera hello-begäran. Annars har hello begäran nekats med felkoden 403 (förbjuden).

## <a name="shared-access-signature-parameters"></a>Parametrar för signatur för delad åtkomst
hello konto-SAS service SAS-token innehåller några vanliga parametrar och också tar några parametrar som är olika.

### <a name="parameters-common-tooaccount-sas-and-service-sas-tokens"></a>Parametrarna vanliga tooaccount SAS och tjänsten SAS-token
* **API-versionen** en valfri parameter som anger hello lagring version toouse tooexecute hello tjänstbegäran.
* **Service-version** en obligatorisk parameter som anger hello lagring version toouse tooauthenticate hello tjänstbegäran.
* **Starttid.** Detta är hello tid vid vilken hello SAS börjar gälla. hello starttiden för en signatur för delad åtkomst är valfritt. Om en starttid utelämnas är hello SAS gälla omedelbart. hello starttid måste anges i UTC (Coordinated Universal Time) med en särskild UTC beteckning (”Z”), till exempel `1994-11-05T13:15:30Z`.
* **Förfallotiden.** Detta är Hej då efter vilken hello SAS är inte längre giltig. Bästa praxis rekommenderar att du antingen ange en förfallotiden för en SAS eller koppla den till en princip för lagrade åtkomst. hello förfallotiden måste anges i UTC (Coordinated Universal Time) med en särskild UTC beteckning (”Z”), till exempel `1994-11-05T13:15:30Z` (se mer nedan).
* **Behörigheter.** hello behörigheter anges på hello SAS visar vilka åtgärder som hello klienten kan utföra mot hello storage-resursen med hjälp av hello SAS. Tillgängliga behörigheter skiljer sig för en konto-SAS och en tjänst-SAS.
* **IP-ADRESSEN.** En valfri parameter som anger en IP-adress eller ett intervall med IP-adresser utanför Azure (hello i avsnittet [konfiguration av Routning sessionstillstånd](../../expressroute/expressroute-workflows.md#routing-session-configuration-state) för Express Route) från vilka tooaccept begäranden.
* **Protokoll.** En valfri parameter som anger hello protokoll tillåts för en begäran. Möjliga värden är HTTPS- och HTTP (`https,http`), vilket är standardvärdet för hello eller HTTPS endast (`https`). Observera att HTTP endast inte är tillåtna värdet.
* **Signatur.** hello signatur konstrueras utifrån hello andra parametrar som angetts som en del token och sedan krypteras. Den används tooauthenticate hello SAS.

### <a name="parameters-for-a-service-sas-token"></a>Parametrar för en tjänst-SAS-token
* **Storage-resursen.** Storage-resurser som du kan delegera åtkomst med en tjänst SAS inkluderar:
  * Behållare och blobbar
  * Filresurser och filer
  * Köer
  * Tabeller och intervallen för tabellentiteter.

### <a name="parameters-for-an-account-sas-token"></a>Parametrar för ett konto SAS-token
* **Tjänsten eller tjänster.** En konto-SAS kan delegera åtkomst tooone eller flera av hello lagringstjänster. Du kan till exempel skapa ett konto SAS som ombud för dataåtkomsttjänsten toohello Blob och fil. Eller så kan du skapa en SAS att delegater åtkomst till tooall fyra tjänster (Blob, kön, tabell och filen).
* **Typer av lagring.** En konto-SAS gäller tooone eller flera klasser av lagringsresurser, i stället för en viss resurs. Du kan skapa ett konto SAS toodelegate åtkomst till:
  * Servicenivå API: er, som kallas mot hello lagringsresurs för kontot. Exempel inkluderar **Get/Set-tjänstegenskaper**, **få statistik för tjänsten**, och **lista behållare/köer/tabeller/resurser**.
  * Behållare på objektnivå API: er, som kallas mot hello behållarobjekt för varje tjänst: blob-behållare, köer, tabeller och filresurser. Exempel inkluderar **skapa/ta bort behållaren**, **skapa/ta bort kön**, **skapa/ta bort tabellen**, **skapa/ta bort resursen**, och  **Lista över BLOB-filer och kataloger**.
  * På objektnivå API: er, som kallas mot BLOB, Kömeddelanden, tabellentiteter och filer. Till exempel **placera Blob**, **frågan entiteten**, **få meddelanden**, och **Create File**.

## <a name="examples-of-sas-uris"></a>Exempel på SAS URI: er

### <a name="service-sas-uri-example"></a>Tjänsten SAS-URI-exempel

Här är ett exempel på en SAS-URI som innehåller läsa och skriva behörigheter tooa blob-tjänst. hello tabell uppdelad varje del av hello URI toounderstand hur det bidrar toohello SAS:

```
https://myaccount.blob.core.windows.net/sascontainer/sasblob.txt?sv=2015-04-05&st=2015-04-29T22%3A18%3A26Z&se=2015-04-30T02%3A23%3A26Z&sr=b&sp=rw&sip=168.1.5.60-168.1.5.70&spr=https&sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D
```

| Namn | SAS-delen | Beskrivning |
| --- | --- | --- |
| BLOB-URI |`https://myaccount.blob.core.windows.net/sascontainer/sasblob.txt` |hello adress hello-blob. Observera att med hjälp av HTTPS rekommenderas starkt. |
| Storage services version |`sv=2015-04-05` |För lagringstjänster version 12-02-2012 och senare kan den här parametern anger hello version toouse. |
| Starttid |`st=2015-04-29T22%3A18%3A26Z` |Angivet i UTC-tid. Om du vill hello SAS toobe giltig omedelbart utelämna hello starttid. |
| Förfallotiden |`se=2015-04-30T02%3A23%3A26Z` |Angivet i UTC-tid. |
| Resurs |`sr=b` |hello resursen är en blob. |
| Behörigheter |`sp=rw` |hello behörigheterna av hello SAS omfattar Read (r) och skriva (b). |
| IP-intervall |`sip=168.1.5.60-168.1.5.70` |hello intervall av IP-adresser som en begäran kommer att accepteras. |
| Protokoll |`spr=https` |Endast de begäranden som använder HTTPS tillåts. |
| Signatur |`sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D` |Använda tooauthenticate åtkomst toohello blob. hello signaturen är en HMAC beräknas över ett sträng-inloggning och nyckeln med hjälp av hello SHA256-algoritmen och sedan kodad med Base64-kodning. |

### <a name="account-sas-uri-example"></a>Kontot SAS-URI-exempel

Här är ett exempel på en konto-SAS att hello använder samma gemensamma parametrar för hello-token. Eftersom dessa parametrar beskrivs ovan, beskrivs de inte här. Hello-parametrar som är specifika tooaccount SAS beskrivs i hello nedan.

```
https://myaccount.blob.core.windows.net/?restype=service&comp=properties&sv=2015-04-05&ss=bf&srt=s&st=2015-04-29T22%3A18%3A26Z&se=2015-04-30T02%3A23%3A26Z&sr=b&sp=rw&sip=168.1.5.60-168.1.5.70&spr=https&sig=F%6GRVAZ5Cdj2Pw4tgU7IlSTkWgn7bUkkAg8P6HESXwmf%4B
```

| Namn | SAS-delen | Beskrivning |
| --- | --- | --- |
| Resurs-URI |`https://myaccount.blob.core.windows.net/?restype=service&comp=properties` |hello Blob-tjänstens slutpunkt, med parametrar för att hämta tjänstegenskaper (när den anropas med GET) eller ange tjänstegenskaper (när den anropas med). |
| Tjänster |`ss=bf` |hello SAS gäller toohello Blob- och Filtjänster |
| Typer av resurser |`srt=s` |hello SAS gäller tooservice nivå åtgärder. |
| Behörigheter |`sp=rw` |hello behörigheter bevilja åtkomst tooread- och skrivåtgärder. |

Med hänsyn till att behörigheterna är begränsad toohello servicenivå, tillgängliga åtgärder med den här SAS är **hämta egenskaper för Blob-tjänsten** (läsa) och **ange egenskaper för Blob-tjänsten** (Skriv). Med en annan resurs URI hello samma SAS-token kan dock också vara används toodelegate åtkomst för**få statistik för Blob-tjänsten** (läsa).

## <a name="controlling-a-sas-with-a-stored-access-policy"></a>Kontrollera en SAS med en princip lagrade åtkomst
En signatur för delad åtkomst kan göra något av två typer:

* **Ad hoc-SAS:** när du skapar en ad hoc-SAS, hello starttid, förfallotiden och behörigheter för hello SAS anges i hello SAS-URI (eller underförstådda, i hello fall där starttiden utelämnas). Den här typen av SAS kan skapas som ett konto SAS eller en tjänst-SAS.
* **SAS med lagrade åtkomstprincip:** lagrade åtkomstprincip har definierats för en resurs behållare – en blob-behållare, tabell, kö, eller filresurs-- och kan vara används toomanage begränsningarna för en eller flera signaturer för delad åtkomst. När du kopplar en SAS med en lagrad till ärver hello SAS hello begränsningar--hello starttid och förfallotiden behörigheter--har definierats för hello lagras åtkomstprincip.

> [!NOTE]
> För närvarande måste en konto-SAS vara en ad hoc-SAS. Lagrade åtkomst principer ännu inte stöds för konto-SAS.

hello skillnaden mellan hello två formulär är viktigt för ett key-scenario: återkallade certifikat. Eftersom en SAS-URI är en URL, alla som erhåller hello SAS kan använda det, oavsett vem som skapade den. Om en SAS publiceras offentligt, kan den användas av alla i hälsningsmeddelande. En SAS beviljar åtkomst tooresources tooanyone innehar den tills något av följande händer:

1. hello förfallotiden anges på hello SAS har uppnåtts.
2. hello förfallotiden har angetts på hello lagras åtkomstprincip som refereras av hello SAS har uppnåtts (om en lagrad åtkomstprincip refereras, och det anger en förfallotiden). Detta kan inträffa eftersom hello intervall långa eller eftersom du har ändrat hello lagras åtkomstprincip med en förfallotiden i hello tidigare, vilket är ett sätt toorevoke hello SAS.
3. hello lagras åtkomstprincip som refereras av hello SAS tas bort, vilket är ett annat sätt toorevoke hello SAS. Observera att om du återskapa hello lagras åtkomstprincip med exakt hello samma namn, alla befintliga SAS-token kommer igen giltig bl.a toohello behörigheter associeras med den lagrade åtkomstprincip (förutsatt att hello förfallotiden på hello SAS inte har passerats). Om du har för avsikt toorevoke hello SAS, vara säker på att toouse ett annat namn om du återskapa hello åtkomstprincip med en förfallotiden i hello framtida.
4. Hej kontonyckel som har använt toocreate hello SAS genereras. Återskapande av någon kontonyckel gör att alla programkomponenter med hjälp av den viktiga toofail tooauthenticate förrän de har uppdaterats toouse antingen hello andra giltig kontonyckel eller hello nyligen återskapats kontonyckel.

> [!IMPORTANT]
> En signatur för delad åtkomst URI är associerad med hello konto nyckel används toocreate hello signatur och hello associerad lagrade åtkomstprincip (eventuella). Om inga lagrade åtkomstprincip anges är hello endast sätt toorevoke en signatur för delad åtkomst toochange hello kontonyckel.

## <a name="authenticating-from-a-client-application-with-a-sas"></a>Autentisering från ett klientprogram med en SAS
En klient som innehar en SAS kan använda hello SAS tooauthenticate en begäran mot ett lagringskonto som de inte har hello nycklar. En SAS kan ingå i en anslutningssträng eller användas direkt från hello lämplig konstruktor eller metod.

### <a name="using-a-sas-in-a-connection-string"></a>Med hjälp av en SAS i en anslutningssträng
[!INCLUDE [storage-use-sas-in-connection-string-include](../../../includes/storage-use-sas-in-connection-string-include.md)]

### <a name="using-a-sas-in-a-constructor-or-method"></a>Med hjälp av en SAS i en konstruktor eller metod
Flera Azure Storage client biblioteket konstruktorer och metoden överlagringar erbjuder en SAS-parameter, så att du kan autentisera en begäran toohello tjänst med en SAS.

Till exempel är här SAS-URI används toocreate en referens tooa block-blob. hello SAS innehåller hello endast autentiseringsuppgifter som krävs för hello-begäran. Hej blockblobben används sedan för en skrivåtgärd:

```csharp
string sasUri = "https://storagesample.blob.core.windows.net/sample-container/" +
    "sampleBlob.txt?sv=2015-07-08&sr=b&sig=39Up9JzHkxhUIhFEjEH9594DJxe7w6cIRCg0V6lCGSo%3D" +
    "&se=2016-10-18T21%3A51%3A37Z&sp=rcw";

CloudBlockBlob blob = new CloudBlockBlob(new Uri(sasUri));

// Create operation: Upload a blob with hello specified name toohello container.
// If hello blob does not exist, it will be created. If it does exist, it will be overwritten.
try
{
    MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
    msWrite.Position = 0;
    using (msWrite)
    {
        await blob.UploadFromStreamAsync(msWrite);
    }

    Console.WriteLine("Create operation succeeded for SAS {0}", sasUri);
    Console.WriteLine();
}
catch (StorageException e)
{
    if (e.RequestInformation.HttpStatusCode == 403)
    {
        Console.WriteLine("Create operation failed for SAS {0}", sasUri);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }
    else
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}

```

## <a name="best-practices-when-using-sas"></a>Bästa metoder när du med hjälp av SAS
När du använder signaturer för delad åtkomst i dina program, måste du toobe medveten om två potentiella risker:

* Om en SAS har läckts kan den användas av alla som erhåller, vilket potentiellt kan påverka ditt lagringskonto.
* Om en SAS tooa klientprogrammet upphör att gälla och hello programmet är tooretrieve en ny SAS från din tjänst kan hello programfunktionen hindras.

hello kan följande rekommendationer för att använda signaturer för delad åtkomst minska riskerna:

1. **Använder alltid HTTPS** toocreate eller distribuera en SAS. Om en SAS skickas via HTTP och snappas upp, en obehörig som utför en man-in-the-middle-attack kan tooread hello SAS och sedan använda den precis som hello avsedd användare kan ha kompromettera potentiellt känsliga data eller tillåta för skadade data av hello skadliga användare.
2. **Referens lagrad åtkomstprinciper där det är möjligt.** Lagrade åtkomst principer ger du hello alternativet toorevoke behörigheter utan tooregenerate hello lagringskontonycklar. Ställa in ett utgångsdatum för hello på dessa mycket långt i hello framtida (eller oändlig) och kontrollera att den regelbundet har uppdaterats toomove längre till hello framtida.
3. **Använd kortsiktig giltighetstid gånger på en ad hoc-SAS.** På så sätt kan även om en SAS äventyras, är det bara giltig för en kort tid. Den här övningen är särskilt viktigt om du inte kan referera till en princip för lagrade åtkomst. Kortsiktig giltighetstid gånger också begränsa hello mängden data som kan skrivas tooa blob genom att begränsa hello tid tillgänglig tooupload tooit.
4. **Ha klienter automatiskt förnya hello SAS om det behövs.** Klienter måste förnya hello SAS innan hello förfallodatum, i ordning tooallow tid för återförsök om hello-tjänst som tillhandahåller hello SAS är inte tillgänglig. Om din SAS är avsedd för ett litet antal omedelbara toobe, tillfällig åtgärder som är förväntat toobe slutföras inom hello förfalloperiod och detta kan vara onödiga hello SAS inte är korrekt toobe förnyas. Men om du har klienten som regelbundet göra begäranden via SAS kommer sedan hello möjligheten att upphör att gälla in i play. hello nyckelfaktor är toobalance hello behovet för hello SAS toobe tillfällig (som tidigare nämnts anger) med hello måste tooensure som hello klienten begär förnyelse tidigt tillräckligt med (tooavoid avbrott på grund av toohello SAS utgående tidigare toosuccessful förnyelse).
5. **Var försiktig med SAS starttiden.** Om du anger hello starttiden för en SAS för**nu**, och klicka sedan på grund av tooclock förskjutning (skillnader i aktuell tid enligt toodifferent datorer) fel kan observeras periodvis för hello första några minuter. I allmänhet ange hello start tid toobe minst 15 minuter i hello tidigare. Eller inte anger den, vilket gör det giltiga omedelbart i samtliga fall. hello samma gäller vanligtvis tooexpiry tid samt--Kom ihåg att du kan se in too15 minuter klockan skeva i vardera riktning på varje begäran. För klienter som använder en REST version tidigare too2012-02-12, är hello maximal varaktighet för en Signatur som inte refererar till en lagrad åtkomstprincip 1 timme och policyer för att ange längre sikt än vad som kommer att misslyckas.
6. **Måste vara specifikt med hello resurs toobe nås.** Av säkerhetsskäl är tooprovide en användare med hello lägsta behörighet som krävs. Om en användare behöver bara läsbehörighet tooa enhet, ge dem läsbehörighet toothat enda entitet och inte läsa/skriva och ta bort åtkomst tooall entiteter. Det hjälper även minska hello skada om en SAS äventyras eftersom hello SAS har mindre ström i hello händer för att en angripare.
7. **Förstå att ditt konto kommer att debiteras för användning, inklusive göra med SAS.** Om du tillhandahåller skrivbehörighet tooa blob, kan användaren välja tooupload 200GB-blob. Om du har gett dem samt läsbehörighet, kan de välja toodownload den 10 gånger ta upp 2 TB utgång kostnader för dig. Igen, ange begränsade behörigheter toohelp minimera hello potentiella åtgärder av obehöriga användare. Använd tillfällig SAS tooreduce hotet (men Tänk också på klockavvikelser på hello sluttid).
8. **Validera data som skrivs med hjälp av SAS.** När ett klientprogram skriver data tooyour storage-konto, Kom ihåg att det kan finnas problem med dessa data. Om ditt program kräver att data kan verifieras eller behörighet innan den är klar toouse, utför den här verifieringen när hello data skrivs och innan den används av ditt program. Den här övningen även skyddar mot skadad eller skadliga data skrivs tooyour konto, antingen av en användare som korrekt förvärvade hello SAS eller av en användare som utnyttjar en känd SAS.
9. **Inte alltid att använda SAS.** Ibland uppväger hello risker associerade med en viss åtgärd mot ditt lagringskonto hello fördelarna med SAS. Skapa en mellannivå-tjänst som skriver tooyour storage-konto när du utför företag för dessa åtgärder regel verifiering, autentisering och granskning. Ibland kan enklare toomanage åtkomst på annat sätt. Till exempel om du vill toomake alla blobbar i en behållare kan läsas offentligt, kan du hello behållaren allmänheten, i stället för att tillhandahålla en tooevery SAS-klient för åtkomst.
10. **Använd Storage Analytics toomonitor ditt program.** Du kan använda loggning och mått tooobserve någon ökar i autentiseringsfel på grund av tooan avbrott i din SAS-providern tjänsten eller toohello oavsiktlig borttagning av en lagrade åtkomstprincip. Se hello [Azure Storage-teamets blogg](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/08/03/windows-azure-storage-logging-using-logs-to-track-storage-requests.aspx) för ytterligare information.

## <a name="sas-examples"></a>SAS-exempel
Nedan följer några exempel på båda typerna av signaturer för delad åtkomst, konto-SAS och tjänst-SAS.

toorun exemplen C# måste tooreference hello följande NuGet-paket i projektet:

* [Azure Storage-klientbibliotek för .NET](http://www.nuget.org/packages/WindowsAzure.Storage), version 6.x eller senare (toouse konto SAS).
* [Azure Configuration Manager](http://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager)

Ytterligare exempel som visar hur toocreate och testa en SAS finns [Azure kodexempel för lagring](https://azure.microsoft.com/documentation/samples/?service=storage).

### <a name="example-create-and-use-an-account-sas"></a>Exempel: Skapa och använda en konto-SAS
hello följande exempel skapar en konto-SAS som gäller för hello Blob- och Filtjänster och ger hello klienten behörigheterna Läs-, Skriv- och listbehörigheter tooaccess servicenivå API: er. hello konto-SAS begränsar hello protokollet tooHTTPS så hello begäran måste göras med HTTPS.

```csharp
static string GetAccountSASToken()
{
    // toocreate hello account SAS, you need toouse your shared key credentials. Modify for your account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

    // Create a new access policy for hello account.
    SharedAccessAccountPolicy policy = new SharedAccessAccountPolicy()
        {
            Permissions = SharedAccessAccountPermissions.Read | SharedAccessAccountPermissions.Write | SharedAccessAccountPermissions.List,
            Services = SharedAccessAccountServices.Blob | SharedAccessAccountServices.File,
            ResourceTypes = SharedAccessAccountResourceTypes.Service,
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Protocols = SharedAccessProtocol.HttpsOnly
        };

    // Return hello SAS token.
    return storageAccount.GetSharedAccessSignature(policy);
}
```

toouse hello konto SAS tooaccess servicenivå API: er för hello Blob-tjänsten, skapa ett objekt för Blob med hello SAS och hello slutpunkt för Blob-lagring för ditt lagringskonto.

```csharp
static void UseAccountSAS(string sasToken)
{
    // Create new storage credentials using hello SAS token.
    StorageCredentials accountSAS = new StorageCredentials(sasToken);
    // Use these credentials and hello account name toocreate a Blob service client.
    CloudStorageAccount accountWithSAS = new CloudStorageAccount(accountSAS, "account-name", endpointSuffix: null, useHttps: true);
    CloudBlobClient blobClientWithSAS = accountWithSAS.CreateCloudBlobClient();

    // Now set hello service properties for hello Blob client created with hello SAS.
    blobClientWithSAS.SetServiceProperties(new ServiceProperties()
    {
        HourMetrics = new MetricsProperties()
        {
            MetricsLevel = MetricsLevel.ServiceAndApi,
            RetentionDays = 7,
            Version = "1.0"
        },
        MinuteMetrics = new MetricsProperties()
        {
            MetricsLevel = MetricsLevel.ServiceAndApi,
            RetentionDays = 7,
            Version = "1.0"
        },
        Logging = new LoggingProperties()
        {
            LoggingOperations = LoggingOperations.All,
            RetentionDays = 14,
            Version = "1.0"
        }
    });

    // hello permissions granted by hello account SAS also permit you tooretrieve service properties.
    ServiceProperties serviceProperties = blobClientWithSAS.GetServiceProperties();
    Console.WriteLine(serviceProperties.HourMetrics.MetricsLevel);
    Console.WriteLine(serviceProperties.HourMetrics.RetentionDays);
    Console.WriteLine(serviceProperties.HourMetrics.Version);
}
```

### <a name="example-create-a-stored-access-policy"></a>Exempel: Skapa en princip för lagrade åtkomst
hello skapar följande kod en åtkomstprincip som är lagrade på en behållare. Du kan använda hello åtkomst toospecify principbegränsningar för en tjänst-SAS hello behållare eller dess blobbar.

```csharp
private static async Task CreateSharedAccessPolicyAsync(CloudBlobContainer container, string policyName)
{
    // Create a new shared access policy and define its constraints.
    // hello access policy provides create, write, read, list, and delete permissions.
    SharedAccessBlobPolicy sharedPolicy = new SharedAccessBlobPolicy()
    {
        // When hello start time for hello SAS is omitted, hello start time is assumed toobe hello time when hello storage service receives hello request.
        // Omitting hello start time for a SAS that is effective immediately helps tooavoid clock skew.
        SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
        Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.List |
            SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Create | SharedAccessBlobPermissions.Delete
    };

    // Get hello container's existing permissions.
    BlobContainerPermissions permissions = await container.GetPermissionsAsync();

    // Add hello new policy toohello container's permissions, and set hello container's permissions.
    permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
    await container.SetPermissionsAsync(permissions);
}
```

### <a name="example-create-a-service-sas-on-a-container"></a>Exempel: Skapa en tjänst-SAS på en behållare
hello följande kod skapar en SAS för en behållare. Om hello namnet på en befintlig lagrade åtkomstprincip får är principen associerad med hello SAS. Om inga lagrade åtkomstprincip skapar hello koden en ad hoc-SAS hello behållaren.

```csharp
private static string GetContainerSasUri(CloudBlobContainer container, string storedPolicyName = null)
{
    string sasContainerToken;

    // If no stored policy is specified, create a new access policy and define its constraints.
    if (storedPolicyName == null)
    {
        // Note that hello SharedAccessBlobPolicy class is used both toodefine hello parameters of an ad-hoc SAS, and
        // tooconstruct a shared access policy that is saved toohello container's shared access policies.
        SharedAccessBlobPolicy adHocPolicy = new SharedAccessBlobPolicy()
        {
            // When hello start time for hello SAS is omitted, hello start time is assumed toobe hello time when hello storage service receives hello request.
            // Omitting hello start time for a SAS that is effective immediately helps tooavoid clock skew.
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List
        };

        // Generate hello shared access signature on hello container, setting hello constraints directly on hello signature.
        sasContainerToken = container.GetSharedAccessSignature(adHocPolicy, null);

        Console.WriteLine("SAS for blob container (ad hoc): {0}", sasContainerToken);
        Console.WriteLine();
    }
    else
    {
        // Generate hello shared access signature on hello container. In this case, all of hello constraints for the
        // shared access signature are specified on hello stored access policy, which is provided by name.
        // It is also possible toospecify some constraints on an ad-hoc SAS and others on hello stored access policy.
        sasContainerToken = container.GetSharedAccessSignature(null, storedPolicyName);

        Console.WriteLine("SAS for blob container (stored access policy): {0}", sasContainerToken);
        Console.WriteLine();
    }

    // Return hello URI string for hello container, including hello SAS token.
    return container.Uri + sasContainerToken;
}
```

### <a name="example-create-a-service-sas-on-a-blob"></a>Exempel: Skapa en tjänst-SAS på en blob
hello följande kod skapar en SAS för en blob. Om hello namnet på en befintlig lagrade åtkomstprincip får är principen associerad med hello SAS. Om inga lagrade åtkomstprincip skapar hello koden en ad hoc-SAS på hello-blob.

```csharp
private static string GetBlobSasUri(CloudBlobContainer container, string blobName, string policyName = null)
{
    string sasBlobToken;

    // Get a reference tooa blob within hello container.
    // Note that hello blob may not exist yet, but a SAS can still be created for it.
    CloudBlockBlob blob = container.GetBlockBlobReference(blobName);

    if (policyName == null)
    {
        // Create a new access policy and define its constraints.
        // Note that hello SharedAccessBlobPolicy class is used both toodefine hello parameters of an ad-hoc SAS, and
        // tooconstruct a shared access policy that is saved toohello container's shared access policies.
        SharedAccessBlobPolicy adHocSAS = new SharedAccessBlobPolicy()
        {
            // When hello start time for hello SAS is omitted, hello start time is assumed toobe hello time when hello storage service receives hello request.
            // Omitting hello start time for a SAS that is effective immediately helps tooavoid clock skew.
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Create
        };

        // Generate hello shared access signature on hello blob, setting hello constraints directly on hello signature.
        sasBlobToken = blob.GetSharedAccessSignature(adHocSAS);

        Console.WriteLine("SAS for blob (ad hoc): {0}", sasBlobToken);
        Console.WriteLine();
    }
    else
    {
        // Generate hello shared access signature on hello blob. In this case, all of hello constraints for the
        // shared access signature are specified on hello container's stored access policy.
        sasBlobToken = blob.GetSharedAccessSignature(null, policyName);

        Console.WriteLine("SAS for blob (stored access policy): {0}", sasBlobToken);
        Console.WriteLine();
    }

    // Return hello URI string for hello container, including hello SAS token.
    return blob.Uri + sasBlobToken;
}
```

## <a name="conclusion"></a>Slutsats
Signaturer för delad åtkomst är användbara för att tillhandahålla begränsade behörigheter tooyour storage-konto tooclients som inte ska ha hello kontonyckel. Därför att de är en viktig del av hello säkerhetsmodell för alla program som använder Azure-lagring. Om du följer hello bästa praxis som anges här använda du SAS tooprovide större flexibilitet av åtkomst tooresources i ditt lagringskonto, utan att kompromissa med hello säkerheten för ditt program.

## <a name="next-steps"></a>Nästa steg
* [Hantera anonym läsbehörighet toocontainers och blobbar](../blobs/storage-manage-access-to-resources.md)
* [Delegera åtkomst med signatur för delad åtkomst](http://msdn.microsoft.com/library/azure/ee395415.aspx)
* [Introduktion till tabellen och kön SAS](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)

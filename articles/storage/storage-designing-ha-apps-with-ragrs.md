---
title: "aaaDesigning hög tillgängliga program med hjälp av Azure Geo-Redundant lagring med läsbehörighet (RA-GRS) | Microsoft Docs"
description: "Hur toouse Azure RA-GRS lagring tooarchitect en högtillgänglig programmet flexibla tillräckligt med toohandle avbrott."
services: storage
documentationcenter: .net
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 8f040b0f-8926-4831-ac07-79f646f31926
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 1/19/2017
ms.author: robinsh
ms.openlocfilehash: e4a9fe7ef33eecd894408b3c1ef59920a248d1bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="designing-highly-available-applications-using-ra-grs"></a>Designa hög tillgängliga program med hjälp av RA-GRS

En vanlig funktion för molnbaserad infrastruktur är att de ger en plattform för hög tillgänglighet som värd för program. Utvecklare av molnbaserade program måste du överväga att noggrant hur tooleverage den här plattformen toodeliver högtillgänglig program tootheir användare. Den här artikeln fokuserar särskilt på hur utvecklare kan använda hello Azure Storage Geo-Redundant lagring med läsbehörighet (RA-GRS) toomake sina fler tillgängliga program.

Det finns fyra alternativ för redundans – LRS (lokalt Redundant lagring), ZRS (zonen Redundant lagring), GRS (Geo-Redundant lagring) och RA-GRS (Geo-Redundant lagring med läsbehörighet). Vi toodiscuss GRS och RA-GRS i den här artikeln. Med GRS sparas tre kopior av dina data i hello primära region som du valde när du ställer in hello storage-konto. Tre ytterligare kopior underhålls asynkront i en sekundär region som anges av Azure. RA-GRS är hello samma sak som GRS förutom att du har läsbehörighet toohello sekundär kopia. Mer information om alternativ för hello olika Azure Storage redundans finns [Azure Storage-replikering](https://docs.microsoft.com/en-us/azure/storage/storage-redundancy). hello replikering artikeln beskriver också hello pairings hello primära och sekundära regioner.

Det finns kodstycken som ingår i den här artikeln och en länk tooa fullständigt exempel hello slutet som du kan hämta och köra.

## <a name="key-features-of-ra-grs"></a>Viktiga funktioner i RA-GRS

Innan vi berättar hur toouse RA-GRS lagring, vi pratar om dess egenskaper och beteende.

* Azure Storage upprätthåller en skrivskyddad kopia av hello data som du lagrar i din primära region i en sekundär region. som nämnts ovan anger hello lagringstjänsten hello platsen för hello sekundär region.

* hello skrivskyddad kopia är [överensstämmelse](https://en.wikipedia.org/wiki/Eventual_consistency) med hello data i hello primär region.

* För blobbar, tabeller och köer, kan du fråga hello sekundär region för en *tid för senaste synkronisering* värde som anger när Hej senaste replikeringen från hello primära toohello sekundär region inträffade. (Detta stöds inte för Azure File storage som saknar RA-GRS redundans just nu.)

* Du kan använda hello Storage-klientbibliotek toointeract med hello data i antingen hello primär eller sekundär region. Du kan också omdirigera Läs begär automatiskt toohello sekundär region om tidsgränsen uppnås för en läsbegäran toohello primär region.

* Om det finns ett större problem som påverkar tillgängligheten hello hello data i hello primär region, kan hello Azure-teamet utlösa en geo-redundans, då hello DNS-poster pekar toohello primära regionen kommer att vara ändrade toopoint toohello sekundär region.

* Om det uppstår geo-redundans, kommer Azure Markera en ny sekundär plats och replikera hello Dataplats toothat och peka hello sekundär DNS-poster tooit. sekundär slutpunkt för hello blir otillgänglig tills hello storage-konto är klar replikeras. Mer information finns [vilka toodo om ett Azure Storage-avbrott inträffar](https://docs.microsoft.com/en-us/azure/storage/storage-disaster-recovery-guidance).

## <a name="application-design-considerations-when-using-ra-grs"></a>Designöverväganden för programmet när du använder RA-GRS

hello Huvudsyftet med den här artikeln är tooshow du hur toodesign ett program som kommer att fortsätta toofunction (i en begränsad kapacitet) ens vid hello en större katastrof på hello primära data center. Det gör du genom att ditt program toohandle tillfälligt eller långvariga problem genom att växla tooread från hello sekundär region när det finns ett problem och växla tillbaka när hello primära region som är tillgänglig igen.

### <a name="using-eventually-consistent-data"></a>Med hjälp av överensstämmelse data

Den här föreslagna lösningen förutsätter att det är bra tooreturn vad kan vara inaktuella data toohello anropande programmet. Eftersom hello sekundära data är överensstämmelse, är det möjligt att hello data skrevs toohello primära men hello uppdatering toohello sekundära inte hade slutförts replikera när hello primär region blev inte tillgänglig.

Till exempel kunden kunde skicka en uppdatering som lyckas och sedan hello primära kunde kraschar innan hello uppdateras spridda toohello sekundär. I det här fallet får hello kunden ber sedan tillbaka tooread hello data, han hello inaktuella data i stället för hello uppdaterade data. Du måste bestämma om detta är acceptabelt och i så fall, hur du kommer meddelandet hello kunden. Du ser hur toocheck hello tid för senaste synkronisering på hello sekundära data senare i den här artikeln toosee om hello sekundära är uppdaterad.

### <a name="handling-services-separately-or-all-together"></a>Hantera tjänster separat eller alla tillsammans

Även inte troligt är det möjligt för en tjänst toobecome inte tillgänglig medan hello andra tjänster är fortfarande fungerar. Du kan hantera hello återförsök och skrivskyddat läge för varje service separat (BLOB, köer, tabeller) eller kan du hantera försök Allmänt för alla hello lagringstjänster tillsammans.

Om du använder köer och blobbar i ditt program, kan du till exempel bestämma tooput separat kod toohandle återförsökbart fel för var och en av dessa. Sedan om du får ett nytt försök från hello blob-tjänsten, men hello kötjänsten fortfarande fungerar, att endast hello del av programmet hanterar BLOB påverkas. Om du väljer toohandle alla lagringstjänsten återförsök Allmänt och anropet toohello blob-tjänsten returnerar ett återförsökbart fel sedan begäranden tooboth hello blob-tjänsten och hello kötjänsten påverkas.

Slutligen beror detta på hello komplexiteten i ditt program. Kan du välja inte toohandle hello fel av tjänsten, men i stället tooredirect läsförfrågningar för alla storage services toohello sekundär region och kör programmet hello i skrivskyddat läge när du upptäcker ett problem med storage-tjänst i hello primära region.

### <a name="other-considerations"></a>Andra överväganden

Dessa hello andra överväganden som diskuteras i hello resten av den här artikeln.

*   Hantera återförsök för läsbegäranden hello strömbrytare mönster

*   Slutligen konsekventa data och hello tid för senaste synkronisering

*   Testning

## <a name="running-your-application-in-read-only-mode"></a>Kör ditt program i skrivskyddat läge

toouse RA-GRS lagring, du måste vara kan toohandle både kunde inte läsa begäranden och misslyckade uppdateringsbegäranden (med uppdatering i det här fallet innebörd infogningar, uppdateringar och borttagningar). Om hello primära data center misslyckas, läsa begäranden kan vara omdirigerade toohello sekundära datacenter, men begäranden om att uppdatera inte eftersom hello sekundära är skrivskyddad. Därför måste vissa sätt toorun ditt program i skrivskyddat läge.

Du kan till exempel ange en flagga som ska kontrolleras innan du skickar alla begäranden toohello lagring uppdateringstjänsten. När en av begäranden om att uppdatera hello kommer kan du hoppa över den och returnera en lämplig respons toohello kund. Du kanske även vill toodisable vissa funktioner helt och hållet förrän hello problemet har lösts och meddela användarna att dessa funktioner är inte tillgänglig för tillfället.

Om du väljer toohandle fel för varje service separat, måste du också toohandle hello möjlighet toorun ditt program i skrivskyddat läge av tjänsten. Du kan ha skrivskyddad flaggor för varje tjänst som kan aktiveras och inaktiveras och referensen hello lämpliga flaggan i hello lämpliga platser i din kod.

Som kan toorun ditt program i skrivskyddat läge har en annan sida fördel – den ger dig hello möjlighet tooensure begränsade funktioner under en Programuppgradering för viktiga. Du kan utlösa ditt program toorun i skrivskyddat läge och punkt toohello sekundära datacenter, se till att ingen kommer åt hello data på hello primär region medan du gör uppgraderingar.

## <a name="handling-updates-when-running-in-read-only-mode"></a>Hantera uppdateringar när den körs i skrivskyddat läge

Det finns många sätt toohandle uppdatering begäranden när den körs i skrivskyddat läge. Vi kommer inte täcker detta utförligt, men i allmänhet det finns ett par av mönster som du vill.

1.  Du kan svara tooyour användare och informera dem om du för närvarande inte accepterar uppdateringar. Kontakta hanteringssystemet kan aktivera kunder tooaccess kontaktinformation men inte göra uppdateringar.

2.  Du kan placera dina uppdateringar i en annan region. I detta fall skulle du skriva väntar på att uppdatera begäranden tooa kön i en annan region och sedan har en sätt tooprocess autentiseringsbegäranden när hello primära Datacenter är online igen. I det här scenariot bör du låta hello kunder vet att hello update begärt kö för senare bearbetning.

3.  Du kan skriva dina uppdateringar tooa storage-konto i en annan region. Sedan kan hello primära Datacenter är online igen du ha en sätt toomerge uppdateringarna i hello primära data, beroende på hello datastrukturen hello. Om du skapar separata filer med en stämpel för datum/tid i hello namn, kan du kopiera dessa filer tillbaka toohello primär region. Detta fungerar för vissa arbetsbelastningar som till exempel loggnings- och iOT-data.

## <a name="handling-retries"></a>Hantera nya försök

Hur vill du veta vilka fel återförsökbart? Detta bestäms av hello storage-klientbiblioteket. Ett 404-fel (Det gick inte att hitta resurs) är exempelvis inte återförsökbart eftersom du försöker den inte är sannolikt tooresult i lyckades. På hello däremot 500 fel är återförsökbart eftersom det är ett serverfel och det kan vara ett övergående problem. Mer information finns på hello [öppna källkoden för hello ExponentialRetry klassen](https://github.com/Azure/azure-storage-net/blob/87b84b3d5ee884c7adc10e494e2c7060956515d0/Lib/Common/RetryPolicies/ExponentialRetry.cs) i hello .NET storage-klientbiblioteket. (Leta efter hello ShouldRetry metod).

### <a name="read-requests"></a>Läs begäranden

Läs begäranden kan vara omdirigerade toosecondary lagring om det är problem med primär lagring. Som beskrivs ovan i [med slutligen konsekventa Data](#using-eventually-consistent-data), det måste vara godkänd för ditt program toopotentially läsa inaktuella data. Om du använder hello lagring klienten biblioteket tooaccess RA-GRS data, kan du ange hello försök beteendet för en läsbegäran genom att ange ett värde för hello **LocationMode** egenskapen tooone av hello följande:

*   **PrimaryOnly** (hello standard)

*   **PrimaryThenSecondary**

*   **SecondaryOnly**

*   **SecondaryThenPrimary**

När du anger hello **LocationMode** för**PrimaryThenSecondary**om hello inledande läsa begäran toohello primära slutpunkten misslyckas med ett återförsökbart fel, hello klienten gör automatiskt en annan Läs sekundär slutpunkt för toohello-begäran. Om hello fel är en tidsgräns för servern, kommer hello klienten ha toowait för hello timeout tooexpire innan den får ett återförsökbart fel från hello-tjänsten.

Det finns två scenarier tooconsider när du bestämmer hur toorespond tooa återförsökbart fel:

*   Det här är en isolerad problem och primär slutpunkt för efterföljande förfrågningar toohello kommer inte att returnera ett återförsökbart fel inträffade. Ett exempel på där detta kan inträffa är när det är ett tillfälligt fel.

    I det här scenariot, det finns inga betydande prestandaproblem i med **LocationMode** ställa in också**PrimaryThenSecondary** som det här inträffar bara sällan.

*   Detta är ett problem med minst en av hello lagringstjänster i hello primär region och alla efterföljande förfrågningar toothat tjänst i hello primära regionen är sannolikt tooreturn återförsökbart fel under en tidsperiod. Ett exempel på detta är om hello primära region inte är helt tillgänglig.

    I det här scenariot finns det en prestandaförsämring eftersom alla skrivskyddade förfrågningar kommer först försöka hello primära slutpunkt, vänta tills hello timeout tooexpire och sedan växla toohello sekundär slutpunkt.

För dessa scenarier kan du identifiera att det är ett pågående problem med hello primära slutpunkt och skicka alla Läs begäranden direkt toohello sekundära slutpunkten genom att ange hello **LocationMode** egenskapen för **SecondaryOnly**. För tillfället bör du också ändra hello programmet toorun i skrivskyddat läge. Den här metoden kallas hello [strömbrytare mönster](https://msdn.microsoft.com/library/dn589784.aspx).

### <a name="update-requests"></a>Begäranden om att uppdatera

hello strömbrytare mönster kan också vara tillämpade tooupdate begäranden. Begäranden om att uppdatera måste omdirigerade toosecondary lagringen, vilket är skrivskyddad. För sådana begäranden, bör du lämna hello **LocationMode** egenskapsuppsättning för**PrimaryOnly** (hello standard). toohandle felen, du kan tillämpa en mått toothese begäranden – till exempel 10 fel i en rad – och när din tröskeln uppfylls växla hello program i skrivskyddat läge. Du kan använda hello samma metoder för att returnera tooupdate läge som de som beskrivs nedan i hello nästa avsnitt om hello strömbrytare mönster.

## <a name="circuit-breaker-pattern"></a>Mönster för strömbrytare

Med hjälp av hello strömbrytare mönster i ditt program kan förhindra att den du försöker en åtgärd som är sannolikt toofail upprepade gånger. Det gör hello programmet toocontinue toorun snarare än tar tid medan hello åtgärden försöks exponentiellt. Dessutom upptäcks när hello-fel har åtgärdats där programmet hello kan försöka hello igen.

### <a name="how-tooimplement-hello-circuit-breaker-pattern"></a>Hur tooimplement hello strömbrytare mönster

tooidentify att det finns en pågående problem med en primär slutpunkt kan du övervaka hur ofta hello klienten påträffar ett återförsökbart fel. Eftersom varje fall skiljer sig, har du toodecide på hello tröskelvärde du vill toouse för hello beslut tooswitch toohello sekundär slutpunkt och köra programmet hello i skrivskyddat läge. Du kan till exempel bestämma tooperform hello växel om det finns 10 fel i en rad med ingen lyckade försök. Ett annat exempel är tooswitch om 90% av hello begäranden under en 2-minutersperiod misslyckas.

Du kan bara ha ett antal hello fel och om det finns en fungerande innan det nådde hello maximala, Ställ in hello antal tillbaka toozero hello första scenariot. Hello andra scenariot hello enkelriktade tooimplement är det toouse MemoryCache-objekt (i .NET). För varje begäran att lägga till en CacheItem toohello cache, ange hello värdet toosuccess (1) eller misslyckas (0) och ange hello Förfallodatum tid too2 minuter från nu (eller vad den tid begränsningen är). När en post giltighetstid har uppnåtts, tas hello-posten bort automatiskt. Detta ger ett rullande fönster 2 minuter. Varje gång du gör en begäran toohello lagringstjänst använda du först en Linq-fråga över hello MemoryCache objektet toocalculate hello procent lyckades genom att summera värdena för hello och dividera med hello antal. När hello procent lyckade hamnar under tröskelvärdet för (till exempel 10%), ange hello **LocationMode** -egenskapen för diskläsningsbegäranden för**SecondaryOnly** och växla hello program i skrivskyddat läge innan fortsätter.

hello tröskelvärde för fel används toodetermine när toomake hello växeln kan skilja sig från tjänsten tooservice i ditt program, så du bör överväga att göra dem till parametrar. Detta är även om du beslutar toohandle återförsökbart fel från varje service separat eller som en, vilket beskrivs ovan.

Ett annat övervägande är hur toohandle flera instanser av ett program, och vilka toodo när du identifiera återförsökbart fel i varje instans. Du kan till exempel ha 20 virtuella datorer som körs med hello samma program lästes in. Ska du hantera varje instans separat? Om en instans startar problem, vill du toolimit hello svar toojust som en instans eller vill du tootry toohave alla instanser svara i hello samma sätt när en instans har ett problem? Hantera hello instanser separat är mycket enklare än om du försöker toocoordinate hello svar över dem, men hur du gör detta beror på ditt program arkitektur.

### <a name="options-for-monitoring-hello-error-frequency"></a>Alternativ för att övervaka hello frekvens för fel

Det finns tre huvudsakliga alternativ för att övervaka hello frekvensen för återförsök i hello primära region i ordning toodetermine när tooswitch över toohello sekundär region och ändrar hello programmet toorun i skrivskyddat läge.

*   Lägger till en hanterare för hello [ **försöker på nytt** ](http://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.operationcontext.retrying.aspx) händelse på hello [ **OperationContext** ](http://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.operationcontext.aspx) objekt du skickar begäranden för lagring av tooyour – detta är hello metoden visas i den här artikeln och används i hello tillhörande exempel. Dessa händelser eller när hello klientens en begäran, vilket gör att du tootrack hur ofta hello klienten påträffar återförsökbart fel på en primär slutpunkt.

    ```csharp 
    operationContext.Retrying += (sender, arguments) =>
    {
        // Retrying in hello primary region
        if (arguments.Request.Host == primaryhostname)
            ...
    };
    ```

*   I hello [ **Evaluate** ](http://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.retrypolicies.iextendedretrypolicy.evaluate.aspx) metod i en egen återförsöksprincip du kan köra anpassad kod när ett nytt försök görs. I tillägg toorecording när ett nytt försök sker detta också ger du hello möjlighet toomodify retry-beteende.

    ```csharp 
    public RetryInfo Evaluate(RetryContext retryContext,
    OperationContext operationContext)
    {
        var statusCode = retryContext.LastRequestResult.HttpStatusCode;
        if (retryContext.CurrentRetryCount >= this.maximumAttempts
        || ((statusCode &gt;= 300 && statusCode &lt; 500 && statusCode != 408)
        || statusCode == 501 // Not Implemented
        || statusCode == 505 // Version Not Supported
            ))
        {
        // Do not retry
            return null;
        }

        // Monitor retries in hello primary location
        ...

        // Determine RetryInterval and TargetLocation
        RetryInfo info =
            CreateRetryInfo(retryContext.CurrentRetryCount);

        return info;
    }
    ```

*   hello tredje sätt är tooimplement en anpassad övervakning komponent i ditt program som pingar kontinuerligt primära lagringsplatsen slutpunkten med dummy läsa begäranden (till exempel läsa en liten blob) toodetermine dess hälsa. Detta kan ta några resurser, men inte mycket. När ett problem har identifierats som når din tröskelvärde kan du sedan utför hello växeln för**SecondaryOnly** och skrivskyddat läge.

Vid något tillfälle vill tooswitch tillbaka toousing hello primära slutpunkt och tillåta uppdateringar. Om du använder en av hello första två metoderna ovan kan du helt enkelt växla tillbaka toohello primära slutpunkt och aktivera uppdateringsläge när ett godtyckligt valda tid eller antal åtgärder har utförts. Sedan kan du låta den gå igenom hello logik igen. Om hello problemet har åtgärdats, den fortsätta toouse hello primära slutpunkt och tillåta uppdateringar. Om det finns fortfarande ett problem, växlar en gång tillbaka toohello sekundär slutpunkt och skrivskyddat läge efter misslyckade hello kriterier som du har ställt in.

Hello tredje scenariot när pinga hello primära lagringsplatsen endpoint blir lyckade igen, du kan utlösa hello växla tillbaka för**PrimaryOnly** och fortsätta att tillåta uppdateringar.

## <a name="handling-eventually-consistent-data"></a>Hantering av överensstämmelse data

RA-GRS fungerar genom att replikera transaktioner från hello primära toohello sekundär region. Den här replikeringen garanterar att hello data i hello sekundära regionen är *överensstämmelse*. Detta innebär att alla hello transaktioner i hello primär region så småningom kommer att visas i hello sekundär region, utan att det kan finnas en fördröjning innan de kan visas och att det är inte säkert hello transaktioner tas emot i hello sekundär region i samma ordning som hello som som de ursprungligen tillämpades i hello primära region. Om transaktionerna kommer till hello sekundär region utanför måste du *kan* Överväg att dina data i hello sekundär region toobe i ett inkonsekvent tillstånd tills hello service fångar.

hello nedan visas ett exempel på vad som händer när du uppdaterar hello information om en medarbetare toomake hon medlem i hello *administratörer* roll. För hello ut för det här exemplet detta kräver att du uppdaterar hello **medarbetare** entiteten och uppdatera en **administratörsroll** entitet med ett antal hello totala antalet administratörer. Observera hur hello uppdateringar tillämpas i ordning i hello sekundär region.

| **Tid** | **Transaktionen**                                            | **Replikering**                       | **Tid för senaste synkronisering** | **Resultat** |
|----------|------------------------------------------------------------|---------------------------------------|--------------------|------------| 
| T0       | Transaktionen A: <br> Infoga medarbetare <br> entiteten i primär |                                   |                    | Transaktionen A infogas tooprimary<br> replikeras inte ännu. |
| T1       |                                                            | Transaktionen A <br> replikeras till<br> sekundär | T1 | Transaktionen A replikerade toosecondary. <br>Synkronisera senast uppdaterat.    |
| T2       | Transaktionen B:<br>Uppdatering<br> medarbetare entitet<br> i primära  |                                | T1                 | Transaktionen skrivs tooprimary, B<br> replikeras inte ännu.  |
| T3       | Transaktionen C:<br> Uppdatering <br>Administratören<br>rollentiteten i<br>primär |                    | T1                 | Transaktionen skrivs tooprimary, C<br> replikeras inte ännu.  |
| *T4*     |                                                       | Transaktionen C <br>replikeras till<br> sekundär | T1         | Transaktionen C replikerade toosecondary.<br>LastSyncTime inte uppdateras eftersom <br>transaktionen B har ännu inte replikerats.|
| *T5*     | Läs entiteter <br>från sekundär                           |                                  | T1                 | Du får hello inaktuella värdet för medarbetare <br> entiteten eftersom transaktionen B har inte <br> replikerade ännu. Du får hello nytt värde för<br> administratören rollentiteten eftersom C har<br> replikeras. Tid för senaste synkronisering har fortfarande inte<br> har uppdaterats eftersom transaktionen B<br> har inte replikeras. Du kan se den<br>administratören rollentiteten är inkonsekvent <br>eftersom hello entitet datum/tid är efter <br>hello tid för senaste synkronisering. |
| *T6*     |                                                      | Transaktionen B<br> replikeras till<br> sekundär | T6                 | *T6* – alla transaktioner via C har <br>har replikerats tid för senaste synkronisering<br> har uppdaterats. |

I det här exemplet förutsätter att hello klienten växlar tooreading från hello sekundär region på T5. Det kan läsa hello **administratörsroll** entitet just nu, men hello entiteten innehåller ett värde för hello antalet administratörer som inte är konsekvent med hello antalet **medarbetare** entiteter som är markerade som administratörer i hello sekundär region just nu. Klienten kan bara visa det här värdet med hello risk att den är inkonsekvent information. Alternativt hello kunde försöker klienten toodetermine som hello **administratörsroll** är i ett eventuellt inkonsekvent tillstånd eftersom hello uppdateringar har hänt fel ordning och informera hello användaren om detta.

toorecognize att det finns potentiellt inkonsekventa data, hello klienten kan använda hello värdet för hello *tid för senaste synkronisering* att du kan få när som helst genom att fråga en storage-tjänst. Detta visar hello tid då hello data i hello sekundär region senast konsekvent och när hello-tjänsten hade tillämpas alla hello transaktioner tidigare toothat punkt i tiden. I hello-exemplet ovan när hello tjänsten infogar hello **medarbetare** entiteten i hello sekundär region hello tid för senaste synkronisering har angetts för*T1*. Det förblir *T1* tills hello uppdateras hello **medarbetare** entiteten i hello sekundär region när den är inställd för*T6*. Om hello klienten hämtar hello tid för senaste synkronisering när det läser hello-entiteten vid *T5*, den kan jämföra den mot hello tidsstämpeln på hello entitet. Om hello tidstämpel för hello entiteten är senare än hello tid för senaste synkronisering och sedan hello målentiteten är i ett eventuellt inkonsekvent tillstånd och du kan ta allt är hello lämplig åtgärd för programmet. Det här fältet kräver att du vet när hello senaste uppdatering toohello primära slutfördes.

## <a name="testing"></a>Testning

Det är viktigt tootest som programmet fungerar som förväntat när återförsökbart fel påträffas. Till exempel behöver du tootest som hello programmet växlar toohello sekundära och i skrivskyddat läge när ett problem upptäcks och växlar tillbaka när hello primär region blir tillgänglig igen. toodo, du behöver ett sätt toosimulate återförsökbart fel och styra hur ofta de inträffar.

Du kan använda [Fiddler](http://www.telerik.com/fiddler) toointercept och ändra HTTP-svar i ett skript. Det här skriptet kan identifiera svar som kommer från din primära slutpunkt och ändra hello HTTP-status kod tooone som hello Storage-klientbibliotek känner igen som ett återförsökbart fel inträffade. Det här kodstycket visar ett enkelt exempel på ett Fiddler skript som fångar upp svar tooread förfrågningar mot hello **employeedata** tabell tooreturn status 502:

```java
static function OnBeforeResponse(oSession: Session) {
    ...
    if ((oSession.hostname == "\[yourstorageaccount\].table.core.windows.net")
      && (oSession.PathAndQuery.StartsWith("/employeedata?$filter"))) {
        oSession.responseCode = 502;
    }
}
```

Du kan utöka ett brett spektrum av begäranden för det här exemplet toointercept och bara ändra hello **responseCode** på några av dem toobetter simulera ett verkligt scenario. Mer information om hur du anpassar Fiddler skript finns [ändra en begäran eller ett svar](http://docs.telerik.com/fiddler/KnowledgeBase/FiddlerScript/ModifyRequestOrResponse) i hello Fiddler dokumentation.

Om du har gjort hello tröskelvärden för växling av ditt program tooread läget endast konfigurerbara blir enklare tootest hello fungerar med icke-produktion transaktionsvolymer.

## <a name="next-steps"></a>Nästa steg

* Mer information om läsbehörighet Geo-redundans, inklusive ett annat exempel på hur hello LastSyncTime ställs finns [Windows Azure-lagringsalternativ redundans och Geo-Redundant lagring med läsbehörighet](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/).

* Ett fullständigt exempel som visar hur toomake hello växla fram och tillbaka mellan hello primära och sekundära slutpunkter, se [Azure exempel – som använder hello strömbrytare mönster med RA-GRS lagring](https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs).

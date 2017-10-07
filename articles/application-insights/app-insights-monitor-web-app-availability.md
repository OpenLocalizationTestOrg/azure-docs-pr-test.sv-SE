---
title: "aaaMonitor tillgänglighet och svarstider för någon webbplats | Microsoft Docs"
description: "Konfigurera webbtester i Application Insights. Få aviseringar om en webbplats blir otillgänglig eller svarar långsamt."
services: application-insights
documentationcenter: 
author: SoubhagyaDash
manager: carmonm
ms.assetid: 46dc13b4-eb2e-4142-a21c-94a156f760ee
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/25/2017
ms.author: bwren
ms.openlocfilehash: 4c5425c948770cc57a648ca50e217c75ac75fbd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-availability-and-responsiveness-of-any-web-site"></a>Övervaka tillgänglighet och svarstider på valfri webbplats
När du har distribuerat din webbapp eller webbplats tooany server kan ställa du in testerna toomonitor dess tillgänglighet och svarstider. [Azure Application Insights](app-insights-overview.md) skickar webbegäranden tooyour program med jämna mellanrum från punkter runt hälsningsmeddelande. Den varnar dig om programmet inte svarar eller svarar långsamt.

Du kan ställa in tillgänglighetstester för alla HTTP eller HTTPS-slutpunkt som är tillgänglig från hello offentliga internet. Du har inte tooadd något annat toohello webbplats som du testar. Ingen även har toobe webbplatsen: du kan testa en REST API-tjänst som du är beroende av.

Det finns två typer av tillgänglighetstester:

* [URL: en ping-testet](#create): ett enkelt test som du kan skapa i hello Azure-portalen.
* [Flera steg webbtest](#multi-step-web-tests): som du skapar i Visual Studio Enterprise och överför toohello portal.

Du kan skapa upp too25 tillgänglighetstester per resurs för program.

## <a name="create"></a>1. Öppna en resurs för dina tillgänglighetstestrapporter

**Om du redan har konfigurerat Application Insights** för ditt webbprogram, öppna dess Application Insights-resurs i hello [Azure-portalen](https://portal.azure.com).

**Eller, om du vill toosee dina rapporter i en ny resurs** registrera dig för[Microsoft Azure](http://azure.com), gå toohello [Azure-portalen](https://portal.azure.com), och skapa en Application Insights-resurs.

![Nytt > Application Insights](./media/app-insights-monitor-web-app-availability/11-new-app.png)

Klicka på **alla resurser** tooopen hello översikt bladet för hello ny resurs.

## <a name="setup"></a>2. Skapa ett URL-pingtest
Öppna hello tillgänglighet bladet och Lägg till ett test.

![Fill minst hello URL för din webbplats](./media/app-insights-monitor-web-app-availability/13-availability.png)

* **hello URL** kan vara en webbsida som du vill tootest, men det måste vara synlig från hello offentliga internet. hello-URL kan innehålla en frågesträng. Du kan arbeta med din databas om du vill. Om hello URL matchar tooa omdirigering, följ vi in too10 omdirigeringar.
* **Parsa beroende begäranden**: om det här alternativet är markerat hello test begär avbildningar, skript, filer och andra filer som ingår i hello webbsida under testet. hello innehåller inspelade svarstid hello tidsåtgång tooget dessa filer. hello testet misslyckas om dessa resurser inte kan har hämtats inom hello tidsgränsen för hello hela testet. 

    Om hello-alternativet inte är markerat begär hello test endast hello fil vid hello-URL som du angav.
* **Aktivera återförsök**: om det här alternativet är markerat när hello testet misslyckas, det är ett nytt försök efter en kort tid. Ett fel rapporteras endast om tre på varandra följande försök misslyckas. Efterföljande tester utförs sedan på hello vanliga Testfrekvensen. Försök igen är tillfälligt tills hello nästa lyckades. Den här regeln tillämpas separat på varje testplats. Vi rekommenderar det här alternativet. I genomsnitt försvinner ca 80 % av felen vid återförsök.
* **Testfrekvens**: Anger hur ofta hello testet körs från varje test-plats. Med en frekvens på fem minuter och fem testplatser testas din webbplats i genomsnitt varje minut.
* **Testa platser** är hello placerar från som våra servrar skicka Webbadress begäranden tooyour. Välj mer än en så att du kan skilja mellan problem på din webbplats och nätverksproblem. Du kan välja in too16 platser.
* **Villkor för lyckad test**:

    **Testtidsgräns**: minska det här värdet toobe aviserad om långsamt svar. hello test räknas som ett fel om hello svar från din plats inte har tagits emot inom den tiden. Om du har valt **parsa beroende begäranden**sedan alla hello bilder, filer, skript och andra beroende resurser måste har tagits emot inom den tiden.

    **HTTP-svar**: hello returnerade statuskoden räknas som lyckas. 200 är hello-kod som anger att en normal webbsida har returnerats.

    **Innehållsmatchning**: en sträng, t.ex. ”Välkommen!”. Vi testar i varje svar om någon skiftlägeskänslig matchning förekommer. Den måste vara en enkel sträng utan jokertecken. Glöm inte att om sidan innehåll ändringarna du kanske tooupdate den.
* **Aviseringar** , som standard skickas tooyou om det finns fel på tre platser över fem minuter. Ett fel på en plats är sannolikt toobe nätverksproblem och inte ett problem med din webbplats. Men du kan ändra hello tröskelvärdet toobe mer eller mindre känslig, och du kan också ändra som hello e-postmeddelanden ska skickas till.

    Du kan konfigurera en [webhook](../monitoring-and-diagnostics/insights-webhooks-alerts.md) som anropas när en avisering genereras. (Observera dock att frågeparametrar inte skickas som egenskaper för närvarande.)

### <a name="test-more-urls"></a>Testa fler URL:er
Lägg till fler test. Exempelvis lägga tootesting startsidan, du kan kontrollera i din databas körs genom att testa hello URL: en för en sökning.


## <a name="monitor"></a>3. Visa tillgänglighetstestresultat

Efter några minuter, klickar du på **uppdatera** toosee testresultaten. 

![Sammanfattningen av resultaten på hello hem blad](./media/app-insights-monitor-web-app-availability/14-availSummary-3.png)

Hej scatterplot innehåller exempel på hello testresultaten som har diagnostiska test-steg-information. hello test motorn lagrar diagnostisk information för tester med fel. För lyckad tester som diagnostisk information lagras för en delmängd av hello körningar. Hovra över alla hello grönt/red punkter toosee hello test tidsstämpel, testperioden, plats och namn på testet. Klicka dig igenom några punkt i hello punktdiagram ritytans toosee hello information om testresultat för hello.  

Välj en viss test, plats, eller minska tiden för hello period toosee resultat till runt hello perioden av intresse. Använd Sök Explorer toosee resultat från alla körningar, eller Analytics frågor toorun anpassade rapporter på dessa data.

I tillägg toohello raw resultat, att det finns två mått för tillgänglighet i Metrics Explorer: 

1. Tillgänglighet: Procentandelen hello tester, som över alla test körningar. 
2. Testets varaktighet: genomsnittlig tid för alla testkörningar.

Du kan använda filter på hello test namn, plats tooanalyze trender för en viss test och/eller plats.

## <a name="edit"></a>Granska och redigera tester

Välj en specifik test hello sammanfattningssidan. Där du kan se specifika resultat för testet och redigera eller tillfälligt inaktivera det.

![Redigera eller inaktivera ett webbtest](./media/app-insights-monitor-web-app-availability/19-availEdit-3.png)

Du kanske vill toodisable tillgänglighetstester eller hello varningen regler som är kopplade till dem när du utför underhåll på din tjänst. 

## <a name="failures"></a>Om du ser fel
Klicka på en röd punkt.

![Klicka på en röd punkt](./media/app-insights-monitor-web-app-availability/open-instance-3.png)


Från ett tillgänglighetstestresultat kan du:

* Kontrollera hello svar togs emot från servern.
* Öppna hello telemetri som skickas av server-appen under bearbetning av hello-instans för misslyckade begäranden.
* Logga ett problem eller arbetsobjekt i Git eller VSTS tootrack hello problem. hello-programfel innehåller en länk toothis händelse.
* Öppna hello web testresultat i Visual Studio.


*Ser det okej ut trots att fel har rapporterats?* Kontrollera alla hello-avbildningar, skript, formatmallar och andra filer som lästs in av hello-sidan. Om någon av dem inte rapporteras hello test som misslyckad, även om hello huvudsakliga HTML-sidan läses in OK.

*Inga relaterade objekt?* Om du har konfigurerat Application Insights för din app på serversidan kan detta bero på att [sampling](app-insights-sampling.md) pågår. 

## <a name="multi-step-web-tests"></a>Webbtester med flera steg
Du kan övervaka ett scenario med en serie URL:er. Till exempel om du övervakar en försäljning webbplats, kan du testa att lägga till objekt toohello kundvagn fungerar.

> [!NOTE] 
> Det utgår en avgift för webbtester med flera steg. [Prisschema](http://azure.microsoft.com/pricing/details/application-insights/).
> 

toocreate ett test i flera steg du registrera hello scenario med hjälp av Visual Studio Enterprise och överför hello inspelning tooApplication insikter. Application Insights spelar upp hello scenario med intervall och verifierar hello svar.

> [!NOTE]
> Du kan inte använda kodade funktioner eller loopar i dina tester. hello test måste finnas helt i hello .webtest skript. Du kan dock använda standardplugin-program.
>

#### <a name="1-record-a-scenario"></a>1. Spela in ett scenario
Använd Visual Studio Enterprise toorecord en webbsession.

1. Skapa ett testprojekt för webbprestanda.

    ![Skapa ett projekt i Visual Studio Enterprise-utgåvan från hello webbprestanda och belastningstest mall.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-create.png)

 * *Ser hello webbprestanda och belastningstest mallen?* - Stäng Visual Studio Enterprise. Öppna **Visual Studio Installer** toomodify Visual Studio Enterprise-installation. Välj **Web Performance and load testing tools** (Verktyg för webbprestanda- och inläsningstest) under **Individual Components** (Enskilda komponenter).

2. Öppna hello .webtest fil och börja spela in.

    ![Öppna hello .webtest filen och klicka på posten.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-start.png)
3. Hello användaråtgärder som du vill använda toosimulate i testet: Öppna din webbplats, lägga till en produkt toohello kundvagn och så vidare. Stoppa sedan testet.

    ![Hej webbtestet lådan körs i Internet Explorer.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-record.png)

    Håll scenariot kort. Det finns en gräns på 100 steg och 2 minuter.
4. Redigera hello test till:

   * Lägg till verifieringar toocheck hello emot text och svar koder.
   * Ta bort eventuella överflödiga interaktioner. Du kan också ta bort beroende begäranden för bilder eller tooad eller spåra platser.

     Kom ihåg att du bara redigera hello testskript - du kan inte lägga till egen kod eller anropa andra webbtester. Infoga inte slingor i hello test. Du kan använda standard-plugin-program för webbtester.
5. Kör hello test i Visual Studio toomake att den fungerar.

    hello web testaren öppnar en webbläsare och upprepas hello åtgärder som du antecknade. Kontrollera att det fungerar som förväntat.

    ![Öppna hello .webtest filen i Visual Studio och klicka på Kör.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-run.png)

#### <a name="2-upload-hello-web-test-tooapplication-insights"></a>2. Överför hello web test tooApplication insikter
1. Skapa ett webbtest i hello Application Insights-portalen.

    ![Välj Lägg till på hello web tester bladet.](./media/app-insights-monitor-web-app-availability/16-another-test.png)
2. Markera flera steg test och överför hello .webtest-fil.

    ![Välj webbtestet med flera steg.](./media/app-insights-monitor-web-app-availability/appinsights-71webtestUpload.png)

    Ange hello test platser, frekvens och avisering parametrar i hello testar samma sätt som för ping.

#### <a name="3-see-hello-results"></a>3. Se hello resultat

Visa testet resultat och eventuella fel i hello samma sätt som enda-url-tester.

Dessutom kan du hämta hello test resultat tooview dem i Visual Studio.

#### <a name="too-many-failures"></a>För många fel?

* En vanlig orsak till felet är att hello testet körs för lång. Det får inte köras längre än två minuter.

* Glöm inte att alla hello resurser på en sida måste läsa in korrekt för hello test toosucceed, inklusive skript, formatmallar, bilder och så vidare.

* Hej webbtest helt finnas i hello .webtest skript: du kan inte använda kodade i hello test.

### <a name="plugging-time-and-random-numbers-into-your-multi-step-test"></a>Använda tid och slumptal i flerstegstest
Anta att du testar ett verktyg som hämtar tidsberoende data, till exempel aktier från ett externt flöde. När du registrerar ditt webbtest toouse vissa tider är, men du har angett dem som parametrar av hello testa kan StartTime och EndTime.

![Ett webbtest med parametrar.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-parameters.png)

När du kör hello test, som EndTime toobe hello finns alltid tid och StartTime bör vara 15 minuter sedan.

Web Test plugin-program ger hello sätt toodo parameterstyra gånger.

1. Lägg till ett plugin-program för webbtester för varje variabelt parametervärde som du behöver. I hello test verktygsfältet väljer **lägga till Web testa plugin-programmet**.

    ![Välj Lägg till plugin-program för webbtest och välj en typ.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugins.png)

    I det här exemplet använder vi två instanser av hello datum tid plugin-program. En instans är för ”15 minuter tidigare” och en annan för ”nu”.
2. Öppna hello egenskaper för varje plugin-program. Ge det ett namn och ange det toouse hello aktuell tid. För den ena anger du Lägg till minuter = -15.

    ![Ange namn, Använd aktuell tid och Lägg till minuter.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugin-parameters.png)
3. I hello parametrar för web, använder du {{plugin-name}} tooreference ett plugin-namn.

    ![Använd {{plugin-name}} i hello test-parametern.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugin-name.png)

Ladda upp din test toohello portal. Hello dynamiska värden används vid varje körning av hello test.

## <a name="dealing-with-sign-in"></a>Hantera inloggning
Om dina användare loggar in tooyour app, har du olika alternativ för att simulera inloggning så att du kan testa sidor bakom hello-inloggning. hello-metod som du använder beror på hello tillhandahålls av hello app.

I samtliga fall bör du skapa ett konto i ditt program för hello syftet testning. Om möjligt begränsa hello behörigheter för det här testet kontot så att det inte finns någon möjlighet till hello webbtester påverkar verkliga användare.

### <a name="simple-username-and-password"></a>Enkelt användarnamn och lösenord
Registrera ett webbtest i hello vanligt. Ta bort cookies först.

### <a name="saml-authentication"></a>SAML-autentisering
Använd hello SAML-plugin-programmet som är tillgänglig för web tests.

### <a name="client-secret"></a>Klienthemlighet
Om din app kräver inloggning med en klienthemlighet använder du det. Azure Active Directory (AAD) är ett exempel på en tjänst som erbjuder inloggning med klienthemligheter. I AAD är hello klienthemlighet hello Appkey.

Här är ett exempel på ett webbtest för en Azure-webbapp som använder en appnyckel:

![Exempel med en klienthemlighet](./media/app-insights-monitor-web-app-availability/110.png)

1. Hämta en token från AAD med hjälp av en klienthemlighet (AppKey).
2. Extrahera ägartoken från svaret.
3. Anropa API: et med ägartoken i hello authorization-huvud.

Kontrollera att hello webbtest är en verklig klient - som är den har en egen app i AAD - och använda dess clientId + appkey. Tjänsten provas också har sin egen app i AAD: hello appID URI: N för den här appen avspeglas i hello webbtest i fältet för hello ”resurser”.

### <a name="open-authentication"></a>Öppen autentisering
Ett exempel på öppen autentisering är inloggning med ett Microsoft- eller Google-konto. Många appar att Använd OAuth innebär hello klienten hemliga alternativ, så att din första skräpposten bör vara tooinvestigate möjligheter.

Om testet måste logga in med hjälp av OAuth är hello allmänna sättet:

* Använd ett verktyg som Fiddler tooexamine hello trafik mellan webbläsaren och hello autentisering plats din app.
* Utföra minst två inloggningar med olika datorer eller webbläsare, eller vid långt intervall (tooallow token tooexpire).
* Identifiera hello-token som skickas tillbaka från hello autentisera plats som skickas sedan tooyour app-servern efter inloggning genom att jämföra olika sessioner.
* Spela in ett webbtest med hjälp av Visual Studio.
* Parameterstyra hello tokens inställningen hello parametern när hello token har returnerats från hello authenticator och använda den i hello frågan toohello plats.
  (Visual Studio försöker tooparameterize hello test, men korrekt parameterstyra inte hello token.)


## <a name="performance-tests"></a>Prestandatester
Du kan köra ett inläsningstest på din webbplats. Som hello tillgänglighetstest, kan du skicka enkla begäranden eller flera steg begäranden från våra punkter runt hello world. Till skillnad från ett tillgänglighetstest skickas många begäranden, som simulerar flera samtidiga användare.

Hello översikt bladet öppna **inställningar**, **prestandatesterna**. När du skapar ett test du inbjuds tooconnect tooor skapa ett Visual Studio Team Services-konto.

När hello test har slutförts visas svarstider och slutförandefrekvenser.


![Prestandatest](./media/app-insights-monitor-web-app-availability/perf-test.png)

> [!TIP]
> Använd tooobserve hello effekterna av en prestandatest [Liveströmning](app-insights-live-stream.md) och [Profiler](app-insights-profiler.md).
>

## <a name="automation"></a>Automation
* [Använd PowerShell-skript tooset upp ett tillgänglighetstest](app-insights-powershell.md#add-an-availability-test) automatiskt.
* Konfigurera en [webhook](../monitoring-and-diagnostics/insights-webhooks-alerts.md) som anropas när en avisering genereras.

## <a name="qna"></a>Har du några frågor? Har du problem?
* *Kan jag anropa kod från mitt webbtest?*

    Nej. hello stegen i test hello måste vara i hello .webtest-filen. Och du kan inte anropa andra webbtester eller använda loopar. Men det finns flera plugin-program som kan vara användbara.
* *Stöds HTTPS?*

    Vi stöder TLS 1.1 och TLS 1.2.
* *Är det någon skillnad mellan ”webbtester” och ”tillgänglighetstester”?*

    hello kan två termer refereras synonymt. Tillgänglighetstester är en generisk term som inkluderar hello enstaka URL-ping testar dessutom toohello webbtester med flera steg.
* *Jag vill toouse tillgänglighetstester på vår interna server som kör bakom en brandvägg.*

    Det finns två möjliga lösningar:
    
    * Konfigurera din brandvägg toopermit inkommande begäranden från hello [IP-adresser i vår webbplats testa agenter](app-insights-ip-addresses.md).
    * Skriva egen kod tooperiodically testet interna servern. Köra hello kod i bakgrunden på en testserver bakom brandväggen. Testa processen kan skicka resultaten tooApplication insikter med hjälp av [TrackAvailability()](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.telemetryclient.trackavailability) API i hello core SDK-paketet. Detta kräver test server toohave utgående åtkomst toohello Application Insights införandet slutpunkten, men det är mycket mindre säkerhetsrisk än hello alternativ för att tillåta inkommande begäranden. hello resultat visas inte i hello tillgänglighet web tester blad, men visas som tillgänglighet resultaten i Analytics, Sök och mått Explorer.
* *Det går inte att överföra ett flerstegstest för webbplatser*

    Det finns en storleksgräns på 300 kB.

    Loopar stöds inte.

    Referenser tooother webbtester stöds inte.

    Datakällor stöds inte.
* *Mitt test med flera steg slutförs inte*

    Det finns en gräns på 100 förfrågningar per test.

    Stoppa hello testet om den körs längre än två minuter.
* *Hur kan jag köra ett test med klientcertifikat?*

    Det stöds tyvärr inte.


## <a name="next"></a>Nästa steg
[Sök i diagnostikloggar][diagnostic]

[Felsökning][qna]

[IP-adresser för webbtestagenter](app-insights-ip-addresses.md)

<!--Link references-->

[azure-availability]: ../insights-create-web-tests.md
[diagnostic]: app-insights-diagnostic-search.md
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md

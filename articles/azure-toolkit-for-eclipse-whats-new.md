---
title: "aaaWhat är ny i hello Azure Toolkit för Eclipse"
description: "Läs mer om hello senaste funktionerna i hello Azure Toolkit för Eclipse."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 16b066ea-aae7-4c30-9a12-fa0c3711b93e
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm;asirveda;martinsawicki
ms.openlocfilehash: d74eacfb75447a3d659a0c2dc2e247ae6e3ce1b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="whats-new-in-hello-azure-toolkit-for-eclipse"></a>Vad är nytt i hello Azure Toolkit för Eclipse
## <a name="azure-toolkit-for-eclipse-releases"></a>Azure Toolkit för Eclipse-versioner
Den här artikeln innehåller information om hello olika versioner och senaste uppdateringar toohello Azure Toolkit för Eclipse.

> [!NOTE]
> Det finns också en Azure-verktygen för hello IntelliJ IDE. Mer information finns i [Azure Toolkit för IntelliJ].
> 
> 

### <a name="april-14-2017"></a>14 april 2017
hello Azure Toolkit för Eclipse - April 2017 versionen innehåller hello följande förbättringar:

* **Förbättrad Azure logga Inloggningsupplevelse**: hello Azure Toolkit för Eclipse nu stöder två metoder för att logga in på ditt Azure-konto: *interaktiv* och *automatisk*. Mer information finns i [Azure logga i instruktioner för hello Azure Toolkit för Eclipse].
* **Publicera med hjälp av behållare med Docker**: du kan nu publicera dina webbprogram som Docker-behållare med hjälp av Azure Toolkit för Eclipse. Mer information finns i [hur toopublish ett webbprogram som använder en Dockerbehållare hello Azure Toolkit för Eclipse].
* **Konto för lagringshantering**: hello Azure Toolkit för Eclipse stöder nu hantera storage-konton från hello Azure Utforskarvy. Mer information finns i [Lagringskonton hantera med hjälp av hello Azure Explorer för Eclipse].
* **Virtual Machine Management**: hello Azure Toolkit för Eclipse stöder nu hantera dina virtuella datorer från hello Azure Utforskarvy. Mer information finns i [hantera virtuella datorer med hjälp av hello Azure Explorer för Eclipse].
* **Borttagning av Remote felsökning Support**. Fjärrfelsökning av Java-webbappar på Azure App Service har tagits bort från hello Azure Toolkit för Eclipse; Det var nödvändigt tooresolve några problem som kunder problem när du använder hello toolkit.

### <a name="august-26-2016"></a>26 augusti 2016
hello Azure Toolkit för Eclipse - versionen av augusti 2016 innehåller hello följande förbättringar:

* **Anpassade JDK distributioner**. hello Azure Toolkit för Eclipse stöder nu att ange och distribuera en godtycklig JDK version tooyour Azure WebApp-behållare:
  * Dessutom kan toohello JDKs tillhandahålls av Azure, du också välja från en mängd Zulu OpenJDK versioner som gjorts tillgängliga på Azure av Azul system.
  * Du kan också ange egna JDK-distribution om du överför den som en ZIP-filen tooyour storage-konto.
* **Förbättringar av toohello Azure Utforskarvy**:
  * Stöd för hantering av virtuell dator med Azures ny modell för hanteraren för filserverresurser: du kan visa, skapa och ta bort resource manager-baserade virtuella datorer utan att lämna hello IDE.
  * Stöd för hantering av blob Storage-konto med Azure Resource Manager som kompletterar hello befintliga funktioner för att hantera ”klassisk” storage-konton.
* **Microsoft JDBC Driver 6.0 för SQLServer**. Den här uppdateringen innehåller hello senaste JDBC-drivrutinen för Microsoft SQL Server (v6.0), som nu ingår som ett bibliotek som du kan enkelt lägga tooyour Java-projekt, vilket ersätter hello äldre version.

### <a name="june-29-2016"></a>29 juni 2016
hello Azure Toolkit för Eclipse - versionen för juni 2016 innehåller hello följande förbättringar:

* **Krav för Java 8**. hello Azure Toolkit för Eclipse kräver nu Java 8, även om det här kravet gäller endast för hello toolkit - programmen kan fortsätta toouse alla versioner av Java som stöds av Azure.
* **Stöd för hello senaste Java JDKs**. hello senaste versionerna av hello Java JDKs stöds nu av hello Azure Toolkit för Eclipse.
* **Stöd för Azure SDK v2.9.1**. hello senaste versionen av hello Azure SDK är nu hello lägsta krav för hello Azure Toolkit för Eclipse.
* **Integrerad exempel**. hello Azure Toolkit för Eclipse nu innehåller flera exempelprogram toohelp utvecklare komma igång.
* **HDInsight-verktyget Integration**. Azure HDInsight-verktyg ingår nu hello Azure Toolkit för Eclipse. Mer information finns i [HDInsight Tools-Plugin för Eclipse].
* **Fjärrfelsökning av Java-Webbappar**. hello Azure Toolkit för Eclipse stöder nu fjärrfelsökning av Java-webbappar på Azure App Service.
* **Stöd för hello Eclipse Luna versionen.** hello nya Eclipse IDE version som krävs är Luna.

### <a name="april-12-2016"></a>12 april 2016
hello Azure Toolkit för Eclipse - April 2016-versionen innehåller hello följande förbättringar:

* **Stöd för Azure SDK v2.9.0**. hello senaste versionen av hello Azure SDK är nu hello lägsta krav för hello Azure Toolkit för Eclipse.
* **Diverse förbättringar för användbarhet, tillgänglighet och prestanda relaterade tooAzure Web App support**. Ett antal prestandaoptimeringar i hur hello Toolkit kommunicerar med Azure resultatet i ett mer flexibelt gränssnitt.
* **Möjlighet toodelete en befintlig webbprogrammet behållare i Azure från i Eclipse**. hello Azure Toolkit för Eclipse kan du nu toodelete en befintlig Azure-webbehållare utan att lämna Eclipse.

### <a name="march-7-2016"></a>7 mars 2016
hello Azure Toolkit för Eclipse - versionen av mars 2016 innehåller hello följande förbättringar:

* **Stöd för snabb distribution av lightweight Java-program**. hello Azure Toolkit för Eclipse stöder nu hello snabb distribution av lightweight Java-program till Azure Web App behållare, så nu distribuera Java-program tar några sekunder i stället för minuter.
* **Stöd för Web App-hantering med hello Azure Utforskarvy**. hello Azure Utforskarvy i hello toolkit stöder nu för att lista, starta och stoppa Azure Web Apps.
* **Uppdatera Tomcat och Jetty Zulu OpenJDK distributioner**. hello Azure Toolkit för Eclipse ger stöd för uppdaterade versioner av Tomcat, Jetty och Zulu OpenJDK för Java-distributioner i Azure-molntjänster.

### <a name="january-4-2016"></a>4 januari 2016
hello Azure Toolkit för Eclipse - januari 2016-versionen innehåller hello följande förbättringar:

* **Stöd för hello Zulu OpenJDK uppdaterar**. Mer information finns i hello [Azul system webbsida för hello Zulu OpenJDK].
* **Uppdatera Tomcat- och Jetty-distributioner**. Hej Jetty och Tomcat-distributioner som är tillgängliga på Microsoft Azure för användning med hello Azure Toolkit för Eclipse har uppdaterats.
* **Har paritet mellan Eclipse och IntelliJ verktyg för Azure**. hello Azure Toolkit för Eclipse och hello [Azure Toolkit för IntelliJ] stöder nu hello samma uppsättning funktioner.

### <a name="september-1-2015"></a>Den 1 september 2015
hello Azure Toolkit för Eclipse - utgåvan från September 2015 innehåller hello följande förbättringar:

* **Stöd för hello Zulu OpenJDK uppdaterar**. Mer information finns i hello [Azul system webbsida för hello Zulu OpenJDK].
* **Uppdatera Tomcat- och Jetty-distributioner**. Hej Jetty och Tomcat-distributioner som är tillgängliga på Microsoft Azure för användning med hello Azure Toolkit för Eclipse har uppdaterats. (Dessa distributioner kan utvecklare toocreate snabb utveckling och testning projekt med hello Azure Toolkit för Eclipse.
* **Stöd för automatiskt uppdaterade referenser till Tomcat- och Jetty**. Tillägg toohello specifika versioner av Tomcat- och Jetty som är tillgängliga på Azure, utvecklare kan nu använda referera till en distribution som anges tooas hello ”senaste (automatisk uppdaterade)”, som uppdateras automatiskt toohello senaste distribution av varje större version av Jetty eller Tomcat hello nästa gång som dina rollinstanser har återvunnits. (Återanvändning sker automatiskt, men utvecklare kan utlösa en begäran om återvinning via hello Azure-portalen manuellt.) Den här nya funktionen innebär att utvecklare inte har tooredeploy sina program toobe kan toohave sina serverprogramvara uppdateras. (
* Den här funktionen är för närvarande endast är avsett för utveckling och testning syfte eller icke-kritiska program, och rekommenderas inte för produktion.)
* **Azure Utforskarvy för BLOB, köer och tabeller i Azure storage**. Detta gör att utvecklare tooperform en uppsättning vanliga aktiviteter med deras lagring artefakter direkt från hello Eclipse IDE. Exempel: Om du tar bort, överföra eller hämta blobbar.

### <a name="august-1-2015"></a>1 augusti 2015
hello Azure Toolkit för Eclipse - augusti 2015-versionen innehåller hello följande förbättringar:

* **Application Insights Instrumentation nyckelhantering**. Den här uppdateringen kan du tooacquire, skapa och hantera dina Application Insights instrumentation nycklar direkt från hello Eclipse IDE.
* **Microsoft JDBC Driver 4.1 för SQLServer**. Den här uppdateringen innehåller stöd för hello senaste JDBC-drivrutinen för Microsoft SQL Server.
* **Hello Azure SDK version 2.7**. Den här senaste uppdatering toohello Azure SDK är hello nya krav för hello Toolkit när installeras i Windows. (Observera detta inte behövs på Windows-operativsystem.)
* **Stöd för hello Zulu OpenJDK v7 uppdatering**. Mer information finns i hello [Azul system webbsida för hello Zulu OpenJDK].

### <a name="may-1-2015"></a>1 maj 2015
hello Azure Toolkit för Eclipse - maj 2015-versionen innehåller hello följande förbättringar:

* **Bättre val av Server UI**. Den här versionen förenklar hello användning av hello toolkit på Windows-operativsystem.
* **Stöd för Maven-projekt**. Den här versionen stöder Maven-projekt som program, vilka hello toolkit kan distribuera tooAzure och konfigurera Application Insights.
* **Hello Azure SDK version 2.6**. Den här senaste uppdatering toohello Azure SDK är hello nya krav för hello Toolkit när installeras i Windows. (Observera detta inte behövs på Windows-operativsystem.)
* **Uppgradering av distributionen i stället för att publicera**. Om du publicerar ett distributionsprojekt när hello tidigare version redan är live använder nu Azures distribution uppgraderingsfunktionen i stället för att stänga av hello tidigare distribution och publicera från början, som i tidigare hello hello toolkit. Detta gör att din cloud service toorun utan avbrott när det är möjligt att hjälpa att uppnå hög tillgänglighet även under en uppdatering och snabbare hello nytt publiceringsprocessen.
* **Stöd för hello senaste Zulu OpenJDK v8 - uppdatera 40**. Mer information finns i hello [Azul system webbsida för hello Zulu OpenJDK].

### <a name="march-9-2015"></a>9 mars 2015
hello Azure Toolkit för Eclipse - mars 2015-versionen innehåller hello följande förbättringar:

* **Stöd för Mac, Ubuntu och ytterligare Linux varianter**. Den här versionen av hello Azure Toolkit för Eclipse lägger till stöd för flera Unix-plattformar och Mac OS x, så att utvecklare kan installera hello toolkit toocreate, konfigurera och publicera Java-projekt tooAzure molntjänster (PaaS) från Eclipse som körs på operativsystem andra än Windows.

> [!NOTE]
> Den här funktionen är i förhandsvisning och det rekommenderas inte för användning i produktionsmiljöer. Det finns inget stöd för kunden serviceavtalet (SLA), men alla feedback uppskattar och uppmuntras.
> 
> 

* **Ny Application Insights-plugin-programmet**. Utvecklare kan nu kan tooconfigure automatisk telemetri med Application Insights på Azure.
* **Kommandoraden Ant-baserad distribution automation**. Den här funktionen gör det möjligt för utvecklare tooautomate hello publicering för nyare versioner av sina distributioner med hjälp av Ant utanför Eclipse. Ett förgenererade skript konfigureras automatiskt för ett projekt när hello första gången den distribueras från Eclipse och efterföljande distributioner kan använda hello skriptet toofully automatisera distributioner via hello kommandoraden endast.
* **Tomcat- och Jetty tillgänglighet på Azure för enklare och snabbare distribution**. Utvecklare kan nu referera till olika Tomcat och Jetty-versioner som är tillgängliga på Azure direkt i stället för att tooupload tootheir för Java serverkonton (eller via hello Toolkit) så det finns inga måste tooupload en Java-server för snabbt, komma igång-scenarier.
* **Kortkommandon för publicering av Java web apps tooAzure molntjänster**. tooreduce hello inlärningskurvan för enkla scenarier för utveckling och testning, utvecklare kan nu publicera Java-program mer direkt tooAzure. I stället för att toogo via hello processen att skapa och konfigurera en Azure distributionsprojekt distribueras program med en standardinstans av Tomcat v8 och Zulu JVM (OpenJDK).

### <a name="january-30-2015"></a>30 januari 2015
hello Azure Toolkit för Eclipse - januari 2015-versionen innehåller hello följande förbättringar:

* **Stöd för IBM® WebSphere® programmet Server Liberty Core**. Den här versionen lägger till hello IBM WebSphere programmet Server Liberty Core toohello lista över programservrar som stöds från vilka hello toolkit är kan toodeploy tooAzure. Det här senaste tillägget expanderar hello aktuella listan över programservrar som stöds &quot;out box&quot; som hello Toolkit, som redan med olika versioner av Tomcat, Jetty, JBoss och GlassFish.
* **Inkludering av Application Insights SDK**. Nyligen utgivna API klientbiblioteket (v0.9.0) är nu en del av hello paketet för Azure-biblioteken för Java.
* **Uppdaterat paketet för Azure-bibliotek för Java**. Den här uppdateringen innehåller Azure-bibliotek för Java v0.7.0 och Storage klienten API v2.0.0 samt hello nyligen utgivna Application Insights SDK v0.9.0.

### <a name="november-12-2014"></a>12 november 2014
hello Azure Toolkit för Eclipse - November 2014-versionen innehåller hello följande förbättringar:

* **Stöd för Azure SDK 2,5**. Den här senaste uppdatering toohello Azure SDK är hello nya krav för hello Toolkit.
* **Stöd för uppdaterade versionen av hello Zulu OpenJDK v1.8, v1.7 och 1.6 paket**. Mer information finns i hello [Azul system webbsida för hello Zulu OpenJDK].
* **Stöd för hello nya Standard D storlekar för molntjänster**, som erbjuder bättre prestanda och ytterligare minnesresurser. Mer information finns i [virtuell dator och Molntjänststorlekar för Azure].

### <a name="october-17-2014"></a>17 oktober 2014
hello Azure Toolkit för Eclipse - oktober 2014-versionen innehåller hello följande förbättringar:

* **Prestandaförbättringar i hello publicera tooCloud scenarier**. Inläsning av prenumerationsinformation är mycket snabbare när användarna har flera prenumerationer och storage-konton.
* **Stöd för uppdaterade versionen av hello Zulu OpenJDK v1.8 paketet**. Mer information finns i hello [Azul system webbsida för hello Zulu OpenJDK].
* **Stöd för äldre versioner av 3 part JDKs sluta**. Föråldrad JDK paket visas inte längre i hello nedrullningsbara menyn för nya projekt för distribution. Befintliga projekt som refererar till föråldrad JDK paket fortsätter som kan toodo så hello tid som, men det rekommenderas för tooupgrade sådana projekt toorely på hello senaste.
* **Uppdaterad version av hello paketet för Azure-bibliotek för Java-klientbiblioteket API**. Mer information finns i hello [API: T för Microsoft Azure-klient].
* **Felkorrigeringar.** Den här versionen innehåller ett antal olika felkorrigeringar som baserades på Användarrapporter och testning.

### <a name="august-5-2014"></a>5 augusti 2014
hello Azure Toolkit för Eclipse - augusti 2014-versionen innehåller följande förbättringar hello

* **Stöd för Azure SDK 2.4.** Äldre versioner av hello Eclipse Toolkit fungerar inte med den här nyligen utgivna SDK.
* **Uppdaterade versioner av hello Zulu OpenJDK 1.6, 1.7 och v1.8 paket.** Mer information finns i hello [Azul system webbsida för hello Zulu OpenJDK].
* **Uppdaterad version av hello paketet för Azure-bibliotek för Java API klientbiblioteket.** Mer information finns i hello [API: T för Microsoft Azure-klient].
* **Stöd för senaste publicera inställningar filformat.** Stöd har lagts till för version 2.0 av hello Publiceringsinställningar filformat.
* **Ändringarna av arkitekturen bakom hello publicera tooCloud funktion.** hello Toolkit är nu med hello nyligen publicerat Microsoft Azure-klient-API för Java för stödet publicera till molnet.
* **Felkorrigeringar.** Den här versionen innehåller ett antal användaren begärde felkorrigeringar.

### <a name="june-12-2014"></a>12 juni 2014
hello Azure Toolkit för Eclipse - juni 2014-versionen är en mindre underhåll uppdatering som innehåller hello följande förbättringar:

* **Stöd för hello Zulu OpenJDK paketet v1.8.** Mer information finns i hello [Azul system webbsida för hello Zulu OpenJDK].
* **Uppdaterade versioner av hello Zulu OpenJDK 1.6 och 1.7 paket.** Mer information finns i hello [Azul system webbsida för hello Zulu OpenJDK].
* **Uppdaterad version av hello paketet för Azure-bibliotek för Java API klientbiblioteket.** Mer information finns i hello [API: T för Microsoft Azure-klient].
* **Felkorrigeringar.** Den här versionen innehåller ett antal användaren begärde felkorrigeringar.

### <a name="april-4-2014"></a>4 april 2014
hello Azure plugin-program för Eclipse - April 2014-versionen har publicerat. Detta är en uppdatering som medföljer hello-versionen av hello Azure SDK 2.3, vilket är ett krav och kommer att hämtas automatiskt när du installerar hello plugin-programmet. Den här uppdateringen innehåller nya funktioner, felkorrigeringar och vissa feedback-driven förbättringar sedan hello februari 2014 förhandsgranska:

* **Stöd för hello Azure SDK 2.3 versionen.** hello Azure plugin-program för Eclipse - April 2014-versionen kräver Azure SDK 2.3. När du använder hello nytt plugin-program, om du inte redan har Azure SDK 2.3, kommer du att ange tooallow installationen. Använd inte Azure SDK 2.3 med tidigare versioner av hello plugin-programmet.
* **Uppgradering av program utan fullständig paketdistributionen.** När du distribuerar Java-program som ingår i ditt projekt överför hello plugin-programmet nu automatiskt dem till valda storage-konto så att du kan uppdatera den och Återanvänd hello rollen instanser toodeploy hello senaste programmet bits utan toorebuild och distribuera om hello hela paketet.
* **Tomcat 8 nu är en känd programserver.** Om du väljer en Tomcat 8 installationskatalogen på din dator i hello **Server** för hello **projekt för distribution av Azure** dialogrutan hello plugin-programmet kommer nu automatiskt identifieras och vara kan toodeploy Tomcat 8 i obevakat läge, liknande toohello äldre versioner av Tomcat redan i hello-listan.
* **Azul Zulu OpenJDK paketet uppdateringar: uppdatera 47 v1.7 update 51 och 1.6.** Effektiva med den här versionen, Azul System Zulu öppna JDK v7 Paketuppdatering 51 är tillgänglig. Dessutom starta Zulu öppna JDK v6-paket är tillgängliga från och med uppdatering 47. De här uppdateringarna är dessutom toohello tidigare Zulu öppna JDK v7 Paketuppdatering 45, uppdatera 40 och uppdatera 25.
* **Stöd för A8 och A9 Microsoft Azure-dator storlek.** Du kan nu distribuera en cloud service toohello RAM-minne A8 och A9 virtuella datorstorlekar. Mer information om dessa VM-storlekar finns [virtuell dator och Molntjänststorlekar för Azure].
* **Automatisk omdirigering från http-tooHTTPS för SSL-aktiverade roller.** När Molntjänsten innehåller endast HTTPS-roller om hello användarbegäran anger HTTP, omdirigeras automatiskt tooHTTPS. Det finns inget behov av toocreate en separat rollen toohandle hello HTTP-begäranden.
* **Express emulatorn som används för lokal emulering.** hello Azure Express emulatorn används nu som hello emulatorn vid felsökning av dina program lokalt.
* **Azure har tagits byta namn som Microsoft Azure.** Användargränssnittet skärmar avspeglar nu att Azure har byta namn och inte längre kallas Azure.

### <a name="february-6-2014"></a>6 februari 2014
hello Azure plugin-programmet för Eclipse - februari 2014 Preview har publicerat. Den här uppdateringen innehåller nya funktioner, felkorrigeringar och vissa feedback-driven förbättringar sedan hello oktober 2013 Preview:

* **Stöd för SSL-avlastning.** Avlastning av Secure Sockets Layer (SSL) har lagts till som en funktion så att du tooeasily aktivera Hypertext Transfer Protocol Secure (HTTPS) som stöder i Java-distributionen på Azure, utan att du tooconfigure SSL i Java-programservern. Detta är särskilt relevant i sessionen tillhörighet och/eller autentiserad kommunikationsscenarier. Till exempel när du använder hello Access Control Service (ACS) filtrera, som redan stöds av hello toolkit. Mer information finns i [SSL genom att avlasta] och [hur tooUse SSL genom att avlasta].
* **GlassFish 4 nu är en känd programserver.** Om du väljer en GlassFish 4 installationskatalogen på din dator i hello **Server** för hello **projekt för distribution av Azure** dialogrutan hello plugin-programmet kommer nu automatiskt identifieras och vara kan toodeploy GlassFish OSE 4 i obevakat läge, liknande toohello GlassFish OSE 3 version redan i hello-listan.
* **Azul Zulu OpenJDK Paketuppdatering 45.** Effektiva med den här versionen, Azul System Zulu (Öppna JDK v7-paket) uppdatering 45 är nu tillgängliga. Detta är dessutom toohello tidigare uppdatering 40 och uppdatera 25.
* **Stöd för 'auto' för privat slutpunktsportar.** Du kan ange en privat port tooautomatic för inkommande slutpunkter och interna slutpunkter toolet Azure tilldela automatiskt en port toothat slutpunkt. Tidigare kunde du bara tilldela ett visst portnummer.
* **Stöd för att anpassa hello certifikatets namn (CN) i hello självsignerade certifikat skapa UI.** Tidigare hello samma hårdkodade namn används för alla nya certifikat; Nu kan du ange egna certifikat namn toohelp särskilja flera certifikat i hello Azure portal som används för olika ändamål.
* **Azure verktygsfältet:** hello Azure verktygsfältet har en uppdaterad med hello följande ändringar: 
  * ![][ic710876]Den här ikonen har lagts till för hello **nya projekt för distribution av Azure**.
  * ![][ic710877]Den här ikonen har lagts till som en genväg toohello självsignerat certifikat skapas dialogruta.
* **Stöd för A5 Azure virtuella datorstorleken.** Du kan nu distribuera en cloud service toohello RAM-minne A5 virtuella datorstorleken. Mer information om storleken på den här virtuella datorn finns [virtuell dator och Molntjänststorlekar för Azure].
* **Stöd för Microsoft Windows Server 2012 R2.** Nu kan du välja Windows Server 2012 R2 som hello moln-operativsystem.

### <a name="october-22-2013"></a>Den 22 oktober 2013
hello Azure plugin-programmet för Eclipse - oktober 2013 Preview har publicerat. Den här uppdateringen innehåller nya funktioner, felkorrigeringar och vissa feedback-driven förbättringar sedan hello September 2013 Preview:

* **Stöd för hello Azure SDK 2.2 versionen.** hello Förhandsgranska Azure plugin-program för Eclipse - oktober 2013 stöder Azure SDK 2.2. hello-plugin-programmet fortfarande fungerar med Azure SDK 2.1 och installerar automatiskt Azure SDK 2.2 om du inte redan har minst Azure SDK 2.1 installerad.
* **Azul Zulu OpenJDK Paketuppdatering 40.** Som presenterades för hello September 2013 Preview hello plugin-programmet nu gör det möjligt med hjälp av en tredjeparts-tillhandahållna JDK direkt på Azure, utan att du tooupload egna JDK. I hello oktober 2013 release är Azul System Zulu (Öppna JDK v7-paket) uppdateringen 40 tillgängliga. Detta är i tillägg toohello som publicerats ursprungligen uppdatera 25.
* **Cloud deployment länken i hello aktivitetsloggen.** Inom hello Azure-aktivitetsloggen, när distributionen har statusen **publicerade**, kan du klicka på **publicerade** eftersom det är nu en länk tooyour distribution; distributionen kommer sedan att öppnas i webbläsaren. (hello status för **publicerade** tidigare var märkta **kör**.)
* **Mål-operativsystem finns på Publicera tid.** Hej **publicera tooAzure** dialogrutan innehåller ett nytt fält **mål OS**, vilket möjliggör mer synliga sätt för du tooset för ditt måloperativsystem.
* **Automatiskt skriva över tidigare distribution.** Hej **publicera tooAzure** dialogrutan innehåller en ny kryssruta **skriva över tidigare distribution**. Om det här alternativet är markerat när distributionen av nya publiceras automatiskt skrivs tidigare hello-distribution Du kan inte ha &quot;409 konflikt&quot; problem när du publicerar toohello samma plats utan första Avpublicerar hello tidigare distribution.
* **Jetty 9 nu är en känd programserver.** Om du väljer en Jetty 9 installationskatalogen på din dator i hello **Server** för hello **projekt för distribution av Azure** dialogrutan hello plugin-programmet kommer nu automatiskt identifieras och vara kan toodeploy Jetty 9 i ett automatiserat sätt liknande toohello äldre versioner av Jetty redan i hello-listan.
* **Lägg till en roll hello snabbmenyn för projektet.** Hej **Azure** snabbmenyn för projektet innehåller nu ett nytt menyalternativ **Lägg till roll**, vilket möjliggör ett snabbare och mer identifierbart sätt för tooadd du en ny roll tooyour Azure-projekt.
* **En uppdatering toohello paketet för hello Azure-bibliotek för Java-bibliotek.** Detta baseras på hello-version 0.4.6 [API: T för Microsoft Azure-klient].

### <a name="september-25-2013"></a>25 september 2013
hello Azure plugin-programmet för Eclipse - September 2013 Preview har publicerat. Den här uppdateringen innehåller nya funktioner, felkorrigeringar och vissa feedback-driven förbättringar sedan hello augusti 2013 Preview:

* **Möjlighet toodeploy hello Azul Zulu OpenJDK paketet finns på Azure.** Ett nytt alternativ har lagts till när du anger hello JDK toouse med din Azure-distribution. Med det här alternativet kan du distribuera en från tredje part JDK paket direkt på hello Azure-molnet utan att behöva tooupload egna. Azul system tillhandahåller hello först sådana paket kallas Zulu, baserat på hello OpenJDK som kan nu distribueras med hjälp av det här alternativet.
* **En uppdatering toohello paketet för hello Azure-bibliotek för Java-bibliotek.** Detta baseras på hello-version 0.4.5 [API: T för Microsoft Azure-klient].

### <a name="august-1-2013"></a>1 augusti 2013
hello Azure plugin-programmet för Eclipse - augusti 2013 Preview har publicerat. Detta är en uppdatering som medföljer hello-versionen av hello Azure SDK 2.1 som är ett krav och kommer att hämtas automatiskt när du installerar hello plugin-programmet. Den här uppdateringen innehåller nya funktioner, felkorrigeringar och vissa feedback-driven förbättringar sedan hello juli 2013 Preview:

* **Borttagning av alternativ tooinclude hello lokala JDK och lokala programserver som en del av hello distributionspaketet.** Det är bättre tooembedding komponenterna i hello paketet, sedan hämta hello objekt resultat i mindre distribution paketets storlek, snabbare distribution gånger, och enklare att hämta hello JDK och programserver från lagringsutrymmet i molnet under distributionen av hello Underhåll. Därför har hello alternativ tooinclude hello JDK och programserver i hello distributionspaketet tagits bort. Befintliga projekt som var konfigurerad tooinclude hello lokala JDK och lokala programserver som en del av hello distributionspaketet kommer automatiskt att konverterade tooauto-överföringen hello JDK och programmet server toocloud lagring.
* **Stöd för hello Azure SDK 2.1-versionen.** hello Azure plugin-programmet för Eclipse - augusti 2013 Preview kräver Azure SDK 2.1. Använd inte hello augusti 2013 preview med tidigare versioner av hello Azure SDK och Använd inte Azure SDK 2.1 med tidigare versioner av hello Azure-plugin-programmet för Eclipse.
* **Stöd för hello Eclipse Kepler versionen.** Relaterade toothis hello nya minimikravet Eclipse IDE version är indigoblå. hello Azure plugin-program för Eclipse testas inte längre officiellt på Helios.

### <a name="july-3-2013"></a>3 juli 2013
hello Azure plugin-programmet för Eclipse - juli 2013 Preview har publicerat. Den här uppdateringen innehåller nya funktioner, felkorrigeringar och vissa feedback-driven förbättringar eftersom hello kan 2013 förhandsgranska:

* **Möjlighet toocreate ett nytt lagringskonto.** En **ny** knappen har lagts toohello **lägga till Lagringskontot** dialogrutan. Detta ger dig toocreate ett lagringskonto i hello Eclipse plugin-program, utan att du toolog i toohello Azure-hanteringsportalen. (Du måste redan ha en Azure-prenumeration toouse den här funktionen.) Mer information om hur du skapar ett nytt lagringskonto finns [toocreate ett nytt lagringskonto].
* **Nya &quot;(auto)&quot; alternativ för storage-konto som används för automatisk distribution av JDK och servern och för cachelagring.** När du använder hello **automatiskt överför** alternativ för hello JDK och programserver, kan du nu ange **(auto)** för hello URL och storage-konto toouse när överföring hello JDK och programserver eller när du använder Azure Caching. Sedan använder automatiskt de här funktionerna hello samma lagringskonto som hello något som du väljer i hello **publicera tooAzure** dialogrutan. Hej [skapar en Hello World-program för Azure i Eclipse] kursen har uppdaterats toouse hello nya **(auto)** alternativet.
* **Möjlighet tooset Azure-Tjänsteslutpunkter.** Ange hello slutpunkter som avgör om ditt program är distribuerade tooand som hanteras av hello globala Azure-plattformen, Azure som drivs av 21Vianet i Kina eller en privat Azure-plattformen. Mer information finns i [Azure Tjänsteslutpunkter].
* **Stora distributioner kan ange en resurs för lokal lagring.** I hello-händelse som distributionen är för stor toobe i hello standardmappen approot nu kan du ange en resurs för lokal lagring som hello distribution mål för din JDK och programservrar. Mer information finns i [distribuera stora distributioner].
* **Stöd för A6 och A7 Azure-dator.** Du kan nu distribuera en cloud service toohello RAM-minne A6 och A7 virtuella datorstorlekar. Mer information om dessa storlekar finns [virtuell dator och Molntjänststorlekar för Azure].
* **En uppdatering toohello paketet för hello Azure-bibliotek för Java-bibliotek.** Detta baseras på hello-version 0.4.4 [API: T för Microsoft Azure-klient].

### <a name="may-1-2013"></a>1 maj 2013
hello Azure plugin-program för Eclipse - kan 2013 Preview har publicerat. Detta är en större uppdatering som medföljer hello-versionen av hello Azure SDK 2.0, vilket är ett krav och kommer att hämtas automatiskt när du installerar hello plugin-programmet. Den här versionen innehåller nya funktioner, felkorrigeringar och vissa feedback-driven förbättringar sedan hello februari 2013 Preview:

* **Automatisk överföring av hello JDK och programserver och distributionen från Azure storage.** Ett nytt alternativ som överför automatiskt Hej valda JDK och programserver, vid behov, tooa angetts Azure storage-konto och distribuerar dessa komponenter finns i stället för att bädda in i hello distributionspaketet eller med hello användaren överför sedan manuellt. Funktionen efterfrågade kan öka hello enkel distribution hello JDK- och serverkomponenter, särskilt för nybörjare. En genomgång som använder de här alternativen finns i [skapar en Hello World-program för Azure i Eclipse].
* **Centraliserad storage-konto spårning och möjlighet tooreference lagringskonton enklare (via en dropdown-kontroll).** Detta gäller toomultiple funktioner som är beroende av lagring, till exempel JDK och distribution av server-komponent och cachelagring. Mer information finns i [Azure Lagringskontolistan].
* **Förenklad fjärråtkomst-installationen i hello publicera tooCloud guiden.** Allt du behöver toodo är att ange ett användarnamn och lösenord tooenable fjärråtkomst, eller lämna den tomma tookeep fjärråtkomst har inaktiverats.
* **En uppdatering toohello paketet för hello Azure-bibliotek för Java-bibliotek.** Detta baseras på hello-version 0.4.2 [API: T för Microsoft Azure-klient].
* **Stöd för Fäst sessioner på Windows Server 2012.** Fäst sessioner arbetat tidigare endast på Windows Server 2008 R2 nu både molnet operativsystemet mål stöd session tillhörighet.
* **Prestandaförbättringar för överföringen av paketet.** Även om hello JDK och programserver är inbäddade i hello distributionspaketet, kan hello Överför delen av distributionsprocessen hello vara ungefär dubbelt så snabbt som jämfört med tooprevious versioner.

### <a name="february-8-2013"></a>8 februari 2013
hello Azure plugin-programmet för Eclipse - februari 2013 Preview har publicerat. Detta är en mindre uppdatering som innehåller felkorrigeringar, feedback-driven förbättringar och vissa nya funktioner sedan hello November 2012 förhandsgranska:

* Stöd för att distribuera JDKs programservrar och valfri andra komponenter från offentligt eller privat Azure blob storage hämtningsbara filer i stället för att inkludera dem i distributionspaket hello när du distribuerar toohello moln.
* Möjlighet toochange hello ordning där användardefinierade komponenter i en roll bearbetas via hello tillägg av **Flytta upp** och **Flytta ned** knapparna i hello **komponenter** avsnitt i hello **Azure rollegenskaper**.
* En uppdatering toohello **paketet för hello Azure-biblioteken för Java** bibliotek, baserat på version 0.4.0 av hello [API: T för Microsoft Azure-klient].

### <a name="november-5-2012"></a>5 november 2012
hello Azure plugin-programmet för Eclipse - November 2012 Preview har publicerat. Detta är en större uppdatering som innehåller ett antal nya funktioner, samt ytterligare felkorrigeringar och feedback-driven förbättringar sedan hello September 2012 förhandsgranska:

* Stöd för Microsoft Windows Server 2012 som hello moln-operativsystem.
* Stöd för Azure cachelagring samordnade stöd för klienter som memcached.
* Inkludering av hello Apache Qpid JMS klientbibliotek för att utnyttja Azure AMQP-baserade meddelanden.
* En bättre **nytt projekt** guiden med en ny sida hello slutet som ger användare med hello möjlighet tooquickly aktivera flera vanliga viktiga funktioner i projektet: Fäst sessioner, cachelagring och fjärrfelsökning.
* Automatisk minskning av rollen instanser too1 vid körning i hello beräkningsemulatorn, tooavoid port bindningskonflikter mellan server-instanser.

### <a name="september-28-2012"></a>28 september 2012
hello Azure plugin-programmet för Eclipse - September 2012 Preview har publicerat. Uppdateringen innehåller ett antal ytterligare felkorrigeringar sedan hello augusti 2012 Förhandsgranska samt vissa feedback-driven förbättringar i befintliga funktioner:

* Stöd för Microsoft Windows 8 och Microsoft Windows Server 2012 som hello operativsystemet för utveckling och lösa problem som tidigare har förhindrat hello plugin-programmet fungerar korrekt på dessa operativsystem.
* Förbättrat stöd för att ange endpoint-portintervall.
* Felkorrigeringar relaterade toofile sökvägar som innehåller blanksteg.
* Rollen kontexten menyn förbättringar för snabbare åtkomstinställningar toorole konfiguration.
* Lägre förfining i hello **publicera toocloud** guiden och ett antal ytterligare felkorrigeringar.

### <a name="august-28-2012"></a>28 augusti 2012
hello Azure plugin-programmet för Eclipse - augusti 2012 Preview har publicerat. Uppdateringen innehåller ytterligare felkorrigeringar sedan hello juli 2012 Förhandsgranska samt flera förbättringar av användbarheten för feedback-driven för befintliga funktioner:

* I dialogrutan för hello Azure Access Control tjänstfilter:
  * **Alternativet tooembed hello signeringscertifikat** i ditt program WAR-filen toosimplify molnet distribution.
  * **Alternativet toocreate ett självsignerat certifikat** inom hello ACS filtrera Användargränssnittet. Mer information om hello Azure Access Control tjänstfilter finns [hur tooAuthenticate webbanvändare med Azure Access Control Service med Eclipse].
* Guiden för hello projekt för distribution av Azure (gäller även toohello roll serverkonfiguration egenskapssidan):
  * **Automatisk identifiering av hello JDK plats** på din dator (som du kan åsidosätta om du vill).
  * **Automatisk identifiering av hello servertyp** när du väljer hello application server-installationskatalogen.

### <a name="july-15-2012"></a>15 juli 2012
hello Azure plugin-programmet för Eclipse - juli 2012 Preview som åtgärdar ett antal hello högsta prioritet buggar hitta och/eller rapporteras av användare efter hello juni 2012-versionen har publicerat. Detta är endast en tjänstuppdatering, inga nya funktioner ingår.

### <a name="june-7-2012"></a>7 juni 2012
Azure-plugin-programmet för Eclipse - juni 2012 CTP har publicerat. Nya funktioner är:

* **Guiden Nytt projekt för distribution av Azure:** aktiverar du tooselect din JDK Java-programservern och Java-program direkt i hello förbättrad guiden Användargränssnittet. I hello lista över out box server konfigurationer toochoose från ingår Tomcat 6, Tomcat 7, GlassFish OSE 3, Jetty 7, 8 Jetty, JBoss 6 och JBoss 7 (fristående). Du kan dessutom anpassa hello lista över serverkonfigurationer. Denna förbättring i Användargränssnittet är en alternativ toodragging och släppa komprimerade filer och kopiera över startskript som var tidigare hello main-metoden. Metoden kan du fortfarande fungerar, men troligen kommer att användas endast för mer avancerade scenarier.
* **Konfigurationen egenskapen serverrollsida:** kan du tooeasily växel hello JDKs, Java-programservrar och program som är associerade med din distribution när du har skapat hello-projekt. Mer information finns i [Server konfigurationsegenskaper].
* **&quot;Publicera toocloud&quot; guiden:** ger ett enkelt sätt toodeploy ditt projekt tooAzure direkt från Eclipse automatiskt hello tidigare manuell frekventa lyfta för hämtning av autentiseringsuppgifter som loggar in toohello Azure-hanteringsportalen ladda upp ett paket, osv. Ett exempel på hur toodirectly distribuera ditt projekt tooAzure, se [skapar en Hello World-program för Azure i Eclipse].
* **Azure verktygsfältet:** Azure verktygsfältet är nu tillgänglig i Eclipse som innehåller knappar som anropar hello följande funktioner:
  * ![][ic710879]**Körs i Azure-emulatorn**: kör projektet i hello-emulatorn.
  * ![][ic710880]**Återställa Azure-emulatorn**: återställer hello emulatorn.
  * ![][ic710881]**Skapa moln paketet för Azure**: kompilerar ditt paket för distribution.
  * ![][ic710876]**Nya projekt för distribution av Azure**: skapar ett nytt projekt i Azure-distribution.
  * ![][ic710882]**Publicera tooAzure moln**: publicerar ditt projekt tooAzure.
  * ![][ic710883]**Upphäv publicering**: tar bort din distribution.
  * Många av dessa Azure knappar som används i [skapar en Hello World-program för Azure i Eclipse].
* **Azure-bibliotek för Java:** nu tillgängligt som en del av hello enkel paketet för Azure-bibliotek för Java-biblioteket i Eclipse tillhörande hello plugin-programmet för installation och som innehåller alla hello samt nödvändiga beroenden. Lägg till en referens toohello bibliotek i projektet Java och du behöver inte toodownload något separat. Mer information finns i [installerar hello Azure Toolkit för Eclipse].
* **Microsoft JDBC-drivrutin 4.0 för SQL Server som är tillgängliga under installationen av plugin-program:** under installationen av hello nya plugin-programmet hello senaste versionen av hello Microsoft JDBC-drivrutinen för SQL Server kan installeras.
* **Azure Access Control tjänstfilter tillgängliga under installationen av plugin-program:** komponenten kan inkluderas som en Eclipse-bibliotek i hello toolkit gör det möjligt för din Java web application tooseamlessly dra nytta av Azure Access Control Service (ACS) autentisering med hjälp av olika leverantörer, till exempel Google, Live.com och Yahoo!. Du behöver inte toowrite autentiseringslogiken själv, bara konfigurera några alternativ och låta hello filter gör hello tunga lyfta för att aktivera användare toosign i med hjälp av ACS. Du kan bara fokusera på skriver hello-kod som ger användare åtkomst tooresources baserat på sin identitet, som returneras tooyour program av hello filter i hello Request-objektet. En självstudiekurs om hur du använder hello ACS filtrera, se [hur tooAuthenticate webbanvändare med Azure Access Control Service med Eclipse].
* **Automatisk identifiering av hello Azure SDK 1.7 förutsättning:** när du skapar ett nytt Azure-projekt för distribution av Azure SDK 1.7 hämtas automatiskt om den inte redan är installerad.
* **Instansen slutpunkter:** tillåter direkt port endpoint åtkomst för kommunikation med belastning belastningsutjämnade rollinstanser. Instansslutpunkter kan läggas till via hello slutpunkter UI, tillgängliga via hello [slutpunkter egenskaper] sidan. Detta hjälper till att aktivera fjärrfelsökning och JMX diagnostik för specifika compute-instanser som körs i hello moln i scenarier med multi-instansen distributioner. 
* **Komponenter UI:** gör det enklare för avancerade användare tooset in projektberoenden mellan enskilda Azure roller i hello-projektet och andra externa resurser, till exempel Java-programprojekt; gör det också enkelt toodescribe sin logik för distribution . Mer information finns i [komponenter egenskaper].
* **Automatisk uppgradering av tidigare versioner av projekt:** när du öppnar en arbetsyta som har Azure projektet har skapats med en tidigare version av hello plugin-programmet hello gamla projekt som ska visas i Eclipse som stängd, eftersom tidigare versioner av projekt inte kompatibel med hello ny utgåva. Om du försöker tooopen något av dessa gamla projekt, startar en Uppgraderingsguide. Om du godkänner toohello uppgradering kan ett nytt projekt **_Upgraded** tillagda toohello namn, kommer att skapas och uppdateras automatiskt toowork med hello ny utgåva. Du kan byta namn på hello nytt projekt efter behov. Som en del av hello uppgraderingen, det ursprungliga projektet ändras inte (och förblir stängd).

### <a name="december-10-2011"></a>10 december 2011
Azure-plugin-programmet för Eclipse - December 2011 CTP har publicerat. Nya funktioner är:

* **Sessionen tillhörighet (&quot;Fäst sessioner&quot;) stöder:** hjälpa aktivera stateful, klustrade Java-program med bara en enda kryssruta. Mer information finns i [Session tillhörighet].
* **Färdiga Start skriptexempel:** för hello populäraste Java-servrar (Tomcat, Jetty, JBoss, GlassFish) som du kan bara kopiera och klistra in från katalogen samples för ditt projekt i startskript.
* **Emulatorn Start utdata i realtid:** kan du nu titta på hello körningen av alla hello steg från din startskriptet i en dedikerad konsolfönstret visar du hello förlopp och fel i ditt skript som körs i Azure.
* **Automatisk, inaktiverad java.exe övervakning:** som tvingar en omarbetning av rollen när java.exe slutar att köras, med hjälp av en lätt, fördefinierad skript som automatiskt ska ingå i din distribution.
* **Fjärrprogram felsökning Konfigurationsgränssnittet Java:** tillåter du tooeasily aktivera Eclipse's remote felsökare tooaccess din Java-app som körs i hello emulatorn eller hello Azure-molnet så att du kan gå igenom och felsöka din Java-kod i realtid. Mer information finns i [felsökning Azure-program i Eclipse].
* **Lokal lagring resurskonfiguration UI:** så att du inte längre behöver tooconfigure lokala resurser genom att ändra hello XML direkt. Den här funktionen kan du också tooaccess toohello effektiva filsökvägen till den lokala resursen när den har distribuerats via en miljövariabel som du kan referera till direkt från din startskriptet. Mer information finns i [egenskaper för lokal lagring].
* **Miljö variabeln Konfigurationsgränssnittet:** så att du inte längre behöver tooset miljövariabler via manuell redigering av hello konfigurations-XML. Mer information finns i [variabler miljöegenskaper].
* **JDBC-drivrutin för SQL Azure:** installeras via hello plugin som sömlöst integrerade Eclipse bibliotek, aktivera enklare programmera mot SQL Azure. 
* **Snabb snabbmenyn åtkomst toorole Konfigurationsgränssnittet**: bara högerklicka på hello rollen mappen och klicka på **egenskaper**.
* **Anpassad Azure projekt och roll ikoner i mappen:** för bättre synlighet och enklare att navigera i din arbetsyta och projekt.

## <a name="see-also"></a>Se även
Mer information om hello Azure-verktyg för Java IDEs finns hello följande länkar:

* [Azure Toolkit för Eclipse]
  * *Vad är nytt i hello Azure Toolkit för Eclipse (den här artikeln)*
  * [installerar hello Azure Toolkit för Eclipse]
  * [Skapa en Hello World-Webbapp för Azure i Eclipse]
  * [Logga i instruktioner för hello Azure Toolkit för Eclipse]
* [Azure Toolkit för IntelliJ]
  * [Vad är nytt i hello Azure Toolkit för IntelliJ]
  * [Installera hello Azure Toolkit för IntelliJ]
  * [Logga i instruktioner för hello Azure Toolkit för IntelliJ]
  * [Skapa en Hello World-Webbapp för Azure i IntelliJ]

Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center].

<!-- URL List -->

[Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij.md
[Skapa en Hello World-Webbapp för Azure i Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Skapa en Hello World-Webbapp för Azure i IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[installerar hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Installera hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Logga i instruktioner för hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Logga i instruktioner för hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[What's New in hello Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Vad är nytt i hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure logga i instruktioner för hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[hur toopublish ett webbprogram som använder en Dockerbehållare hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-publish-as-docker-container.md
[Lagringskonton hantera med hjälp av hello Azure Explorer för Eclipse]: ./azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer.md
[hantera virtuella datorer med hjälp av hello Azure Explorer för Eclipse]: ./azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer.md

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547

[Azul system webbsida för hello Zulu OpenJDK]: http://go.microsoft.com/fwlink/?LinkId=402457
[Azure Tjänsteslutpunkter]: http://go.microsoft.com/fwlink/?LinkID=699526
[Azure Lagringskontolistan]: http://go.microsoft.com/fwlink/?LinkID=699528
[komponenter egenskaper]: http://go.microsoft.com/fwlink/?LinkID=699525#components_properties
[skapar en Hello World-program för Azure i Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[felsökning Azure-program i Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699535
[distribuera stora distributioner]: http://go.microsoft.com/fwlink/?LinkID=699536
[slutpunkter egenskaper]: http://go.microsoft.com/fwlink/?LinkID=699525#endpoints_properties
[variabler miljöegenskaper]: http://go.microsoft.com/fwlink/?LinkID=699525#environment_variables_properties
[HDInsight Tools-Plugin för Eclipse]: ./hdinsight/hdinsight-apache-spark-eclipse-tool-plugin.md
[hur tooAuthenticate webbanvändare med Azure Access Control Service med Eclipse]: http://go.microsoft.com/fwlink/?LinkID=264703
[hur tooUse SSL genom att avlasta]: http://go.microsoft.com/fwlink/?LinkID=699545
[Installera hello Azure Toolkit för Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[egenskaper för lokal lagring]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties
[API: T för Microsoft Azure-klient]: http://go.microsoft.com/fwlink/?LinkId=280397
[Server konfigurationsegenskaper]: http://go.microsoft.com/fwlink/?LinkID=699525#server_configuration_properties
[Session tillhörighet]: http://go.microsoft.com/fwlink/?LinkID=699548
[SSL genom att avlasta]: http://go.microsoft.com/fwlink/?LinkID=699549
[toocreate ett nytt lagringskonto]: http://go.microsoft.com/fwlink/?LinkID=699528#create_new
[virtuell dator och Molntjänststorlekar för Azure]: http://go.microsoft.com/fwlink/?LinkId=466520

<!-- IMG List -->

[ic710876]: ./media/azure-toolkit-for-eclipse-whats-new/ic710876.png
[ic710877]: ./media/azure-toolkit-for-eclipse-whats-new/ic710877.png
[ic710879]: ./media/azure-toolkit-for-eclipse-whats-new/ic710879.png
[ic710880]: ./media/azure-toolkit-for-eclipse-whats-new/ic710880.png
[ic710881]: ./media/azure-toolkit-for-eclipse-whats-new/ic710881.png
[ic710876]: ./media/azure-toolkit-for-eclipse-whats-new/ic710876.png
[ic710882]: ./media/azure-toolkit-for-eclipse-whats-new/ic710882.png
[ic710883]: ./media/azure-toolkit-for-eclipse-whats-new/ic710883.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh694270.aspx -->

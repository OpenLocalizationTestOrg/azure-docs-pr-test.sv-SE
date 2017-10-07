---
title: aaaConfigure web apps i Azure App Service
description: Hur tooconfigure en webbapp i Azure App Service
services: app-service\web
documentationcenter: 
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 9af8a367-7d39-4399-9941-b80cbc5f39a0
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 8697ab6f21cfeb470e11f0d82c68692d43142fc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-web-apps-in-azure-app-service"></a>Konfigurera webbappar i Azure App Service
Det här avsnittet beskrivs hur en web app med hjälp av tooconfigure hello [Azure Portal].

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="application-settings"></a>Programinställningar
1. I hello [Azure Portal]öppnar hello bladet för hello webbprogrammet.
2. Klicka på **alla inställningar**.
3. Klicka på **programinställningar**.

![Programinställningar][configure01]

Hej **programinställningar** bladet har inställningar som är grupperade under flera kategorier.

### <a name="general-settings"></a>Allmänna inställningar
**Versioner av Framework**. Ange dessa alternativ om din app använder alla dessa ramverk: 

* **.NET framework**: Ange hello .NET framework version. 
* **PHP**: Ange hello PHP version eller **OFF** toodisable PHP. 
* **Java**: Välj hello Java-version eller **OFF** toodisable Java. Använd hello **Webbehållaren** alternativet toochoose mellan Tomcat- och Jetty-versioner.
* **Python**: Välj hello Python-versionen eller **OFF** toodisable Python.

Av tekniska skäl inaktiveras när du aktiverar Java för din app hello alternativ för .NET, PHP och Python.

<a name="platform"></a>
**Plattform**. Anger om ditt webbprogram kör i en 32-bitars eller 64-bitars miljö. hello 64-bitars miljö kräver Basic eller Standard-läge. Frigör och delade lägen köras alltid i en 32-bitars-miljö.

**Web Sockets**. Ange **ON** tooenable hello WebSocket-protokollet, till exempel, om ditt webbprogram använder [ASP.NET SignalR] eller [socket.io].

<a name="alwayson"></a>
**Always On**. Som standard inaktiveras webbappar om de är inaktiv under en tidsperiod. På så sätt kan hello system spara resurser. I Basic eller Standard-läge kan du aktivera **alltid på** tookeep hello app laddas alla hello tid. Om din app körs kontinuerliga Webbjobb eller körs WebJobs aktiveras med hjälp av ett CRON-uttryck, bör du aktivera **alltid på**, eller hello web jobb körs inte på ett tillförlitligt sätt.

**Hanterat Pipeline-Version**. Anger hello IIS [pipelineläge]. Lämna detta anges tooIntegrated (hello standard) om du har en äldre app som kräver en äldre version av IIS.

**Automatisk växling**. Om du aktiverar automatisk växling för en distributionsplats ska Apptjänst automatiskt växla hello webbprogram till produktionen när du trycker på en uppdatering toothat plats. Mer information finns i [distribuera toostaging fack för web apps i Azure App Service](web-sites-staged-publishing.md).

### <a name="debugging"></a>Felsökning
**Fjärrfelsökning**. Aktiverar fjärrfelsökning. När aktiverat, kan du använda hello remote felsökning i Visual Studio tooconnect direkt tooyour web app. Fjärrfelsökning förblir aktiverat för 48 timmar. 

### <a name="app-settings"></a>App-inställningar
Det här avsnittet innehåller namn/värde-par som du web app läser in vid start. 

* För .NET-appar de här inställningarna är injekteras i konfigurationen av .NET `AppSettings` vid körning kan åsidosätta befintliga inställningar. 
* PHP, Python, Java och noden program kan komma åt de här inställningarna som miljövariabler vid körning. Två miljövariabler som skapas för varje appinställning. en med hello namn anges av hello app inställningen som och en annan med prefixet APPSETTING_. Innehåller båda hello samma värde.

### <a name="connection-strings"></a>Anslutningssträngar
Anslutningssträngar för länkade resurser. 

För .NET-appar har dessa anslutningssträngar injekteras i konfigurationen av .NET `connectionStrings` inställningar vid körning kan åsidosätta befintliga poster där hello nyckel är lika med hello länkade databasnamnet. 

PHP, Python, Java och noden program, ska inställningarna vara tillgänglig som miljövariabler vid körning, prefixet hello anslutningstypen. hello miljö variabeln prefix är följande: 

* SQLServer:`SQLCONNSTR_`
* MySQL:`MYSQLCONNSTR_`
* SQL-databas:`SQLAZURECONNSTR_`
* Anpassad:`CUSTOMCONNSTR_`

Om exempelvis en MySql-anslutningssträng vid namn `connectionstring1`, den kan nås via hello miljövariabeln `MYSQLCONNSTR_connectionString1`.

### <a name="default-documents"></a>Standarddokument
hello standarddokument är hello webbsida som visas på hello rot-URL för en webbplats.  hello första matchande fil i listan hello används. 

Webbprogram kan använda moduler att vägen baserat på URL: en i stället för betjänar statiskt innehåll, då det finns inga standarddokument som sådana.    

### <a name="handler-mappings"></a>Hanterarmappningar
Använda det här området tooadd anpassat skript processorer toohandle begäranden för specifika filtillägg. 

* **Tillägget**. hello filen tillägget toobe hanteras som *.php eller handler.fcgi. 
* **Script Processor sökvägen**. hello absolut sökväg hello Skriptprocessor. Begäranden toofiles som matchar filnamnstillägget hello bearbetas av hello Skriptprocessor. Använd hello sökväg `D:\home\site\wwwroot` toorefer tooyour appens rotkatalog.
* **Ytterligare argument**. Valfria kommandoradsargument för hello Skriptprocessor 

### <a name="virtual-applications-and-directories"></a>Virtuella program och kataloger
Ange varje virtuell katalog och dess motsvarande fysisk sökväg relativa toohello webbplatsroten tooconfigure virtuella program och kataloger. Du kan också markera hello **programmet** kryssrutan toomark en virtuell katalog som ett program.

## <a name="enabling-diagnostic-logs"></a>Aktivera diagnostikloggar
tooenable diagnostikloggar:

1. Klicka i hello bladet för din webbapp **alla inställningar**.
2. Klicka på **diagnostikloggar**. 

Alternativ för att skriva diagnostiska loggar från webbprogram som har stöd för loggning: 

* **Programloggning**. Skriver programloggarna toohello filsystem. Loggning varar under en period på 12 timmar. 

**Nivå**. När programmet loggning är aktiverat anger det här alternativet hello mängden information som är registrerade (fel, varning, Information eller utförlig).

**Web server-loggning**. Loggar sparas i hello W3C utökad loggning. 

**Detaljerade felmeddelanden**. Sparar felmeddelanden detaljerade .htm-filer. hello filerna sparas under /LogFiles/DetailedErrors. 

**Spårning av misslyckade begäranden**. Det gick inte att begäranden tooXML filer loggar. hello filerna sparas under loggfilerna/W3SVC*xxx*, där xxx är en unik identifierare. Den här mappen innehåller en XSL-fil och en eller flera XML-filer. Se till att toodownload hello XSL-filen eftersom det ger funktioner för att formatera och filtrera hello innehållet i hello XML-filer.

tooview hello loggfiler, måste du skapa FTP-autentiseringsuppgifter, enligt följande:

1. Klicka i hello bladet för din webbapp **alla inställningar**.
2. Klicka på **distributionsbehörigheterna**.
3. Ange ett användarnamn och lösenord.
4. Klicka på **Spara**.

![Ange autentiseringsuppgifter för distribution][configure03]

hello fullständig FTP-användarnamnet är ”app\username” där *app* är hello namnet på ditt webbprogram. hello användarnamnet visas i hello web app bladet under **Essentials**.  

![Autentiseringsuppgifter för FTP-distribution][configure02]

## <a name="other-configuration-tasks"></a>Andra konfigurationsuppgifter
### <a name="ssl"></a>SSL
Du kan överföra SSL-certifikat för en anpassad domän i Basic eller Standard-läge. Mer information finns i [aktivera HTTPS för en webbapp]. 

tooview överförda certifikat, klicka på **alla inställningar** > **anpassade domäner och SSL**.

### <a name="domain-names"></a>Domännamn
Lägg till anpassade domännamn för din webbapp. Mer information finns i [konfigurera ett anpassat domännamn för en webbapp i Azure App Service].

tooview ditt domännamn, klicka på **alla inställningar** > **anpassade domäner och SSL**.

### <a name="deployments"></a>Distributioner
* Ställ in kontinuerlig distribution. Se [med Git toodeploy Web Apps i Azure App Service](web-sites-deploy.md).
* Distributionsplatser. Se [distribuera tooStaging miljöer för Web Apps i Azure App Service].

tooview dina distributionsplatser, klicka på **alla inställningar** > **distributionsfack**.

### <a name="monitoring"></a>Övervakning
I Basic eller Standard-läge kan du testa hello tillgängligheten för HTTP eller HTTPS-slutpunkter från in toothree fördelade platser. En övervakning testet misslyckas om hello HTTP-svarskoden är ett fel (4xx eller 5xx) eller hello svar tar mer än 30 sekunder. En slutpunkt anses vara tillgänglig om hello övervakningstesten lyckas från alla hello angivna platser. 

Mer information finns i [så här: övervaka status för web endpoint].

> [!NOTE]
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service], där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

## <a name="next-steps"></a>Nästa steg
* [Konfigurera ett anpassat domännamn i Azure App Service]
* [Aktivera HTTPS för en app i Azure App Service]
* [Skala en webbapp i Azure App Service]
* [Övervakning grunderna för Web Apps i Azure App Service]

<!-- URL List -->

[ASP.NET-SignalR]: http://www.asp.net/signalr
[Azure Portal]: https://portal.azure.com/
[Konfigurera ett anpassat domännamn i Azure App Service]: ./app-service-web-tutorial-custom-domain.md
[Distribuera tooStaging miljöer för Web Apps i Azure App Service]: ./web-sites-staged-publishing.md
[Aktivera HTTPS för en app i Azure App Service]: ./app-service-web-tutorial-custom-ssl.md
[Så här: övervaka status på slutpunkt]: http://go.microsoft.com/fwLink/?LinkID=279906
[Övervakning grunderna för Web Apps i Azure App Service]: ./web-sites-monitor.md
[Pipeline-läge]: http://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application
[Skala en webbapp i Azure App Service]: ./web-sites-scale.md
[socket.IO]: ./web-sites-nodejs-chat-app-socketio.md
[Prova App Service]: https://azure.microsoft.com/try/app-service/

<!-- IMG List -->

[configure01]: ./media/web-sites-configure/configure01.png
[configure02]: ./media/web-sites-configure/configure02.png
[configure03]: ./media/web-sites-configure/configure03.png

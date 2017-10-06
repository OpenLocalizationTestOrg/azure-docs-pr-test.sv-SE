---
title: "aaaBest metoder och felsökningsguiden för noden program på Azure-Webbappar"
description: "Lär dig hello metodtips och felsökningssteg för noden program på Azure Web Apps."
services: app-service\web
documentationcenter: nodejs
author: ranjithr
manager: wadeh
editor: 
ms.assetid: 387ea217-7910-4468-8987-9a1022a99bef
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 06/06/2016
ms.author: ranjithr
ms.openlocfilehash: 975898142a224f14df1091a46d16e9074d9e2831
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-and-troubleshooting-guide-for-node-applications-on-azure-web-apps"></a>Bästa praxis och felsökningsguiden för noden program på Azure-Webbappar
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

I den här artikeln får du lära dig hello metodtips och felsökningssteg för [noden program](app-service-web-get-started-nodejs.md) körs på Azure-Webbappar (med [iisnode](https://github.com/azure/iisnode)).

> [!WARNING]
> Var försiktig när du använder för felsökning på webbplatsen produktion. Rekommendation är tootroubleshoot din app på en produktion till exempel konfigurera din mellanlagringsplatsen och när hello problemet har åtgärdats, byta din mellanlagringsplatsen med din produktionsplatsen.
> 
> 

## <a name="iisnode-configuration"></a>IISNODE-konfiguration
Detta [schemafilen](https://github.com/Azure/iisnode/blob/master/src/config/iisnode_schema_x64.xml) visar alla hello-inställningar som kan konfigureras för iisnode. Vissa hello-inställningar som kan användas för ditt program är:

* nodeProcessCountPerApplication
  
    Den här inställningen styr hello antalet noden processer som startas per IIS-program. Standardvärdet är 1. Du kan starta så många node.exe som din Virtuella core antal genom att ange den här too0. Rekommenderat värde är 0 för de flesta program så att du kan använda alla hello kärnor på datorn. Node.exe är en enskild threaded så att en node.exe kommer att använda högst 1 core och tooget maximala prestanda utanför tillämpningsprogrammet nod du vill ha tooutilize alla kärnor.
* nodeProcessCommandLine
  
    Den här inställningen styr hello sökvägen toohello node.exe. Du kan ange det här värdet toopoint tooyour node.exe version.
* maxConcurrentRequestsPerProcess
  
    Den här inställningen styr hello maximalt antal samtidiga förfrågningar som skickas av iisnode tooeach node.exe. På azure webbappar är hello standardvärdet för det här oändligt. Du har inte tooworry om den här inställningen. Utanför azure webbappar är hello standardvärdet 1024. Du kanske vill tooconfigure detta beroende på hur många begär programmet hämtar och hur snabbt programmet bearbetar varje begäran.
* maxNamedPipeConnectionRetry
  
    Den här inställningen styr hello maximalt antal gånger som iisnode kommer att försöka att anslutningen på hello namngivna pipe toosend hello begäran via toonode.exe. Den här inställningen i kombination med namedPipeConnectionRetryDelay avgör hello totala tidsgränsen för varje begäran inom iisnode. Standardvärdet är 200 på Azure-Webbappar. Totalt antal tidsgräns i sekunder = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000
* namedPipeConnectionRetryDelay
  
    Den här inställningen kontrollerar hello mängden tid (i ms) iisnode ska vänta mellan varje försök toosend begäran toonode.exe över hello namngiven pipe. Standardvärdet är 250ms.
    Totalt antal tidsgräns i sekunder = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000
  
    Som standard är hello totala timeout i iisnode på azure webbappar 200 \* 250ms = 50 sekunder.
* logDirectory
  
    Den här inställningen styr hello katalogen där iisnode loggas stdout/stderr. Standardvärdet är iisnode som är relativa toohello huvudskriptet katalogen (directory där huvudsakliga server.js finns)
* debuggerExtensionDll
  
    Den här inställningen styr vilken version av node-inspector iisnode ska använda vid felsökning av tillämpningsprogrammet nod. Iisnode-inspector-0.7.3.dll och iisnode inspector.dll för närvarande hello 2 endast giltiga värden för den här inställningen. Standardvärdet är iisnode-inspector-0.7.3.dll. iisnode-inspector-0.7.3.dll version använder node-inspector-0.7.3 och använder websockets, så du måste tooenable websockets på azure webapp-toouse den här versionen. Se <http://www.ranjithr.com/?p=98> för mer information om hur tooconfigure iisnode toouse hello nya node-inspector.
* flushResponse
  
    hello standardbeteendet för IIS är att den buffrar svarsdata upp too4MB innan eller tills hello slutet av hello svar, beroende på vilket som kommer först. iisnode erbjuder en konfigurationsinställning toooverride problemet: tooflush fragment av hello svarstexten entiteten så snart som iisnode tar emot det från node.exe måste du tooset hello iisnode/@flushResponse attribut i web.config too'true':
  
    ```
    <configuration>    
        <system.webServer>    
            <!-- ... -->    
            <iisnode flushResponse="true" />    
        </system.webServer>    
    </configuration>
    ```
  
    Genom att aktivera bokföring för varje fragment av hello svarstexten entitet läggs prestanda försämras som minskar hello genomflödet i hello system med ~ 5% (från och med v0.1.13), så det är bästa tooscope den här inställningen bara tooendpoints som kräver svar strömning (t.ex. med hello <location> -elementet i web.config hello)
  
    I tillägg toothis behöver för direktuppspelning av program, du tooalso set responseBufferLimit av din iisnode hanterare too0.
  
    ```
    <handlers>    
        <add name="iisnode" path="app.js" verb="\*" modules="iisnode" responseBufferLimit="0"/>    
    </handlers>
    ```
* watchedFiles
  
    Detta är en lista över filer som ska övervakas för ändringar som gäller avgränsas med semikolon. En ändring tooa fil gör hello programmet toorecycle. Varje post består av ett valfritt katalognamnet samt nödvändiga filnamn som är relativa toohello katalogen där hello programmets startpunkt finns. Jokertecken är tillåtna i hello filen namnet delen. Standardvärdet är ”\*. js;web.config”
* recycleSignalEnabled
  
    Standardvärdet är false. Om aktiverad, node-App kan ansluta tooa namngiven pipe (miljövariabeln IISNODE\_kontrollen\_PIPE) och skicka ett meddelande om ”återvinning”. Detta innebär att hello w3wp toorecycle avslutas.
* idlePageOutTimePeriod
  
    Standardvärdet är 0 vilket betyder att den här funktionen är inaktiverad. När set toosome-värde som är större än 0, iisnode kommer sidan ut alla dess underordnade bearbetar alla 'idlePageOutTimePeriod' millisekunder. toounderstand vad sidan ut medel, se toothis [dokumentationen](https://msdn.microsoft.com/library/windows/desktop/ms682606.aspx). Den här inställningen kan vara användbart för program som använder mycket minne och vill toopageout minne toodisk ibland toofree upp vissa RAM-minne.

> [!WARNING]
> Var försiktig när du aktiverar hello följande inställningar på program i produktion. Rekommendation är toonot aktivera dem på live produktionsprogram.
> 
> 

* debugHeaderEnabled
  
    hello standardvärdet är false. Om ange tootrue, iisnode lägger till ett HTTP-svar huvud iisnode-debug tooevery HTTP svar skickas hello iisnode-debug-huvud är en URL. Enskilda delar diagnostisk information kan vara uppnår genom att titta på hello URL-fragment, men en mycket bättre visualisering uppnås genom att öppna hello URL i hello webbläsare.
* loggingEnabled
  
    Den här inställningen styr hello loggning av stdout och stderr av iisnode. Iisnode ska avbilda stdout/stderr från noden processer som startas och skriva toohello katalogen som anges i inställningen för hello 'logDirectory'. När detta är att aktivera programmet skriver loggar toohello filsystemet och beroende på hello mängden loggning som utförs av programmet hello, kan det finnas prestanda.
* devErrorsEnabled
  
    Standardvärdet är false. Om värdet är tootrue, iisnode visar hello HTTP-statuskoden och Win32-felkod i webbläsaren. hello win32-kod är användbar vid felsökning av vissa typer av problem.
* debuggingEnabled (aktivera inte på live produktionsplatsen)
  
    Den här inställningen styr felsökningsfunktionen. Iisnode är integrerad med node-inspector. Genom att aktivera den här inställningen kan aktivera du felsökning av tillämpningsprogrammet nod. När den här inställningen är aktiverad kommer iisnode layout hello nödvändiga node-inspector filer i 'debuggerVirtualDir'-katalogen för hello första debug-begäran tooyour nod. Du kan läsa in hello node-inspector genom att skicka en begäran toohttp://yoursite/server.js/debug. Du kan styra hello debug URL-segment med inställningen 'debuggerPathSegment'. Som standard debuggerPathSegment = 'debug ”. Du kan ange den här tooa GUID, till exempel så att det är svårare toobe som identifieras av andra.
  
    Kontrollera detta [länken](https://tomasz.janczuk.org/2011/11/debug-nodejs-applications-on-windows.html) för mer information om hur du felsöker.

## <a name="scenarios-and-recommendationstroubleshooting"></a>Scenarier och rekommendationer/felsökning
### <a name="my-node-application-is-making-too-many-outbound-calls"></a>Node-App är att för många utgående anrop.
Många program kan toomake utgående anslutningar som en del av deras regelbunden drift. Till exempel när en begäran kommer, skulle appen nod vill toocontact REST API någon annanstans och hämta viss information tooprocess hello-begäran. Du kan toouse en keep-alive agent när du anropar http eller https. Exempelvis kan också använda hello agentkeepalive modul som agent keep-alive när du gör dessa utgående anrop. Detta säkerställer att hello sockets återanvänds på azure webapp VM och minska hello arbetet med att skapa nya sockets för varje utgående begäran. Detta ser också till att du använder mindre antal sockets toomake många utgående förfrågningar och därför du inte överstiga hello maxsocket som allokeras per VM. Rekommendation om Azure-Webbappar skulle vara tooset hello agentKeepAlive maxsocket värdet tooa totalt 160 sockets per virtuell dator. Det innebär att om du har 4 node.exe körs på hello VM du bör tooset hello agentKeepAlive maxsocket too40 per node.exe som är 160 Totalt per virtuell dator.

Exempel [agentKeepALive](https://www.npmjs.com/package/agentkeepalive) konfiguration:

```
var keepaliveAgent = new Agent({    
    maxSockets: 40,    
    maxFreeSockets: 10,    
    timeout: 60000,    
    keepAliveTimeout: 300000    
});
```

Det här exemplet förutsätter att du har 4 node.exe körs på den virtuella datorn. Om du har ett annat antal node.exe körs på hello VM har du toomodify hello maxsocket ange därefter.

### <a name="my-node-application-is-consuming-too-much-cpu"></a>Min noden program förbrukar för mycket CPU.
Förmodligen visas en rekommendation från Azure-Webbappar på portalen om hög cpu-förbrukning. Du kan också installationsprogrammet Övervakare toowatch för vissa [mått](web-sites-monitor.md). När du kontrollerar hello CPU-användning på hello [Azure Portal instrumentpanelen](../application-insights/app-insights-web-monitor-performance.md), kontrollera hello maxvärde för CPU så att du inte missar ut hello högsta värden.
I fall där du tror att ditt program förbrukar för mycket CPU och du kan förklara varför behöver du tooprofile node-App.

### 
#### <a name="profiling-your-node-application-on-azure-webapps-with-v8-profiler"></a>Profilering av noden programmet på azure webbappar med V8-Profiler
Exempelvis kan säga du har en hello world-app som du vill tooprofile enligt nedan:

```
var http = require('http');    
function WriteConsoleLog() {    
    for(var i=0;i<99999;++i) {    
        console.log('hello world');    
    }    
}

function HandleRequest() {    
    WriteConsoleLog();    
}

http.createServer(function (req, res) {    
    res.writeHead(200, {'Content-Type': 'text/html'});    
    HandleRequest();    
    res.end('Hello world!');    
}).listen(process.env.PORT);
```

Gå tooyour scm plats https://yoursite.scm.azurewebsites.net/DebugConsole

Kommandotolken visas enligt nedan. Gå till din plats/wwwroot-katalog

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_install_v8.png)

Kör kommandot hello ”npm installera v8 profiler”

Detta ska installera v8 profileraren under noden\_moduler katalogen och alla dess beroenden.
Nu kan redigera din server.js tooprofile ditt program.

```
var http = require('http');    
var profiler = require('v8-profiler');    
var fs = require('fs');

function WriteConsoleLog() {    
    for(var i=0;i<99999;++i) {    
        console.log('hello world');    
    }    
}

function HandleRequest() {    
    profiler.startProfiling('HandleRequest');    
    WriteConsoleLog();    
    fs.writeFileSync('profile.cpuprofile', JSON.stringify(profiler.stopProfiling('HandleRequest')));    
}

http.createServer(function (req, res) {    
    res.writeHead(200, {'Content-Type': 'text/html'});    
    HandleRequest();    
    res.end('Hello world!');    
}).listen(process.env.PORT);
```

hello ovan ändringar kommer profilen hello WriteConsoleLog funktionen och sedan skriva hello profil utdata too'profile.cpuprofile-filen under din plats wwwroot. Skicka en begäran tooyour program. Du ser en 'profile.cpuprofile'-fil som skapats under din plats wwwroot.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_profile.cpuprofile.png)

Hämta den här filen och du behöver tooopen filen med Chrome F12 verktyg. Tryck på F12 på chrome, och klicka sedan på hello ”fliken profiler”. Klicka på ”belastningen”. Välj din profile.cpuprofile-fil som du precis har laddat ned. Klicka på hello-profil som du just läste in.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/chrome_tools_view.png)

Du ser att 95% hello tid har förbrukats av WriteConsoleLog funktionen enligt nedan. Detta visar också hello exakt radnummer och källfiler som orsakar hello problemet.

### <a name="my-node-application-is-consuming-too-much-memory"></a>Min noden program förbrukar för mycket minne.
Du kommer förmodligen en rekommendation från Azure-Webbappar på portalen om hög minnesförbrukning. Du kan också installationsprogrammet Övervakare toowatch för vissa [mått](web-sites-monitor.md). När du kontrollerar hello minnesanvändningen för hello [Azure Portal instrumentpanelen](../application-insights/app-insights-web-monitor-performance.md), kontrollera hello maxvärde för minne så att du inte missar ut hello högsta värden.

#### <a name="leak-detection-and-heap-diffing-for-nodejs"></a>Identifiering av läcka och Heap Diffing för node.js
Du kan använda [nod memwatch](https://github.com/lloyd/node-memwatch) toohelp du identifiera minne läcker.
Du kan installera memwatch precis som v8 profiler och redigera din kod toocapture och diff heapar tooidentify hello minnesläckor i ditt program.

### <a name="my-nodeexes-are-getting-killed-randomly"></a>Min node.exe är komma avslutats slumpmässigt
Det finns ett par skäl till varför detta skulle kunna förekomma:

1. Programmet som utlöste undantagsfel utan felhantering – ta kontroll d:\\hem\\loggfiler\\programmet\\loggning errors.txt fil hello information på hello undantag uppstod. Den här filen har hello stack-spårning så att du kan åtgärda baserat på det här programmet.
2. Programmet förbrukar för mycket minne som påverkar andra processer från att komma igång. Om hello totalt Virtuellt minne Stäng too100%, din node.exe kunde avslutas av hello process manager toolet andra processer arbetar en chansen toodo. toofix, se till att programmet inte läcker minne eller om du program verkligen behöver toouse mycket minne skala upp tooa större virtuell dator med mycket mer RAM-minne.

### <a name="my-node-application-does-not-start"></a>Node-App startar inte
Om programmet returnerar 500 fel vid start, kan det finnas flera skäl:

1. Node.exe finns inte på hello rätt plats. Kontrollera nodeProcessCommandLine inställningen.
2. Huvudsakliga skriptfilen finns inte på hello rätt plats. Kontrollera web.config och att hello namnet på hello huvudsakliga skriptfilen i hello hanterare avsnittet matchar hello huvudsakliga skriptfilen.
3. Web.config konfigurationen är inte korrekt – Kontrollera hello namn/värden.
4. Kallstart – ditt program tar för lång toostartup. Om ditt program tar längre än (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1 000 sekunder iisnode returnerar fel 500. Öka hello värdena för dessa inställningar toomatch programmet starta tid tooprevent iisnode från timeout och returnerar hello 500-fel.

### <a name="my-node-application-crashed"></a>Min nod programmet kraschade
Programmet som utlöste undantagsfel utan felhantering – ta kontroll d:\\hem\\loggfiler\\programmet\\loggning errors.txt fil hello information på hello undantag uppstod. Den här filen har hello stack-spårning så att du kan åtgärda baserat på det här programmet.

### <a name="my-node-application-takes-too-much-time-toostartup-cold-start"></a>Min noden tar för lång tid toostartup (kalla Start)
De vanligaste anledningen är att hello program har många filer i hello noden\_moduler och hello program försöker tooload de flesta av dessa filer vid start. Som standard eftersom filerna finnas på hello nätverksresurs på Azure-Webbappar kan inläsning av så många filer ta lite tid.
Vissa lösningar toomake detta snabbare är:

1. Kontrollera att du har en platt beroende struktur och inga dubbla beroenden genom att använda npm3 tooinstall modulerna.
2. Försök toolazy belastningen noden\_moduler och inte att läsa in alla hello moduler vid start. Det innebär att hello anropet toorequire('module') bör utföras när du verkligen behöver det i hello funktion försök toouse hello modulen.
3. Azure Webbappar innehåller en funktion som kallas lokala cache. Den här funktionen kopierar innehållet från hello nätverk resursen toohello lokal disk på hello VM. Eftersom hello filer är lokala hello hämtningstid för nod\_moduler är mycket snabbare. -Det här [dokumentationen](../app-service/app-service-local-cache.md) förklarar hur toouse lokalt cacheminne i mer detalj.

## <a name="iisnode-http-status-and-substatus"></a>IISNODE HTTP-status och understatus
Detta [källfilen](https://github.com/Azure/iisnode/blob/master/src/iisnode/cnodeconstants.h) visar alla hello möjliga status/understatus kombination iisnode kan returnera vid fel.

Aktivera FREB för ditt program toosee hello win32-felkod (se till att du aktiverar FREB bara på icke-produktionsmiljöer av prestandaskäl).

| HTTP-Status | HTTP-status | Möjlig orsak? |
| --- | --- | --- |
| 500 |1000 |Det fanns vissa problem sändning hello begäran tooIISNODE – kontrollera om node.exe har startats. Node.exe kunde har kraschat vid start. Kontrollera konfigurationen av web.config innehåller fel. |
| 500 |1001 |-Win32Error 0x2 - App svarar inte toohello URL. Kontrollera URL-omskrivning om regler eller om en uttrycklig app har rätt hello vägar som definierats. -Win32Error 0x6d – namngiven pipe är upptagen – Node.exe kan ta emot inte begäranden eftersom hello pipe är upptaget. Kontrollera hög cpu-användning. -Andra fel – kontrollera om node.exe kraschade. |
| 500 |1002 |Node.exe kraschat – Kontrollera d:\\hem\\loggfiler\\loggning errors.txt för stack-spårning. |
| 500 |1003 |Pipe-konfiguration problemet – bör aldrig visas men om du gör hello namngivna pipe-konfigurationen är felaktig. |
| 500 |1004-1018 |Det uppstod ett fel när hello begäran eller bearbetning hello svar skickas till eller från node.exe. Kontrollera om node.exe kraschade. Kontrollera d:\\hem\\loggfiler\\loggning errors.txt för stack-spårning. |
| 503 |1000 |Det finns inte tillräckligt med minne tooallocate mer namngivna pipe-anslutningar. Kontrollera varför appen förbrukar för mycket minne. Kontrollera maxConcurrentRequestsPerProcess värdet. Om det är inte oändligt och du har många begäranden, öka det här värdet tooprevent det här felet. |
| 503 |1001 |Begäran kunde inte skickade toonode.exe eftersom hello programåtervinning. När programmet hello har återvunnits ska begäranden omdirigeras normalt. |
| 503 |1002 |Kontrollera win32-felkod faktiska orsak – begäran gick inte att skickade tooa node.exe. |
| 503 |1003 |Namngiven pipe är upptagen – kontrollera om noden förbrukar för mycket processortid |

Det finns en inställning i NODE.exe kallas nod\_väntar\_PIPE\_INSTANSER. Det här värdet är 4 som standard utanför azure webbappar. Det innebär att node.exe kan endast accepterar begäranden som 4 samtidigt på hello namngiven pipe. Det här värdet anges too5000 och värdet ska vara bra att de flesta noden program som körs på azure-webbappar på Azure-Webbappar. Du bör inte se 503.1003 på azure webbappar eftersom vi har ett högt värde för hello nod\_väntar\_PIPE\_INSTANSER.  |

## <a name="more-resources"></a>Fler resurser
Följ dessa länkar toolearn mer om node.js-program på Azure App Service.

* [Kom igång med Node.js-webbappar i Azure App Service](app-service-web-get-started-nodejs.md)
* [Hur toodebug en Node.js webbapp i Azure App Service](web-sites-nodejs-debug.md)
* [Använda Node.js-moduler med Azure-program](../nodejs-use-node-modules-azure-apps.md)
* [Azure App Service Web Apps: Node.js](https://blogs.msdn.microsoft.com/silverlining/2012/06/14/windows-azure-websites-node-js/)
* [Node.js Developer Center](../nodejs-use-node-modules-azure-apps.md)
* [Utforska hello Super hemlighet Kudu-Felsökningskonsolen](https://azure.microsoft.com/documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/)


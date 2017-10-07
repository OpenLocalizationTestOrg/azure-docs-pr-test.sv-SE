---
title: aaaCreate en Node.js-chattprogram med Socket.IO i Azure App Service
description: "En genomgång som visar med socket.io i en node.js-webbapp i Azure."
services: app-service\web
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: c4c4af36-3ecf-4619-b586-ca90d53ce35b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: 3bd7867ccc297dc0a21c7a00cc9db06358877f5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-nodejs-chat-application-with-socketio-in-azure-app-service"></a>Skapa ett Node.js-chattprogram med Socket.IO i Azure Apptjänst
Socket.IO ger kommunikation mellan din node.js-server och klienter som använder WebSockets i realtid. Det stöder också återställningsplats tooother transporter (till exempel lång avsökning) som fungerar med äldre webbläsare. Den här självstudiekursen kommer igenom värd för en Socket.IO baserat chattprogram som en Azure webbapp och visa hur tooscale hello program använder [Azure Redis-Cache]. Mer information om Socket.IO finns <http://socket.io/>.

> [!NOTE]
> hello procedurer i det här steget gäller för[App Service Web Apps]; för molntjänster, se [skapa ett Node.js-chatta program med Socket.IO i Azure-molntjänst].
> 
> 

## <a name="download-hello-chat-example"></a>Hämta hello chatt-exempel
För det här projektet använder vi hello chatt exempel från hello [Socket.IO GitHub-lagringsplatsen]. Utför följande steg toodownload hello exempel hello och lägga till den toohello projekt som du skapade tidigare.

1. Hämta en [ZIP- eller GZ arkiveras versionen] av hello Socket.IO projekt (version 1.3.5 har använts för det här dokumentet)
2. Extrahera hello arkivet och kopiera hello **exempel\\chatt** directory tooa ny plats. Till exempel  **\\nod\\chatt**.

## <a name="modify-appjs-and-install-modules"></a>Ändra app.js och installera moduler
1. Byt namn på hello **index.js** filen för**app.js**. Detta gör att Azure toodetect att detta är ett Node.js-program.
2. Öppna hello **app.js** i en textredigerare. Ändra hello rad som innehåller `var io = require('../..')(server);` enligt nedan:
   
       var express = require('express');
       var app = express();
       var server = require('http').createServer(app);
       // var io = require('../..')(server);
       // New:
       var io = require('socket.io')(server);
       var port = process.env.PORT || 3000;
3. Öppna hello **package.json** och Lägg till en referens toosocket.io under `dependencies`, enligt nedan:
   
        "dependencies": {
          "express": "3.4.8",
          "socket.io": "1.3.5"
        }
4. Från kommandoraden hello, ändra toohello  **\\nod\\chatt** katalogen och Använd npm tooinstall hello moduler som krävs av programmet:
   
        npm install
   
    Detta kommer att installera hello moduler i en undermapp som heter **node_modules**.

## <a name="create-an-azure-web-app"></a>Skapa en Azure-Webbapp
Följ dessa steg toocreate en Azure-webbapp, aktivera Git-publicering och sedan aktivera WebSocket-stöd för hello webbprogrammet.

> [!NOTE]
> toocomplete den här självstudiekursen kommer du behöver ett Azure-konto. Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information om den <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A7171371E" target="_blank">kostnadsfria utvärderingsversionen av Azure</a>.
> 
> 

1. Installera hello Azure-kommandoradsgränssnittet (Azure CLI) och ansluta tooyour Azure-prenumeration. Se [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md).
2. Om det här är första gången du upp en databas i Azure måste toocreate inloggningsuppgifter. Ange följande kommando hello från hello Azure CLI:
   
        azure site deployment user set [username] [password]
3. Ändra toohello  **\\node\chat** katalogen och Använd hello följande kommandot toocreate en ny Azure webbapp och en lokal Git-lagringsplats. Det här kommandot skapar även en Git fjärråtkomst med namnet ”azure”.
   
        azure site create mysitename --git
   
    Du måste ersätta 'mysitename' med ett unikt namn för ditt webbprogram.
4. Genomför hello befintliga filer toohello lokal databas med hjälp av hello följande kommandon:
   
        git add .
        git commit -m "Initial commit"
5. Push hello filer toohello Azure Web Apps databasen med hello följande kommando:
   
        git push azure master
   
    När du uppmanas ange dina autentiseringsuppgifter från steg 2. Du tar emot statusmeddelanden som importeras moduler på hello-servern. När den här processen är klar, finnas hello program på Azure-webbapp.
   
   > [!NOTE]
   > Under installationen av modulen kan du eventuellt se felen som ' hello importerade projekt... hittades inte ”. Dessa kan ignoreras.
   > 
   > 
6. Socket.IO använder WebSockets som inte är aktiverat som standard på Azure. tooenable web sockets använder hello följande kommando:
   
        azure site set -w
   
    Om du uppmanas ange hello namn på hello webbprogrammet.
   
   > [!NOTE]
   > Hej ”azure site set -w-kommandot kommer fungerar bara med version 0.7.4 eller senare av hello Azure-kommandoradsgränssnittet. Du kan också aktivera WebSocket stöd med hello [Azure Portal](https://portal.azure.com).
   > 
   > tooenable WebSockets med hello Azure-portalen klickar du på hello webbprogrammet från hello Web Apps-bladet, klickar du på **alla inställningar** > **programinställningar**. Under **Webbsocketar**, klickar du på **på**. Klicka sedan på **Spara**.
   > 
   > 
7. tooview hello-webbapp på Azure, Använd hello följande kommando toolaunch din webbläsare och gå toohello som är värd för webbprogram:
   
        azure site browse

Appen körs nu på Azure och kan vidarebefordra chatt meddelanden mellan olika klienter med Socket.IO.

## <a name="scale-out"></a>Skala ut
Socket.IO program kan skaländras ut med hjälp av en **nätverkskort** toodistribute meddelanden och händelser mellan flera instanser av programmet. Det finns flera nätverkskort, hello [socket.io redis] kortet enkelt kan användas med hello Azure Redis-Cache-funktionen.

> [!NOTE]
> Ytterligare krav för att skala ut en Socket.IO lösning finns stöd för Fäst sessioner. Fäst sessioner är aktiverade som standard för Azure Web Apps via Azure begära routning. Mer information finns i [instans tillhörighet i Azure webbplatser].
> 
> 

### <a name="create-a-redis-cache"></a>Skapa ett Redis-cache
Utför hello steg i [skapa en cache i Azure Redis-Cache] toocreate ett nytt cacheminne.

> [!NOTE]
> Spara hello **värdnamn** och **primärnyckel** för ditt cacheminne hello eftersom de behövs i nästa steg.
> 
> 

### <a name="add-hello-redis-and-socketio-redis-modules"></a>Lägg till hello redis och socket.io redis-moduler
1. Ändra toohello från en kommandorad  **\\nod\\chatt** katalogen och Använd hello följande kommando.
   
        npm install socket.io-redis@0.1.4 redis@0.12.1 --save
   
   > [!NOTE]
   > hello-versioner som anges i det här kommandot är hello-versioner som används vid testning av den här artikeln.
   > 
   > 
2. Ändra hello **app.js** filen tooadd hello följande rader omedelbart efter`var io = require('socket.io')(server);`
   
        var pub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
        var sub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
   
        var redis = require('socket.io-redis');
        io.adapter(redis({pubClient: pub, subClient: sub}));
   
    Ersätt **redishostname** och **rediskey** med hello värdnamnet och nyckeln för Redis-cache.
   
    Detta skapar en publicera och prenumerera på klienten toohello Redis-cache som skapats tidigare. hello klienter sedan används med hello kortet tooconfigure Socket.IO toouse hello Redis-cache för att skicka meddelanden och händelser mellan instanser av programmet
   
   > [!NOTE]
   > När hello **socket.io redis** nätverkskort kan kommunicera direkt tooRedis, hello aktuella versionen stöder inte hello-autentisering krävs av Azure Redis-cache. Så hello första anslutningen skapas med hjälp av hello **redis** modulen, skickas sedan hello klienten toohello **socket.io redis** nätverkskort.
   > 
   > Azure Redis-Cache har stöd för säkra anslutningar som använder port 6380, men hello-moduler som används i det här exemplet stöder inte säkra anslutningar från 2014-7-14. hello ovan koden använder hello standard osäkra port för 6379.
   > 
   > 
3. Spara hello ändrade **app.js**

### <a name="commit-changes-and-redeploy"></a>Genomför ändringar och distribuera
Från kommandoraden i hello hello  **\\nod\\chatt** directory, Använd följande kommandon toocommit ändringar hello och distribuera programmet hello.

    git add .
    git commit -m "implementing scale out"
    git push azure master

När hello ändringar har aviserats toohello server, kan du skala webbplatsen på flera instanser med hjälp av följande kommando hello.

    azure site scale instances --instances #

Där  **#**  är hello antalet instanser toocreate.

Du kan ansluta tooyour webbprogrammet från flera webbläsare eller datorer tooverify som meddelanden skickas korrekt tooall klienter.

## <a name="troubleshooting"></a>Felsökning
### <a name="connection-limits"></a>Anslutningsbegränsningar
Azure Web Apps finns i flera SKU: er som avgör hello resurser tillgängliga tooyour plats. Detta inkluderar hello antalet tillåtna WebSocket-anslutningar. Mer information finns i hello [prissättning för Web Apps].

### <a name="messages-arent-being-sent-using-websockets"></a>Meddelanden skickas inte med WebSockets
Om klientwebbläsare hålla återgång toolong avsökning istället för att använda WebSockets, kan det bero på något av följande hello.

* **Försök att begränsa hello transport toojust WebSockets**
  
    För att Socket.IO toouse WebSockets som hello messaging transport både hello-servern och klienten måste ha stöd för WebSockets. Om en eller hello andra inte, förhandlar Socket.IO en annan transport, till exempel lång avsökning. hello standardlistan över transporter som används av Socket.IO är ` websocket, htmlfile, xhr-polling, jsonp-polling`. Du kan tvinga den tooonly Använd WebSockets genom att lägga till följande kod toohello hello **app.js** filen efter hello rad som innehåller `, nicknames = {};`.
  
        io.configure(function() {
          io.set('transports', ['websocket']);
        });
  
  > [!NOTE]
  > Observera att gamla webbläsare som inte stöder WebSockets inte kan tooconnect toohello plats medan hello ovan koden är aktiv, eftersom den begränsar bara kommunikation tooWebSockets.
  > 
  > 
* **Använd SSL**
  
    WebSockets förlitar sig på vissa mindre används HTTP-huvuden, till exempel hello **uppgradera** huvud. Vissa mellanliggande nätverksenheter, till exempel webbproxyservrar, ta bort dessa huvuden. tooavoid det här problemet kan du upprätta hello WebSocket-anslutningen via SSL.
  
    Ett enkelt sätt tooaccomplish detta är tooconfigure Socket.IO för`match origin protocol`. Detta instruerar Socket.IO toosecure WebSockets kommunikation hello samma som hello ursprung HTTP/HTTPS-begäran för hello webbsida. Om en webbläsare använder en HTTPS-URL-toovisit din webbplats, vara framtida WebSocket-kommunikation via Socket.IO skyddad via SSL.
  
    toomodify det här exemplet tooenable den här konfigurationen, Lägg till följande kod toohello hello **app.js** filen efter hello rad som innehåller `, nicknames = {};`.
  
        io.configure(function() {
          io.set('match origin protocol', true);
        });
* **Kontrollera inställningarna för web.config**
  
    Azure-webbappar som värd för Node.js-program använda hello **web.config** filen tooroute inkommande begäranden toohello Node.js-program. WebSockets toofunction korrekt med Node.js-program, hello **web.config** måste innehålla följande post hello.
  
        <webSocket enabled="false"/>
  
    Detta inaktiverar hello IIS WebSockets modulen som innehåller sin egen implementering av WebSockets och står i konflikt med Node.js specifika WebSocket-moduler, till exempel Socket.IO. Om den här raden finns inte eller har angetts för`true`, kan detta hello orsaken till att hello WebSocket transport inte fungerar för ditt program.
  
    Normalt sett Node.js-program innehåller inte en **web.config** -fil, så Azure Websites genererar automatiskt en för Node.js-program när de har distribuerats. Eftersom den här filen genereras automatiskt på hello-servern, måste du använda hello FTP eller FTPS URL för din webbplats tooview den här filen. Du kan hitta hello FTP och FTPS URL: er för webbplatsen i hello klassiska portalen genom att välja ditt webbprogram och sedan hello **instrumentpanelen** länk. hello URL: er visas i hello **snabböversikten** avsnitt.
  
  > [!NOTE]
  > Hej **web.config** filen genereras bara av Azure Websites om programmet inte tillhandahåller någon. Om du anger en **web.config** filen i hello roten av projektet program kommer det att användas av Azure Web Apps.
  > 
  > 
  
    Om hello posten finns inte eller är värdet tooa för `true`, bör du skapa en **web.config** i hello roten för ditt Node.js-program och ange ett värde för `false`.  För referens anger hello nedan är ett standardvärde **web.config** för ett program som använder **app.js** som hello startpunkt.
  
        <?xml version="1.0" encoding="utf-8"?>
        <!--
             This configuration file is required if iisnode is used toorun node processes behind
             IIS or IIS Express.  For more information, visit:
  
             https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config
        -->
  
        <configuration>
          <system.webServer>
            <!-- Visit http://blogs.msdn.com/b/windowsazure/archive/2013/11/14/introduction-to-websockets-on-windows-azure-web-sites.aspx for more information on WebSocket support -->
            <webSocket enabled="false" />
            <handlers>
              <!-- Indicates that hello server.js file is a node.js web app toobe handled by hello iisnode module -->
              <add name="iisnode" path="app.js" verb="*" modules="iisnode"/>
            </handlers>
            <rewrite>
              <rules>
                <!-- Do not interfere with requests for node-inspector debugging -->
                <rule name="NodeInspector" patternSyntax="ECMAScript" stopProcessing="true">
                  <match url="^app.js\/debug[\/]?" />
                </rule>
  
                <!-- First we consider whether hello incoming URL matches a physical file in hello /public folder -->
                <rule name="StaticContent">
                  <action type="Rewrite" url="public{REQUEST_URI}"/>
                </rule>
  
                <!-- All other URLs are mapped toohello node.js web app entry point -->
                <rule name="DynamicContent">
                  <conditions>
                    <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="True"/>
                  </conditions>
                  <action type="Rewrite" url="app.js"/>
                </rule>
              </rules>
            </rewrite>
            <!--
              You can control how Node is hosted within IIS using hello following options:
                * watchedFiles: semi-colon separated list of files that will be watched for changes toorestart hello server
                * node_env: will be propagated toonode as NODE_ENV environment variable
                * debuggingEnabled - controls whether hello built-in debugger is enabled
  
              See https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config for a full list of options
            -->
            <!--<iisnode watchedFiles="web.config;*.js"/>-->
          </system.webServer>
        </configuration>
  
    Om programmet använder en startpunkt än **app.js**, måste du ersätta alla förekomster av **app.js** med hello rätt startpunkt. Till exempel ersätta **app.js** med **server.js**.

> [!NOTE]
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service], där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

## <a name="next-steps"></a>Nästa steg
I den här självstudiekursen du lärt dig hur toocreate en chattprogram finns i en Azure-webbapp. Du kan också vara värd för programmet som en Azure-molntjänst. Stegvisa instruktioner för hur tooaccomplish detta, se [skapa ett Node.js-chatta program med Socket.IO i Azure-molntjänst].

Mer information finns också hello [Node.js Developer Center].

## <a name="whats-changed"></a>Nyheter
* En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster].

<!-- URL List -->

[Azure Redis-Cache]: /documentation/services/redis-cache/
[App Service Web Apps]: http://go.microsoft.com/fwlink/?LinkId=529714
[prissättning för Web Apps]: http://go.microsoft.com/fwlink/?LinkId=511643
[skapa ett Node.js-chatta program med Socket.IO i Azure-molntjänst]: ../cloud-services/cloud-services-nodejs-chat-app-socketio.md
[Install and Configure hello Azure CLI]: ../cli-install-nodejs.md
[Azure App Service och dess påverkan på befintliga Azure-tjänster]: http://go.microsoft.com/fwlink/?LinkId=529714
[Node.js Developer Center]: /develop/nodejs/
[prova App Service]: https://azure.microsoft.com/try/app-service/
[instans tillhörighet i Azure webbplatser]: https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/
[skapa en cache i Azure Redis-Cache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md

[socket.io redis]: https://github.com/socketio/socket.io-redis
[Socket.IO GitHub-lagringsplatsen]: https://github.com/socketio/socket.io
[ZIP- eller GZ arkiveras versionen]: https://github.com/socketio/socket.io/releases

<!-- IMG List -->

[chat-example-view]: ./media/web-sites-nodejs-chat-app-socketio/socketio-2.png
[npm-output]: ./media/web-sites-nodejs-chat-app-socketio/socketio-7.png
[completed-app]: ./media/web-sites-nodejs-chat-app-socketio/websitesocketcomplete.png

---
title: aaaTesting Azure Functions | Microsoft Docs
description: "Testa dina Azure funktioner med hjälp av Postman cURL och Node.js."
services: functions
documentationcenter: na
author: wesmc7777
manager: erikre
editor: 
tags: 
keywords: "Azure funktioner, funktioner, händelsebearbetning, webhooks, dynamiska beräkning, serverlösa arkitektur och testning"
ms.assetid: c00f3082-30d2-46b3-96ea-34faf2f15f77
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/02/2017
ms.author: wesmc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a084f8dbc8089356c3c19d789dc9098f2bb63052
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="strategies-for-testing-your-code-in-azure-functions"></a>Strategier för att testa din kod i Azure Functions

Det här avsnittet visar hello olika sätt tootest funktioner, inklusive användning av hello följande allmänna närmar sig:

+ HTTP-baserade verktyg som cURL, Postman och även en webbläsare för Webbaserad utlösare
+ Azure Lagringsutforskaren tootest Azure Storage-baserade utlösare
+ Testa fliken i hello Azure Functions-portalen
+ Funktion som utlöses av timer
+ Testa programmet eller framework

Alla dessa metoder funktionen en HTTP-utlösare som accepterar indata via antingen en query sträng parameter eller hello brödtext i begäran. Du skapar den här funktionen i hello första avsnittet.

## <a name="create-a-function-for-testing"></a>Skapa en funktion för testning
För merparten av den här kursen använder vi en lite annorlunda version av hello HttpTrigger JavaScript funktionen mall som är tillgängliga när du skapar en funktion. Om du behöver hjälp med att skapa en funktion kan du granska detta [kursen](functions-create-first-azure-function.md). Välj hello **HttpTrigger - JavaScript** mallen när du skapar hello funktionsnamnet i hello [Azure-portalen].

hello standardmallen för funktionen är i grunden en ”hello world”-funktion som ekon tillbaka hello namn från hello begäran brödtext eller fråga strängparameter, `name=<your name>`.  Vi uppdaterar hello kod tooalso kan du tooprovide hello namn och en adress som JSON-innehåll i hello frågans brödtext. Hello funktionen ekon sedan dessa tillbaka toohello klienten när det är tillgängligt.   

Uppdatera hello-funktionen med följande kod, som ska användas för att testa hello:

```javascript
module.exports = function (context, req) {
    context.log("HTTP trigger function processed a request. RequestUri=%s", req.originalUrl);
    context.log("Request Headers = " + JSON.stringify(req.headers));
    var res;

    if (req.query.name || (req.body && req.body.name)) {
        if (typeof req.query.name != "undefined") {
            context.log("Name was provided as a query string param...");
            res = ProcessNewUserInformation(context, req.query.name);
        }
        else {
            context.log("Processing user info from request body...");
            res = ProcessNewUserInformation(context, req.body.name, req.body.address);
        }
    }
    else {
        res = {
            status: 400,
            body: "Please pass a name on hello query string or in hello request body"
        };
    }
    context.done(null, res);
};
function ProcessNewUserInformation(context, name, address) {
    context.log("Processing user information...");
    context.log("name = " + name);
    var echoString = "Hello " + name;
    var res;

    if (typeof address != "undefined") {
        echoString += "\n" + "hello address you provided is " + address;
        context.log("address = " + address);
    }
    res = {
        // status: 200, /* Defaults too200 */
        body: echoString
    };
    return res;
}
```

## <a name="test-a-function-with-tools"></a>Testa en funktion med verktyg
Det finns olika verktyg som du kan använda tootrigger dina funktioner för att testa utanför hello Azure-portalen. Dessa inkluderar HTTP testning verktyg (både UI-baserade och kommandot rad), verktyg för Azure Storage-åtkomst och även en enkel webbläsare.

### <a name="test-with-a-browser"></a>Testa med en webbläsare
hello webbläsaren är ett enkelt sätt tootrigger fungerar via HTTP. Du kan använda en webbläsare för GET-begäranden som inte kräver en brödtext nyttolast och att använda endast fråga strängparametrar.

tootest hello funktionen vi definierade tidigare, kopiera hello **funktions-Url** från hello-portalen. Det har hello följande format:

    https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>

Lägg till hello `name` parametern toohello frågesträngen. Använda ett faktiska namn för hello `<Enter a name here>` platshållare.

    https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>&name=<Enter a name here>

Klistra in hello URL i webbläsaren, och du bör få en liknande toohello följande i svaret.

![Skärmbild av Chrome webbläsarflik med test-svar](./media/functions-test-a-function/browser-test.png)

Det här exemplet är hello webbläsaren Chrome som omsluter hello returnerade sträng i XML. Andra webbläsare bara hello strängvärde.

I hello portal **loggar** fönstret utdata liknande toohello följande loggas vid körning hello funktionen:

    2016-03-23T07:34:59  Welcome, you are now connected toolog-streaming service.
    2016-03-23T07:35:09.195 Function started (Id=61a8c5a9-5e44-4da0-909d-91d293f20445)
    2016-03-23T07:35:10.338 Node.js HTTP trigger function processed a request. RequestUri=https://functionsExample.azurewebsites.net/api/WesmcHttpTriggerNodeJS1?code=XXXXXXXXXX==&name=Glenn from a browser
    2016-03-23T07:35:10.338 Request Headers = {"cache-control":"max-age=0","connection":"Keep-Alive","accept":"text/html","accept-encoding":"gzip","accept-language":"en-US"}
    2016-03-23T07:35:10.338 Name was provided as a query string param.
    2016-03-23T07:35:10.338 Processing User Information...
    2016-03-23T07:35:10.369 Function completed (Success, Id=61a8c5a9-5e44-4da0-909d-91d293f20445)

### <a name="test-with-postman"></a>Testa med Postman
hello rekommenderas verktyget tootest det mesta av dina funktioner är Postman som integreras med hello Chrome webbläsare. tooinstall Postman, se [hämta Postman](https://www.getpostman.com/). Postman ger kontroll över många fler attribut för en HTTP-begäran.

> [!TIP]
> Verktyget hello HTTP tester som du är mest bekväm med. Här följer några alternativ tooPostman:  
>
> * [Fiddler](http://www.telerik.com/fiddler)  
> * [Paw](https://luckymarmot.com/paw)  
>
>

tootest hello funktion med en begärantext i Postman:

1. Starta Postman från hello **appar** i hello övre vänstra hörnet av Chrome-webbläsarfönstret.
2. Kopiera ditt **funktions-Url**, och klistra in den i Postman. Den omfattar hello åtkomst kod frågesträngparametern.
3. Ändra hello HTTP-metod för**efter**.
4. Klicka på **brödtext** > **raw**, och Lägg till en JSON-begäran brödtext liknande toohello följande:

    ```json
    {
        "name" : "Wes testing with Postman",
        "address" : "Seattle, WA 98101"
    }
    ```
5. Klicka på **skicka**.

hello visar följande bild tester hello enkla echo funktionen exempel i den här självstudiekursen.

![Skärmbild av Postman användargränssnitt](./media/functions-test-a-function/postman-test.png)

I hello portal **loggar** fönstret utdata liknande toohello följande loggas vid körning hello funktionen:

    2016-03-23T08:04:51  Welcome, you are now connected toolog-streaming service.
    2016-03-23T08:04:57.107 Function started (Id=dc5db8b1-6f1c-4117-b5c4-f6b602d538f7)
    2016-03-23T08:04:57.763 HTTP trigger function processed a request. RequestUri=https://functions841def78.azurewebsites.net/api/WesmcHttpTriggerNodeJS1?code=XXXXXXXXXX==
    2016-03-23T08:04:57.763 Request Headers = {"cache-control":"no-cache","connection":"Keep-Alive","accept":"*/*","accept-encoding":"gzip","accept-language":"en-US"}
    2016-03-23T08:04:57.763 Processing user info from request body...
    2016-03-23T08:04:57.763 Processing User Information...
    2016-03-23T08:04:57.763 name = Wes testing with Postman
    2016-03-23T08:04:57.763 address = Seattle, W.A. 98101
    2016-03-23T08:04:57.795 Function completed (Success, Id=dc5db8b1-6f1c-4117-b5c4-f6b602d538f7)

### <a name="test-with-curl-from-hello-command-line"></a>Testa med cURL från hello kommandorad
Ofta när du testar programmet är det inte nödvändigt toolook alla ytterligare än hello kommandoraden toohelp debug ditt program. Detta är inte annorlunda med testning funktioner. Observera att hello cURL är tillgängliga som standard på Linux-baserade system. I Windows, måste du först hämta och installera hello [cURL verktyget](https://curl.haxx.se/).

tootest hello funktion som vi definierade tidigare, kopiera hello **funktions-URL** från hello-portalen. Det har hello följande format:

    https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>

Det här är hello URL för att utlösa funktionen. Testa detta genom att använda hello cURL-kommando på hello kommandoraden toomake GET (`-G` eller `--get`) begäran mot hello funktionen:

    curl -G https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>

Det här exemplet kräver en frågesträngsparameter som kan skickas som Data (`-d`) i hello cURL-kommando:

    curl -G https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code> -d name=<Enter a name here>

Kör hello, och du se hello följande utdata för hello-funktionen på hello kommandoraden:

![Skärmbild av kommandotolk-utdata](./media/functions-test-a-function/curl-test.png)

I hello portal **loggar** fönstret utdata liknande toohello följande loggas vid körning hello funktionen:

    2016-04-05T21:55:09  Welcome, you are now connected toolog-streaming service.
    2016-04-05T21:55:30.738 Function started (Id=ae6955da-29db-401a-b706-482fcd1b8f7a)
    2016-04-05T21:55:30.738 Node.js HTTP trigger function processed a request. RequestUri=https://functionsExample.azurewebsites.net/api/HttpTriggerNodeJS1?code=XXXXXXX&name=Azure Functions
    2016-04-05T21:55:30.738 Function completed (Success, Id=ae6955da-29db-401a-b706-482fcd1b8f7a)

### <a name="test-a-blob-trigger-by-using-storage-explorer"></a>Testa en blob-utlösare med Lagringsutforskaren
Du kan testa en utlösare blob-funktion med hjälp av [Azure Lagringsutforskaren](http://storageexplorer.com/).

1. I hello [Azure-portalen] för funktionen appen, skapa en C#, F # eller JavaScript blob Utlösarfunktion. Ange hello toomonitor toohello sökvägen för blob-behållare. Exempel:

        files
2. Klicka på hello  **+**  knappen tooselect eller skapa hello-lagringskonto som du vill toouse. Klicka sedan på **Skapa**.
3. Skapa en textfil med hello följande text och spara den:

        A text file for blob trigger function testing.
4. Kör [Azure Lagringsutforskaren](http://storageexplorer.com/), och Anslut toohello blob-behållaren i hello storage-konto som övervakas.
5. Klicka på **överför** tooupload hello textfil.

    ![Skärmbild av Lagringsutforskaren](./media/functions-test-a-function/azure-storage-explorer-test.png)

hello standard blob utlösaren Funktionskoden rapporterar hello bearbetningen av hello blob i hello loggar:

    2016-03-24T11:30:10  Welcome, you are now connected toolog-streaming service.
    2016-03-24T11:30:34.472 Function started (Id=739ebc07-ff9e-4ec4-a444-e479cec2e460)
    2016-03-24T11:30:34.472 C# Blob trigger function processed: A text file for blob trigger function testing.
    2016-03-24T11:30:34.472 Function completed (Success, Id=739ebc07-ff9e-4ec4-a444-e479cec2e460)

## <a name="test-a-function-within-functions"></a>Testa en funktion i funktioner
hello Azure Functions-portalen är utformad toolet du testa HTTP och tidsinställda utlösts funktioner. Du kan också skapa funktioner tootrigger andra funktioner som du vill testa.

### <a name="test-with-hello-functions-portal-run-button"></a>Testa med portalen kör-knappen i hello funktion
Hej och ger en **kör** knapp som du kan använda toodo vissa begränsade testning. Du kan ange en begärantext hello-knappen, men du kan inte ange frågeparametrar sträng eller uppdaterar huvuden för begäran.

Testa hello HTTP utlösaren funktionen som vi skapade tidigare genom att lägga till en JSON-sträng liknande toohello följande i hello **Begärandetext** fältet. Klicka på hello **kör** knappen.

```json
{
    "name" : "Wes testing Run button",
    "address" : "USA"
}
```

I hello portal **loggar** fönstret utdata liknande toohello följande loggas vid körning hello funktionen:

    2016-03-23T08:03:12  Welcome, you are now connected toolog-streaming service.
    2016-03-23T08:03:17.357 Function started (Id=753a01b0-45a8-4125-a030-3ad543a89409)
    2016-03-23T08:03:18.697 HTTP trigger function processed a request. RequestUri=https://functions841def78.azurewebsites.net/api/wesmchttptriggernodejs1
    2016-03-23T08:03:18.697 Request Headers = {"connection":"Keep-Alive","accept":"*/*","accept-encoding":"gzip","accept-language":"en-US"}
    2016-03-23T08:03:18.697 Processing user info from request body...
    2016-03-23T08:03:18.697 Processing User Information...
    2016-03-23T08:03:18.697 name = Wes testing Run button
    2016-03-23T08:03:18.697 address = USA
    2016-03-23T08:03:18.744 Function completed (Success, Id=753a01b0-45a8-4125-a030-3ad543a89409)


### <a name="test-with-a-timer-trigger"></a>Testa med en timer som utlösare
Vissa funktioner kan inte testas tillräckligt med hello verktyg som tidigare nämnts. Anta till exempel att en kö Utlösarfunktion som körs när ett meddelande har släppts i [Azure Queue storage](../storage/queues/storage-dotnet-how-to-use-queues.md). Du skriva alltid kod toodrop meddelandet i kön och ett exempel på detta i konsolen projekt tillhandahålls nedan. Det finns en annan metod som du kan använda som testar funktioner direkt.  

Du kan använda en timer som utlösare konfigurerats med en kö utdatabindning. Timer utlösaren koden kan sedan skriva hello test toohello kön. Det här avsnittet går igenom ett exempel.

Mer information om hur du använder bindningar med Azure Functions finns i avsnittet hello [Azure Functions för utvecklare](functions-reference.md).

#### <a name="create-a-queue-trigger-for-testing"></a>Skapa en kö utlösare för testning
toodemonstrate den här metoden vi först skapa en kö Utlösarfunktion som vi vill tootest för en kö med namnet `queue-newusers`. Den här funktionen bearbetas namn och adress information släppa i Queue storage för en ny användare.

> [!NOTE]
> Om du använder en annan kö, kontrollera hello-namn som du använder överensstämmer toohello [namngivning av köer och MetaData](https://msdn.microsoft.com/library/dd179349.aspx) regler. Annars får du ett felmeddelande.
>
>

1. I hello [Azure-portalen] appen funktionen klickar du på **nya funktionen** > **QueueTrigger - C#**.
2. Ange hello kön namn toobe övervakas av hello kön funktionen:

        queue-newusers
3. Klicka på hello  **+**  knappen tooselect eller skapa hello-lagringskonto som du vill toouse. Klicka sedan på **Skapa**.
4. Lämna den här portalen webbläsarfönster öppen, så att du kan övervaka hello loggposter för mallen för hello standard kön funktionskoden.

#### <a name="create-a-timer-trigger-toodrop-a-message-in-hello-queue"></a>Skapa en timer utlösaren toodrop ett meddelande i kön hello
1. Öppna hello [Azure-portalen] i ett nytt webbläsarfönster och navigera tooyour funktionsapp.
2. Klicka på **nya funktionen** > **TimerTrigger - C#**. Ange en cron uttryck tooset hur ofta hello timer kod testar kö-funktionen. Klicka sedan på **Skapa**. Om du vill hello test toorun med 30 sekunders mellanrum, kan du använda följande hello [CRON-uttryck](https://wikipedia.org/wiki/Cron#CRON_expression):

        */30 * * * * *
3. Klicka på hello **integrera** för din nya timer som utlösare.
4. Under **utdata**, klickar du på **+ nya utdata**. Klicka på **kön** och **Välj**.
5. Obs hello namn i hello **meddelandet köobjekt**. Du kan använda den i hello timer funktionskoden.

        myQueue
6. Ange hello könamnet där hello-meddelande skickas:

        queue-newusers
7. Klicka på hello  **+**  knappen tooselect hello storage-konto som du tidigare använt med hello kö utlösaren. Klicka sedan på **Spara**.
8. Klicka på hello **utveckla** för din timer som utlösare.
9. Du kan använda hello följande kod för hello C# timer-funktionen, så länge som du använde hello samma kö meddelandet objektnamn som visades tidigare. Klicka sedan på **Spara**.

    ```cs
    using System;

    public static void Run(TimerInfo myTimer, out String myQueue, TraceWriter log)
    {
        String newUser =
        "{\"name\":\"User testing from C# timer function\",\"address\":\"XYZ\"}";

        log.Verbose($"C# Timer trigger function executed at: {DateTime.Now}");   
        log.Verbose($"{newUser}");   

        myQueue = newUser;
    }
    ```

Hello C# funktionen timer kör nu med 30 sekunders mellanrum om du använde hello exempel cron-uttryck. hello loggar för hello timer funktionen rapportera varje körning:

    2016-03-24T10:27:02  Welcome, you are now connected toolog-streaming service.
    2016-03-24T10:27:30.004 Function started (Id=04061790-974f-4043-b851-48bd4ac424d1)
    2016-03-24T10:27:30.004 C# Timer trigger function executed at: 3/24/2016 10:27:30 AM
    2016-03-24T10:27:30.004 {"name":"User testing from C# timer function","address":"XYZ"}
    2016-03-24T10:27:30.004 Function completed (Success, Id=04061790-974f-4043-b851-48bd4ac424d1)

Du kan se varje meddelande bearbetas i hello webbläsarfönster för hello kön funktionen:

    2016-03-24T10:27:06  Welcome, you are now connected toolog-streaming service.
    2016-03-24T10:27:30.607 Function started (Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)
    2016-03-24T10:27:30.607 C# Queue trigger function processed: {"name":"User testing from C# timer function","address":"XYZ"}
    2016-03-24T10:27:30.607 Function completed (Success, Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)

## <a name="test-a-function-with-code"></a>Testa en funktion med kod
Du kan behöva toocreate ett externt program eller framework tootest dina funktioner.

### <a name="test-an-http-trigger-function-with-code-nodejs"></a>Testa en HTTP-Utlösarfunktion med kod: Node.js
Du kan använda en Node.js-app tooexecute en HTTP-begäran tootest din funktion.
Se till att tooset:

* Hej `host` hello begäran alternativ tooyour funktionen app värden.
* Din funktionsnamnet i hello `path`.
* Koden åtkomst (`<your code>`) i hello `path`.

Exempel:

```javascript
var http = require("http");

var nameQueryString = "name=Wes%20Query%20String%20Test%20From%20Node.js";

var nameBodyJSON = {
    name : "Wes testing with Node.JS code",
    address : "Dallas, T.X. 75201"
};

var bodyString = JSON.stringify(nameBodyJSON);

var options = {
  host: "functions841def78.azurewebsites.net",
  //path: "/api/HttpTriggerNodeJS2?code=sc1wt62opn7k9buhrm8jpds4ikxvvj42m5ojdt0p91lz5jnhfr2c74ipoujyq26wab3wk5gkfbt9&" + nameQueryString,
  path: "/api/HttpTriggerNodeJS2?code=sc1wt62opn7k9buhrm8jpds4ikxvvj42m5ojdt0p91lz5jnhfr2c74ipoujyq26wab3wk5gkfbt9",
  method: "POST",
  headers : {
      "Content-Type":"application/json",
      "Content-Length": Buffer.byteLength(bodyString)
    }    
};

callback = function(response) {
  var str = ""
  response.on("data", function (chunk) {
    str += chunk;
  });

  response.on("end", function () {
    console.log(str);
  });
}

var req = http.request(options, callback);
console.log("*** Sending name and address in body ***");
console.log(bodyString);
req.end(bodyString);
```


Resultat:

    C:\Users\Wesley\testing\Node.js>node testHttpTriggerExample.js
    *** Sending name and address in body ***
    {"name" : "Wes testing with Node.JS code","address" : "Dallas, T.X. 75201"}
    Hello Wes testing with Node.JS code
    hello address you provided is Dallas, T.X. 75201

I hello portal **loggar** fönstret utdata liknande toohello följande loggas vid körning hello funktionen:

    2016-03-23T08:08:55  Welcome, you are now connected toolog-streaming service.
    2016-03-23T08:08:59.736 Function started (Id=607b891c-08a1-427f-910c-af64ae4f7f9c)
    2016-03-23T08:09:01.153 HTTP trigger function processed a request. RequestUri=http://functionsExample.azurewebsites.net/api/WesmcHttpTriggerNodeJS1/?code=XXXXXXXXXX==
    2016-03-23T08:09:01.153 Request Headers = {"connection":"Keep-Alive","host":"functionsExample.azurewebsites.net"}
    2016-03-23T08:09:01.153 Name not provided as query string param. Checking body...
    2016-03-23T08:09:01.153 Request Body Type = object
    2016-03-23T08:09:01.153 Request Body = [object Object]
    2016-03-23T08:09:01.153 Processing User Information...
    2016-03-23T08:09:01.215 Function completed (Success, Id=607b891c-08a1-427f-910c-af64ae4f7f9c)


### <a name="test-a-queue-trigger-function-with-code-c"></a>Testa en kö Utlösarfunktion med koden: C# #
Vi nämnt tidigare kan du testa en utlösare för kön med hjälp av koden toodrop meddelandet i kön. Följande exempelkod hello baseras på hello C#-koden som visas i hello [komma igång med Azure Queue storage](../storage/queues/storage-dotnet-how-to-use-queues.md) kursen. Koden för andra språk är också tillgänglig från den länken.

tootest denna kod i en konsolapp måste du:

* [Konfigurera anslutningssträngen för lagring i hello app.config-filen](../storage/queues/storage-dotnet-how-to-use-queues.md).
* Skicka en `name` och `address` som parametrar toohello app. Till exempel `C:\myQueueConsoleApp\test.exe "Wes testing queues" "in a console app"`. (Den här koden accepterar hello namn och adress för en ny användare som kommandoradsargument under körningen.)

C#-exempelkod:

```cs
static void Main(string[] args)
{
    string name = null;
    string address = null;
    string queueName = "queue-newusers";
    string JSON = null;

    if (args.Length > 0)
    {
        name = args[0];
    }
    if (args.Length > 1)
    {
        address = args[1];
    }

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConfigurationManager.AppSettings["StorageConnectionString"]);

    // Create hello queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference tooa queue
    CloudQueue queue = queueClient.GetQueueReference(queueName);

    // Create hello queue if it doesn't already exist
    queue.CreateIfNotExists();

    // Create a message and add it toohello queue.
    if (name != null)
    {
        if (address != null)
            JSON = String.Format("{{\"name\":\"{0}\",\"address\":\"{1}\"}}", name, address);
        else
            JSON = String.Format("{{\"name\":\"{0}\"}}", name);
    }

    Console.WriteLine("Adding message too" + queueName + "...");
    Console.WriteLine(JSON);

    CloudQueueMessage message = new CloudQueueMessage(JSON);
    queue.AddMessage(message);
}
```

Du kan se varje meddelande bearbetas i hello webbläsarfönster för hello kön funktionen:

    2016-03-24T10:27:06  Welcome, you are now connected toolog-streaming service.
    2016-03-24T10:27:30.607 Function started (Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)
    2016-03-24T10:27:30.607 C# Queue trigger function processed: {"name":"Wes testing queues","address":"in a console app"}
    2016-03-24T10:27:30.607 Function completed (Success, Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)


<!-- URLs. -->

[Azure-portalen]: https://portal.azure.com

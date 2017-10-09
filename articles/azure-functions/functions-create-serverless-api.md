---
title: "aaaCreate ett serverlösa API med hjälp av Azure Functions | Microsoft Docs"
description: "Hur toocreate ett serverlösa API med hjälp av Azure-funktioner"
services: functions
author: mattchenderson
manager: erikre
ms.service: functions
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: tutorial
ms.date: 05/04/2017
ms.author: mahender
ms.custom: mvc
ms.openlocfilehash: 877e3b229d5477fc5fec594ccd284fb55d7f3c07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-serverless-api-using-azure-functions"></a>Skapa en serverlösa API med hjälp av Azure-funktioner

I den här kursen får du lära dig hur Azure Functions kan du toobuild mycket skalbar API: er. Azure Functions innehåller en uppsättning inbyggda HTTP-utlösare och bindningar som gör det enkelt tooauthor en slutpunkt på flera olika språk, inklusive Node.JS, C# och mycket mer. I den här självstudiekursen kommer du anpassa en HTTP-utlösaren toohandle specifika åtgärder i utformningen av din API. Dessutom förbereder du för växande din API genom att integrera med Azure Functions proxyservrar och ställa in fingerad API: er. Allt detta sker ovanpå hello funktioner serverlösa beräkning miljö, så att du inte har tooworry om att skala resurser – du kan bara fokusera på din API-logik.

## <a name="prerequisites"></a>Krav 

[!INCLUDE [Previous quickstart note](../../includes/functions-quickstart-previous-topics.md)]

hello resulterande funktionen används för hello resten av den här kursen.

### <a name="sign-in-tooazure"></a>Logga in tooAzure

Öppna hello Azure-portalen. toodo, logga in för[https://portal.azure.com](https://portal.azure.com) med ditt Azure-konto.

## <a name="customize-your-http-function"></a>Anpassa din HTTP-funktion

HTTP-utlösta-funktionen är som standard konfigurerade tooaccept HTTP-metoden. Det finns också en standard-URL i formatet hello `http://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>`. Om du har följt hello quickstart sedan `<funcname>` förmodligen ser ut ungefär så ”HttpTriggerJS1”. I det här avsnittet ska du ändra hello funktionen toorespond endast tooGET förfrågningar mot `/api/hello` dirigera i stället. 

Navigera tooyour funktion i hello Azure-portalen. Välj **integrera** i hello vänstra navigeringsrutan.

![Anpassa en HTTP-funktion](./media/functions-create-serverless-api/customizing-http.png)

Använda HTTP-inställningarna som anges i hello tabell.

| Fält | Exempelvärde | Beskrivning |
|---|---|---|
| Tillåtna HTTP-metoder | Valda metoder | Bestämmer vilka HTTP-metoder kan vara används tooinvoke den här funktionen |
| Den valda http-metoder | HÄMTA | Tillåter endast valda http-metoderna toobe används tooinvoke för den här funktionen |
| Flödesmallen | Sverige | Anger vilka vägen används tooinvoke den här funktionen |

Observera att du inte inkluderade hello `/api` basera sökväg prefix i hello flödesmallen som hanteras av en global inställning.

Klicka på **Spara**.

Du kan lära dig mer om hur du anpassar http-funktioner i [Azure Functions HTTP och webhook bindningar](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook#customizing-the-http-endpoint).

### <a name="test-your-api"></a>Testa ditt API

Sedan testa funktionen-toosee den arbetar med hello nya API-ytan.

Gå tillbaka toohello development sidan genom att klicka på hello funktionens namn i hello vänstra navigeringsrutan.

Klicka på **få funktionen URL** och kopiera hello-URL. Du bör se till att den använder hello `/api/hello` vidarebefordra nu.

Kopiera hello URL till en ny webbläsarflik eller önskade REST-klient. Webbläsare använder GET som standard.

Kör hello-funktionen och bekräfta att den fungerar. Du kan behöva tooprovide hello ”name”-parametern som en fråga sträng toosatisfy hello quickstart kod.

Du kan också försöka hello slutpunkt med ett annat HTTP-metoden tooconfirm inte utförs hello-funktionen anropas. För att göra detta behöver du toouse en REST-klient, till exempel cURL, Postman eller Fiddler.

## <a name="proxies-overview"></a>Översikt över proxyservrar

I nästa avsnitt om hello, kommer du ansluta din API via en proxyserver. Azure Functions proxyservrar är en förhandsversion av funktionen som gör att du tooforward begäranden tooother resurser. Du definierar en HTTP-slutpunkt precis som med HTTP-utlösare, men i stället för att skriva kod tooexecute när denna slutpunkt anropas, du anger en URL tooa remote implementering. Detta ger dig toocompose flera API datakällor i en enda API-yta som är enkelt för klienter tooconsume. Detta är särskilt användbart om du inte vill toobuild din API som mikrotjänster.

En proxy kan peka tooany HTTP-resurs, som:
- Azure Functions 
- API apps i [Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-value-prop-what-is)
- Docker-behållare i [Apptjänst på Linux](https://docs.microsoft.com/azure/app-service/app-service-linux-readme)
- Värdbaserade API

toolearn mer information om proxyservrar, se [arbeta med Azure Functions-proxyservrar (förhandsgranskning)].

## <a name="create-your-first-proxy"></a>Skapa din första proxy

I det här avsnittet skapar du en ny proxy som fungerar som en klientdel tooyour övergripande API. 

### <a name="setting-up-hello-frontend-environment"></a>Konfigurera hello frontend-miljö

Upprepa hello steg för[skapa en funktionsapp](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function#create-a-function-app) toocreate en ny funktionsapp i som du skapar din proxyserver. Den här nya appen fungerar som klientdel hello för vårt API och hello funktionsapp du redigerade tidigare fungerar som en serverdel.

Navigera tooyour ny klientdel funktionsapp hello-portalen.

Välj **inställningar**. Sedan växla **aktivera Azure Functions proxyservrar (förhandsgranskning)** för ”On”.

Välj **plattform inställningar** och välj **programinställningar**.

Rulla nedåt för**appinställningar** och skapa en ny inställning med nyckeln ”HELLO_HOST”. Ange värdet toohello värden för din app för backend-funktion, till exempel `<YourApp>.azurewebsites.net`. Detta är en del av hello-URL som du kopierade tidigare när du testar din HTTP-funktion. Du måste referera till den här inställningen i konfigurationen för hello senare.

> [!NOTE] 
> App-inställningar rekommenderas för hello värden configuration tooprevent ett hårdkodat miljö beroende för hello-proxy. Med hjälp av app-inställningar innebär att du kan flytta hello proxykonfiguration mellan miljöer och hälsning miljöspecifikt appinställningar tillämpas.

Klicka på **Spara**.

### <a name="creating-a-proxy-on-hello-frontend"></a>Att skapa en proxy på hello klientdel

Gå tillbaka tooyour klientdel funktionsapp hello-portalen.

I hello vänstra navigeringsfönstret klickar du på hello plustecknet '+' Nästa för ”proxyservrar (förhandsgranskning)”.

![Att skapa en proxy](./media/functions-create-serverless-api/creating-proxy.png)

Använd proxy-inställningar som anges i hello tabell.

| Fält | Exempelvärde | Beskrivning |
|---|---|---|
| Namn | HelloProxy | Ett eget namn som används endast för hantering |
| Flödesmallen | / api/hello | Anger vilka vägen används tooinvoke denna proxy |
| Backend-URL | https://%HELLO_HOST%/API/hello | Anger hello endpoint toowhich hello begäran bör vara via proxy |

Observera att proxyservrar inte innehåller hello `/api` bassökväg prefix och måste tas med i hello flödesmallen.

Hej `%HELLO_HOST%` syntax refererar hello appinställning du skapade tidigare. hello matcha URL: en pekar tooyour ursprungliga funktion.

Klicka på **Skapa**.

Du kan testa den nya proxyserverkonfigurationen genom att kopiera hello Proxy-URL och testa det i hello webbläsare eller med din favorit HTTP-klienten.

## <a name="create-a-mock-api"></a>Skapa en fingerad API

Sedan använder du en proxy-toocreate en fingerad API för din lösning. Detta gör att klienten development tooprogress utan att behöva hello backend fullt ut. Senare i utveckling, kan du skapa en ny funktionsapp som stöder denna logik och omdirigera proxy-tooit.

toocreate detta mock API, skapas en ny proxy med hjälp hello [App Service Editor](https://github.com/projectkudu/kudu/wiki/App-Service-Editor). tooget igång, navigera tooyour funktionsapp hello-portalen. Välj **plattformsfunktioner** och hitta **App Service Editor**. Klicka på det här öppnas hello App Service Editor i en ny flik.

Välj `proxies.json` i hello vänstra navigeringsrutan. Detta är hello-fil som lagrar hello konfiguration för alla dina proxyservrar. Om du använder en hello [fungerar distributionsmetoder](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment), detta är hello-fil som du ska behålla i källkontroll. toolearn mer om den här filen finns [proxyservrar avancerad konfiguration](https://docs.microsoft.com/azure/azure-functions/functions-proxies#advanced-configuration).

Om du har följt hittills, bör din proxies.json se ut hello följande:

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "HelloProxy": {
            "matchCondition": {
                "route": "/api/hello"
            },
            "backendUri": "https://%HELLO_HOST%/api/hello"
        }
    }
}
```

Härnäst ska du lägga till din fingerad API. Ersätt filen proxies.json med hello följande:

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "HelloProxy": {
            "matchCondition": {
                "route": "/api/hello"
            },
            "backendUri": "https://%HELLO_HOST%/api/hello"
        },
        "GetUserByName" : {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/users/{username}"
            },
            "responseOverrides": {
                "response.statusCode": "200",
                "response.headers.Content-Type" : "application/json",
                "response.body": {
                    "name": "{username}",
                    "description": "Awesome developer and master of serverless APIs",
                    "skills": [
                        "Serverless",
                        "APIs",
                        "Azure",
                        "Cloud"
                    ]
                }
            }
        }
    }
}
```

Detta lägger till en ny proxy, ”GetUserByName” utan hello backendUri egenskap. Funktionen ändrar hello Standardsvar från proxyservrar med en åsidosättning för svar i stället för att anropa en annan resurs. Förfrågan och svar åsidosättningar kan också användas tillsammans med en backend-URL. Detta är särskilt användbart när proxyanslutning tooa äldre system, där du kanske behöver toomodify huvuden frågeparametrar, etc. toolearn mer om begäran och svar åsidosättningar, se [ändra begäranden och -svar i proxyservrar](https://docs.microsoft.com/azure/azure-functions/functions-proxies#a-namemodify-requests-responsesamodifying-requests-and-responses).

Testa din fingerad API genom att anropa hello `/api/users/{username}` slutpunkten med hjälp av en webbläsare eller ditt favoritprogram REST-klient. Vara säker på att tooreplace _{username}_ med ett strängvärde som representerar ett användarnamn.

## <a name="next-steps"></a>Nästa steg

I kursen får du lärt dig hur toobuild och anpassa en API på Azure Functions. Du också lära dig hur toobring flera API: er, inklusive mocks, samtidigt som en enhetlig API-ytan. Du kan använda dessa tekniker toobuild ut API: er för alla komplexitet, alla när du kör på hello serverlösa compute modellen som tillhandahålls av Azure Functions.

hello kan följande referenser vara till hjälp när du utvecklar dina API ytterligare:

- [Azure Functions HTTP och webhook bindningar](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook)
- [arbeta med Azure Functions-proxyservrar (förhandsgranskning)]
- [Dokumentera Azure Functions API (förhandsgranskning)](https://docs.microsoft.com/azure/azure-functions/functions-api-definition-getting-started)


[Create your first function]: https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function
[arbeta med Azure Functions-proxyservrar (förhandsgranskning)]: https://docs.microsoft.com/azure/azure-functions/functions-proxies

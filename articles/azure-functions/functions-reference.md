---
title: "aaaGuidance för att utveckla Azure Functions | Microsoft Docs"
description: "Lär dig hello Azure Functions koncept och tekniker som du behöver toodevelop funktioner i Azure, för alla programmeringsspråk och bindningar."
services: functions
documentationcenter: na
author: christopheranderson
manager: erikre
editor: 
tags: 
keywords: "utvecklare guide, azure-funktion, funktioner, händelsebearbetning, webhooks, dynamiska beräkning, serverlösa arkitektur"
ms.assetid: d8efe41a-bef8-4167-ba97-f3e016fcd39e
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/30/2017
ms.author: chrande
ms.openlocfilehash: 86d37dae5333f615faafc966e9da6e08e0a6354e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-developers-guide"></a>Azure Functions-guide för utvecklare
I Azure Functions dela funktioner några grundläggande tekniska begrepp och komponenter, oavsett hello språk eller bindning som du använder. Vara säker på att tooread via den här översikten som gäller tooall av dem innan du går till learning information specifik tooa språk eller bindning.

Den här artikeln förutsätter att du redan har läst hello [översikt över Azure Functions](functions-overview.md) och är bekant med [WebJobs SDK begrepp som utlösare och bindningar hello JobHost runtime](../app-service-web/websites-dotnet-webjobs-sdk.md). Azure Functions baseras på hello WebJobs-SDK. 

## <a name="function-code"></a>Funktionskoden
En *funktionen* är hello primära begrepp i Azure Functions. Du skriva kod för en funktion på ett annat språk och spara hello kod och konfigurationsfiler i hello samma mapp. hello configuration heter `function.json`, som innehåller JSON-konfigurationsdata. Olika språk som stöds och var och en har ett något annorlunda upplevelse optimerade toowork bäst för det språket. 

Hej function.json filen definierar hello funktionsbindningar och andra konfigurationsinställningar. hello runtime använder den här filen toodetermine hello händelser toomonitor och hur toopass data till och returnera data från att fungera körning. hello följande är ett exempel function.json-fil.

```json
{
    "disabled":false,
    "bindings":[
        // ... bindings here
        {
            "type": "bindingType",
            "direction": "in",
            "name": "myParamName",
            // ... more depending on binding
        }
    ]
}
```

Ange hello `disabled` egenskapen för`true` tooprevent hello funktion från utförs.

Hej `bindings` egenskapen är där du kan konfigurera både utlösare och bindningar. Varje bindning delar några vanliga inställningar och vissa inställningar som är specifika tooa viss typ av bindningen. Alla bindningar kräver hello följande inställningar:

| Egenskap | Värdetyper / | Kommentarer |
| --- | --- | --- |
| `type` |Sträng |Bindningstyp. Till exempel `queueTrigger`. |
| `direction` |'i', 'out' |Anger om hello-bindning för att ta emot data till hello funktionen eller skicka data från hello-funktionen. |
| `name` |Sträng |hello-namnet som används för hello bunden data i hello-funktionen. Detta är ett argumentnamn; för C# för JavaScript, det är hello nyckeln i en lista med nyckel/värde. |

## <a name="function-app"></a>Funktionsapp
En funktionsapp består av en eller flera enskilda funktioner som hanteras tillsammans i Azure App Service. Alla hello funktioner på en resurs som funktionen app hello samma priser plan, kontinuerlig distribution och versionen av körningsmiljön. Funktioner som skrivits i flera språk kan alla dela hello funktionsapp samma. Tänk på en funktionsapp som ett sätt tooorganize och gemensamt hantera dina funktioner. 

## <a name="runtime-script-host-and-web-host"></a>Runtime (skriptvärden och webbvärd)
hello runtime eller skriptvärden, är hello underliggande WebJobs SDK värden som lyssnar efter händelser, samlar in och skickar data och slutligen körs din kod. 

toofacilitate HTTP-utlösare, det finns också en värd som är utformad toosit framför hello skriptvärden i produktion scenarier. Med två värdar hjälper tooisolate hello script host från hello klientdelen trafiken hanteras av hello webbvärd.

## <a name="folder-structure"></a>Mappstruktur
[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

När inställningen upp ett projekt för distribution av funktioner tooa funktionsapp i Azure App Service, kan du behandla den här mappstrukturen som din Platskod. Du kan använda befintliga verktyg kontinuerlig integrering och distribution eller distribution av anpassade skript för detta distribuera tid installationen eller code transpilation.

> [!NOTE]
> Se till att toodeploy din `host.json` fil- och mappar direkt toohello `wwwroot` mapp. Inkludera inte hello `wwwroot` mapp i dina distributioner. Annars kan du få `wwwroot\wwwroot` mappar. 
> 
> 

## <a id="fileupdate"></a>Hur fungerar tooupdate programfilerna
hello funktionen editor inbyggd hello Azure-portalen kan du uppdatera hello *function.json* fil- och hello kodfilen för en funktion. tooupload eller uppdatera andra filer som *package.json* eller *project.json* eller beroenden, du har toouse andra metoder för distribution.

Funktionen appar bygger på Apptjänst, så alla hello [distributionsalternativ tillgängliga toostandard webbappar](../app-service-web/web-sites-deploy.md) är också tillgängliga för funktionen appar. Här följer några metoder som du kan använda tooupload eller uppdatera funktionen programfilerna. 

#### <a name="toouse-app-service-editor"></a>toouse App Service Editor
1. I hello Azure Functions-portalen klickar du på **fungerar appinställningar**.
2. I hello **avancerade inställningar** klickar du på **gå tooApp inställningar**.
3. Klicka på **App Service Editor** i Appmenyn Nav under **UTVECKLINGSVERKTYG**.
4. Klicka på **Gå**.
   
   När App Service Editor har lästs in, ser hello *host.json* fil- och mappar under *wwwroot*. 
5. Öppna filer tooedit dem, eller dra och släpp från tooupload för utveckling datorfiler.

#### <a name="toouse-hello-function-apps-scm-kudu-endpoint"></a>toouse hello funktionen appens SCM (Kudu) slutpunkt
1. Gå till: `https://<function_app_name>.scm.azurewebsites.net`.
2. Klicka på **Debug konsolen > CMD**.
3. Navigera för`D:\home\site\wwwroot\` tooupdate *host.json* eller `D:\home\site\wwwroot\<function_name>` tooupdate filer för en funktion.
4. Dra och släpp en fil som du vill tooupload till hello lämplig mapp i hello filen rutnät. Det finns två områden i hello filen rutnät där du kan släppa en fil. För *.zip* filer, visas en ruta med hello etiketten ”dra hit tooupload och packa”. Ta bort i rutnätet för hello-filen men utanför hello ”packa” för andra filtyper.

<!--NOTE: I've removed documentation on FTP, because it does not sync triggers on hello consumption plan --DonnaM -->

#### <a name="toouse-continuous-deployment"></a>toouse kontinuerlig distribution
Följ hello anvisningarna i avsnittet hello [kontinuerlig distribution för Azure Functions](functions-continuous-deployment.md).

## <a name="parallel-execution"></a>Parallell körning
När flera utlösande händelser inträffar snabbare än en enkeltrådig funktionen runtime kan bearbeta dem, kan hello körning anropa hello-funktionen flera gånger i parallellt.  Om en funktionsapp använder hello [förbrukning som värd för planen](functions-scale.md#how-the-consumption-plan-works), hello funktionsapp kan byggas ut automatiskt.  Varje instans av hello funktionsapp om hello appen körs på hello förbrukning värd plan eller en vanlig [Apptjänst som är värd för planen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), kan bearbeta samtidiga funktionsanrop parallellt med flera trådar.  hello maximalt antal samtidiga funktionsanrop i varje funktionen app-instansen varierar beroende på hello typen av utlösare som används samt hello resurser som används av andra funktioner i hello funktionsapp.

## <a name="functions-runtime-versioning"></a>Funktioner runtime versionshantering

Du kan konfigurera hello version av hello Functions-runtime med hello `FUNCTIONS_EXTENSION_VERSION` appinställningen. Till exempel anger hello värdet ”~ 1” att appen funktionen ska använda 1 enligt dess huvudversion. Funktionen appar är uppgraderade tooeach nya delversion när de blir tillgängliga. Du kan visa hello exakt vilken version av appen funktionen i hello **inställningar** fliken i hello Azure-portalen.

## <a name="repositories"></a>Centrallager
hello-koden för Azure Functions är öppen källkod och lagras i GitHub-databaser:

* [Azure Functions-runtime](https://github.com/Azure/azure-webjobs-sdk-script/)
* [Azure Functions-portalen](https://github.com/projectkudu/AzureFunctionsPortal)
* [Azure Functions-mallar](https://github.com/Azure/azure-webjobs-sdk-templates/)
* [Azure WebJobs-SDK](https://github.com/Azure/azure-webjobs-sdk/)
* [Azure WebJobs-SDK-tillägg](https://github.com/Azure/azure-webjobs-sdk-extensions/)

## <a name="bindings"></a>Bindningar
Här är en tabell med alla bindningar som stöds.

[!INCLUDE [dynamic compute](../../includes/functions-bindings.md)]

## <a name="reporting-issues"></a>Rapportera problem
[!INCLUDE [Reporting Issues](../../includes/functions-reporting-issues.md)]

## <a name="next-steps"></a>Nästa steg
Mer information finns i hello följande resurser:

* [Metodtips för Azure Functions](functions-best-practices.md)
* [Azure Functions C# för utvecklare](functions-reference-csharp.md)
* [Azure Functions F # för utvecklare](functions-reference-fsharp.md)
* [Azure Functions NodeJS för utvecklare](functions-reference-node.md)
* [Azure Functions-utlösare och bindningar](functions-triggers-bindings.md)
* [Azure Functions: hello resa](https://blogs.msdn.microsoft.com/appserviceteam/2016/04/27/azure-functions-the-journey/) på hello Azure App Service-teamets blogg. En historik över hur Azure Functions utvecklades.


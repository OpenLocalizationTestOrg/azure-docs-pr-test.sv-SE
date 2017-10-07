---
title: "aaaDevelop och kör Azure functions lokalt | Microsoft Docs"
description: "Lär dig hur toocode och testa Azure functions lokalt på datorn innan du kör dem på Azure Functions."
services: functions
documentationcenter: na
author: lindydonna
manager: erikre
editor: 
ms.assetid: 242736be-ec66-4114-924b-31795fd18884
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: article
ms.date: 07/12/2017
ms.author: glenga
ms.openlocfilehash: 342ed4d6df41a2d2df9067948e19e347bb52c844
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="code-and-test-azure-functions-locally"></a>Platskod och testa Azure functions lokalt

När hello [Azure-portalen] ger en fullständig uppsättning av verktyg för att utveckla och testa Azure Functions, många utvecklare föredrar en lokal utveckling upplevelse. Azure Functions gör det enkelt toouse din favorit code redigerare och lokal utveckling verktyg toodevelop och testa dina funktioner på den lokala datorn. Dina funktioner kan utlösa händelser i Azure och du kan felsöka ditt C# och JavaScript-funktioner på den lokala datorn. 

Om du är en Visual Studio C# utvecklare Azure Functions även [kan integreras med Visual Studio 2017](functions-develop-vs.md).

## <a name="install-hello-azure-functions-core-tools"></a>Installera verktyg för hello Azure Functions kärnor

Azure Functions grundläggande verktyg är en lokal version av hello Azure Functions-runtime som du kan köra på den lokala Windows-datorn. Det är inte en emulator eller simulatorn. Det har hello samma runtime som stänger funktioner i Azure.

Hej [Azure Functions grundläggande verktyg] tillhandahålls som ett npm. Du måste först [installera NodeJS](https://docs.npmjs.com/getting-started/installing-node), som innehåller npm.  

>[!NOTE]
>För tillfället installeras hello Azure Functions Core-verktygspaketet bara på Windows-datorer. Den här begränsningen är på grund av tooa tillfälliga begränsningen i hello funktioner värden.

[Azure Functions grundläggande verktyg] lägger du till följande kommando alias hello:
* **funktionen**
* **azfun**
* **azurefunctions**

Alla dessa alias kan användas i stället för `func` visas i hello exemplen i det här avsnittet.

## <a name="create-a-local-functions-project"></a>Skapa ett projekt med lokala funktioner

När du kör lokalt är ett funktioner projekt en katalog som har hello filer host.json och local.settings.json. Den här katalogen är hello motsvarar en funktionsapp i Azure. toolearn mer om hello Azure Functions mappstrukturen finns hello [Azure Functions utvecklarguide för](functions-reference.md#folder-structure).

Kör hello följande kommando vid en kommandotolk:

```
func init MyFunctionProj
```

hello utdata ser ut som följande exempel hello:

```
Writing .gitignore
Writing host.json
Writing local.settings.json
Created launch.json
Initialized empty Git repository in D:/Code/Playground/MyFunctionProj/.git/
```

tooopt out-of-skapa en Git-lagringsplatsen, Använd hello alternativet `--no-source-control [-n]`.

<a name="local-settings"></a>

## <a name="local-settings-file"></a>Lokala inställningsfilen

hello filen local.settings.json lagrar app-inställningar, anslutningssträngar och inställningar för Azure Functions Core verktyg. Det har hello följande struktur:

```json
{
  "IsEncrypted": false,   
  "Values": {
    "AzureWebJobsStorage": "<connection string>", 
    "AzureWebJobsDashboard": "<connection string>", 
  },
  "Host": {
    "LocalHttpPort": 7071, 
    "CORS": "*" 
  },
  "ConnectionStrings": {
    "SQLConnectionString": "Value"
  }
}
```
| Inställning      | Beskrivning                            |
| ------------ | -------------------------------------- |
| **IsEncrypted** | När värdet för**SANT**, alla värden som är krypterade med en lokal dator-nyckel. Används med `func settings` kommandon. Standardvärdet är **FALSKT**. |
| **Värden** | Samling av programinställningar som används när du kör lokalt. Lägg till toothis för programmet inställningsobjektet.  |
| **AzureWebJobsStorage** | Anger hello anslutning sträng toohello Azure Storage-konto som används internt av hello Azure Functions-runtime. hello storage-konto har stöd för din funktion utlösare. Inställningen storage-konto anslutning krävs för alla funktioner utom HTTP utlöses funktioner.  |
| **AzureWebJobsDashboard** | Anger hello anslutning sträng toohello Azure Storage-konto som används toostore hello funktionsloggar. Det här valfria värdet gör hello loggar tillgängliga i hello-portalen.|
| **Värden** | Inställningarna i det här avsnittet Anpassa hello värdprocess för funktioner när du kör lokalt. | 
| **LocalHttpPort** | Anger hello standardport som används när du kör hello lokala funktioner värden (`func host start` och `func run`). Hej `--port` kommandoradsalternativet har högre prioritet än detta värde. |
| **CORS** | Definierar hello ursprung tillåts för [resursdelning för korsande ursprung (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing). Ursprung tillhandahålls som en kommaavgränsad lista med inga blanksteg. Hej jokertecknet (**\***) stöds, vilket gör att begäranden från alla ursprung. |
| **ConnectionStrings** | Innehåller hello databasanslutningssträngar för dina funktioner. Anslutningssträngar i det här objektet läggs toohello miljö med hello providertyp av **System.Data.SqlClient**.  | 

De flesta utlösare och bindningar har en **anslutning** egenskap som mappar toohello namnet på en miljöinställning för variabeln eller app. För varje anslutning-egenskap måste det finnas appinställningen som definierats i local.settings.json-filen. 

De här inställningarna kan också läsa i koden som miljövariabler. I C#, använda [System.Environment.GetEnvironmentVariable](https://msdn.microsoft.com/library/system.environment.getenvironmentvariable(v=vs.110).aspx) eller [ConfigurationManager.AppSettings](https://msdn.microsoft.com/library/system.configuration.configurationmanager.appsettings%28v=vs.110%29.aspx). I JavaScript, använda `process.env`. Inställningarna som en systemmiljövariabler åsidosätter värden i hello local.settings.json filen. 

Inställningarna i hello local.settings.json filen används endast av funktioner verktyg när du kör lokalt. Som standard dessa inställningar migreras inte automatiskt när hello projektet är publicerade tooAzure. Använd hello `--publish-local-settings` växla [när du publicerar](#publish) toomake att dessa inställningar har lagts till toohello funktionsapp i Azure.

Om ingen giltig lagringsanslutningssträng anges för **AzureWebJobsStorage**, hello följande felmeddelande visas:  

>Värde saknas för AzureWebJobsStorage i local.settings.json. Detta krävs för alla utlösare än HTTP. Du kan köra ' func azure functionary fetch-app-inställningar <functionAppName>' eller ange en anslutningssträng i local.settings.json.
  
[!INCLUDE [Note toonot use local storage](../../includes/functions-local-settings-note.md)]

### <a name="configure-app-settings"></a>Konfigurera appinställningar

tooset ett värde för anslutningssträngar, du kan göra något av följande hello:
* Ange hello anslutningssträng från [Azure Lagringsutforskaren](http://storageexplorer.com/).
* Använd någon av följande kommandon hello:

    ```
    func azure functionapp fetch-app-settings <FunctionAppName>
    ```
    ```
    func azure functionapp storage fetch-connection-string <StorageAccountName>
    ```
    Båda kommandon måste du toofirst inloggning tooAzure.

## <a name="create-a-function"></a>Skapa en funktion

toocreate en funktion som kör hello följande kommando:

```
func new
``` 
`func new`stöder hello följande valfria argument:

| Argumentet     | Beskrivning                            |
| ------------ | -------------------------------------- |
| **`--language -l`** | hello mall programmeringsspråket, till exempel C#, F # eller JavaScript. |
| **`--template -t`** | hello mallnamn. |
| **`--name -n`** | hello funktionsnamn. |

Till exempel toocreate en JavaScript-HTTP-utlösare, kör:

```
func new --language JavaScript --template HttpTrigger --name MyHttpTrigger
```

toocreate en funktion som utlöses av kön, kör:

```
func new --language JavaScript --template QueueTrigger --name QueueTriggerJS
```

## <a name="run-functions-locally"></a>Kör funktioner lokalt

toorun köra hello funktioner värden för en funktion i projektet. hello-värden kan utlösare för alla funktioner i hello projekt:

```
func host start
```

`func host start`stöder hello följande alternativ:

| Alternativ     | Beskrivning                            |
| ------------ | -------------------------------------- |
|**`--port -p`** | hello lokal port toolisten på. Standardvärde: 7071. |
| **`--debug <type>`** | hello alternativ är `VSCode` och `VS`. |
| **`--cors`** | En kommaavgränsad lista över CORS ursprung, utan blanksteg. |
| **`--nodeDebugPort -n`** | hello-port för hello nod felsökare toouse. Standard: Ett värde från launch.json eller 5858. |
| **`--debugLevel -d`** | hello konsolen spårningsnivån (av, verbose, info, warning eller error). Standard: Info.|
| **`--timeout -t`** | hello tidsgräns för hello funktioner värden t o start, i sekunder. Standard: 20 sekunder.|
| **`--useHttps`** | Binda toohttps://localhost: {porten} i stället för toohttp://localhost: {port}. Som standard skapas ett betrott certifikat på datorn.|
| **`--pause-on-error`** | Pausa för ytterligare information innan du avslutar hello-processen. Användbar när den startas Azure Functions grundläggande verktyg från en integrerad utvecklingsmiljö (IDE).|

När hello funktioner värden startar utdata hello URL: en för HTTP-utlösta funktioner:

```
Found hello following functions:
Host.Functions.MyHttpTrigger

ob host started
Http Function MyHttpTrigger: http://localhost:7071/api/MyHttpTrigger
```

### <a name="debug-in-vs-code-or-visual-studio"></a>Felsöka i VS-kod eller Visual Studio

tooattach en felsökare skicka hello `--debug` argumentet. toodebug JavaScript-funktioner, använda Visual Studio-koden. Använda Visual Studio för C#.

toodebug C#-funktion, Använd `--debug vs`. Du kan också använda [Azure Functions Visual Studio 2017 Tools](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/). 

toolaunch hello värden och Ställ in JavaScript-felsökning, kör:

```
func host start --debug vscode
```

Klicka i Visual Studio Code hello **felsöka** väljer **bifoga tooAzure funktioner**. Du kan koppla brytpunkter inspektera variabler och stega igenom koden.

![JavaScript-felsökning med Visual Studio Code](./media/functions-run-local/vscode-javascript-debugging.png)

### <a name="passing-test-data-tooa-function"></a>Skicka test data tooa funktion

Du kan även anropa en funktion direkt med hjälp av `func run <FunctionName>` och ange indata för hello-funktionen. Det här kommandot är liknande toorunning en funktion med hjälp av hello **Test** fliken i hello Azure-portalen. Detta kommando startar hello hela funktioner värden.

`func run`stöder hello följande alternativ:

| Alternativ     | Beskrivning                            |
| ------------ | -------------------------------------- |
| **`--content -c`** | Infogade innehållet. |
| **`--debug -d`** | Koppla en felsökare toohello värdprocess innan du kör hello-funktionen.|
| **`--timeout -t`** | Tid toowait (i sekunder) tills hello lokala funktioner värden är klar.|
| **`--file -f`** | hello-filen namnet toouse som innehåll.|
| **`--no-interactive`** | Begär inte indata. Bra automation.|

Toocall en HTTP-utlösta funktion och skicka innehållsavsnitt, kör följande kommando hello:

```
func run MyHttpTrigger -c '{\"name\": \"Azure\"}'
```

## <a name="publish"></a>Publicera tooAzure

toopublish en funktion projektet tooa funktionsapp i Azure, Använd hello `publish` kommando:

```
func azure functionapp publish <FunctionAppName>
```

Du kan använda hello följande alternativ:

| Alternativ     | Beskrivning                            |
| ------------ | -------------------------------------- |
| **`--publish-local-settings -i`** |  Publiceringsinställning i local.settings.json tooAzure, fråga toooverwrite om hello inställningen redan finns.|
| **`--overwrite-settings -y`** | Måste användas med `-i`. Skriver över AppSettings i Azure med lokalt värde om olika. Standardvärdet är fråga.|

Hej `publish` kommandot överför hello innehållet i hello funktioner projektkatalogen. Om du tar bort filer lokalt hello `publish` kommandot tar inte bort dem från Azure. Du kan ta bort filer i Azure med hjälp av hello [Kudu verktyget](functions-how-to-use-azure-function-app-settings.md#kudu) i hello [Azure-portalen].

## <a name="next-steps"></a>Nästa steg

Azure Functions grundläggande verktyg är [öppna datakällan och finns på GitHub](https://github.com/azure/azure-functions-cli).  
toofile en bugg eller funktion begäran [öppna ett problem med GitHub](https://github.com/azure/azure-functions-cli/issues). 

<!-- LINKS -->

[Azure Functions grundläggande verktyg]: https://www.npmjs.com/package/azure-functions-core-tools
[Azure-portalen]: https://portal.azure.com 

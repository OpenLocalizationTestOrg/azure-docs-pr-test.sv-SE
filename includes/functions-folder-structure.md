
hello-koden för alla hello funktioner i en viss funktionsapp finns i rotmappen som innehåller en konfigurationsfil för värden och en eller flera undermappar som innehåller hello kod för en separat funktion, som i följande exempel hello:

```
wwwroot
 | - host.json
 | - mynodefunction
 | | - function.json
 | | - index.js
 | | - node_modules
 | | | - ... packages ...
 | | - package.json
 | - mycsharpfunction
 | | - function.json
 | | - run.csx
```

Hej *host.json* filen innehåller vissa runtime konfiguration och placeras i hello funktionsapp hello rotmapp. Mer information om inställningar som är tillgängliga finns [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) i hello WebJobs.Script databasen wiki.

Varje funktion har en mapp som innehåller en eller flera kodfiler, hello function.json konfiguration och andra beroenden.


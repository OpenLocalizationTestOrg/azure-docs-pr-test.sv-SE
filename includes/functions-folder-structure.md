
<span data-ttu-id="8a355-101">hello-koden för alla hello funktioner i en viss funktionsapp finns i rotmappen som innehåller en konfigurationsfil för värden och en eller flera undermappar som innehåller hello kod för en separat funktion, som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="8a355-101">hello code for all of hello functions in a given function app lives in a root folder that contains a host configuration file and one or more subfolders, each of which contain hello code for a separate function, as in hello following example:</span></span>

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

<span data-ttu-id="8a355-102">Hej *host.json* filen innehåller vissa runtime konfiguration och placeras i hello funktionsapp hello rotmapp.</span><span class="sxs-lookup"><span data-stu-id="8a355-102">hello *host.json* file contains some runtime-specific configuration and sits in hello root folder of hello function app.</span></span> <span data-ttu-id="8a355-103">Mer information om inställningar som är tillgängliga finns [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) i hello WebJobs.Script databasen wiki.</span><span class="sxs-lookup"><span data-stu-id="8a355-103">For information on settings that are available, see [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) in hello WebJobs.Script repository wiki.</span></span>

<span data-ttu-id="8a355-104">Varje funktion har en mapp som innehåller en eller flera kodfiler, hello function.json konfiguration och andra beroenden.</span><span class="sxs-lookup"><span data-stu-id="8a355-104">Each function has a folder that contains one or more code files, hello function.json configuration and other dependencies.</span></span>


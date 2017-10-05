---
title: "Felsöka Azure mikrotjänster i Linux | Microsoft Docs"
description: "Lär dig mer om att övervaka och diagnostisera dina tjänster som skrivits med Microsoft Azure Service Fabric på en dator för lokal utveckling."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 4eebe937-ab42-4429-93db-f35c26424321
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 4bc73f581f4855ebc724df19dd56fab8bf103854
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-and-diagnose-services-in-a-local-machine-development-setup"></a><span data-ttu-id="7de75-103">Övervaka och diagnostisera tjänster i en inställning för utveckling av lokal dator</span><span class="sxs-lookup"><span data-stu-id="7de75-103">Monitor and diagnose services in a local machine development setup</span></span>


> [!div class="op_single_selector"]
> * [<span data-ttu-id="7de75-104">Windows</span><span class="sxs-lookup"><span data-stu-id="7de75-104">Windows</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
> * [<span data-ttu-id="7de75-105">Linux</span><span class="sxs-lookup"><span data-stu-id="7de75-105">Linux</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
>
>

<span data-ttu-id="7de75-106">Övervaka, identifiera, diagnostisera och felsöka Tillåt för tjänster att fortsätta med minimala störningar för användarupplevelsen.</span><span class="sxs-lookup"><span data-stu-id="7de75-106">Monitoring, detecting, diagnosing, and troubleshooting allow for services to continue with minimal disruption to the user experience.</span></span> <span data-ttu-id="7de75-107">Övervaknings- och diagnostikfunktionerna är viktiga i en verklig distribuerade produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="7de75-107">Monitoring and diagnostics are critical in an actual deployed production environment.</span></span> <span data-ttu-id="7de75-108">Införandet av en liknande modell under utvecklingen av tjänster garanterar att diagnostiska pipeline fungerar när du flyttar till en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="7de75-108">Adopting a similar model during development of services ensures that the diagnostic pipeline works when you move to a production environment.</span></span> <span data-ttu-id="7de75-109">Service Fabric gör det enkelt för tjänstutvecklare att implementera diagnostik fungerar sömlöst över både inställningar för enskild dator lokal utveckling och produktion verkliga klustret installationsprogram.</span><span class="sxs-lookup"><span data-stu-id="7de75-109">Service Fabric makes it easy for service developers to implement diagnostics that can seamlessly work across both single-machine local development setups and real-world production cluster setups.</span></span>


## <a name="debugging-service-fabric-java-applications"></a><span data-ttu-id="7de75-110">Felsökning av Service Fabric Java-program</span><span class="sxs-lookup"><span data-stu-id="7de75-110">Debugging Service Fabric Java applications</span></span>

<span data-ttu-id="7de75-111">För Java-program, [flera loggning ramverk](http://en.wikipedia.org/wiki/Java_logging_framework) är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="7de75-111">For Java applications, [multiple logging frameworks](http://en.wikipedia.org/wiki/Java_logging_framework) are available.</span></span> <span data-ttu-id="7de75-112">Eftersom `java.util.logging` är standardalternativet med JRE, det används också för de [kodexempel i github](http://github.com/Azure-Samples/service-fabric-java-getting-started).</span><span class="sxs-lookup"><span data-stu-id="7de75-112">Since `java.util.logging` is the default option with the JRE, it is also used for the [code examples in github](http://github.com/Azure-Samples/service-fabric-java-getting-started).</span></span>  <span data-ttu-id="7de75-113">I följande avsnittet beskriver hur du konfigurerar den `java.util.logging` framework.</span><span class="sxs-lookup"><span data-stu-id="7de75-113">The following discussion explains how to configure the `java.util.logging` framework.</span></span>

<span data-ttu-id="7de75-114">Med hjälp av java.util.logging dirigera du dina programloggar till minne utdataströmmar, console-filer eller sockets.</span><span class="sxs-lookup"><span data-stu-id="7de75-114">Using java.util.logging you can redirect your application logs to memory, output streams, console files, or sockets.</span></span> <span data-ttu-id="7de75-115">Det finns standard hanterare som redan tillhandahålls inom ramen för var och en av dessa alternativ.</span><span class="sxs-lookup"><span data-stu-id="7de75-115">For each of these options, there are default handlers already provided in the framework.</span></span> <span data-ttu-id="7de75-116">Du kan skapa en `app.properties` fil att konfigurera Filhanteraren för programmet att omdirigera alla loggar till en lokal fil.</span><span class="sxs-lookup"><span data-stu-id="7de75-116">You can create a `app.properties` file to configure the file handler for your application to redirect all logs to a local file.</span></span>

<span data-ttu-id="7de75-117">Följande kodavsnitt innehåller en exempelkonfiguration:</span><span class="sxs-lookup"><span data-stu-id="7de75-117">The following code snippet contains an example configuration:</span></span>

```java
handlers = java.util.logging.FileHandler

java.util.logging.FileHandler.level = ALL
java.util.logging.FileHandler.formatter = java.util.logging.SimpleFormatter
java.util.logging.FileHandler.limit = 1024000
java.util.logging.FileHandler.count = 10
java.util.logging.FileHandler.pattern = /tmp/servicefabric/logs/mysfapp%u.%g.log             
```

<span data-ttu-id="7de75-118">Mappen som pekar på den `app.properties` filen måste finnas.</span><span class="sxs-lookup"><span data-stu-id="7de75-118">The folder pointed to by the `app.properties` file must exist.</span></span> <span data-ttu-id="7de75-119">Efter den `app.properties` filen har skapats, måste du också ändra skriptet post punkt `entrypoint.sh` i den `<applicationfolder>/<servicePkg>/Code/` mapp för att ange egenskapen `java.util.logging.config.file` till `app.propertes` filen.</span><span class="sxs-lookup"><span data-stu-id="7de75-119">After the `app.properties` file is created, you need to also modify your entry point script, `entrypoint.sh` in the `<applicationfolder>/<servicePkg>/Code/` folder to set the property `java.util.logging.config.file` to `app.propertes` file.</span></span> <span data-ttu-id="7de75-120">Posten bör se ut som följande utdrag:</span><span class="sxs-lookup"><span data-stu-id="7de75-120">The entry should look like the following snippet:</span></span>

```sh
java -Djava.library.path=$LD_LIBRARY_PATH -Djava.util.logging.config.file=<path to app.properties> -jar <service name>.jar
```


<span data-ttu-id="7de75-121">Den här konfigurationen leder loggar som samlas in i ett roterar sätt på `/tmp/servicefabric/logs/`.</span><span class="sxs-lookup"><span data-stu-id="7de75-121">This configuration results in logs being collected in a rotating fashion at `/tmp/servicefabric/logs/`.</span></span> <span data-ttu-id="7de75-122">Loggfilen i det här fallet heter mysfapp%u.%g.log där:</span><span class="sxs-lookup"><span data-stu-id="7de75-122">The log file in this case is named mysfapp%u.%g.log where:</span></span>
* <span data-ttu-id="7de75-123">**%u** är ett unikt nummer för att lösa konflikter mellan samtidiga Java processer.</span><span class="sxs-lookup"><span data-stu-id="7de75-123">**%u** is a unique number to resolve conflicts between simultaneous Java processes.</span></span>
* <span data-ttu-id="7de75-124">**%g** är antalet generation att skilja mellan rotera loggar.</span><span class="sxs-lookup"><span data-stu-id="7de75-124">**%g** is the generation number to distinguish between rotating logs.</span></span>

<span data-ttu-id="7de75-125">Som standard om ingen hanterare uttryckligen har konfigurerats i konsolen hanteraren är registrerad.</span><span class="sxs-lookup"><span data-stu-id="7de75-125">By default if no handler is explicitly configured, the console handler is registered.</span></span> <span data-ttu-id="7de75-126">En kan visa loggarna i syslog under /var/log/syslog.</span><span class="sxs-lookup"><span data-stu-id="7de75-126">One can view the logs in syslog under /var/log/syslog.</span></span>

<span data-ttu-id="7de75-127">Mer information finns i [kodexempel i github](http://github.com/Azure-Samples/service-fabric-java-getting-started).</span><span class="sxs-lookup"><span data-stu-id="7de75-127">For more information, see the [code examples in github](http://github.com/Azure-Samples/service-fabric-java-getting-started).</span></span>  


## <a name="debugging-service-fabric-c-applications"></a><span data-ttu-id="7de75-128">Felsöka Service Fabric C#-program</span><span class="sxs-lookup"><span data-stu-id="7de75-128">Debugging Service Fabric C# applications</span></span>


<span data-ttu-id="7de75-129">Flera ramverk är tillgängliga för att spåra CoreCLR program på Linux.</span><span class="sxs-lookup"><span data-stu-id="7de75-129">Multiple frameworks are available for tracing CoreCLR applications on Linux.</span></span> <span data-ttu-id="7de75-130">Mer information finns i [GitHub: loggning](http:/github.com/aspnet/logging).</span><span class="sxs-lookup"><span data-stu-id="7de75-130">For more information, see [GitHub: logging](http:/github.com/aspnet/logging).</span></span>  <span data-ttu-id="7de75-131">Eftersom EventSource är bekant för C# utvecklare, som den här artikeln använder EventSource för spårning i CoreCLR prov på Linux.</span><span class="sxs-lookup"><span data-stu-id="7de75-131">Since EventSource is familiar to C# developers,\`this article uses EventSource for tracing in CoreCLR samples on Linux.</span></span>

<span data-ttu-id="7de75-132">Det första steget är att inkludera System.Diagnostics.Tracing så att du kan skriva dina loggar till minne, utdataströmmar eller console-filer.</span><span class="sxs-lookup"><span data-stu-id="7de75-132">The first step is to include System.Diagnostics.Tracing so that you can write your logs to memory, output streams, or console files.</span></span>  <span data-ttu-id="7de75-133">Lägg till följande projektet din project.json för loggning med hjälp av EventSource är:</span><span class="sxs-lookup"><span data-stu-id="7de75-133">For logging using EventSource, add the following project to your project.json:</span></span>

```
    "System.Diagnostics.StackTrace": "4.0.1"
```

<span data-ttu-id="7de75-134">Du kan använda en anpassad EventListener att lyssna efter händelsen tjänsten och sedan på lämpligt sätt dirigera dem till spårningsfiler.</span><span class="sxs-lookup"><span data-stu-id="7de75-134">You can use a custom EventListener to listen for the service event and then appropriately redirect them to trace files.</span></span> <span data-ttu-id="7de75-135">Följande kodavsnitt visar ett exempel på implementering av loggning med hjälp av EventSource och anpassade EventListener:</span><span class="sxs-lookup"><span data-stu-id="7de75-135">The following code snippet shows a sample implementation of logging using EventSource and a custom EventListener:</span></span>


```csharp

 public class ServiceEventSource : EventSource
 {
        public static ServiceEventSource Current = new ServiceEventSource();

        [NonEvent]
        public void Message(string message, params object[] args)
        {
            if (this.IsEnabled())
            {
                var finalMessage = string.Format(message, args);
                this.Message(finalMessage);
            }
        }

        // TBD: Need to add method for sample event.

}

```


```csharp
   internal class ServiceEventListener : EventListener
   {

        protected override void OnEventSourceCreated(EventSource eventSource)
        {
            EnableEvents(eventSource, EventLevel.LogAlways, EventKeywords.All);
        }
        protected override void OnEventWritten(EventWrittenEventArgs eventData)
        {
            using (StreamWriter Out = new StreamWriter( new FileStream("/tmp/MyServiceLog.txt", FileMode.Append)))           
        { 
                 // report all event information               
         Out.Write(" {0} ",  Write(eventData.Task.ToString(), eventData.EventName, eventData.EventId.ToString(), eventData.Level,""));
                if (eventData.Message != null)              
            Out.WriteLine(eventData.Message, eventData.Payload.ToArray());              
            else             
        { 
                    string[] sargs = eventData.Payload != null ? eventData.Payload.Select(o => o.ToString()).ToArray() : null; 
                    Out.WriteLine("({0}).", sargs != null ? string.Join(", ", sargs) : "");             
        }
           }
        }
    }
```


<span data-ttu-id="7de75-136">Föregående kodfragment matar ut loggar till en fil i `/tmp/MyServiceLog.txt`.</span><span class="sxs-lookup"><span data-stu-id="7de75-136">The preceding snippet outputs the logs to a file in `/tmp/MyServiceLog.txt`.</span></span> <span data-ttu-id="7de75-137">Det här namnet måste uppdateras korrekt.</span><span class="sxs-lookup"><span data-stu-id="7de75-137">This file name needs to be appropriately updated.</span></span> <span data-ttu-id="7de75-138">Om du vill omdirigera loggar till konsolen använder du följande kodavsnitt i din anpassade EventListener-klass:</span><span class="sxs-lookup"><span data-stu-id="7de75-138">In case you want to redirect the logs to console, use the following snippet in your customized EventListener class:</span></span>

```csharp
public static TextWriter Out = Console.Out;
```

<span data-ttu-id="7de75-139">Exempel på [C#-exempel](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started) använda EventSource och anpassade EventListener för att logga händelser till en fil.</span><span class="sxs-lookup"><span data-stu-id="7de75-139">The samples at [C# Samples](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started) use EventSource and a custom EventListener to log events to a file.</span></span>



## <a name="next-steps"></a><span data-ttu-id="7de75-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7de75-140">Next steps</span></span>
<span data-ttu-id="7de75-141">Samma spårning koden som lagts till i ditt program fungerar även med diagnostik för programmet på ett Azure-kluster.</span><span class="sxs-lookup"><span data-stu-id="7de75-141">The same tracing code added to your application also works with the diagnostics of your application on an Azure cluster.</span></span> <span data-ttu-id="7de75-142">Gå igenom dessa artiklar som beskrivs de olika alternativen för verktyg och beskriver hur du ställer in.</span><span class="sxs-lookup"><span data-stu-id="7de75-142">Check out these articles that discuss the different options for the tools and describe how to set them up.</span></span>
* [<span data-ttu-id="7de75-143">Hur du samlar in loggar med Azure-diagnostik</span><span class="sxs-lookup"><span data-stu-id="7de75-143">How to collect logs with Azure Diagnostics</span></span>](service-fabric-diagnostics-how-to-setup-lad.md)

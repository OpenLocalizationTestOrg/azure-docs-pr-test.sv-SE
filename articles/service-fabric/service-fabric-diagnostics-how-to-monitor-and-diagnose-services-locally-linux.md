---
title: "aaaDebug Azure mikrotjänster i Linux | Microsoft Docs"
description: "Lär dig hur toomonitor och diagnostisera dina tjänster som skrivits med Microsoft Azure Service Fabric på en dator för lokal utveckling."
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
ms.openlocfilehash: bee47bbabcf6b84ff2da14079e026529e36a198b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-diagnose-services-in-a-local-machine-development-setup"></a><span data-ttu-id="1dae4-103">Övervaka och diagnostisera tjänster i en inställning för utveckling av lokal dator</span><span class="sxs-lookup"><span data-stu-id="1dae4-103">Monitor and diagnose services in a local machine development setup</span></span>


> [!div class="op_single_selector"]
> * [<span data-ttu-id="1dae4-104">Windows</span><span class="sxs-lookup"><span data-stu-id="1dae4-104">Windows</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
> * [<span data-ttu-id="1dae4-105">Linux</span><span class="sxs-lookup"><span data-stu-id="1dae4-105">Linux</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
>
>

<span data-ttu-id="1dae4-106">Övervaka, identifiera, diagnostisera och felsöka tillåta tjänster toocontinue med minimala störningar toohello användarupplevelsen.</span><span class="sxs-lookup"><span data-stu-id="1dae4-106">Monitoring, detecting, diagnosing, and troubleshooting allow for services toocontinue with minimal disruption toohello user experience.</span></span> <span data-ttu-id="1dae4-107">Övervaknings- och diagnostikfunktionerna är viktiga i en verklig distribuerade produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="1dae4-107">Monitoring and diagnostics are critical in an actual deployed production environment.</span></span> <span data-ttu-id="1dae4-108">Införandet av en liknande modell under utvecklingen av tjänster garanterar att hello diagnostiska pipeline fungerar när du flyttar tooa produktionsmiljön.</span><span class="sxs-lookup"><span data-stu-id="1dae4-108">Adopting a similar model during development of services ensures that hello diagnostic pipeline works when you move tooa production environment.</span></span> <span data-ttu-id="1dae4-109">Service Fabric gör det enkelt för tjänsten utvecklare tooimplement diagnostik fungerar sömlöst över både inställningar för enskild dator lokal utveckling och produktion verkliga klustret installationsprogram.</span><span class="sxs-lookup"><span data-stu-id="1dae4-109">Service Fabric makes it easy for service developers tooimplement diagnostics that can seamlessly work across both single-machine local development setups and real-world production cluster setups.</span></span>


## <a name="debugging-service-fabric-java-applications"></a><span data-ttu-id="1dae4-110">Felsökning av Service Fabric Java-program</span><span class="sxs-lookup"><span data-stu-id="1dae4-110">Debugging Service Fabric Java applications</span></span>

<span data-ttu-id="1dae4-111">För Java-program, [flera loggning ramverk](http://en.wikipedia.org/wiki/Java_logging_framework) är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="1dae4-111">For Java applications, [multiple logging frameworks](http://en.wikipedia.org/wiki/Java_logging_framework) are available.</span></span> <span data-ttu-id="1dae4-112">Eftersom `java.util.logging` är standardalternativet hello med hello JRE, det används också för hello [kodexempel i github](http://github.com/Azure-Samples/service-fabric-java-getting-started).</span><span class="sxs-lookup"><span data-stu-id="1dae4-112">Since `java.util.logging` is hello default option with hello JRE, it is also used for hello [code examples in github](http://github.com/Azure-Samples/service-fabric-java-getting-started).</span></span>  <span data-ttu-id="1dae4-113">hello följande diskussion förklarar hur tooconfigure hello `java.util.logging` framework.</span><span class="sxs-lookup"><span data-stu-id="1dae4-113">hello following discussion explains how tooconfigure hello `java.util.logging` framework.</span></span>

<span data-ttu-id="1dae4-114">Med hjälp av java.util.logging som du kan dirigera om ditt program loggar toomemory utdataströmmar, console-filer eller sockets.</span><span class="sxs-lookup"><span data-stu-id="1dae4-114">Using java.util.logging you can redirect your application logs toomemory, output streams, console files, or sockets.</span></span> <span data-ttu-id="1dae4-115">För var och en av dessa alternativ finns standard hanterare som redan tillhandahålls i hello framework.</span><span class="sxs-lookup"><span data-stu-id="1dae4-115">For each of these options, there are default handlers already provided in hello framework.</span></span> <span data-ttu-id="1dae4-116">Du kan skapa en `app.properties` tooconfigure hello filen Filhanteraren för ditt program tooredirect alla loggar tooa lokal fil.</span><span class="sxs-lookup"><span data-stu-id="1dae4-116">You can create a `app.properties` file tooconfigure hello file handler for your application tooredirect all logs tooa local file.</span></span>

<span data-ttu-id="1dae4-117">följande kodstycke hello innehåller en exempelkonfiguration:</span><span class="sxs-lookup"><span data-stu-id="1dae4-117">hello following code snippet contains an example configuration:</span></span>

```java
handlers = java.util.logging.FileHandler

java.util.logging.FileHandler.level = ALL
java.util.logging.FileHandler.formatter = java.util.logging.SimpleFormatter
java.util.logging.FileHandler.limit = 1024000
java.util.logging.FileHandler.count = 10
java.util.logging.FileHandler.pattern = /tmp/servicefabric/logs/mysfapp%u.%g.log             
```

<span data-ttu-id="1dae4-118">hello mappen pekar tooby hello `app.properties` filen måste finnas.</span><span class="sxs-lookup"><span data-stu-id="1dae4-118">hello folder pointed tooby hello `app.properties` file must exist.</span></span> <span data-ttu-id="1dae4-119">Efter hello `app.properties` filen har skapats måste du tooalso ändra skriptet post punkt `entrypoint.sh` i hello `<applicationfolder>/<servicePkg>/Code/` mappen tooset hello egenskapen `java.util.logging.config.file` för`app.propertes` fil.</span><span class="sxs-lookup"><span data-stu-id="1dae4-119">After hello `app.properties` file is created, you need tooalso modify your entry point script, `entrypoint.sh` in hello `<applicationfolder>/<servicePkg>/Code/` folder tooset hello property `java.util.logging.config.file` too`app.propertes` file.</span></span> <span data-ttu-id="1dae4-120">hello post bör se ut som följande fragment hello:</span><span class="sxs-lookup"><span data-stu-id="1dae4-120">hello entry should look like hello following snippet:</span></span>

```sh
java -Djava.library.path=$LD_LIBRARY_PATH -Djava.util.logging.config.file=<path tooapp.properties> -jar <service name>.jar
```


<span data-ttu-id="1dae4-121">Den här konfigurationen leder loggar som samlas in i ett roterar sätt på `/tmp/servicefabric/logs/`.</span><span class="sxs-lookup"><span data-stu-id="1dae4-121">This configuration results in logs being collected in a rotating fashion at `/tmp/servicefabric/logs/`.</span></span> <span data-ttu-id="1dae4-122">hello loggfilen i det här fallet heter mysfapp%u.%g.log där:</span><span class="sxs-lookup"><span data-stu-id="1dae4-122">hello log file in this case is named mysfapp%u.%g.log where:</span></span>
* <span data-ttu-id="1dae4-123">**%u** är ett unikt nummer tooresolve konflikter mellan samtidiga Java processer.</span><span class="sxs-lookup"><span data-stu-id="1dae4-123">**%u** is a unique number tooresolve conflicts between simultaneous Java processes.</span></span>
* <span data-ttu-id="1dae4-124">**%g** är hello generation nummer toodistinguish mellan rotera loggar.</span><span class="sxs-lookup"><span data-stu-id="1dae4-124">**%g** is hello generation number toodistinguish between rotating logs.</span></span>

<span data-ttu-id="1dae4-125">Som standard om ingen hanterare uttryckligen har konfigurerats hello konsolen hanteraren är registrerad.</span><span class="sxs-lookup"><span data-stu-id="1dae4-125">By default if no handler is explicitly configured, hello console handler is registered.</span></span> <span data-ttu-id="1dae4-126">En kan visa hello loggar i syslog under /var/log/syslog.</span><span class="sxs-lookup"><span data-stu-id="1dae4-126">One can view hello logs in syslog under /var/log/syslog.</span></span>

<span data-ttu-id="1dae4-127">Mer information finns i hello [kodexempel i github](http://github.com/Azure-Samples/service-fabric-java-getting-started).</span><span class="sxs-lookup"><span data-stu-id="1dae4-127">For more information, see hello [code examples in github](http://github.com/Azure-Samples/service-fabric-java-getting-started).</span></span>  


## <a name="debugging-service-fabric-c-applications"></a><span data-ttu-id="1dae4-128">Felsöka Service Fabric C#-program</span><span class="sxs-lookup"><span data-stu-id="1dae4-128">Debugging Service Fabric C# applications</span></span>


<span data-ttu-id="1dae4-129">Flera ramverk är tillgängliga för att spåra CoreCLR program på Linux.</span><span class="sxs-lookup"><span data-stu-id="1dae4-129">Multiple frameworks are available for tracing CoreCLR applications on Linux.</span></span> <span data-ttu-id="1dae4-130">Mer information finns i [GitHub: loggning](http:/github.com/aspnet/logging).</span><span class="sxs-lookup"><span data-stu-id="1dae4-130">For more information, see [GitHub: logging](http:/github.com/aspnet/logging).</span></span>  <span data-ttu-id="1dae4-131">Eftersom EventSource är bekant tooC # utvecklare, som den här artikeln använder EventSource för spårning i CoreCLR prov på Linux.</span><span class="sxs-lookup"><span data-stu-id="1dae4-131">Since EventSource is familiar tooC# developers,\`this article uses EventSource for tracing in CoreCLR samples on Linux.</span></span>

<span data-ttu-id="1dae4-132">hello första steget är tooinclude System.Diagnostics.Tracing så att du kan skriva ditt loggar toomemory, utdataströmmar eller konsolfiler.</span><span class="sxs-lookup"><span data-stu-id="1dae4-132">hello first step is tooinclude System.Diagnostics.Tracing so that you can write your logs toomemory, output streams, or console files.</span></span>  <span data-ttu-id="1dae4-133">Lägg till hello följande projekt tooyour project.json för loggning med hjälp av EventSource är:</span><span class="sxs-lookup"><span data-stu-id="1dae4-133">For logging using EventSource, add hello following project tooyour project.json:</span></span>

```
    "System.Diagnostics.StackTrace": "4.0.1"
```

<span data-ttu-id="1dae4-134">Du kan använda en anpassad EventListener toolisten för hello service händelsen och sedan korrekt dirigera dem tootrace filer.</span><span class="sxs-lookup"><span data-stu-id="1dae4-134">You can use a custom EventListener toolisten for hello service event and then appropriately redirect them tootrace files.</span></span> <span data-ttu-id="1dae4-135">hello visar följande kodavsnitt ett exempel på implementering av loggning med hjälp av EventSource och anpassade EventListener:</span><span class="sxs-lookup"><span data-stu-id="1dae4-135">hello following code snippet shows a sample implementation of logging using EventSource and a custom EventListener:</span></span>


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

        // TBD: Need tooadd method for sample event.

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


<span data-ttu-id="1dae4-136">hello föregående kodfragment matar ut hello loggar tooa filen i `/tmp/MyServiceLog.txt`.</span><span class="sxs-lookup"><span data-stu-id="1dae4-136">hello preceding snippet outputs hello logs tooa file in `/tmp/MyServiceLog.txt`.</span></span> <span data-ttu-id="1dae4-137">Det här namnet måste toobe uppdateras korrekt.</span><span class="sxs-lookup"><span data-stu-id="1dae4-137">This file name needs toobe appropriately updated.</span></span> <span data-ttu-id="1dae4-138">Om du vill tooredirect hello loggar tooconsole Använd hello följande kodavsnitt i din anpassade EventListener-klass:</span><span class="sxs-lookup"><span data-stu-id="1dae4-138">In case you want tooredirect hello logs tooconsole, use hello following snippet in your customized EventListener class:</span></span>

```csharp
public static TextWriter Out = Console.Out;
```

<span data-ttu-id="1dae4-139">Hej prov på [C#-exempel](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started) använder EventSource och en anpassad EventListener toolog händelser tooa-fil.</span><span class="sxs-lookup"><span data-stu-id="1dae4-139">hello samples at [C# Samples](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started) use EventSource and a custom EventListener toolog events tooa file.</span></span>



## <a name="next-steps"></a><span data-ttu-id="1dae4-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1dae4-140">Next steps</span></span>
<span data-ttu-id="1dae4-141">Hej samma spårning kod läggs tooyour program fungerar även med hello diagnostik för programmet på ett Azure-kluster.</span><span class="sxs-lookup"><span data-stu-id="1dae4-141">hello same tracing code added tooyour application also works with hello diagnostics of your application on an Azure cluster.</span></span> <span data-ttu-id="1dae4-142">Checka ut dessa artiklar som hello olika användningsalternativ för hello verktyg och beskriver hur tooset dem upp.</span><span class="sxs-lookup"><span data-stu-id="1dae4-142">Check out these articles that discuss hello different options for hello tools and describe how tooset them up.</span></span>
* [<span data-ttu-id="1dae4-143">Hur toocollect loggar med Azure-diagnostik</span><span class="sxs-lookup"><span data-stu-id="1dae4-143">How toocollect logs with Azure Diagnostics</span></span>](service-fabric-diagnostics-how-to-setup-lad.md)

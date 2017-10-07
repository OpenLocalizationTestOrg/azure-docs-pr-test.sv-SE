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
# <a name="monitor-and-diagnose-services-in-a-local-machine-development-setup"></a>Övervaka och diagnostisera tjänster i en inställning för utveckling av lokal dator


> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
> * [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
>
>

Övervaka, identifiera, diagnostisera och felsöka tillåta tjänster toocontinue med minimala störningar toohello användarupplevelsen. Övervaknings- och diagnostikfunktionerna är viktiga i en verklig distribuerade produktionsmiljö. Införandet av en liknande modell under utvecklingen av tjänster garanterar att hello diagnostiska pipeline fungerar när du flyttar tooa produktionsmiljön. Service Fabric gör det enkelt för tjänsten utvecklare tooimplement diagnostik fungerar sömlöst över både inställningar för enskild dator lokal utveckling och produktion verkliga klustret installationsprogram.


## <a name="debugging-service-fabric-java-applications"></a>Felsökning av Service Fabric Java-program

För Java-program, [flera loggning ramverk](http://en.wikipedia.org/wiki/Java_logging_framework) är tillgängliga. Eftersom `java.util.logging` är standardalternativet hello med hello JRE, det används också för hello [kodexempel i github](http://github.com/Azure-Samples/service-fabric-java-getting-started).  hello följande diskussion förklarar hur tooconfigure hello `java.util.logging` framework.

Med hjälp av java.util.logging som du kan dirigera om ditt program loggar toomemory utdataströmmar, console-filer eller sockets. För var och en av dessa alternativ finns standard hanterare som redan tillhandahålls i hello framework. Du kan skapa en `app.properties` tooconfigure hello filen Filhanteraren för ditt program tooredirect alla loggar tooa lokal fil.

följande kodstycke hello innehåller en exempelkonfiguration:

```java
handlers = java.util.logging.FileHandler

java.util.logging.FileHandler.level = ALL
java.util.logging.FileHandler.formatter = java.util.logging.SimpleFormatter
java.util.logging.FileHandler.limit = 1024000
java.util.logging.FileHandler.count = 10
java.util.logging.FileHandler.pattern = /tmp/servicefabric/logs/mysfapp%u.%g.log             
```

hello mappen pekar tooby hello `app.properties` filen måste finnas. Efter hello `app.properties` filen har skapats måste du tooalso ändra skriptet post punkt `entrypoint.sh` i hello `<applicationfolder>/<servicePkg>/Code/` mappen tooset hello egenskapen `java.util.logging.config.file` för`app.propertes` fil. hello post bör se ut som följande fragment hello:

```sh
java -Djava.library.path=$LD_LIBRARY_PATH -Djava.util.logging.config.file=<path tooapp.properties> -jar <service name>.jar
```


Den här konfigurationen leder loggar som samlas in i ett roterar sätt på `/tmp/servicefabric/logs/`. hello loggfilen i det här fallet heter mysfapp%u.%g.log där:
* **%u** är ett unikt nummer tooresolve konflikter mellan samtidiga Java processer.
* **%g** är hello generation nummer toodistinguish mellan rotera loggar.

Som standard om ingen hanterare uttryckligen har konfigurerats hello konsolen hanteraren är registrerad. En kan visa hello loggar i syslog under /var/log/syslog.

Mer information finns i hello [kodexempel i github](http://github.com/Azure-Samples/service-fabric-java-getting-started).  


## <a name="debugging-service-fabric-c-applications"></a>Felsöka Service Fabric C#-program


Flera ramverk är tillgängliga för att spåra CoreCLR program på Linux. Mer information finns i [GitHub: loggning](http:/github.com/aspnet/logging).  Eftersom EventSource är bekant tooC # utvecklare, som den här artikeln använder EventSource för spårning i CoreCLR prov på Linux.

hello första steget är tooinclude System.Diagnostics.Tracing så att du kan skriva ditt loggar toomemory, utdataströmmar eller konsolfiler.  Lägg till hello följande projekt tooyour project.json för loggning med hjälp av EventSource är:

```
    "System.Diagnostics.StackTrace": "4.0.1"
```

Du kan använda en anpassad EventListener toolisten för hello service händelsen och sedan korrekt dirigera dem tootrace filer. hello visar följande kodavsnitt ett exempel på implementering av loggning med hjälp av EventSource och anpassade EventListener:


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


hello föregående kodfragment matar ut hello loggar tooa filen i `/tmp/MyServiceLog.txt`. Det här namnet måste toobe uppdateras korrekt. Om du vill tooredirect hello loggar tooconsole Använd hello följande kodavsnitt i din anpassade EventListener-klass:

```csharp
public static TextWriter Out = Console.Out;
```

Hej prov på [C#-exempel](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started) använder EventSource och en anpassad EventListener toolog händelser tooa-fil.



## <a name="next-steps"></a>Nästa steg
Hej samma spårning kod läggs tooyour program fungerar även med hello diagnostik för programmet på ett Azure-kluster. Checka ut dessa artiklar som hello olika användningsalternativ för hello verktyg och beskriver hur tooset dem upp.
* [Hur toocollect loggar med Azure-diagnostik](service-fabric-diagnostics-how-to-setup-lad.md)

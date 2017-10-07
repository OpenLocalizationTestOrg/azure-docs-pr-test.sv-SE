---
title: "aaaHandle Molntjänsten Livscykelhändelser | Microsoft Docs"
description: "Lär dig hur hello livscykel metoder för en tjänst i molnet roll kan användas i .NET"
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 39b30acd-57b9-48b7-a7c4-40ea3430e451
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: cc0ccc5f055b965202b6e081a6ab72ad5d39b034
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="customize-hello-lifecycle-of-a-web-or-worker-role-in-net"></a>Anpassa hello livscykeln för en webb- eller Arbetarroll roll i .NET
När du skapar en arbetsroll kan du utöka hello [RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx) klassen som innehåller metoder du toooverride som du kan svara toolifecycle händelser. Den här klassen är valfritt, för webbroller så måste du använda den toorespond toolifecycle händelser.

## <a name="extend-hello-roleentrypoint-class"></a>Utöka hello RoleEntryPoint klass
Hej [RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx) klassen innehåller metoder som anropas av Azure när den **startar**, **kör**, eller **stoppar** rollen webb eller arbetare. Alternativt kan du åsidosätta dessa metoder toomanage roll initiering, roll avstängning sekvenser eller hello körning tråd hello roll. 

När du utökar **RoleEntryPoint**, bör du vara medveten om hello följande beteendena hos hello metoder:

* Hej [OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) och [OnStop](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) metoder returnerar ett booleskt värde så att det är möjligt tooreturn **FALSKT** från dessa metoder.
  
   Om din kod returnerar **FALSKT**hello rollen processen avslutas plötsligt, utan att köra alla stängas ned har på plats. I allmänhet bör du undvika returnerar **FALSKT** från hello **OnStart** metod.
* Någon utan felhantering undantag inom en överlagring av en **RoleEntryPoint** metoden behandlas som ett ohanterat undantag.
  
   Om ett undantag uppstår inom någon av hello livscykel metoder, Azure höjer hello [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) händelse, och sedan hello processen avslutas. När din roll har varit offline, startas av Azure. När ett ohanterat undantag inträffar, hello [stoppar](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.stopping.aspx) händelsen utlöses inte och hello **OnStop** -metoden inte anropas.

Om din roll inte startar eller återvinning mellan hello initierar, upptagen och stoppa tillstånd kan koden utlöser ett ohanterat undantag i en av hello Livscykelhändelser varje gång hello roll startar om. I det här fallet använder hello [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) händelse toodetermine hello orsaken till hello undantag och hantera korrekt. Din roll kan också returnera från hello [kör](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metod, vilket gör hello rollen toorestart. Mer information om distributionstillstånd finns [vanliga problem som orsakar roller tooRecycle](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md).

> [!NOTE]
> Om du använder hello **Azure Tools för Microsoft Visual Studio** toodevelop programmet hello rollen projektmallar Utöka automatiskt hello **RoleEntryPoint** klass, i hello *WebRole.cs* och *WorkerRole.cs* filer.
> 
> 

## <a name="onstart-method"></a>OnStart-metoden
Hej **OnStart** metoden anropas när din rollinstans är online med Azure. Medan hello OnStart koden körs hello-rollinstans som har markerats som **upptagen** och inga externa trafiken kommer att dirigerad tooit som hello belastningsutjämnaren. Du kan åsidosätta den här metoden tooperform initieringsarbete, till exempel implementera händelsehanterare och starta [Azure Diagnostics](cloud-services-how-to-monitor.md).

Om **OnStart** returnerar **SANT**, hello-instansen har initierats och Azure anropar hello **RoleEntryPoint.Run** metod. Om **OnStart** returnerar **FALSKT**, hello rollen omedelbart, avslutar utan att köra alla planerad avstängning sekvenser.

Hej följande exempel visas hur toooverride hello **OnStart** metod. Den här metoden konfigurerar och startar en diagnostikövervakare när hälsningspaket rollinstansen startar och ställer in en överföring av loggning av data tooa storage-konto:

```csharp
public override bool OnStart()
{
    var config = DiagnosticMonitor.GetDefaultInitialConfiguration();

    config.DiagnosticInfrastructureLogs.ScheduledTransferLogLevelFilter = LogLevel.Error;
    config.DiagnosticInfrastructureLogs.ScheduledTransferPeriod = TimeSpan.FromMinutes(5);

    DiagnosticMonitor.Start("DiagnosticsConnectionString", config);

    return true;
}
```

## <a name="onstop-method"></a>OnStop-metoden
Hej **OnStop** metoden anropas när en rollinstans har varit offline i Azure och hello processen avslutas. Du kan åsidosätta den här metoden toocall koden som krävs för din roll instans toocleanly stängas av.

> [!IMPORTANT]
> Kod som körs i hello **OnStop** metoden har en begränsad tid toofinish när den anropas av andra orsaker än användarinitierad avstängning. Efter den här tiden hello processen avslutas, så måste du se till att koden i hello **OnStop** metod kan köra snabbt eller körs inte toocompletion kan tolerera. Hej **OnStop** metoden anropas efter hello **stoppar** händelsen inträffar.
> 
> 

## <a name="run-method"></a>Metoden Kör
Du kan åsidosätta hello **kör** metoden tooimplement en tidskrävande tråd för din rollinstans.

Åsidosätta hello **kör** metoden är inte obligatoriska; hello standardimplementering startar en tråd som alltid i viloläge. Om du åsidosätta hello **kör** metoden koden ska blockera på obestämd tid. Om hello **kör** metoden returnerar hello rollen återanvänds automatiskt smidigt; med andra ord genererar Azure hello **stoppar** händelse och anrop hello **OnStop** metoden så att stänga-sekvenser kan köras innan hello rollen tas offline.

### <a name="implementing-hello-aspnet-lifecycle-methods-for-a-web-role"></a>Implementera hello ASP.NET livscykel metoder för en webbroll
Du kan använda hello ASP.NET livscykel metoder, förutom toothose som tillhandahålls av hello **RoleEntryPoint** class, toomanage initiering och avstängning sekvenser för en webbroll. Detta kan vara användbart för kompatibilitet om du portera tooAzure för en befintlig ASP.NET-program. hello ASP.NET livscykel metoder anropas från inom hello **RoleEntryPoint** metoder. Hej **programmet\_starta** metoden anropas efter hello **RoleEntryPoint.OnStart** metod har avslutats. Hej **programmet\_End** metoden anropas innan hello **RoleEntryPoint.OnStop** metoden anropas.

## <a name="next-steps"></a>Nästa steg
Lär dig hur för[skapa ett paket för cloud service](cloud-services-model-and-package.md).


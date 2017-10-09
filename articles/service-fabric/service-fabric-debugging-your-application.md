---
title: aaaDebug ditt program i Visual Studio | Microsoft Docs
description: "Förbättra hello pålitligheten och prestandan för dina tjänster genom att utveckla och felsökning i Visual Studio dem på ett kluster för lokal utveckling."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: cb888532-bcdb-4e47-95e4-bfbb1f644da4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: vturecek;mikhegn
ms.openlocfilehash: 8d08689e82195d09f57b462631ad04fd237bc4fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="debug-your-service-fabric-application-by-using-visual-studio"></a>Felsöka Service Fabric-program med hjälp av Visual Studio
> [!div class="op_single_selector"]
> * [Visual Studio/CSharp](service-fabric-debugging-your-application.md) 
> * [Eclipse/Java](service-fabric-debugging-your-application-java.md)
>


## <a name="debug-a-local-service-fabric-application"></a>Felsöka ett lokala Service Fabric-program
Du kan spara tid och pengar genom att distribuera och felsöka ditt Azure Service Fabric-program i ett kluster för utveckling av lokal dator. Visual Studio 2017 eller Visual Studio 2015 kan distribuera hello programmet toohello lokala klustret och ansluta automatiskt hello felsökare tooall instanser av programmet.

1. Starta en lokal utveckling klustret genom att följa stegen hello i [ställa in din utvecklingsmiljö för Service Fabric](service-fabric-get-started.md).
2. Tryck på **F5** eller klicka på **felsöka** > **Start Debugging**.
   
    ![Starta felsökning av ett program][startdebugging]
3. Ange brytpunkter i koden och stega igenom hello program genom att klicka på kommandon i hello **felsöka** menyn.
   
   > [!NOTE]
   > Visual Studio bifogar tooall instanser av programmet. När du stega igenom koden hämta brytpunkter nådde av flera processer som resulterar i samtidiga sessioner. Prova att inaktivera hello brytpunkter när de träffar, genom att göra varje brytpunkt villkorlig på hello tråd-ID eller med hjälp av diagnostiska händelser.
   > 
   > 
4. Hej **diagnostiska händelser** öppnas automatiskt så att du kan visa diagnostik händelser i realtid.
   
    ![Visa diagnostik händelser i realtid][diagnosticevents]
5. Du kan också öppna hello **diagnostiska händelser** fönster i Cloud Explorer.  Under **Service Fabric**, högerklicka på en nod och välj **visa strömning spårningar**.
   
    ![Öppna hello diagnostiska händelser fönster][viewdiagnosticevents]
   
    Om du vill toofilter spårningar tooa specifika tjänsten eller programmet kan bara aktivera strömmande spårningar på den specifika tjänsten eller programmet.
6. hello diagnostiska händelser kan ses i hello genereras automatiskt **ServiceEventSource.cs** filen och anropas från programkod.
   
    ```csharp
    ServiceEventSource.Current.ServiceMessage(this, "My ServiceMessage with a parameter {0}", result.Value.ToString());
    ```
7. Hej **diagnostiska händelser** fönstret stöder filtrering, pausa och undersökning av händelser i realtid.  hello-filter är en sträng sökning i hello händelsemeddelande, inklusive dess innehåll.
   
    ![Filtrera, pausa och återuppta eller granska händelser i realtid][diagnosticeventsactions]
8. Tjänster-felsökning är precis som du felsöker andra program. Du kommer normalt ange brytpunkter via Visual Studio för enkelt felsökning. Även om tillförlitliga samlingar replikeras över flera noder, implementerar de fortfarande IEnumerable. Detta innebär att du kan använda hello resultat i Visual Studio när du felsöker toosee vad du har lagrat i. Ange en brytpunkt bara, var som helst i koden.
   
    ![Starta felsökning av ett program][breakpoint]

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->

## <a name="debug-a-remote-service-fabric-application"></a>Felsöka fjärrprogram Service Fabric
Om Service Fabric-program körs på ett Service Fabric-kluster i Azure, du kan tooremotely felsöka dessa direkt från Visual Studio.

> [!NOTE]
> hello-funktionen kräver [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) och [Azure SDK för .NET 2.9](https://azure.microsoft.com/downloads/).    
> 
> 

<!-- -->
> [!WARNING]
> Fjärrfelsökning är avsett för scenarier för utveckling och testning och inte toobe användas i produktionsmiljöer, på grund av hello inverkan på hello program som körs.
> 
> 

1. Navigera tooyour klustret i **Cloud Explorer**, högerklicka och välj **aktivera felsökning**
   
    ![Aktivera fjärrfelsökning][enableremotedebugging]
   
    Detta kommer startar hello innebär att aktivera hello fjärrfelsökning tillägg på klusternoderna, samt krävs nätverkskonfigurationer.
2. Högerklicka på hello klusternod i **Cloud Explorer**, och välj **koppla felsökare**
   
    ![Koppla felsökare][attachdebugger]
3. I hello **bifoga tooprocess** dialogrutan Välj hello process du toodebug och på **bifoga**
   
    ![Välj process][chooseprocess]
   
    hello namn hello process som du vill tooattach till, som är lika med hello namnet på sammansättningen för tjänsten projektnamn.
   
    hello-felsökaren kan koppla tooall noder som kör hello-processen.
   
   * I hello fall där du felsöker tillståndslösa tjänsten måste tillhör alla instanser av hello-tjänsten på alla noder hello felsökningssessionen.
   * Om du felsöker en tillståndskänslig service blir bara hello primära repliken av någon partition active och därför fångats med hello felsökningsprogram. Om hello primära repliken flyttas under felsökningssessionen hello, kommer hello bearbetningen av den repliken fortfarande att en del av hello felsökningssessionen.
   * Du kan använda villkorlig brytpunkter tooonly break en specifik partition eller instans i ordning tooonly catch relevanta partitioner eller instanser av en viss tjänst.
     
     ![Villkorlig brytpunkt][conditionalbreakpoint]
     
     > [!NOTE]
     > Det finns inte stöd för felsökning av ett Service Fabric-kluster med flera instanser av hello samma körbara namn.
     > 
     > 
4. När du är klar med att felsöka ditt program kan du inaktivera hello remote felsökning tillägg genom att högerklicka på hello-kluster i **Cloud Explorer** och välj **inaktivera felsökning**
   
    ![Inaktivera fjärrfelsökning][disableremotedebugging]

## <a name="streaming-traces-from-a-remote-cluster-node"></a>Strömning spår från en fjärransluten klusternod
Du kan också kan toostream spårningssessioner direkt från ett kluster nod tooVisual Studio. Den här funktionen kan du toostream ETW spåra händelser som genereras på en klusternod för Service Fabric.

> [!NOTE]
> Den här funktionen kräver [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) och [Azure SDK för .NET 2.9](https://azure.microsoft.com/downloads/).
> Den här funktionen har endast stöd för kluster som körs i Azure.
> 
> 

<!-- -->
> [!WARNING]
> Strömning spårningar är avsett för scenarier för utveckling och testning och inte toobe användas i produktionsmiljöer, på grund av hello inverkan på hello program som körs.
> I ett produktionsscenario ska lita på vidarebefordran av händelser med hjälp av Azure-diagnostik.
> 
> 

1. Navigera tooyour klustret i **Cloud Explorer**, högerklicka och välj **aktivera strömning spårningar**
   
    ![Aktivera remote strömmande spårningar][enablestreamingtraces]
   
    Detta kommer startar hello innebär att aktivera hello strömning spårningar tillägg på klusternoderna, samt krävs nätverkskonfigurationer.
2. Expandera hello **noder** element i **Cloud Explorer**, högerklicka på hello nod toostream spår från och välj **visa strömning spårningar**
   
    ![Visa remote strömning spårningar][viewremotestreamingtraces]
   
    Upprepa steg 2 för så många noder som du vill toosee spår från. Varje noder dataström kommer att visas i ett dedikerat fönster.
   
    Du är nu kan toosee hello spårningar sänds av Service Fabric och dina tjänster. Om du vill visa toofilter hello händelser tooonly ett visst program bara ange hello namn för hello programmet hello filter.
   
    ![Visa strömning spårar][viewingstreamingtraces]
3. När du är klar strömmande spår från ditt kluster, du kan inaktivera fjärråtkomst strömmande spårning, genom att högerklicka på klustret hello i **Cloud Explorer** och välj **inaktivera strömning spårningar**
   
    ![Inaktivera fjärråtkomst strömmande spårningar][disablestreamingtraces]

## <a name="next-steps"></a>Nästa steg
* [Testa en tjänst för Service Fabric](service-fabric-testability-overview.md).
* [Hantera Service Fabric-program i Visual Studio](service-fabric-manage-application-in-visual-studio.md).

<!--Image references-->
[startdebugging]: ./media/service-fabric-debugging-your-application/startdebugging.png
[diagnosticevents]: ./media/service-fabric-debugging-your-application/diagnosticevents.png
[viewdiagnosticevents]: ./media/service-fabric-debugging-your-application/viewdiagnosticevents.png
[diagnosticeventsactions]: ./media/service-fabric-debugging-your-application/diagnosticeventsactions.png
[breakpoint]: ./media/service-fabric-debugging-your-application/breakpoint.png
[enableremotedebugging]: ./media/service-fabric-debugging-your-application/enableremotedebugging.png
[attachdebugger]: ./media/service-fabric-debugging-your-application/attachdebugger.png
[chooseprocess]: ./media/service-fabric-debugging-your-application/chooseprocess.png
[conditionalbreakpoint]: ./media/service-fabric-debugging-your-application/conditionalbreakpoint.png
[disableremotedebugging]: ./media/service-fabric-debugging-your-application/disableremotedebugging.png
[enablestreamingtraces]: ./media/service-fabric-debugging-your-application/enablestreamingtraces.png
[viewingstreamingtraces]: ./media/service-fabric-debugging-your-application/viewingstreamingtraces.png
[viewremotestreamingtraces]: ./media/service-fabric-debugging-your-application/viewremotestreamingtraces.png
[disablestreamingtraces]: ./media/service-fabric-debugging-your-application/disablestreamingtraces.png

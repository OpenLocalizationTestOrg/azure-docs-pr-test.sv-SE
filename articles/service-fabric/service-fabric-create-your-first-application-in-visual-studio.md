---
title: "aaaCreate en tillförlitlig Azure Service Fabric-tjänsten med C#"
description: "Skapa, distribuera och felsöka ett tillförlitligt tjänstprogram som bygger på Azure Service Fabric med Visual Studio."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: vturecek
ms.assetid: c3655b7b-de78-4eac-99eb-012f8e042109
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/28/2017
ms.author: ryanwi
ms.openlocfilehash: 740c866da6e639219b529fe92ed63cbeaa702a35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-c-service-fabric-stateful-reliable-services-application"></a>Skapa ditt första tillståndskänsliga tillförlitliga C# Service Fabric-program

Lär dig hur toodeploy ditt första Service Fabric-program för .NET i Windows på bara några minuter. När du är klar har du ett lokala kluster som körs med ett tillförlitligt tjänstprogram.

## <a name="prerequisites"></a>Krav

Du måste [konfigurera utvecklingsmiljön](service-fabric-get-started.md) innan du börjar. Detta omfattar att installera hello Service Fabric-SDK och Visual Studio 2017 eller 2015.

## <a name="create-hello-application"></a>Skapa hello program

Starta Visual Studio som **administratör**.

Skapa ett projekt med `CTRL`+`SHIFT`+`N`

I hello **nytt projekt** dialogrutan Välj **molntjänster > Fabric tjänstprogrammet**.

Namnge programmet hello **MyApplication** och tryck på **OK**.

   
![Dialogrutan Nytt projekt i Visual Studio][1]

Du kan skapa Service Fabric-program från hello nästa dialogruta. För den här snabbstarten, väljer du **Tillståndskänslig tjänst**.

Namnge hello service **MyStatefulService** och tryck på **OK**.

![Dialogrutan Ny tjänst i Visual Studio][2]


Visual Studio skapar hello programmet projektet och hello tillståndskänslig service och visar dem i Solution Explorer.

![Solution Explorer när ett program med en tillståndskänslig tjänst har skapats][3]

hello projektet (**MyApplication**) innehåller inte någon kod direkt. I stället refererar det till en uppsättning tjänstprojekt. Dessutom innehåller det tre andra typer av innehåll:

* **Publicera profiler**  
Profiler för att distribuera toodifferent miljöer.

* **Skript**  
PowerShell-skript för distribution/uppgradering av program.

* **Programdefinition**  
Innehåller hello ApplicationManifest.xml fil under *ApplicationPackageRoot* som beskriver ditt programs sammansättning. Associerade programfiler för parametern är *ApplicationParameters*, vilket kan vara används toospecify miljö-specifika parametrar. Visual Studio väljer en programfil parameter som anges i hello associerade publiceringsprofil under distributionen tooa specifika miljö.
    
En översikt över hello innehållet i hello service-projekt, se [komma igång med Reliable Services](service-fabric-reliable-services-quick-start.md).

## <a name="deploy-and-debug-hello-application"></a>Distribuera och felsöka hello program

Nu när du har ett program kan du prova att köra det.

I Visual Studio trycker du på `F5` toodeploy hello program för felsökning.

>[!NOTE]
>hello skapar första gången du kör och distribuera hello programmet lokalt, Visual Studio ett lokala kluster för felsökning. Det kan ta lite tid. hello klustret Skapandestatus visas i utdatafönstret för hello Visual Studio.

När hello klustret är klar, kan du få ett meddelande från hello lokala klustret fack manager systemprogram medföljer hello SDK.
   
![Meddelande från Local Cluster Manager i systemfältet][4]

En gång hello programmet startar Visual Studio automatiskt öppnar hello **diagnostik Loggboken**, där du kan se spårningsutdata från dina tjänster.
   
![Loggboken Diagnostik][5]

hello tillståndskänslig tjänstmall som vi använde visas endast en räknare värde ökar i hello `RunAsync` metod för **MyStatefulService.cs**.

Expandera en hello händelser toosee mer information, inklusive hello-nod där hello kod körs. I det här fallet är det \_Node\_2, men det kan skilja sig på din dator.
   
![Detaljer från loggboken Diagnostik][6]

hello lokala kluster innehåller fem noder som finns på en enskild dator. Varje nod finns på en distinkt fysisk eller virtuell dator i en produktionsmiljö. toosimulate hello förlust av en dator när utöva hello Visual Studio för felsökning på hello samma tid, ska vi ta bort en av noderna hello på hello lokala klustret.

I hello **Solution Explorer** fönster, **MyStatefulService.cs**. 

Hitta hello `RunAsync` metod och ange en brytpunkt på hello första raden i hello-metoden.

![Brytpunkt i den tillståndskänsliga tjänstens RunAsync-metod][7]

Starta hello **Service Fabric Explorer** verktyget genom att högerklicka på hello **lokala Klusterhanterare** fack systemprogram och välj **hantera lokala klustret**.

![Starta Service Fabric Explorer från hello Klusterhanterare för lokala][systray-launch-sfx]

[**Service Fabric Explorer**](service-fabric-visualizing-your-cluster.md) visar en bild av ett kluster. Det innehåller hello uppsättning program som distribueras tooit och hello uppsättning fysiska noder som utgör den.

I hello till vänster och expanderar **kluster > noder** och hitta hello nod där din kod körs.

Klicka på **åtgärder > Inaktivera (omstart)** toosimulate en datorn startas om.

![Stoppa en nod i Service Fabric Explorer][sfx-stop-node]

Tillfälligt, bör du se din brytpunkt träffar i Visual Studio som hello beräkning som du gjorde på en nod sömlöst växlar tooanother.


Sedan returnera toohello diagnostiska Loggboken och se hälsningsmeddelande. hello räknare har blivit ökar, även om hello händelser faktiskt kommer från en annan nod.

![Loggboken Diagnostik efter en redundansväxling][diagnostic-events-viewer-detail-post-failover]

## <a name="cleaning-up-hello-local-cluster-optional"></a>Rensa hello lokala klustret (valfritt)

Kom ihåg att det här lokala klustret är verkligt. Stoppa hello felsökare tar bort din programinstansen och Avregistrerar hello programtyp. Hello klustret fortsätter dock toorun i hello bakgrund. När du är klar toostop hello lokala klustret, finns det några alternativ.

### <a name="keep-application-and-trace-data"></a>Bibehåll program- och spårningsdata

Stäng hello klustret genom att högerklicka på hello **lokala Klusterhanterare** fack systemprogram och välj sedan **stoppa lokala klustret**.

### <a name="delete-hello-cluster-and-all-data"></a>Ta bort hello kluster och alla data

Ta bort hello klustret genom att högerklicka på hello **lokala Klusterhanterare** fack systemprogram och välj sedan **ta bort lokala klustret**. 

Om du väljer det här alternativet, kommer Visual Studio omdistribuera hello klustret hello nästa gång som din kör hello program. Välj det här alternativet om du inte avser toouse hello lokala kluster under en viss tid eller om du behöver tooreclaim resurser.

## <a name="next-steps"></a>Nästa steg
Mer information om [Reliable Services](service-fabric-reliable-services-introduction.md).
<!-- Image References -->

[1]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog.png
[2]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog-2.png
[3]: ./media/service-fabric-create-your-first-application-in-visual-studio/solution-explorer-stateful-service-template.png
[4]: ./media/service-fabric-create-your-first-application-in-visual-studio/local-cluster-manager-notification.png
[5]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer.png
[6]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer-detail.png
[7]: ./media/service-fabric-create-your-first-application-in-visual-studio/runasync-breakpoint.png
[sfx-stop-node]: ./media/service-fabric-create-your-first-application-in-visual-studio/sfe-deactivate-node.png
[systray-launch-sfx]: ./media/service-fabric-create-your-first-application-in-visual-studio/launch-sfx.png
[diagnostic-events-viewer-detail-post-failover]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer-detail-post-failover.png
[sfe-delete-application]: ./media/service-fabric-create-your-first-application-in-visual-studio/sfe-delete-application.png
[switch-cluster-mode]: ./media/service-fabric-create-your-first-application-in-visual-studio/switch-cluster-mode.png
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png
